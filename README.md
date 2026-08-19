<!DOCTYPE html>
<html lang="my">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Die Again Style Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            -webkit-user-select: none;
        }
        body {
            background-color: #111;
            color: #fff;
            font-family: sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            overflow: hidden;
        }
        #game-container {
            position: relative;
            width: 100vw;
            max-width: 800px;
            height: 60vh;
            max-height: 450px;
            background: #222;
            border: 2px solid #555;
        }
        canvas {
            width: 100%;
            height: 100%;
            display: block;
        }
        #ui {
            position: absolute;
            top: 10px;
            left: 10px;
            font-size: 14px;
            background: rgba(0,0,0,0.6);
            padding: 6px 12px;
            border-radius: 4px;
            pointer-events: none;
        }
        /* Phone Touch Controls */
        #controls {
            width: 100vw;
            max-width: 800px;
            height: 25vh;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background: #111;
        }
        .btn-group {
            display: flex;
            gap: 15px;
        }
        .btn {
            width: 60px;
            height: 60px;
            background: #444;
            color: white;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            font-weight: bold;
            active-bg: #666;
        }
        .btn:active {
            background: #777;
        }
    </style>
</head>
<body>

<div id="game-container">
    <div id="ui">Deaths: <span id="death-count">0</span> | Goal: Reach the Gold Door!</div>
    <canvas id="gameCanvas"></canvas>
</div>

<!-- Touch Controls for Mobile -->
<div id="controls">
    <div class="btn-group">
        <div class="btn" id="btn-left">◄</div>
        <div class="btn" id="btn-right">►</div>
    </div>
    <div class="btn-group">
        <div class="btn" id="btn-jump">▲</div>
    </div>
</div>

<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

canvas.width = 800;
canvas.height = 450;

let deaths = 0;
let deadBodies = []; // သေဆုံးသွားသော ခန္ဓာကိုယ်များ (အတားအဆီးအဖြစ် သုံးရန်)

const gravity = 0.5;

// Player
const player = {
    x: 50,
    y: 300,
    width: 20,
    height: 30,
    vx: 0,
    vy: 0,
    speed: 4,
    jumpPower: -10,
    grounded: false
};

// Level Design
const platforms = [
    { x: 0, y: 400, width: 800, height: 50 }, // Ground
    { x: 200, y: 320, width: 100, height: 20 },
    { x: 400, y: 240, width: 100, height: 20 },
    { x: 600, y: 160, width: 120, height: 20 }
];

const spikes = [
    { x: 250, y: 380, width: 40, height: 20 },
    { x: 450, y: 380, width: 60, height: 20 }
];

const goal = { x: 700, y: 100, width: 40, height: 60 };

// Controls State
const keys = { left: false, right: false, up: false };

// Keyboard Control
window.addEventListener("keydown", e => {
    if (e.key === "ArrowLeft" || e.key === "a") keys.left = true;
    if (e.key === "ArrowRight" || e.key === "d") keys.right = true;
    if (e.key === "ArrowUp" || e.key === "w" || e.key === " ") keys.up = true;
});

window.addEventListener("keyup", e => {
    if (e.key === "ArrowLeft" || e.key === "a") keys.left = false;
    if (e.key === "ArrowRight" || e.key === "d") keys.right = false;
    if (e.key === "ArrowUp" || e.key === "w" || e.key === " ") keys.up = false;
});

// Touch Controls for Mobile Screen
function setupTouch(id, keyName) {
    const btn = document.getElementById(id);
    btn.addEventListener("touchstart", (e) => { e.preventDefault(); keys[keyName] = true; });
    btn.addEventListener("touchend", (e) => { e.preventDefault(); keys[keyName] = false; });
    btn.addEventListener("mousedown", () => { keys[keyName] = true; });
    btn.addEventListener("mouseup", () => { keys[keyName] = false; });
}
setupTouch("btn-left", "left");
setupTouch("btn-right", "right");
setupTouch("btn-jump", "up");

function killPlayer() {
    // သေသွားပါက ခန္ဓာကိုယ်အလောင်း ကျန်ခဲ့မည်
    deadBodies.push({ x: player.x, y: player.y, width: player.width, height: player.height });
    deaths++;
    document.getElementById("death-count").innerText = deaths;
    
    // Respawn
    player.x = 50;
    player.y = 300;
    player.vx = 0;
    player.vy = 0;
}

function update() {
    // Movement
    if (keys.left) player.vx = -player.speed;
    else if (keys.right) player.vx = player.speed;
    else player.vx = 0;

    if (keys.up && player.grounded) {
        player.vy = player.jumpPower;
        player.grounded = false;
    }

    player.vy += gravity;
    player.x += player.vx;
    player.y += player.vy;

    player.grounded = false;

    // Platform Collision (รวม အလောင်းများပါ ခုန်ကူးရန် အကာအကွယ်အဖြစ် သုံးနိုင်သည်)
    const allPlatforms = [...platforms, ...deadBodies];
    
    allPlatforms.forEach(p => {
        if (player.x < p.x + p.width &&
            player.x + player.width > p.x &&
            player.y < p.y + p.height &&
            player.y + player.height > p.y) {
            
            // Colliding from top
            if (player.vy > 0 && player.y + player.height - player.vy <= p.y + 10) {
                player.y = p.y - player.height;
                player.vy = 0;
                player.grounded = true;
            }
        }
    });

    // Spike Collision (Spike ထိရင် သေမည်)
    spikes.forEach(s => {
        if (player.x < s.x + s.width &&
            player.x + player.width > s.x &&
            player.y < s.y + s.height &&
            player.y + player.height > s.y) {
            killPlayer();
        }
    });

    // Check Goal
    if (player.x < goal.x + goal.width &&
        player.x + player.width > goal.x &&
        player.y < goal.y + goal.height &&
        player.y + player.height > goal.y) {
        alert("ဂိမ်းနိုင်သွားပါပြီ! Total Deaths: " + deaths);
        deadBodies = [];
        deaths = 0;
        document.getElementById("death-count").innerText = deaths;
        player.x = 50;
        player.y = 300;
    }

    // Out of bounds
    if (player.y > canvas.height) {
        killPlayer();
    }
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Draw Platforms
    ctx.fillStyle = "#666";
    platforms.forEach(p => ctx.fillRect(p.x, p.y, p.width, p.height));

    // Draw Spikes (ဆူးများ)
    ctx.fillStyle = "#e74c3c";
    spikes.forEach(s => {
        ctx.beginPath();
        ctx.moveTo(s.x, s.y + s.height);
        ctx.lineTo(s.x + s.width / 2, s.y);
        ctx.lineTo(s.x + s.width + s.width/2, s.y + s.height);
        ctx.fill();
    });

    // Draw Dead Bodies (သေဆုံးသွားသော ခန္ဓာကိုယ်များ - အနီရောင်ဖျော့ဖျော့)
    ctx.fillStyle = "#95a5a6";
    deadBodies.forEach(b => ctx.fillRect(b.x, b.y, b.width, b.height));

    // Draw Goal (ပန်းတိုင်)
    ctx.fillStyle = "#f1c40f";
    ctx.fillRect(goal.x, goal.y, goal.width, goal.height);

    // Draw Player (ကစားသမား - အပြာရောင်)
    ctx.fillStyle = "#3498db";
    ctx.fillRect(player.x, player.y, player.width, player.height);
}

function loop() {
    update();
    draw();
    requestAnimationFrame(loop);
}

loop();
</script>
</body>
</html>
