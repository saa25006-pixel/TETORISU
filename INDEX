<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEON TETRIS - ネオンテトリス</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Noto+Sans+JP:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Orbitron', 'Noto Sans JP', sans-serif;
            background-color: #0b0d19;
            overflow-x: hidden;
            user-select: none;
            -webkit-user-select: none;
        }
        /* ネオングロー効果 */
        .neon-text-cyan {
            text-shadow: 0 0 5px #00f0ff, 0 0 15px #00f0ff;
        }
        .neon-text-pink {
            text-shadow: 0 0 5px #ff007f, 0 0 15px #ff007f;
        }
        .neon-border {
            box-shadow: 0 0 10px rgba(0, 240, 255, 0.3), inset 0 0 10px rgba(0, 240, 255, 0.2);
            border-color: #00f0ff;
        }
        .neon-border-pink {
            box-shadow: 0 0 10px rgba(ff, 0, 127, 0.3), inset 0 0 10px rgba(ff, 0, 127, 0.2);
            border-color: #ff007f;
        }
        /* スムーズなボタンアニメーション */
        .btn-glow:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body class="flex flex-col min-h-screen text-slate-100 items-center justify-between p-2 md:p-4">

<!-- メインヘッダー & スコアボード -->
<header class="w-full max-w-md flex justify-between items-center px-4 py-2 bg-slate-900/80 rounded-2xl border border-slate-800 backdrop-blur-md mb-2">
    <div>
        <h1 class="text-2xl font-black tracking-wider text-cyan-400 neon-text-cyan">NEON</h1>
        <p class="text-xs tracking-widest text-pink-500 font-bold -mt-1">TETRIS</p>
    </div>
    <div class="flex gap-4">
        <div class="text-right">
            <span class="block text-[10px] text-slate-400 font-bold">SCORE</span>
            <span id="score" class="text-xl font-bold text-pink-400 neon-text-pink">0</span>
        </div>
        <div class="text-right border-l border-slate-700 pl-4">
            <span class="block text-[10px] text-slate-400 font-bold">HI-SCORE</span>
            <span id="hi-score" class="text-xl font-bold text-yellow-400">0</span>
        </div>
    </div>
</header>

