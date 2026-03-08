<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Prime Solutions Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        :root { --bg: #020617; --card: rgba(30, 41, 59, 0.5); --gold: #fbbf24; --border: rgba(251, 191, 36, 0.1); }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: white; transition: 0.3s; padding-bottom: 100px; }
        .glass { background: var(--card); backdrop-filter: blur(12px); border: 1px solid var(--border); border-radius: 24px; }
        .page { display: none; animation: slideUp 0.4s ease forwards; }
        .active-page { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .btn-gold { background: linear-gradient(135deg, #b45309, #fbbf24); color: #000; font-weight: 800; border-radius: 16px; }
        .machine-glow { box-shadow: 0 0 15px rgba(251, 191, 36, 0.1); border-left: 4px solid var(--gold); }
        .premium-glow { border-left: 4px solid #10b981; box-shadow: 0 0 15px rgba(16, 185, 129, 0.1); }
        .animate-marquee { display: inline-block; animation: marquee 15s linear infinite; }
        @keyframes marquee { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
        ::-webkit-scrollbar { display: none; }
    </style>
</head>
<body>

    <header class="p-4 flex justify-between items-center sticky top-0 bg-[#020617]/80 backdrop-blur-md border-b border-white/5 z-[5000]">
        <div class="flex items-center gap-2">
            <div onclick="adminTap()" class="w-9 h-9 bg-yellow-500 rounded-xl flex items-center justify-center font-black text-black italic">P</div>
            <span class="font-black text-xs uppercase tracking-tighter">Pak<span class="text-yellow-500">Gold</span></span>
        </div>
        <div id="top-bal" class="text-[10px] font-black bg-yellow-500/10 text-yellow-500 px-4 py-1.5 rounded-full border border-yellow-500/20 italic">₨ 0.00</div>
    </header>

    <div class="p-4">
        <div class="bg-yellow-500/5 border border-yellow-500/10 p-2 rounded-xl overflow-hidden whitespace-nowrap">
            <p id="v-news" class="animate-marquee inline-block text-[8px] font-black uppercase text-yellow-500 italic">
                Welcome to PakGold! Start Mining with only ₨ 200 | Prime Solutions Node Active... 😘
            </p>
        </div>
    </div>

    <section id="auth-ui" class="fixed inset-0 z-[20000] bg-[#020617] flex flex-col items-center justify-center p-8 text-center">
        <h1 class="text-3xl font-black italic tracking-tighter mb-8 uppercase text-yellow-500">Initialize Node</h1>
        <div class="w-full max-w-[320px] space-y-4">
            <input type="text" id="user-name" placeholder="Username" class="w-full p-4 bg-white/5 border border-white/10 rounded-2xl text-center font-bold outline-none">
            <input type="password" id="user-pass" placeholder="Password" class="w-full p-4 bg-white/5 border border-white/10 rounded-2xl text-center font-bold outline-none">
            <button onclick="login()" class="w-full p-4 btn-gold uppercase text-[10px] tracking-widest shadow-xl">Authorize Access 🚀</button>
        </div>
    </section>

    <main id="app-ui" class="hidden p-4 space-y-6">
        
        <div id="p-home" class="page active-page space-y-6">
            <div class="p-8 rounded-[2.5rem] bg-gradient-to-br from-yellow-600 to-yellow-900 text-black shadow-2xl relative overflow-hidden">
                <p class="text-[10px] font-black uppercase opacity-60 tracking-widest">Active Balance</p>
                <h2 class="text-4xl font-black tracking-tighter mt-1" id="v-bal">₨ 0.00</h2>
                <div class="mt-6 flex justify-between items-center bg-black/20 p-4 rounded-2xl backdrop-blur-md text-white">
                    <span class="text-[8px] font-bold uppercase italic">Server: Online</span>
                    <div class="w-24 h-1 bg-white/20 rounded-full overflow-hidden"><div class="bg-white h-full animate-pulse" style="width: 60%"></div></div>
                </div>
            </div>

            <div class="flex gap-2 p-1 bg-white/5 rounded-2xl">
                <button onclick="renderPlans('normal')" id="btn-normal" class="flex-1 p-3 rounded-xl bg-yellow-500 text-black text-[9px] font-black uppercase">15 Standard Nodes</button>
                <button onclick="renderPlans('special')" id="btn-special" class="flex-1 p-3 rounded-xl text-white text-[9px] font-black uppercase opacity-40">5 VIP Machines</button>
            </div>
            
            <div id="plans-grid" class="grid grid-cols-1 gap-4"></div>
        </div>

        <div id="p-wallet" class="page space-y-6">
            <div class="glass p-6 space-y-4">
                <h3 class="text-[10px] font-black uppercase text-yellow-500 tracking-widest">Payment Methods</h3>
                <div class="space-y-2 text-[11px] font-bold">
                    <div class="flex justify-between p-3 bg-white/5 rounded-xl border-l-2 border-green-500"><span>EasyPaisa</span><span>033379827882</span></div>
                    <div class="flex justify-between p-3 bg-white/5 rounded-xl border-l-2 border-red-500"><span>JazzCash</span><span>03705519562</span></div>
                </div>
                <input type="number" id="dep-amt" placeholder="Amount (₨)" class="w-full p-4 bg-white/5 border border-white/10 rounded-xl outline-none">
                <input type="text" id="dep-tid" placeholder="Transaction ID" class="w-full p-4 bg-white/5 border border-white/10 rounded-xl outline-none">
                <button onclick="submitDep()" class="w-full p-4 btn-gold uppercase text-[10px] tracking-widest">Submit Proof ⚡</button>
            </div>
        </div>

        <div id="p-about" class="page space-y-6">
            <div class="glass p-6 space-y-4 text-[11px] font-bold opacity-80 leading-relaxed">
                <h3 class="text-yellow-500 uppercase">Prime Solutions Official</h3>
                <p>PakGold is a secure cloud-mining infrastructure. We use high-end nodes to generate daily yields for our investors.</p>
                <div class="h-[1px] bg-white/5"></div>
                <h4 class="text-yellow-500 uppercase">Privacy Policy</h4>
                <p>All data is encrypted via Firebase. Withdrawals are processed within 24 hours. Support: webhub262@gmail.com</p>
            </div>
        </div>
    </main>

    <nav id="bottom-nav" class="hidden fixed bottom-0 w-full glass border-t border-white/10 p-4 flex justify-around items-center rounded-t-[2.5rem] z-[4000]">
        <button onclick="changePage('home')" id="n-home" class="flex flex-col items-center gap-1 text-yellow-500"><span class="text-lg">🏠</span><span class="text-[7px] font-black">NODE</span></button>
        <button onclick="changePage('wallet')" id="n-wallet" class="flex flex-col items-center gap-1 opacity-40"><span class="text-lg">📥</span><span class="text-[7px] font-black">FUND</span></button>
        <button onclick="changePage('about')" id="n-about" class="flex flex-col items-center gap-1 opacity-40"><span class="text-lg">🏢</span><span class="text-[7px] font-black">LEGAL</span></button>
        <button onclick="logout()" class="flex flex-col items-center gap-1 opacity-40"><span class="text-lg">❌</span><span class="text-[7px] font-black">EXIT</span></button>
    </nav>

    <script>
        // --- 1. FIREBASE SETUP ---
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let user = null;

        // --- 2. MODERN MACHINE DATA (15+5) ---
        const STANDARD_NODES = [];
        for(let i=1; i<=15; i++) {
            let prc = 200 + (i-1)*1000;
            let prf = (10 + (i*5)).toFixed(0);
            STANDARD_NODES.push({ id: `S-NODE ${i}`, price: prc, profit: prf });
        }
        const VIP_NODES = [
            { id: "GOLD VIP-1", price: 50000, profit: 4500 },
            { id: "DIAMOND VIP-2", price: 100000, profit: 10000 },
            { id: "PLATINUM VIP-3", price: 250000, profit: 28000 },
            { id: "MASTER VIP-4", price: 500000, profit: 60000 },
            { id: "ULTRA VIP-5", price: 1000000, profit: 150000 }
        ];

        // --- 3. CORE FUNCTIONS ---
        async function login() {
            const n = document.getElementById('user-name').value.trim().toLowerCase();
            const p = document.getElementById('user-pass').value.trim();
            if(!n || !p) return alert("Credentials required! 😘");
            const ref = db.collection("users").doc(n);
            const d = await ref.get();
            if(!d.exists) { await ref.set({ name: n, password: p, balance: 0, time: Date.now() }); }
            else if(d.data().password !== p) return alert("Wrong pass! 😘");
            localStorage.setItem('pakgold_user', n);
            location.reload();
        }

        function sync(n) {
            db.collection("users").doc(n).onSnapshot(d => {
                user = d.data();
                const b = (user.balance || 0).toLocaleString();
                document.getElementById('v-bal').innerText = "₨ " + b;
                document.getElementById('top-bal').innerText = "₨ " + b;
            });
        }

        function renderPlans(cat) {
            const grid = document.getElementById('plans-grid');
            const data = cat === 'normal' ? STANDARD_NODES : VIP_NODES;
            grid.innerHTML = '';
            data.forEach(p => {
                grid.innerHTML += `
                <div class="glass p-5 flex justify-between items-center ${cat==='special'?'premium-glow':'machine-glow'}">
                    <div>
                        <p class="text-[9px] font-black uppercase text-yellow-500">${p.id}</p>
                        <h3 class="text-lg font-black tracking-tighter">₨ ${p.price.toLocaleString()}</h3>
                    </div>
                    <div class="text-right">
                        <p class="text-[10px] font-black text-green-500">+ ₨ ${p.profit}</p>
                        <button onclick="buyNode('${p.id}', ${p.price})" class="mt-2 bg-white/5 border border-white/10 px-4 py-1.5 rounded-lg text-[8px] font-black uppercase">Activate</button>
                    </div>
                </div>`;
            });
        }

        function changePage(p) {
            document.querySelectorAll('.page').forEach(pg=>pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b=>b.classList.add('opacity-40'));
            document.getElementById('n-'+p).classList.remove('opacity-40');
        }

        window.onload = () => {
            const saved = localStorage.getItem('pakgold_user');
            if(saved) {
                document.getElementById('auth-ui').classList.add('hidden');
                document.getElementById('app-ui').classList.remove('hidden');
                document.getElementById('bottom-nav').classList.remove('hidden');
                sync(saved);
                renderPlans('normal');
            }
        };

        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }
    </script>
</body>
</html>
