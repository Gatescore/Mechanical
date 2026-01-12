<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- Meta tags for GitHub/SEO -->
    <meta name="description" content="GATE Mechanical Engineering Practice App">
    <title>GATE SCORE</title>
    
    <!-- Tailwind and Icons -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc;
            -webkit-tap-highlight-color: transparent;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(226, 232, 240, 0.8);
        }

        .subject-card:active {
            transform: scale(0.98);
        }

        /* Smooth transitions for view switching */
        .view-section {
            animation: fadeIn 0.3s ease-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .no-scrollbar::-webkit-scrollbar { display: none; }
    </style>
</head>
<body class="pb-24">

    <!-- Header -->
    <header class="sticky top-0 z-50 bg-blue-700 text-white p-4 shadow-lg rounded-b-3xl">
        <div class="flex justify-between items-center max-w-2xl mx-auto">
            <div>
                <h1 class="text-xl font-bold">GATE Mechanical</h1>
                <p class="text-xs text-blue-100">Mock Test & Formulae</p>
            </div>
            <div class="bg-blue-600 p-2 rounded-full h-10 w-10 flex items-center justify-center border border-blue-400">
                <i class="fas fa-user"></i>
            </div>
        </div>
    </header>

    <main class="max-w-2xl mx-auto p-4 space-y-6">
        
        <!-- Stats Section -->
        <div class="grid grid-cols-2 gap-4">
            <div class="glass-card p-4 rounded-2xl shadow-sm border-l-4 border-blue-500">
                <p class="text-gray-500 text-sm">Solved Today</p>
                <p class="text-2xl font-bold text-gray-800" id="stat-solved">0</p>
            </div>
            <div class="glass-card p-4 rounded-2xl shadow-sm border-l-4 border-green-500">
                <p class="text-gray-500 text-sm">Accuracy</p>
                <p class="text-2xl font-bold text-gray-800">0%</p>
            </div>
        </div>

        <div id="view-container">
            <!-- Subjects View -->
            <section id="view-subjects" class="view-section space-y-4">
                <div class="flex items-center justify-between px-1">
                    <h2 class="text-lg font-semibold text-gray-700">Study Modules</h2>
                    <span class="text-xs bg-blue-100 text-blue-700 px-2 py-1 rounded-full font-bold">FREE ACCESS</span>
                </div>
                <div class="grid grid-cols-1 gap-4" id="subject-list">
                    <!-- Loaded via JS -->
                </div>
            </section>

            <!-- Quiz View -->
            <section id="view-quiz" class="view-section hidden space-y-4">
                <div class="flex items-center justify-between">
                    <button onclick="changeView('subjects')" class="text-blue-600 font-medium">
                        <i class="fas fa-chevron-left mr-1"></i> Quit Quiz
                    </button>
                    <span id="quiz-progress" class="text-sm font-bold text-blue-600 bg-blue-50 px-3 py-1 rounded-full">Question 1/5</span>
                </div>
                
                <div class="glass-card p-6 rounded-3xl shadow-md min-h-[350px] flex flex-col justify-between">
                    <div id="question-area">
                        <p id="question-text" class="text-lg font-semibold text-gray-800 mb-8 leading-relaxed"></p>
                        <div id="options-container" class="space-y-3">
                            <!-- Options generated here -->
                        </div>
                    </div>
                    
                    <div id="feedback-area" class="mt-6 hidden p-4 rounded-2xl text-sm border"></div>
                    
                    <button id="next-btn" onclick="nextQuestion()" class="hidden w-full bg-blue-700 text-white py-4 rounded-2xl font-bold shadow-xl mt-6 active:scale-95 transition-transform">
                        Next Question
                    </button>
                </div>
            </section>

            <!-- Formulas View -->
            <section id="view-formulas" class="view-section hidden space-y-4">
                <h2 class="text-lg font-semibold text-gray-700 px-1">Quick Revision Sheets</h2>
                <div class="space-y-3" id="formula-list">
                    <!-- Loaded via JS -->
                </div>
            </section>
        </div>

    </main>

    <!-- Navigation -->
    <nav class="fixed bottom-0 left-0 right-0 bg-white/80 backdrop-blur-md border-t border-gray-100 py-3 px-8 flex justify-between items-center z-50 max-w-2xl mx-auto rounded-t-3xl shadow-[0_-5px_20px_rgba(0,0,0,0.05)]">
        <button onclick="changeView('subjects')" class="nav-btn flex flex-col items-center transition-colors text-blue-700" id="nav-subjects">
            <i class="fas fa-home text-xl"></i>
            <span class="text-[10px] mt-1 font-bold">Home</span>
        </button>
        <button onclick="changeView('formulas')" class="nav-btn flex flex-col items-center transition-colors text-gray-400" id="nav-formulas">
            <i class="fas fa-book text-xl"></i>
            <span class="text-[10px] mt-1 font-bold">Formula</span>
        </button>
        <button onclick="alert('Profile sync coming soon!')" class="nav-btn flex flex-col items-center transition-colors text-gray-400">
            <i class="fas fa-trophy text-xl"></i>
            <span class="text-[10px] mt-1 font-bold">Leaderboard</span>
        </button>
        <button onclick="alert('Settings: App version 1.0.2')" class="nav-btn flex flex-col items-center transition-colors text-gray-400">
            <i class="fas fa-gear text-xl"></i>
            <span class="text-[10px] mt-1 font-bold">Settings</span>
        </button>
    </nav>

    <script>
        const appData = {
            subjects: [
                { id: 'td', name: 'Thermodynamics', icon: 'fa-fire', color: 'bg-orange-100 text-orange-600', count: 120 },
                { id: 'som', name: 'Strength of Materials', icon: 'fa-vector-square', color: 'bg-blue-100 text-blue-600', count: 85 },
                { id: 'fm', name: 'Fluid Mechanics', icon: 'fa-droplet', color: 'bg-cyan-100 text-cyan-600', count: 94 },
                { id: 'tom', name: 'Theory of Machines', icon: 'fa-gears', color: 'bg-indigo-100 text-indigo-600', count: 70 },
                { id: 'ht', name: 'Heat Transfer', icon: 'fa-temperature-high', color: 'bg-red-100 text-red-600', count: 65 }
            ],
            questions: {
                td: [
                    {
                        q: "A closed system undergoes a process in which there is no heat transfer. This is called?",
                        o: ["Isothermal", "Isobaric", "Adiabatic", "Isentropic"],
                        a: 2,
                        ex: "In an adiabatic process, the system is thermally insulated from its surroundings (Q=0)."
                    },
                    {
                        q: "The internal energy of an ideal gas is a function of:",
                        o: ["Pressure only", "Temperature only", "Volume only", "P and V"],
                        a: 1,
                        ex: "According to Joule's Law, internal energy (U) depends only on Absolute Temperature (T)."
                    }
                ],
                som: [
                    {
                        q: "The ratio of lateral strain to axial strain is called:",
                        o: ["Young's Modulus", "Bulk Modulus", "Poisson's Ratio", "Modulus of Rigidity"],
                        a: 2,
                        ex: "Poisson's ratio is defined as the negative ratio of lateral strain to longitudinal strain."
                    }
                ],
                fm: [
                    {
                        q: "Viscosity of gases _______ with increase in temperature.",
                        o: ["Increases", "Decreases", "Remains Constant", "First increases then decreases"],
                        a: 0,
                        ex: "In gases, viscosity is due to molecular momentum transfer, which increases with temperature."
                    }
                ]
            },
            formulas: [
                { title: "Thermodynamics", eq: "Q - W = ΔU (1st Law)" },
                { title: "Fluid Mechanics", eq: "P + ½ρv² + ρgh = const" },
                { title: "SOM - Hooke's Law", eq: "σ = E × ε" },
                { title: "HT - Fourier Law", eq: "q = -k(dT/dx)" }
            ]
        };

        let currentSubject = '';
        let currentQuestionIndex = 0;
        let score = 0;

        function init() {
            renderSubjects();
            renderFormulas();
            loadStats();
        }

        function renderSubjects() {
            const list = document.getElementById('subject-list');
            list.innerHTML = appData.subjects.map(s => `
                <div onclick="startQuiz('${s.id}')" class="subject-card glass-card p-5 rounded-3xl flex items-center justify-between cursor-pointer shadow-sm active:bg-blue-50 transition-all">
                    <div class="flex items-center space-x-4">
                        <div class="${s.color} w-14 h-14 rounded-2xl flex items-center justify-center text-2xl shadow-inner">
                            <i class="fas ${s.icon}"></i>
                        </div>
                        <div>
                            <h3 class="font-bold text-gray-800 text-md">${s.name}</h3>
                            <p class="text-xs text-gray-400 font-medium">${s.count} Questions</p>
                        </div>
                    </div>
                    <div class="bg-gray-50 p-2 rounded-full">
                        <i class="fas fa-play text-blue-600 text-xs ml-0.5"></i>
                    </div>
                </div>
            `).join('');
        }

        function renderFormulas() {
            const list = document.getElementById('formula-list');
            list.innerHTML = appData.formulas.map(f => `
                <div class="glass-card p-5 rounded-2xl border-l-4 border-indigo-500">
                    <p class="text-[10px] font-bold text-indigo-500 uppercase tracking-widest mb-1">${f.title}</p>
                    <p class="text-xl font-mono text-gray-800 bg-gray-50 p-3 rounded-lg border border-gray-100">${f.eq}</p>
                </div>
            `).join('');
        }

        function changeView(viewId) {
            document.querySelectorAll('.view-section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`view-${viewId}`).classList.remove('hidden');
            
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.replace('text-blue-700', 'text-gray-400'));
            const activeBtn = document.getElementById(`nav-${viewId}`);
            if(activeBtn) activeBtn.classList.replace('text-gray-400', 'text-blue-700');
            
            // If returning to home, reset state
            if(viewId === 'subjects') {
                document.getElementById('question-area').innerHTML = `
                    <p id="question-text" class="text-lg font-semibold text-gray-800 mb-8 leading-relaxed"></p>
                    <div id="options-container" class="space-y-3"></div>
                `;
            }
        }

        function startQuiz(subjectId) {
            currentSubject = subjectId;
            currentQuestionIndex = 0;
            score = 0;
            changeView('quiz');
            loadQuestion();
        }

        function loadQuestion() {
            const questions = appData.questions[currentSubject] || appData.questions['td'];
            const q = questions[currentQuestionIndex];
            
            document.getElementById('quiz-progress').innerText = `Question ${currentQuestionIndex + 1}/${questions.length}`;
            document.getElementById('question-text').innerText = q.q;
            
            const optionsHtml = q.o.map((opt, idx) => `
                <button onclick="checkAnswer(${idx})" class="option-btn w-full p-4 text-left border-2 border-gray-100 rounded-2xl hover:border-blue-200 transition-all bg-white flex items-center">
                    <span class="w-8 h-8 rounded-lg bg-gray-100 text-center leading-8 mr-4 font-bold text-gray-500 text-sm">${String.fromCharCode(65 + idx)}</span>
                    <span class="font-medium text-gray-700">${opt}</span>
                </button>
            `).join('');
            
            document.getElementById('options-container').innerHTML = optionsHtml;
            document.getElementById('feedback-area').classList.add('hidden');
            document.getElementById('next-btn').classList.add('hidden');
        }

        function checkAnswer(idx) {
            const questions = appData.questions[currentSubject] || appData.questions['td'];
            const q = questions[currentQuestionIndex];
            const btns = document.querySelectorAll('.option-btn');
            
            btns.forEach(b => b.disabled = true);
            const feedback = document.getElementById('feedback-area');
            feedback.classList.remove('hidden');
            
            if (idx === q.a) {
                btns[idx].classList.add('border-green-500', 'bg-green-50');
                feedback.innerHTML = `<div class="flex items-center text-green-700 font-bold mb-1"><i class="fas fa-check-circle mr-2"></i> Excellent!</div><p class="text-green-600">${q.ex}</p>`;
                feedback.className = "mt-6 p-4 rounded-2xl text-sm bg-green-50 border-green-200";
                score++;
            } else {
                btns[idx].classList.add('border-red-500', 'bg-red-50');
                btns[q.a].classList.add('border-green-500', 'bg-green-50');
                feedback.innerHTML = `<div class="flex items-center text-red-700 font-bold mb-1"><i class="fas fa-times-circle mr-2"></i> Not Quite</div><p class="text-red-600">${q.ex}</p>`;
                feedback.className = "mt-6 p-4 rounded-2xl text-sm bg-red-50 border-red-200";
            }
            
            document.getElementById('next-btn').classList.remove('hidden');
        }

        function nextQuestion() {
            const questions = appData.questions[currentSubject] || appData.questions['td'];
            currentQuestionIndex++;
            if (currentQuestionIndex < questions.length) {
                loadQuestion();
            } else {
                finishQuiz();
            }
        }

        function finishQuiz() {
            const questions = appData.questions[currentSubject] || appData.questions['td'];
            const container = document.getElementById('question-area');
            const percent = Math.round((score / questions.length) * 100);
            
            container.innerHTML = `
                <div class="text-center py-6">
                    <div class="inline-block p-6 bg-blue-50 rounded-full mb-6">
                        <i class="fas fa-trophy text-6xl text-blue-600"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-gray-800">Results</h2>
                    <p class="text-gray-500 mt-2">You scored <strong>${score}</strong> out of ${questions.length}</p>
                    <div class="w-full bg-gray-100 h-3 rounded-full mt-6 overflow-hidden">
                        <div class="bg-blue-600 h-full" style="width: ${percent}%"></div>
                    </div>
                    <p class="text-xs font-bold text-blue-600 mt-2">${percent}% Accuracy</p>
                    
                    <button onclick="changeView('subjects')" class="mt-10 w-full bg-blue-700 text-white py-4 rounded-2xl font-bold shadow-lg">Back to Home</button>
                </div>
            `;
            document.getElementById('feedback-area').classList.add('hidden');
            document.getElementById('next-btn').classList.add('hidden');
            
            saveStats(score);
        }

        function saveStats(newScore) {
            let total = parseInt(localStorage.getItem('gate_solved') || 0);
            total += newScore;
            localStorage.setItem('gate_solved', total);
            loadStats();
        }

        function loadStats() {
            const total = localStorage.getItem('gate_solved') || 0;
            document.getElementById('stat-solved').innerText = total;
        }

        window.onload = init;
    </script>
</body>
</html>