<!-- メインゲームエリア -->
<main class="w-full max-w-lg flex flex-col items-center gap-2">
    <!-- スマホ/デスクトップ兼用 3カラムレイアウト -->
    <div class="w-full flex justify-center items-stretch gap-2 md:gap-4 px-2">
        
        <!-- 左サイド：HOLD & STATS -->
        <div class="flex flex-col justify-between w-20 md:w-28 gap-2">
            <!-- HOLD -->
            <div class="bg-slate-900/90 rounded-2xl border-2 neon-border p-2 flex flex-col items-center flex-1">
                <span class="text-xs font-bold text-cyan-400 tracking-wider mb-1">HOLD</span>
                <canvas id="holdCanvas" width="80" height="80" class="w-full aspect-square bg-slate-950 rounded-lg"></canvas>
            </div>
            
            <!-- STATS -->
            <div class="bg-slate-900/90 rounded-2xl border border-slate-800 p-2 flex flex-col justify-around flex-1 text-center">
                <div>
                    <span class="block text-[10px] text-slate-400 font-bold">LEVEL</span>
                    <span id="level" class="text-lg font-bold text-cyan-400">1</span>
                </div>
                <div class="border-t border-slate-800/80 pt-1">
                    <span class="block text-[10px] text-slate-400 font-bold">LINES</span>
                    <span id="lines" class="text-lg font-bold text-purple-400">0</span>
                </div>
            </div>
        </div>

        <!-- 中央：メインゲーム盤面 -->
        <div class="relative flex-1 max-w-[280px] md:max-w-[320px]">
            <!-- ゲームオーバー/ポーズのオーバーレイ -->
            <div id="screenOverlay" class="absolute inset-0 bg-slate-950/90 rounded-2xl border-2 neon-border-pink flex flex-col items-center justify-center gap-4 z-10 p-4 text-center">
                <div id="overlayTextContainer">
                    <h2 id="overlayTitle" class="text-3xl font-black text-pink-500 neon-text-pink tracking-widest mb-2">NEON TETRIS</h2>
                    <p id="overlayDesc" class="text-sm text-slate-300 max-w-xs leading-relaxed">矢印キーまたは画面下のボタンで操作。ネオンの世界へようこそ！</p>
                </div>
                <button id="startBtn" class="px-6 py-3 bg-gradient-to-r from-cyan-500 to-blue-600 rounded-xl font-bold tracking-wider hover:brightness-110 active:scale-95 transition-all text-white border-b-4 border-blue-800 shadow-lg shadow-cyan-500/30">
                    PLAY START
                </button>
            </div>
            
            <!-- キャンバス -->
            <div class="bg-slate-950 p-[3px] rounded-2xl border-2 border-slate-800">
                <canvas id="gameCanvas" width="300" height="600" class="w-full h-auto block rounded-xl bg-slate-950"></canvas>
            </div>
        </div>

        <!-- 右サイド：NEXT LIST -->
        <div class="flex flex-col w-20 md:w-28 gap-2 bg-slate-900/90 rounded-2xl border-2 border-slate-800 p-2 items-center">
            <span class="text-xs font-bold text-pink-400 tracking-wider mb-1">NEXT</span>
            <div class="flex flex-col gap-2 w-full flex-1 justify-around">
                <canvas id="next1" width="80" height="80" class="w-full aspect-square bg-slate-950 rounded-lg border border-slate-800/50"></canvas>
                <canvas id="next2" width="60" height="60" class="w-full aspect-square bg-slate-950 rounded-lg border border-slate-800/30 opacity-70"></canvas>
                <canvas id="next3" width="60" height="60" class="w-full aspect-square bg-slate-950 rounded-lg border border-slate-800/30 opacity-40"></canvas>
            </div>
        </div>

    </div>

    <!-- コントロールボタン / オプション -->
    <div class="w-full max-w-md flex justify-between px-2 gap-4 my-1">
        <button id="soundToggle" class="flex-1 py-1.5 px-3 bg-slate-900 hover:bg-slate-800 text-slate-300 rounded-xl border border-slate-800 text-xs font-bold flex items-center justify-center gap-1.5 transition-all">
            <svg id="soundOnIcon" class="w-4 h-4 text-cyan-400" fill="currentColor" viewBox="0 0 24 24"><path d="M5 17h-5v-10h5v10zm2-10v10l9 5v-20l-9 5zm11.025 5c0-2.438-1.423-4.516-3.473-5.5v1.8c1.1.751 1.83 2.039 1.83 3.5s-.73 2.749-1.83 3.5v1.8c2.05-.984 3.473-3.062 3.473-5.5zm2.975 0c0-4.453-2.613-8.256-6.425-9.975v1.942c2.812 1.614 4.706 4.671 4.706 8.033s-1.894 6.419-4.706 8.033v1.942c3.812-1.719 6.425-5.522 6.425-9.975z"/></svg>
            <svg id="soundOffIcon" class="w-4 h-4 text-slate-500 hidden" fill="currentColor" viewBox="0 0 24 24"><path d="M19 6.41l-1.41-1.41-5.59 5.59-5.59-5.59-1.41 1.41 5.59 5.59-5.59 5.59 1.41 1.41 5.59-5.59 5.59 5.59 1.41-1.41-5.59-5.59zM5 17h-5v-10h5v10zm2-10v10l9 5v-20l-9 5z"/></svg>
            <span id="soundText">SOUND: ON</span>
        </button>
        <button id="pauseBtn" class="flex-1 py-1.5 px-3 bg-slate-900 hover:bg-slate-800 text-slate-300 rounded-xl border border-slate-800 text-xs font-bold transition-all">
            PAUSE
        </button>
        <button id="resetBtn" class="flex-1 py-1.5 px-3 bg-slate-900 hover:bg-slate-800 text-slate-300 rounded-xl border border-slate-800 text-xs font-bold transition-all">
            RESET
        </button>
    </div>

    <!-- スマホ用タッチコントローラー -->
    <div class="w-full max-w-md grid grid-cols-5 gap-2 px-2 mt-1 md:hidden select-none">
        <button id="btnHold" class="btn-glow flex flex-col items-center justify-center py-3 bg-slate-900/90 border border-slate-800 rounded-xl active:bg-cyan-950 active:border-cyan-500 transition-all text-slate-300">
            <span class="text-[9px] font-bold">HOLD</span>
            <span class="text-xs">🔄</span>
        </button>
        <button id="btnLeft" class="btn-glow flex flex-col items-center justify-center py-3 bg-slate-900/90 border border-slate-800 rounded-xl active:bg-cyan-950 active:border-cyan-500 transition-all text-2xl">
            ◀
        </button>
        <div class="flex flex-col gap-1">
            <button id="btnRotate" class="btn-glow flex flex-col items-center justify-center py-1 bg-slate-900/90 border border-slate-800 rounded-xl active:bg-pink-950 active:border-pink-500 transition-all text-lg">
                <span>↻</span>
                <span class="text-[8px] font-bold">ROTATE</span>
            </button>
            <button id="btnHardDrop" class="btn-glow flex flex-col items-center justify-center py-1 bg-cyan-900/80 border border-cyan-700 rounded-xl active:bg-cyan-500 active:border-cyan-300 transition-all text-lg">
                <span>⬇</span>
                <span class="text-[8px] font-bold">HARD</span>
            </button>
        </div>
        <button id="btnRight" class="btn-glow flex flex-col items-center justify-center py-3 bg-slate-900/90 border border-slate-800 rounded-xl active:bg-cyan-950 active:border-cyan-500 transition-all text-2xl">
            ▶
        </button>
        <button id="btnSoftDrop" class="btn-glow flex flex-col items-center justify-center py-3 bg-slate-900/90 border border-slate-800 rounded-xl active:bg-cyan-950 active:border-cyan-500 transition-all text-xl">
            ▼
            <span class="text-[8px] font-bold block -mt-1">SOFT</span>
        </button>
    </div>

    <!-- デスクトップ用操作ガイド -->
    <div class="hidden md:flex justify-around w-full max-w-md text-[11px] text-slate-400 bg-slate-900/40 p-2 rounded-xl border border-slate-900/80 mt-2">
        <span>← → : 移動</span>
        <span>↑ : 回転</span>
        <span>↓ : ソフトドロップ</span>
        <span>SPACE : ハードドロップ</span>
        <span>C / SHIFT : ホールド</span>
    </div>
