<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>YOUR NAME — Web Developer</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Bangers&family=Space+Grotesk:wght@400;500;600;700&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
<style>
  :root{
    --void:#0a0a12;
    --void-2:#111120;
    --brick:#4a0e16;
    --spidey-red:#e0212b;
    --spidey-red-dim:#8c1a20;
    --web-blue:#22377a;
    --web-blue-bright:#3d5cff;
    --paper:#ece6d6;
    --amber:#ffb100;
    --ink:#0a0a12;
    --gutter:#000;
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    background:var(--void);
    color:var(--paper);
    font-family:'Space Grotesk',sans-serif;
    overflow-x:hidden;
    cursor:none;
  }
  ::selection{background:var(--spidey-red);color:var(--paper);}

  /* halftone comic texture overlay */
  body::before{
    content:"";
    position:fixed;inset:0;
    background-image:radial-gradient(circle, rgba(224,33,43,0.10) 1px, transparent 1px);
    background-size:14px 14px;
    pointer-events:none;
    z-index:2;
    opacity:.5;
  }
  /* vignette */
  body::after{
    content:"";
    position:fixed;inset:0;
    background:radial-gradient(ellipse at center, transparent 40%, rgba(0,0,0,.65) 100%);
    pointer-events:none;
    z-index:3;
  }

  h1,h2,h3,.display{
    font-family:'Bangers',cursive;
    letter-spacing:.03em;
    font-weight:400;
  }
  .mono{font-family:'JetBrains Mono',monospace;}

  a{color:inherit;text-decoration:none;}

  /* custom cursor */
  #cursor-dot{
    position:fixed;top:0;left:0;
    width:8px;height:8px;
    background:var(--spidey-red);
    border-radius:50%;
    pointer-events:none;
    z-index:9999;
    transform:translate(-50%,-50%);
    box-shadow:0 0 12px var(--spidey-red);
  }
  #cursor-ring{
    position:fixed;top:0;left:0;
    width:34px;height:34px;
    border:2px solid rgba(224,33,43,.6);
    border-radius:50%;
    pointer-events:none;
    z-index:9998;
    transform:translate(-50%,-50%);
    transition:width .18s ease, height .18s ease, border-color .18s ease;
  }
  #web-canvas{
    position:fixed;inset:0;
    pointer-events:none;
    z-index:9997;
  }

  /* side nav */
  #side-nav{
    position:fixed;
    right:28px;top:50%;
    transform:translateY(-50%);
    z-index:500;
    display:flex;flex-direction:column;gap:18px;
  }
  #side-nav a{
    width:12px;height:12px;
    border:2px solid var(--paper);
    border-radius:50%;
    display:block;
    opacity:.5;
    transition:all .25s ease;
    position:relative;
  }
  #side-nav a.active,#side-nav a:hover{
    opacity:1;
    background:var(--spidey-red);
    border-color:var(--spidey-red);
    box-shadow:0 0 14px var(--spidey-red);
  }
  #side-nav a::after{
    content:attr(data-label);
    position:absolute;
    right:24px;top:50%;
    transform:translateY(-50%);
    font-family:'JetBrains Mono',monospace;
    font-size:11px;
    white-space:nowrap;
    background:var(--ink);
    padding:4px 8px;
    border:1px solid var(--spidey-red-dim);
    opacity:0;
    pointer-events:none;
    transition:opacity .2s ease;
  }
  #side-nav a:hover::after{opacity:1;}

  section{
    position:relative;
    min-height:100vh;
    padding:120px 8vw;
    z-index:10;
  }

  /* skyline parallax */
  #skyline{
    position:absolute;
    bottom:0;left:0;width:100%;
    height:40vh;
    z-index:1;
    opacity:.5;
    pointer-events:none;
  }

  /* HERO */
  #hero{
    display:flex;
    flex-direction:column;
    justify-content:center;
    align-items:flex-start;
    min-height:100vh;
    padding-top:0;
  }
  .eyebrow{
    font-family:'JetBrains Mono',monospace;
    color:var(--spidey-red);
    font-size:13px;
    letter-spacing:.25em;
    text-transform:uppercase;
    margin-bottom:18px;
    display:flex;align-items:center;gap:10px;
  }
  .eyebrow::before{
    content:"";
    width:34px;height:2px;
    background:var(--spidey-red);
  }
  #hero h1{
    font-size:clamp(4rem,13vw,10.5rem);
    line-height:.85;
    color:var(--paper);
    -webkit-text-stroke:2px var(--spidey-red);
    text-shadow:6px 6px 0 var(--spidey-red), 6px 6px 0 var(--spidey-red);
  }
  #hero h1 span{display:block;}
  #hero .tag{
    margin-top:24px;
    font-size:clamp(1.1rem,2.2vw,1.6rem);
    color:#c9c4b4;
    max-width:640px;
    min-height:2.4em;
  }
  #hero .tag .cursor-blink{
    display:inline-block;
    width:2px;height:1.1em;
    background:var(--spidey-red);
    margin-left:4px;
    vertical-align:middle;
    animation:blink 1s steps(1) infinite;
  }
  @keyframes blink{50%{opacity:0;}}

  .scroll-cue{
    position:absolute;
    bottom:48px;left:8vw;
    font-family:'JetBrains Mono',monospace;
    font-size:12px;
    color:#8a8574;
    display:flex;align-items:center;gap:10px;
  }
  .scroll-cue .line{
    width:1px;height:40px;
    background:linear-gradient(var(--spidey-red),transparent);
    animation:drop 1.8s ease-in-out infinite;
  }
  @keyframes drop{0%{transform:scaleY(0);transform-origin:top;}50%{transform:scaleY(1);transform-origin:top;}51%{transform-origin:bottom;}100%{transform:scaleY(0);transform-origin:bottom;}}

  /* comic panel component */
  .panel{
    background:var(--paper);
    color:var(--ink);
    border:3px solid var(--gutter);
    box-shadow:8px 8px 0 var(--gutter);
    padding:28px;
    position:relative;
  }
  .panel-title{
    font-family:'Bangers',cursive;
    font-size:2rem;
    color:var(--spidey-red);
    -webkit-text-stroke:1px var(--ink);
  }

  /* section headers */
  .section-head{
    display:flex;
    align-items:baseline;
    gap:18px;
    margin-bottom:56px;
  }
  .section-head .num{
    font-family:'JetBrains Mono',monospace;
    color:var(--spidey-red-dim);
    font-size:14px;
  }
  .section-head h2{
    font-size:clamp(2.4rem,5vw,4.2rem);
    color:var(--paper);
  }
  .section-head h2 em{
    font-style:normal;
    color:var(--spidey-red);
  }

  /* ABOUT */
  #about{display:grid;grid-template-columns:1fr 1fr;gap:48px;align-items:start;}
  #about .story p{margin-bottom:18px;line-height:1.7;color:#c9c4b4;font-size:1.05rem;}
  #about .story strong{color:var(--paper);}
  .stat-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-top:32px;}
  .stat{
    border-left:3px solid var(--spidey-red);
    padding-left:14px;
  }
  .stat .num{
    font-family:'Bangers',cursive;
    font-size:2.6rem;
    color:var(--paper);
  }
  .stat .label{
    font-family:'JetBrains Mono',monospace;
    font-size:11px;
    color:#8a8574;
    text-transform:uppercase;
    letter-spacing:.1em;
  }

  /* STACK */
  #stack .grid{
    display:grid;
    grid-template-columns:repeat(auto-fill,minmax(160px,1fr));
    gap:20px;
  }
  .chip{
    background:var(--void-2);
    border:2px solid #23233a;
    padding:20px 16px;
    text-align:center;
    font-family:'JetBrains Mono',monospace;
    font-size:13px;
    transition:transform .2s ease, border-color .2s ease, background .2s ease;
    cursor:none;
  }
  .chip:hover{
    transform:translateY(-6px) rotate(-1deg);
    border-color:var(--spidey-red);
    background:var(--brick);
  }

  /* PROJECTS - comic panels that flip */
  #projects .grid{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
    gap:32px;
  }
  .flip-card{
    perspective:1200px;
    height:320px;
  }
  .flip-inner{
    position:relative;
    width:100%;height:100%;
    transition:transform .6s cubic-bezier(.4,.2,.2,1);
    transform-style:preserve-3d;
  }
  .flip-card:hover .flip-inner{transform:rotateY(180deg);}
  .flip-front,.flip-back{
    position:absolute;inset:0;
    backface-visibility:hidden;
    border:3px solid var(--gutter);
    box-shadow:8px 8px 0 var(--gutter);
    padding:26px;
    display:flex;
    flex-direction:column;
    justify-content:space-between;
  }
  .flip-front{
    background:var(--void-2);
    color:var(--paper);
  }
  .flip-front .tag-no{
    font-family:'JetBrains Mono',monospace;
    color:var(--spidey-red);
    font-size:12px;
  }
  .flip-front h3{font-size:2rem;color:var(--paper);margin-top:10px;}
  .flip-front .hint{
    font-family:'JetBrains Mono',monospace;
    font-size:11px;color:#8a8574;
  }
  .flip-back{
    background:var(--spidey-red);
    color:var(--ink);
    transform:rotateY(180deg);
  }
  .flip-back p{font-size:.95rem;line-height:1.55;margin-bottom:14px;}
  .flip-back .stack-line{
    font-family:'JetBrains Mono',monospace;
    font-size:11px;
    opacity:.85;
  }

  /* STATS/GITHUB */
  #ghstats img{
    max-width:100%;
    border:3px solid var(--gutter);
    box-shadow:8px 8px 0 var(--gutter);
  }
  #ghstats .grid{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:24px;
    margin-top:20px;
  }

  /* CONTACT */
  #contact{
    display:flex;
    flex-direction:column;
    align-items:center;
    text-align:center;
    justify-content:center;
  }
  #contact h2{font-size:clamp(2.6rem,7vw,5.5rem);}
  #contact .sub{color:#c9c4b4;margin:18px 0 40px;max-width:520px;}
  .signal-btn{
    font-family:'Bangers',cursive;
    font-size:1.6rem;
    letter-spacing:.04em;
    background:var(--spidey-red);
    color:var(--ink);
    padding:18px 46px;
    border:3px solid var(--gutter);
    box-shadow:6px 6px 0 var(--gutter);
    cursor:none;
    transition:transform .15s ease, box-shadow .15s ease;
  }
  .signal-btn:hover{
    transform:translate(-3px,-3px);
    box-shadow:9px 9px 0 var(--gutter);
  }
  .socials{
    margin-top:44px;
    display:flex;gap:28px;
    font-family:'JetBrains Mono',monospace;
    font-size:13px;
  }
  .socials a{
    border-bottom:1px solid transparent;
    transition:border-color .2s ease, color .2s ease;
  }
  .socials a:hover{color:var(--spidey-red);border-color:var(--spidey-red);}

  footer{
    text-align:center;
    padding:40px 0;
    font-family:'JetBrains Mono',monospace;
    font-size:11px;
    color:#55523f;
    z-index:10;
    position:relative;
  }

  .reveal{
    opacity:0;
    transform:translateY(40px);
    transition:opacity .8s ease, transform .8s ease;
  }
  .reveal.in{opacity:1;transform:translateY(0);}

  @media (max-width:720px){
    #about{grid-template-columns:1fr;}
    #side-nav{display:none;}
    section{padding:100px 6vw;}
  }
  @media (prefers-reduced-motion: reduce){
    *{animation:none !important; transition:none !important;}
  }
