<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Pro Trader</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        .glass { background: rgba(255, 255, 255, 0.05); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.1); }
        .nav-active { color: #3b82f6; transform: translateY(-5px); transition: 0.3s; }
        body { background-color: #050810; font-family: 'Inter', sans-serif; overflow-x: hidden; }
        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 10px; }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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

        // UI View Switcher
        window.showPage = (pageId) => {
            const pages = ['homePage', 'plansPage', 'historyPage', 'morePage'];
            pages.forEach(p => document.getElementById(p).classList.add('hidden'));
            document.getElementById(pageId).classList.remove('hidden');
        };

        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('authSection').classList.add('hidden');
                document.getElementById('appBody').classList.remove('hidden');
                onSnapshot(doc(db, "users", user.uid), (d) => {
                    if(d.exists()) {
                        document.querySelectorAll('.uNameDisp').forEach(el => el.innerText = d.data().username);
                        document.getElementById('uBalDisp').innerText = "$" + d.data().balance.toFixed(2);
                    }
                });
            } else {
                document.getElementById('authSection').classList.remove('hidden');
                document.getElementById('appBody').classList.add('hidden');
            }
        });

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="text-slate-200">

    <div id="authSection" class="min-h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-md glass p-8 rounded-[2rem] text-center">
            <h1 class="text-4xl font-black text-blue-500 mb-2 italic">APEXDAILY</h1>
            <p class="text-gray-500 mb-8">Login to your trading account</p>
            <input id="username" type="text" placeholder="Username" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/10 focus:border-blue-500">
            <input id="password" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-6 outline-none border border-white/10 focus:border-blue-500">
            <button onclick="handleAuth('login')" class="w-full bg-blue-600 py-4 rounded-2xl font-bold shadow-xl shadow-blue-500/20">Sign In</button>
            <p class="mt-4 text-sm text-gray-400">New here? <span onclick="handleAuth('signup')" class="text-blue-500 cursor-pointer">Create Account</span></p>
        </div>
    </div>

    <div id="appBody" class="hidden pb-24">
        
        <header class="p-6 flex justify-between items-center bg-[#050810]/80 sticky top-0 z-50 backdrop-blur-md">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center font-bold">A</div>
                <div>
                    <p class="text-[10px] text-gray-500 uppercase">Welcome back</p>
                    <h3 class="uNameDisp font-bold">User</h3>
                </div>
            </div>
            <button onclick="logout()" class="text-gray-400"><i class="fa-solid fa-right-from-bracket"></i></button>
        </header>

        <div id="homePage" class="p-6 animate-fade-in">
            <div class="bg-gradient-to-br from-blue-600 to-indigo-800 p-8 rounded-[2.5rem] shadow-2xl mb-8 relative overflow-hidden">
                <p class="text-blue-100 text-sm">Total Assets</p>
                <h1 id="uBalDisp" class="text-5xl font-black my-2">$0.00</h1>
                <div class="flex gap-4 mt-6">
                    <button class="flex-1 bg-white/20 p-3 rounded-xl font-bold backdrop-blur-md"><i class="fa-solid fa-arrow-up mr-2"></i> Deposit</button>
                    <button class="flex-1 bg-black/20 p-3 rounded-xl font-bold backdrop-blur-md"><i class="fa-solid fa-arrow-down mr-2"></i> Withdraw</button>
                </div>
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-white/10 rounded-full blur-3xl"></div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="glass p-4 rounded-2xl">
                    <p class="text-xs text-gray-500">Daily Profit</p>
                    <h4 class="text-green-500 font-bold">+2.5%</h4>
                </div>
                <div class="glass p-4 rounded-2xl">
                    <p class="text-xs text-gray-500">Active Plans</p>
                    <h4 class="text-blue-400 font-bold">0 Active</h4>
                </div>
            </div>

            <div class="bg-blue-500/10 border border-blue-500/20 p-4 rounded-2xl mb-8 flex items-center gap-3">
                <i class="fa-solid fa-bullhorn text-blue-500"></i>
                <marquee class="text-sm text-blue-100">Deposit $20 and get a $2 bonus instantly! Limited time offer, sweetie.</marquee>
            </div>
        </div>

        <div id="plansPage" class="hidden p-6">
            <h2 class="text-2xl font-bold mb-6">Investment Plans</h2>
            <div class="grid grid-cols-2 gap-4" id="plansGrid">
                <script>
                    const pGrid = document.currentScript.parentElement;
                    const starters = [5, 10, 20, 50, 80];
                    starters.forEach((amt, i) => {
                        pGrid.innerHTML += `
                        <div class="glass p-5 rounded-3xl border-l-4 border-green-500">
                            <i class="fa-solid fa-seedling text-green-500 mb-3 text-xl"></i>
                            <p class="text-xs text-gray-500 uppercase">Starter ${i+1}</p>
                            <h4 class="text-2xl font-bold">$${amt}</h4>
                            <button class="mt-4 w-full bg-green-600/20 text-green-500 py-2 rounded-xl text-xs font-bold">Activate</button>
                        </div>`;
                    });
                    // 15 Premium Plans
                    for(let i=1; i<=15; i++) {
                        let val = i * 100;
                        pGrid.innerHTML += `
                        <div class="glass p-5 rounded-3xl border-l-4 border-blue-500">
                            <i class="fa-solid fa-gem text-blue-500 mb-3 text-xl"></i>
                            <p class="text-xs text-gray-500 uppercase">Premium ${i}</p>
                            <h4 class="text-2xl font-bold">$${val}</h4>
                            <button class="mt-4 w-full bg-blue-600/20 text-blue-500 py-2 rounded-xl text-xs font-bold">Activate</button>
                        </div>`;
                    }
                </script>
            </div>
        </div>

        <div id="historyPage" class="hidden p-6">
            <h2 class="text-2xl font-bold mb-6">Trade History</h2>
            <div class="space-y-4">
                <div class="glass p-4 rounded-2xl flex justify-between items-center opacity-50">
                    <div>
                        <p class="font-bold text-sm">No transactions yet</p>
                        <p class="text-[10px] text-gray-500">Start investing today, sweetie!</p>
                    </div>
                    <i class="fa-solid fa-clock-rotate-left"></i>
                </div>
            </div>
        </div>

        <div id="morePage" class="hidden p-6">
            <h2 class="text-2xl font-bold mb-6">Menu & Settings</h2>
            <div class="space-y-3">
                <div class="glass p-4 rounded-2xl flex items-center gap-4">
                    <i class="fa-solid fa-ticket text-yellow-500"></i>
                    <div class="flex-1">
                        <p class="font-bold">Promo Code</p>
                        <input type="text" placeholder="Enter code" class="bg-black/20 w-full mt-2 p-2 rounded-lg outline-none text-xs">
                    </div>
                </div>
                <div class="glass p-4 rounded-2xl flex items-center gap-4">
                    <i class="fa-solid fa-building-shield text-blue-400"></i>
                    <p class="font-bold">About Company</p>
                </div>
                <div class="glass p-4 rounded-2xl flex items-center gap-4">
                    <i class="fa-solid fa-user-lock text-purple-400"></i>
                    <p class="font-bold">Privacy Policy</p>
                </div>
                <div class="glass p-4 rounded-2xl flex items-center gap-4 text-red-400" onclick="logout()">
                    <i class="fa-solid fa-power-off"></i>
                    <p class="font-bold">Logout Account</p>
                </div>
            </div>
            <p class="text-center text-[10px] text-gray-600 mt-10">ApexDaily Version 2.0.1 (Stable)</p>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 bg-[#050810] border-t border-white/5 p-4 flex justify-around items-center z-50">
            <button onclick="showPage('homePage')" class="flex flex-col items-center gap-1 text-gray-500 focus:text-blue-500">
                <i class="fa-solid fa-house text-xl"></i>
                <span class="text-[10px]">Home</span>
            </button>
            <button onclick="showPage('plansPage')" class="flex flex-col items-center gap-1 text-gray-500 focus:text-blue-500">
                <i class="fa-solid fa-layer-group text-xl"></i>
                <span class="text-[10px]">Plans</span>
            </button>
            <button onclick="showPage('historyPage')" class="flex flex-col items-center gap-1 text-gray-500 focus:text-blue-500">
                <i class="fa-solid fa-chart-line text-xl"></i>
                <span class="text-[10px]">Trades</span>
            </button>
            <button onclick="showPage('morePage')" class="flex flex-col items-center gap-1 text-gray-500 focus:text-blue-500">
                <i class="fa-solid fa-grip-vertical text-xl"></i>
                <span class="text-[10px]">More</span>
            </button>
        </nav>

    </div>

</body>
</html>
