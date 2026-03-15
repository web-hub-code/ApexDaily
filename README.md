<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro Academy | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; }
        .glass-nav { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); }
        .level-card { transition: 0.4s; position: relative; cursor: pointer; }
        .locked { filter: grayscale(1); opacity: 0.6; cursor: not-allowed; }
        .book-section { background: white; border-radius: 24px; padding: 25px; border-left: 8px solid #1e3a8a; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
        .tab-content { display: none; }
        .active-tab { display: block; animation: slideUp 0.5s ease; }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    </style>
</head>
<body class="pb-24">

    <div id="loginScreen" class="fixed inset-0 bg-[#0f172a] z-[200] flex items-center justify-center p-6 text-white">
        <div class="bg-white text-slate-900 p-10 rounded-[40px] max-w-md w-full shadow-2xl">
            <h2 class="text-3xl font-black mb-2 italic text-blue-900">GBTS ACADEMY</h2>
            <p class="text-xs font-bold text-gray-400 mb-8 uppercase tracking-widest">Training & Recruitment Portal</p>
            <input id="candidateName" type="text" placeholder="Apna Full Name Likhein" class="w-full p-4 bg-gray-100 rounded-2xl mb-4 border-2 border-transparent focus:border-blue-600 outline-none">
            <button onclick="loginUser()" class="w-full bg-blue-900 text-white py-4 rounded-2xl font-black text-lg shadow-lg hover:bg-black transition">SHURU KAREIN</button>
        </div>
    </div>

    <nav class="glass-nav sticky top-0 z-50 p-4 border-b">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <h1 class="font-black text-blue-900 text-2xl italic">PRIME HUB</h1>
            <div class="flex gap-4">
                <button onclick="showTab('levelsTab')" class="text-xs font-black uppercase text-blue-900">Training</button>
                <button onclick="showTab('bookTab')" class="text-xs font-black uppercase text-gray-400">Study Book</button>
                <button onclick="showTab('adminTab')" class="text-xs font-black uppercase text-gray-400">Admin</button>
            </div>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-4 mt-8">

        <div id="levelsTab" class="tab-content active-tab">
            <div class="mb-10">
                <h2 class="text-4xl font-black text-slate-800 italic uppercase">Mission: 10 Levels</h2>
                <p id="statusMsg" class="text-blue-600 font-bold mt-2">Level 1 is unlocked for you.</p>
            </div>
            <div class="grid grid-cols-2 md:grid-cols-5 gap-6" id="levelContainer">
                </div>
        </div>

        <div id="bookTab" class="tab-content">
            <h2 class="text-4xl font-black text-slate-800 mb-10 italic uppercase">Official Preparation Guide</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="book-section">
                    <h4 class="text-blue-900 font-black text-xl mb-4 underline">ENGLISH (20 Marks)</h4>
                    <ul class="text-sm space-y-2 text-gray-600">
                        <li>• <b>Prepositions:</b> Always use 'to' with Senior, Junior, Inferior.</li>
                        <li>• <b>Vocabulary:</b> Memorize synonyms of common police terms.</li>
                        <li>• <b>Grammar:</b> Practice Active/Passive voice and Tenses.</li>
                    </ul>
                </div>
                <div class="book-section border-green-600">
                    <h4 class="text-green-700 font-black text-xl mb-4 underline">URDU (20 Marks)</h4>
                    <ul class="text-sm space-y-2 text-gray-600">
                        <li>• <b>Mazmoon:</b> Police ka kirdar, Dehshat-gardi, Taleem.</li>
                        <li>• <b>Grammar:</b> Wahid/Jama, Muzakkar/Monas, aur Muhawrat.</li>
                    </ul>
                </div>
                <div class="book-section border-orange-500">
                    <h4 class="text-orange-600 font-black text-xl mb-4 underline">ISLAMIYAT (20 Marks)</h4>
                    <ul class="text-sm space-y-2 text-gray-600">
                        <li>• <b>Ghazwat:</b> Badr (2AH), Uhud (3AH), Khandaq (5AH).</li>
                        <li>• <b>Quran:</b> Total Surahs (114), Total Manzil (7).</li>
                    </ul>
                </div>
                <div class="book-section border-purple-600">
                    <h4 class="text-purple-700 font-black text-xl mb-4 underline">GENERAL KNOWLEDGE</h4>
                    <ul class="text-sm space-y-2 text-gray-600">
                        <li>• <b>GB Facts:</b> 14 Districts, Capital Gilgit, Governor name.</li>
                        <li>• <b>Geography:</b> K2, Nanga Parbat, Deosai Plains.</li>
                    </ul>
                </div>
            </div>
        </div>

        <div id="adminTab" class="tab-content">
            <div class="bg-white p-12 rounded-[40px] text-center border-4 border-dashed border-gray-100">
                <h3 class="text-2xl font-black text-gray-800">Admin Control Panel</h3>
                <p class="text-gray-400 mt-4 mb-8 text-sm">Update questions or reset student progress via Firebase.</p>
                <button class="bg-gray-100 px-10 py-4 rounded-2xl font-bold text-gray-300">DASHBOARD LOCKED</button>
            </div>
        </div>

    </main>

    <div id="testBox" class="hidden fixed inset-0 bg-white z-[300] p-6 overflow-y-auto">
        <div class="max-w-2xl mx-auto">
            <div class="flex justify-between items-center mb-10">
                <h2 id="currentLevelTitle" class="text-3xl font-black text-blue-900 uppercase">LEVEL 01</h2>
                <button onclick="closeTest()" class="text-red-500 font-bold uppercase text-xs">Exit Test</button>
            </div>
            <div id="quizContent" class="space-y-8"></div>
            <button onclick="finishLevel()" class="w-full bg-blue-900 text-white py-6 rounded-3xl mt-12 font-black text-xl">SUBMIT PAPER</button>
        </div>
    </div>

    <script>
        // FIREBASE CONFIGURATION (Aapka Diya Hua Config)
        const firebaseConfig = {
          apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
          authDomain: "dark-web-9.firebaseapp.com",
          projectId: "dark-web-9",
          storageBucket: "dark-web-9.firebasestorage.app",
          messagingSenderId: "564328425161",
          appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        let userData = { name: "", level: 1 };

        // LOGIN LOGIC
        function loginUser() {
            const name = document.getElementById('candidateName').value;
            if(!name) return alert("Please enter your name!");
            
            userData.name = name;
            // Local storage se check karein ya Firebase se
            const savedLevel = localStorage.getItem('gb_level_' + name);
            userData.level = savedLevel ? parseInt(savedLevel) : 1;
            
            document.getElementById('loginScreen').style.display = 'none';
            renderLevels();
        }

        function renderLevels() {
            const container = document.getElementById('levelContainer');
            container.innerHTML = '';
            for(let i=1; i<=10; i++) {
                const isLocked = i > userData.level;
                container.innerHTML += `
                    <div onclick="openTest(${i}, ${isLocked})" class="level-card bg-white p-8 rounded-[35px] shadow-sm text-center ${isLocked ? 'locked' : 'border-2 border-blue-100'}">
                        <div class="text-4xl mb-2">${isLocked ? '🔒' : '⭐'}</div>
                        <h4 class="font-black text-gray-800">LEVEL 0${i}</h4>
                        <p class="text-[8px] font-bold text-blue-600 mt-2">${isLocked ? 'LOCKED' : 'START NOW'}</p>
                    </div>
                `;
            }
        }

        function openTest(num, locked) {
            if(locked) return alert("Pichla level pass karein pehle!");
            document.getElementById('testBox').classList.remove('hidden');
            document.getElementById('currentLevelTitle').innerText = "LEVEL 0" + num;
            
            const quiz = document.getElementById('quizContent');
            quiz.innerHTML = `
                <div class="bg-gray-50 p-8 rounded-[30px] border">
                    <p class="font-bold text-lg">Question: Identify the capital of Gilgit-Baltistan?</p>
                    <div class="mt-4 space-y-2">
                        <label class="block p-4 bg-white rounded-xl border cursor-pointer"><input type="radio" name="q1"> Skardu</label>
                        <label class="block p-4 bg-white rounded-xl border cursor-pointer"><input type="radio" name="q1"> Gilgit</label>
                    </div>
                </div>
            `;
        }

        function finishLevel() {
            alert("Congratulations! Level Passed.");
            userData.level++;
            localStorage.setItem('gb_level_' + userData.name, userData.level);
            // Firebase update (Optional: database.ref('users/' + userData.name).set(userData))
            closeTest();
            renderLevels();
        }

        function closeTest() { document.getElementById('testBox').classList.add('hidden'); }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }
    </script>
</body>
</html>