</style>
</head>
<body>

<canvas id="web-canvas"></canvas>
<div id="cursor-ring"></div>
<div id="cursor-dot"></div>

<nav id="side-nav">
  <a href="#hero" data-label="TOP" class="active"></a>
  <a href="#about" data-label="ORIGIN"></a>
  <a href="#stack" data-label="LOADOUT"></a>
  <a href="#projects" data-label="CASE FILES"></a>
  <a href="#ghstats" data-label="INTEL"></a>
  <a href="#contact" data-label="SIGNAL"></a>
</nav>

<!-- HERO -->
<section id="hero">
  <svg id="skyline" viewBox="0 0 1440 300" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg">
    <rect x="0" y="120" width="60" height="180" fill="#15151f"/>
    <rect x="70" y="80" width="45" height="220" fill="#15151f"/>
    <rect x="125" y="150" width="70" height="150" fill="#15151f"/>
    <rect x="205" y="60" width="50" height="240" fill="#15151f"/>
    <rect x="265" y="110" width="60" height="190" fill="#15151f"/>
    <rect x="335" y="40" width="40" height="260" fill="#15151f"/>
    <rect x="385" y="130" width="80" height="170" fill="#15151f"/>
    <rect x="475" y="90" width="55" height="210" fill="#15151f"/>
    <rect x="540" y="150" width="65" height="150" fill="#15151f"/>
    <rect x="615" y="70" width="45" height="230" fill="#15151f"/>
    <rect x="670" y="120" width="90" height="180" fill="#15151f"/>
    <rect x="770" y="50" width="50" height="250" fill="#15151f"/>
    <rect x="830" y="140" width="60" height="160" fill="#15151f"/>
    <rect x="900" y="100" width="70" height="200" fill="#15151f"/>
    <rect x="980" y="60" width="45" height="240" fill="#15151f"/>
    <rect x="1035" y="150" width="80" height="150" fill="#15151f"/>
    <rect x="1125" y="90" width="55" height="210" fill="#15151f"/>
    <rect x="1190" y="120" width="65" height="180" fill="#15151f"/>
    <rect x="1265" y="50" width="50" height="250" fill="#15151f"/>
    <rect x="1325" y="140" width="115" height="160" fill="#15151f"/>
  </svg>

  <div class="eyebrow mono">FRIENDLY NEIGHBORHOOD DEVELOPER</div>
  <h1><span>YOUR</span><span>NAME</span></h1>
  <p class="tag mono" id="typewriter"></p>

  <div class="scroll-cue mono"><span class="line"></span>SWING DOWN</div>