</main>

<footer class="text-center text-[10px] text-slate-600 mt-4 py-2 border-t border-slate-900/50 w-full">
    NEON TETRIS &copy; 2026 / Visual & Audio System Enabled
</footer>

<script>

// Web Audio APIによる効果音システム
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
let soundEnabled = true;

function playSound(type) {
    if (!soundEnabled) return;
    
    // Audio Context の自動再生制限を解除
    if (audioCtx.state === 'suspended') {
        audioCtx.resume();
    }
    
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    
    const now = audioCtx.currentTime;
    
    switch (type) {
        case 'move':
            osc.type = 'triangle';
            osc.frequency.setValueAtTime(150, now);
            osc.frequency.exponentialRampToValueAtTime(80, now + 0.1);
            gain.gain.setValueAtTime(0.15, now);
            gain.gain.exponentialRampToValueAtTime(0.01, now + 0.1);
            osc.start(now);
            osc.stop(now + 0.1);
            break;
            
        case 'rotate':
            osc.type = 'sine';
            osc.frequency.setValueAtTime(300, now);
            osc.frequency.exponentialRampToValueAtTime(450, now + 0.08);
            gain.gain.setValueAtTime(0.1, now);
            gain.gain.exponentialRampToValueAtTime(0.01, now + 0.08);
            osc.start(now);
            osc.stop(now + 0.08);
            break;
            
        case 'drop':
            osc.type = 'triangle';
            osc.frequency.setValueAtTime(100, now);
            osc.frequency.exponentialRampToValueAtTime(40, now + 0.15);
            gain.gain.setValueAtTime(0.2, now);
            gain.gain.exponentialRampToValueAtTime(0.01, now + 0.15);
            osc.start(now);
            osc.stop(now + 0.15);
            break;
            
        case 'clear':
            // 爽快な上行アルペジオ
            const notes = [261.63, 329.63, 392.00, 523.25]; // C4, E4, G4, C5
            notes.forEach((freq, index) => {
                const subOsc = audioCtx.createOscillator();
                const subGain = audioCtx.createGain();
                subOsc.connect(subGain);
                subGain.connect(audioCtx.destination);
                
                subOsc.type = 'sawtooth';
                const startTime = now + index * 0.06;
                subOsc.frequency.setValueAtTime(freq, startTime);
                subOsc.frequency.exponentialRampToValueAtTime(freq * 1.5, startTime + 0.15);
                
                subGain.gain.setValueAtTime(0.08, startTime);
                subGain.gain.exponentialRampToValueAtTime(0.001, startTime + 0.2);
                
                subOsc.start(startTime);
                subOsc.stop(startTime + 0.2);
            });
            break;
            
        case 'levelUp':
            osc.type = 'square';
            osc.frequency.setValueAtTime(440, now);
            osc.frequency.setValueAtTime(554.37, now + 0.1);
            osc.frequency.setValueAtTime(659.25, now + 0.2);
            osc.frequency.exponentialRampToValueAtTime(880, now + 0.4);
            gain.gain.setValueAtTime(0.1, now);
            gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
            osc.start(now);
            osc.stop(now + 0.4);
            break;
            
        case 'gameover':
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(180, now);
            osc.frequency.linearRampToValueAtTime(50, now + 0.8);
            gain.gain.setValueAtTime(0.2, now);
            gain.gain.exponentialRampToValueAtTime(0.001, now + 0.8);
            osc.start(now);
            osc.stop(now + 0.8);
            break;
    }
}


