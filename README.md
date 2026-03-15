<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Recruitment - Online Test</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap');
        body { font-family: 'Poppins', sans-serif; background: #f0f2f5; }
        .glass-card { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-radius: 20px; box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.15); }
        .timer-badge { background: #e11d48; color: white; padding: 5px 15px; border-radius: 50px; font-weight: bold; }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-4xl mx-auto">
        <div class="glass-card p-6 mb-6 flex flex-col md:flex-row justify-between items-center bg-blue-900 text-white">
            <div>
                <h1 class="text-2xl font-bold uppercase tracking-wider">GB Police Recruitment 2026</h1>
                <p class="text-blue-200 text-sm">Post: Constable (BPS-07) - Mock Test</p>
            </div>
            <div class="mt-4 md:mt-0 text-center">
                <span class="text-xs block uppercase">Remaining Time</span>
                <div class="timer-badge" id="timer">90:00</div>
            </div>
        </div>

        <form id="quiz-form" class="space-y-6">
            
            <div class="glass-card p-6 border-l-8 border-blue-600">
                <p class="font-semibold text-lg mb-4">Q1: Which mountain peak is known as "Savage Mountain" and located in Gilgit-Baltistan?</p>
                <div class="space-y-3">
                    <label class="flex items-center p-3 border rounded-lg hover:bg-blue-50 cursor-pointer transition">
                        <input type="radio" name="q1" value="a" class="w-4 h-4 text-blue-600">
                        <span class="ml-3">Nanga Parbat</span>
                    </label>
                    <label class="flex items-center p-3 border rounded-lg hover:bg-blue-50 cursor-pointer transition">
                        <input type="radio" name="q1" value="b" class="w-4 h-4 text-blue-600">
                        <span class="ml-3">K2</span>
                    </label>
                    <label class="flex items-center p-3 border rounded-lg hover:bg-blue-50 cursor-pointer transition">
                        <input type="radio" name="q1" value="c" class="w-4 h-4 text-blue-600">
                        <span class="ml-3">Rakaposhi</span>
                    </label>
                </div>
            </div>

            <div class="glass-card p-6 border-l-8 border-green-600">
                <p class="font-semibold text-lg mb-4">Q2: Change into Passive Voice: "He is driving a car."</p>
                <div class="space-y-3">
                    <label class="flex items-center p-3 border rounded-lg hover:bg-blue-50 cursor-pointer transition">
                        <input type="radio" name="q2" value="a" class="w-4 h-4 text-blue-600">
                        <span class="ml-3">A car is driven by him.</span>
                    </label>
                    <label class="flex items-center p-3 border rounded-lg hover:bg-blue-50 cursor-pointer transition">
                        <input type="radio" name="q2" value="b" class="w-4 h-4 text-blue-600">
                        <span class="ml-3">A car is being driven by him.</span>
                    </label>
                </div>
            </div>

            <div class="text-center pb-10">
                <button type="submit" class="bg-blue-700 hover:bg-blue-800 text-white font-bold py-3 px-10 rounded-full shadow-lg transform transition hover:scale-105">
                    Submit Final Test
                </button>
            </div>
        </form>
    </div>

    <script>
        // Simple Timer Script
        let time = 5400; // 90 minutes
        const timerEl = document.getElementById('timer');
        setInterval(() => {
            let minutes = Math.floor(time / 60);
            let seconds = time % 60;
            timerEl.innerHTML = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
            if (time > 0) time--;
        }, 1000);
    </script>
</body>
</html>
