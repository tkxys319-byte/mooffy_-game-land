# mooffy_-game-land
<!doctype html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ขอเป็นแฟน — เกมน่ารัก</title>
  <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;800&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg: linear-gradient(135deg,#FFF6F9 0%, #F7FBFF 100%);
      --card: #ffffff;
      --accent: #ff6b9a;
      --accent-2: #ffb3d1;
      --muted: #6b6b7a;
      --glass: rgba(255,255,255,0.6);
      --shadow: 0 8px 30px rgba(32,32,50,0.08);
    }
    html,body{height:100%;margin:0;font-family:'Kanit',system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;}
    body{
      background: var(--bg);
      display:flex;
      align-items:center;
      justify-content:center;
      overflow:hidden;
    }

    /* Stage */
    .stage{
      width:980px;
      max-width:96vw;
      height:640px;
      max-height:90vh;
      position:relative;
      border-radius:20px;
      box-shadow: var(--shadow);
      background: linear-gradient(180deg, rgba(255,255,255,0.6), rgba(255,255,255,0.35));
      backdrop-filter: blur(6px);
      overflow:hidden;
      padding:28px;
      display:grid;
      grid-template-columns: 1fr 380px;
      gap:20px;
    }

    /* Left: interactive playground */
    .playground{
      position:relative;
      border-radius:14px;
      background:
        radial-gradient(600px 200px at 20% 0%, rgba(255,107,154,0.06), transparent 12%),
        linear-gradient(180deg,#ffffff 0%, #fff8fb 100%);
      padding:18px;
      overflow:hidden;
    }
    .title{
      display:flex;
      justify-content:space-between;
      align-items:center;
      gap:16px;
      margin-bottom:8px;
    }
    .title h1{
      font-size:20px;
      margin:0;
      color:#222;
      letter-spacing:0.2px;
      display:flex;
      align-items:center;
      gap:10px;
    }
    .chip{
      background:linear-gradient(90deg,var(--accent),var(--accent-2));
      color:white;
      padding:8px 12px;
      border-radius:999px;
      font-weight:700;
      font-size:13px;
      box-shadow: 0 6px 18px rgba(255,107,154,0.14);
    }

    /* Draggable card (the 'character / boyfriend card') */
    .card{
      width:420px;
      max-width:88%;
      height:300px;
      background: linear-gradient(180deg,#ffffff,#fff0f6);
      border-radius:16px;
      box-shadow: 0 12px 30px rgba(32,32,50,0.08);
      padding:18px;
      position:absolute;
      left:36px;
      top:64px;
      cursor:grab;
      transition:transform 280ms cubic-bezier(.2,.9,.3,1), box-shadow 200ms;
      user-select:none;
      touch-action:none;
    }
    .card:active{cursor:grabbing; transform:scale(0.995); box-shadow: 0 18px 40px rgba(32,32,50,0.12);}

    .card .avatar{
      width:84px;height:84px;border-radius:14px;
      background:linear-gradient(135deg,#ffd1e1,#ffb3d1);
      display:flex;align-items:center;justify-content:center;
      font-weight:800;font-size:34px;color:white;
      box-shadow: 0 8px 24px rgba(255,107,154,0.18);
      float:left;margin-right:14px;
    }
    .card .meta{overflow:hidden}
    .card .name{font-weight:800;font-size:20px;margin:6px 0 2px}
    .card .sub{color:var(--muted);font-size:13px;margin-bottom:10px}
    .card .meter{
      display:flex;gap:8px;align-items:center;margin-top:6px;
    }
    .meter .bar{
      height:10px;background:linear-gradient(90deg,#ffd2e5,#ff9abf);
      border-radius:999px;flex:1;position:relative;overflow:hidden;
      box-shadow: inset 0 -6px 12px rgba(255,255,255,0.18);
    }
    .meter .heart{
      font-size:18px;color:var(--accent);
      animation: float 2.8s ease-in-out infinite;
    }
    @keyframes float{0%{transform:translateY(0)}50%{transform:translateY(-6px)}100%{transform:translateY(0)}}

    /* Action buttons */
    .actions{
      display:flex;gap:12px;position:absolute;bottom:18px;left:18px;
    }
    .btn{
      padding:10px 14px;border-radius:12px;border:0;
      background:linear-gradient(90deg,#fff,#fff);cursor:pointer;
      box-shadow: 0 8px 22px rgba(32,32,50,0.06);
      transition:transform 200ms ease, box-shadow 200ms;
      font-weight:700;
    }
    .btn:hover{transform:translateY(-4px);box-shadow:0 18px 36px rgba(32,32,50,0.08)}
    .btn.primary{
      background:linear-gradient(90deg,var(--accent),var(--accent-2));
      color:#fff;
    }
    .btn.ghost{
      background:transparent;border:1px solid rgba(200,200,210,0.5);color:#222;
    }

    /* Mini game zone: click hearts to collect */
    .zone{
      position:absolute;right:22px;top:38px;width:200px;height:220px;border-radius:12px;
      background:linear-gradient(180deg,#fff7fb,#fff);
      box-shadow: 0 12px 30px rgba(32,32,50,0.04);
      padding:12px;display:flex;flex-direction:column;align-items:center;justify-content:center;
      gap:8px;
    }
    .zone h3{margin:0;font-size:15px}
    .heart-bag{display:flex;gap:6px;align-items:center}
    .collected{font-weight:800;font-size:20px;color:var(--accent)}

    /* Right panel: message + controls */
    .panel{
      padding:18px;border-radius:12px;background:linear-gradient(180deg,#ffffff,#fffafc);
      box-shadow: 0 8px 24px rgba(32,32,50,0.04);
      display:flex;flex-direction:column;gap:12px;
    }
    .panel h2{margin:0;font-size:18px}
    .message{
      flex:1;overflow:auto;padding:14px;border-radius:12px;background:linear-gradient(180deg,#fff,#fff7fb);
      border:1px solid rgba(230,230,240,0.8);position:relative;
    }
    .typewriter{
      font-size:16px;line-height:1.7;color:#222;white-space:pre-wrap;
      min-height:180px;
    }
    .controls{display:flex;gap:8px;flex-wrap:wrap;justify-content:flex-end}

    /* little sparkle effects */
    .sparkle{
      position:absolute;width:8px;height:8px;border-radius:50%;background:rgba(255,255,255,0.9);
      box-shadow:0 0 12px rgba(255,200,230,0.9);opacity:0.9;
      transform:scale(0.9);
      animation: sparkle 1.6s linear infinite;
    }
    @keyframes sparkle{0%{transform:scale(.6);opacity:1}100%{transform:scale(1.6);opacity:0}}

    /* Responsive */
    @media (max-width:880px){
      .stage{grid-template-columns:1fr;grid-auto-rows:auto;height:auto;padding:18px}
      .card{position:relative;left:0;top:0;margin-bottom:16px}
      .actions{position:static}
      .zone{position:static;right:auto;top:auto;width:100%;height:120px;flex-direction:row;}
    }
    /* subtle focus outlines */
    .btn:focus{outline:3px solid rgba(255,107,154,0.14); outline-offset:4px}
  </style>
</head>
<body>
  <div class="stage" role="application" aria-label="ขอเป็นแฟน แบบเกมน่ารัก">
    <div class="playground">
      <div class="title">
        <h1>💌 ขอเป็นแฟน — เกมน่ารัก <span style="font-size:14px;color:var(--muted);font-weight:500">(กดเล่นได้ ขยับได้)</span></h1>
        <div class="chip" id="statusChip">สถานะ: ยังไม่ได้ตอบ</div>
      </div>

      <div class="card" id="draggable" aria-grabbed="false" tabindex="0">
        <div class="avatar" aria-hidden="true">พี่</div>
        <div class="meta">
          <div class="name">ขอเป็นแฟนเธอคนนี้</div>
          <div class="sub">อยากให้พี่ยิ้มทุกวัน — เกมเล็ก ๆ ที่เต็มไปด้วยหัวใจ</div>

          <div class="meter" aria-hidden="true">
            <div class="bar" id="affectionBar" style="width:60%"></div>
            <div class="heart">💗</div>
          </div>
        </div>

        <div style="clear:both"></div>

        <div style="margin-top:14px;color:var(--muted);font-size:13px">
          ลองลากกล่องไปมา หรือกดปุ่มด้านล่างเพื่อส่งความในใจ
        </div>

        <div class="actions">
          <button class="btn primary" id="sendButton" aria-pressed="false">ส่งความในใจ ❤️</button>
          <button class="btn ghost" id="petButton">โอ๋ <span style="opacity:.8">🤗</span></button>
        </div>
      </div>

      <div class="zone" aria-label="มินิเกมสะสมหัวใจ">
        <h3>มินิเกม: สะสมหัวใจ</h3>
        <div class="heart-bag">
          <div style="font-size:22px">💖</div>
          <div class="collected" id="collectedCount">0</div>
        </div>
        <div style="font-size:12px;color:var(--muted);margin-top:6px;text-align:center">
          กดปุ่ม "ปล่อยหัวใจ" เพื่อให้หัวใจร่วง แล้วกดบนนั้นเพื่อเก็บ
        </div>
        <div style="margin-top:8px;display:flex;gap:6px">
          <button class="btn" id="spawnHearts">ปล่อยหัวใจ</button>
          <button class="btn" id="clearHearts">เก็บทั้งหมด</button>
        </div>
      </div>

      <!-- small decorative sparkles -->
      <div class="sparkle" style="left:20px;top:18px;animation-delay:0s"></div>
      <div class="sparkle" style="left:120px;top:6px;animation-delay:.3s"></div>
      <div class="sparkle" style="right:60px;top:40px;animation-delay:.6s"></div>
    </div>

    <aside class="panel" aria-label="ข้อความขอเป็นแฟน">
      <h2>ข้อความจากใจ (กดเล่นเพื่อไทป์)</h2>
      <div class="message" id="messageBox" role="region" aria-live="polite">
        <div class="typewriter" id="typewriter">
          <!-- text appears here -->
        </div>
      </div>

      <div class="controls">
        <button class="btn ghost" id="revealBtn">แสดงข้อความชั่วคราว</button>
        <button class="btn primary" id="agreeBtn">ตอบรับเป็นแฟน 💍</button>
        <button class="btn" id="surpriseBtn">เซอร์ไพรส์! 🎉</button>
      </div>
    </aside>
  </div>

  <!-- canvas for confetti/hearts -->
  <canvas id="fxCanvas" style="position:fixed;left:0;top:0;pointer-events:none;z-index:9999"></canvas>

  <script>
    /* ========== Helpers ========== */
    const $ = (s)=>document.querySelector(s);
    const $$ = (s)=>Array.from(document.querySelectorAll(s));

    // resize canvas
    const canvas = document.getElementById('fxCanvas');
    const ctx = canvas.getContext('2d');
    function fitCanvas(){
      canvas.width = innerWidth;
      canvas.height = innerHeight;
    }
    addEventListener('resize', fitCanvas);
    fitCanvas();

    /* ========== Typewriter (for the heartfelt Thai message) ========== */
    const messageText = [
`ชอบพี่มาตลอดเลย... ซึ่ง ๆ กินใจแบบรักมากทุกวัน
อยากคอยโอ๋ตลอดเวลา อยากเห็นพี่มีความสุข ยิ้มได้เยอะ ๆ
ไม่ว่าจะนานแค่ไหน ถึงจะมีช่วงหาย ๆ กันบ้าง แต่ก็ชอบพี่เสมอมา
ขอซึ่ง ๆ น่ารักหวาน ๆ แบบไม่มีโอกาสบอกพี่สักที กลัวใจตัวเองด้วย
แต่วันนี้อยากมีพี่อยู่ไปนาน ๆ 💌`
    ].join('\n');

    const typeEl = $('#typewriter');
    let typingTimeout;
    function typeWriterRun(text, speed=26){
      typeEl.textContent = '';
      let i=0;
      function step(){
        if(i<=text.length){
          typeEl.textContent = text.slice(0,i);
          i++;
          typingTimeout = setTimeout(step, speed + (Math.random()*20));
        } else {
          // blinking caret effect
          typeEl.innerHTML = typeEl.innerHTML + '<span style="opacity:.6"> ▎</span>';
          setTimeout(()=>{ typeEl.innerHTML = typeEl.innerHTML.replace(' ▎','') }, 800);
        }
      }
      step();
    }

    // reveal temporary (fast) vs full (slow)
    $('#revealBtn').addEventListener('click', ()=> {
      clearTimeout(typingTimeout);
      typeWriterRun(messageText, 8);
    });

    /* ========== Draggable card ========== */
    const card = $('#draggable');
    let dragging = false;
    let startX, startY, origX, origY;
    // compute initial from getBoundingClientRect
    function getCardPos(){
      const r = card.getBoundingClientRect();
      return {left:r.left, top:r.top};
    }
    // mouse events
    card.addEventListener('pointerdown', (e)=>{
      card.setPointerCapture(e.pointerId);
      dragging = true;
      card.setAttribute('aria-grabbed','true');
      startX = e.clientX; startY = e.clientY;
      const rect = card.getBoundingClientRect();
      origX = rect.left; origY = rect.top;
      card.style.transition = 'none';
    });
    window.addEventListener('pointermove', (e)=>{
      if(!dragging) return;
      const dx = e.clientX - startX;
      const dy = e.clientY - startY;
      // limit within stage
      const parentRect = card.parentElement.getBoundingClientRect();
      let nx = origX + dx;
      let ny = origY + dy;
      // clamp
      nx = Math.max(parentRect.left + 8, Math.min(nx, parentRect.right - card.offsetWidth - 8));
      ny = Math.max(parentRect.top + 8, Math.min(ny, parentRect.bottom - card.offsetHeight - 8));
      // apply transform by translating relative to initial position
      card.style.transform = `translate(${nx - origX}px, ${ny - origY}px)`;
    });
    window.addEventListener('pointerup', (e)=>{
      if(!dragging) return;
      dragging = false;
      card.releasePointerCapture(e.pointerId);
      card.setAttribute('aria-grabbed','false');
      // commit final position
      const transform = card.style.transform;
      // compute numbers
      const m = /translate\\((-?\\d+(?:\\.\\d+)?)px,\\s*(-?\\d+(?:\\.\\d+)?)px\\)/.exec(transform || '');
      if(m){
        const tx = parseFloat(m[1]), ty = parseFloat(m[2]);
        const left = (card.offsetLeft + tx);
        const top = (card.offsetTop + ty);
        // apply absolute position and reset transform
        card.style.transition = 'transform 260ms cubic-bezier(.2,.9,.3,1)';
        card.style.transform = 'none';
        card.style.left = left + 'px';
        card.style.top = top + 'px';
      } else {
        card.style.transform = 'none';
      }
    });

    /* ========== Affection meter (increases when clicking send/pet) ========== */
    const affectionBar = $('#affectionBar');
    function setAffection(percent){
      const p = Math.max(0, Math.min(100, percent));
      affectionBar.style.width = p + '%';
    }
    let affection = 60;
    setAffection(affection);

    $('#sendButton').addEventListener('click', ()=>{
      affection = Math.min(100, affection + 8);
      setAffection(affection);
      pulseCard();
      launchConfetti(18);
      showToast('ส่งความในใจสำเร็จ 💌');
      // start typing if not yet shown
      typeWriterRun(messageText, 32);
    });
    $('#petButton').addEventListener('click', ()=>{
      affection = Math.min(100, affection + 4);
      setAffection(affection);
      wobble(card);
      smallHearts(8, card.getBoundingClientRect().left + 80, card.getBoundingClientRect().top + 40);
    });

    function pulseCard(){
      card.animate([
        { transform: 'scale(1)' },
        { transform: 'scale(1.04)' },
        { transform: 'scale(1)' }
      ], { duration:420, easing:'cubic-bezier(.2,.9,.3,1)' });
    }
    function wobble(el){
      el.animate([
        { transform:'rotate(0deg)' },
        { transform:'rotate(-6deg)' },
        { transform:'rotate(6deg)' },
        { transform:'rotate(0deg)' }
      ], { duration:520, easing:'ease-out' });
    }

    /* ========== Mini hearts spawn & collect ========== */
    const spawnBtn = $('#spawnHearts');
    const clearBtn = $('#clearHearts');
    const collected = $('#collectedCount');
    let bagCount = 0;
    const activeHearts = new Set();

    spawnBtn.addEventListener('click', ()=> {
      // spawn a few hearts that fall
      const zone = document.querySelector('.zone');
      for(let i=0;i<6;i++){
        createFallingHeart(zone, 20 + i*18 + Math.random()*40);
      }
    });

    clearBtn.addEventListener('click', ()=> {
      bagCount = 0;
      collected.textContent = bagCount;
      // remove hearts
      activeHearts.forEach(el=>el.remove());
      activeHearts.clear();
      showToast('เก็บทั้งหมดแล้ว ✨');
    });

    function createFallingHeart(container, offsetX){
      const heart = document.createElement('div');
      heart.textContent = '💞';
      heart.style.position='absolute';
      heart.style.left = (offsetX) + 'px';
      heart.style.top = '-30px';
      heart.style.fontSize = (18 + Math.random()*14) + 'px';
      heart.style.cursor = 'pointer';
      heart.style.userSelect='none';
      container.appendChild(heart);
      activeHearts.add(heart);
      // fall animation
      const duration = 2200 + Math.random()*1400;
      const endY = container.offsetHeight - 24 - Math.random()*30;
      let start = null;
      function step(ts){
        if(!start) start = ts;
        const progress = Math.min(1, (ts-start)/duration);
        heart.style.top = (-30 + (endY+40) * progress) + 'px';
        heart.style.left = (offsetX + Math.sin(progress*6 + Math.random())*18) + 'px';
        heart.style.opacity = 1 - progress*0.25;
        if(progress < 1) requestAnimationFrame(step);
        else {
          // stays on ground; clickable to collect
        }
      }
      requestAnimationFrame(step);
      heart.addEventListener('click', ()=>{
        // collect
        bagCount++;
        collected.textContent = bagCount;
        activeHearts.delete(heart);
        heart.animate([{transform:'scale(1)'},{transform:'scale(1.4)'},{transform:'scale(0)'}],{duration:380,easing:'ease-out'});
        setTimeout(()=>heart.remove(),380);
        smallHearts(6, heart.getBoundingClientRect().left + 10, heart.getBoundingClientRect().top + 10);
      });
      // auto-remove after some time
      setTimeout(()=>{ if(activeHearts.has(heart)){activeHearts.delete(heart); heart.remove()} }, 14000);
    }

    /* ========== Confetti & hearts FX on canvas ========== */
    const particles = [];
    function launchConfetti(n=30){
      for(let i=0;i<n;i++){
        particles.push({
          x: Math.random()*canvas.width,
          y: -20 - Math.random()*60,
          vx: (Math.random()-0.5)*6,
          vy: 2 + Math.random()*4,
          size: 6 + Math.random()*8,
          life: 60 + Math.random()*40,
          color: randomHeartColor(),
          type: Math.random()>0.6 ? 'heart' : 'dot'
        });
      }
    }
    function smallHearts(n, x, y){
      for(let i=0;i<n;i++){
        particles.push({
          x: x + (Math.random()-0.5)*40,
          y: y + (Math.random()-0.5)*40,
          vx: (Math.random()-0.5)*3,
          vy: -2 - Math.random()*3,
          size: 8 + Math.random()*10,
          life: 40 + Math.random()*30,
          color: randomHeartColor(),
          type: 'heart'
        });
      }
    }
    function randomHeartColor(){
      const arr = ['#ff6b9a','#ffb3d1','#ffc7e0','#ff87b5','#ffd2e5'];
      return arr[Math.floor(Math.random()*arr.length)];
    }

    function drawParticles(){
      ctx.clearRect(0,0,canvas.width,canvas.height);
      for(let i=particles.length-1;i>=0;i--){
        const p = particles[i];
        p.x += p.vx;
        p.y += p.vy;
        p.vy += 0.06; // gravity
        p.life--;
        const alpha = Math.max(0, p.life/80);
        ctx.globalAlpha = alpha;
        if(p.type === 'heart'){
          drawHeart(ctx, p.x, p.y, p.size, p.color);
        } else {
          ctx.fillStyle = p.color;
          ctx.beginPath();
          ctx.ellipse(p.x, p.y, p.size, p.size*0.75, 0,0,Math.PI*2);
          ctx.fill();
        }
        if(p.life<=0){
          particles.splice(i,1);
        }
      }
      requestAnimationFrame(drawParticles);
    }
    function drawHeart(c, x, y, size, color){
      c.save();
      c.translate(x,y);
      c.scale(size/24, size/24);
      c.beginPath();
      c.moveTo(0, -6);
      c.bezierCurveTo(-12, -24, -36, -2, 0, 18);
      c.bezierCurveTo(36, -2, 12, -24, 0, -6);
      c.closePath();
      c.fillStyle = color;
      c.fill();
      c.restore();
    }
    drawParticles();

    /* ========== toast messages (tiny notifications) ========== */
    function showToast(text, timeout=1800){
      const t = document.createElement('div');
      t.textContent = text;
      t.style.position='fixed';
      t.style.left='50%';
      t.style.bottom='8%';
      t.style.transform='translateX(-50%) translateY(6px)';
      t.style.background='rgba(20,20,30,0.92)';
      t.style.color='white';
      t.style.padding='10px 16px';
      t.style.borderRadius='10px';
      t.style.fontWeight='700';
      t.style.zIndex=99999;
      t.style.boxShadow='0 12px 30px rgba(0,0,0,0.18)';
      t.style.opacity='0';
      t.style.transition='all 320ms ease';
      document.body.appendChild(t);
      requestAnimationFrame(()=>{ t.style.opacity='1'; t.style.transform='translateX(-50%) translateY(0)'; });
      setTimeout(()=>{ t.style.opacity='0'; t.style.transform='translateX(-50%) translateY(8px)'; setTimeout(()=>t.remove(),320) }, timeout);
    }

    /* ========== Surprise / Agree buttons ========== */
    $('#surpriseBtn').addEventListener('click', ()=>{
      // big confetti + message
      launchConfetti(80);
      showToast('เซอร์ไพรส์! 🎉 ส่งความหวานแล้ว');
      $('#statusChip').textContent = 'สถานะ: พิเศษ ♥';
    });
    $('#agreeBtn').addEventListener('click', ()=>{
      $('#statusChip').textContent = 'สถานะ: เป็นแฟนแล้ว 💍';
      launchConfetti(120);
      showToast('น่ารักจัง... ตอบรับเป็นแฟนแล้ว 💞', 2400);
      typeWriterRun('วันนี้...ขอเป็นคนที่อยู่ข้าง ๆ พี่นะ\nขอเป็นแฟนของพี่ไปนาน ๆ 💖', 18);
    });

    /* ========== Small startup animations ========== */
    window.addEventListener('load', ()=>{
      // gentle entrance
      card.style.opacity = 0;
      card.style.transform = 'translateY(20px)';
      setTimeout(()=>{ card.style.transition='all 520ms cubic-bezier(.2,.9,.3,1)'; card.style.opacity=1; card.style.transform='translateY(0)'; }, 120);
      // small intro type
      setTimeout(()=>{ typeWriterRun('สวัสดี... กด "ส่งความในใจ" หรือ "ตอบรับเป็นแฟน" ได้เลยนะ 😊', 18); }, 650);
    });

    /* ========== Extra small UI niceties ========== */
    // accessibility: keyboard support to 'send' or 'pet'
    card.addEventListener('keydown', (e)=>{
      if(e.key === 'Enter' || e.key === ' '){
        $('#sendButton').click();
        e.preventDefault();
      }
      if(e.key === 'o' || e.key === 'O') $('#petButton').click();
    });

    // ensure canvas always on top of stage area
    // no-op (already fixed)

    // small utility: click anywhere on stage to make a mini heart burst
    document.querySelector('.playground').addEventListener('click', (ev)=>{
      if(ev.target.closest('.btn')) return; // ignore buttons
      const r = ev.target.getBoundingClientRect ? ev.target.getBoundingClientRect() : {left:ev.clientX,top:ev.clientY};
      smallHearts(6, ev.clientX, ev.clientY);
    });

  </script>
</body>
</html>