// ゲーム定数
const COLS = 10;
const ROWS = 20;
const BLOCK_SIZE = 30; // キャンバスピクセルサイズ (10x20で300x600)

// キャンバス要素の取得
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const holdCanvas = document.getElementById('holdCanvas');
const ctxHold = holdCanvas.getContext('2d');
const nextCanvases = [
    document.getElementById('next1'),
    document.getElementById('next2'),
    document.getElementById('next3')
];
const ctxNexts = nextCanvases.map(c => c.getContext('2d'));

// ミノ(Tetromino)の形状とネオンカラー
const SHAPES = {
    I: [[0,0,0,0], [1,1,1,1], [0,0,0,0], [0,0,0,0]],
    O: [[1,1], [1,1]],
    T: [[0,1,0], [1,1,1], [0,0,0]],
    S: [[0,1,1], [1,1,0], [0,0,0]],
    Z: [[1,1,0], [0,1,1], [0,0,0]],
    J: [[1,0,0], [1,1,1], [0,0,0]],
    L: [[0,0,1], [1,1,1], [0,0,0]]
};

const COLORS = {
    I: { base: '#00f0ff', glow: 'rgba(0, 240, 255, 0.5)', border: '#80f8ff' }, // Cyan
    O: { base: '#ffea00', glow: 'rgba(255, 234, 0, 0.5)', border: '#fff580' }, // Yellow
    T: { base: '#d500f9', glow: 'rgba(213, 0, 249, 0.5)', border: '#f180ff' }, // Magenta/Purple
    S: { base: '#00e676', glow: 'rgba(0, 230, 118, 0.5)', border: '#80ffbb' }, // Green
    Z: { base: '#ff1744', glow: 'rgba(255, 23, 68, 0.5)', border: '#ff8bb0' }, // Red
    J: { base: '#2979ff', glow: 'rgba(41, 121, 255, 0.5)', border: '#8cb7ff' }, // Blue
    L: { base: '#ff9100', glow: 'rgba(255, 145, 0, 0.5)', border: '#ffd180' }  // Orange
};

// ゲーム状態管理
let grid = Array.from({ length: ROWS }, () => Array(COLS).fill(0));
let currentPiece = null;
let queue = [];
let holdPiece = null;
let canHold = true;
let score = 0;
let level = 1;
let linesCleared = 0;
let isGameOver = false;
let isPaused = false;
let dropCounter = 0;
let lastTime = 0;
let highScor = localStorage.getItem('neon_tetris_hi_score') || 0;

// パーティクルシステム（ライン消去時のエフェクト用）
let particles = [];


class Particle {
    constructor(x, y, color) {
        this.x = x;
        this.y = y;
        this.size = Math.random() * 4 + 2;
        this.vx = (Math.random() - 0.5) * 8;
        this.vy = (Math.random() - 0.5) * 8 - 2; // 少し上に飛び散るように
        this.alpha = 1;
        this.decay = Math.random() * 0.02 + 0.015;
        this.color = color;
    }
    
    update() {
        this.x += this.vx;
        this.y += this.vy;
        this.vy += 0.1; // 重力
        this.alpha -= this.decay;
    }
    
    draw(targetCtx) {
        targetCtx.save();
        targetCtx.globalAlpha = this.alpha;
        targetCtx.shadowBlur = 10;
        targetCtx.shadowColor = this.color;
        targetCtx.fillStyle = this.color;
        targetCtx.beginPath();
        targetCtx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        targetCtx.fill();
        targetCtx.restore();
    }
}

function spawnLineParticles(row, color) {
    const y = row * BLOCK_SIZE + BLOCK_SIZE / 2;
    for (let col = 0; col < COLS; col++) {
        const x = col * BLOCK_SIZE + BLOCK_SIZE / 2;
        for (let i = 0; i < 6; i++) {
            particles.push(new Particle(x, y, color));
        }
    }
}