</section>

<!-- ABOUT -->
<section id="about">
  <div class="reveal">
    <div class="section-head">
      <span class="num">01</span>
      <h2>Origin <em>Story</em></h2>
    </div>
    <div class="story">
      <p><strong>Bitten by a radioactive bug</strong> somewhere between a CS degree and a production outage, I've spent the years since building software that (mostly) doesn't crash. I move between frontend and backend the way Spidey moves between rooftops — fast, and always with a safety line.</p>
      <p>These days I'm slinging code across <strong>web apps, APIs, and the occasional 2am hotfix.</strong> When I'm not shipping, I'm reading the comics that started this whole obsession.</p>
    </div>
    <div class="stat-grid">
      <div class="stat"><div class="num" data-count="47">0</div><div class="label">Repos Web-Slung</div></div>
      <div class="stat"><div class="num" data-count="120">0</div><div class="label">Bugs Caught</div></div>
      <div class="stat"><div class="num" data-count="6">0</div><div class="label">Years Swinging</div></div>
      <div class="stat"><div class="num" data-count="9">0</div><div class="label">Coffees / Day</div></div>
    </div>
  </div>
  <div class="reveal panel" style="align-self:center;">
    <div class="panel-title">SPIDEY-SENSE LOG</div>
    <p class="mono" style="font-size:13px;line-height:1.8;margin-top:10px;">
      > alert: incoming deadline<br>
      > status: web-shooters loaded<br>
      > mood: friendly, neighborhood-grade<br>
      > weakness: merge conflicts<br>
      > mantra: "with great power comes<br>&nbsp;&nbsp;great unit test coverage"
    </p>
  </div>
