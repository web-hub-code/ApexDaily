<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Digital Mining 🇵🇰</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        /* --- PRO UI DESIGN --- */
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #020617; color: white; min-height: 100vh; display: flex; flex-direction: column; align-items: center; padding-bottom: 80px; }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(15px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 20px; padding: 15px; width: 94%; max-width: 480px; margin: 10px auto; }
        .brand-name { font-size: 1.8rem; font-weight: 600; color: #fbbf24; text-align: center; margin: 20px 0; }
        
        /* Machine Grid */
        .machine-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 15px; }
        .m-card { background: rgba(255,255,255,0.05); border-radius: 15px; padding: 12px; border: 1px solid rgba(255,255,255,0.1); text-align: center; transition: 0.3s; position: relative; overflow: hidden; }
        .m-card:hover { transform: translateY(-5px); border-color: #fbbf24; }
        .special-tag { position: absolute; top: 5px; right: -15px; background: #ef4444; color: white; font-size: 0.5rem; padding: 2px 15px; transform: rotate(45deg); font-weight: bold; }
        
        .btn-main { width: 100%; padding: 12px; background: linear-gradient(90deg, #fbbf24, #d97706); border: none; border-radius: 10px; color: black; font-weight: bold; cursor: pointer; margin-top: 8px; font-size: 0.8rem; }
        input, select { width: 100%; padding: 12px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 10px; color: white; margin-bottom: 10px; outline: none; }
        
        #loginPage, #dashboardPage, #adminPage { display: none; }
        .active { display: block !important; }
        .history-table { width: 100%; font-size: 0.65rem; border-collapse: collapse; margin-top: 10px; text-align: left; }
        .history-table td { padding: 8px; border-bottom: 1px solid rgba(255,255,255,0.05); }

        /* Floating WhatsApp */
        .wa-btn { position: fixed; bottom: 20px; right: 20px; background: #25d366; color: white; width: 50px; height: 50px; border-radius: 50%; display: flex; align-items: center; justify-content: center; text-decoration: none; font-size: 24px; z-index: 1000; box-shadow: 0 4px 10px rgba(0,0,0,0.3); }
    </style>
</head>
<body>

    <a href="https://wa.me/923705519562" class="wa-btn" target="_blank">💬</a>

    <div id="loginPage" class="active" style="text-align: center; margin-top: 20vh;">
        <div class="brand-name">PakGold</div>
        <div class="glass-card">
            <p style="color: #94a3b8; font-size: 0.8rem; margin-bottom: 20px;">Safe Mining & Investment Platform</p>
            <button id="loginBtn" class="btn-main">Start Your Journey</button>
            <p onclick="adminLogin()" style="font-size: 0.6rem; color: #4facfe; margin-top: 15px; cursor: pointer;">Admin Dashboard Access</p>
        </div>
    </div>

    <div id="dashboardPage" class="glass-card">
        <div style="display: flex; justify-content: space-between; align-items: center;">
            <span class="brand-name" style="font-size: 1.2rem; margin: 0;">PakGold</span>
            <button id="logoutBtn" style="background:none; border:1px solid #ef4444; color:#ef4444; padding:4px 10px; border-radius:8px; font-size: 0.6rem;">Logout</button>
        </div>

        <div style="background: rgba(251, 191, 36, 0.1); padding: 15px; border-radius: 15px; margin: 15px 0; border: 1px solid rgba(251, 191, 36, 0.2);">
            <p style="font-size: 0.7rem; color: #94a3b8;">Current Balance</p>
            <h2 style="color: #fbbf24;">PKR <span id="userBal">0.00</span></h2>
            <p style="font-size: 0.6rem; color: #10b981; margin-top: 5px;">Active Machine: <b id="activeM">None</b></p>
        </div>

        <button id="taskBtn" class="btn-main" style="background: #10b981; color: white;">Claim Daily Mining Profit ⛏️</button>

        <h4 style="text-align: left; margin-top: 25px; color: #fbbf24; font-size: 0.9rem;">Mining Machine Store 🏪</h4>
        <div class="machine-grid" id="mGrid"></div>

        <h4 style="text-align: left; margin-top: 25px; color: #4facfe; font-size: 0.9rem;">Financial Center 🏦</h4>
        <div style="margin-top: 10px;">
            <select id="txnType"><option value="Deposit">Deposit (Min 200)</option><option value="Withdraw">Withdraw (Min 100)</option></select>
            <select id="mthd"><option value="JazzCash">JazzCash</option><option value="EasyPaisa">EasyPaisa</option></select>
            <input type="number" id="txnAmt" placeholder="Enter Amount">
            <button id="txnBtn" class="btn-main" style="background: #4facfe; color: white;">Submit Transaction</button>
        </div>

        <table class="history-table"><thead><tr style="color: #94a3b8;"><th>Type</th><th>PKR</th><th>Status</th></tr></thead><tbody id="logs"></tbody></table>
    </div>

    <div id="adminPage" class="glass-card">
        <h2 style="color: #fbbf24;">Admin Control 👑</h2>
        <div id="adminList"></div>
        <button onclick="location.reload()" class="btn-main" style="background:#ef4444; color:white;">Exit Admin</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, onSnapshot, collection, addDoc, query, where, increment, serverTimestamp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // --- 20 MACHINES DATA ---
        const machines = [
            { n: "Nano", p: 200, d: 14, t: "regular" }, { n: "Micro", p: 350, d: 25, t: "regular" },
            { n: "Bronze", p: 500, d: 35, t: "regular" }, { n: "Iron", p: 800, d: 55, t: "regular" },
            { n: "Silver", p: 1000, d: 70, t: "regular" }, { n: "Gold", p: 1500, d: 110, t: "regular" },
            { n: "Ruby", p: 2000, d: 150, t: "regular" }, { n: "Emerald", p: 3000, d: 230, t: "regular" },
            { n: "Sapphire", p: 4000, d: 310, t: "regular" }, { n: "Diamond", p: 5000, d: 400, t: "regular" },
            { n: "Onyx", p: 7000, d: 580, t: "regular" }, { n: "Crystal", p: 10000, d: 850, t: "regular" },
            { n: "Titan", p: 15000, d: 1300, t: "regular" }, { n: "Legend", p: 20000, d: 1800, t: "regular" },
            { n: "Ultra", p: 30000, d: 2800, t: "regular" },
            { n: "Dragon 🔥", p: 3000, d: 350, t: "special" }, { n: "Phoenix 🔥", p: 6000, d: 750, t: "special" },
            { n: "Thunder 🔥", p: 12000, d: 1600, t: "special" }, { n: "Cyber 🔥", p: 25000, d: 3500, t: "special" },
            { n: "Quantum 🔥", p: 50000, d: 8000, t: "special" }
        ];

        // --- RENDER GRID ---
        const g = document.getElementById('mGrid');
        machines.forEach((m, i) => {
            g.innerHTML += `
                <div class="m-card">
                    ${m.t=='special'?'<div class="special-tag">HOT</div>':''}
                    <p style="font-size:0.75rem; font-weight:600;">${m.n}</p>
                    <p style="color:#10b981; font-size:0.6rem;">Daily: PKR ${m.d}</p>
                    <button onclick="buyM(${i})" class="btn-main" style="font-size:0.6rem; padding:6px;">PKR ${m.p}</button>
                </div>`;
        });

        // --- AUTH & SYNC ---
        document.getElementById('loginBtn').onclick = () => signInAnonymously(auth);
        document.getElementById('logoutBtn').onclick = () => signOut(auth).then(()=>location.reload());

        onAuthStateChanged(auth, async (user) => {
            if(user) {
                document.getElementById('loginPage').classList.remove('active');
                document.getElementById('dashboardPage').classList.add('active');
                const userRef = doc(db, "users", user.uid);
                const snap = await getDoc(userRef);
                if(!snap.exists()) await setDoc(userRef, { balance: 0, uid: user.uid, dailyProfit: 0, activeMachine: "None", lastClaim: 0 });

                onSnapshot(userRef, (d) => {
                    document.getElementById('userBal').innerText = d.data().balance.toFixed(2);
                    document.getElementById('activeM').innerText = d.data().activeMachine;
                });

                onSnapshot(query(collection(db, "transactions"), where("uid", "==", user.uid)), (s) => {
                    const l = document.getElementById('logs'); l.innerHTML = "";
                    s.forEach(doc => { const d = doc.data(); l.innerHTML += `<tr><td>${d.type}</td><td>${d.amount}</td><td style="color:${d.status=='approved'?'#10b981':'#fbbf24'}">${d.status}</td></tr>`; });
                });
            }
        });

        // --- ACTIONS ---
        window.buyM = async (i) => {
            const m = machines[i];
            const uRef = doc(db, "users", auth.currentUser.uid);
            const bal = (await getDoc(uRef)).data().balance;
            if(bal < m.p) return alert("Paisa kam hai sweetie! 😘");
            await updateDoc(uRef, { balance: increment(-m.p), activeMachine: m.n, dailyProfit: m.d });
            alert(m.n + " Activated! ⛏️");
        };

        document.getElementById('taskBtn').onclick = async () => {
            const uRef = doc(db, "users", auth.currentUser.uid);
            const d = (await getDoc(uRef)).data();
            if(d.dailyProfit === 0) return alert("Pehle Machine lo sweetie! ✋");
            if(Date.now() - d.lastClaim < 86400000) return alert("Wait 24h! ⏳");
            alert("Mining in Progress... 10s wait! ⏳");
            setTimeout(async () => {
                await updateDoc(uRef, { balance: increment(d.dailyProfit), lastClaim: Date.now() });
                alert("Profit Added! 💰");
            }, 10000);
        };

        document.getElementById('txnBtn').onclick = async () => {
            const amt = Number(document.getElementById('txnAmt').value);
            const type = document.getElementById('txnType').value;
            if(type=="Deposit" && amt < 200) return alert("Min 200 Deposit! 😘");
            if(type=="Withdraw" && amt < 100) return alert("Min 100 Withdraw! 😘");
            await addDoc(collection(db, "transactions"), { uid: auth.currentUser.uid, amount: amt, type: type, method: document.getElementById('mthd').value, status: "pending", timestamp: serverTimestamp() });
            alert("Request Sent! ✅");
        };

        // --- ADMIN ---
        window.adminLogin = () => { if(prompt("Password:")=="admin123") { document.getElementById('loginPage').style.display='none'; document.getElementById('adminPage').classList.add('active'); loadAdmin(); } };
        function loadAdmin() {
            onSnapshot(query(collection(db, "transactions"), where("status", "==", "pending")), (s) => {
                const al = document.getElementById('adminList'); al.innerHTML = "";
                s.forEach(ds => { const d = ds.data(); al.innerHTML += `<div class="glass-card">${d.type}: ${d.amount} (${d.method})<button onclick="approve('${ds.id}','${d.uid}',${d.amount},'${d.type}')" class="btn-main" style="padding:5px; background:#10b981;">Approve</button></div>`; });
            });
        }
        window.approve = async (id, uid, amt, type) => {
            await updateDoc(doc(db, "transactions", id), { status: "approved" });
            const change = type=="Deposit" ? amt : -(amt + (amt*0.05));
            await updateDoc(doc(db, "users", uid), { balance: increment(change) });
            alert("Done! ✅");
        };
    </script>
</body>
</html>