// 7-Bagランダマイザー（偏りをなくすシステム）
function refillQueue() {
    const keys = Object.keys(SHAPES);
    while (keys.length) {
        const idx = Math.floor(Math.random() * keys.length);
        queue.push(keys.splice(idx, 1)[0]);
    }
}

function getNextPiece() {
    if (queue.length < 5) {
        refillQueue();
    }
    const type = queue.shift();
    return {
        type: type,
        matrix: JSON.parse(JSON.stringify(SHAPES[type])),
        x: Math.floor((COLS - SHAPES[type][0].length) / 2),
        y: type === 'I' ? -1 : 0 // I型は上部ぎりぎりから配置
    };
}

// 衝突判定
function collide(field, piece, offset = { x: 0, y: 0 }) {
    const matrix = piece.matrix;
    const px = piece.x + offset.x;
    const py = piece.y + offset.y;
    
    for (let r = 0; r < matrix.length; r++) {
        for (let c = 0; c < matrix[r].length; c++) {
            if (matrix[r][c] !== 0) {
                const targetX = px + c;
                const targetY = py + r;
                
                // 壁・床の範囲外チェック
                if (targetX < 0 || targetX >= COLS || targetY >= ROWS) {
                    return true;
                }
                
                // 既存の固定ブロック衝突チェック
                if (targetY >= 0 && field[targetY][targetX] !== 0) {
                    return true;
                }
            }
        }
    }
    return false;
}

// 盤面に固定
function merge(field, piece) {
    piece.matrix.forEach((row, r) => {
        row.forEach((value, c) => {
            if (value !== 0) {
                const targetY = piece.y + r;
                if (targetY >= 0) {
                    field[targetY][piece.x + c] = piece.type;
                }
            }
        });
    });
}

// 回転（SRS風のシンプルなキックバック処理を含む）
function rotate(matrix, dir) {
    // 転置
    for (let r = 0; r < matrix.length; r++) {
        for (let c = r; c < matrix[r].length; c++) {
            [matrix[r][c], matrix[c][r]] = [matrix[c][r], matrix[r][c]];
        }
    }
    // 反転
    if (dir > 0) {
        matrix.forEach(row => row.reverse());
    } else {
        matrix.reverse();
    }
}

function playerRotate(dir) {
    if (isGameOver || isPaused) return;
    const originalMatrix = JSON.parse(JSON.stringify(currentPiece.matrix));
    rotate(currentPiece.matrix, dir);
    
    // 壁キック簡易実装：回転後に壁やブロックと重なったら左右に最大2マスずらして判定
    const kicks = [0, -1, 1, -2, 2];
    let success = false;
    for (let kick of kicks) {
        if (!collide(grid, currentPiece, { x: kick, y: 0 })) {
            currentPiece.x += kick;
            success = true;
            break;
        }
    }
    
    if (success) {
        playSound('rotate');
    } else {
        // ダメなら元に戻す
        currentPiece.matrix = originalMatrix;
    }
}


function playerMove(dir) {
    if (isGameOver || isPaused) return;
    if (!collide(grid, currentPiece, { x: dir, y: 0 })) {
        currentPiece.x += dir;
        playSound('move');
    }
}

function playerDrop() {
    if (isGameOver || isPaused) return;
    if (!collide(grid, currentPiece, { x: 0, y: 1 })) {
        currentPiece.y++;
        dropCounter = 0; // 手動落下時はタイマーリセット
    } else {
        lockPiece();
    }
}

function playerHardDrop() {
    if (isGameOver || isPaused) return;
    let dropDist = 0;
    while (!collide(grid, currentPiece, { x: 0, y: 1 })) {
        currentPiece.y++;
        dropDist++;
    }
    if (dropDist > 0) {
        score += dropDist * 2;
    }
    playSound('drop');
    lockPiece();
}

function playerHold() {
    if (isGameOver || isPaused || !canHold) return;
    playSound('rotate');
    
    if (!holdPiece) {
        holdPiece = currentPiece.type;
        currentPiece = getNextPiece();
    } else {
        const temp = holdPiece;
        holdPiece = currentPiece.type;
        currentPiece = {
            type: temp,
            matrix: JSON.parse(JSON.stringify(SHAPES[temp])),
            x: Math.floor((COLS - SHAPES[temp][0].length) / 2),
            y: temp === 'I' ? -1 : 0
        };
    }
    
    canHold = false;
    drawHold();
}