</section>

<!-- STACK -->
<section id="stack">
  <div class="reveal">
    <div class="section-head">
      <span class="num">02</span>
      <h2>Web-Shooter <em>Loadout</em></h2>
    </div>
    <div class="grid">
      <div class="chip">Python</div>
      <div class="chip">JavaScript</div>
      <div class="chip">TypeScript</div>
      <div class="chip">React</div>
      <div class="chip">Node.js</div>
      <div class="chip">Next.js</div>
      <div class="chip">PostgreSQL</div>
      <div class="chip">Docker</div>
      <div class="chip">AWS</div>
      <div class="chip">GraphQL</div>
      <div class="chip">Redis</div>
      <div class="chip">Git</div>
    </div>
  </div>
</section>

<!-- PROJECTS -->
<section id="projects">
  <div class="reveal">
    <div class="section-head">
      <span class="num">03</span>
      <h2>Case <em>Files</em></h2>
    </div>
    <div class="grid">
      <div class="flip-card">
        <div class="flip-inner">
          <div class="flip-front">
            <div><span class="tag-no">FILE NO. 001</span><h3>Project One</h3></div>
            <div class="hint">HOVER TO DECLASSIFY →</div>
          </div>
          <div class="flip-back">
            <p>A short, honest description of what this project does and why it exists. Swap this copy for your real project.</p>
            <div class="stack-line">React · Node · PostgreSQL</div>
          </div>
        </div>
      </div>
      <div class="flip-card">
        <div class="flip-inner">
          <div class="flip-front">
            <div><span class="tag-no">FILE NO. 002</span><h3>Project Two</h3></div>
            <div class="hint">HOVER TO DECLASSIFY →</div>
          </div>
          <div class="flip-back">
            <p>Describe the problem this solved and the interesting technical decision behind it.</p>
            <div class="stack-line">Next.js · Prisma · AWS</div>
          </div>
        </div>
      </div>
      <div class="flip-card">
        <div class="flip-inner">
          <div class="flip-front">
            <div><span class="tag-no">FILE NO. 003</span><h3>Project Three</h3></div>
            <div class="hint">HOVER TO DECLASSIFY →</div>
          </div>
          <div class="flip-back">
            <p>What it does, who it's for, and the one detail you're proudest of.</p>
            <div class="stack-line">Python · FastAPI · Docker</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- GITHUB STATS -->
