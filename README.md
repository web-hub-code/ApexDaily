<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Digital Earnings 🇵🇰</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #020617; color: white; min-height: 100vh; display: flex; flex-direction: column; align-items: center; }

        .blob { position: fixed; width: 300px; height: 300px; background: #fbbf24; filter: blur(100px); border-radius: 50%; z-index: -1; top: -5%; right: -5%; opacity: 0.2; }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 20px; padding: 20px; width: 92%; max-width: 450px; text-align: center; margin: 15px auto; }

        .brand-name { font-size: 1.6rem; font-weight: 600; color: #fbbf24; }
        input, select { width: 100%; padding: 12px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 10px; color: white; margin-bottom: 10px; outline: none; }
        .btn-main { width: 100%; padding: 12px; background: linear-gradient(90deg, #fbbf24, #d97706); border: none; border-radius: 10px; color: black; font-weight: bold; cursor: pointer; transition: 0.3s; margin-top: 5px; }
        
        #loginPage, #dashboardPage, #termsModal { display: none; }
        .active { display: block !important; }
        .overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.9); z-index: 5000; display: flex; justify-content: center; align-items: center; }

        .task-box { background: rgba(16, 185, 129, 0.1); padding: 10px; border-radius: 8px; margin-bottom: 8px; font-size: 0.8rem; border: 1px solid rgba(16, 185, 129, 0.3); cursor: pointer; }
        .history-table { width: 100%; font-size: 0.7rem; border-collapse: collapse; margin-top: 10px; }
        .history-table td { padding: 8px; border-bottom: 1px solid rgba(255,255,255,0.05); }
    </style>
</head>
<body>

    <div class="blob"></div>

    <div id="termsModal" class="overlay">
        <div class="glass-card">
            <h2 style="color: #fbbf24;">PakGold Rules 🇵🇰</h2>
            <div style="text-align: left; font-size: 0.75rem; margin: 15px 0; color: #cbd5e1;">
                <p>• Min. Deposit: <b>PKR 200</b></p>
                <p>• Min. Withdraw: <b>PKR 100</b></p>
                <p>• Complete Daily Tasks to earn profit.</p>
                <p>• Referral Commission: 10% instant.</p>
            </div>
            <button id="acceptBtn" class="btn-main">Accept & Start Earning</button>
        </div>
    </div>

    <div id="loginPage" class="glass-card">
        <div class="brand-name">PakGold</div>
        <p style="font-size: 0.7rem; color: #94a3b8; margin-bottom: 20px;">Safe Investment & Daily Tasks</p>
        <button id="loginBtn" class="btn-main">Login Anonymously</button>
    </div>

    <div id="dashboardPage" class="glass-card">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
            <span class="brand-name" style="font-size: 1.1rem;">PakGold Dashboard</span>
            <button id="logoutBtn" style="background:none; color:#ef4444; border:1px solid #ef4444; font-size:0.6rem; padding:3px 7px; border-radius:5px; cursor:pointer;">Logout</button>
        </div>

        <div style="background: rgba(255,255,255,0.02); padding: 15px; border-radius: 15px; border: 1px solid rgba(251, 191, 36, 0.2);">
            <p style="font-size: 0.7rem; color: #94a3b8;">Current Balance</p>
            <h2 style="color: #fbbf24;">PKR <span id="userBal">0.00</span></h2>
        </div>

        <div style="margin-top: 20px; text-align: left;">
            <h4 style="font-size: 0.9rem; color: #10b981;">Daily Work Tasks 🛠️</h4>
            <div id="taskList" style="margin-top: 10px;">
                <div class="task-box" onclick="runTask(1)">Task 1: Visit Mining Site (PKR 20)</div>
                <div class="task-box" onclick="runTask(2)">Task 2: Check Market Rates (PKR 20)</div>
            </div>
        </div>

        <hr style="opacity: 0.1; margin: 15px 0;">

        <select id="type"><option value="Deposit">Deposit</option><option value="Withdraw">Withdraw</option></select>
        <select id="mthd"><option value="JazzCash">JazzCash</option><option value="EasyPaisa">EasyPaisa</option></select>
        <input type="number" id="amt" placeholder="Amount (Min. 100/200)">
        <button id="subBtn" class="btn-main">Submit Transaction</button>

        <table class="history-table">
            <thead><tr style="color:#94a3b8;"><th>Type</th><th>PKR</th><th>Status</th></tr></thead>
            <tbody id="txnLog"></tbody>
        </table>
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

        // --- UI LOGIC ---
        const modal = document.getElementById('termsModal');
        if(!localStorage.getItem('pakgold_terms')) modal.classList.add('active');
        document.getElementById('acceptBtn').onclick = () => { modal.classList.remove('active'); localStorage.setItem('pakgold_terms', 'true'); };

        document.getElementById('loginBtn').onclick = () => signInAnonymously(auth);
        document.getElementById('logoutBtn').onclick = () => signOut(auth).then(()=>location.reload());

        onAuthStateChanged(auth, async (user) => {
            if(user) {
                document.getElementById('loginPage').style.display = 'none';
                document.getElementById('dashboardPage').classList.add('active');
                
                const userRef = doc(db, "users", user.uid);
                const snap = await getDoc(userRef);
                if(!snap.exists()) await setDoc(userRef, { balance: 0, uid: user.uid, lastTask: null });

                onSnapshot(userRef, (d) => { document.getElementById('userBal').innerText = d.data().balance.toFixed(2); });

                const q = query(collection(db, "transactions"), where("uid", "==", user.uid));
                onSnapshot(q, (s) => {
                    const l = document.getElementById('txnLog'); l.innerHTML = "";
                    s.forEach(doc => { const d = doc.data(); l.innerHTML += `<tr><td>${d.type}</td><td>${d.amount}</td><td style="color:${d.status=='approved'?'#10b981':'#fbbf24'}">${d.status}</td></tr>`; });
                });
            } else { document.getElementById('loginPage').classList.add('active'); }
        });

        // --- TASK SYSTEM ---
        window.runTask = async (id) => {
            alert("Starting Task... Please wait 10 seconds sweetie! ⏳");
            setTimeout(async () => {
                await updateDoc(doc(db, "users", auth.currentUser.uid), { balance: increment(20) });
                alert("Task Complete! PKR 20 added to balance. 💰");
            }, 10000);
        };

        // --- TRANSACTIONS (New Limits) ---
        document.getElementById('subBtn').onclick = async () => {
            const amt = Number(document.getElementById('amt').value);
            const type = document.getElementById('type').value;

            // NEW LIMITS
            if(type === "Deposit" && amt < 200) return alert("Min Deposit PKR 200 hai sweetie! 😘");
            if(type === "Withdraw" && amt < 100) return alert("Min Withdraw PKR 100 hai sweetie! 😘");

            await addDoc(collection(db, "transactions"), {
                uid: auth.currentUser.uid, amount: amt, type: type,
                method: document.getElementById('mthd').value, status: "pending", timestamp: serverTimestamp()
            });
            alert("Request Sent Successfully! ✅");
        };
    </script>
</body>
</html>
