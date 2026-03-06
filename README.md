<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Pro Investment</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, addDoc, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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
        const PKR_RATE = 285;

        // --- AUTH LOGIC ---
        window.handleAuth = async (type) => {
            let u = document.getElementById('username').value.trim();
            let p = document.getElementById('password').value;
            const email = u.includes('@') ? u : `${u}@apex.com`;
            try {
                if(type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, p);
                    await setDoc(doc(db, "users", res.user.uid), { username: u, balance: 0, role: "user" });
                } else { await signInWithEmailAndPassword(auth, email, p); }
            } catch(e) { alert("Error: " + e.message); }
        };

        // --- VIEW CONTROLLER ---
        onAuthStateChanged(auth, async (user) => {
            if(user) {
                document.getElementById('authSection').classList.add('hidden');
                if(user.email === "admin@apexdaily.com") {
                    document.getElementById('adminDash').classList.remove('hidden');
                    loadAdminRequests();
                } else {
                    document.getElementById('userDash').classList.remove('hidden');
                    onSnapshot(doc(db, "users", user.uid), (d) => {
                        if(d.exists()) {
                            document.getElementById('uBal').innerText = "$" + d.data().balance.toFixed(2);
                            document.getElementById('uPkr').innerText = (d.data().balance * PKR_RATE).toLocaleString() + " PKR";
                        }
                    });
                }
            } else {
                document.getElementById('authSection').classList.remove('hidden');
                document.getElementById('userDash').classList.add('hidden');
                document.getElementById('adminDash').classList.add('hidden');
            }
        });

        // --- DEPOSIT SYSTEM ---
        window.submitDeposit = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('depTid').value;
            const proof = document.getElementById('depProof').value; // Screenshot link
            const method = document.getElementById('depMethod').value;

            if(!amt || !tid) return alert("Please fill all details, sweetie!");

            await addDoc(collection(db, "deposits"), {
                uid: auth.currentUser.uid,
                email: auth.currentUser.email,
                amount: parseFloat(amt),
                tid: tid,
                proof: proof,
                method: method,
                status: "pending",
                time: new Date()
            });
            alert("Deposit Request Submitted! Admin will verify soon.");
        };

        // --- WITHDRAW SYSTEM ---
        window.submitWithdraw = async () => {
            const amt = document.getElementById('witAmt').value;
            const acc = document.getElementById('witAcc').value;
            const method = document.getElementById('witMethod').value;

            await addDoc(collection(db, "withdrawals"), {
                uid: auth.currentUser.uid,
                amount: parseFloat(amt),
                account: acc,
                method: method,
                status: "pending",
                time: new Date()
            });
            alert("Withdrawal Requested!");
        };

        // --- ADMIN FUNCTIONS ---
        async function loadAdminRequests() {
            onSnapshot(query(collection(db, "deposits"), where("status", "==", "pending")), (snaps) => {
                const list = document.getElementById('adminDepList');
                list.innerHTML = "";
                snaps.forEach(d => {
                    const data = d.data();
                    list.innerHTML += `
                    <div class="bg-gray-800 p-4 rounded-xl border-l-4 border-green-500">
                        <p><b>User:</b> ${data.email}</p>
                        <p><b>Amount:</b> $${data.amount} (${data.method})</p>
                        <p><b>TID:</b> ${data.tid}</p>
                        <a href="${data.proof}" target="_blank" class="text-blue-400 underline">View Proof</a>
                        <button onclick="approveDep('${d.id}', '${data.uid}', ${data.amount})" class="w-full mt-2 bg-green-600 py-1 rounded">Approve</button>
                    </div>`;
                });
            });
        }

        window.approveDep = async (id, uid, amt) => {
            const uRef = doc(db, "users", uid);
            const uSnap = await getDoc(uRef);
            await updateDoc(uRef, { balance: (uSnap.data().balance || 0) + amt });
            await updateDoc(doc(db, "deposits", id), { status: "approved" });
            alert("Balance Updated!");
        };

        window.copyNum = (id) => {
            const val = document.getElementById(id).innerText;
            navigator.clipboard.writeText(val);
            alert("Number Copied, Sweetie!");
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="bg-[#0f172a] text-slate-200 font-sans p-4 md:p-8">

    <div id="authSection" class="max-w-md mx-auto mt-20 bg-[#1e293b] p-8 rounded-3xl shadow-2xl border border-slate-700">
        <h1 class="text-4xl font-black text-center text-blue-500 mb-8 uppercase tracking-widest italic">ApexDaily</h1>
        <div class="space-y-4">
            <input id="username" type="text" placeholder="Username" class="w-full p-4 bg-[#0f172a] rounded-2xl outline-none focus:ring-2 ring-blue-500">
            <input id="password" type="password" placeholder="Password" class="w-full p-4 bg-[#0f172a] rounded-2xl outline-none focus:ring-2 ring-blue-500">
            <div class="flex gap-4 pt-4">
                <button onclick="handleAuth('login')" class="flex-1 bg-blue-600 py-4 rounded-2xl font-bold shadow-lg shadow-blue-500/20">Login</button>
                <button onclick="handleAuth('signup')" class="flex-1 bg-slate-700 py-4 rounded-2xl font-bold">Signup</button>
            </div>
        </div>
    </div>

    <div id="userDash" class="hidden max-w-5xl mx-auto space-y-6">
        <div class="flex justify-between items-center bg-[#1e293b] p-6 rounded-[2rem] border border-slate-800">
            <h2 class="text-xl font-bold">💎 Apex <span class="text-blue-500 italic">Pro</span></h2>
            <button onclick="logout()" class="text-red-400 font-bold bg-red-400/10 px-4 py-2 rounded-xl">Logout</button>
        </div>

        <div class="bg-gradient-to-br from-blue-600 via-blue-700 to-indigo-800 p-10 rounded-[2.5rem] shadow-2xl relative overflow-hidden">
            <div class="relative z-10">
                <p class="text-blue-100 uppercase tracking-widest text-sm font-bold">Current Balance</p>
                <h1 id="uBal" class="text-7xl font-black my-4">$0.00</h1>
                <div class="inline-block bg-white/20 backdrop-blur-md px-6 py-2 rounded-full font-bold">
                    ≈ <span id="uPkr">0 PKR</span>
                </div>
            </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="bg-[#1e293b] p-8 rounded-[2.5rem] border border-slate-800">
                <h3 class="text-2xl font-black mb-6 flex items-center gap-2 text-green-400"><i class="fas fa-wallet"></i> Deposit</h3>
                
                <div class="space-y-3 mb-8">
                    <div class="flex justify-between p-3 bg-black/30 rounded-xl border border-slate-700">
                        <span>JazzCash: <b id="num1">0300XXXXXXX</b></span>
                        <button onclick="copyNum('num1')" class="text-blue-400"><i class="fas fa-copy"></i></button>
                    </div>
                    <div class="flex justify-between p-3 bg-black/30 rounded-xl border border-slate-700">
                        <span>EasyPaisa: <b id="num2">0311XXXXXXX</b></span>
                        <button onclick="copyNum('num2')" class="text-blue-400"><i class="fas fa-copy"></i></button>
                    </div>
                </div>

                <div class="space-y-4">
                    <select id="depMethod" class="w-full p-4 bg-[#0f172a] rounded-xl outline-none">
                        <option>JazzCash</option>
                        <option>EasyPaisa</option>
                        <option>SadaPay</option>
                        <option>Binance (USDT)</option>
                    </select>
                    <input id="depAmt" type="number" placeholder="Amount in Dollars ($)" class="w-full p-4 bg-[#0f172a] rounded-xl outline-none">
                    <input id="depTid" type="text" placeholder="Transaction ID (TID)" class="w-full p-4 bg-[#0f172a] rounded-xl outline-none">
                    <input id="depProof" type="text" placeholder="Screenshot Link (Imgur/PostImage)" class="w-full p-4 bg-[#0f172a] rounded-xl outline-none">
                    <button onclick="submitDeposit()" class="w-full bg-green-600 py-4 rounded-xl font-bold hover:scale-[1.02] transition">Submit Deposit</button>
                </div>
            </div>

            <div class="space-y-6 text-center">
                <div class="bg-[#1e293b] p-8 rounded-[2.5rem] border border-slate-800 text-left">
                    <h3 class="text-2xl font-black mb-6 text-orange-400"><i class="fas fa-arrow-down"></i> Withdraw</h3>
                    <div class="space-y-4">
                        <select id="witMethod" class="w-full p-4 bg-[#0f172a] rounded-xl outline-none">
                            <option>JazzCash</option>
                            <option>EasyPaisa</option>
                            <option>Binance</option>
                        </select>
                        <input id="witAmt" type="number" placeholder="Amount ($)" class="w-full p-4 bg-[#0f172a] rounded-xl outline-none">
                        <input id="witAcc" type="text" placeholder="Account Number / Wallet" class="w-full p-4 bg-[#0f172a] rounded-xl outline-none">
                        <button onclick="submitWithdraw()" class="w-full bg-orange-600 py-4 rounded-xl font-bold">Request Cashout</button>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-4">
                    <div class="bg-blue-900/30 p-4 rounded-2xl border border-blue-500/30">
                        <p class="text-xs text-blue-400 uppercase">Pro Plan</p>
                        <h4 class="text-2xl font-bold">$15</h4>
                        <p class="text-[10px] opacity-60">High Daily Profit</p>
                    </div>
                    <div class="bg-purple-900/30 p-4 rounded-2xl border border-purple-500/30">
                        <p class="text-xs text-purple-400 uppercase">Starter</p>
                        <h4 class="text-2xl font-bold">$5</h4>
                        <p class="text-[10px] opacity-60">Stable Growth</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="adminDash" class="hidden max-w-6xl mx-auto bg-[#1e293b] p-8 rounded-[3rem] border-2 border-yellow-500/40">
        <div class="flex justify-between mb-10">
            <h1 class="text-3xl font-black text-yellow-500">APEX COMMAND CENTER</h1>
            <button onclick="logout()" class="text-red-500 font-bold uppercase tracking-widest">Exit Securely</button>
        </div>
        
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-10">
            <div class="space-y-6">
                <div class="bg-black/20 p-6 rounded-2xl border border-slate-700">
                    <h3 class="text-xl font-bold mb-4 text-yellow-500">Global Profit Pulse</h3>
                    <input id="pPercent" type="number" placeholder="Enter % (e.g. 1.8)" class="w-full p-4 bg-[#0f172a] rounded-xl mb-4 border border-slate-800">
                    <button onclick="sendProfit()" class="w-full bg-yellow-600 py-4 rounded-xl font-black text-black">PULSE PROFIT TO ALL USERS</button>
                </div>
            </div>

            <div class="space-y-4">
                <h3 class="text-xl font-bold text-green-400 uppercase">Pending Deposits</h3>
                <div id="adminDepList" class="space-y-4 max-h-[500px] overflow-y-auto pr-2">
                    </div>
            </div>
        </div>
    </div>

</body>
</html>