<section id="ghstats">
  <div class="reveal">
    <div class="section-head">
      <span class="num">04</span>
      <h2>Daily Bugle <em>Intel</em></h2>
    </div>
    <div class="grid">
      <img src="https://github-readme-stats.vercel.app/api?username=YOUR_USERNAME&show_icons=true&theme=red&hide_border=true&bg_color=0d0d0d&title_color=e0212b&icon_color=3d5cff&text_color=ece6d6&count_private=true" alt="GitHub stats"/>
      <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=YOUR_USERNAME&layout=compact&theme=red&hide_border=true&bg_color=0d0d0d&title_color=e0212b&text_color=ece6d6" alt="Top languages"/>
    </div>
    <img style="margin-top:24px;width:100%;" src="https://github-readme-streak-stats.herokuapp.com/?user=YOUR_USERNAME&theme=red&hide_border=true&background=0D0D0D&ring=E0212B&fire=3D5CFF&currStreakLabel=E0212B" alt="Streak stats"/>
  </div>
</section>

<!-- CONTACT -->
<section id="contact">
  <div class="reveal">
    <h2>Send a <em style="color:var(--spidey-red);">Signal</em></h2>
    <p class="sub mono">Point the Spider-Signal at the sky, or just use email — either works.</p>
    <a class="signal-btn" href="mailto:you@example.com">SHOOT A WEB</a>
    <div class="socials">
      <a href="https://github.com/YOUR_USERNAME" target="_blank">GITHUB</a>
      <a href="https://linkedin.com/in/YOUR_HANDLE" target="_blank">LINKEDIN</a>
      <a href="https://twitter.com/YOUR_HANDLE" target="_blank">TWITTER / X</a>
    </div>
  </div>
</section>

<footer class="mono">© 2026 YOUR NAME — BUILT WITH GREAT POWER AND GREATER RESPONSIBILITY</footer>

<script>
// ---------- custom cursor ----------
const dot = document.getElementById('cursor-dot');
const ring = document.getElementById('cursor-ring');
let mx=innerWidth/2, my=innerHeight/2, rx=mx, ry=my;
document.addEventListener('mousemove', e=>{
  mx=e.clientX; my=e.clientY;
  dot.style.left=mx+'px'; dot.style.top=my+'px';
});
function animateRing(){
  rx += (mx-rx)*0.18; ry += (my-ry)*0.18;
  ring.style.left=rx+'px'; ring.style.top=ry+'px';
  requestAnimationFrame(animateRing);
}
animateRing();
document.querySelectorAll('a,.chip,.flip-card,.signal-btn').forEach(el=>{
  el.addEventListener('mouseenter',()=>{ring.style.width='54px';ring.style.height='54px';ring.style.borderColor='rgba(224,33,43,.9)';});
  el.addEventListener('mouseleave',()=>{ring.style.width='34px';ring.style.height='34px';ring.style.borderColor='rgba(224,33,43,.6)';});
});

// ---------- web-trail canvas ----------
const canvas = document.getElementById('web-canvas');
const ctx = canvas.getContext('2d');
function resize(){canvas.width=innerWidth;canvas.height=innerHeight;}
resize(); addEventListener('resize',resize);

let trail=[];
document.addEventListener('mousemove', e=>{
  trail.push({x:e.clientX,y:e.clientY,life:1});
  if(trail.length>18) trail.shift();
});

let shots=[]; // click web-shot ripples
document.addEventListener('click', e=>{
  shots.push({x:e.clientX,y:e.clientY,r:0,life:1});
});