function lockPiece() {
    merge(grid, currentPiece);
    checkLines();
    
    // 新しいミノを生成
    currentPiece = getNextPiece();
    canHold = true;
    
    // 出現直後にすでに衝突していたらゲームオーバー
    if (collide(grid, currentPiece)) {
        gameOver();
    }
}


// ゴースト（落下地点予測）の計算
function getGhostPosition() {
    const ghost = { ...currentPiece };
    while (!collide(grid, ghost, { x: 0, y: 1 })) {
        ghost.y++;
    }
    return ghost.y;
}

// 描画：ブロック単位
function drawBlock(targetCtx, x, y, type, isGhost = false, blockSize = BLOCK_SIZE) {
    const info = COLORS[type];
    if (!info) return;

    targetCtx.save();
    
    const px = x * blockSize;
    const py = y * blockSize;

    if (isGhost) {
        // ゴーストは細い枠線と薄い発光のみ
        targetCtx.strokeStyle = info.base;
        targetCtx.lineWidth = 1.5;
        targetCtx.shadowBlur = 4;
        targetCtx.shadowColor = info.base;
        targetCtx.strokeRect(px + 1.5, py + 1.5, blockSize - 3, blockSize - 3);
    } else {
        // 通常のネオンブロック
        targetCtx.shadowBlur = 12;
        targetCtx.shadowColor = info.glow;
        
        // ベースのブロック色
        targetCtx.fillStyle = info.base;
        targetCtx.beginPath();
        targetCtx.roundRect(px + 1.5, py + 1.5, blockSize - 3, blockSize - 3, 4);
        targetCtx.fill();

        // 立体感を出すインナーグロー風ハイライト
        targetCtx.strokeStyle = info.border;
        targetCtx.lineWidth = 1.5;
        targetCtx.beginPath();
        targetCtx.roundRect(px + 2, py + 2, blockSize - 4, blockSize - 4, 3);
        targetCtx.stroke();
    }
    
    targetCtx.restore();
}

// メイン盤面の描画
function drawBoard() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    // 背景の微細なグリッド線（サイバー感強化）
    ctx.strokeStyle = '#161930';
    ctx.lineWidth = 0.5;
    for (let c = 0; c <= COLS; c++) {
        ctx.beginPath();
        ctx.moveTo(c * BLOCK_SIZE, 0);
        ctx.lineTo(c * BLOCK_SIZE, canvas.height);
        ctx.stroke();
    }
    for (let r = 0; r <= ROWS; r++) {
        ctx.beginPath();
        ctx.moveTo(0, r * BLOCK_SIZE);
        ctx.lineTo(canvas.width, r * BLOCK_SIZE);
        ctx.stroke();
    }
    
    // 固定済みブロック
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (grid[r][c] !== 0) {
                drawBlock(ctx, c, r, grid[r][c]);
            }
        }
    }
    
    // 操作中のミノ＆ゴースト
    if (currentPiece && !isGameOver) {
        const ghostY = getGhostPosition();
        
        // ゴーストを先に描画
        currentPiece.matrix.forEach((row, r) => {
            row.forEach((value, c) => {
                if (value !== 0) {
                    const targetY = ghostY + r;
                    if (targetY >= 0) {
                        drawBlock(ctx, currentPiece.x + c, targetY, currentPiece.type, true);
                    }
                }
            });
        });
        
        // アクティブなミノを描画
        currentPiece.matrix.forEach((row, r) => {
            row.forEach((value, c) => {
                if (value !== 0) {
                    const targetY = currentPiece.y + r;
                    if (targetY >= 0) {
                        drawBlock(ctx, currentPiece.x + c, targetY, currentPiece.type);
                    }
                }
            });
        });
    }

    // パーティクルの更新と描画
    particles.forEach((p, idx) => {
        p.update();
        p.draw(ctx);
        if (p.alpha <= 0) {
            particles.splice(idx, 1);
        }
    });
}


// ホールドとネクストプレビューの描画処理
function drawHold() {
    ctxHold.clearRect(0, 0, holdCanvas.width, holdCanvas.height);
    if (holdPiece) {
        const matrix = SHAPES[holdPiece];
        const offset = getCenteredOffset(matrix, holdCanvas.width, 20);
        matrix.forEach((row, r) => {
            row.forEach((value, c) => {
                if (value !== 0) {
                    drawBlock(ctxHold, c + offset.x, r + offset.y, holdPiece, false, 20);
                }
            });
        });
    }
}

