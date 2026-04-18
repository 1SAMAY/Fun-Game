<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Epic Whack-a-Mole</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }
        
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');
        
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            overflow: hidden;
            position: relative;
        }
        
        .stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }
        
        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite;
        }
        
        @keyframes twinkle {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }
        
        .container {
            background: rgba(255, 255, 255, 0.15);
            border-radius: 25px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            width: 100%;
            max-width: 550px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        
        .container::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(255,255,255,0.1), transparent);
            transform: rotate(45deg);
            animation: shine 6s infinite;
        }
        
        @keyframes shine {
            0% { transform: rotate(45deg) translateX(-100%); }
            100% { transform: rotate(45deg) translateX(100%); }
        }
        
        h1 {
            font-size: 3rem;
            margin-bottom: 20px;
            text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.3);
            color: #ffde59;
            font-weight: 700;
            letter-spacing: 2px;
            position: relative;
        }
        
        h1::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 4px;
            background: linear-gradient(90deg, #ffde59, #ff6b6b);
            border-radius: 2px;
        }
        
        .game-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
            background: rgba(0, 0, 0, 0.25);
            padding: 20px;
            border-radius: 15px;
            border: 2px solid rgba(255, 255, 255, 0.1);
        }
        
        .score, .time {
            font-size: 1.8rem;
            font-weight: 600;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .score span, .time span {
            font-size: 2.5rem;
            font-weight: 700;
            color: #ffde59;
            margin-top: 5px;
        }
        
        .board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .hole {
            width: 100%;
            height: 140px;
            background: linear-gradient(145deg, #5d4037, #4e342e);
            border-radius: 50%;
            position: relative;
            overflow: hidden;
            cursor: pointer;
            box-shadow: inset 0 15px 0 rgba(0, 0, 0, 0.4),
                        0 10px 20px rgba(0, 0, 0, 0.3);
            transition: transform 0.3s ease;
        }
        
        .hole:hover {
            transform: scale(1.05);
        }
        
        .hole:after {
            content: '';
            position: absolute;
            width: 130%;
            height: 60px;
            background: #3e2723;
            bottom: -30px;
            left: -15%;
            border-radius: 50%;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        .mole {
            position: absolute;
            width: 85%;
            height: 85%;
            top: 100%;
            left: 7.5%;
            background: linear-gradient(145deg, #8d6e63, #6d4c41);
            border-radius: 50%;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            border: 3px solid #5d4037;
        }
        
        .mole::before {
            content: '';
            position: absolute;
            top: 20%;
            left: 30%;
            width: 15%;
            height: 15%;
            background: #333;
            border-radius: 50%;
        }
        
        .mole::after {
            content: '';
            position: absolute;
            top: 20%;
            right: 30%;
            width: 15%;
            height: 15%;
            background: #333;
            border-radius: 50%;
        }
        
        .mole.up {
            top: 7.5%;
            transform: translateY(0);
        }
        
        .mole.whacked {
            top: 100%;
            transition: top 0.2s ease;
            transform: scale(0.9);
        }
        
        .controls {
            margin-top: 25px;
        }
        
        button {
            background: linear-gradient(145deg, #ffde59, #ffc107);
            color: #333;
            border: none;
            padding: 15px 35px;
            font-size: 1.3rem;
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            box-shadow: 0 8px 25px rgba(255, 193, 7, 0.4);
            text-transform: uppercase;
            letter-spacing: 1px;
            position: relative;
            overflow: hidden;
        }
        
        button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            transition: 0.5s;
        }
        
        button:hover::before {
            left: 100%;
        }
        
        button:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 30px rgba(255, 193, 7, 0.6);
        }
        
        button:active {
            transform: translateY(0);
        }
        
        .game-over {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 100;
            backdrop-filter: blur(10px);
        }
        
        .game-over-content {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            padding: 40px;
            border-radius: 25px;
            text-align: center;
            color: white;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
            border: 2px solid rgba(255, 255, 255, 0.1);
            animation: popIn 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        
        @keyframes popIn {
            0% { transform: scale(0.5); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        
        .game-over h2 {
            font-size: 2.5rem;
            margin-bottom: 20px;
            color: #ffde59;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .final-score {
            font-size: 4rem;
            font-weight: 700;
            margin: 30px 0;
            color: #ffde59;
            text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.3);
        }
        
        .particle {
            position: absolute;
            background: #ffde59;
            border-radius: 50%;
            pointer-events: none;
            animation: particleFloat 1s ease-out forwards;
        }
        
        @keyframes particleFloat {
            0% {
                opacity: 1;
                transform: translate(0, 0) scale(1);
            }
            100% {
                opacity: 0;
                transform: translate(var(--tx), var(--ty)) scale(0);
            }
        }
        
        .combo {
            position: absolute;
            font-size: 2rem;
            font-weight: 700;
            color: #ffde59;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            animation: comboFloat 1s ease-out forwards;
            pointer-events: none;
        }
        
        @keyframes comboFloat {
            0% {
                opacity: 1;
                transform: translate(-50%, -50%) scale(1);
            }
            100% {
                opacity: 0;
                transform: translate(-50%, -100px) scale(1.5);
            }
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 25px;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .hole {
                height: 100px;
            }
            
            .score, .time {
                font-size: 1.4rem;
            }
            
            .score span, .time span {
                font-size: 2rem;
            }
            
            button {
                padding: 12px 25px;
                font-size: 1.1rem;
            }
        }
    </style>
</head>
<body>
    <div class="stars" id="stars"></div>
    
    <div class="container">
        <h1>EPIC WHACK-A-MOLE</h1>
        
        <div class="game-info">
            <div class="score">SCORE<br><span id="score">0</span></div>
            <div class="time">TIME<br><span id="time">30</span>s</div>
        </div>
        
        <div class="board" id="board">
            <!-- Holes will be generated by JavaScript -->
        </div>
        
        <div class="controls">
            <button id="startBtn">START GAME</button>
        </div>
    </div>
    
    <div class="game-over" id="gameOver">
        <div class="game-over-content">
            <h2>GAME OVER!</h2>
            <p>YOUR FINAL SCORE:</p>
            <div class="final-score" id="finalScore">0</div>
            <button id="restartBtn">PLAY AGAIN</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // Create background stars
            function createStars() {
                const starsContainer = document.getElementById('stars');
                for (let i = 0; i < 50; i++) {
                    const star = document.createElement('div');
                    star.className = 'star';
                    star.style.width = Math.random() * 3 + 1 + 'px';
                    star.style.height = star.style.width;
                    star.style.left = Math.random() * 100 + '%';
                    star.style.top = Math.random() * 100 + '%';
                    star.style.animationDelay = Math.random() * 3 + 's';
                    starsContainer.appendChild(star);
                }
            }
            
            // Game variables
            let score = 0;
            let timeLeft = 30;
            let gameActive = false;
            let timerId = null;
            let moleTimerId = null;
            let lastHole = null;
            let combo = 0;
            let lastWhackTime = 0;
            
            // DOM elements
            const board = document.getElementById('board');
            const scoreDisplay = document.getElementById('score');
            const timeDisplay = document.getElementById('time');
            const startBtn = document.getElementById('startBtn');
            const gameOverScreen = document.getElementById('gameOver');
            const finalScoreDisplay = document.getElementById('finalScore');
            const restartBtn = document.getElementById('restartBtn');
            
            // Create holes
            function createBoard() {
                board.innerHTML = '';
                for (let i = 0; i < 9; i++) {
                    const hole = document.createElement('div');
                    hole.className = 'hole';
                    hole.innerHTML = '<div class="mole"></div>';
                    hole.addEventListener('click', whackMole);
                    board.appendChild(hole);
                }
            }
            
            // Get random hole
            function randomHole() {
                const holes = document.querySelectorAll('.hole');
                const idx = Math.floor(Math.random() * holes.length);
                const hole = holes[idx];
                
                // Avoid same hole twice in a row
                if (hole === lastHole) {
                    return randomHole();
                }
                
                lastHole = hole;
                return hole;
            }
            
            // Make mole appear
            function showMole() {
                if (!gameActive) return;
                
                const hole = randomHole();
                const mole = hole.querySelector('.mole');
                
                // Reset classes
                mole.classList.remove('whacked');
                mole.classList.add('up');
                
                // Make mole disappear after random time
                setTimeout(() => {
                    if (mole.classList.contains('up')) {
                        mole.classList.remove('up');
                    }
                }, 700 + Math.random() * 800);
            }
            
            // Create particles
            function createParticles(x, y, count, color) {
                for (let i = 0; i < count; i++) {
                    const particle = document.createElement('div');
                    particle.className = 'particle';
                    particle.style.left = x + 'px';
                    particle.style.top = y + 'px';
                    particle.style.width = Math.random() * 8 + 4 + 'px';
                    particle.style.height = particle.style.width;
                    particle.style.background = color;
                    particle.style.setProperty('--tx', (Math.random() - 0.5) * 100 + 'px');
                    particle.style.setProperty('--ty', (Math.random() - 0.5) * 100 + 'px');
                    document.body.appendChild(particle);
                    
                    setTimeout(() => {
                        particle.remove();
                    }, 1000);
                }
            }
            
            // Show combo text
            function showCombo(x, y, text) {
                const combo = document.createElement('div');
                combo.className = 'combo';
                combo.textContent = text;
                combo.style.left = x + 'px';
                combo.style.top = y + 'px';
                document.body.appendChild(combo);
                
                setTimeout(() => {
                    combo.remove();
                }, 1000);
            }
            
            // Whack the mole
            function whackMole(e) {
                if (!gameActive) return;
                
                const mole = e.target.closest('.mole');
                if (!mole || !mole.classList.contains('up')) {
                    return;
                }
                
                // Whack successful
                mole.classList.remove('up');
                mole.classList.add('whacked');
                
                // Calculate combo
                const currentTime = Date.now();
                if (currentTime - lastWhackTime < 1000) {
                    combo++;
                } else {
                    combo = 1;
                }
                lastWhackTime = currentTime;
                
                // Add score with combo bonus
                const points = 10 * Math.min(combo, 5);
                score += points;
                scoreDisplay.textContent = score;
                
                // Create particles at mole position
                const rect = mole.getBoundingClientRect();
                const x = rect.left + rect.width / 2;
                const y = rect.top + rect.height / 2;
                
                createParticles(x, y, 15, '#ffde59');
                
                // Show combo text
                if (combo > 1) {
                    showCombo(x, y - 50, combo + 'x COMBO!');
                }
                
                // Add visual feedback to hole
                const hole = mole.parentElement;
                hole.animate([
                    { transform: 'scale(0.95)' },
                    { transform: 'scale(1.05)' },
                    { transform: 'scale(1)' }
                ], {
                    duration: 300,
                    easing: 'ease-out'
                });
            }
            
            // Start the game
            function startGame() {
                score = 0;
                timeLeft = 30;
                gameActive = true;
                combo = 0;
                
                scoreDisplay.textContent = score;
                timeDisplay.textContent = timeLeft;
                
                startBtn.textContent = 'PLAYING...';
                startBtn.disabled = true;
                
                gameOverScreen.style.display = 'none';
                
                // Start timer
                timerId = setInterval(() => {
                    timeLeft--;
                    timeDisplay.textContent = timeLeft;
                    
                    if (timeLeft <= 0) {
                        endGame();
                    }
                }, 1000);
                
                // Start mole appearances
                moleTimerId = setInterval(showMole, 900);
            }
            
            // End the game
            function endGame() {
                gameActive = false;
                
                clearInterval(timerId);
                clearInterval(moleTimerId);
                
                startBtn.textContent = 'START GAME';
                startBtn.disabled = false;
                
                // Show game over screen
                finalScoreDisplay.textContent = score;
                gameOverScreen.style.display = 'flex';
                
                // Create celebration particles
                createParticles(window.innerWidth / 2, window.innerHeight / 2, 50, '#ffde59');
                
                // Hide all moles
                document.querySelectorAll('.mole').forEach(mole => {
                    mole.classList.remove('up');
                });
            }
            
            // Event listeners
            startBtn.addEventListener('click', startGame);
            restartBtn.addEventListener('click', startGame);
            
            // Initialize the game
            createStars();
            createBoard();
        });
    </script>
</body>
</html>
