<!doctype html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title> precious- mooffy</title>
  <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;800&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg: linear-gradient(135deg,#FFF6F9 0%, #F7FBFF 100%);
      --card:#fff; --accent:#ff6b9a; --accent-2:#ffb3d1; --muted:#6b6b7a;
      --shadow:0 8px 30px rgba(32,32,50,0.08);
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:'Kanit',system-ui,-apple-system,Segoe UI,Roboto,Arial}
    body{
      background:var(--bg);display:flex;align-items:center;justify-content:center;overflow:hidden;padding:20px;
    }
    .stage{
      width:980px;max-width:96vw;height:640px;max-height:92vh;border-radius:18px;padding:22px;
      display:grid;grid-template-columns:1fr 360px;gap:18px;background:linear-gradient(180deg,#fff,#fff6fb);
      box-shadow:var(--shadow);position:relative;overflow:hidden;
    }
    .left{
      background:linear-gradient(180deg,#fff,#fff9fb);border-radius:12px;padding:16px;position:relative;
    }
    .title{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
    .chip{background:linear-gradient(90deg,var(--accent),var(--accent-2));color:#fff;padding:8px 12px;border-radius:999px;font-weight:700}
    .card{
      width:420px;height:300px;border-radius:14px;padding:16px;background:linear-gradient(180deg,#fff,#fff0f6);
      box-shadow:0 12px 30px rgba(32,32,50,0.08);position:absolute;left:24px;top:64px;cursor:grab;user-select:none;
      transition:transform .25s cubic-bezier(.2,.9,.3,1),box-shadow .2s;
    }
    .card:active{cursor:grabbing;transform:scale(.995)}
    .avatar{width:84px;height:84px;border-radius:12px;background:linear-gradient(135deg,#ffd1e1,#ffb3d1);display:flex;align-items:center;justify-content:center;font-weight:800;font-size:34px;color:#fff;float:left;margin-right:12px;box-shadow:0 8px 24px rgba(255,107,154,0.18)}
    .name{font-weight:800;font-size:20px;margin-top:8px}
    .sub{color:var(--muted);font-size:13px}
    .meter{display:flex;gap:8px;align-items:center;margin-top:10px}
    .bar{height:10px;border-radius:999px;background:linear-gradient(90deg,#ffd2e5,#ff9abf);flex:1;box-shadow:inset 0 -6px 12px rgba(255,255,255,0.18)}
    .actions{position:absolute;bottom:16px;left:16px;display:flex;gap:10px}
    .btn{padding:10px 14px;border-radius:12px;border:0;background:#fff;cursor:pointer;font-weight:700;box-shadow:0 8px 22px rgba(32,32,50,0.06);transition:transform .18s}
    .btn.primary{background:linear-gradient(90deg,var(--accent),var(--accent-2));color:#fff}
    .zone{position:absolute;right:20px;top:32px;width:220px;height:220px;border-radius:12px;padding:12px;background:linear-gradient(180deg,#fff7fb,#fff);box-shadow:0 12px 30px rgba(32,32,50,0.04)}
    .panel{padding:16px;border-radius:12px;background:linear-gradient(180deg,#fff,#fffafc);box-shadow:0 8px 24px rgba(32,32,50,0.04);display:flex;flex-direction:column;gap:12px}
    .message{flex:1;padding:12px;border-radius:10px;background:linear-gradient(180deg,#fff,#fff7fb);border:1px solid rgba(230,230,240,0.8);overflow:auto}
    .typewriter{white-space:pre-wrap;font-size:16px;line-height:1.6;color:#222;min-height:180px}
    .controls{display:flex;gap:8px;justify-content:flex-end}
    canvas{position:fixed;left:0;top:0;pointer-events:none;z-index:9999}
    @media (max-width:880px){.stage{grid-template-columns:1fr;grid-auto-rows:auto;height:auto}.card{position:relative;left:0;top:0;margin-bottom:14px}.zone{position:static;width:100%;height:120px}}
  </style>
</head>
<body>
  <div class="stage" aria-label="เกมน่ารัก — พี่ครับ">
    <div class="left">
      <div class="title">
        <h3 style="margin:0">🎮 เกมน่ารัก</h3>
        <div class="chip" id="statusChip">สถานะ: ยังไม่ระบุ</div>
      </div>

      <div class="card" id="draggable" tabindex="0" aria-grabbed="false">
        <div class="avatar">คุณ</div>
        <div style="overflow:hidden">
          <div class="name">การ์ดของพี่ครับ</div>
          <div class="sub">ลากหรือกดปุ่มด้านล่างเพื่อดูเอฟเฟกต์</div>
          <div class="meter" style="margin-top:10px">
            <div class="bar" id="affectionBar" style="width:40%"></div>
            <div style="font-size:18px;color:var(--accent)">💗</div>
          </div>
        </div>

        <div class="actions">
          <button class="btn primary" id="sendButton">แสดงข้อความ</button>
          <button class="btn" id="petButton">โอ๋</button>
        </div>
      </div>

      <div class="zone" aria-label="มินิเกม">
        <h4 style="margin:0 0 8px 0">มินิเกม: หัวใจ</h4>
        <div style="display:flex;align-items:center;gap:8px">
          <div style="font-size:22px">💖</div>
          <div style="font-weight:800" id="collectedCount">0</div>
        </div>
        <div style="margin-top:10px;font-size:13px;color:var(--muted)">
          ปล่อยหัวใจแล้วแตะเพื่อเก็บ
        </div>
        <div style="margin-top:10px;display:flex;gap:8px">
          <button class="btn" id="spawnHearts">ปล่อยหัวใจ</button>
          <button class="btn" id="clearHearts">เก็บทั้งหมด</button>
        </div>
      </div>
    </div>

    <aside class="panel">
      <h3 style="margin:0">ข้อความถึงพี่</h3>
      <div class="message" id="messageBox" role="region" aria-live="polite">
        <div class="typewriter" id="typewriter"></div>
      </div>

      <div class="controls">
        <button class="btn" id="revealBtn">ไทป์เร็ว</button>
        <button class="btn primary" id="agreeBtn">แสดงสถานะ</button>
        <button class="btn" id="surpriseBtn">เซอร์ไพรส์</button>
      </div>
    </aside>
  </div>

  <canvas id="fxCanvas"></canvas>

  <script>
    /* ข้อความเจ๋ง ๆ เวอร์ชันพี่ครับ */
    const messageText = 
`ชิง ๆ หราน ๆ ของ่มั้ยครับ
ว่าตั้งแต่มีพี่อยู่
ทุกวันของธิมันสดใส และ แฮปปี้ ขึ้นเยอะเลยครับ  

ธิอาจจะไม่เก่งเรื่องใช้พูดตรง ๆ  หรือ ไม่เด่งมากในบางเรื่อง
แต่ในใจมันเต็มไปด้วยความรู้สึกดี ๆ ให้พี่เสมอเลยครับ  

ธิอยากเป็นคนที่อยู่ข้าง ๆ พี่ คอยโอ๋ คอยปลอบ
และอยากเห็นพี่ยิ้มได้ในทุก ๆ วันนะครับ  

ถึงบางทีจะมีช่วงที่เงียบหาย  
แต่ความรู้สึกที่มีให้พี่ ไม่เคยลดลงเลยครับ  

ถ้าเป็นไปได้ก็อยาก
ขอให้มีพี่อยู่ด้วยกันไปนาน ๆ นะครับ 💖`;

    /* Canvas setup */
    const canvas = document.getElementById('fxCanvas'), ctx = canvas.getContext('2d');
    function fit(){ canvas.width = innerWidth; canvas.height = innerHeight; }
    addEventListener('resize', fit); fit();

    /* Typewriter */
    const typeEl = document.getElementById('typewriter');
    let typingTimer;
    function typeWriter(text, speed=30){
      clearTimeout(typingTimer);
      typeEl.textContent = '';
      let i=0;
      (function step(){
        if(i<=text.length){
          typeEl.textContent = text.slice(0,i);
          i++;
          typingTimer = setTimeout(step, speed + Math.random()*20);
        }
      })();
    }

    /* Draggable card */
    const card = document.getElementById('draggable');
    let dragging=false, sx=0, sy=0, ox=0, oy=0;
    card.addEventListener('pointerdown', e=>{
      card.setPointerCapture(e.pointerId);
      dragging=true; card.setAttribute('aria-grabbed','true');
      sx=e.clientX; sy=e.clientY;
      const r = card.getBoundingClientRect(); ox = r.left; oy = r.top;
      card.style.transition='none';
    });
    window.addEventListener('pointermove', e=>{
      if(!dragging) return;
      const dx = e.clientX - sx, dy = e.clientY - sy;
      const parent = card.parentElement.getBoundingClientRect();
      let nx = ox + dx, ny = oy + dy;
      nx = Math.max(parent.left+8, Math.min(nx, parent.right - card.offsetWidth - 8));
      ny = Math.max(parent.top+8, Math.min(ny, parent.bottom - card.offsetHeight - 8));
      card.style.transform = `translate(${nx - ox}px, ${ny - oy}px)`;
    });
    window.addEventListener('pointerup', e=>{
      if(!dragging) return;
      dragging=false; card.releasePointerCapture(e.pointerId); card.setAttribute('aria-grabbed','false');
      card.style.transform='none'; card.style.transition='transform .26s cubic-bezier(.2,.9,.3,1)';
    });

    /* Affection meter */
    const bar = document.getElementById('affectionBar');
    let affection = 40;
    function setAff(p){ p=Math.max(0,Math.min(100,p)); bar.style.width = p + '%'; }
    setAff(affection);

    function pulse(el){ el.animate([{transform:'scale(1)'},{transform:'scale(1.04)'},{transform:'scale(1)'}],{duration:420}); }
    document.getElementById('sendButton').addEventListener('click', ()=>{
      affection = Math.min(100, affection + 6); setAff(affection);
      pulse(card); spawnConfetti(18); typeWriter(messageText,28);
    });
    document.getElementById('petButton').addEventListener('click', ()=>{
      affection = Math.min(100, affection + 3); setAff(affection);
      card.animate([{transform:'rotate(0)'},{transform:'rotate(-6deg)'},{transform:'rotate(6deg)'},{transform:'rotate(0)'}],{duration:520});
      spawnSmallHearts(8, card.getBoundingClientRect().left + 80, card.getBoundingClientRect().top + 40);
    });

    /* Particles */
    const particles = [];
    function spawnConfetti(n=30){
      for(let i=0;i<n;i++) particles.push({
        x: Math.random()*canvas.width, y: -20 - Math.random()*60,
        vx: (Math.random()-0.5)*6, vy: 2 + Math.random()*4,
        size: 6 + Math.random()*8, life: 60 + Math.random()*40,
        color: pickColor(), type: Math.random()>0.6 ? 'heart' : 'dot'
      });
    }
    function spawnSmallHearts(n, x, y){
      for(let i=0;i<n;i++) particles.push({
        x: x + (Math.random()-0.5)*40, y: y + (Math.random()-0.5)*40,
        vx: (Math.random()-0.5)*3, vy: -2 - Math.random()*3,
        size: 8 + Math.random()*8, life: 40 + Math.random()*30, color: pickColor(), type:'heart'
      });
    }
    function pickColor(){ const a=['#ff6b9a','#ffb3d1','#ffc7e0','#ff87b5','#ffd2e5']; return a[Math.floor(Math.random()*a.length)]; }
    function draw(){
      ctx.clearRect(0,0,canvas.width,canvas.height);
      for(let i=particles.length-1;i>=0;i--){
        const p = particles[i]; p.x += p.vx; p.y += p.vy; p.vy += 0.06; p.life--;
        ctx.globalAlpha = Math.max(0, p.life/80);
        if(p.type==='heart') drawHeart(ctx,p.x,p.y,p.size,p.color);
        else { ctx.fillStyle = p.color; ctx.beginPath(); ctx.ellipse(p.x,p.y,p.size,p.size*0.75,0,0,Math.PI*2); ctx.fill(); }
        if(p.life<=0) particles.splice(i,1);
      }
      requestAnimationFrame(draw);
    }
    function drawHeart(c,x,y,size,color){ c.save(); c.translate(x,y); c.scale(size/24,size/24); c.beginPath(); c.moveTo(0,-6); c.bezierCurveTo(-12,-24,-36,-2,0,18); c.bezierCurveTo(36,-2,12,-24,0,-6); c.closePath(); c.fillStyle=color; c.fill(); c.restore(); }
    draw();

    /* Extra controls */
    document.getElementById('surpriseBtn').addEventListener('click', ()=>{ spawnConfetti(80); });
    document.getElementById('agreeBtn').addEventListener('click', ()=>{
      document.getElementById('statusChip').textContent = 'สถานะ: พี่ครับ 💖';
      spawnConfetti(120);
    });
    document.getElementById('revealBtn').addEventListener('click', ()=> typeWriter(messageText,10));

    // เริ่มต้นโชว์ข้อความ
    window.addEventListener('load', ()=> {
      setTimeout(()=>typeWriter(messageText,18), 600);
    });
  </script>
</body>
</html>