<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional Mining 🇵🇰</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        /* --- PREMIUM UI --- */
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #020617; color: white; min-height: 100vh; padding-bottom: 50px; }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(15px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 20px; padding: 20px; width: 92%; max-width: 450px; margin: 20px auto; text-align: center; }
        .brand-name { font-size: 2rem; font-weight: 600; color: #fbbf24; margin: 20px 0; }
        input, select { width: 100%; padding: 12px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 10px; color: white; margin-bottom: 12px; outline: none; }
        .btn-main { width: 100%; padding: 12px; background: linear-gradient(90deg, #fbbf24, #d97706); border: none; border-radius: 10px; color: black; font-weight: bold; cursor: pointer; transition: 0.3s; }
        .btn-main:hover { opacity: 0.8; transform: scale(1.01); }
        
        #authSection, #dashboardPage, #adminPage { display: none; }
        .active { display: block !important; }
        .machine-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 15px; }
        .m-card { background: rgba(255,255,255,0.05); border-radius: 12px; padding: 10px; border: 1px solid rgba(255,255,255,0.1); font-size: 0.7rem; }
        .wa-btn { position: fixed; bottom: 20px; right: 20px; background: #25d366; color: white; width: 55px; height: 55px; border-radius: 50%; display: flex; align-items: center; justify-content: center; text-decoration: none; font-size: 28px; z-index: 999; box-shadow: 0 5px 15px rgba(0,0,0,0.4); }
    </style>
</head>
<body>

    <a href="https://wa.me/923705519562" class="wa-btn" target="_blank">💬</a>

    <div id="authSection" class="active">
        <div class="brand-name" style="text-align: center;">PakGold</div>
        <div class="glass-card">
            <h3 id="authTitle" style="margin-bottom: 15px; color: #fbbf24;">Sign In</h3>
            <input type="text" id="email" placeholder="Email Address (e.g. user@gmail.com)">
            <input type="password" id="pass" placeholder="Password">
            <button id="mainAuthBtn" class="btn-main">Login Now</button>
            <p id="toggleAuth" style="font-size: 0.7rem; color: #4facfe; margin-top: 15px; cursor: pointer;">Don't have an account? Sign Up</p>
            <p onclick="adminAccess()" style="font-size: 0.5rem; color: #94a3b8; margin-top: 20px; cursor: pointer;">Admin Panel Access</p>
        </div>
    </div>

    <div id="dashboardPage" class="glass-card">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
            <span style="color: #fbbf24; font-weight: bold;">Dashboard</span>
            <button id="logoutBtn" style="background:none; border:1px solid #ef4444; color:#ef4444; font-size: 0.6rem; padding:3px 8px; border-radius:5px;">Logout</button>
        </div>

        <div style="background: rgba(251, 191, 36, 0.1); padding: 15px; border-radius: 15px; border: 1px solid rgba(251, 191, 36, 0.2);">
            <p style="font-size: 0.65rem; color: #94a3b8;">Welcome back, <span id="userName" style="color: #fff;">Miner</span></p>
            <h2 style="color: #fbbf24; margin: 10px 0;">PKR <span id="userBal">0.00</span></h2>
            <p style="font-size: 0.6rem;">Active Machine: <b id="activeM" style="color:#10b981;">None</b></p>
        </div>

        <button id="taskBtn" class="btn-main" style="background: #10b981; color: white;">Claim Daily Mining Reward ⛏️</button>

        <h4 style="text-align: left; margin-top: 25px; font-size: 0.8rem; color: #fbbf24;">Mining Machines 🏪</h4>
        <div id="mGrid" class="machine-grid"></div>

        <h4 style="text-align: left; margin-top: 25px; font-size: 0.8rem; color: #4facfe;">Wallet Operations 🏦</h4>
        <div style="margin-top: 10px;">
            <select id="txnType"><option value="Deposit">Deposit (Min 200)</option><option value="Withdraw">Withdraw (Min 100)</option></select>
            <input type="number" id="txnAmt" placeholder="Amount">
            <button id="txnBtn" class="btn-main" style="background: #4facfe; color: white;">Submit Request</button>
        </div>
    </div>

    <div id="adminPage" class="glass-card">
        <h2 style="color: #fbbf24;">Admin Control</h2>
        <div id="adminList" style="margin-top: 20px;"></div>
        <button onclick="location.reload()" class="btn-main" style="background: #ef4444; color: white; margin-top: 15px;">Exit</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-auth.js";
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

        let isLoginMode = true;

        // --- AUTH TOGGLE ---
        document.getElementById('toggleAuth').onclick = () => {
            isLoginMode = !isLoginMode;
            document.getElementById('authTitle').innerText = isLoginMode ? "Sign In" : "Sign Up";
            document.getElementById('mainAuthBtn').innerText = isLoginMode ? "Login Now" : "Create Account";
            document.getElementById('toggleAuth').innerText = isLoginMode ? "Don't have an account? Sign Up" : "Already have an account? Sign In";
        };

        // --- SIGN IN / SIGN UP ---
        document.getElementById('mainAuthBtn').onclick = async () => {
            const email = document.getElementById('email').value;
            const pass = document.getElementById('pass').value;
            if(!email || !pass) return alert("Details fill karein sweetie! 😘");

            try {
                if(isLoginMode) {
                    await signInWithEmailAndPassword(auth, email, pass);
                } else {
                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    await setDoc(doc(db, "users", res.user.uid), { balance: 0, uid: res.user.uid, dailyProfit: 0, activeMachine: "None", lastClaim: 0, email: email });
                }
            } catch (e) { alert("Error: " + e.message); }
        };

        document.getElementById('logoutBtn').onclick = () => signOut(auth).then(()=>location.reload());

        // --- APP LOGIC ---
        onAuthStateChanged(auth, async (user) => {
            if(user) {
                document.getElementById('authSection').classList.remove('active');
                document.getElementById('dashboardPage').classList.add('active');
                onSnapshot(doc(db, "users", user.uid), (d) => {
                    const data = d.data();
                    document.getElementById('userBal').innerText = data.balance.toFixed(2);
                    document.getElementById('activeM').innerText = data.activeMachine;
                    document.getElementById('userName').innerText = data.email.split('@')[0];
                });
            }
        });

        // --- RENDER MACHINES (Example Data) ---
        const machines = [{n:"Nano",p:200,d:14},{n:"Micro",p:500,d:35},{n:"Silver",p:1000,d:70},{n:"Gold",p:2000,d:150}];
        const mGrid = document.getElementById('mGrid');
        machines.forEach((m, i) => {
            mGrid.innerHTML += `<div class="m-card"><p><b>${m.n}</b></p><p style="color:#10b981;">Daily: ${m.d}</p><button onclick="buyM('${m.n}', ${m.p}, ${m.d})" class="btn-main" style="padding:5px; font-size:0.6rem; margin-top:5px;">PKR ${m.p}</button></div>`;
        });

        window.buyM = async (name, price, daily) => {
            const uRef = doc(db, "users", auth.currentUser.uid);
            const bal = (await getDoc(uRef)).data().balance;
            if(bal < price) return alert("Paisa kam hai sweetie! 😘");
            await updateDoc(uRef, { balance: increment(-price), activeMachine: name, dailyProfit: daily });
            alert(name + " Activated! ✅");
        };

        document.getElementById('taskBtn').onclick = async () => {
            const uRef = doc(db, "users", auth.currentUser.uid);
            const d = (await getDoc(uRef)).data();
            if(d.dailyProfit === 0) return alert("Pehle machine kharidein! ✋");
            if(Date.now() - d.lastClaim < 86400000) return alert("Kal aana sweetie! ⏳");
            alert("Mining... 10s wait! ⏳");
            setTimeout(async () => {
                await updateDoc(uRef, { balance: increment(d.dailyProfit), lastClaim: Date.now() });
                alert("Profit Added! 💰");
            }, 10000);
        };

        document.getElementById('txnBtn').onclick = async () => {
            const amt = Number(document.getElementById('txnAmt').value);
            const type = document.getElementById('txnType').value;
            if(type=="Deposit" && amt < 200) return alert("Min 200 Deposit!");
            await addDoc(collection(db, "transactions"), { uid: auth.currentUser.uid, amount: amt, type: type, status: "pending", timestamp: serverTimestamp() });
            alert("Request Sent! ✅");
        };

        // --- ADMIN PANEL ---
        window.adminAccess = () => { if(prompt("Pass:")=="admin123") { document.getElementById('authSection').style.display='none'; document.getElementById('adminPage').classList.add('active'); loadAdmin(); } };
        function loadAdmin() {
            onSnapshot(query(collection(db, "transactions"), where("status", "==", "pending")), (s) => {
                const al = document.getElementById('adminList'); al.innerHTML = "";
                s.forEach(ds => { const d = ds.data(); al.innerHTML += `<div class="glass-card" style="font-size:0.7rem;">${d.type}: ${d.amount}<button onclick="approve('${ds.id}','${d.uid}',${d.amount},'${d.type}')" class="btn-main" style="padding:5px; background:#10b981; color:white;">Approve</button></div>`; });
            });
        }
        window.approve = async (id, uid, amt, type) => {
            await updateDoc(doc(db, "transactions", id), { status: "approved" });
            const change = type=="Deposit" ? amt : -(amt + (amt*0.05));
            await updateDoc(doc(db, "users", uid), { balance: increment(change) });
            alert("Approved! ✅");
        };
    </script>
</body>
</html>
