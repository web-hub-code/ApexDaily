<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Official Exam Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; }
        .question-card { background: white; border-radius: 15px; padding: 25px; border-left: 6px solid #1e3a8a; box-shadow: 0 4px 10px rgba(0,0,0,0.05); margin-bottom: 25px; }
        .option-item { display: flex; align-items: center; padding: 14px; border: 1px solid #e2e8f0; border-radius: 12px; cursor: pointer; transition: 0.3s; margin-top: 10px; }
        .option-item:hover { background: #eff6ff; border-color: #1e3a8a; }
        input[type="radio"] { width: 20px; height: 20px; margin-right: 15px; cursor: pointer; accent-color: #1e3a8a; }
        .section-header { background: #1e3a8a; color: white; padding: 10px 20px; border-radius: 8px; display: inline-block; margin-bottom: 30px; font-weight: 800; letter-spacing: 1px; }
    </style>
</head>
<body class="pb-24">

    <header class="bg-blue-900 text-white sticky top-0 z-50 shadow-2xl p-4 md:p-6">
        <div class="max-w-5xl mx-auto flex justify-between items-center">
            <div>
                <h1 class="text-xl md:text-3xl font-black uppercase tracking-tighter">GB POLICE WRITTEN TEST</h1>
                <p class="text-[10px] md:text-xs text-blue-300 italic">Official Recruitment Mock 2026 | Prime Solutions</p>
            </div>
            <div class="bg-blue-800 p-3 rounded-2xl border border-blue-400 text-center">
                <span class="block text-[10px] font-bold text-blue-200">REMAINING TIME</span>
                <div id="timer" class="text-xl font-mono font-bold text-yellow-400">90:00</div>
            </div>
        </div>
    </header>

    <main class="max-w-4xl mx-auto p-4 mt-10">
        <form id="fullExamForm">

            <div class="section-header">SECTION A: ENGLISH</div>
            
            <div class="question-card">
                <p class="font-bold text-lg">1. Choose the correct spelling:</p>
                <label class="option-item"><input type="radio" name="q1" value="A"> Accomodation</label>
                <label class="option-item"><input type="radio" name="q1" value="B"> Accommodation</label>
            </div>

            <div class="question-card">
                <p class="font-bold text-lg">2. "She has been writing for two hours." This is ______ tense.</p>
                <label class="option-item"><input type="radio" name="q2" value="A"> Present Continuous</label>
                <label class="option-item"><input type="radio" name="q2" value="B"> Present Perfect Continuous</label>
            </div>

            <div class="question-card">
                <p class="font-bold text-lg">3. Opposite of 'GIGANTIC' is:</p>
                <label class="option-item"><input type="radio" name="q3" value="A"> Tiny</label>
                <label class="option-item"><input type="radio" name="q3" value="B"> Huge</label>
            </div>

            <div class="section-header bg-green-700">SECTION B: GB GEOGRAPHY</div>

            <div class="question-card">
                <p class="font-bold text-lg">4. What is the total area of Gilgit-Baltistan?</p>
                <label class="option-item"><input type="radio" name="q4" value="A"> 72,496 sq km</label>
                <label class="option-item"><input type="radio" name="q4" value="B"> 85,000 sq km</label>
            </div>

            <div class="question-card">
                <p class="font-bold text-lg">5. Baltistan is also known as:</p>
                <label class="option-item"><input type="radio" name="q5" value="A"> Little Tibet</label>
                <label class="option-item"><input type="radio" name="q5" value="B"> Roof of the world</label>
            </div>

            <div class="question-card">
                <p class="font-bold text-lg">6. The highest plateau in the world, Deosai, is in:</p>
                <label class="option-item"><input type="radio" name="q6" value="A"> Astore</label>
                <label class="option-item"><input type="radio" name="q6" value="B"> Skardu</label>
            </div>

            <div class="section-header bg-yellow-600">SECTION C: ISLAMIYAT</div>

            <div class="question-card">
                <p class="font-bold text-lg">7. Battle of Badr was fought in which Hijri year?</p>
                <label class="option-item"><input type="radio" name="q7" value="A"> 1st Hijri</label>
                <label class="option-item"><input type="radio" name="q7" value="B"> 2nd Hijri</label>
            </div>

            <div class="question-card">
                <p class="font-bold text-lg">8. Who is called 'Saifullah' (The Sword of Allah)?</p>
                <label class="option-item"><input type="radio" name="q8" value="A"> Hazrat Khalid bin Waleed (R.A)</label>
                <label class="option-item"><input type="radio" name="q8" value="B"> Hazrat Ali (R.A)</label>
            </div>

            <div class="section-header bg-red-700">SECTION D: PAKISTAN STUDIES</div>

            <div class="question-card">
                <p class="font-bold text-lg">9. Who was the first Prime Minister of Pakistan?</p>
                <label class="option-item"><input type="radio" name="q9" value="A"> Quaid-e-Azam</label>
                <label class="option-item"><input type="radio" name="q9" value="B"> Liaquat Ali Khan</label>
            </div>

            <div class="question-card">
                <p class="font-bold text-lg">10. The 1973 Constitution declared Pakistan as ______ Republic.</p>
                <label class="option-item"><input type="radio" name="q10" value="A"> Secular</label>
                <label class="option-item"><input type="radio" name="q10" value="B"> Islamic</label>
            </div>

            <button type="button" onclick="submitTest()" class="w-full bg-blue-900 text-white py-6 rounded-2xl font-black text-2xl shadow-2xl hover:bg-black transition transform hover:scale-105 mb-20">
                SUBMIT FINAL TEST
            </button>

        </form>
    </main>

    <div id="resultBox" class="hidden fixed inset-0 bg-black/90 backdrop-blur-md flex items-center justify-center p-4 z-[100]">
        <div class="bg-white p-12 rounded-3xl max-w-md w-full text-center">
            <h2 class="text-2xl font-bold">Exam Result Card</h2>
            <div id="finalScore" class="text-7xl font-black text-blue-700 my-6">0%</div>
            <p id="feedback" class="text-lg font-bold italic mb-8"></p>
            <button onclick="location.reload()" class="bg-blue-900 text-white w-full py-4 rounded-xl font-bold">Restart Exam</button>
        </div>
    </div>

    <script>
        const key = { q1: "B", q2: "B", q3: "A", q4: "A", q5: "A", q6: "B", q7: "B", q8: "A", q9: "B", q10: "B" };

        function submitTest() {
            let score = 0;
            const total = Object.keys(key).length;
            const data = new FormData(document.getElementById('fullExamForm'));

            for(let q in key) {
                if(data.get(q) === key[q]) score++;
            }

            const perc = Math.round((score / total) * 100);
            document.getElementById('finalScore').innerText = perc + "%";
            document.getElementById('feedback').innerText = perc >= 50 ? "PASS: Well Done!" : "FAIL: Keep Practicing.";
            document.getElementById('resultBox').classList.remove('hidden');
        }

        let secondsLeft = 5400;
        setInterval(() => {
            if(secondsLeft <= 0) return;
            secondsLeft--;
            let m = Math.floor(secondsLeft / 60);
            let s = secondsLeft % 60;
            document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
        }, 1000);
    </script>
</body>
</html>
