<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Pro Academy | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; scroll-behavior: smooth; }
        .glass-nav { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(10px); }
        .q-card { background: white; border-radius: 20px; padding: 30px; margin-bottom: 25px; transition: 0.3s; border: 1px solid #e2e8f0; }
        .q-card:hover { transform: scale(1.01); box-shadow: 0 10px 30px rgba(0,0,0,0.05); }
        .option-btn { display: block; padding: 15px; border: 2px solid #f1f5f9; border-radius: 12px; margin-top: 10px; cursor: pointer; transition: 0.2s; font-weight: 500; }
        .option-btn:hover { border-color: #1e3a8a; background: #f0f7ff; }
        .correct { background: #dcfce7 !important; border-color: #22c55e !important; color: #166534; }
        .wrong { background: #fee2e2 !important; border-color: #ef4444 !important; color: #991b1b; }
        .section-tag { font-size: 10px; font-weight: 900; text-transform: uppercase; letter-spacing: 1px; padding: 4px 12px; border-radius: 50px; }
    </style>
</head>
<body class="pb-24">

    <div id="startPage" class="fixed inset-0 bg-blue-900 z-[100] flex items-center justify-center p-6">
        <div class="max-w-4xl w-full text-center text-white">
            <h1 class="text-5xl md:text-7xl font-black italic mb-4 tracking-tighter">GB POLICE HUB</h1>
            <p class="text-blue-300 font-bold mb-12 uppercase">Tayari Karein Ya Test Dein - Choice Aapki Hai Sweetie</p>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div onclick="initModule('practice')" class="bg-white text-blue-900 p-8 rounded-[40px] cursor-pointer hover:scale-105 transition shadow-2xl">
                    <div class="text-4xl mb-4">📖</div>
                    <h3 class="font-black text-xl underline">PRACTICE MODE</h3>
                    <p class="text-xs mt-2 text-gray-500">No Timer + Correct Answers for preparation</p>
                </div>
                <div onclick="initModule('exam')" class="bg-yellow-500 text-white p-8 rounded-[40px] cursor-pointer hover:scale-105 transition shadow-2xl">
                    <div class="text-4xl mb-4">⏱️</div>
                    <h3 class="font-black text-xl underline">EXAM MODE</h3>
                    <p class="text-xs mt-2 text-yellow-100">100 Marks Real Test with 90min Timer</p>
                </div>
                <div onclick="showNotes()" class="bg-blue-800 border-2 border-blue-400 text-white p-8 rounded-[40px] cursor-pointer hover:scale-105 transition shadow-2xl">
                    <div class="text-4xl mb-4">📂</div>
                    <h3 class="font-black text-xl underline">STUDY NOTES</h3>
                    <p class="text-xs mt-2 text-blue-200">Download Syllabus & Past Papers</p>
                </div>
            </div>
        </div>
    </div>

    <nav class="glass-nav sticky top-0 z-50 p-4 border-b border-gray-200">
        <div class="max-w-5xl mx-auto flex justify-between items-center">
            <h2 class="font-black text-blue-900 text-2xl">PRIME POLICE PORTAL</h2>
            <div id="timerDisplay" class="hidden bg-red-600 text-white px-6 py-2 rounded-2xl font-black font-mono text-xl animate-pulse">
                90:00
            </div>
        </div>
    </nav>

    <main class="max-w-4xl mx-auto p-4 mt-8">
        <div id="quizArea"></div>
        <button onclick="finishSession()" class="w-full bg-blue-900 text-white py-6 rounded-3xl font-black text-3xl shadow-2xl mt-12 hover:bg-black transition-all uppercase tracking-tighter">
            Submit Final Work
        </button>
    </main>

    <div id="notesModal" class="hidden fixed inset-0 bg-black/80 backdrop-blur-md z-[110] flex items-center justify-center p-6">
        <div class="bg-white p-10 rounded-[40px] max-w-lg w-full">
            <h2 class="text-2xl font-black text-blue-900 mb-6 uppercase">Free Study Material</h2>
            <ul class="space-y-4 mb-8">
                <li class="flex justify-between items-center p-4 bg-gray-50 rounded-2xl">
                    <span class="font-bold">GB Geography Past Papers</span>
                    <a href="#" class="text-blue-600 font-black">PDF</a>
                </li>
                <li class="flex justify-between items-center p-4 bg-gray-50 rounded-2xl">
                    <span class="font-bold">1000+ Urdu Grammer MCQs</span>
                    <a href="#" class="text-blue-600 font-black">PDF</a>
                </li>
            </ul>
            <button onclick="hideNotes()" class="w-full bg-blue-900 text-white py-4 rounded-2xl font-bold">Back to Menu</button>
        </div>
    </div>

    <script>
        const questionBank = [
            // English
            { q: "Opposite of 'ENORMOUS' is:", a: "Tiny", b: "Gigantic", ans: "a", cat: "English", color: "bg-blue-100 text-blue-800" },
            { q: "Synonym of 'VIGILANT' is:", a: "Careless", b: "Watchful", ans: "b", cat: "English", color: "bg-blue-100 text-blue-800" },
            // Urdu
            { q: "Allama Iqbal ki mashhoor kitab kon si hai?", a: "Bang-e-Dra", b: "Shahnamah", ans: "a", cat: "Urdu", color: "bg-green-100 text-green-800" },
            { q: "Urdu ke kitne huroof-e-tahajji hain?", a: "36", b: "39", ans: "b", cat: "Urdu", color: "bg-green-100 text-green-800" },
            // Math
            { q: "If x + 5 = 12, what is x?", a: "7", b: "17", ans: "a", cat: "Mathematics", color: "bg-red-100 text-red-800" },
            { q: "25% of 200 is:", a: "40", b: "50", ans: "b", cat: "Mathematics", color: "bg-red-100 text-red-800" },
            // GK & GB
            { q: "Rakaposhi peak is in which range?", a: "Karakoram", b: "Himalaya", ans: "a", cat: "GB General Knowledge", color: "bg-purple-100 text-purple-800" },
            { q: "Total number of districts in GB?", a: "10", b: "14", ans: "a", cat: "GB General Knowledge", color: "bg-purple-100 text-purple-800" }
        ];

        let mode = '';
        let timer;

        function initModule(m) {
            mode = m;
            document.getElementById('startPage').style.display = 'none';
            if(mode === 'exam') {
                document.getElementById('timerDisplay').classList.remove('hidden');
                startTimer(5400);
            }
            loadQuestions();
        }

        function loadQuestions() {
            // Randomly shuffle questions
            const shuffled = questionBank.sort(() => Math.random() - 0.5);
            const quizArea = document.getElementById('quizArea');
            quizArea.innerHTML = '';

            shuffled.forEach((data, i) => {
                const html = `
                    <div class="q-card">
                        <span class="section-tag ${data.color}">${data.cat}</span>
                        <h3 class="text-xl font-bold text-gray-800 mt-4">${i+1}. ${data.q}</h3>
                        <div class="mt-6 space-y-3">
                            <label class="option-btn" id="q${i}a_label">
                                <input type="radio" name="q${i}" value="a" onchange="handleSelection(this, '${data.ans}', 'q${i}a_label')"> ${data.a}
                            </label>
                            <label class="option-btn" id="q${i}b_label">
                                <input type="radio" name="q${i}" value="b" onchange="handleSelection(this, '${data.ans}', 'q${i}b_label')"> ${data.b}
                            </label>
                        </div>
                    </div>
                `;
                quizArea.insertAdjacentHTML('beforeend', html);
            });
        }

        function handleSelection(input, correct, labelId) {
            if(mode === 'practice') {
                const label = document.getElementById(labelId);
                if(input.value === correct) {
                    label.classList.add('correct');
                } else {
                    label.classList.add('wrong');
                    alert("Galat Jawab! Sahi jawab '" + correct.toUpperCase() + "' tha.");
                }
            }
        }

        function startTimer(sec) {
            let t = sec;
            timer = setInterval(() => {
                let m = Math.floor(t/60); let s = t%60;
                document.getElementById('timerDisplay').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
                if(--t < 0) finishSession();
            }, 1000);
        }

        function showNotes() { document.getElementById('notesModal').classList.remove('hidden'); }
        function hideNotes() { document.getElementById('notesModal').classList.add('hidden'); }
        function finishSession() { alert("Session Completed! Sweetie, aapka result tayar hai."); location.reload(); }
    </script>
</body>
</html>
