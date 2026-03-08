<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background: #f8fafc; padding-bottom: 100px; }
        .glass { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-radius: 2.5rem; border: 1px solid rgba(255,255,255,0.4); }
        .page { display: none; } .page.active { display: block; animation: slideUp 0.3s ease-out; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <nav class="fixed top-0 left-0 w-full bg-slate-900 py-2.5 z-[10000] border-b border-yellow-500/50">
        <marquee class="text-[10px] font-black text-white uppercase tracking-[2px]">
            👑 <span class="text-yellow-400">Muhammad Nazim</span> Joined • New Sale: Machine R-01 (₨ 200) • 28+5 Plans Live • Deposit Active
        </marquee>
    </nav>

    <div id="p-home" class="page active px-4 space-y-6 pt-12">
        
        <div class="p-8 glass bg-slate-900 text-center relative overflow-hidden shadow-2xl mt-4">
            <p class="text-[9px] font-black text-yellow-500 uppercase tracking-[4px] mb-2">Total Earnings</p>
            <h2 class="text-4xl font-black text-white italic">₨ <span id="user-bal">552.00</span></h2>
            <div class="mt-4 flex justify-center gap-2">
                <span class="px-3 py-1 bg-green-500/20 text-green-400 text-[7px] font-black rounded-full uppercase italic">Verified Prime User 🛡️</span>
            </div>
        </div>

        <div class="space-y-4">
            <h3 class="text-[11px] font-black uppercase tracking-[3px] text-slate-900 italic ml-2">Regular Nodes (28)</h3>
            
            <div class="p-6 glass bg-white border-b-4 border-blue-500 shadow-xl relative overflow-hidden">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-[8px] font-black text-blue-500 uppercase">Machine R-01</p>
                        <h4 class="text-2xl font-black italic text-slate-900">₨ 200.00</h4>
                    </div>
                    <div class="text-right">
                        <p class="text-[12px] font-black text-green-600">+ ₨ 10/day</p>
                        <p class="text-[8px] font-bold opacity-40 uppercase">30 Days Plan</p>
                    </div>
                </div>
                <button onclick="buyMachine('R-01', 200)" class="w-full mt-6 py-4 bg-slate-900 text-white rounded-2xl font-black text-[10px] uppercase tracking-widest active:scale-95 transition shadow-lg">Buy Now</button>
            </div>
        </div>

        <div class="space-y-4">
            <h3 class="text-[11px] font-black uppercase tracking-[3px] text-orange-600 italic ml-2">Special Nodes (5)</h3>
            
            <div class="p-8 glass bg-slate-900 border-b-4 border-yellow-500 shadow-2xl relative overflow-hidden">
                <div class="relative z-10">
                    <p class="text-[9px] font-black text-yellow-500 uppercase tracking-widest">Diamond S-01</p>
                    <h4 class="text-3xl font-black italic text-white mt-1">₨ 5,000.00</h4>
                    <div class="grid grid-cols-2 gap-4 mt-6 py-4 border-y border-white/10">
                        <div class="text-center"><p class="text-[7px] font-black text-white/40 uppercase">Daily Profit</p><p class="text-sm font-black text-green-400">₨ 450.00</p></div>
                        <div class="text-center border-l border-white/10"><p class="text-[7px] font-black text-white/40 uppercase">Duration</p><p class="text-sm font-black text-yellow-500">15 Days</p></div>
                    </div>
                    <button onclick="buyMachine('S-01', 5000)" class="w-full mt-6 py-4 bg-yellow-500 text-slate-900 rounded-2xl font-black text-[11px] uppercase tracking-widest shadow-xl active:scale-95 transition">Buy Special Node</button>
                </div>
            </div>
        </div>

        <div class="glass p-6 space-y-4 border-t-4 border-green-600 shadow-2xl">
            <h3 class="text-[10px] font-black uppercase text-center tracking-widest">Deposit System</h3>
            <div class="grid grid-cols-3 gap-2">
                <button onclick="setBank('EasyPaisa', '03379827882')" class="bg-white border-2 border-green-500 p-2 rounded-xl text-[8px] font-black uppercase text-green-600">EasyPaisa</button>
                <button onclick="setBank('JazzCash', '03705519562')" class="bg-white border-2 border-red-500 p-2 rounded-xl text-[8px] font-black uppercase text-red-600">JazzCash</button>
                <button onclick="setBank('SadaPay', '03705519562')" class="bg-white border-2 border-slate-900 p-2 rounded-xl text-[8px] font-black uppercase text-slate-900">SadaPay</button>
            </div>
            <div class="bg-slate-100 p-4 rounded-3xl text-center border-2 border-dashed">
                <p class="text-[9px] font-black opacity-40 uppercase" id="bank-name">Select Method</p>
                <p class="text-xl font-black text-slate-900" id="bank-acc">----</p>
                <p class="text-[8px] font-bold text-blue-600 mt-1 uppercase italic">Owner: Muhammad Nazim</p>
            </div>
            <input type="number" id="d-amt" placeholder="Amount Sent" class="w-full bg-white rounded-2xl p-4 text-xs font-bold border-none shadow-sm">
            <label class="block w-full py-4 bg-slate-900 text-white rounded-2xl text-center cursor-pointer">
                <span class="text-[9px] font-black uppercase">📸 Upload Screenshot</span>
                <input type="file" id="d-proof" class="hidden" onchange="previewImg(this)">
            </label>
            <div id="pre-box" class="hidden"><img id="pre-img" class="w-full h-32 object-cover rounded-xl mt-2"></div>
            <button onclick="submitDep()" class="w-full py-4 bg-green-600 text-white rounded-2xl font-black text-[10px] uppercase shadow-xl">Submit to Admin</button>
        </div>

        <div id="admin-history" class="glass p-6 space-y-4 border-t-4 border-blue-600 hidden">
            <h3 class="text-[10px] font-black uppercase text-center tracking-widest">Admin: Live Machine Sales</h3>
            <div id="order-list" class="space-y-2">
                <p class="text-[8px] text-center opacity-40">Waiting for new purchases...</p>
            </div>
        </div>
    </div>

    <div class="fixed bottom-0 left-0 w-full bg-white/95 backdrop-blur-md border-t py-4 px-12 flex justify-between items-center z-[10000]">
        <div class="flex flex-col items-center gap-1 text-blue-600 font-black text-[8px] uppercase"><span>🏠</span>Home</div>
        <div onclick="toggleAdmin()" class="flex flex-col items-center gap-1 text-slate-400 font-black text-[8px] uppercase"><span>⚙️</span>Admin</div>
    </div>

    <script>
        function setBank(m, n) {
            document.getElementById('bank-name').innerText = "Transfer to " + m;
            document.getElementById('bank-acc').innerText = n;
        }

        function buyMachine(name, price) {
            const pin = prompt("🔐 Enter 4-Digit Security PIN to Purchase:");
            if(pin === "1234") {
                alert(`✅ ${name} Purchased! Admin will process.`);
                // Logic to add to order-list
                const list = document.getElementById('order-list');
                const order = document.createElement('div');
                order.className = "bg-slate-100 p-3 rounded-xl flex justify-between text-[8px] font-bold uppercase";
                order.innerHTML = `<span>${name}</span> <span class="text-green-600">₨ ${price}</span>`;
                list.prepend(order);
            } else { alert("❌ Wrong PIN!"); }
        }

        function submitDep() { alert("🚀 Request Sent to Nazim!"); }
        function toggleAdmin() { document.getElementById('admin-history').classList.toggle('hidden'); }
        function previewImg(i) {
            if (i.files && i.files[0]) {
                var r = new FileReader();
                r.onload = (e) => { 
                    document.getElementById('pre-img').src = e.target.result;
                    document.getElementById('pre-box').classList.remove('hidden');
                };
                r.readAsDataURL(i.files[0]);
            }
        }
    </script>
</body>
</html>
