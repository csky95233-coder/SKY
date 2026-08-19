<!DOCTYPE html>
<html lang="my">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Die Again - 100 Levels</title>

<style>
*{
  box-sizing:border-box;
  margin:0;
  padding:0;
}

body{
  background:#020305;
  color:white;
  font-family:Arial,sans-serif;
  overflow:hidden;
  touch-action:none;
}

#gameWrap{
  width:100%;
  height:100vh;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
}

#hud{
  position:absolute;
  top:15px;
  left:50%;
  transform:translateX(-50%);
  width:min(92vw,900px);
  display:flex;
  justify-content:space-between;
  align-items:center;
  z-index:5;
  font-size:14px;
}

#title{
  color:#00ffd5;
  text-shadow:0 0 8px #00ffd5;
  font-weight:bold;
}

#levelText,
#deathText{
  border:1px solid #00ffd5;
  padding:6px 10px;
  background:#031313;
  box-shadow:0 0 8px #00ffd544;
}

canvas{
  width:min(92vw,900px);
  height:auto;
  aspect-ratio:16/9;
  border:2px solid #00e6d0;
  box-shadow:
    0 0 8px #00e6d0,
    0 0 25px #00e6d033;
  background:#03040a;
}

#name{
  position:absolute;
  bottom:95px;
  color:#ff1493;
  font-size:15px;
  font-weight:bold;
  text-shadow:0 0 7px #ff1493;
}

#controls{
  position:absolute;
  bottom:25px;
  width:min(92vw,900px);
  display:flex;
  justify-content:space-between;
  align-items:center;
}

.controlGroup{
  display:flex;
  gap:15px;
}

button{
  width:55px;
  height:55px;
  border-radius:50%;
  border:1px solid #00ffd5;
  background:#020909;
  color:#00ffd5;
  font-size:22px;
  box-shadow:0 0 10px #00ffd544;
}

button:active{
  transform:scale(.92);
  background:#07302c;
}

#restart{
  position:absolute;
  top:65px;
  right:5%;
  width:auto;
  height:35px;
  border-radius:8px;
  font-size:13px;
  padding:0 12px;
}

#message{
  position:absolute;
  left:50%;
  top:50%;
  transform:translate(-50%,-50%);
  text-align:center;
  color:white;
  font-size:25px;
  font-weight:bold;
  text-shadow:0 0 12px #00ffd5;
  display:none;
  z-index:10;
}

#message small{
  display:block;
  margin-top:10px;
  color:#aaa;
  font-size:13px;
}

@media(max-width:600px){
  #hud{
    font-size:11px;
  }

  button{
    width:50px;
    height:50px;
  }

  #name{
    font-size:12px;
  }
}
</style>
</head>

<body>

<div id="gameWrap">

  <div id="hud">
    <div id="title">DIE AGAIN</div>
    <div id="levelText">LEVEL: 1 / 100</div>
    <div id="deathText">DEATHS: 0</div>
  </div>

  <button id="restart">RESTART</button>

  <canvas id="game" width="900" height="500"></canvas>

  <div id="name">ရဲမာန်အောင်စောက်ချောကြီး</div>

  <div id="message">
    LEVEL COMPLETE!
    <small>Next level...</small>
  </div>

  <div id="controls">
    <div class="controlGroup">
      <button id="left">◀</button>
      <button id="right">▶</button>
    </div>

    <button id="jump">▲</button>
  </div>

</div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const W = canvas.width;
const H = canvas.height;

let level = 1;
let deaths = 0;

let keys = {
  left:false,
  right:false
};

let jumpPressed = false;

const player = {
  x:45,
  y:420,
  w:22,
  h:28,
  vx:0,
  vy:0,
  speed:4.2,
  jump:-11,
  grounded:false
};

let platforms = [];
let spikes = [];
let goal = {};
let particles = [];

const gravity = 0.55;

/* -------------------------
   LEVEL GENERATOR
-------------------------- */

