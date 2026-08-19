from pathlib import Path

html = r'''<!DOCTYPE html>
<html lang="my">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">
<title>SKY Die</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
html,body{width:100%;height:100%;overflow:hidden;background:#020305;font-family:Arial,sans-serif}
body{display:flex;align-items:center;justify-content:center;color:#fff;touch-action:none}
#app{width:100%;height:100%;display:flex;flex-direction:column;align-items:center;justify-content:center}
#hud{position:absolute;top:14px;width:min(92vw,900px);display:flex;justify-content:space-between;align-items:center;z-index:5;font-size:13px}
.badge{border:1px solid #00ffd5;background:#031313;padding:6px 9px;box-shadow:0 0 8px #00ffd544}
#title{color:#00ffd5;font-weight:bold;text-shadow:0 0 8px #00ffd5}
#game{width:min(92vw,900px);height:auto;aspect-ratio:16/9;border:2px solid #00e6d0;box-shadow:0 0 10px #00e6d0,0 0 28px #00e6d033;background:#03040a}
#name{position:absolute;bottom:92px;color:#ff1493;font-size:14px;font-weight:bold;text-shadow:0 0 8px #ff1493}
#controls{position:absolute;bottom:22px;width:min(92vw,900px);display:flex;justify-content:space-between}
.group{display:flex;gap:14px}
button{width:56px;height:56px;border-radius:50%;border:1px solid #00ffd5;background:#020909;color:#00ffd5;font-size:23px;box-shadow:0 0 10px #00ffd544}
button:active{transform:scale(.92);background:#07302c}
#restart{position:absolute;top:54px;right:5%;width:auto;height:34px;border-radius:8px;padding:0 12px;font-size:12px}
#overlay{position:absolute;inset:0;display:none;align-items:center;justify-content:center;z-index:10;background:#0008;text-align:center}
.card{border:1px solid #00ffd5;background:#050b0d;padding:24px 30px;box-shadow:0 0 25px #00ffd555}
.card h1{color:#00ffd5;text-shadow:0 0 10px #00ffd5;font-size:25px}
.card p{margin-top:9px;color:#ddd;font-size:14px}
@media(max-width:600px){
 #hud{font-size:10px}.badge{padding:5px 7px}
 button{width:52px;height:52px}
 #name{font-size:11px}
}
</style>
</head>
<body>
<div id="app">
  <div id="hud">
    <div id="title">SKY DIE</div>
    <div class="badge" id="level">LEVEL: 1 / 100</div>
    <div class="badge" id="deaths">DEATHS: 0</div>
  </div>

  <button id="restart">RESTART</button>
  <canvas id="game" width="900" height="500"></canvas>
  <div id="name">ရဲမာန်အောင်စောက်ချောကြီး</div>

  <div id="overlay">
    <div class="card">
      <h1 id="msgTitle">LEVEL COMPLETE!</h1>
      <p id="msgText">Next level...</p>
    </div>
  </div>

  <div id="controls">
    <div class="group">
      <button id="left">◀</button>
      <button id="right">▶</button>
    </div>
    <button id="jump">▲</button>
  </div>
</div>

<script>
const canvas=document.getElementById("game"),ctx=canvas.getContext("2d");
const W=900,H=500;
let level=1,deaths=0,changing=false;
let keys={left:false,right:false},jumpPressed=false;
let platforms=[],spikes=[],goal={},particles=[];

const player={x:45,y:420,w:22,h:28,vx:0,vy:0,speed:4.2,jump:-11,grounded:false};
const gravity=.55;

function resetPlayer(){
  player.x=45;player.y=420;player.vx=0;player.vy=0;player.grounded=false;
}

function createLevel(n){
  platforms=[];spikes=[];
  const d=Math.min(1+n*.045,5.5);
  platforms.push({x:0,y:455,w:900,h:45});

  const count=3+Math.floor(n/10);
  let lastX=105;
  for(let i=0;i<count;i++){
    const gap=45+d*5+Math.random()*(55+d*9);
    let x=lastX+gap;
    if(x>760)x=110+Math.random()*500;
    const w=Math.max(65,120-n*.35+Math.random()*35);
    let y=380-i*38-Math.random()*50;
    y=Math.max(175,y);
    platforms.push({x,y,w,h:14});
    lastX=x+w;
  }

  if(n>=20){
    for(let i=0;i<Math.floor(n/20);i++)
      platforms.push({x:100+Math.random()*650,y:220+Math.random()*180,w:65+Math.random()*50,h:14});
  }

  const spikeCount=Math.min(1+Math.floor(n/3),18);
  for(let i=0;i<spikeCount;i++){
    let x=100+Math.random()*700;
    if(x<170)x+=150;
    spikes.push({x,y:441,w:18,h:14});
  }

  if(n>=15){
    for(let i=0;i<Math.floor(n/8);i++){
      const p=platforms[1+Math.floor(Math.random()*(platforms.length-1))];
      spikes.push({x:p.x+Math.random()*Math.max(5,p.w-20),y:p.y-14,w:18,h:14});
    }
  }

  const end=platforms[platforms.length-1];
  goal={x:800,y:100,w:30,h:55};
  if(end){goal.x=Math.min(850,end.x+end.w/2);goal.y=end.y-65}
  resetPlayer();
}

function rect(a,b){
  return a.x<b.x+b.w&&a.x+a.w>b.x&&a.y<b.y+b.h&&a.y+a.h>b.y;
}

function die(){
  deaths++;
  document.getElementById("deaths").textContent="DEATHS: "+deaths;
  burst(player.x+11,player.y+14);
  setTimeout(resetPlayer,120);
}

function complete(){
  if(changing)return;
  changing=true;
  if(level>=100){
    show("🏆 YOU BEAT ALL 100 LEVELS!","SKY Die — ရဲမာန်အောင်စောက်ချောကြီး");
    return;
  }
  show("LEVEL "+level+" COMPLETE!","Loading Level "+(level+1)+"...");
  setTimeout(()=>{
    level++;
    document.getElementById("level").textContent="LEVEL: "+level+" / 100";
    createLevel(level);
    hide();
    changing=false;
  },850);
}

function show(a,b){
  document.getElementById("msgTitle").textContent=a;
  document.getElementById("msgText").textContent=b;
  document.getElementById("overlay").style.display="flex";
}
function hide(){document.getElementById("overlay").style.display="none"}

function update(){
  if(keys.left)player.vx=-player.speed;
  else if(keys.right)player.vx=player.speed;
  else player.vx*=.78;

  player.x+=player.vx;
  player.x=Math.max(0,Math.min(W-player.w,player.x));

  if(jumpPressed&&player.grounded){player.vy=player.jump;player.grounded=false}
  jumpPressed=false;

  const oldY=player.y;
  player.vy+=gravity;
  player.y+=player.vy;
  player.grounded=false;

  for(const p of platforms){
    if(player.x+player.w>p.x&&player.x<p.x+p.w&&oldY+player.h<=p.y&&player.y+player.h>=p.y&&player.vy>=0){
      player.y=p.y-player.h;player.vy=0;player.grounded=true;
    }
  }

  for(const s of spikes)if(rect(player,s)){die();return}
  if(player.y>H+80){die();return}
  if(rect(player,goal))complete();
  updateParticles();
}

function burst(x,y){
  for(let i=0;i<18;i++)particles.push({x,y,vx:(Math.random()-.5)*5,vy:(Math.random()-.5)*5,life:30});
}
function updateParticles(){
  for(let i=particles.length-1;i>=0;i--){
    const p=particles[i];p.x+=p.vx;p.y+=p.vy;p.life--;
    if(p.life<=0)particles.splice(i,1);
  }
}

function draw(){
  ctx.fillStyle="#03040a";ctx.fillRect(0,0,W,H);
  ctx.strokeStyle="#10162a";ctx.lineWidth=1;
  for(let x=0;x<W;x+=30){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,H);ctx.stroke()}
  for(let y=0;y<H;y+=30){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(W,y);ctx.stroke()}

  for(const p of platforms){
    ctx.save();ctx.fillStyle="#0b1020";ctx.strokeStyle="#39445e";ctx.lineWidth=2;
    ctx.shadowColor="#273b66";ctx.shadowBlur=7;
    ctx.fillRect(p.x,p.y,p.w,p.h);ctx.strokeRect(p.x,p.y,p.w,p.h);ctx.restore();
  }

  for(const s of spikes){
    ctx.save();ctx.fillStyle="#ff0055";ctx.shadowColor="#ff0055";ctx.shadowBlur=12;
    ctx.beginPath();ctx.moveTo(s.x,s.y+s.h);ctx.lineTo(s.x+s.w/2,s.y);ctx.lineTo(s.x+s.w,s.y+s.h);ctx.closePath();ctx.fill();ctx.restore();
  }

  ctx.save();ctx.fillStyle="#fff0a8";ctx.shadowColor="#ffe76a";ctx.shadowBlur=20;
  ctx.fillRect(goal.x,goal.y,goal.w,goal.h);ctx.restore();

  ctx.save();ctx.fillStyle="#00ffcc";ctx.shadowColor="#00ffcc";ctx.shadowBlur=15;
  ctx.fillRect(player.x,player.y,player.w,player.h);
  ctx.shadowBlur=0;ctx.fillStyle="#021515";
  ctx.fillRect(player.x+5,player.y+7,4,4);ctx.fillRect(player.x+14,player.y+7,4,4);ctx.restore();

  for(const p of particles){ctx.globalAlpha=p.life/30;ctx.fillStyle="#00ffd5";ctx.fillRect(p.x,p.y,3,3)}
  ctx.globalAlpha=1;
}

function loop(){update();draw();requestAnimationFrame(loop)}

function bindHold(el,down,up){
  el.addEventListener("touchstart",e=>{e.preventDefault();down()},{passive:false});
  el.addEventListener("touchend",e=>{e.preventDefault();up()},{passive:false});
  el.addEventListener("mousedown",down);el.addEventListener("mouseup",up);el.addEventListener("mouseleave",up);
}
bindHold(left,()=>keys.left=true,()=>keys.left=false);
bindHold(right,()=>keys.right=true,()=>keys.right=false);
document.getElementById("jump").addEventListener("touchstart",e=>{e.preventDefault();jumpPressed=true},{passive:false});
document.getElementById("jump").addEventListener("mousedown",()=>jumpPressed=true);

document.addEventListener("keydown",e=>{
  if(e.key==="ArrowLeft"||e.key.toLowerCase()==="a")keys.left=true;
  if(e.key==="ArrowRight"||e.key.toLowerCase()==="d")keys.right=true;
  if(e.key==="ArrowUp"||e.key===" "||e.key.toLowerCase()==="w")jumpPressed=true;
});
document.addEventListener("keyup",e=>{
  if(e.key==="ArrowLeft"||e.key.toLowerCase()==="a")keys.left=false;
  if(e.key==="ArrowRight"||e.key.toLowerCase()==="d")keys.right=false;
});
document.getElementById("restart").onclick=()=>{createLevel(level);hide()};

createLevel(1);loop();
</script>
</body>
</html>'''

path = Path("/mnt/data/SKY_Die.html")
path.write_text(html, encoding="utf-8")
print(f"Created: {path}")
