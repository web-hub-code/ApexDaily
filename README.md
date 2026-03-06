<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | PKR Investment Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: white; font-family: 'Outfit', sans-serif; overflow-x: hidden; }
        
        .glass { background: rgba(255, 255, 255, 0.02); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.08); }
        .neon-card { background: linear-gradient(135deg, #0284c7, #7c3aed, #db2777); box-shadow: 0 20px 60px rgba(124, 58, 237, 0.3); }
        .btn-glow { background: linear-gradient(90deg, #0ea5e9, #d946ef); transition: 0.3s; box-shadow: 0 10px 20px rgba(14, 165, 233, 0.2); }
        
        #toast { transition: 0.5s; transform: translateY(-150%); z-index: 9999; }
        #toast.active { transform: translateY(0); }
        
        .tab-content { display: none; animation: fadeIn 0.4s ease; }
        .tab-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        
        .verified-tick { color: #3b82f6; font-size: 0.8rem; }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, addDoc, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg",
            authDomain: "investment-84f4e.firebaseapp.com",
            projectId: "investment-84f4e",
            storageBucket: "investment-84f4e.firebasestorage.app",
            messagingSenderId: "975293889308",
            appId: "1:975293889308:web:6d034a99cc966c75ff58d9"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // --- AUTH & USER SYNC ---
        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('authSec').classList.add('hidden');
                document.getElementById('appSec').classList.remove('hidden');
                syncData(user.uid);
                if(user.email === "admin@apexdaily.com") document.getElementById('adminTabBtn').classList.remove('hidden');
            } else {
                document.getElementById('authSec').classList.remove('hidden');
                document.getElementById('appSec').classList.add('hidden');
            }
        });

        function syncData(uid) {
            onSnapshot(doc(db, "users", uid), (d) => {
                if(d.exists()) {
                    const val = d.data().balance || 0;
                    document.getElementById('mainBal').innerText = "Rs. " + val.toLocaleString();
                    document.getElementById('userDisplayName').innerText = d.data().username || "Investor";
                }
            });
        }

        // --- COUNTDOWN TIMER (24 Hours) ---
        function startProfitTimer() {
            let h=23, m=59, s=59;
            setInterval(() => {
                s--; if(s<0){s=59; m--;} if(m<0){m=59; h--;}
                document.getElementById('timerText').innerText = `${h}:${m}:${s}`;
            }, 1000);
        }
        startProfitTimer();

        // --- UI SWITCHER ---
        window.showTab = (id) => {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            window.scrollTo(0,0);
        };

        window.handleAuth = async (type) => {
            const e = document.getElementById('email').value;
            const p = document.getElementById('pass').value;
            const u = document.getElementById('user').value;
            try {
                if(type === 'signup') {
                    const r = await createUserWithEmailAndPassword(auth, e, p);
                    await setDoc(doc(db, "users", r.user.uid), { username: u, balance: 0, email: e, joinDate: new Date().toLocaleDateString() });
                } else { await signInWithEmailAndPassword(auth, e, p); }
            } catch(err) { alert(err.message); }
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="pb-32">

    <div id="authSec" class="h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-sm glass p-10 rounded-[3rem] text-center border-t border-white/10">
            <h1 class="text-3xl font-black italic text-sky-500 mb-8">APEXDAILY</h1>
            <input id="user" type="text" placeholder="Username" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/10 text-sm">
            <input id="email" type="email" placeholder="Email" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/10 text-sm">
            <input id="pass" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/10 text-sm">
            <button onclick="handleAuth('login')" class="w-full btn-glow py-4 rounded-2xl font-bold mb-4">LOGIN</button>
            <button onclick="handleAuth('signup')" class="text-xs text-gray-400 underline">Create New Account</button>
        </div>
    </div>

    <div id="appSec" class="hidden">
        
        <div class="p-6 flex justify-between items-center sticky top-0 bg-[#020617]/90 backdrop-blur-md z-50">
            <div class="flex items-center gap-2">
                <div class="w-9 h-9 rounded-full bg-gradient-to-tr from-sky-500 to-fuchsia-600 flex items-center justify-center font-bold">A</div>
                <div>
                    <h4 class="text-sm font-black flex items-center">@<span id="userDisplayName">User</span> <i class="fa-solid fa-circle-check verified-tick ml-1"></i></h4>
                    <p class="text-[8px] text-green-500 font-bold uppercase tracking-widest">● Trusted Investor</p>
                </div>
            </div>
            <button onclick="logout()" class="text-red-500 text-xs font-bold bg-red-500/10 px-3 py-1.5 rounded-lg border border-red-500/20">EXIT</button>
        </div>

        <div id="home" class="tab-content active p-6">
            <div class="neon-card p-8 rounded-[2.5rem] relative overflow-hidden mb-10">
                <p class="text-[10px] font-bold opacity-80 uppercase tracking-widest">Available Balance</p>
                <h1 id="mainBal" class="text-5xl font-black my-3 tracking-tighter">Rs. 0</h1>
                <div class="flex gap-3 mt-8">
                    <button onclick="showTab('wallet')" class="flex-1 bg-white text-sky-600 py-3 rounded-xl font-black text-xs active:scale-95 transition">RECHARGE</button>
                    <button onclick="showTab('wallet')" class="flex-1 bg-black/20 py-3 rounded-xl font-black text-xs border border-white/20 active:scale-95 transition">WITHDRAW</button>
                </div>
            </div>

            <div class="glass p-6 rounded-3xl mb-8 border-l-4 border-sky-500 flex justify-between items-center">
                <div>
                    <p class="text-[10px] text-gray-400 font-bold uppercase">Next Profit Pulse</p>
                    <h3 id="timerText" class="text-2xl font-black text-sky-400 tracking-widest">23:59:59</h3>
                </div>
                <i class="fa-solid fa-hourglass-half text-sky-500/30 text-2xl animate-spin-slow"></i>
            </div>

            <h3 class="text-xs font-black text-gray-500 uppercase tracking-[4px] mb-6 ml-1">Premium Plans</h3>
            <div class="space-y-4 mb-10">
                <div class="glass p-6 rounded-[2rem] flex justify-between items-center border border-white/5">
                    <div>
                        <h4 class="text-xl font-black">Plan Rs. 200</h4>
                        <p class="text-[10px] text-green-400 font-bold">Daily Profit: Rs. 25 (15+10 Bonus)</p>
                    </div>
                    <button class="btn-glow px-5 py-2.5 rounded-xl text-[10px] font-bold">ACTIVATE</button>
                </div>
                <div class="glass p-6 rounded-[2rem] flex justify-between items-center border border-white/5">
                    <div>
                        <h4 class="text-xl font-black">Plan Rs. 500</h4>
                        <p class="text-[10px] text-green-400 font-bold">Daily Profit: Rs. 65</p>
                    </div>
                    <button class="btn-glow px-5 py-2.5 rounded-xl text-[10px] font-bold">ACTIVATE</button>
                </div>
            </div>
        </div>

        <div id="wallet" class="tab-content p-6 text-center">
            <h2 class="text-2xl font-black mb-10 italic">Financial <span class="text-sky-500">Node</span></h2>
            <div class="space-y-4 mb-10 text-left">
                <div class="glass p-5 rounded-2xl border-l-4 border-yellow-500 flex justify-between items-center">
                    <div><p class="text-[10px] text-gray-500 uppercase">JazzCash / SadaPay</p><p class="font-black text-lg">03705519562</p></div>
                    <i class="fa-solid fa-bolt text-yellow-500"></i>
                </div>
                <div class="glass p-5 rounded-2xl border-l-4 border-green-500 flex justify-between items-center">
                    <div><p class="text-[10px] text-gray-400 uppercase">EasyPaisa</p><p class="font-black text-lg">03379827882</p></div>
                    <i class="fa-solid fa-mobile-screen text-green-500"></i>
                </div>
            </div>
            <div class="glass p-8 rounded-[2.5rem]">
                <input type="number" placeholder="Enter Amount (PKR)" class="w-full p-4 bg-white/5 rounded-2xl mb-4 border border-white/10 outline-none focus:border-sky-500">
                <input type="text" placeholder="Transaction ID (TID)" class="w-full p-4 bg-white/5 rounded-2xl mb-8 border border-white/10 outline-none focus:border-sky-500">
                <button class="w-full btn-glow py-4 rounded-2xl font-black">UPLOAD DEPOSIT</button>
            </div>
        </div>

        <div id="privacy" class="tab-content p-6">
            <h2 class="text-2xl font-black mb-6 text-sky-500">Privacy & <span class="text-white">Trust</span></h2>
            <div class="glass p-6 rounded-3xl text-[11px] leading-relaxed text-gray-400 space-y-4">
                <p><b class="text-white">1. Data Security:</b> Humara system Firebase Encrypted hai, aapka data kisi teesre bande ko nahi diya jayega.</p>
                <p><b class="text-white">2. Investment Risk:</b> Trading mein utaar charhao hota hai, magar humara system fixed ROI model par kaam karta hai.</p>
                <p><b class="text-white">3. Withdrawal Policy:</b> Har withdrawal 2 se 12 ghante ke andar approve kiya jayega.</p>
                <p><b class="text-white">4. Verified Status:</b> Har deposit karne wala user "Verified Merchant" badge hasil kar leta hai.</p>
            </div>
            <div class="mt-8 glass p-6 rounded-3xl text-center">
                <i class="fa-solid fa-shield-halved text-4xl text-sky-500 mb-4 opacity-40"></i>
                <p class="text-xs font-bold">Official ApexDaily License v2.0</p>
            </div>
        </div>

        <div id="help" class="tab-content p-6">
            <h2 class="text-2xl font-black mb-8 italic">Support <span class="text-sky-500">Desk</span></h2>
            <div class="glass p-6 rounded-[2rem] mb-6">
                <textarea placeholder="Write your problem to Admin..." class="w-full h-32 bg-black/30 rounded-2xl p-4 text-xs outline-none border border-white/10 focus:border-sky-500"></textarea>
                <button class="w-full btn-glow mt-4 py-3 rounded-xl font-bold text-xs uppercase">Send Ticket</button>
            </div>
            <a href="https://chat.whatsapp.com/yourlink" target="_blank" class="w-full glass p-4 rounded-2xl flex items-center justify-center gap-3 border-green-500/30 text-green-400 font-bold text-sm">
                <i class="fa-brands fa-whatsapp text-xl"></i> JOIN OFFICIAL GROUP
            </a>
        </div>

        <div id="admin" class="tab-content p-6">
            <h2 class="text-2xl font-black text-yellow-500 mb-8 italic"><i class="fa-solid fa-crown mr-2"></i>GOD MODE</h2>
            <div class="space-y-6">
                <div class="glass p-6 rounded-3xl border-t-4 border-yellow-500">
                    <p class="text-[10px] font-bold text-gray-500 mb-4">PENDING APPROVALS</p>
                    <div class="text-[10px] opacity-40 italic">Waiting for new TIDs from users...</div>
                </div>
                <div class="glass p-6 rounded-3xl">
                    <p class="text-[10px] font-bold text-gray-500 mb-4 uppercase">Direct User Credit</p>
                    <input type="text" placeholder="User Email" class="w-full p-3 bg-black/40 rounded-xl mb-3 text-xs border border-white/5">
                    <input type="number" placeholder="Bonus Amount (Rs)" class="w-full p-3 bg-black/40 rounded-xl mb-4 text-xs border border-white/5">
                    <button class="w-full bg-yellow-600 py-3 rounded-xl font-bold text-black text-[10px]">ADD BALANCE</button>
                </div>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 glass p-6 flex justify-around items-center rounded-t-[3rem] z-50 border-t border-white/5">
            <button onclick="showTab('home')" class="flex flex-col items-center gap-1 focus:text-sky-500"><i class="fa-solid fa-house text-xl"></i><span class="text-[8px] font-bold">HOME</span></button>
            <button onclick="showTab('wallet')" class="flex flex-col items-center gap-1 focus:text-sky-500"><i class="fa-solid fa-wallet text-xl"></i><span class="text-[8px] font-bold">WALLET</span></button>
            <button onclick="showTab('privacy')" class="flex flex-col items-center gap-1 focus:text-sky-500"><i class="fa-solid fa-shield text-xl"></i><span class="text-[8px] font-bold">TRUST</span></button>
            <button onclick="showTab('help')" class="flex flex-col items-center gap-1 focus:text-sky-500"><i class="fa-solid fa-headset text-xl"></i><span class="text-[8px] font-bold">HELP</span></button>
            <button id="adminTabBtn" onclick="showTab('admin')" class="hidden flex flex-col items-center gap-1 text-yellow-500"><i class="fa-solid fa-crown text-xl"></i><span class="text-[8px] font-bold">MASTER</span></button>
        </nav>

    </div>

</body>
</html>
