<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Mokshi will you be my velentine ? 🧸</title>
  <style>
    :root{
      --bg:#fff7fb;
      --card:#ffffff;
      --text:#111;
      --pink:#ff69b4;
      --muted:#666;
    }

    /* Use Times New Roman for all text */
    html,body{height:100%;margin:0;font-family:"Times New Roman", Times, serif;}
    body{
      background: linear-gradient(180deg,#fff #ffeef8 60%);
      display:flex;
      align-items:center;
      justify-content:center;
      padding:24px;
      box-sizing:border-box;
      color:var(--text);
    }

    .wrap{
      width:100%;
      max-width:720px;
      background:var(--card);
      border-radius:14px;
      box-shadow:0 8px 30px rgba(17,17,17,0.08);
      padding:28px;
      text-align:center;
      position:relative;
    }

    h1{
      font-size:1.6rem;
      margin:0 0 18px;
      color:var(--text);
      line-height:1.2;
      display:flex;
      align-items:center;
      gap:10px;
      justify-content:center;
    }

    .answers{
      display:flex;
      gap:18px;
      justify-content:center;
      margin-top:8px;
    }

    .box{
      min-width:120px;
      padding:14px 20px;
      border-radius:12px;
      border:2px solid rgba(0,0,0,0.06);
      background:#fff;
      box-shadow:0 4px 12px rgba(17,17,17,0.04);
      cursor:pointer;
      user-select:none;
      transition:transform .18s ease, box-shadow .18s ease;
      display:inline-flex;
      align-items:center;
      justify-content:center;
      font-weight:600;
      font-size:1.05rem;
    }

    .box:active{ transform:scale(.98); }
    .box:hover{ box-shadow:0 8px 22px rgba(17,17,17,0.06); }

    /* Yes styling - only this should be pink */
    .box.yes{
      background:var(--pink);
      color:#fff;
      border-color:rgba(255,105,180,0.9);
    }

    /* No styling - neutral */
    .box.no{
      background:linear-gradient(180deg,#fff #f6f6f6);
      color:var(--text);
    }

    /* When No becomes floating */
    .floating{
      position:fixed !important;
      z-index:9999;
      width:auto;
      min-width:120px;
      pointer-events:auto;
      transition:none !important;
      transform:translate(-50%,-50%);
      border-radius:12px;
    }

    /* Slide that appears after clicking Yes */
    .slide{
      position:absolute;
      inset:0;
      display:flex;
      flex-direction:column;
      align-items:center;
      justify-content:center;
      gap:18px;
      background:linear-gradient(180deg, rgba(255,255,255,0.9), rgba(255,255,255,0.95));
      border-radius:14px;
      padding:28px;
      opacity:0;
      pointer-events:none;
      transition:opacity .36s ease;
    }

    .slide.show{
      opacity:1;
      pointer-events:auto;
    }

    .slide h2{ margin:0; font-size:2rem; color:var(--pink); text-shadow:0 2px 8px rgba(255,105,180,0.08); }
    .slide img{ max-width:80%; border-radius:12px; box-shadow:0 8px 30px rgba(17,17,17,0.08); }

    .bottom-note{
      margin-top:20px;
      font-size:0.95rem;
      color:var(--muted);
    }

    footer{
      margin-top:16px;
      color:var(--muted);
      font-weight:600;
    }

    @media (max-width:480px){
      .answers{ gap:12px; flex-wrap:wrap; }
      .box{ min-width:100px; padding:12px 16px; }
      .slide img{ max-width:90%; }
    }
  </style>
</head>
<body>
  <main class="wrap" role="main">
    <h1 id="question">Mokshi will you be my velentine ? <span aria-hidden="true">🧸</span></h1>

    <div class="answers" id="answers">
      <div class="box yes" id="yesBtn">Yes</div>
      <div class="box no" id="noBtn">No</div>
    </div>

    <div class="slide" id="yaySlide" aria-hidden="true">
      <h2>Yay!</h2>
      <!-- If you have an attached GIF, name it `celebration.gif` in the same folder.
           Otherwise the fallback GIF URL will be used automatically. -->
      <img id="celebrationGif" alt="Celebration GIF" src="celebration.gif" />
    </div>

    <footer class="bottom-note">
      " Dum hai to No click ker ke dikha "
    </footer>
  </main>

  <script>
    // Elements
    const yesBtn = document.getElementById('yesBtn');
    const noBtn = document.getElementById('noBtn');
    const answers = document.getElementById('answers');
    const yaySlide = document.getElementById('yaySlide');
    const gif = document.getElementById('celebrationGif');

    // Fallback GIF (used if local file not found)
    const fallbackGif = 'https://media.giphy.com/media/111ebonMs90YLu/giphy.gif';

    // Try to load local gif; if error, use fallback
    gif.addEventListener('error', () => { gif.src = fallbackGif; });

    // YES click: show slide
    yesBtn.addEventListener('click', () => {
      // hide question/answers and show the slide
      yaySlide.classList.add('show');
      answers.style.opacity = '0';
      answers.style.pointerEvents = 'none';
      // remove any floating behaviour from No if active
      stopFloating();
    });

    // NO click: convert to floating and start circulating
    let floating = false;
    let animFrame;
    let vx = 2 + Math.random()*3;
    let vy = 2 + Math.random()*3;

    function startFloating() {
      if (floating) return;
      floating = true;

      // get current position relative to viewport
      const rect = noBtn.getBoundingClientRect();

      // make a copy that is positioned fixed
      noBtn.classList.add('floating');

      // place it in the same visual place
      noBtn.style.left = (rect.left + rect.width/2) + 'px';
      noBtn.style.top  = (rect.top  + rect.height/2) + 'px';

      // ensure it's added to body for free movement
      document.body.appendChild(noBtn);

      // randomize velocity direction sometimes
      if (Math.random() > 0.5) vx *= -1;
      if (Math.random() > 0.5) vy *= -1;

      // start loop
      moveLoop();
    }

    function moveLoop(){
      animFrame = requestAnimationFrame(() => {
        const rect = noBtn.getBoundingClientRect();
        let x = rect.left + rect.width/2;
        let y = rect.top + rect.height/2;

        x += vx;
        y += vy;

        // bounce off edges with some margin
        const margin = 20;
        const w = window.innerWidth;
        const h = window.innerHeight;

        if (x < margin) { x = margin; vx = Math.abs(vx); }
        if (x > w - margin) { x = w - margin; vx = -Math.abs(vx); }
        if (y < margin) { y = margin; vy = Math.abs(vy); }
        if (y > h - margin) { y = h - margin; vy = -Math.abs(vy); }

        // apply new coords
        noBtn.style.left = x + 'px';
        noBtn.style.top  = y + 'px';

        // gentle rotation for fun
        const r = Math.sin(Date.now()/300) * 10;
        noBtn.style.transform = `translate(-50%,-50%) rotate(${r}deg)`;

        moveLoop();
      });
    }

    function stopFloating(){
      if (!floating) return;
      floating = false;
      cancelAnimationFrame(animFrame);
      // put the No button back into the answers container
      noBtn.classList.remove('floating');
      noBtn.style.transform = '';
      noBtn.style.left = '';
      noBtn.style.top = '';
      answers.appendChild(noBtn);
      answers.style.opacity = '0'; // keep hidden if the Yes slide is showing
    }

    noBtn.addEventListener('click', () => {
      // if slide already shown, ignore
      if (yaySlide.classList.contains('show')) return;
      startFloating();
    });

    // If user clicks the floating No repeatedly, increase speed slightly
    noBtn.addEventListener('mousedown', () => {
      if (floating) {
        vx *= 1.15;
        vy *= 1.15;
      }
    });

    // Accessibility: allow keyboard activation
    yesBtn.tabIndex = 0;
    noBtn.tabIndex = 0;
    yesBtn.addEventListener('keydown', (e) => { if (e.key === 'Enter' || e.key === ' ') yesBtn.click(); });
    noBtn.addEventListener('keydown', (e) => { if (e.key === 'Enter' || e.key === ' ') noBtn.click(); });

    // Keep floating element inside viewport on resize
    window.addEventListener('resize', () => {
      if (!floating) return;
      const rect = noBtn.getBoundingClientRect();
      const m = 20;
      const w = window.innerWidth;
      const h = window.innerHeight;
      let x = rect.left + rect.width/2;
      let y = rect.top + rect.height/2;
      if (x < m) x = m;
      if (x > w - m) x = w - m;
      if (y < m) y = m;
      if (y > h - m) y = h - m;
      noBtn.style.left = x + 'px';
      noBtn.style.top  = y + 'px';
    });
  </script>
</body>
</html>
