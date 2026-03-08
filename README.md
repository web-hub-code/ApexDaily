<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Professional Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: #06090f; color: #f0f6fc; padding-bottom: 90px; }
        .sky-card { background: #0d1117; border: 1px solid #30363d; border-radius: 20px; transition: 0.3s; }
        .page { display: none; }
        .active-page { display: block !important; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .btn-gold { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 12px; }
        .nav-active { color: #fbbf24 !important; border-top: 2px solid #fbbf24; }
        input { background: #161b22; border: 1px solid #30363d; border-radius: 12px; padding: 12px; color: white; width: 100%; outline: none; }
    </style>
</head>
<body>

    <main id="app-ui" class="hidden">
        
        <header class="p-6 sticky top-0 bg-[#06090f]/90 backdrop-blur-md z-[5000] border-b border-white/5">
            <div class="flex justify-between items-center mb-6">
                <h1 onclick="adminAccess()" class="text-xl font-black text-yellow-500 italic uppercase">PAK<span class="text-white">GOLD</span></h1>
                <button onclick="logout()" class="bg-red-500/10 text-red-500 text-[9px] font-black px-4 py-2 rounded-xl uppercase tracking-widest">Exit</button>
            </div>
            <div class="sky-card p-6 bg-gradient-to-br from-[#1c2128] to-[#0d1117]">
                <p class="text-[8px] font-bold opacity-40 uppercase tracking-[3px]">Total Assets</p>
                <h2 class="text-4xl font-black mt-1 tracking-tighter" id="u-bal">₨ 0.00</h2>
                <div class="grid grid-cols-2 gap-3 mt-6">
                    <button onclick="showPage('fund')" class="p-3 bg-yellow-500 text-black rounded-xl text-[10px] font-black uppercase">Recharge</button>
                    <button onclick="showPage('withdraw')" class="p-3 bg-white/5 rounded-xl text-[10px] font-black uppercase border border-white/10">Withdraw</button>
                </div>
            </div>
        </header>

        <div class="p-4 space-y-6">
            <div id="p-home" class="page active-page space-y-4">
                <div id="m-grid" class="space-y-4"></div>
            </div>

            <div id="p-team" class="page space-y-4">
                <div class="sky-card p-6 text-center">
                    <h3 class="text-yellow-500 font-black text-xs uppercase mb-2">Your Referral Link</h3>
                    <p id="ref-link" class="text-[10px] bg-white/5 p-3 rounded-xl border border-dashed border-white/10 font-mono mb-4 text-blue-400 break-all select-all">Loading link...</p>
                    <div class="grid grid-cols-3 gap-2">
                        <div class="bg-white/5 p-3 rounded-xl border border-white/5">
                            <p class="text-[7px] opacity-40 uppercase">Level 1</p>
                            <p id="l1-count" class="text-xs font-black">0</p>
                            <p class="text-[6px] text-green-500 font-bold">10% Comm</p>
                        </div>
                        <div class="bg-white/5 p-3 rounded-xl border border-white/5">
                            <p class="text-[7px] opacity-40 uppercase">Level 2</p>
                            <p id="l2-count" class="text-xs font-black">0</p>
                            <p class="text-[6px] text-yellow-500 font-bold">5% Comm</p>
                        </div>
                        <div class="bg-white/5 p-3 rounded-xl border border-white/5">
                            <p class="text-[7px] opacity-40 uppercase">Level 3</p>
                            <p id="l3-count" class="text-xs font-black">0</p>
                            <p class="text-[6px] text-orange-500 font-bold">2% Comm</p>
                        </div>
                    </div>
                </div>
                <h3 class="text-xs font-black uppercase text-yellow-500 ml-1">Team Members</h3>
                <div id="team-list" class="space-y-2">
                    </div>
            </div>

            <div id="p-fund" class="page space-y-4">
                <div class="sky-card p-6">
                    <h3 class="text-yellow-500 font-black text-xs uppercase mb-6 tracking-widest">Recharge Fund</h3>
                    <input type="number" id="dep-amt" placeholder="Amount" class="w-full mb-3">
                    <input type="text" id="dep-tid" placeholder="TID Number" class="w-full mb-4">
                    <button onclick="submitRequest('deposit')" class="w-full p-4 btn-gold uppercase text-[10px]">Submit</button>
                </div>
            </div>
            
            <div id="p-withdraw" class="page space-y-4">
                <div class="sky-card p-6">
                    <h3 class="text-yellow-500 font-black text-xs uppercase mb-6 tracking-widest">Withdrawal</h3>
                    <input type="number" id="wit-amt" placeholder="Amount" class="w-full mb-3">
                    <input type="text" id="wit-acc" placeholder="Account Details" class="w-full mb-4">
                    <button onclick="submitRequest('withdrawal')" class="w-full p-4 btn-gold uppercase text-[10px]">Submit</button>
                </div>
            </div>
        </div>
    </main>

    <nav id="nav-ui" class="hidden fixed bottom-0 w-full p-4 flex justify-around items-center bg-[#0d1117]/95 border-t border-white/5 rounded-t-[2.5rem] z-[5000]">
        <button onclick="showPage('home')" id="n-home" class="flex flex-col items-center gap-1 nav-active pt-2"><span class="text-xl">⚡</span><span class="text-[7px] font-black uppercase">Nodes</span></button>
        <button onclick="showPage('team')" id="n-team" class="flex flex-col items-center gap-1 opacity-40 pt-2"><span class="text-xl">👥</span><span class="text-[7px] font-black uppercase">Team</span></button>
        <button onclick="showPage('fund')" id="n-fund" class="flex flex-col items-center gap-1 opacity-40 pt-2"><span class="text-xl">💰</span><span class="text-[7px] font-black uppercase">Asset</span></button>
    </nav>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let currentUserId = localStorage.getItem('pg_user');

        // --- TEAM SYSTEM LOGIC ---
        function loadTeam() {
            document.getElementById('ref-link').innerText = "https://pakgold.net/join?ref=" + currentUserId;
            
            // Level 1 logic
            db.collection("users").where("referrer", "==", currentUserId).onSnapshot(snap => {
                document.getElementById('l1-count').innerText = snap.size;
                const list = document.getElementById('team-list');
                list.innerHTML = '';
                snap.forEach(doc => {
                    list.innerHTML += `<div class="sky-card p-4 text-[10px] flex justify-between"><span>${doc.id}</span><span class="text-green-500 font-bold">Lvl 1</span></div>`;
                });
            });
        }

        // --- CORE FUNCTIONS ---
        function checkUser() {
            if(currentUserId) {
                document.getElementById('app-ui').classList.remove('hidden');
                document.getElementById('nav-ui').classList.remove('hidden');
                db.collection("users").doc(currentUserId).onSnapshot(d => {
                    if(!d.exists) db.collection("users").doc(currentUserId).set({ balance: 0, referrer: "none" });
                    document.getElementById('u-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                loadTeam();
                renderNodes();
            } else {
                let n = prompt("Enter Username:");
                let r = prompt("Referrer ID (Optional):") || "none";
                if(n) { 
                    localStorage.setItem('pg_user', n.toLowerCase()); 
                    db.collection("users").doc(n.toLowerCase()).set({ balance: 0, referrer: r.toLowerCase() });
                    location.reload(); 
                }
            }
        }

        function renderNodes() {
            const grid = document.getElementById('m-grid');
            for(let i=1; i<=3; i++) {
                let cost = 200 + (i*1000);
                grid.innerHTML += `
                <div class="sky-card p-5 border-l-4 border-yellow-500 flex justify-between items-center">
                    <div><p class="text-[9px] font-black text-yellow-500 uppercase">S-NODE 0${i}</p><p class="text-sm font-black">₨ ${cost.toLocaleString()}</p></div>
                    <button class="px-5 py-2 btn-gold text-[8px] uppercase">Activate</button>
                </div>`;
            }
        }

        async function submitRequest(type) {
            const amt = type === 'deposit' ? document.getElementById('dep-amt').value : document.getElementById('wit-amt').value;
            const info = type === 'deposit' ? document.getElementById('dep-tid').value : document.getElementById('wit-acc').value;
            if(!amt || !info) return alert("Fill all fields.");
            await db.collection("requests").add({ user: currentUserId, type, amount: parseFloat(amt), info, status: "pending", time: Date.now() });
            alert("Sent to Admin.");
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => { b.classList.add('opacity-40'); b.classList.remove('nav-active'); });
            document.getElementById('n-'+p).classList.remove('opacity-40');
            document.getElementById('n-'+p).classList.add('nav-active');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = checkUser;
    </script>
</body>
</html>
