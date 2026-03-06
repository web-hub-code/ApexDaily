<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Ultimate Trading Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: white; font-family: 'Outfit', sans-serif; }
        
        .neon-border { border: 1px solid rgba(59, 130, 246, 0.5); box-shadow: 0 0 15px rgba(59, 130, 246, 0.2); }
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.08); }
        
        /* Countdown Animation */
        .timer-glow { color: #22c55e; text-shadow: 0 0 8px rgba(34, 197, 94, 0.5); font-family: monospace; }
        
        .btn-gradient { background: linear-gradient(90deg, #3b82f6, #8b5cf6); transition: 0.3s; }
        .btn-gradient:active { transform: scale(0.95); }

        /* Help Desk Appearance */
        #helpDesk { transition: all 0.4s ease; transform: translateY(100%); }
        #helpDesk.open { transform: translateY(0); }
    </style>

    <script type="module">
        // Firebase config aur baqi logic same rahegi sweetie...
        // Countdown Timer Logic
        function startTimer(duration, display) {
            let timer = duration, hours, minutes, seconds;
            setInterval(() => {
                hours = parseInt(timer / 3600, 10);
                minutes = parseInt((timer % 3600) / 60, 10);
                seconds = parseInt(timer % 60, 10);

                display.textContent = (hours < 10 ? "0" + hours : hours) + ":" + 
                                     (minutes < 10 ? "0" + minutes : minutes) + ":" + 
                                     (seconds < 10 ? "0" + seconds : seconds);

                if (--timer < 0) timer = duration;
            }, 1000);
        }

        window.onload = () => {
            const twentyFourHours = 24 * 60 * 60;
            const display = document.querySelector('#timer');
            if(display) startTimer(twentyFourHours, display);
        };

        window.toggleHelp = () => document.getElementById('helpDesk').classList.toggle('open');
    </script>
</head>
<body class="pb-28">

    <div id="userUI" class="p-6">
        <div class="flex justify-between items-center mb-8">
            <h1 class="text-2xl font-black italic text-blue-500">APEX.PRO</h1>
            <a href="https://chat.whatsapp.com/your-link" target="_blank" class="bg-green-600/20 text-green-400 text-[10px] px-4 py-2 rounded-full font-bold border border-green-500/30">
                <i class="fa-brands fa-whatsapp mr-1"></i> JOIN GROUP
            </a>
        </div>

        <div class="glass p-6 rounded-[2rem] mb-8 border-l-4 border-green-500">
            <div class="flex justify-between items-center">
                <div>
                    <p class="text-[10px] text-gray-400 font-bold uppercase tracking-widest">Next Profit In</p>
                    <h2 id="timer" class="text-3xl font-black timer-glow">23:59:59</h2>
                </div>
                <i class="fa-solid fa-clock-rotate-left text-2xl text-green-500 opacity-30"></i>
            </div>
        </div>

        <div class="bg-gradient-to-br from-blue-600 to-purple-700 p-8 rounded-[2.5rem] shadow-2xl mb-10 relative overflow-hidden">
            <p class="text-[10px] uppercase font-bold opacity-80">Current Assets</p>
            <h1 class="text-6xl font-black my-2">$120.00</h1>
            <p class="text-xs opacity-70">≈ Rs. 33,600</p>
            <button onclick="toggleHelp()" class="mt-6 w-full bg-white/20 py-3 rounded-xl text-xs font-bold backdrop-blur-md">
                <i class="fa-solid fa-headset mr-2"></i> OPEN HELP DESK
            </button>
        </div>

        <h3 class="text-sm font-bold mb-4 text-gray-400 tracking-widest uppercase">Premium Plans</h3>
        <div class="space-y-4">
            <div class="glass p-6 rounded-3xl flex justify-between items-center border border-white/5">
                <div><p class="font-black text-xl">$5 Plan</p><p class="text-[10px] text-blue-400">Daily Rs. 1,400 returns</p></div>
                <button class="btn-gradient px-6 py-3 rounded-xl text-[10px] font-bold">ACTIVATE</button>
            </div>
        </div>
    </div>

    <div id="helpDesk" class="fixed inset-x-0 bottom-0 z-[200] glass rounded-t-[3rem] p-8 border-t border-blue-500/30">
        <div class="flex justify-between items-center mb-6">
            <h3 class="text-lg font-black italic text-blue-400">ADMIN HELP DESK</h3>
            <button onclick="toggleHelp()" class="text-gray-500"><i class="fa-solid fa-times"></i></button>
        </div>
        <div class="space-y-4">
            <p class="text-[10px] text-gray-500 italic">Sweetie, write your problem here and admin will reply soon.</p>
            <textarea placeholder="Write your message..." class="w-full h-32 bg-black/40 rounded-2xl p-4 text-sm outline-none border border-white/10 focus:border-blue-500"></textarea>
            <button class="w-full btn-gradient py-4 rounded-2xl font-bold text-sm">SEND MESSAGE</button>
        </div>
    </div>

    <nav class="fixed bottom-0 left-0 right-0 p-6 flex justify-around items-center glass rounded-t-[2.5rem] z-50 border-t border-white/5">
        <button class="text-blue-500 flex flex-col items-center gap-1"><i class="fa-solid fa-house text-xl"></i><span class="text-[8px] font-bold">HOME</span></button>
        <button class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-wallet text-xl"></i><span class="text-[8px] font-bold">WALLET</span></button>
        <button onclick="toggleHelp()" class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-headset text-xl"></i><span class="text-[8px] font-bold">SUPPORT</span></button>
    </nav>

</body>
</html>