function drawNexts() {
    for (let i = 0; i < 3; i++) {
        const type = queue[i];
        const subCtx = ctxNexts[i];
        const subCanvas = nextCanvases[i];
        subCtx.clearRect(0, 0, subCanvas.width, subCanvas.height);
        
        if (type) {
            const matrix = SHAPES[type];
            const size = i === 0 ? 20 : 15; // 2番目以降は少し小さく表示
            const offset = getCenteredOffset(matrix, subCanvas.width, size);
            matrix.forEach((row, r) => {
                row.forEach((value, c) => {
                    if (value !== 0) {
                        drawBlock(subCtx, c + offset.x, r + offset.y, type, false, size);
                    }
                });
            });
        }
    }
}

// 中央寄せ配置用ヘルパー
function getCenteredOffset(matrix, canvasSize, blockSize) {
    const rows = matrix.length;
    // 空行/空列を削ることで完璧な中央寄せを算出
    let minRow = rows, maxRow = -1, minCol = rows, maxCol = -1;
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < matrix[r].length; c++) {
            if (matrix[r][c] !== 0) {
                if (r < minRow) minRow = r;
                if (r > maxRow) maxRow = r;
                if (c < minCol) minCol = c;
                if (c > maxCol) maxCol = c;
            }
        }
    }
    const w = (maxCol - minCol + 1) * blockSize;
    const h = (maxRow - minRow + 1) * blockSize;
    
    return {
        x: (canvasSize - w) / 2 / blockSize - minCol,
        y: (canvasSize - h) / 2 / blockSize - minRow
    };
}


// ラインの揃いチェックと消去
function checkLines() {
    let linesToClear = [];
    
    for (let r = 0; r < ROWS; r++) {
        if (grid[r].every(value => value !== 0)) {
            linesToClear.push(r);
        }
    }
    
    if (linesToClear.length > 0) {
        linesToClear.forEach(row => {
            // 直近のブロックカラーを代表色として粒子を散らす
            const colIdx = Math.floor(COLS / 2);
            const sampleType = grid[row][colIdx] || 'I';
            const sampleColor = COLORS[sampleType].base;
            spawnLineParticles(row, sampleColor);
            
            // 該当行を消して最上部に空の行を挿入
            grid.splice(row, 1);
            grid.unshift(Array(COLS).fill(0));
        });
        
        // 得点算出 (クラシック方式倍率)
        const basePoints = [0, 100, 300, 500, 800];
        const points = basePoints[linesToClear.length] * level;
        score += points;
        linesCleared += linesToClear.length;
        
        // レベルアップ判定 (10ラインごと)
        const nextLevel = Math.floor(linesCleared / 10) + 1;
        if (nextLevel > level) {
            level = nextLevel;
            playSound('levelUp');
        } else {
            playSound('clear');
        }
        
        updateStats();
    }
}

// 統計表示更新
function updateStats() {
    document.getElementById('score').innerText = score;
    document.getElementById('level').innerText = level;
    document.getElementById('lines').innerText = linesCleared;
    
    if (score > highScor) {
        highScor = score;
        localStorage.setItem('neon_tetris_hi_score', highScor);
        document.getElementById('hi-score').innerText = highScor;
    }
    drawNexts();
}


// 難易度（落下速度）調整
function getDropInterval() {
    // レベルが上がるほど落下待機時間が減少 (最小 50ms)
    return Math.max(50, 1000 - (level - 1) * 90);
}

// メインゲームループ
function update(time = 0) {
    if (isGameOver || isPaused) return;
    
    const deltaTime = time - lastTime;
    lastTime = time;
    
    dropCounter += deltaTime;
    if (dropCounter > getDropInterval()) {
        playerDrop();
    }
    
    drawBoard();
    requestAnimationFrame(update);
}

// ゲーム開始
function startGame() {
    // 盤面リセット
    grid = Array.from({ length: ROWS }, () => Array(COLS).fill(0));
    score = 0;
    level = 1;
    linesCleared = 0;
    holdPiece = null;
    canHold = true;
    queue = [];
    isGameOver = false;
    isPaused = false;
    particles = [];
    
    refillQueue();
    currentPiece = getNextPiece();
    
    document.getElementById('hi-score').innerText = highScor;
    updateStats();
    drawHold();
    
    // UIオーバレイを非表示に
    document.getElementById('screenOverlay').classList.add('hidden');
    
    lastTime = performance.now();
    playSound('levelUp');
    update();
}

