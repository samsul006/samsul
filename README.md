<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BST Engineering Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;500&family=Kanit:wght@200;400;600&display=swap');

        :root {
            --neon-blue: #00f2ff;
            --dark-panel: #0a0f18;
            --grid-color: rgba(0, 242, 255, 0.05);
        }

        body {
            background-color: #05070a;
            background-image: 
                linear-gradient(var(--grid-color) 1px, transparent 1px),
                linear-gradient(90deg, var(--grid-color) 1px, transparent 1px);
            background-size: 30px 30px;
            font-family: 'Kanit', sans-serif;
            color: #e2e8f0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: screen;
        }

        .eng-panel {
            background: rgba(10, 15, 24, 0.9);
            border: 1px solid rgba(0, 242, 255, 0.3);
            box-shadow: 0 0 20px rgba(0, 242, 255, 0.1), inset 0 0 10px rgba(0, 242, 255, 0.05);
            position: relative;
            overflow: hidden;
        }

        /* มุมเหลี่ยมแบบวิศวกรรม */
        .eng-panel::before {
            content: "";
            position: absolute;
            top: 0; right: 0;
            border-style: solid;
            border-width: 0 40px 40px 0;
            border-color: transparent #1e293b transparent transparent;
        }

        .mono { font-family: 'JetBrains Mono', monospace; }

        .input-eng {
            background: rgba(15, 23, 42, 0.8);
            border-bottom: 2px solid #1e293b;
            transition: all 0.3s;
        }

        .input-eng:focus {
            border-bottom: 2px solid var(--neon-blue);
            box-shadow: 0 5px 15px rgba(0, 242, 255, 0.1);
            outline: none;
        }

        .btn-eng {
            position: relative;
            background: transparent;
            border: 1px solid rgba(0, 242, 255, 0.5);
            color: var(--neon-blue);
            text-transform: uppercase;
            letter-spacing: 1px;
            overflow: hidden;
            transition: 0.3s;
        }

        .btn-eng:hover {
            background: var(--neon-blue);
            color: #000;
            box-shadow: 0 0 15px var(--neon-blue);
        }

        .terminal-box {
            background: #000;
            border: 1px solid #1e293b;
            position: relative;
        }

        .terminal-box::after {
            content: "SYSTEM_READY";
            position: absolute;
            bottom: -18px; right: 0;
            font-size: 10px;
            color: var(--neon-blue);
            opacity: 0.5;
        }

        .scanline {
            width: 100%; height: 2px;
            background: rgba(0, 242, 255, 0.1);
            position: absolute;
            animation: scan 4s linear infinite;
        }

        @keyframes scan {
            0% { top: 0; }
            100% { top: 100%; }
        }
    </style>
</head>
<body class="p-6">

    <div class="eng-panel w-full max-w-lg rounded-sm p-8 border-l-4 border-l-[#00f2ff]">
        <div class="scanline"></div>
        
        <div class="mb-10">
            <div class="flex items-center gap-2 mb-2 text-[10px] mono text-cyan-500/60">
                <span class="animate-pulse">●</span> CORE_PROCESSOR_V1 // BST_ALGORITHM
            </div>
            <h1 class="text-3xl font-black tracking-tighter text-white">
                DATABASE <span class="text-[#00f2ff]">TREE</span> STRUCTURE
            </h1>
            <div class="h-1 w-20 bg-[#00f2ff] mt-2"></div>
        </div>

        <div class="space-y-6">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div class="flex flex-col">
                    <label class="text-[10px] mono mb-1 text-gray-500">PRIMARY_KEY</label>
                    <input type="text" id="val" placeholder="0x000" class="input-eng p-3 text-white mono">
                </div>
                <div class="flex flex-col">
                    <label class="text-[10px] mono mb-1 text-gray-500">UPDATE_VAL</label>
                    <input type="text" id="nVal" placeholder="NULL" class="input-eng p-3 text-white mono">
                </div>
            </div>

            <div class="grid grid-cols-2 md:grid-cols-4 gap-2">
                <button onclick="cmd('INSERT')" class="btn-eng py-2 text-xs font-bold">Create</button>
                <button onclick="cmd('FETCH')" class="btn-eng py-2 text-xs font-bold">Read</button>
                <button onclick="cmd('PATCH')" class="btn-eng py-2 text-xs font-bold">Update</button>
                <button onclick="cmd('PURGE')" class="btn-eng py-2 text-xs font-bold">Delete</button>
            </div>
        </div>

        <div class="mt-8">
            <div class="terminal-box h-40 overflow-y-auto p-4 mono text-xs leading-relaxed text-cyan-200">
                <div id="output">
                    <p class="opacity-50">[INFO] อุปกรณ์พร้อมใช้งาน...</p>
                    <p class="opacity-50">[INFO] กำลังรอรับพารามิเตอร์ผ่านอินพุต</p>
                </div>
            </div>
        </div>

        <div class="mt-6 flex justify-between items-center border-t border-gray-800 pt-4">
            <div class="flex gap-4">
                <div class="text-[9px] mono">CPU: <span class="text-green-500">2.4%</span></div>
                <div class="text-[9px] mono">LATENCY: <span class="text-cyan-500">12ms</span></div>
            </div>
            <div class="text-[9px] mono text-gray-600">© 2026 ENG_DEPT</div>
        </div>
    </div>

    <script>
        function cmd(type) {
            const out = document.getElementById('output');
            const v = document.getElementById('val').value || 'NaN';
            const timestamp = new Date().toLocaleTimeString();
            
            const line = document.createElement('div');
            line.innerHTML = `<span class="text-gray-600">[${timestamp}]</span> <span class="text-yellow-500">CMD::</span><span class="text-white">${type}</span>_SUCCESS <span class="text-cyan-500">#VAL:${v}</span>`;
            
            out.appendChild(line);
            out.scrollTop = out.scrollHeight;
        }
    </script>
</body>
</html>
