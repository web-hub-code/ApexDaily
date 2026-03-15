<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro Academy | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; }
        .glass-nav { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); }
        .level-card { transition: 0.4s; position: relative; cursor: pointer; border-radius: 30px; }
        .locked { filter: grayscale(1); opacity: 0.6; cursor: not-allowed; background: #e2e8f0 !important; }
        .book-section { background: white; border-radius: 24px; padding: 25px; border-top: 6px solid #1e3a8a; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
        .tab-content { display: none; }
        .active-tab { display: block; animation: slideUp 0.5s ease; }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    </style>
</head>
<body class="pb-24">

    <div id="loginScreen" class="fixed inset-0 bg-[#0f172a] z-[200] flex items-center justify-center p-6 text-white hidden">
        <div class="bg-white text-slate-900 p-10 rounded-[40px] max-w-md w-full shadow-2xl">
            <h2 class="text-3xl font-black mb-2 text-blue-900 italic">GBTS ACADEMY</h2>
            <p class="text-xs font-bold text-gray-400 mb-8 uppercase tracking-widest">Candidate Secure Login</p>
            <input id="candidateName" type="text" placeholder="Apna Full Name Likhein" class="w-full p-4 bg-gray-100 rounded-2xl mb-4 border-2 border-transparent focus:border-blue-600 outline-none">
            <button onclick="loginUser()" class="w-full bg-blue-900 text-white py-4 rounded-2xl font-black text-lg shadow-lg hover:bg-black transition">Dakhila Lein</button>
        </div>
    </div>

    <nav class="glass-nav sticky top-0 z-50 p-4 border-b">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <h1 class="font-black text-blue-900 text-2xl italic">PRIME HUB</h1>
            <div class="flex gap-4">
                <button onclick="showTab('levelsTab')" class="text-xs font-black uppercase text-blue-900">Training</button>
                <button onclick="showTab('bookTab')" class="text-xs font-black uppercase text-gray-500">Full Book</button>
                <button onclick="logout()" class="text-xs font-black uppercase text-red-500">Logout</button>
            </div>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-4 mt-8">
        
        <div id="levelsTab" class="tab-content active-tab">
            <h2 class="text-3xl font-black mb-8 italic uppercase text-slate-800">Mission: 10 Advanced Levels</h2>
            <div class="grid grid-cols-2 md:grid-cols-5 gap-6" id="levelContainer"></div>
        </div>

        <div id="bookTab" class="tab-content">
            <h2 class="text-3xl font-black mb-8 italic uppercase text-slate-800">Complete Study Material</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="book-section">
                    <h3 class="font-black text-blue-900 text-xl mb-4">📘 English Language (Full Details)</h3>
                    <div class="text-sm space-y-4 text-gray-600">
                        <p><b>Prepositions:</b> GB Police tests mein ye sab se zaroori hain. Yaad rakhein: <i>Accused of, Aware of, Die of (disease), Senior to, Differ with.</i></p>
                        <p><b>Active/Passive:</b> Tenses ka khayal rakhein. Present Continuous mein 'Being' lagta hai.</p>
                        <p><b>Spelling Correction:</b> 'Lieutenant', 'Committee', 'Pneumonia' jaise words baar baar aate hain.</p>
                    </div>
                </div>
                <div class="book-section">
                    <h3 class="font-black text-blue-900 text-xl mb-4">🌍 General Knowledge & GB</h3>
                    <div class="text-sm space-y-4 text-gray-600">
                        <p><b>GB Districts:</b> Gilgit-Baltistan mein kul 14 districts hain (3 Divisions: Gilgit, Baltistan, Diamer).</p>
                        <p><b>Mountains:</b> K2 Karakoram range mein hai. Nanga Parbat Himalayas mein hai.</p>
                        <p><b>World Facts:</b> Largest Ocean (Pacific), Smallest Country (Vatican City).</p>
                    </div>
                </div>
                <div class="book-section">
                    <h3 class="font-black text-blue-900 text-xl mb-4">🔢 Mathematics & IQ</h3>
                    <div class="text-sm space-y-4 text-gray-600">
                        <p><b>BODMAS:</b> Pehle Bracket, phir Division, phir Multiplication solve karein.</p>
                        <p><b>Percentage:</b> Value ko 100 se divide kar ke percentage nikaali jati hai.</p>
                        <p><b>Ratio:</b> Comparison of two numbers.</p>
                    </div>
                </div>
                <div class="book-section">
                    <h3 class="font-black text-blue-900 text-xl mb-4">🕌 Islamiyat (Complete)</h3>
                    <div class="text-sm space-y-4 text-gray-600">
                        <p><b>Quran:</b> 114 Surahs, 6666 Ayats, 7 Manzils, 30 Paras.</p>
                        <p><b>Ghazwat:</b> Badr (2AH), Uhud (3AH), Khandaq (5AH), Khyber (7AH), Makkah Conquer (8AH).</p>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <div id="testBox" class="hidden fixed inset-0 bg-white z-[300] p-6 overflow-y-auto">
        <div class="max-w-2xl mx-auto">
            <h2 id="currentLevelTitle" class="text-4xl font-black text-blue-900 mb-10 uppercase italic">Level 01</h2>
            <div id="quizContent" class="space-y-10"></div>
            <button onclick="processTest()" class="w-full bg-blue-900 text-white py-6 rounded-3xl mt-12 font-black text-xl shadow-xl">Level Complete Karein</button>
        </div>
    </div>

    <script>
        const firebaseConfig = {
          apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
          authDomain: "dark-web-9.firebaseapp.com",
          projectId: "dark-web-9",
          storageBucket: "dark-web-9.firebasestorage.app",
          messagingSenderId: "564328425161",
          appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(firebaseConfig);

        let user = { name: "", level: 1 };

        // PERSISTENT LOGIN CHECK
        window.onload = () => {
            const savedName = localStorage.getItem('gb_academy_user');
            if (savedName) {
                user.name = savedName;
                user.level = parseInt(localStorage.getItem('gb_academy_level_' + savedName)) || 1;
                renderLevels();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function loginUser() {
            const name = document.getElementById('candidateName').value;
            if(!name) return alert("Apna naam likhein!");
            user.name = name;
            localStorage.setItem('gb_academy_user', name);
            if(!localStorage.getItem('gb_academy_level_' + name)) {
                localStorage.setItem('gb_academy_level_' + name, 1);
            }
            user.level = parseInt(localStorage.getItem('gb_academy_level_' + name));
            document.getElementById('loginScreen').classList.add('hidden');
            renderLevels();
        }

        function logout() {
            localStorage.removeItem('gb_academy_user');
            location.reload();
        }

        function renderLevels() {
            const container = document.getElementById('levelContainer');
            container.innerHTML = '';
            for(let i=1; i<=10; i++) {
                const isLocked = i > user.level;
                container.innerHTML += `
                    <div onclick="openTest(${i}, ${isLocked})" class="level-card bg-white p-8 shadow-sm text-center border-2 ${isLocked ? 'locked' : 'border-blue-100 hover:border-blue-500'}">
                        <div class="text-4xl mb-4">${isLocked ? '🔒' : '⭐'}</div>
                        <h4 class="font-black text-slate-800">LEVEL ${i}</h4>
                        <p class="text-[10px] font-bold text-gray-400 mt-2">${isLocked ? 'LOCKED' : 'AVAILABLE'}</p>
                    </div>
                `;
            }
        }

        function openTest(num, locked) {
            if(locked) return alert("Pehle pichla level pass karein!");
            document.getElementById('testBox').classList.remove('hidden');
            document.getElementById('currentLevelTitle').innerText = "LEVEL 0" + num;
            
            // Hard Dynamic Questions Logic
            const quiz = document.getElementById('quizContent');
            if(num === 1) {
                quiz.innerHTML = `<div class='p-8 bg-blue-50 rounded-[40px]'><p class='font-bold text-xl'>Q1: Siachen Glacier ki lambai kitni hai?</p><input type='radio' name='q' class='mt-4'> 76 KM</div>`;
            } else if(num === 2) {
                quiz.innerHTML = `<div class='p-8 bg-blue-50 rounded-[40px]'><p class='font-bold text-xl'>Q2: Hazrat Muhammad (S.A.W) ne kul kitne Hajj kiye?</p><input type='radio' name='q' class='mt-4'> 1 Hajj</div>`;
            } else {
                quiz.innerHTML = `<div class='p-8 bg-blue-50 rounded-[40px]'><p class='font-bold text-xl'>Q: Level ${num} ka advanced question loading...</p></div>`;
            }
        }

        function processTest() {
            user.level++;
            localStorage.setItem('gb_academy_level_' + user.name, user.level);
            alert("Mubarak ho! Agla level unlock ho gaya hai.");
            document.getElementById('testBox').classList.add('hidden');
            renderLevels();
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }
    </script>
</body>
</html>
