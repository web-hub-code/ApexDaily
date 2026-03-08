<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secret Admin | Control Center</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #020617; color: white; display: flex; justify-content: center; padding: 40px; }

        /* Admin Glass Card */
        .admin-card { background: rgba(30, 41, 59, 0.7); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 20px; width: 100%; max-width: 900px; padding: 30px; box-shadow: 0 20px 50px rgba(0,0,0,0.5); }

        h2 { color: #4facfe; margin-bottom: 20px; border-bottom: 1px solid rgba(255,255,255,0.1); padding-bottom: 10px; }

        /* Table Styling */
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th { text-align: left; padding: 15px; background: rgba(79, 172, 254, 0.1); color: #4facfe; border-bottom: 2px solid #4facfe; }
        td { padding: 15px; border-bottom: 1px solid rgba(255,255,255,0.05); }

        .btn-approve { background: #10b981; color: white; border: none; padding: 8px 15px; border-radius: 8px; cursor: pointer; font-weight: bold; transition: 0.3s; }
        .btn-approve:hover { background: #059669; transform: scale(1.05); }
        
        .status-tag { padding: 4px 10px; border-radius: 12px; font-size: 0.75rem; background: rgba(251, 191, 36, 0.2); color: #fbbf24; border: 1px solid #fbbf24; }
        
        #notify { position: fixed; top: 20px; right: 20px; background: #4facfe; padding: 15px; border-radius: 10px; display: none; }
    </style>
</head>
<body>

    <div id="notify">Action Done, sweetie! ✅</div>

    <div class="admin-card">
        <h2>Admin Management - Deposits 📥</h2>
        <p style="font-size: 0.9rem; color: #94a3b8;">Manage user payments and add balance manually.</p>

        <table>
            <thead>
                <tr>
                    <th>User ID</th>
                    <th>Method</th>
                    <th>Amount</th>
                    <th>Status</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody id="depositTable">
                </tbody>
        </table>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, collection, query, where, onSnapshot, doc, updateDoc, increment } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

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

        // Live Listen to Pending Deposits
        const q = query(collection(db, "deposits"), where("status", "==", "pending"));

        onSnapshot(q, (snapshot) => {
            const tableBody = document.getElementById('depositTable');
            tableBody.innerHTML = "";
            
            snapshot.forEach((docSnap) => {
                const data = docSnap.data();
                const id = docSnap.id;

                tableBody.innerHTML += `
                    <tr>
                        <td><code>${data.uid.substring(0, 8)}...</code></td>
                        <td>${data.method}</td>
                        <td style="color: #10b981; font-weight: bold;">PKR ${data.amount}</td>
                        <td><span class="status-tag">Pending</span></td>
                        <td><button onclick="approve('${id}', '${data.uid}', ${data.amount})" class="btn-approve">Approve ✅</button></td>
                    </tr>
                `;
            });
        });

        // Approve Logic
        window.approve = async (docId, userUid, amount) => {
            try {
                // 1. Mark Deposit as Approved
                await updateDoc(doc(db, "deposits", docId), { status: "approved" });

                // 2. Add Balance to User Account
                const userRef = doc(db, "users", userUid);
                await updateDoc(userRef, {
                    balance: increment(Number(amount))
                });

                showNote();
            } catch (err) {
                console.error(err);
                alert("Error approving deposit!");
            }
        };

        function showNote() {
            const n = document.getElementById('notify');
            n.style.display = 'block';
            setTimeout(() => n.style.display = 'none', 3000);
        }
    </script>
</body>
</html>
