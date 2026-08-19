<!DOCTYPE html>
<html lang="my">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SKY - Level Game</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #05050a; color: #00f3ff; font-family: monospace; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; overflow: hidden; }
        #game-box { position: relative; width: 100vw; max-width: 400px; height: 60vh; background: #0d0f18; border: 2px solid #00f3ff; overflow: hidden; }
        .bg-watermark { position: absolute; top: 30%; left: 50%; transform: translate(-50%, -50%); font-size: 20px; font-weight: bold; color: rgba(0, 243, 255, 0.2); text-align: center; pointer-events: none; z-index: 1; }
        #ui { position: absolute; top: 10px; left: 10px; z-index: 10; font-size: 16px; background: rgba(0,0,0,0.8); padding: 5px; border: 1px solid #00f3ff; }
        #player { position: absolute; width: 20px; height: 20px; background: #00f3ff; z-index: 5; }
        #door { position: absolute; width: 25px; height: 30px; background: #ffe600; z-index: 3; }
        #controls { display: flex; gap: 20px; margin-top: 20px; }
        .btn { width: 70px; height: 70px; border-radius: 50%; border: 2px solid #00f3ff; background: transparent; color: #00f3ff; font-size: 20px; }
    </style>
</head>
<body>
    <div id="game-box">
        <div id="ui">LEVEL: <span id="lvl-display">1</span>/100</div>
        <div class="bg-watermark">ရဲမာန်အောင်<br>စောက်ချောကြီး</div>
        <div id="player"></div>
        <div id="door"></div>
    </div>
    <div id="controls">
        <button class="btn" onclick="move(-15)">◀</button>
        <button class="btn" onclick="jump()">▲</button>
        <button class="btn" onclick="move(15)">▶</button>
    </div>

    <script>
        let player = document.getElementById('player');
        let door = document.getElementById('door');
        let lvlDisplay = document.getElementById('lvl-display');
        let level = 1;
        let px = 50, py = 300, vy = 0;

        function updateLevelUI() {
            lvlDisplay.textContent = level;
            // တံခါးကို နေရာရွှေ့ပေးခြင်း
            door.style.left = (Math.min(350, 100 + (level * 5))) + 'px';
            door.style.top = '320px';
        }

        function move(dir) { px += dir; player.style.left = px + 'px'; checkWin(); }
        function jump() { if(py >= 300) vy = -15; }
        
        function update() {
            vy += 0.8; py += vy;
            if (py >= 300) { py = 300; vy = 0; }
            player.style.left = px + 'px';
            player.style.top = py + 'px';
            requestAnimationFrame(update);
        }

        function checkWin() {
            let doorX = parseInt(door.style.left || 100);
            if (px > doorX - 30 && px < doorX + 30) {
                if(level < 100) {
                    level++;
                    updateLevelUI();
                    px = 50; 
                } else {
                    alert("Game Completed!");
                }
            }
        }

        updateLevelUI();
        update();
    </script>
</body>
</html>
