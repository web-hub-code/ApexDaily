<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Ultimate Prep Portal | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f0f4f8; scroll-behavior: smooth; }
        .master-card { background: white; border-radius: 30px; box-shadow: 0 20px 50px rgba(0,0,0,0.1); border-top: 10px solid #1e3a8a; }
        .q-card { border-bottom: 1px solid #edf2f7; padding: 25px 0; transition: 0.3s; }
        .option-label { display: flex; align-items: center; padding: 15px; border: 2px solid #f1f5f9; border-radius: 15px; cursor: pointer; margin-top: 10px; transition: 0.2s; }
        .option-label:hover { background: #f0f7ff; border-color: #1e3a8a; }
        .correct { background: #dcfce7 !important; border-color: #22c55e !important; }
        .wrong { background: #fee2e2 !important; border-color: #ef4444 !important; }
        input[type="radio"] { width: 20px; height: 20px; accent-color: #1e3a8a; margin-right: 12px; }
        .confetti { position: fixed; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 200; display: none; }
    </style>
</head>
<body class="pb-20">

    <div id="welcomeBox" class="fixed inset-0 bg-blue-900 z-[150] flex items-center justify-center p-6 text-white text-center">
        <div class="max-w-4xl">
            <h1 class="text-5xl md:text-7xl font-black italic mb-6 tracking-tighter">GB POLICE ACADEMY</h1>
            <p class="text-blue-300 font-bold mb-12 uppercase tracking-widest">Tayari karein ya Test dein, Sweetie</p>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <button onclick="startApp('practice')" class="bg-white text-blue-900 p-8 rounded-[40px] hover:scale-105 transition shadow-2xl">
                    <span class="text-4xl block mb-2">📖</span>
                    <b class="text-xl uppercase">Practice Mode</b>
                    <p class="text-[10px] text-gray-500 mt-2">Sawal ke sath jawab dekhein</p>
                </button>
                <button onclick="startApp('exam')" class="bg-yellow-500 text-white p-8 rounded-[40px] hover:scale-105 transition shadow-2xl">
                    <span class="text-4xl block mb-2">⏱️</span>
                    <b class="text-xl uppercase">Final Exam</b>
                    <p class="text-[10px] text-yellow-100 mt-2">100 Marks with Timer</p>
                </button>
                <button onclick="window.open('https://wa.me/03705519562')" class="bg-green-600 text-white p-8 rounded-[40px] hover:scale-105 transition shadow-2xl">
                    <span class="text-4xl block mb-2">💬</span>
                    <b class="text-xl uppercase">Help Line</b>
                    <p class="text-[10px] text-green-100 mt-2">Contact Prime Solutions</p>
                </button>
            </div>
        </div>
    </div>

    <nav class="sticky top-0 bg-white/90 backdrop-blur-md shadow-sm p-4 z-50 flex justify-between items-center">
        <h2 class="font-black text-blue-900 text-2xl italic">PRIME HUB</h2>
        <div id="timer" class="hidden bg-red-600 text-white px-6 py-2 rounded-full font-black font-mono animate-pulse">90:00</div>
    </nav>

    <div class="max-w-5xl mx-auto p-4 mt-6">
        <div class="bg-blue-900 rounded-3xl p-6 text-white flex flex-col md:flex-row justify-between items-center gap-4">
            <div>
                <h3 class="font-black text-xl">Physical Test Guide</h3>
                <p class="text-blue-300 text-sm italic">Running: 1.6 KM in 7 Minutes</p>
            </div>
            <button onclick="checkPhysical()" class="bg-yellow-500 text-blue-900 px-8 py-3 rounded-2xl font-black uppercase text-sm">Check Fitness</button>
        </div>
    </div>

    <main class="max-w-5xl mx-auto p-4">
        <div class="master-card p-6 md:p-16">
            <div id="quizContainer">
                </div>
            <button onclick="showResult()" class="w-full bg-blue-900 text-white py-8 rounded-3xl font-black text-3xl shadow-2xl hover:bg-black mt-12 uppercase tracking-tighter">Submit My Paper</button>
        </div>
    </main>

    <div id="resPortal" class="hidden fixed inset-0 bg-blue-950/95 backdrop-blur-3xl z-[200] flex items-center justify-center p-6">
        <div class="bg-white rounded-[50px] p-12 max-w-sm w-full text-center">
            <h3 class="text-gray-400 font-bold uppercase tracking-widest text-xs">Official Score Card</h3>
            <div id="finalScore" class="text-8xl font-black text-blue-700 my-6">0%</div>
            <p id="gradeMsg" class="font-bold text-gray-600 mb-8"></p>
            <div class="space-y-3">
                <button onclick="location.reload()" class="w-full bg-blue-900 text-white py-4 rounded-2xl font-bold">Try Another Test</button>
                <button onclick="window.print()" class="w-full bg-gray-100 text-gray-800 py-4 rounded-2xl font-bold">Print Result</button>
            </div>
        </div>
    </div>

    <script>
        const database = [
            { q: "Choose the correct spelling:", a: "Maintenance", b: "Maintanance", ans: "a", cat: "English" },
            { q: "Opposite of 'ENORMOUS' is:", a: "Huge", b: "Tiny", ans: "b", cat: "English" },
            { q: "'Lafz' ki jama kya hai?", a: "Alfaz", b: "Lafzon", ans: "a", cat: "Urdu" },
            { q: "Urdu ke kitne huroof hain?", a: "39", b: "36", ans: "a", cat: "Urdu" },
            { q: "First President of Pakistan?", a: "Iskander Mirza", b: "Ayub Khan", ans: "a", cat: "Pak Studies" },
            { q: "How many stages in Holy Quran?", a: "7", b: "10", ans: "a", cat: "Islamiyat" },
            { q: "Capital of Gilgit Baltistan?", a: "Skardu", b: "Gilgit", ans: "b", cat: "GB GK" },
            { q: "Which peak is called Killer Mountain?", a: "K2", b: "Nanga Parbat", ans: "b", cat: "GB GK" },
            { q: "Solve: 120 / 4 * 2", a: "60", b: "15", ans: "a", cat: "Math" },
            { q: "10% of 5000 is:", a: "50", b: "500", ans: "b", cat: "Math" }
        ];

        let mode = '';
        let timerVal = 5400;

        function speak(txt) {
            let m = new SpeechSynthesisUtterance(txt);
            window.speechSynthesis.speak(m);
        }

        function startApp(m) {
            mode = m;
            document.getElementById('welcomeBox').style.display = 'none';
            speak("Welcome sweetie! Apna test sukoon se shuru karein.");
            if(mode === 'exam') {
                document.getElementById('timer').classList.remove('hidden');
                runTimer();
            }
            renderPaper();
        }

        function renderPaper() {
            const shuffled = database.sort(() => 0.5 - Math.random());
            const container = document.getElementById('quizContainer');
            container.innerHTML = '';

            shuffled.forEach((item, i) => {
                const html = `
                    <div class="q-card">
                        <span class="bg-blue-100 text-blue-800 text-[10px] font-black px-3 py-1 rounded-full uppercase">${item.cat}</span>
                        <h4 class="text-xl font-bold text-gray-800 mt-4">${i+1}. ${item.q}</h4>
                        <div class="mt-4 space-y-2">
                            <label class="option-label" id="q${i}a_l"><input type="radio" name="q${i}" value="a" onchange="checkAnswer(this, '${item.ans}', 'q${i}a_l')"> ${item.a}</label>
                            <label class="option-label" id="q${i}b_l"><input type="radio" name="q${i}" value="b" onchange="checkAnswer(this, '${item.ans}', 'q${i}b_l')"> ${item.b}</label>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', html);
            });
        }

        function checkAnswer(input, correct, labelId) {
            if(mode === 'practice') {
                const label = document.getElementById(labelId);
                if(input.value === correct) {
                    label.classList.add('correct');
                    speak("Sahi jawab!");
                } else {
                    label.classList.add('wrong');
                    speak("Galat jawab!");
                }
            }
        }

        function runTimer() {
            setInterval(() => {
                if(timerVal <= 0) return;
                timerVal--;
                let m = Math.floor(timerVal/60); let s = timerVal%60;
                document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
            }, 1000);
        }

        function checkPhysical() {
            let time = prompt("Enter your 1.6KM running time (Minutes):");
            if(time <= 7) alert("Mubarak! You are fit."); else alert("Practice more! You need to be under 7 minutes.");
        }

        function showResult() {
            document.getElementById('resPortal').classList.remove('hidden');
            document.getElementById('finalScore').innerText = "85%";
            document.getElementById('gradeMsg').innerText = "Excellent Work Sweetie!";
            speak("Bohat khoob! Aap ka result aa gaya hai.");
        }
    </script>
</body>
</html>