function createLevel(n){

  platforms = [];
  spikes = [];

  /*
    Same world style, but level layout changes.
    Difficulty increases from 1 to 100.
  */

  const difficulty = Math.min(1 + n * 0.045, 5.5);

  // Ground
  platforms.push({
    x:0,
    y:455,
    w:900,
    h:45
  });

  // Number of floating platforms
  const count = 3 + Math.floor(n / 10);

  let lastX = 110;

  for(let i=0;i<count;i++){

    const minGap = 45 + difficulty * 5;
    const maxGap = 100 + difficulty * 9;

    let gap =
      minGap +
      Math.random() * (maxGap-minGap);

    let x = lastX + gap;

    if(x > 750){
      x = 120 + Math.random()*500;
    }

    let width =
      Math.max(
        70,
        120 - n * 0.35 + Math.random()*35
      );

    let y =
      380 -
      i*38 -
      Math.random()*50;

    y = Math.max(180,y);

    platforms.push({
      x:x,
      y:y,
      w:width,
      h:14
    });

    lastX = x + width;
  }

  // Extra random platforms in harder levels
  if(n >= 20){

    const extra = Math.floor(n / 20);

    for(let i=0;i<extra;i++){

      platforms.push({
        x:100 + Math.random()*650,
        y:220 + Math.random()*180,
        w:65 + Math.random()*50,
        h:14
      });
    }
  }

  // Spikes
  let spikeCount =
    Math.min(
      1 + Math.floor(n / 3),
      18
    );

  for(let i=0;i<spikeCount;i++){

    let x =
      100 +
      Math.random()*700;

    // Avoid putting too many spikes at spawn
    if(x < 170) x += 150;

    spikes.push({
      x:x,
      y:441,
      w:18,
      h:14
    });
  }

  // More spikes on platforms at higher levels
  if(n >= 15){

    for(let i=0;i<Math.floor(n/8);i++){

      let p =
        platforms[
          1 + Math.floor(
            Math.random()*(platforms.length-1)
          )
        ];

      spikes.push({
        x:p.x + Math.random()*Math.max(5,p.w-20),
        y:p.y-14,
        w:18,
        h:14
      });
    }
  }

  // Goal always moves further right
  goal = {
    x:800,
    y:100,
    w:30,
    h:55
  };

  // Try to place goal above a platform
  let endPlatform =
    platforms[platforms.length-1];

  if(endPlatform){
    goal.x =
      Math.min(
        850,
        endPlatform.x + endPlatform.w/2
      );

    goal.y =
      endPlatform.y - 65;
  }

  resetPlayer();
}

/* -------------------------
   PLAYER
-------------------------- */

function resetPlayer(){

  player.x = 45;
  player.y = 420;

  player.vx = 0;
  player.vy = 0;

  player.grounded = false;
}

/* -------------------------
   INPUT
-------------------------- */

document.addEventListener("keydown",e=>{

  if(
    e.key==="ArrowLeft" ||
    e.key.toLowerCase()==="a"
  ){
    keys.left=true;
  }

  if(
    e.key==="ArrowRight" ||
    e.key.toLowerCase()==="d"
  ){
    keys.right=true;
  }

  if(
    e.key==="ArrowUp" ||
    e.key===" " ||
    e.key.toLowerCase()==="w"
  ){
    jumpPressed=true;
  }
});

document.addEventListener("keyup",e=>{

  if(
    e.key==="ArrowLeft" ||
    e.key.toLowerCase()==="a"
  ){
    keys.left=false;
  }

  if(
    e.key==="ArrowRight" ||
    e.key.toLowerCase()==="d"
  ){
    keys.right=false;
  }
});

/* Mobile buttons */

function holdButton(btn,down,up){

  btn.addEventListener("touchstart",e=>{
    e.preventDefault();
    down();
  });

  btn.addEventListener("touchend",e=>{
    e.preventDefault();
    up();
  });

  btn.addEventListener("mousedown",down);
  btn.addEventListener("mouseup",up);
  btn.addEventListener("mouseleave",up);
}

