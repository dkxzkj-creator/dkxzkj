# dkxzkj



<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dkxz Push Controller</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --neon-green: #00ff41;
            --neon-green-dim: #00cc33;
            --neon-green-dark: #009922;
            --neon-green-glow: rgba(0, 255, 65, 0.5);
            --bg-black: #000000;
            --bg-dark: #0a0a0a;
            --bg-panel: #0d0d0d;
            --bg-input: #111111;
            --text-primary: #e0e0e0;
            --text-secondary: #a0a0a0;
            --border-color: #1a1a1a;
            --border-neon: #00ff41;
            --font-mono: 'Courier New', 'Consolas', monospace;
        }

        body {
            font-family: var(--font-mono);
            background: var(--bg-black);
            color: var(--text-primary);
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* Binary Rain Canvas */
        #binary-rain {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            opacity: 0;
            transition: opacity 2s ease;
        }

        #binary-rain.active {
            opacity: 0.15;
        }

        /* Intro Animation */
        #intro {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--bg-black);
            z-index: 1000;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
        }

        .terminal {
            width: 90%;
            max-width: 800px;
            padding: 2rem;
            background: rgba(0, 0, 0, 0.9);
            border: 1px solid var(--neon-green-dim);
            border-radius: 8px;
            box-shadow: 0 0 30px rgba(0, 255, 65, 0.1);
        }

        .terminal-header {
            display: flex;
            gap: 8px;
            margin-bottom: 1rem;
            padding-bottom: 1rem;
            border-bottom: 1px solid var(--border-color);
        }

        .terminal-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
        }

        .terminal-dot:nth-child(1) { background: #ff5f56; }
        .terminal-dot:nth-child(2) { background: #ffbd2e; }
        .terminal-dot:nth-child(3) { background: #27c93f; }

        .terminal-body {
            font-size: 1.1rem;
            line-height: 1.6;
            min-height: 120px;
        }

        .terminal-prompt {
            color: var(--neon-green);
            margin-right: 8px;
        }

        .typed-text {
            color: var(--text-primary);
        }

        .cursor {
            display: inline-block;
            width: 10px;
            height: 1.2em;
            background: var(--neon-green);
            vertical-align: text-bottom;
            animation: blink 1s infinite;
        }

        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0; }
        }

        .intro-complete {
            opacity: 0;
            transition: opacity 1s ease;
            pointer-events: none;
        }

        /* Main Interface */
        #main-interface {
            position: relative;
            z-index: 1;
            max-width: 1400px;
            margin: 0 auto;
            padding: 2rem;
            opacity: 0;
            transform: translateY(20px);
            transition: all 1s ease;
        }

        #main-interface.visible {
            opacity: 1;
            transform: translateY(0);
        }

        .header {
            text-align: center;
            margin-bottom: 3rem;
            padding: 2rem;
            background: linear-gradient(135deg, rgba(0, 255, 65, 0.05), rgba(0, 0, 0, 0.3));
            border: 1px solid var(--border-color);
            border-radius: 12px;
        }

        .header h1 {
            font-size: 2.5rem;
            color: var(--neon-green);
            text-shadow: 0 0 20px var(--neon-green-glow);
            margin-bottom: 0.5rem;
            letter-spacing: 2px;
        }

        .header p {
            color: var(--text-secondary);
            font-size: 0.95rem;
        }

        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }

        .panel {
            background: var(--bg-panel);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 1.5rem;
            transition: all 0.3s ease;
        }

        .panel:hover {
            border-color: var(--neon-green-dim);
            box-shadow: 0 0 20px rgba(0, 255, 65, 0.1);
        }

        .panel-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 1.5rem;
            padding-bottom: 1rem;
            border-bottom: 1px solid var(--border-color);
        }

        .panel-icon {
            width: 32px;
            height: 32px;
            background: rgba(0, 255, 65, 0.1);
            border: 1px solid var(--neon-green-dim);
            border-radius: 6px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--neon-green);
            font-size: 1.2rem;
        }

        .panel-title {
            font-size: 1.1rem;
            color: var(--neon-green);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        /* Form Elements */
        .form-group {
            margin-bottom: 1.5rem;
        }

        .form-label {
            display: block;
            margin-bottom: 0.5rem;
            color: var(--text-secondary);
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .form-input,
        .form-textarea,
        .form-select {
            width: 100%;
            padding: 0.75rem;
            background: var(--bg-input);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            color: var(--text-primary);
            font-family: var(--font-mono);
            font-size: 0.95rem;
            transition: all 0.3s ease;
        }

        .form-input:focus,
        .form-textarea:focus,
        .form-select:focus {
            outline: none;
            border-color: var(--neon-green);
            box-shadow: 0 0 10px rgba(0, 255, 65, 0.2);
        }

        .form-textarea {
            resize: vertical;
            min-height: 80px;
        }

        /* Buttons */
        .btn {
            padding: 0.75rem 1.5rem;
            background: transparent;
            border: 1px solid var(--neon-green-dim);
            border-radius: 6px;
            color: var(--neon-green);
            font-family: var(--font-mono);
            font-size: 0.95rem;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .btn:hover {
            background: rgba(0, 255, 65, 0.1);
            box-shadow: 0 0 15px rgba(0, 255, 65, 0.3);
        }

        .btn-primary {
            background: var(--neon-green);
            color: var(--bg-black);
            font-weight: bold;
        }

        .btn-primary:hover {
            background: var(--neon-green-dim);
            box-shadow: 0 0 20px rgba(0, 255, 65, 0.5);
        }

        .btn-small {
            padding: 0.5rem 1rem;
            font-size: 0.85rem;
        }

        .btn-group {
            display: flex;
            gap: 0.75rem;
            flex-wrap: wrap;
        }

        /* Preview */
        .preview-container {
            background: var(--bg-input);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 1rem;
            margin-top: 1rem;
        }

        .preview-label {
            font-size: 0.8rem;
            color: var(--text-secondary);
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 0.75rem;
        }

        .notification-preview {
            background: var(--bg-dark);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 1rem;
            transform-origin: top left;
            transition: transform 0.3s ease;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
        }

        .notification-preview .notif-title {
            font-weight: bold;
            color: var(--neon-green);
            margin-bottom: 0.5rem;
            font-size: 1rem;
        }

        .notification-preview .notif-body {
            color: var(--text-primary);
            font-size: 0.9rem;
            line-height: 1.4;
        }

        .notification-preview .notif-icon {
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, var(--neon-green), var(--neon-green-dark));
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            margin-bottom: 0.75rem;
        }

        /* Size Controls */
        .size-controls {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 0.5rem;
            margin-bottom: 1rem;
        }

        .size-btn {
            padding: 0.5rem;
            background: var(--bg-input);
            border: 1px solid var(--border-color);
            border-radius: 4px;
            color: var(--text-secondary);
            font-family: var(--font-mono);
            font-size: 0.85rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .size-btn:hover {
            border-color: var(--neon-green-dim);
            color: var(--neon-green);
        }

        .size-btn.active {
            background: rgba(0, 255, 65, 0.1);
            border-color: var(--neon-green);
            color: var(--neon-green);
        }

        /* Sound Library */
        .sound-category {
            margin-bottom: 1.5rem;
        }

        .sound-category-title {
            font-size: 0.85rem;
            color: var(--neon-green);
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 0.75rem;
            padding-bottom: 0.5rem;
            border-bottom: 1px solid var(--border-color);
        }

        .sound-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: 0.75rem;
        }

        .sound-item {
            background: var(--bg-input);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 0.75rem;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
        }

        .sound-item:hover {
            border-color: var(--neon-green-dim);
            transform: translateY(-2px);
        }

        .sound-item.selected {
            background: rgba(0, 255, 65, 0.1);
            border-color: var(--neon-green);
        }

        .sound-item.selected::after {
            content: '✓';
            position: absolute;
            top: 5px;
            right: 5px;
            color: var(--neon-green);
            font-size: 0.9rem;
        }

        .sound-name {
            font-size: 0.85rem;
            color: var(--text-primary);
            margin-bottom: 0.5rem;
        }

        .sound-play-btn {
            width: 100%;
            padding: 0.4rem;
            background: rgba(0, 255, 65, 0.1);
            border: 1px solid var(--neon-green-dim);
            border-radius: 4px;
            color: var(--neon-green);
            font-family: var(--font-mono);
            font-size: 0.75rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .sound-play-btn:hover {
            background: rgba(0, 255, 65, 0.2);
        }

        /* Volume Control */
        .volume-control {
            margin-top: 1rem;
        }

        .volume-slider {
            width: 100%;
            height: 6px;
            background: var(--bg-input);
            border-radius: 3px;
            outline: none;
            -webkit-appearance: none;
            appearance: none;
        }

        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 18px;
            height: 18px;
            background: var(--neon-green);
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 0 10px var(--neon-green-glow);
        }

        .volume-slider::-moz-range-thumb {
            width: 18px;
            height: 18px;
            background: var(--neon-green);
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 0 10px var(--neon-green-glow);
        }

        .volume-display {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 0.5rem;
        }

        .volume-value {
            color: var(--neon-green);
            font-size: 1.1rem;
            font-weight: bold;
        }

        /* Device Management */
        .device-list {
            max-height: 300px;
            overflow-y: auto;
            margin-bottom: 1rem;
        }

        .device-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0.75rem;
                       background: var(--bg-input);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            margin-bottom: 0.5rem;
            transition: all 0.3s ease;
        }

        .device-item:hover {
            border-color: var(--neon-green-dim);
        }

        .device-item.selected-device {
            background: rgba(0, 255, 65, 0.1);
            border-color: var(--neon-green);
        }

        .device-info {
            flex: 1;
        }

        .device-name {
            color: var(--neon-green);
            font-weight: bold;
            margin-bottom: 0.25rem;
        }

        .device-meta {
            font-size: 0.75rem;
            color: var(--text-secondary);
        }

        .device-actions {
            display: flex;
            gap: 0.5rem;
        }

        .device-btn {
            padding: 0.4rem 0.6rem;
            font-size: 0.75rem;
        }

        /* Status Bar */
        .status-bar {
            margin-top: 1rem;
            padding: 0.75rem;
            background: rgba(0, 0, 0, 0.5);
            border-left: 3px solid var(--neon-green);
            font-size: 0.85rem;
            color: var(--text-secondary);
            border-radius: 4px;
            min-height: 40px;
            display: flex;
            align-items: center;
        }

        .status-bar .status-icon {
            margin-right: 10px;
            color: var(--neon-green);
        }

        /* Responsive */
        @media (max-width: 768px) {
            .dashboard {
                grid-template-columns: 1fr;
            }
            
            .header h1 {
                font-size: 1.8rem;
            }

            .size-controls {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        /* Scrollbar styling */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: var(--bg-dark);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--neon-green-dark);
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: var(--neon-green);
        }
    </style>
</head>
<body>

    <!-- Background Layer -->
    <canvas id="binary-rain"></canvas>

    <!-- Intro Animation Layer -->
    <div id="intro">
        <div class="terminal">
            <div class="terminal-header">
                <div class="terminal-dot"></div>
                <div class="terminal-dot"></div>
                <div class="terminal-dot"></div>
            </div>
            <div class="terminal-body">
                <span class="terminal-prompt">dkxz@core:~$</span>
                <span class="typed-text" id="typed-text"></span><span class="cursor" id="intro-cursor"></span>
            </div>
        </div>
    </div>

    <!-- Main Application Interface -->
    <div id="main-interface">
        <header class="header">
            <h1>DKXZ PUSH CONTROLLER</h1>
            <p>Sistema de Orquestração de Notificações Web em Tempo Real</p>
        </header>

        <div class="dashboard">
            <!-- Column 1: Customization & Preview -->
            <section class="panel">
                <div class="panel-header">
                    <div class="panel-icon">✍️</div>
                    <h2 class="panel-title">Personalização</h2>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Título da Notificação</label>
                    <input type="text" id="notif-title" class="form-input" placeholder="Alerta de Sistema" value="Dkxz Alert">
                </div>

                <div class="form-group">
                    <label class="form-label">Conteúdo da Mensagem</label>
                    <textarea id="notif-body" class="form-textarea" placeholder="Descreva a mensagem do push...">O sistema detectou uma atividade crítica no perímetro digital.</textarea>
                </div>

                <div class="form-group">
                    <label class="form-label">Tamanho da Notificação</label>
                    <div class="size-controls" id="size-controls">
                        <!-- Generated by JS -->
                    </div>
                </div>

                <div class="preview-container">
                    <div class="preview-label">Pré-visualização em Tempo Real</div>
                    <div class="notification-preview" id="notif-preview" style="transform: scale(1);">
                        <div class="notif-icon">📡</div>
                        <div class="notif-title" id="preview-title">Dkxz Alert</div>
                        <div class="notif-body" id="preview-body">O sistema detectou uma atividade crítica no perímetro digital.</div>
                    </div>
                </div>

                <div class="btn-group" style="margin-top: 1rem;">
                    <button class="btn btn-primary" id="btn-test-local">Testar Localmente</button>
                    <button class="btn" id="btn-request-permission">Solicitar Permissão</button>
                </div>
            </section>

            <!-- Column 2: Audio Engine -->
            <section class="panel">
                <div class="panel-header">
                    <div class="panel-icon">🔊</div>
                    <h2 class="panel-title">Motor de Áudio</h2>
                </div>

                <div class="volume-control">
                    <label class="form-label">Volume Global</label>
                    <input type="range" min="0" max="100" value="70" class="volume-slider" id="volume-slider">
                    <div class="volume-display">
                        <span>Nível</span>
                        <span class="volume-value" id="volume-value">70%</span>
                    </div>
                </div>

                <div style="margin-top: 1.5rem;">
                    <label class="form-label">Biblioteca de Sons Sintetizados</label>
                    <div id="sound-library">
                        <!-- Categories generated by JS -->
                    </div>
                </div>
            </section>

            <!-- Column 3: Device Orchestration -->
            <section class="panel">
                <div class="panel-header">
                    <div class="panel-icon">📡</div>
                    <h2 class="panel-title">Dispositivos Autorizados</h2>
                </div>

                <div class="form-group">
                    <label class="form-label">Registrar Novo Dispositivo (Voluntário)</label>
                    <input type="text" id="new-device-name" class="form-input" placeholder="Ex: iPhone da Ana, Tablet da Ops...">
                    <button class="btn btn-small" id="btn-add-device" style="margin-top: 0.5rem; width: 100%;">+ Registrar Dispositivo</button>
                </div>

                <div class="form-group">
                    <label class="form-label">Dispositivos na Rede</label>
                    <div class="device-list" id="device-list">
                        <!-- Devices rendered here -->
                    </div>
                </div>

                <div class="status-bar" id="status-bar">
                    <span class="status-icon">ℹ️</span>
                    <span id="status-text">Sistema iniciado. Aguardando comandos.</span>
                </div>
            </section>
        </div>
    </div>

    <script>
        // --- 1. BINARY RAIN BACKGROUND ---
        const canvas = document.getElementById('binary-rain');
        const ctx = canvas.getContext('2d');
        
        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        const chars = '01';
        const fontSize = 16;
        let columns = Math.floor(canvas.width / fontSize);
        let drops = Array(columns).fill(1);

        function drawBinaryRain() {
            ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = '#00ff41';
            
            for (let i = 0; i < drops.length; i++) {
                const text = chars[Math.floor(Math.random() * chars.length)];
                ctx.fillText(text, i * fontSize, drops[i] * fontSize);
                
                if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                drops[i]++;
            }
        }
        setInterval(drawBinaryRain, 50);

        // --- 2. INTRO SEQUENCE ---
        const introText = "Dkxz push controller";
        const typedTextEl = document.getElementById('typed-text');
        const introContainer = document.getElementById('intro');
        const mainInterface = document.getElementById('main-interface');
        const binaryCanvas = document.getElementById('binary-rain');
        
        // Start rain after 1 second
        setTimeout(() => binaryCanvas.classList.add('active'), 1000);

        let charIndex = 0;
        function typeWriter() {
            if (charIndex < introText.length) {
                typedTextEl.innerHTML += introText.charAt(charIndex);
                charIndex++;
                setTimeout(typeWriter, 100 + Math.random() * 100); // Random typing speed
            } else {
                // Hide cursor, wait, then transition
                document.getElementById('intro-cursor').style.display = 'none';
                setTimeout(() => {
                    introContainer.classList.add('intro-complete');
                    setTimeout(() => {
                        introContainer.style.display = 'none';
                        mainInterface.classList.add('visible');
                    }, 1000);
                }, 1500);
            }
        }
        setTimeout(typeWriter, 2500); // Start typing after rain begins


        // --- 3. AUDIO ENGINE (Web Audio API) ---
        let audioCtx = null;
        let masterGain = null;
        let currentVolume = 0.7;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                masterGain = audioCtx.createGain();
                masterGain.gain.value = currentVolume;
                masterGain.connect(audioCtx.destination);
            }
            if (audioCtx.state === 'suspended') audioCtx.resume();
        }

        // Synthesize sounds based on frequency patterns
        const soundLibrary = {
            "Tecnológicos": [
                { id: 'tech_1', name: 'Laser Ping', type: 'sine', freq: 800, decay: 0.1, slide: 200 },
                { id: 'tech_2', name: 'Data Stream', type: 'square', freq: 400, decay: 0.5, steps: 4, stepInterval: 0.1 },
                { id: 'tech_3', name: 'Hologram', type: 'sine', freq: 1200, decay: 0.3, vibrato: 5 }
            ],
            "Alertas Digitais": [
                { id: 'alert_1', name: 'Critical Warning', type: 'sawtooth', freq: 220, decay: 0.8, repeat: 3, interval: 0.2 },
                { id: 'alert_2', name: 'Error Buzz', type: 'square', freq: 150, decay: 0.2, repeat: 4, interval: 0.1 }
            ],
            "Sons Discretos": [
                { id: 'soft_1', name: 'Soft Chime', type: 'sine', freq: 600, decay: 0.5 },
                { id: 'soft_2', name: 'Muted Tap', type: 'triangle', freq: 300, decay: 0.1 }
            ],
            "Alertas Intensos": [
                { id: 'loud_1', name: 'Siren Pulse', type: 'sawtooth', freq: 440, decay: 1.0, slide: -100, repeat: 2 },
                { id: 'loud_2', name: 'Explosion', type: 'noise', duration: 0.5, decay: 0.5 }
            ],
            "Futuristas": [
                { id: 'fut_1', name: 'Subspace', type: 'sine', freq: 100, decay: 2.0, slide: 400 },
                { id: 'fut_2', name: 'Cyber Glitch', type: 'square', freq: 800, decay: 0.1, steps: 8, stepInterval: 0.05, random: true }
            ]
        };

        let selectedSoundId = 'tech_1';

        function playSound(id) {
            initAudio();
            
            // Find sound config
            let config = null;
            for (const cat in soundLibrary) {
                const found = soundLibrary[cat].find(s => s.id === id);
                if (found) config = found;
            }
            if (!config) return;

            const now = audioCtx.currentTime;
            
            // Create main oscillator
            if (config.type !== 'noise') {
                const osc = audioCtx.createOscillator();
                const gainNode = audioCtx.createGain();
                
                osc.type = config.type;
                osc.frequency.setValueAtTime(config.freq, now);
                
                if (config.slide) {
                    osc.frequency.linearRampToValueAtTime(config.freq + config.slide, now + config.decay);
                }
                
                if (config.vibrato) {
                    const lfo = audioCtx.createOscillator();
                    const lfoGain = audioCtx.createGain();
                    lfo.frequency.value = 5;
                    lfoGain.gain.value = config.vibrato;
                    lfo.connect(lfoGain);
                    lfoGain.connect(osc.frequency);
                    lfo.start(now);
                    lfo.stop(now + config.decay);
                }

                gainNode.gain.setValueAtTime(1, now);
                gainNode.gain.exponentialRampToValueAtTime(0.001, now + config.decay);
                
                osc.connect(gainNode);
                gainNode.connect(masterGain);
                
                // Handle repeats/steps
                if (config.repeat) {
                    for(let i=1; i<config.repeat; i++) {
                        const oscRepeat = audioCtx.createOscillator();
                        const gainRepeat = audioCtx.createGain();
                        oscRepeat.type = config.type;
                        oscRepeat.frequency.value = config.freq;
                        gainRepeat.gain.setValueAtTime(1, now + (i * config.interval));
                        gainRepeat.gain.exponentialRampToValueAtTime(0.001, now + (i * config.interval) + config.decay);
                        oscRepeat.connect(gainRepeat);
                        gainRepeat.connect(masterGain);
                        oscRepeat.start(now + (i * config.interval));
                        oscRepeat.stop(now + (i * config.interval) + config.decay);
                    }
                }
                
                osc.start(now);
                osc.stop(now + config.decay);
            } else {
                // Noise generation
                const bufferSize = audioCtx.sampleRate * config.duration;
                const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
                const data = buffer.getChannelData(0);
                for (let i = 0; i < data.length; i++) {
                    data[i] = Math.random() * 2 - 1;
                }
                const noiseSource = audioCtx.createBufferSource();
                noiseSource.buffer = buffer;
                const gainNode = audioCtx.createGain();
                gainNode.gain.setValueAtTime(1, now);
                gainNode.gain.exponentialRampToValueAtTime(0.001, now + config.decay);
                noiseSource.connect(gainNode);
                gainNode.connect(masterGain);
                noiseSource.start(now);
                noiseSource.stop(now + config.decay);
            }
        }

        // Render Sound Library UI
        const soundLibraryContainer = document.getElementById('sound-library');
        for (const [category, sounds] of Object.entries(soundLibrary)) {
            const catDiv = document.createElement('div');
            catDiv.className = 'sound-category';
            catDiv.innerHTML = `<div class="sound-category-title">${category}</div><div class="sound-grid"></div>`;
            const grid = catDiv.querySelector('.sound-grid');
            
            sounds.forEach(sound => {
                const item = document.createElement('div');
                item.className = `sound-item ${sound.id === selectedSoundId ? 'selected' : ''}`;
                item.dataset.id = sound.id;
                item.innerHTML = `                    <div class="sound-name">${sound.name}</div>
                    <button class="sound-play-btn" data-id="${sound.id}">▶ Testar</button>
                `;
                grid.appendChild(item);
            });
            soundLibraryContainer.appendChild(catDiv);
        }

        // Sound Interaction (Event Delegation)
        soundLibraryContainer.addEventListener('click', (e) => {
            const target = e.target;
            if (target.classList.contains('sound-play-btn')) {
                playSound(target.dataset.id);
            } else if (target.classList.contains('sound-item')) {
                // Select sound
                document.querySelectorAll('.sound-item').forEach(el => el.classList.remove('selected'));
                target.classList.add('selected');
                selectedSoundId = target.dataset.id;
                setStatus(`Som selecionado: ${target.querySelector('.sound-name').textContent}`);
            }
        });

        // Volume Control
        const volumeSlider = document.getElementById('volume-slider');
        const volumeValue = document.getElementById('volume-value');
        volumeSlider.addEventListener('input', (e) => {
            const val = e.target.value;
            volumeValue.textContent = `${val}%`;
            currentVolume = val / 100;
            if (masterGain) {
                masterGain.gain.setTargetAtTime(currentVolume, audioCtx.currentTime, 0.05);
            }
        });

        // --- 4. NOTIFICATION PREVIEW & SIZING ---
        const sizeControlsContainer = document.getElementById('size-controls');
        const notifPreview = document.getElementById('notif-preview');
        const titleInput = document.getElementById('notif-title');
        const bodyInput = document.getElementById('notif-body');
        const previewTitle = document.getElementById('preview-title');
        const previewBody = document.getElementById('preview-body');
        let currentScale = 1;

        // Generate Size Buttons
        for (let i = 1; i <= 10; i++) {
            const btn = document.createElement('button');
            btn.className = 'size-btn';
            btn.textContent = `${i * 10}%`;
            btn.dataset.scale = i * 0.1;
            if (i === 10) btn.classList.add('active');
            
            btn.addEventListener('click', () => {
                document.querySelectorAll('.size-btn').forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                currentScale = parseFloat(btn.dataset.scale);
                notifPreview.style.transform = `scale(${currentScale})`;
                // Adjust container to prevent overflow issues visually
                notifPreview.parentElement.style.height = `${currentScale * 150 + 20}px`;
            });
            sizeControlsContainer.appendChild(btn);
        }

        // Live Preview Update
        titleInput.addEventListener('input', (e) => previewTitle.textContent = e.target.value || 'Título');
        bodyInput.addEventListener('input', (e) => previewBody.textContent = e.target.value || 'Mensagem');

        // --- 5. PUSH NOTIFICATION SYSTEM ---
        
        // Request Permission
        document.getElementById('btn-request-permission').addEventListener('click', () => {
            Notification.requestPermission().then(permission => {
                if (permission === 'granted') {
                    setStatus("Permissão de notificações concedida. Dispositivo local registrado.");
                    addDevice("Dispositivo Local (Este Navegador)");
                } else {
                    setStatus("Permissão negada. Não será possível enviar notificações push.");
                }
            });
        });

        // Test Local Notification
        document.getElementById('btn-test-local').addEventListener('click', () => {
            if (Notification.permission !== 'granted') {
                alert("Solicite a permissão de notificação antes de testar.");
                return;
            }
            
            const title = titleInput.value;
            const body = bodyInput.value;
            
            // Play selected sound if volume > 0
            if (currentVolume > 0) {
                playSound(selectedSoundId);
            }

            const notification = new Notification(title, {
                body: body,
                icon: generateIconDataURI(), // Generates a dynamic icon
                badge: generateIconDataURI(),
                tag: 'dkxz-main',
                dir: 'auto',
                lang: 'pt-BR',
                data: { url: window.location.href }
            });

            notification.onclick = function() {
                window.open(this.data.url);
                this.close();
            };

            setStatus(`Notificação local enviada: "${title}"`);
        });

        // Helper: Generate a simple data URI icon for the notification
        function generateIconDataURI() {
            const canvas = document.createElement('canvas');
            canvas.width = 64;
            canvas.height = 64;
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = '#000000';
            ctx.fillRect(0, 0, 64, 64);
            ctx.fillStyle = '#00ff41';
            ctx.font = 'bold 40px monospace';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText('D', 32, 32);
            return canvas.toDataURL();
        }

        // --- 6. DEVICE ORCHESTRATION (SIMULATED MULTI-DEVICE) ---
        // In a real production environment, this list would be populated by a backend 
        // service (e.g., Firebase Cloud Messaging or a custom WebSocket server) where 
        // devices register via their unique Push Subscription ID. 
        // Here, we simulate the "authorized" registry for the UI.

        let devices = JSON.parse(localStorage.getItem('dkxz_devices') || '[]');
        const deviceListEl = document.getElementById('device-list');
        const newDeviceInput = document.getElementById('new-device-name');

        function renderDevices() {
            deviceListEl.innerHTML = '';
            if (devices.length === 0) {
                deviceListEl.innerHTML = '<div style="padding: 1rem; text-align: center; color: var(--text-secondary);">Nenhum dispositivo registrado.</div>';
                return;
            }

            devices.forEach((device, index) => {
                const item = document.createElement('div');
                item.className = 'device-item';
                item.innerHTML = `
                    <div class="device-info">
                        <div class="device-name">${device.name}</div>
                        <div class="device-meta">ID: ${device.id} | Status: Online</div>
                    </div>
                    <div class="device-actions">
                        <button class="btn btn-small device-btn btn-send" data-index="${index}">Enviar</button>
                        <button class="btn btn-small device-btn btn-remove" data-index="${index}">✕</button>
                    </div>
                `;
                deviceListEl.appendChild(item);
            });
        }

        function addDevice(name) {
            if (!name.trim()) return;
            const device = {
                id: 'dev_' + Date.now().toString(36),
                name: name.trim(),
                addedAt: new Date().toISOString()
            };
            devices.push(device);
            localStorage.setItem('dkxz_devices', JSON.stringify(devices));
            newDeviceInput.value = '';
            renderDevices();
            setStatus(`Dispositivo "${name}" registrado na rede.`);
        }

        document.getElementById('btn-add-device').addEventListener('click', () => {
            addDevice(newDeviceInput.value);
        });

        // Device Actions (Send/Remove)
        deviceListEl.addEventListener('click', (e) => {
            const target = e.target;
            if (target.classList.contains('btn-send')) {
                const index = parseInt(target.dataset.index);
                const device = devices[index];
                
                // Simulate sending to a specific device ID
                if (Notification.permission === 'granted') {
                    // In a real scenario, this would trigger a server-side FCM/Web Push send
                    // For local demo, we trigger the local notification again with device context
                    const title = titleInput.value;
                    const body = bodyInput.value;
                    
                    playSound(selectedSoundId);
                    
                    new Notification(`[${device.name}] ${title}`, {
                        body: body,
                        icon: generateIconDataURI()
                    });
                    
                    setStatus(`Push enviado para: ${device.name}`);
                } else {
                    setStatus("Aviso: Permissão local não concedida. Simulação de envio para rede externa.");
                    setStatus(`Comando de envio gerado para ${device.id}.`);
                }
            } else if (target.classList.contains('btn-remove')) {
                const index = parseInt(target.dataset.index);
                const removedName = devices[index].name;
                devices.splice(index, 1);
                localStorage.setItem('dkxz_devices', JSON.stringify(devices));
                renderDevices();
                setStatus(`Dispositivo "${removedName}" removido.`);
            }
        });

        // --- 7. STATUS BAR HELPER ---
        const statusText = document.getElementById('status-text');
        const statusBar = document.getElementById('status-bar');
        
        function setStatus(message) {
            statusText.textContent = message;
            statusBar.style.borderLeftColor = '#00ff41';
            statusBar.style.animation = 'pulse 0.3s';
            setTimeout(() => {
                statusBar.style.animation = 'none';
            }, 300);
        }

        // Initial Render
        renderDevices();

    </script>
</body>
</html>
