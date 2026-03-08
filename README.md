<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | ApexDaily Mining</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; --red: #ef4444; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; overflow-x: hidden; }
        
        /* Glassmorphism Design */
        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; padding: 20px; width: 92%; max-width: 450px; margin: 15px auto; }
        .btn-main { width: 100%; padding: 15px; background: var(--gold); border: none; border-radius: 14px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        input, select { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 14px; color: white; margin-bottom: 12px; outline: none; }
        
        #welcomeScreen, #authSection, #dashboardPage, #adminPage, #adminTrigger, #loadingOverlay { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }
        
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .spinner { width: 40px; height: 40px; border: 3px solid rgba(251, 191, 36, 0.1); border-top: 3px solid var(--gold); border-radius: 50%; animation: spin 1s linear infinite; }
    </style>
</head>
<body>

    <div id="welcomeScreen" class="flex-active" style="position:fixed; inset:0; background:#020617; z-index:10000; flex-direction:column; align-items:center; justify-content:center;">
        <h1 style="font-size:3.5rem; color:var(--gold); font-weight:600;">PakGold</h1>
        <p style="color:#94a3b8; letter-spacing:2px; font-size:0.7rem;">POWERED BY PRIME SOLUTIONS</p>
        <button onclick="enterApp()" class="btn-main" style="width:200px; margin-top:40px;">Get Started 🚀</button>
    </div>

    <div id="loadingOverlay" style="position:fixed; inset:0; background:rgba(2,6,23,0.9); z-index:20000; display:none; flex-direction:column; align-items:center; justify-content:center;">
        <div class="spinner"></div>
    </div>

    <div id="authSection" class="active">
        <h1 id="secretTap" style="text-align:center; margin:40px 0; color:var(--gold);">PakGold</h1>
        <div class="glass-card">
            <h2 id="authTitle" style="font-size:1.1rem; margin-bottom:20px;">Sign In</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <button id="authBtn" class="btn-main">Continue</button>
            <p id="toggleAuth" style="text-align:center; font-size:0.8rem; margin-top:20px; cursor:pointer; color:#94a3b8;">New user? <span style="color:var(--gold)">Create Account</span></p>
            
            <div id="adminTrigger" style="margin-top:20px; display:none;">
                <input type="password" id="adminKey" placeholder="System Access Key" style="border-color:var(--red);">
                <button onclick="checkAdmin()" class="btn-main" style="background:var(--red); color:white;">Access God Mode</button>
            </div>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="border-color:var(--gold); margin-top:10px;">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
                <span id="timerBadge" style="font-size:0.7rem; color:var(--red); font-weight:bold;">00:00:00</span>
                <button onclick="logout()" style="background:none; border:none; color:var(--red); font-size:0.6rem; font-weight:bold;">LOGOUT</button>
            </div>
            <div style="text-align:center; margin-bottom:20px;">
                <p style="font-size:0.6rem; color:#94a3b8; text-transform:uppercase;">Current Balance</p>
                <h1 style="color:var(--gold); font-size:2.8rem;">PKR <span id="bal">0.00</span></h1>
                <p style="font-size:0.65rem; color:var(--green); margin-top:5px;">Rig: <b id="machine">None</b></p>
            </div>
            <div style="width:100%; height:6px; background:rgba(255,255,255,0.05); border-radius:10px; overflow:hidden;">
                <div id="miningProgress" style="width:0%; height:100%; background:var(--gold); box-shadow:0 0 10px var(--gold); transition:1s;"></div>
            </div>
        </div>

        <div class="glass-card">
            <h4 style="font-size:0.8rem; margin-bottom:15px; color:var(--gold);">Wallets & Payments</h4>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px;">
                <button onclick="togglePop('depPop')" style="background:var(--glass); color:white; border:1px solid var(--gold); padding:12px; border-radius:12px; font-size:0.7rem; font-weight:bold;">DEPOSIT</button>
                <button onclick="togglePop('witPop')" style="background:var(--glass); color:white; border:1px solid var(--red); padding:12px; border-radius:12px; font-size:0.7rem; font-weight:bold;">WITHDRAW</button>
            </div>
        </div>

        <div id="depPop" class="glass-card" style="display:none; border-color:var(--gold);">
            <div style="font-size:0.7rem; margin-bottom:15px; background:rgba(255,255,255,0.03); padding:10px; border-radius:10px;">
                <p><b>EasyPaisa:</b> 03379827882</p>
                <p><b>JazzCash:</b> 03705519562</p>
                <p><b>SadaPay:</b> 03705519562</p>
            </div>
            <input type="number" id="depAmt" placeholder="Amount (PKR)">
            <input type="text" id="depTID" placeholder="TID / Transaction ID">
            <button onclick="submitDep()" class="btn-main">Submit Proof</button>
        </div>

        <div id="witPop" class="glass-card" style="display:none; border-color:var(--red);">
            <input type="number" id="witAmt" placeholder="Minimum PKR 100">
            <input type="text" id="witNum" placeholder="Account Number (03xx...)">
            <select id="witMethod">
                <option>EasyPaisa</option>
                <option>JazzCash</option>
                <option>SadaPay</option>
            </select>
            <button onclick="submitWit()" class="btn-main" style="background:var(--red); color:white;">Request Payout</button>
        </div>

        <div class="glass-card" style="border-color:#25d366;">
            <a href="YOUR_WHATSAPP_GROUP_LINK" style="text-decoration:none;">
                <button style="width:100%; padding:12px; background:#25d366; color:white; border:none; border-radius:12px; font-weight:bold; cursor:pointer;">Join WhatsApp Community</button>
            </a>
            <p style="text-align:center; font-size:0.7rem; margin-top:12px; color:#94a3b8;">webhub262@gmail.com</p>
        </div>
    </div>

    <div id="adminPage">
        <h2 style="text-align:center; color:var(--red); margin:30px 0;">SYSTEM CONTROL 👑</h2>
        <div class="glass-card">
            <h4 style="font-size:0.8rem; margin-bottom:10px;">Pending Actions</h4>
            <div id="adminActionList"></div>
        </div>
        <button onclick="location.reload()" class="btn-main" style="background:#334155; color:white; margin-top:20px;">Exit System</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, onSnapshot, collection, query, where, addDoc, serverTimestamp, increment } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        let currentUser = localStorage.getItem('pakgold_user');
        let isLogin = true;

        // --- AUTH & INITIALIZATION ---
        window.enterApp = () => {
            document.getElementById('welcomeScreen').style.opacity = '0';
            setTimeout(() => {
                document.getElementById('welcomeScreen').style.display = 'none';
                if(!currentUser) document.getElementById('authSection').classList.add('active');
                else startDashboard();
            }, 500);
        };

        document.getElementById('authBtn').onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;
            if(!user || !pass) return alert("Fill all fields");
            
            document.getElementById('loadingOverlay').style.display = 'flex';
            const uRef = doc(db, "users", user);
            const snap = await getDoc(uRef);

            if(isLogin) {
                if(snap.exists() && snap.data().pass === pass) login(user);
                else { alert("Access Denied"); document.getElementById('loadingOverlay').style.display = 'none'; }
            } else {
                if(snap.exists()) { alert("Username taken"); document.getElementById('loadingOverlay').style.display = 'none'; }
                else {
                    await setDoc(uRef, { user, pass, balance: 0, machine: "None", profit: 0, lastClaim: 0, machineExpiry: 0 });
                    login(user);
                }
            }
        };

        function login(u) { localStorage.setItem('pakgold_user', u); location.reload(); }
        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }

        // --- DASHBOARD LOGICS (Auto-Profit & Daily Bonus) ---
        function startDashboard() {
            document.getElementById('authSection').style.display = 'none';
            document.getElementById('dashboardPage').classList.add('active');
            
            const uRef = doc(db, "users", currentUser);
            onSnapshot(uRef, async (s) => {
                const d = s.data();
                document.getElementById('bal').innerText = d.balance.toFixed(2);
                document.getElementById('machine').innerText = d.machine;
                
                // Auto-Mining Check
                if(d.machine !== "None" && Date.now() > d.machineExpiry) {
                    document.getElementById('timerBadge').innerText = "EXPIRED";
                } else if(d.machine !== "None") {
                    updateTimer(d.machineExpiry);
                    if(Date.now() - d.lastClaim > 86400000) {
                        await updateDoc(uRef, { balance: increment(d.profit), lastClaim: Date.now() });
                        alert("Daily profit added! 💰");
                    }
                }

                // Daily Bonus Check
                const today = new Date().toDateString();
                if(d.lastBonusDate !== today) {
                    await updateDoc(uRef, { balance: increment(5), lastBonusDate: today });
                    alert("PKR 5 Daily Bonus Added! 🎁");
                }
            });
        }

        function updateTimer(expiry) {
            const dist = expiry - Date.now();
            const d = Math.floor(dist / 86400000), h = Math.floor((dist % 86400000) / 3600000), m = Math.floor((dist % 3600000) / 60000);
            document.getElementById('timerBadge').innerText = `${d}d ${h}h ${m}m`;
            document.getElementById('miningProgress').style.width = ((Date.now() % 86400000) / 864000) + "%";
        }

        // --- TRANSACTIONS ---
        window.togglePop = (id) => { const e = document.getElementById(id); e.style.display = e.style.display==='none'?'block':'none'; };
        
        window.submitDep = async () => {
            const a = Number(document.getElementById('depAmt').value), t = document.getElementById('depTID').value;
            if(!a || !t) return alert("Fill TID & Amount");
            await addDoc(collection(db, "deposits"), { user: currentUser, amount: a, tid: t, status: "pending", time: serverTimestamp() });
            alert("Deposit proof submitted! 🚀");
            togglePop('depPop');
        };

        window.submitWit = async () => {
            const a = Number(document.getElementById('witAmt').value), n = document.getElementById('witNum').value, m = document.getElementById('witMethod').value;
            const s = await getDoc(doc(db, "users", currentUser));
            if(a < 100 || a > s.data().balance) return alert("Check balance/minimum limit");
            await updateDoc(doc(db, "users", currentUser), { balance: increment(-a) });
            await addDoc(collection(db, "withdrawals"), { user: currentUser, amount: a, num: n, method: m, status: "pending", time: serverTimestamp() });
            alert("Withdrawal requested! 💸");
            togglePop('witPop');
        };

        // --- ADMIN GOD MODE ---
        let taps = 0;
        document.getElementById('secretTap').onclick = () => { taps++; if(taps===4) document.getElementById('adminTrigger').style.display='block'; };
        window.checkAdmin = () => {
            if(document.getElementById('adminKey').value === "admin123") {
                document.getElementById('authSection').style.display='none';
                document.getElementById('adminPage').classList.add('active');
                onSnapshot(query(collection(db, "deposits"), where("status", "==", "pending")), (s) => {
                    const l = document.getElementById('adminActionList'); l.innerHTML = "";
                    s.forEach(d => {
                        l.innerHTML += `<div class="glass-card" style="font-size:0.6rem;">${d.data().user} | TID: ${d.data().tid} | PKR ${d.data().amount}<br><button onclick="appDep('${d.id}','${d.data().user}',${d.data().amount})" style="color:var(--green); border:none; background:none; cursor:pointer;">APPROVE ✅</button></div>`;
                    });
                });
            }
        };
        window.appDep = async (id, u, a) => { await updateDoc(doc(db, "users", u), { balance: increment(a) }); await updateDoc(doc(db, "deposits", id), { status: "success" }); };
    </script>
</body>
</html>
