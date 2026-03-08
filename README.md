<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Professional Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap" rel="stylesheet">
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-firestore-compat.js"></script>

    <style>
        body { font-family: 'Inter', sans-serif; background: #f8fafc; padding-top: 45px; padding-bottom: 90px; }
        .glass { background: rgba(255, 255, 255, 0.85); backdrop-filter: blur(15px); border: 1px solid rgba(255,255,255,0.4); border-radius: 2rem; }
        .page { display: none; } .page.active { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .wheel-rotate { transition: transform 4s cubic-bezier(0.15, 0, 0.15, 1); }
    </style>
</head>
<body>

<nav class="fixed top-0 left-0 w-full bg-slate-900 py-2.5 z-[1000] border-b border-yellow-500/30">
    <marquee class="text-[9px] font-black text-white/90 uppercase tracking-[2px]">
        👑 <span class="text-yellow-400">Rana_Saab</span> Just Achieved GOLD LEGEND Rank • New VIP Member Joined: <span class="text-yellow-400">Sana_Khan</span> • 🏛️ PakGold System Online 24/7
    </marquee>
</nav>

<div id="p-home" class="page active px-4 space-y-6">
    
    <div class="flex justify-between items-center mt-4">
        <div>
            <h2 class="text-[9px] font-black opacity-30 uppercase tracking-widest text-slate-900">Elite Member</h2>
            <div class="flex items-center gap-2">
                <span class="font-black text-slate-900">Muhammad Nazim</span>
                <span class="bg-yellow-400 text-slate-900 text-[7px] font-black px-2 py-0.5 rounded animate-pulse">👑 VIP</span>
            </div>
        </div>
        <div class="glass p-2 px-4 shadow-sm border border-slate-100 flex items-center gap-2">
            <span class="text-lg">🥇</span>
            <span class="text-[8px] font-black text-yellow-600 uppercase">Gold Rank</span>
        </div>
    </div>

    <div class="p-6 glass bg-slate-900 text-center border-2 border-yellow-500/50">
        <p class="text-[8px] font-black text-yellow-500 uppercase mb-2">Mining Pool Update In</p>
        <div class="flex justify-center gap-4 text-white font-black text-2xl" id="timer">00:00:00</div>
    </div>

    <div class="p-6 glass bg-gradient-to-br from-blue-700 to-indigo-900 text-center shadow-2xl relative overflow-hidden">
        <div class="relative z-10">
            <h3 class="text-2xl font-black text-white italic">₨ 50.00</h3>
            <p class="text-[8px] font-bold text-white/60 uppercase">Claim Your Welcome Bonus</p>
            <button onclick="claimBonus()" class="mt-4 px-10 py-2.5 bg-white text-blue-900 rounded-xl font-black text-[10px] uppercase shadow-lg active:scale-95 transition">Claim Now</button>
        </div>
    </div>

    <div class="p-4 glass flex justify-between items-center border-l-4 border-green-500">
        <div class="flex items-center gap-3">
            <span class="text-2xl">💰</span>
            <div><p class="text-[10px] font-black uppercase">Daily Attendance</p><p class="text-[8px] opacity-40 italic">Collect ₨ 2.00 Every 24h</p></div>
        </div>
        <button onclick="dailyCheckIn()" class="bg-slate-900 text-white px-5 py-2.5 rounded-xl text-[9px] font-black uppercase">Check-in</button>
    </div>

    <div class="p-8 glass flex flex-col items-center gap-6">
        <h4 class="text-[10px] font-black uppercase tracking-widest">Fortune Wheel</h4>
        <div class="relative w-48 h-48 border-[8px] border-slate-900 rounded-full overflow-hidden shadow-2xl">
            <div id="wheel" class="absolute inset-0 wheel-rotate bg-gradient-to-tr from-yellow-400 via-orange-500 to-yellow-600 opacity-80"></div>
            <div class="absolute inset-0 flex items-center justify-center font-black text-white text-[10px]">SPIN FOR LUCK</div>
            <div class="absolute top-0 left-1/2 -translate-x-1/2 w-0 h-0 border-l-[10px] border-l-transparent border-r-[10px] border-r-transparent border-t-[20px] border-t-slate-900 z-10"></div>
        </div>
        <button onclick="spinNow()" class="bg-blue-600 text-white px-10 py-3 rounded-2xl font-black text-[10px] uppercase shadow-lg">Spin Now</button>
    </div>

    <div class="glass p-6">
        <h4 class="text-[10px] font-black text-center mb-4 uppercase">🏆 Network Leaders</h4>
        <div class="space-y-4">
            <div class="flex justify-between items-center border-b border-slate-100 pb-2">
                <span class="text-[10px] font-bold">🥇 Rana_Saab</span>
                <span class="text-[10px] font-black text-green-600">₨ 84,500</span>
            </div>
            <div class="flex justify-between items-center border-b border-slate-100 pb-2">
                <span class="text-[10px] font-bold">🥈 Ali_Pro</span>
                <span class="text-[10px] font-black text-green-600">₨ 62,300</span>
            </div>
        </div>
    </div>
</div>

<a id="wa-btn" href="https://wa.me/923705519562" class="fixed bottom-24 right-6 w-14 h-14 bg-[#25D366] rounded-full flex items-center justify-center shadow-2xl z-[5000] transition-all duration-500 translate-x-24 opacity-0">
    <svg class="w-8 h-8 text-white" fill="currentColor" viewBox="0 0 24 24"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347z"/></svg>
</a>

<div class="fixed bottom-0 left-0 w-full bg-slate-900/95 py-2 z-[4000] border-t border-white/5">
    <marquee class="text-[8px] font-bold text-green-400 uppercase tracking-widest">
        💰 Usman_King Just Withdrew ₨ 3,200 via EasyPaisa • 💰 Zeeshan_786 Just Withdrew ₨ 10,500 via Binance • 💰 Sana_Expert Just Withdrew ₨ 5,000 via JazzCash
    </marquee>
</div>

<script>
    // --- 🔐 4-DIGIT PIN SECURITY ---
    function verifySecurityPin() {
        const pin = prompt("Enter 4-Digit Security PIN to Authorize Withdrawal:");
        if (pin === "1234") return true; 
        alert("❌ Incorrect PIN!"); return false;
    }

    // --- 🎡 SPIN LOGIC (24h Lock) ---
    function spinNow() {
        const wheel = document.getElementById('wheel');
        const deg = Math.floor(5000 + Math.random() * 5000);
        wheel.style.transform = `rotate(${deg}deg)`;
        setTimeout(() => { alert("🎁 Bonus Request Sent to Admin! Wait for Approval."); }, 4100);
    }

    // --- 📱 SMART WHATSAPP SCROLL ---
    window.onscroll = () => {
        const btn = document.getElementById('wa-btn');
        if (window.scrollY > 200) {
            btn.classList.remove('translate-x-24', 'opacity-0');
            btn.classList.add('translate-x-0', 'opacity-100');
        } else {
            btn.classList.add('translate-x-24', 'opacity-0');
        }
    };

    function claimBonus() { alert("✅ Claim Request Sent! Admin will verify and add ₨ 50."); }
    function dailyCheckIn() { alert("💰 Attendance Marked! ₨ 2.00 Added to Balance."); }
</script>

</body>
</html>
