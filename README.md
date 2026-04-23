<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Ball Sort 2 | 1500 Levels Puzzle Game</title>
    
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@300;400;500;600;700&family=Bungee+Inline&display=swap" rel="stylesheet">
    
    <!-- Confetti Library -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Quicksand', sans-serif;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* Game Container */
        .game-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 15px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        /* Header */
        .game-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(255,255,255,0.15);
            backdrop-filter: blur(10px);
            padding: 12px 20px;
            border-radius: 20px;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
            animation: slideDown 0.5s ease;
        }

        .level-info {
            background: rgba(0,0,0,0.3);
            padding: 8px 18px;
            border-radius: 30px;
            display: flex;
            gap: 20px;
        }

        .level-number, .moves-count {
            font-size: 1rem;
            font-weight: bold;
            color: white;
        }

        .moves-count {
            color: #ffd700;
        }

        .btn-group {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .btn {
            background: rgba(255,255,255,0.2);
            border: none;
            padding: 8px 18px;
            border-radius: 25px;
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            font-size: 0.9rem;
        }

        .btn:active {
            transform: scale(0.95);
        }

        .btn:hover {
            background: rgba(255,255,255,0.3);
            transform: translateY(-2px);
        }

        .btn-primary {
            background: #4CAF50;
        }

        .btn-primary:hover {
            background: #45a049;
        }

        .btn-warning {
            background: #ff9800;
        }

        .btn-warning:hover {
            background: #f57c00;
        }

        /* Volume Slider */
        .volume-control {
            display: flex;
            align-items: center;
            gap: 8px;
            background: rgba(0,0,0,0.3);
            padding: 5px 12px;
            border-radius: 25px;
        }

        .volume-control span {
            color: white;
            font-size: 0.9rem;
        }

        .volume-slider {
            width: 80px;
            height: 4px;
            -webkit-appearance: none;
            background: rgba(255,255,255,0.3);
            border-radius: 5px;
            outline: none;
        }

        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 15px;
            height: 15px;
            border-radius: 50%;
            background: #ffd700;
            cursor: pointer;
        }

        /* Tubes Grid */
        .tubes-grid {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 15px;
            padding: 20px;
            flex: 1;
            align-items: center;
            animation: fadeInUp 0.6s ease;
        }

        /* Tube */
        .tube {
            width: 85px;
            height: 280px;
            background: linear-gradient(180deg, rgba(255,255,255,0.2) 0%, rgba(255,255,255,0.1) 100%);
            border: 3px solid rgba(255,255,255,0.4);
            border-radius: 0 0 25px 25px;
            border-top: none;
            display: flex;
            flex-direction: column-reverse;
            align-items: center;
            padding: 8px;
            gap: 6px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
        }

        .tube:hover {
            transform: translateY(-5px);
            background: rgba(255,255,255,0.25);
        }

        .tube.selected {
            background: rgba(255,255,255,0.35);
            transform: scale(1.03);
            border-color: #ffd700;
            box-shadow: 0 0 20px rgba(255,215,0,0.5);
            animation: pulse 0.5s ease;
        }

        .tube-ball {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            transition: all 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
            box-shadow: inset -5px -5px 10px rgba(0,0,0,0.3), inset 5px 5px 10px rgba(255,255,255,0.3), 0 5px 15px rgba(0,0,0,0.2);
            animation: bounceIn 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }

        .tube-ball.floating {
            transform: translateY(-15px) scale(1.15);
            box-shadow: 0 15px 30px rgba(0,0,0,0.3);
            filter: brightness(1.1);
        }

        .empty-tube {
            width: 60px;
            height: 60px;
            border: 2px dashed rgba(255,255,255,0.3);
            border-radius: 50%;
            transition: all 0.3s ease;
        }

        /* Loading Screen */
        .loading-screen {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            animation: fadeIn 0.5s ease;
        }

        .loading-title {
            font-family: 'Bungee Inline', cursive;
            font-size: 3rem;
            color: white;
            margin-bottom: 30px;
            animation: pulse 1.5s infinite;
        }

        .loading-bar {
            width: 300px;
            height: 20px;
            background: rgba(255,255,255,0.3);
            border-radius: 10px;
            overflow: hidden;
        }

        .loading-progress {
            height: 100%;
            background: linear-gradient(90deg, #ffd700, #ff9800);
            width: 0%;
            transition: width 0.1s linear;
            border-radius: 10px;
        }

        /* Start Screen */
        .start-screen {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            animation: fadeIn 0.8s ease;
        }

        .game-title {
            font-family: 'Bungee Inline', cursive;
            font-size: 3.5rem;
            color: #ffd700;
            text-shadow: 3px 3px 0 #ff9800, 6px 6px 0 rgba(0,0,0,0.3);
            margin-bottom: 40px;
            text-align: center;
            animation: slideDown 0.6s ease;
        }

        .ball-preview {
            display: flex;
            gap: 15px;
            margin-bottom: 40px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .color-ball {
            width: 55px;
            height: 55px;
            border-radius: 50%;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
            animation: float 2s ease-in-out infinite;
        }

        .stats {
            background: rgba(255,255,255,0.2);
            padding: 12px 25px;
            border-radius: 30px;
            margin-bottom: 30px;
            color: white;
            font-weight: bold;
            font-size: 1.1rem;
            animation: fadeInUp 0.6s ease;
        }

        .play-btn {
            background: linear-gradient(135deg, #ffd700, #ff9800);
            border: none;
            padding: 15px 50px;
            font-size: 1.5rem;
            font-weight: bold;
            color: white;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
            animation: fadeInUp 0.6s ease;
        }

        .play-btn:active {
            transform: scale(0.95);
        }

        .play-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(0,0,0,0.4);
        }

        .sound-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255,255,255,0.2);
            border: none;
            font-size: 1.5rem;
            padding: 10px;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .sound-btn:hover {
            transform: scale(1.1);
            background: rgba(255,255,255,0.3);
        }

        /* Levels Screen */
        .levels-screen {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: #1a1a2e;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
            animation: fadeIn 0.5s ease;
        }

        .levels-header {
            background: rgba(0,0,0,0.5);
            padding: 15px 20px;
            display: flex;
            align-items: center;
            gap: 15px;
            position: sticky;
            top: 0;
            z-index: 10;
        }

        .back-btn {
            background: rgba(255,255,255,0.2);
            border: none;
            font-size: 1.2rem;
            padding: 8px 18px;
            border-radius: 10px;
            color: white;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .back-btn:hover {
            background: rgba(255,255,255,0.3);
            transform: translateX(-3px);
        }

        .levels-title {
            color: white;
            font-size: 1.3rem;
            flex: 1;
        }

        .levels-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(75px, 1fr));
            gap: 12px;
            padding: 20px;
            max-width: 1000px;
            margin: 0 auto;
            width: 100%;
        }

        .level-card {
            aspect-ratio: 1;
            background: linear-gradient(135deg, #667eea, #764ba2);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.1rem;
            font-weight: bold;
            color: white;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
        }

        .level-card:active {
            transform: scale(0.95);
        }

        .level-card:hover {
            transform: translateY(-5px) scale(1.05);
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
        }

        .level-card.locked {
            background: #333;
            opacity: 0.5;
            cursor: not-allowed;
        }

        .level-card.locked:hover {
            transform: none;
        }

        .level-card.completed {
            background: linear-gradient(135deg, #4CAF50, #45a049);
        }

        .level-card.completed::after {
            content: '✓';
            position: absolute;
            top: -5px;
            right: -5px;
            background: gold;
            color: #333;
            width: 22px;
            height: 22px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            animation: bounceIn 0.4s ease;
        }

        .stars {
            position: absolute;
            bottom: 3px;
            font-size: 10px;
            letter-spacing: 1px;
        }

        /* Win Modal */
        .win-modal {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.95);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 200;
            animation: fadeIn 0.4s ease;
        }

        .modal-content {
            background: white;
            border-radius: 20px;
            padding: 30px;
            text-align: center;
            max-width: 350px;
            width: 90%;
            animation: zoomIn 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }

        .win-icon {
            font-size: 4rem;
            margin-bottom: 15px;
            animation: bounce 0.6s ease;
        }

        .modal-title {
            font-size: 1.8rem;
            color: #4CAF50;
            margin-bottom: 10px;
        }

        .modal-stats {
            background: #f5f5f5;
            padding: 15px;
            border-radius: 12px;
            margin: 20px 0;
        }

        .modal-buttons {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .modal-btn {
            padding: 12px;
            border: none;
            border-radius: 10px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1rem;
        }

        .modal-btn:active {
            transform: scale(0.98);
        }

        .modal-btn:hover {
            transform: translateY(-2px);
        }

        .modal-btn-primary {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            color: white;
        }

        .modal-btn-secondary {
            background: linear-gradient(135deg, #ff9800, #f57c00);
            color: white;
        }

        .modal-btn-text {
            background: none;
            color: #666;
        }

        /* Preview Modal */
        .preview-modal {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.95);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 150;
            animation: fadeIn 0.3s ease;
        }

        .preview-content {
            background: white;
            border-radius: 20px;
            padding: 30px;
            text-align: center;
            max-width: 300px;
            width: 90%;
            animation: zoomIn 0.3s ease;
        }

        .preview-level {
            font-size: 3rem;
            font-weight: bold;
            color: #667eea;
            margin: 15px 0;
        }

        .difficulty {
            display: inline-block;
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: bold;
            margin-bottom: 15px;
        }

        /* Animations */
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes slideDown {
            from {
                opacity: 0;
                transform: translateY(-50px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes bounceIn {
            0% {
                transform: scale(0);
                opacity: 0;
            }
            60% {
                transform: scale(1.1);
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-15px); }
        }

        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        @keyframes zoomIn {
            from {
                opacity: 0;
                transform: scale(0.8);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }

        /* Responsive */
        @media (max-width: 768px) {
            .tube {
                width: 70px;
                height: 230px;
            }
            
            .tube-ball {
                width: 50px;
                height: 50px;
            }
            
            .empty-tube {
                width: 50px;
                height: 50px;
            }
            
            .game-title {
                font-size: 2.5rem;
            }
            
            .levels-grid {
                grid-template-columns: repeat(auto-fill, minmax(65px, 1fr));
                gap: 10px;
            }
            
            .volume-slider {
                width: 60px;
            }
        }

        @media (max-width: 480px) {
            .tube {
                width: 60px;
                height: 200px;
            }
            
            .tube-ball {
                width: 42px;
                height: 42px;
            }
            
            .empty-tube {
                width: 42px;
                height: 42px;
            }
        }
    </style>
</head>
<body>
    <div id="app"></div>

    <script>
        // Game Configuration
        const TUBE_CAPACITY = 4;
        const TOTAL_LEVELS = 1500;

        // Beautiful Ball Colors
        const BALL_COLORS = [
            '#FF4444', // Red
            '#4444FF', // Blue
            '#44FF44', // Green
            '#FFD700', // Gold
            '#FF44FF', // Pink
            '#44FFFF', // Cyan
            '#FF8844', // Orange
            '#8844FF', // Purple
            '#FF6688', // Rose
            '#66FF88', // Mint
            '#FFAA44', // Amber
            '#AA44FF'  // Violet
        ];

        // Game State
        let gameState = {
            currentView: 'loading',
            loadingProgress: 0,
            currentLevel: 1,
            tubes: [],
            selectedTube: null,
            history: [],
            moves: 0,
            completedLevels: [],
            levelStars: {},
            soundEnabled: true,
            volume: 0.5
        };

        // DOM Elements
        const app = document.getElementById('app');

        // Audio Context
        let audioContext = null;
        let masterGain = null;

        // Initialize Audio
        function initAudio() {
            if (!audioContext && gameState.soundEnabled) {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                masterGain = audioContext.createGain();
                masterGain.gain.value = gameState.volume;
                masterGain.connect(audioContext.destination);
            }
        }

        function playSound(type) {
            if (!gameState.soundEnabled) return;
            
            try {
                initAudio();
                if (!audioContext) return;
                
                const now = audioContext.currentTime;
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(masterGain);
                
                let frequency = 523.25;
                let duration = 0.1;
                
                switch(type) {
                    case 'move':
                        frequency = 523.25;
                        duration = 0.1;
                        break;
                    case 'win':
                        frequency = 659.25;
                        duration = 0.3;
                        break;
                    case 'click':
                        frequency = 392;
                        duration = 0.05;
                        break;
                    case 'undo':
                        frequency = 293.66;
                        duration = 0.08;
                        break;
                    case 'next':
                        frequency = 783.99;
                        duration = 0.15;
                        break;
                }
                
                oscillator.frequency.value = frequency;
                gainNode.gain.setValueAtTime(0.15, now);
                gainNode.gain.exponentialRampToValueAtTime(0.01, now + duration);
                
                oscillator.start(now);
                oscillator.stop(now + duration);
            } catch(e) {
                console.log('Audio error:', e);
            }
        }

        function setVolume(value) {
            gameState.volume = value;
            if (masterGain) {
                masterGain.gain.value = value;
            }
        }

        // Level Generation
        function generateLevel(level) {
            let numColors, emptyTubes;
            
            if (level <= 300) {
                numColors = Math.min(3 + Math.floor((level - 1) / 60), 4);
                emptyTubes = 2;
            } else if (level <= 600) {
                numColors = Math.min(4 + Math.floor((level - 301) / 75), 5);
                emptyTubes = 2;
            } else if (level <= 900) {
                numColors = Math.min(5 + Math.floor((level - 601) / 100), 6);
                emptyTubes = 3;
            } else if (level <= 1200) {
                numColors = Math.min(6 + Math.floor((level - 901) / 100), 7);
                emptyTubes = 3;
            } else {
                numColors = Math.min(7 + Math.floor((level - 1201) / 100), 8);
                emptyTubes = 4;
            }
            
            const usedColors = [...BALL_COLORS.slice(0, numColors)];
            
            let balls = [];
            usedColors.forEach(color => {
                for (let i = 0; i < TUBE_CAPACITY; i++) {
                    balls.push(color);
                }
            });
            
            for (let i = balls.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [balls[i], balls[j]] = [balls[j], balls[i]];
            }
            
            const tubes = [];
            let ballIndex = 0;
            
            for (let i = 0; i < numColors; i++) {
                const tube = [];
                for (let j = 0; j < TUBE_CAPACITY; j++) {
                    tube.push(balls[ballIndex++]);
                }
                tubes.push(tube);
            }
            
            for (let i = 0; i < emptyTubes; i++) {
                tubes.push([]);
            }
            
            for (let i = tubes.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [tubes[i], tubes[j]] = [tubes[j], tubes[i]];
            }
            
            return tubes;
        }

        // Check Win Condition
        function checkWin(tubes) {
            const completedTubes = tubes.filter(tube => {
                if (tube.length !== TUBE_CAPACITY) return false;
                const firstColor = tube[0];
                return tube.every(color => color === firstColor);
            });
            
            const allTubesValid = tubes.every(tube => {
                if (tube.length === 0) return true;
                if (tube.length === TUBE_CAPACITY) {
                    const firstColor = tube[0];
                    return tube.every(color => color === firstColor);
                }
                return true;
            });
            
            return allTubesValid && completedTubes.length > 0;
        }

        // Calculate Stars
        function calculateStars(moves, colors) {
            const optimalMoves = colors * 3;
            if (moves <= optimalMoves) return 3;
            if (moves <= optimalMoves * 1.8) return 2;
            return 1;
        }

        // Save Progress
        function saveProgress() {
            localStorage.setItem('ballsort2_level', gameState.currentLevel.toString());
            localStorage.setItem('ballsort2_completed', JSON.stringify(gameState.completedLevels));
            localStorage.setItem('ballsort2_stars', JSON.stringify(gameState.levelStars));
        }

        // Load Progress
        function loadProgress() {
            const savedLevel = localStorage.getItem('ballsort2_level');
            const savedCompleted = localStorage.getItem('ballsort2_completed');
            const savedStars = localStorage.getItem('ballsort2_stars');
            
            if (savedLevel) gameState.currentLevel = parseInt(savedLevel);
            if (savedCompleted) gameState.completedLevels = JSON.parse(savedCompleted);
            if (savedStars) gameState.levelStars = JSON.parse(savedStars);
        }

        // Make Move
        function makeMove(fromIndex, toIndex) {
            const fromTube = [...gameState.tubes[fromIndex]];
            const toTube = [...gameState.tubes[toIndex]];
            
            if (fromTube.length === 0) return false;
            
            const ball = fromTube[fromTube.length - 1];
            
            if (toTube.length < TUBE_CAPACITY && (toTube.length === 0 || toTube[toTube.length - 1] === ball)) {
                gameState.history.push(JSON.parse(JSON.stringify(gameState.tubes)));
                
                fromTube.pop();
                toTube.push(ball);
                
                gameState.tubes[fromIndex] = fromTube;
                gameState.tubes[toIndex] = toTube;
                gameState.moves++;
                
                playSound('move');
                
                if (checkWin(gameState.tubes)) {
                    handleWin();
                }
                
                return true;
            }
            
            return false;
        }

        // Handle Win
        function handleWin() {
            const numColors = gameState.tubes.filter(t => t.length === TUBE_CAPACITY).length;
            const stars = calculateStars(gameState.moves, numColors);
            
            if (!gameState.completedLevels.includes(gameState.currentLevel)) {
                gameState.completedLevels.push(gameState.currentLevel);
            }
            
            if (!gameState.levelStars[gameState.currentLevel] || gameState.levelStars[gameState.currentLevel] < stars) {
                gameState.levelStars[gameState.currentLevel] = stars;
            }
            
            saveProgress();
            playSound('win');
            
            canvasConfetti({
                particleCount: 200,
                spread: 100,
                origin: { y: 0.6 },
                colors: ['#ffd700', '#ff9800', '#ff4444', '#44ff44', '#4444ff'],
                startVelocity: 20,
                decay: 0.9
            });
            
            showWinModal(stars);
        }

        // Show Win Modal
        function showWinModal(stars) {
            const nextLevel = gameState.currentLevel < TOTAL_LEVELS ? gameState.currentLevel + 1 : null;
            
            const modal = document.createElement('div');
            modal.className = 'win-modal';
            modal.innerHTML = `
                <div class="modal-content">
                    <div class="win-icon">🏆</div>
                    <h2 class="modal-title">Level Complete!</h2>
                    <p class="modal-subtitle" style="color: #666; margin-bottom: 10px;">Level ${gameState.currentLevel}</p>
                    <div class="modal-stats">
                        <p>🎯 Moves: ${gameState.moves}</p>
                        <p>⭐ Stars: ${'⭐'.repeat(stars)}</p>
                    </div>
                    <div class="modal-buttons">
                        ${nextLevel ? `<button class="modal-btn modal-btn-primary" onclick="nextLevel()">Next Level →</button>` : '<button class="modal-btn modal-btn-primary" onclick="completeGame()">🎉 You Win! 🎉</button>'}
                        <button class="modal-btn modal-btn-secondary" onclick="replayLevel()">Play Again</button>
                        <button class="modal-btn modal-btn-text" onclick="closeModalAndGoToLevels()">Level Map</button>
                    </div>
                </div>
            `;
            app.appendChild(modal);
        }

        // Next Level
        window.nextLevel = function() {
            if (gameState.currentLevel < TOTAL_LEVELS) {
                gameState.currentLevel++;
                saveProgress();
                closeModal();
                playSound('next');
                loadLevel(gameState.currentLevel);
            } else {
                completeGame();
            }
        };

        // Complete Game
        window.completeGame = function() {
            closeModal();
            alert('🎉 Congratulations! You completed all 1500 levels! 🎉');
            gameState.currentView = 'start';
            render();
        };

        // Replay Level
        window.replayLevel = function() {
            closeModal();
            loadLevel(gameState.currentLevel);
        };

        // Close Modal and Go to Levels
        window.closeModalAndGoToLevels = function() {
            closeModal();
            gameState.currentView = 'levels';
            render();
        };

        // Close Modal
        function closeModal() {
            const modal = document.querySelector('.win-modal, .preview-modal');
            if (modal) modal.remove();
        }

        // Load Level
        function loadLevel(level) {
            gameState.tubes = generateLevel(level);
            gameState.selectedTube = null;
            gameState.history = [];
            gameState.moves = 0;
            gameState.currentLevel = level;
            gameState.currentView = 'game';
            render();
        }

        // Reset Level
        function resetLevel() {
            if (confirm('Reset current level?')) {
                loadLevel(gameState.currentLevel);
            }
        }

        // Undo Move
        function undoMove() {
            if (gameState.history.length > 0) {
                gameState.tubes = gameState.history.pop();
                gameState.moves--;
                gameState.selectedTube = null;
                playSound('undo');
                render();
            }
        }

        // Handle Tube Click - NO BLINKING
        function handleTubeClick(index) {
            if (gameState.selectedTube === null) {
                if (gameState.tubes[index].length > 0) {
                    gameState.selectedTube = index;
                    playSound('click');
                    render();
                }
            } else {
                if (gameState.selectedTube === index) {
                    gameState.selectedTube = null;
                    render();
                } else {
                    const success = makeMove(gameState.selectedTube, index);
                    gameState.selectedTube = null;
                    render();
                }
            }
        }

        // Render Loading Screen
        function renderLoading() {
            return `
                <div class="loading-screen">
                    <h1 class="loading-title">BALL SORT 2</h1>
                    <div class="loading-bar">
                        <div class="loading-progress" style="width: ${gameState.loadingProgress}%"></div>
                    </div>
                    <p style="color: white; margin-top: 15px;">Loading... ${Math.round(gameState.loadingProgress)}%</p>
                    <p style="color: rgba(255,255,255,0.7); margin-top: 10px; font-size: 0.9rem;">${TOTAL_LEVELS} Challenging Levels</p>
                </div>
            `;
        }

        // Render Start Screen
        function renderStart() {
            const completedCount = gameState.completedLevels.length;
            const completionPercent = Math.round((completedCount / TOTAL_LEVELS) * 100);
            
            return `
                <div class="start-screen">
                    <button class="sound-btn" onclick="toggleSound()">${gameState.soundEnabled ? '🔊' : '🔈'}</button>
                    <h1 class="game-title">BALL SORT 2</h1>
                    <div class="ball-preview">
                        <div class="color-ball" style="background: #FF4444; animation-delay: 0s"></div>
                        <div class="color-ball" style="background: #4444FF; animation-delay: 0.2s"></div>
                        <div class="color-ball" style="background: #44FF44; animation-delay: 0.4s"></div>
                        <div class="color-ball" style="background: #FFD700; animation-delay: 0.6s"></div>
                        <div class="color-ball" style="background: #FF44FF; animation-delay: 0.8s"></div>
                    </div>
                    <div class="stats">
                        📊 ${completedCount}/${TOTAL_LEVELS} Levels Completed (${completionPercent}%)
                    </div>
                    <button class="play-btn" onclick="startGame()">🎮 PLAY NOW</button>
                </div>
            `;
        }

        // Render Levels Screen
        function renderLevels() {
            const levels = [];
            for (let i = 1; i <= TOTAL_LEVELS; i++) {
                const isUnlocked = i === 1 || gameState.completedLevels.includes(i - 1);
                const isCompleted = gameState.completedLevels.includes(i);
                const stars = gameState.levelStars[i] || 0;
                
                levels.push(`
                    <div class="level-card ${!isUnlocked ? 'locked' : ''} ${isCompleted ? 'completed' : ''}" 
                         onclick="${isUnlocked ? `previewLevel(${i})` : ''}">
                        ${i}
                        ${isCompleted ? `<div class="stars">${'⭐'.repeat(stars)}</div>` : ''}
                    </div>
                `);
            }
            
            return `
                <div class="levels-screen">
                    <div class="levels-header">
                        <button class="back-btn" onclick="goBack()">← Back</button>
                        <h2 class="levels-title">${TOTAL_LEVELS} Levels</h2>
                        <div class="stats" style="margin: 0; padding: 5px 12px;">⭐ ${gameState.completedLevels.length}</div>
                    </div>
                    <div class="levels-grid">
                        ${levels.join('')}
                    </div>
                </div>
            `;
        }

        // Preview Level
        window.previewLevel = function(level) {
            const stars = gameState.levelStars[level] || 0;
            const difficulty = getDifficulty(level);
            const difficultyColor = getDifficultyColor(level);
            
            const modal = document.createElement('div');
            modal.className = 'preview-modal';
            modal.innerHTML = `
                <div class="preview-content">
                    <h3>Level ${level}</h3>
                    <div class="preview-level">${level}</div>
                    <div class="difficulty" style="background: ${difficultyColor}20; color: ${difficultyColor}">
                        ${difficulty}
                    </div>
                    ${stars > 0 ? `<div class="stars" style="margin-bottom: 15px; font-size: 1.2rem;">${'⭐'.repeat(stars)}</div>` : ''}
                    <button class="modal-btn modal-btn-primary" onclick="startLevel(${level})">🚀 START</button>
                    <button class="modal-btn modal-btn-text" onclick="closeModal()">Cancel</button>
                </div>
            `;
            app.appendChild(modal);
        };

        window.startLevel = function(level) {
            closeModal();
            loadLevel(level);
        };

        function getDifficulty(level) {
            if (level <= 300) return 'Easy';
            if (level <= 600) return 'Medium';
            if (level <= 900) return 'Hard';
            if (level <= 1200) return 'Expert';
            return 'Master';
        }

        function getDifficultyColor(level) {
            if (level <= 300) return '#4CAF50';
            if (level <= 600) return '#FF9800';
            if (level <= 900) return '#f44336';
            if (level <= 1200) return '#9C27B0';
            return '#FF0000';
        }

        // Render Game Screen
        function renderGame() {
            const tubesHtml = gameState.tubes.map((tube, idx) => {
                const isSelected = gameState.selectedTube === idx;
                const ballsHtml = tube.map((color, ballIdx) => {
                    const isFloating = isSelected && ballIdx === tube.length - 1;
                    return `<div class="tube-ball ${isFloating ? 'floating' : ''}" style="background: ${color}"></div>`;
                }).join('');
                
                const emptySpaces = Array(TUBE_CAPACITY - tube.length).fill().map(() => 
                    '<div class="empty-tube"></div>'
                ).join('');
                
                return `
                    <div class="tube ${isSelected ? 'selected' : ''}" onclick="handleTubeClick(${idx})">
                        ${ballsHtml}
                        ${emptySpaces}
                    </div>
                `;
            }).join('');
            
            return `
                <div class="game-container">
                    <div class="game-header">
                        <div class="level-info">
                            <div class="level-number">🎯 Level ${gameState.currentLevel}/${TOTAL_LEVELS}</div>
                            <div class="moves-count">🎬 Moves: ${gameState.moves}</div>
                        </div>
                        <div class="btn-group">
                            <button class="btn" onclick="undoMove()" ${gameState.history.length === 0 ? 'disabled' : ''}>↩ Undo</button>
                            <button class="btn btn-warning" onclick="resetLevel()">🔄 Reset</button>
                            <button class="btn" onclick="goToLevels()">📋 Levels</button>
                            <div class="volume-control">
                                <span>🔊</span>
                                <input type="range" class="volume-slider" min="0" max="1" step="0.01" value="${gameState.volume}" onchange="setVolumeVolume(this.value)">
                            </div>
                            <button class="btn" onclick="toggleSound()">${gameState.soundEnabled ? '🔊' : '🔈'}</button>
                        </div>
                    </div>
                    <div class="tubes-grid">
                        ${tubesHtml}
                    </div>
                </div>
            `;
        }

        // Volume Control
        window.setVolumeVolume = function(value) {
            setVolume(parseFloat(value));
        };

        // Navigation Functions
        window.startGame = function() {
            gameState.currentView = 'levels';
            render();
        };

        window.goBack = function() {
            gameState.currentView = 'start';
            render();
        };

        window.goToLevels = function() {
            gameState.currentView = 'levels';
            render();
        };

        window.toggleSound = function() {
            gameState.soundEnabled = !gameState.soundEnabled;
            if (gameState.soundEnabled && !audioContext) {
                initAudio();
            }
            render();
        };

        // Make functions global
        window.handleTubeClick = handleTubeClick;
        window.undoMove = undoMove;
        window.resetLevel = resetLevel;
        window.closeModal = closeModal;

        // Main Render Function
        function render() {
            let html = '';
            
            switch(gameState.currentView) {
                case 'loading':
                    html = renderLoading();
                    break;
                case 'start':
                    html = renderStart();
                    break;
                case 'levels':
                    html = renderLevels();
                    break;
                case 'game':
                    html = renderGame();
                    break;
            }
            
            app.innerHTML = html;
        }

        // Loading Animation
        function startLoading() {
            const startTime = Date.now();
            const duration = 2000;
            
            const interval = setInterval(() => {
                const elapsed = Date.now() - startTime;
                const progress = Math.min((elapsed / duration) * 100, 100);
                gameState.loadingProgress = progress;
                render();
                
                if (progress >= 100) {
                    clearInterval(interval);
                    setTimeout(() => {
                        gameState.currentView = 'start';
                        render();
                    }, 300);
                }
            }, 50);
        }

        // Initialize Game
        function init() {
            loadProgress();
            render();
            startLoading();
        }

        // Start the game
        init();
    </script>
</body>
</html>
