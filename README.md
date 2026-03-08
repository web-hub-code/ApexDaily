<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Premium Mining Infrastructure</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap');
        
        :root { --gold: #fbbf24; --dark-bg: #06090f; --card-bg: #0d1117; }
        
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--dark-bg); color: #f0f6fc; padding-bottom: 100px; overflow-x: hidden; }
        
        /* Glass-morphism Effect like ApexDaily */
        .glass-card { 
            background: rgba(13, 17, 23, 0.8); 
            backdrop-filter: blur(12px); 
            border: 1px solid rgba(255, 255, 255, 0.05); 
            border-radius: 28px; 
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); 
        }
        
        .glass-card:active { transform: scale(0.96); }

        .page { display: none; }
        .active-page { display: block !important; animation: slideUp 0.5s ease-out; }

        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* Premium Gold Gradient */
        .btn-premium { 
            background: linear-gradient(135deg, #fbbf24 0%, #d97706 100%); 
            color: #000; font-weight: 800; border-radius: 18px; 
            box-shadow: 0 10px 20px rgba(217, 119, 6, 0.2);
        }

        .nav-active { color: var(--gold) !important; filter: drop-shadow(0 0 8px var(--gold)); }

        /* Floating News Bar */
        .marquee-box { background: rgba(251, 191, 36, 0.05); border-y: 1px solid rgba(251, 191, 36, 0.1); }
        marquee { font-size: 11px; font-weight: 700; color: var(--gold); padding: 8px 0; text-transform: uppercase; letter-spacing: 1px; }
    </style>
</head>
<body>

    <header class="p-6 flex justify-between items-center sticky top-0 bg-[#06090f]/80 backdrop-blur-lg z-[10000]">
        <div onclick="adminAccess()">
            <h1 class="text-xl font-black italic tracking-tighter text-yellow-500">PAK<span class="text-white">GOLD</span></h1>
            <p class="text-[7px] font-bold opacity-40 uppercase tracking-[2px]">Enterprise Grade</p>
        </div>
        <div class="flex gap-4 items-center">
            <div class="text-right">
                <p id="u-bal" class="text-lg font-black tracking-tighter">₨ 0.00</p>
                <p class="text-[7px] opacity-40 font-bold uppercase">Balance</p>
            </div>
            <div class="w-10 h-10 rounded-full bg-gradient-to-tr from-yellow-500 to-yellow-800 border-2 border-white/10 flex items-center justify-center font-black text-black">A</div>
        </div>
    </header>

    <div class="marquee-box mb-6"><marquee id="v-news">Loading System Announcements...</marquee></div>

    <main class="px-6 space-y-8">
        
        <div id="p-home" class="page active-page space-y-6">
            <h3 class="text-xs font-black uppercase tracking-[3px] opacity-60">Available Mining Nodes</h3>
            <div id="m-grid" class="space-y-4">
                </div>
        </div>

        <div id="p-fund" class="page space-y-6">
            <div class="glass-card p-8 text-center border-yellow-500/20">
                <h3 class="text-yellow-500 font-black uppercase text-xs tracking-widest mb-6">Redeem Promo Code</h3>
                <input type="text" id="promo-input" placeholder="ENTER CODE (e.g. GOLD786)" class="w-full bg-[#161b22] p-4 rounded-2xl border border-white/5 mb-4 text-center font-black uppercase tracking-widest">
                <button onclick="applyPromo()" class="w-full p-4 btn-premium text-xs uppercase tracking-widest">Claim Reward</button>
            </div>

            <div class="glass-card p-8">
                <h3 class="text-white font-black uppercase text-xs tracking-widest mb-6">Manual Recharge</h3>
                <div class="space-y-4 mb-8">
                    <div class="flex justify-between p-4 bg-white/5 rounded-2xl border-l-4 border-yellow-500">
                        <span class="text-[10px] opacity-60 font-bold uppercase">EasyPaisa</span>
                        <span class="text-sm font-black">03379827882</span>
                    </div>
                </div>
                <input type="number" id="dep-amt" placeholder="Amount" class="w-full bg-[#161b22] p-4 rounded-2xl mb-3">
                <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="w-full bg-[#161b22] p-4 rounded-2xl mb-6">
                <button onclick="submitRequest('deposit')" class="w-full p-4 bg-white/5 rounded-2xl font-black uppercase text-[10px] border border-white/10">Submit Deposit</button>
            </div>
        </div>

        <div id="p-admin" class="page space-y-6">
            <div class="glass-card p-8 border-red-500/30">
                <h2 class="text-red-500 font-black uppercase text-xs mb-6">Authority Dashboard</h2>
                <div id="admin-requests" class="space-y-4">
                    </div>
            </div>
        </div>

    </main>

    <nav class="fixed bottom-6 left-6 right-6 h-20 glass-card flex justify-around items-center px-4 z-[10000]">
        <button onclick="showPage('home')" id="n-home" class="nav-active flex flex-col items-center gap-1">
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24"><path d="M11.5 2L15 6.5L10.5 10L14.5 15.5L9 22H11L15.5 16L11 11L15.5 5.5L11.5 2Z"/></svg>
            <span class="text-[8px] font-black uppercase">Nodes</span>
        </button>
        <button onclick="showPage('fund')" id="n-fund" class="opacity-40 flex flex-col items-center gap-1">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.407 2.67 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.407-2.67-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
            <span class="text-[8px] font-black uppercase">Wallet</span>
        </button>
        <button onclick="showPage('admin')" id="n-admin" class="hidden opacity-40 flex flex-col items-center gap-1 text-red-500">
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24"><path d="M12 1L3 5v6c0 5.55 3.84 10.74 9 12 5.16-1.26 9-6.45 9-12V5l-9-4zm0 6c1.4 0 2.5 1.1 2.5 2.5S13.4 12 12 12s-2.5-1.1-2.5-2.5S10.6 7 12 7z"/></svg>
            <span class="text-[8px] font-black uppercase text-red-500">Admin</span>
        </button>
    </nav>

    <script>
        // FIREBASE INITIALIZATION
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let currentUserId = localStorage.getItem('pg_user');

        // PROMO CODE SYSTEM
        async function applyPromo() {
            const code = document.getElementById('promo-input').value.toUpperCase();
            if(code === 'GOLD786') {
                await db.collection("users").doc(currentUserId).update({ balance: firebase.firestore.FieldValue.increment(100) });
                alert("₨ 100 Reward Added Successfully!");
                document.getElementById('promo-input').value = '';
            } else { alert("Invalid or Expired Code"); }
        }

        // DYNAMIC NODE RENDER (ApexDaily Style)
        function renderNodes() {
            const grid = document.getElementById('m-grid');
            const nodes = [
                { name: "QUARTZ-01", cost: 1200, profit: 95 },
                { name: "AMBER-05", cost: 4500, profit: 380 },
                { name: "ONYX-PRO", cost: 15000, profit: 1400 }
            ];
            nodes.forEach(n => {
                grid.innerHTML += `
                <div class="glass-card p-6 flex justify-between items-center relative overflow-hidden group">
                    <div class="absolute top-0 left-0 w-1 h-full bg-yellow-500"></div>
                    <div>
                        <p class="text-[8px] font-black text-yellow-500 uppercase tracking-widest mb-1">${n.name}</p>
                        <h4 class="text-xl font-black italic">₨ ${n.cost.toLocaleString()}</h4>
                        <p class="text-[9px] opacity-40 font-bold mt-1">Daily Yield: ₨ ${n.profit}</p>
                    </div>
                    <button class="px-6 py-3 btn-premium text-[9px] uppercase tracking-widest">Purchase</button>
                </div>`;
            });
        }

        // AUTH & DASHBOARD SYNC
        function checkUser() {
            if(currentUserId) {
                db.collection("users").doc(currentUserId).onSnapshot(d => {
                    if(!d.exists) db.collection("users").doc(currentUserId).set({ balance: 0 });
                    document.getElementById('u-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                db.collection("settings").doc("news").onSnapshot(d => {
                    if(d.exists) document.getElementById('v-news').innerText = d.data().text;
                });
                if(localStorage.getItem('is_admin') === 'true') {
                    document.getElementById('n-admin').classList.remove('hidden');
                    loadAdminPanel();
                }
                renderNodes();
            } else {
                let n = prompt("Create Username:");
                if(n) { localStorage.setItem('pg_user', n.toLowerCase()); location.reload(); }
            }
        }

        // ADMIN SECRETS (10 Taps on Logo)
        let taps = 0;
        function adminAccess() {
            taps++; if(taps === 10) {
                let p = prompt("Admin Key:");
                if(p === "786") { localStorage.setItem('is_admin', 'true'); location.reload(); }
            }
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('nav-active', 'opacity-100'));
            document.getElementById('n-'+p).classList.add('nav-active');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = checkUser;
    </script>
</body>
</html>