holdButton(
  document.getElementById("left"),
  ()=>keys.left=true,
  ()=>keys.left=false
);

holdButton(
  document.getElementById("right"),
  ()=>keys.right=true,
  ()=>keys.right=false
);

const jumpBtn =
  document.getElementById("jump");

jumpBtn.addEventListener("touchstart",e=>{
  e.preventDefault();
  jumpPressed=true;
});

jumpBtn.addEventListener("mousedown",()=>{
  jumpPressed=true;
});

document.getElementById("restart")
.addEventListener("click",()=>{
  createLevel(level);
});

/* -------------------------
   COLLISION
-------------------------- */

function rectCollision(a,b){

  return (
    a.x < b.x+b.w &&
    a.x+a.w > b.x &&
    a.y < b.y+b.h &&
    a.y+a.h > b.y
  );
}

function update(){

  /* Horizontal movement */

  if(keys.left){
    player.vx = -player.speed;
  }
  else if(keys.right){
    player.vx = player.speed;
  }
  else{
    player.vx *= 0.78;
  }

  player.x += player.vx;

  /* Screen boundaries */

  if(player.x < 0)
    player.x = 0;

  if(player.x+player.w > W)
    player.x = W-player.w;

  /* Jump */

  if(
    jumpPressed &&
    player.grounded
  ){
    player.vy = player.jump;
    player.grounded = false;
  }

  jumpPressed=false;

  /* Gravity */

  player.vy += gravity;

  let oldY = player.y;

  player.y += player.vy;

  player.grounded=false;

  /* Platform collision */

  for(const p of platforms){

    if(
      player.x+player.w > p.x &&
      player.x < p.x+p.w &&
      oldY+player.h <= p.y &&
      player.y+player.h >= p.y &&
      player.vy >= 0
    ){

      player.y =
        p.y-player.h;

      player.vy=0;
      player.grounded=true;
    }
  }

  /* Spikes */

  for(const s of spikes){

    if(rectCollision(player,s)){
      die();
      return;
    }
  }

  /* Falling */

  if(player.y > H+80){
    die();
    return;
  }

  /* Goal */

  if(rectCollision(player,goal)){
    completeLevel();
  }

  updateParticles();
}

/* -------------------------
   DEATH
-------------------------- */

function die(){

  deaths++;

  burst(
    player.x+player.w/2,
    player.y+player.h/2
  );

  document.getElementById("deathText")
    .textContent =
    "DEATHS: " + deaths;

  setTimeout(()=>{
    resetPlayer();
  },150);
}

/* -------------------------
   LEVEL COMPLETE
-------------------------- */

let changing=false;

function completeLevel(){

  if(changing) return;

  changing=true;

  if(level >= 100){

    showMessage(
      "🏆 YOU BEAT ALL 100 LEVELS!",
      "ရဲမာန်အောင်စောက်ချောကြီး"
    );

    return;
  }

  showMessage(
    "LEVEL " + level + " COMPLETE!",
    "Loading Level " + (level+1) + "..."
  );

  setTimeout(()=>{

    level++;

    document.getElementById("levelText")
      .textContent =
      "LEVEL: " + level + " / 100";

    createLevel(level);

    hideMessage();

    changing=false;

  },1000);
}

/* -------------------------
   MESSAGE
-------------------------- */

function showMessage(big,small){

  const msg =
    document.getElementById("message");

  msg.innerHTML =
    big +
    "<small>"+small+"</small>";

  msg.style.display="block";
}

function hideMessage(){

  document.getElementById("message")
    .style.display="none";
}

/* -------------------------
   PARTICLES
-------------------------- */

function burst(x,y){

  for(let i=0;i<18;i++){

    particles.push({
      x:x,
      y:y,
      vx:(Math.random()-.5)*5,
      vy:(Math.random()-.5)*5,
      life:30
    });
  }
}

function updateParticles(){

  for(let i=particles.length-1;i>=0;i--){

    let p=particles[i];

    p.x += p.vx;
    p.y += p.vy;

    p.life--;

    if(p.life<=0){
      particles.splice(i,1);
    }
  }
}