function draw(){
  ctx.clearRect(0,0,canvas.width,canvas.height);

  // trail webbing
  ctx.strokeStyle='rgba(224,33,43,0.5)';
  ctx.lineWidth=1;
  for(let i=0;i<trail.length-1;i++){
    const p1=trail[i], p2=trail[i+1];
    ctx.globalAlpha = (i/trail.length) * 0.6;
    ctx.beginPath();
    ctx.moveTo(p1.x,p1.y);
    ctx.lineTo(p2.x,p2.y);
    ctx.stroke();
  }
  // occasional cross-links for web-like feel
  ctx.globalAlpha=0.25;
  for(let i=0;i<trail.length;i+=4){
    for(let j=i+4;j<trail.length;j+=4){
      const p1=trail[i], p2=trail[j];
      if(!p1||!p2) continue;
      const d=Math.hypot(p1.x-p2.x,p1.y-p2.y);
      if(d<120){
        ctx.beginPath();
        ctx.moveTo(p1.x,p1.y);
        ctx.lineTo(p2.x,p2.y);
        ctx.stroke();
      }
    }
  }
  ctx.globalAlpha=1;

  // click web-shot ripple
  shots.forEach(s=>{
    ctx.strokeStyle=`rgba(61,92,255,${s.life})`;
    ctx.lineWidth=2;
    ctx.beginPath();
    ctx.arc(s.x,s.y,s.r,0,Math.PI*2);
    ctx.stroke();
    // radiating web spokes
    for(let k=0;k<8;k++){
      const ang = (Math.PI*2/8)*k;
      ctx.beginPath();
      ctx.moveTo(s.x,s.y);
      ctx.lineTo(s.x+Math.cos(ang)*s.r, s.y+Math.sin(ang)*s.r);
      ctx.stroke();
    }
    s.r += 3.5;
    s.life -= 0.02;
  });
  shots = shots.filter(s=>s.life>0);

  requestAnimationFrame(draw);
}
draw();

// ---------- typewriter ----------
const lines = [
  "Full-stack developer.",
  "Debugger of production fires.",
  "With great power comes great commitology.",
  "Your friendly neighborhood engineer."
];
const tw = document.getElementById('typewriter');
let li=0, ci=0, deleting=false;
function type(){
  const current = lines[li];
  if(!deleting){
    ci++;
    tw.innerHTML = current.slice(0,ci) + '<span class="cursor-blink"></span>';
    if(ci===current.length){ deleting=true; setTimeout(type,1400); return; }
  } else {
    ci--;
    tw.innerHTML = current.slice(0,ci) + '<span class="cursor-blink"></span>';
    if(ci===0){ deleting=false; li=(li+1)%lines.length; }
  }
  setTimeout(type, deleting?35:65);
}
type();

// ---------- scroll reveal ----------
const io = new IntersectionObserver(entries=>{
  entries.forEach(en=>{ if(en.isIntersecting) en.target.classList.add('in'); });
},{threshold:0.15});
document.querySelectorAll('.reveal').forEach(el=>io.observe(el));

// ---------- counters ----------
const counters = document.querySelectorAll('[data-count]');
const cio = new IntersectionObserver(entries=>{
  entries.forEach(en=>{
    if(en.isIntersecting){
      const el=en.target, target=+el.dataset.count;
      let cur=0;
      const step=Math.max(1,Math.ceil(target/40));
      const tick=()=>{
        cur=Math.min(target,cur+step);
        el.textContent=cur;
        if(cur<target) requestAnimationFrame(tick);
      };
      tick();
      cio.unobserve(el);
    }
  });
},{threshold:0.5});
counters.forEach(c=>cio.observe(c));

// ---------- side nav active state + parallax skyline ----------
const navLinks = document.querySelectorAll('#side-nav a');
const sections = document.querySelectorAll('section');
const skyline = document.getElementById('skyline');
addEventListener('scroll',()=>{
  let idx=0;
  sections.forEach((s,i)=>{ if(scrollY >= s.offsetTop-innerHeight/2) idx=i; });
  navLinks.forEach(l=>l.classList.remove('active'));
  navLinks[idx].classList.add('active');
  if(skyline) skyline.style.transform = `translateY(${scrollY*0.08}px)`;
});
</script>
</body>
</html>
