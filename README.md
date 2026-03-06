<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Neon Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        
        body { background: #020617; color: #f1f5f9; font-family: 'Outfit', sans-serif; overflow-x: hidden; }

        /* Animated Neon Background */
        .neon-bg {
            background: linear-gradient(135deg, #3b82f6 0%, #a855f7 50%, #ec4899 100%);
            box-shadow: 0 10px 50px rgba(168, 85, 247, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.2);
            animation: glowMove 8s infinite alternate;
        }
        @keyframes glowMove { from { filter: hue-rotate(0deg); } to { filter: hue-rotate(45deg); } }

        /* Modern Glass Styles */
        .glass-blue { background: rgba(59, 130, 246, 0.1); border: 1px solid rgba(59, 130, 246, 0.2); backdrop-filter: blur(12px); }
        .glass-purple { background: rgba(168, 85, 247, 0.1); border: 1px solid rgba(168, 85, 247, 0.2); backdrop-filter: blur(12px); }
        .glass-green { background: rgba(34, 197, 94, 0.1); border: 1px solid rgba(34, 197, 94, 0.2); backdrop-filter: blur(12px); }

        .btn-gradient { background: linear-gradient(90deg, #3b82f6, #ec4899); transition: 0.3s; }
        .btn-gradient:active { transform: scale(0.95); }
        
        nav { background: rgba(2, 6, 23, 0.8); backdrop-filter: blur(20px); border-top: 1px solid rgba(255, 255, 255, 0.05); }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, collection, addDoc, query, where, updateDoc, getDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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
        const PKR_RATE = 280;

        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('authSection').classList.add('hidden');
                document.getElementById('appBody').classList.remove('hidden');
                
                if(user.email === "admin@apexdaily.com") {
                    document.getElementById('adminTab').classList.remove('hidden');
                    loadAdminRequests();
                } else {
                    syncUser(user.uid);
                }
            }
        });

        function syncUser(uid) {
            onSnapshot(doc(db, "users", uid), (d) => {
                if(d.exists()) {
                    const bal = d.data().balance || 0;
                    document.getElementById('uBal').innerText = "$" + bal.toFixed(2);
                    document.getElementById('uPkr').innerText = "≈ Rs. " + (bal * PKR_RATE).toLocaleString();
                }
            });
        }

        window.showTab = (id) => {
            ['homeTab','walletTab','adminTab'].forEach(t => document.getElementById(t).classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        };

        // ... (Other Logic: handleAuth, approveDeposit, etc.)
        window.logout = () => signOut(auth);
    </script>
</head>
<body class="pb-24">

    <div id="authSection" class="h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-sm glass-blue p-10 rounded-[3rem] text-center">
            <h1 class="text-4xl font-black text-blue-500 mb-8 italic">APEXDAILY</h1>
            <input id="uInp" type="text" placeholder="Username" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/10">
            <input id="pInp" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/10">
            <button onclick="handleAuth('login')" class="w-full btn-gradient py-4 rounded-2xl font-bold shadow-xl">Enter App</button>
        </div>
    </div>

    <div id="appBody" class="hidden">
        <div id="homeTab" class="p-6">
            <div class="flex justify-between items-center mb-8">
                <h2 class="text-2xl font-black italic text-blue-500">APEX.PRO</h2>
                <button onclick="logout()" class="text-[10px] text-red-400 font-bold uppercase">Logout</button>
            </div>

            <div class="neon-bg p-8 rounded-[2.5rem] mb-10 text-white relative overflow-hidden">
                <p class="text-[10px] uppercase font-bold tracking-widest opacity-80">Net Balance</p>
                <h1 id="uBal" class="text-6xl font-black my-2 tracking-tighter">$0.00</h1>
                <p id="uPkr" class="text-sm font-medium opacity-70">Rs. 0</p>
                
                <div class="flex gap-4 mt-8">
                    <button onclick="showTab('walletTab')" class="flex-1 bg-white text-blue-600 py-3 rounded-xl font-bold text-xs">DEPOSIT</button>
                    <button onclick="showTab('walletTab')" class="flex-1 bg-black/20 py-3 rounded-xl font-bold text-xs border border-white/20">WITHDRAW</button>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="glass-green p-5 rounded-3xl">
                    <p class="text-[10px] text-gray-400 font-bold mb-1">PROFIT RATE</p>
                    <h4 class="text-xl font-black text-green-400">+2.85%</h4>
                </div>
                <div class="glass-purple p-5 rounded-3xl">
                    <p class="text-[10px] text-gray-400 font-bold mb-1">PROMO STATUS</p>
                    <h4 class="text-xl font-black text-purple-400">ACTIVE</h4>
                </div>
            </div>

            <div class="glass-blue p-6 rounded-3xl flex gap-2">
                <input id="promo" type="text" placeholder="Promo Code" class="bg-black/40 flex-1 rounded-xl p-3 text-xs outline-none border border-white/5">
                <button class="bg-blue-600 px-6 rounded-xl text-[10px] font-bold">APPLY</button>
            </div>
        </div>

        <div id="walletTab" class="hidden p-6">
            <h2 class="text-2xl font-black mb-6 text-green-400">Financial Hub</h2>
            <div class="space-y-4 mb-8">
                <div class="glass-purple p-5 rounded-3xl border-l-4 border-purple-500">
                    <p class="text-[10px] text-gray-400">JazzCash / SadaPay</p>
                    <p class="font-black text-lg">03705519562</p>
                </div>
                <div class="glass-green p-5 rounded-3xl border-l-4 border-green-500">
                    <p class="text-[10px] text-gray-400">EasyPaisa</p>
                    <p class="font-black text-lg">03379827882</p>
                </div>
            </div>
            
            <input type="number" placeholder="Enter Amount ($)" class="w-full p-4 bg-white/5 rounded-2xl mb-4 border border-white/10 outline-none">
            <button class="w-full btn-gradient py-4 rounded-2xl font-bold">Submit Deposit</button>
        </div>

        <div id="adminTab" class="hidden p-6">
            <h2 class="text-2xl font-black mb-8 text-yellow-500">MASTER CONTROL</h2>
            <div class="glass-purple p-6 rounded-3xl">
                <p class="text-xs font-bold mb-4">Pending Requests</p>
                <div id="adminReqs" class="text-xs opacity-60 italic">No new requests...</div>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 p-5 flex justify-around items-center rounded-t-[2.5rem] z-50">
            <button onclick="showTab('homeTab')" class="text-blue-500 flex flex-col items-center gap-1">
                <i class="fa-solid fa-house-chimney text-xl"></i>
                <span class="text-[8px] font-bold uppercase">Home</span>
            </button>
            <button onclick="showTab('walletTab')" class="text-gray-500 flex flex-col items-center gap-1">
                <i class="fa-solid fa-wallet text-xl"></i>
                <span class="text-[8px] font-bold uppercase">Wallet</span>
            </button>
            <button onclick="showTab('adminTab')" id="adminBtn" class="hidden text-yellow-500 flex flex-col items-center gap-1">
                <i class="fa-solid fa-crown text-xl"></i>
                <span class="text-[8px] font-bold uppercase">Master</span>
            </button>
        </nav>
    </div>

</body>
</html>