function gameOver() {
    isGameOver = true;
    playSound('gameover');
    
    document.getElementById('overlayTitle').innerText = "GAME OVER";
    document.getElementById('overlayTitle').className = "text-3xl font-black text-red-500 neon-text-pink tracking-widest mb-2";
    document.getElementById('overlayDesc').innerHTML = `スコア: <span class="text-cyan-400 font-bold text-lg">${score}</span><br>到達レベル: <span class="text-purple-400 font-bold">${level}</span><br>消去ライン数: <span class="text-yellow-400 font-bold">${linesCleared}</span>`;
    document.getElementById('startBtn').innerText = "RESTART";
    document.getElementById('screenOverlay').classList.remove('hidden');
}

function togglePause() {
    if (isGameOver) return;
    isPaused = !isPaused;
    
    const pauseBtn = document.getElementById('pauseBtn');
    if (isPaused) {
        pauseBtn.innerText = 'RESUME';
        pauseBtn.classList.add('bg-cyan-900', 'text-cyan-200');
        
        document.getElementById('overlayTitle').innerText = "PAUSED";
        document.getElementById('overlayTitle').className = "text-3xl font-black text-cyan-400 neon-text-cyan tracking-widest mb-2";
        document.getElementById('overlayDesc').innerText = "ゲームが一時停止中です";
        document.getElementById('startBtn').innerText = "RESUME GAME";
        document.getElementById('screenOverlay').classList.remove('hidden');
    } else {
        pauseBtn.innerText = 'PAUSE';
        pauseBtn.classList.remove('bg-cyan-900', 'text-cyan-200');
        document.getElementById('screenOverlay').classList.add('hidden');
        lastTime = performance.now();
        update();
    }
}


// キーボード操作
window.addEventListener('keydown', event => {
    if (isGameOver) return;
    
    switch (event.code) {
        case 'ArrowLeft':
        case 'KeyA':
            playerMove(-1);
            break;
        case 'ArrowRight':
        case 'KeyD':
            playerMove(1);
            break;
        case 'ArrowDown':
        case 'KeyS':
            playerDrop();
            break;
        case 'ArrowUp':
        case 'KeyW':
            playerRotate(1);
            break;
        case 'Space':
            playerHardDrop();
            event.preventDefault(); // スペースキーによるブラウザスクロール防止
            break;
        case 'ShiftLeft':
        case 'ShiftRight':
        case 'KeyC':
            playerHold();
            break;
        case 'KeyP':
        case 'Escape':
            togglePause();
            break;
    }
});

// UIボタンコントロール
document.getElementById('startBtn').addEventListener('click', () => {
    if (isPaused) {
        togglePause();
    } else {
        startGame();
    }
});
document.getElementById('pauseBtn').addEventListener('click', togglePause);
document.getElementById('resetBtn').addEventListener('click', () => {
    if (confirm("ゲームを新しく最初から開始しますか？")) {
        startGame();
    }
});

// 音声切り替え
document.getElementById('soundToggle').addEventListener('click', () => {
    soundEnabled = !soundEnabled;
    const soundOnIcon = document.getElementById('soundOnIcon');
    const soundOffIcon = document.getElementById('soundOffIcon');
    const soundText = document.getElementById('soundText');
    
    if (soundEnabled) {
        soundOnIcon.classList.remove('hidden');
        soundOffIcon.classList.add('hidden');
        soundText.innerText = "SOUND: ON";
        playSound('rotate');
    } else {
        soundOnIcon.classList.add('hidden');
        soundOffIcon.classList.remove('hidden');
        soundText.innerText = "SOUND: OFF";
    }
});

// モバイルバーチャルキー操作設定
const touchButtons = {
    btnLeft: () => playerMove(-1),
    btnRight: () => playerMove(1),
    btnRotate: () => playerRotate(1),
    btnSoftDrop: () => playerDrop(),
    btnHardDrop: () => playerHardDrop(),
    btnHold: () => playerHold()
};

// タッチイベント設定 (遅延を低減)
Object.entries(touchButtons).forEach(([id, handler]) => {
    const el = document.getElementById(id);
    if (el) {
        el.addEventListener('touchstart', (e) => {
            e.preventDefault();
            if (navigator.vibrate) navigator.vibrate(10); // スマホの軽微な振動フィードバック
            handler();
        }, { passive: false });
    }
});

// 初回ハイスコアロード
document.getElementById('hi-score').innerText = highScor;

// ゲームループの初期待機 (画面ロード完了)
window.onload = function() {
    drawBoard();
    drawNexts();
}
</script>
</body>
</html>