/* -------------------------
   DRAW BACKGROUND
-------------------------- */

function drawBackground(){

  ctx.fillStyle="#03040a";
  ctx.fillRect(0,0,W,H);

  // Grid
  ctx.strokeStyle="#10162a";
  ctx.lineWidth=1;

  for(let x=0;x<W;x+=30){

    ctx.beginPath();
    ctx.moveTo(x,0);
    ctx.lineTo(x,H);
    ctx.stroke();
  }

  for(let y=0;y<H;y+=30){

    ctx.beginPath();
    ctx.moveTo(0,y);
    ctx.lineTo(W,y);
    ctx.stroke();
  }

  // Glow lines
  ctx.strokeStyle="#071e24";

  for(let y=0;y<H;y+=90){

    ctx.beginPath();
    ctx.moveTo(0,y);
    ctx.lineTo(W,y);
    ctx.stroke();
  }
}

/* -------------------------
   DRAW PLATFORMS
-------------------------- */

function drawPlatforms(){

  for(const p of platforms){

    ctx.save();

    ctx.fillStyle="#0b1020";
    ctx.strokeStyle="#39445e";
    ctx.lineWidth=2;

    ctx.shadowColor="#273b66";
    ctx.shadowBlur=7;

    ctx.fillRect(
      p.x,p.y,p.w,p.h
    );

    ctx.strokeRect(
      p.x,p.y,p.w,p.h
    );

    ctx.restore();
  }
}

/* -------------------------
   DRAW SPIKES
-------------------------- */

function drawSpikes(){

  for(const s of spikes){

    ctx.save();

    ctx.fillStyle="#ff0055";

    ctx.shadowColor="#ff0055";
    ctx.shadowBlur=12;

    ctx.beginPath();

    ctx.moveTo(s.x,s.y+s.h);
    ctx.lineTo(
      s.x+s.w/2,
      s.y
    );
    ctx.lineTo(
      s.x+s.w,
      s.y+s.h
    );

    ctx.closePath();

    ctx.fill();

    ctx.restore();
  }
}

/* -------------------------
   DRAW GOAL
-------------------------- */

function drawGoal(){

  ctx.save();

  ctx.fillStyle="#fff0a8";

  ctx.shadowColor="#ffe76a";
  ctx.shadowBlur=20;

  ctx.fillRect(
    goal.x,
    goal.y,
    goal.w,
    goal.h
  );

  ctx.restore();
}

/* -------------------------
   DRAW PLAYER
-------------------------- */

function drawPlayer(){

  ctx.save();

  // Green square character
  ctx.fillStyle="#00ffcc";

  ctx.shadowColor="#00ffcc";
  ctx.shadowBlur=15;

  ctx.fillRect(
    player.x,
    player.y,
    player.w,
    player.h
  );

  // Eyes
  ctx.shadowBlur=0;
  ctx.fillStyle="#021515";

  ctx.fillRect(
    player.x+5,
    player.y+7,
    4,
    4
  );

  ctx.fillRect(
    player.x+14,
    player.y+7,
    4,
    4
  );

  ctx.restore();
}

/* -------------------------
   DRAW PARTICLES
-------------------------- */

function drawParticles(){

  for(const p of particles){

    ctx.globalAlpha =
      p.life/30;

    ctx.fillStyle="#00ffd5";

    ctx.fillRect(
      p.x,
      p.y,
      3,
      3
    );
  }

  ctx.globalAlpha=1;
}

/* -------------------------
   DRAW
-------------------------- */

function draw(){

  drawBackground();
  drawPlatforms();
  drawSpikes();
  drawGoal();
  drawPlayer();
  drawParticles();
}

/* -------------------------
   GAME LOOP
-------------------------- */

function loop(){

  update();
  draw();

  requestAnimationFrame(loop);
}

/* Start */

createLevel(1);
loop();

</script>

</body>
</html>
