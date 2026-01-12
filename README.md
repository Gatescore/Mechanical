<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GATE Mechanical Master</title>
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

        .progress-ring {
            transition: stroke-dashoffset 0.35s;
            transform: rotate(-90deg);
            transform-origin: 50% 50%;
        }

        /* Hide scrollbars */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="pb-24">

    <!-- Header -->
    <header class="sticky top-0 z-50 bg-blue-600 text-white p-4 shadow-lg rounded-b-3xl">
        <div class="flex justify-between items-center max-w-2xl mx-auto">
            <div>
                <h1 class="text-xl font-bold">GATE Mechanical</h1>
                <p class="text-xs text-blue-100">Excellence in Engineering</p>
            </div>
            <div class="bg-blue-500 p-2 rounded-full h-10 w-10 flex items-center justify-center">
                <i class="fas fa-user"></i>
            </div>
        </div>
    </header>

    <main class="max-w-2xl mx-auto p-4 space-y-6">
        
        <!-- Stats Section -->
        <div class="grid grid-cols-2 gap-4">
            <div class="glass-card p-4 rounded-2xl shadow-sm border-l-4 border-blue-500">
                <p class="text-gray-500 text-sm">Solved Today</p>
                <p class="text-2xl font-bold text-gray-800" id="stat-solved">12</p>
            </div>
            <div class="glass-card p-4 rounded-2xl shadow-sm border-l-4 border-green-500">
                <p class="text-gray-500 text-sm">Accuracy</p>
                <p class="text-2xl font-bold text-gray-800">85%</p>
            </div>
        </div>

        <!-- App Navigation (Tabs) -->
        <div id="view-container">
            <!-- Subjects View (Default) -->
            <section id="view-subjects" class="space-y-4">
                <h2 class="text-lg font-semibold text-gray-700 px-1">Study Modules</h2>
                <div class="grid grid-cols-1 gap-4" id="subject-list">
                    <!-- Loaded via JS -->
                </div>
            </section>

            <!-- Quiz View (Hidden) -->
            <section id="view-quiz" class="hidden space-y-4">
                <div class="flex items-center justify-between">
                    <button onclick="changeView('subjects')" class="text-blue-600"><i class="fas fa-arrow-left mr-2"></i> Back</button>
                    <span id="quiz-progress" class="text-sm font-medium text-gray-500">Question 1/5</span>
                </div>
                
                <div class="glass-card p-6 rounded-2xl shadow-md min-h-[300px] flex flex-col justify-between">
                    <div id="question-area">
                        <p id="question-text" class="text-lg font-medium text-gray-800 mb-6"></p>
                        <div id="options-container" class="space-y-3">
                            <!-- Options generated here -->
                        </div>
                    </div>
                    
                    <div id="feedback-area" class="mt-6 hidden p-4 rounded-xl text-sm"></div>
                    
                    <button id="next-btn" onclick="nextQuestion()" class="hidden w-full bg-blue-600 text-white py-3 rounded-xl font-bold shadow-lg mt-6">
                        Next Question
                    </button>
                </div>
            </section>

            <!-- Formulas View (Hidden) -->
            <section id="view-formulas" class="hidden space-y-4">
                <h2 class="text-lg font-semibold text-gray-700 px-1">Quick Revision Sheets</h2>
                <div class="space-y-3" id="formula-list">
                    <!-- Loaded via JS -->
                </div>
            </section>
        </div>

    </main>

    <!-- Bottom Navigation -->
    <nav class="fixed bottom-0 left-0 right-0 bg-white border-t border-gray-200 py-3 px-6 flex justify-between items-center z-50 max-w-2xl mx-auto">
        <button onclick="changeView('subjects')" class="nav-btn flex flex-col items-center text-blue-600" id="nav-subjects">
            <i class="fas fa-book-open text-xl"></i>
            <span class="text-[10px] mt-1 font-bold">Subjects</span>
        </button>
        <button onclick="changeView('formulas')" class="nav-btn flex flex-col items-center text-gray-400" id="nav-formulas">
            <i class="fas fa-square-root-variable text-xl"></i>
            <span class="text-[10px] mt-1 font-bold">Formulas</span>
        </button>
        <button onclick="openPerformance()" class="nav-btn flex flex-col items-center text-gray-400">
            <i class="fas fa-chart-line text-xl"></i>
            <span class="text-[10px] mt-1 font-bold">Stats</span>
        </button>
        <button onclick="openSettings()" class="nav-btn flex flex-col items-center text-gray-400">
            <i class="fas fa-cog text-xl"></i>
            <span class="text-[10px] mt-1 font-bold">Settings</span>
        </button>
    </nav>

    <script>
        const appData = {
            subjects: [
                { id: 'td', name: 'Thermodynamics', icon: 'fa-fire', color: 'bg-orange-100 text-orange-600', count: 120 },
                { id: 'som', name: 'Strength of Materials', icon: 'fa-dumbbell', color: 'bg-blue-100 text-blue-600', count: 85 },
                { id: 'fm', name: 'Fluid Mechanics', icon: 'fa-tint', color: 'bg-cyan-100 text-cyan-600', count: 94 },
                { id: 'tom', name: 'Theory of Machines', icon: 'fa-cogs', color: 'bg-purple-100 text-purple-600', count: 70 },
                { id: 'ht', name: 'Heat Transfer', icon: 'fa-sun', color: 'bg-red-100 text-red-600', count: 65 }
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
                        o: ["Pressure only", "Temperature only", "Volume only", "Pressure and Volume"],
                        a: 1,
                        ex: "According to Joule's Law, internal energy (U) depends only on Absolute Temperature (T)."
                    }
                ],
                som: [
                    {
                        q: "The ratio of lateral strain to axial strain is called:",
                        o: ["Young's Modulus", "Bulk Modulus", "Poisson's Ratio", "Modulus of Rigidity"],
                        a: 2,
                        ex: "Poisson's ratio is the negative ratio of transverse strain to axial strain."
                    }
                ]
            },
            formulas: [
                { title: "First Law (Closed Sys)", eq: "Q - W = ΔU" },
                { title: "Ideal Gas Law", eq: "PV = mRT" },
                { title: "Young's Modulus", eq: "E = σ / ε" }
            ]
        };

        let currentSubject = '';
        let currentQuestionIndex = 0;
        let score = 0;

        function init() {
            renderSubjects();
            renderFormulas();
        }

        function renderSubjects() {
            const list = document.getElementById('subject-list');
            list.innerHTML = appData.subjects.map(s => `
                <div onclick="startQuiz('${s.id}')" class="subject-card glass-card p-4 rounded-2xl flex items-center justify-between cursor-pointer transition-all active:bg-gray-50">
                    <div class="flex items-center space-x-4">
                        <div class="${s.color} w-12 h-12 rounded-xl flex items-center justify-center text-xl">
                            <i class="fas ${s.icon}"></i>
                        </div>
                        <div>
                            <h3 class="font-bold text-gray-800">${s.name}</h3>
                            <p class="text-xs text-gray-500">${s.count} Questions Available</p>
                        </div>
                    </div>
                    <i class="fas fa-chevron-right text-gray-300"></i>
                </div>
            `).join('');
        }

        function renderFormulas() {
            const list = document.getElementById('formula-list');
            list.innerHTML = appData.formulas.map(f => `
                <div class="glass-card p-4 rounded-xl">
                    <p class="text-xs font-bold text-blue-600 uppercase mb-1">${f.title}</p>
                    <p class="text-xl font-mono text-gray-800">$$ ${f.eq} $$</p>
                </div>
            `).join('');
        }

        function changeView(viewId) {
            document.querySelectorAll('#view-container > section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`view-${viewId}`).classList.remove('hidden');
            
            // Update Nav Icons
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.replace('text-blue-600', 'text-gray-400'));
            const activeBtn = document.getElementById(`nav-${viewId}`);
            if(activeBtn) activeBtn.classList.replace('text-gray-400', 'text-blue-600');
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
                <button onclick="checkAnswer(${idx})" class="option-btn w-full p-4 text-left border-2 border-gray-100 rounded-xl hover:border-blue-200 transition-colors bg-white">
                    <span class="inline-block w-8 h-8 rounded-full bg-gray-50 text-center leading-8 mr-3 font-bold text-gray-400">${String.fromCharCode(65 + idx)}</span>
                    ${opt}
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
            
            // Disable all buttons
            btns.forEach(b => b.disabled = true);
            
            const feedback = document.getElementById('feedback-area');
            feedback.classList.remove('hidden');
            
            if (idx === q.a) {
                btns[idx].classList.add('border-green-500', 'bg-green-50');
                feedback.innerHTML = `<p class="text-green-700 font-bold">Correct!</p><p class="text-green-600">${q.ex}</p>`;
                feedback.className = "mt-6 p-4 rounded-xl text-sm bg-green-100";
                score++;
            } else {
                btns[idx].classList.add('border-red-500', 'bg-red-50');
                btns[q.a].classList.add('border-green-500', 'bg-green-50');
                feedback.innerHTML = `<p class="text-red-700 font-bold">Wrong Answer</p><p class="text-red-600">${q.ex}</p>`;
                feedback.className = "mt-6 p-4 rounded-xl text-sm bg-red-100";
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
            const container = document.getElementById('question-area');
            container.innerHTML = `
                <div class="text-center py-10">
                    <div class="text-6xl mb-4">🏆</div>
                    <h2 class="text-2xl font-bold text-gray-800">Quiz Completed!</h2>
                    <p class="text-gray-500 mt-2">You scored ${score} out of ${appData.questions[currentSubject].length}</p>
                    <button onclick="changeView('subjects')" class="mt-8 bg-blue-600 text-white px-8 py-3 rounded-full font-bold">Back to Subjects</button>
                </div>
            `;
            document.getElementById('feedback-area').classList.add('hidden');
            document.getElementById('next-btn').classList.add('hidden');
            
            // Update local storage stats
            let total = parseInt(document.getElementById('stat-solved').innerText);
            document.getElementById('stat-solved').innerText = total + score;
        }

        function openPerformance() {
            alert("This feature tracks your chapter-wise proficiency. (Demo Mode)");
        }

        function openSettings() {
            alert("Settings: Dark Mode, Notification triggers, and Profile management.");
        }

        window.onload = init;
    </script>
</body>
</html>


