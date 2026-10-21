<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Devendra Gupta — BIM Automation Dev</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;700&display=swap');
  :root {
    --bg:#050810;--surface:#0d1320;--card:#111827;
    --accent:#3b82f6;--accent2:#8b5cf6;--accent3:#06d6a0;
    --text:#e2e8f0;--muted:#64748b;--border:rgba(59,130,246,0.15);
  }
  *,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
  html{scroll-behavior:smooth;}
  body{background:var(--bg);color:var(--text);font-family:'Space Grotesk',sans-serif;overflow-x:hidden;min-height:100vh;}
  #bg-canvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:0;pointer-events:none;}
  .wrapper{position:relative;z-index:1;max-width:900px;margin:0 auto;padding:0 1.5rem 6rem;}
  .hero{min-height:100vh;display:flex;flex-direction:column;justify-content:center;padding:4rem 0 2rem;position:relative;}
  .hero-eyebrow{font-family:'JetBrains Mono',monospace;font-size:.75rem;color:var(--accent3);letter-spacing:.18em;margin-bottom:1.5rem;opacity:0;animation:fadeUp .6s .2s forwards;}
  .hero-name{font-size:clamp(2.8rem,8vw,5.5rem);font-weight:700;line-height:1.05;margin-bottom:1.2rem;opacity:0;animation:fadeUp .7s .35s forwards;}
  .hero-name span{background:linear-gradient(135deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
  .hero-tagline{font-size:clamp(1rem,2.5vw,1.3rem);color:var(--muted);max-width:62ch;line-height:1.65;margin-bottom:2.5rem;opacity:0;animation:fadeUp .7s .5s forwards;}
  .hero-tagline strong{color:var(--text);}
  .hero-links{display:flex;gap:1rem;flex-wrap:wrap;opacity:0;animation:fadeUp .7s .65s forwards;}
  .btn{display:inline-flex;align-items:center;gap:.5rem;padding:.65rem 1.4rem;border-radius:8px;font-size:.9rem;font-weight:500;text-decoration:none;transition:transform .2s,box-shadow .2s;cursor:pointer;border:none;}
  .btn-primary{background:linear-gradient(135deg,var(--accent),var(--accent2));color:#fff;}
  .btn-ghost{background:transparent;border:1px solid var(--border);color:var(--text);}
  .btn:hover{transform:translateY(-2px);box-shadow:0 8px 24px rgba(59,130,246,.2);}
  .building-canvas-wrap{position:absolute;right:-60px;top:50%;transform:translateY(-50%);width:340px;height:380px;opacity:0;animation:fadeIn 1s .8s forwards;}
  #building-canvas{width:100%;height:100%;}
  .section{margin-top:6rem;}
  .section-label{font-family:'JetBrains Mono',monospace;font-size:.7rem;color:var(--accent3);letter-spacing:.2em;margin-bottom:.5rem;}
  .section-title{font-size:clamp(1.5rem,3.5vw,2.2rem);font-weight:700;margin-bottom:2rem;line-height:1.2;}
  .orbit-wrap{position:relative;height:320px;margin:2rem 0;display:flex;align-items:center;justify-content:center;}
  #orbit-canvas{width:100%;height:100%;max-width:700px;}
  .exp-card{border:1px solid var(--border);border-radius:12px;padding:1.6rem;background:var(--card);position:relative;overflow:hidden;transition:border-color .3s,transform .3s;}
  .exp-card::before{content:'';position:absolute;inset:0;background:linear-gradient(135deg,rgba(59,130,246,.04),rgba(139,92,246,.04));opacity:0;transition:opacity .3s;}
  .exp-card:hover{border-color:rgba(59,130,246,.35);transform:translateY(-3px);}
  .exp-card:hover::before{opacity:1;}
  .exp-role{font-size:1.05rem;font-weight:600;margin-bottom:.25rem;}
  .exp-meta{font-family:'JetBrains Mono',monospace;font-size:.75rem;color:var(--accent);margin-bottom:1rem;}
  .exp-list{list-style:none;display:flex;flex-direction:column;gap:.55rem;}
  .exp-list li{font-size:.9rem;color:#94a3b8;padding-left:1.2rem;position:relative;line-height:1.5;}
  .exp-list li::before{content:'▸';position:absolute;left:0;color:var(--accent3);font-size:.7rem;top:.15rem;}
  .projects-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(min(100%,420px),1fr));gap:1.2rem;}
  .proj-card{border:1px solid var(--border);border-radius:12px;padding:1.5rem;background:var(--card);cursor:default;transition:border-color .25s,transform .25s,box-shadow .25s;position:relative;overflow:hidden;}
  .proj-card::after{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,var(--accent),var(--accent2),var(--accent3));opacity:0;transition:opacity .3s;}
  .proj-card:hover{border-color:rgba(59,130,246,.4);transform:translateY(-4px);box-shadow:0 12px 32px rgba(0,0,0,.35);}
  .proj-card:hover::after{opacity:1;}
  .proj-title{font-size:1rem;font-weight:600;margin-bottom:.6rem;}
  .proj-desc{font-size:.85rem;color:#94a3b8;line-height:1.6;margin-bottom:1rem;}
  .proj-tags{display:flex;flex-wrap:wrap;gap:.4rem;}
  .tag{font-family:'JetBrains Mono',monospace;font-size:.7rem;padding:.2rem .55rem;border-radius:4px;background:rgba(59,130,246,.1);color:var(--accent);border:1px solid rgba(59,130,246,.2);}
  .tag.green{background:rgba(6,214,160,.08);color:var(--accent3);border-color:rgba(6,214,160,.2);}
  .tag.purple{background:rgba(139,92,246,.1);color:#a78bfa;border-color:rgba(139,92,246,.2);}
  .stats-row{display:grid;grid-template-columns:repeat(auto-fill,minmax(160px,1fr));gap:1rem;margin-top:2rem;}
  .stat-box{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:1.2rem;text-align:center;transition:border-color .25s,transform .25s;}
  .stat-box:hover{border-color:rgba(59,130,246,.4);transform:translateY(-2px);}
  .stat-num{font-size:1.9rem;font-weight:700;background:linear-gradient(135deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;line-height:1.1;margin-bottom:.35rem;}
  .stat-label{font-size:.78rem;color:var(--muted);line-height:1.4;}
  .clash-wrap{background:var(--card);border:1px solid var(--border);border-radius:12px;overflow:hidden;margin-top:1rem;}
  .clash-header{padding:.8rem 1.2rem;border-bottom:1px solid var(--border);display:flex;align-items:center;gap:.5rem;font-family:'JetBrains Mono',monospace;font-size:.75rem;color:var(--muted);}
  .dot{width:10px;height:10px;border-radius:50%;}
  .dot-red{background:#ef4444;}.dot-yellow{background:#f59e0b;}.dot-green{background:#22c55e;}
  #clash-canvas{display:block;width:100%;height:280px;}
  .about-grid{display:grid;grid-template-columns:1fr 1fr;gap:1rem;}
  .about-item{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:1.2rem;font-size:.88rem;line-height:1.6;color:#94a3b8;}
  .about-item strong{color:var(--text);display:block;margin-bottom:.3rem;}
  .terminal{background:#0a0e1a;border:1px solid rgba(59,130,246,.2);border-radius:10px;overflow:hidden;margin-top:1rem;font-family:'JetBrains Mono',monospace;font-size:.82rem;}
  .terminal-bar{background:#131929;padding:.6rem 1rem;display:flex;align-items:center;gap:.5rem;border-bottom:1px solid rgba(59,130,246,.1);}
  .terminal-body{padding:1.2rem;color:#94a3b8;line-height:1.8;}
  .t-green{color:#22c55e;}.t-blue{color:#60a5fa;}.t-purple{color:#c084fc;}.t-yellow{color:#facc15;}.t-muted{color:#475569;}
  .footer{margin-top:6rem;padding-top:2rem;border-top:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:1rem;font-size:.82rem;color:var(--muted);}
  .footer a{color:var(--accent);text-decoration:none;}
  @keyframes fadeUp{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
  @keyframes fadeIn{from{opacity:0}to{opacity:1}}
  .reveal{opacity:0;transform:translateY(24px);transition:opacity .6s ease,transform .6s ease;}
  .reveal.visible{opacity:1;transform:translateY(0);}
  .cursor-glow{position:fixed;width:400px;height:400px;border-radius:50%;background:radial-gradient(circle,rgba(59,130,246,.06) 0%,transparent 70%);pointer-events:none;transform:translate(-50%,-50%);z-index:0;transition:opacity .3s;}
  @media(max-width:600px){.about-grid{grid-template-columns:1fr}.building-canvas-wrap{display:none}.hero-name{font-size:2.5rem}}
</style>
</head>
<body>
<div class="cursor-glow" id="cursorGlow"></div>
<canvas id="bg-canvas"></canvas>
<div class="wrapper">

  <section class="hero">
    <div class="hero-eyebrow">// hello world &middot; jamshedpur, india &middot; open to work</div>
    <h1 class="hero-name">Devendra<br><span>Gupta</span></h1>
    <p class="hero-tagline">
      Civil Engineering student who got tired of clicking buttons manually &mdash; so I taught Revit to click them for me.
      I build <strong>BIM automation tools</strong>, wire up <strong>AI pipelines</strong>, and turn 3-day RFI workflows
      into a 3-minute script. Currently breaking things at <strong>BMSI</strong> (part-time).
    </p>
    <div class="hero-links">
      <a href="mailto:devendragupta56789@gmail.com" class="btn btn-primary">&#128140; Email Me</a>
      <a href="https://linkedin.com/in/devendra-gupta-b943b4377" target="_blank" class="btn btn-ghost">LinkedIn &rarr;</a>
    </div>
    <div class="building-canvas-wrap"><canvas id="building-canvas"></canvas></div>
  </section>

  <section class="section reveal">
    <div class="section-label">BY THE NUMBERS</div>
    <h2 class="section-title">What I've actually done</h2>
    <div class="stats-row">
      <div class="stat-box"><div class="stat-num">30+</div><div class="stat-label">Revit AI Copilot features</div></div>
      <div class="stat-box"><div class="stat-num">12</div><div class="stat-label">Revit models built self-taught</div></div>
      <div class="stat-box"><div class="stat-num">2333</div><div class="stat-label">Pages ingested in my BIM RAG chatbot</div></div>
      <div class="stat-box"><div class="stat-num">2&ndash;3</div><div class="stat-label">Days saved per clash cycle</div></div>
      <div class="stat-box"><div class="stat-num">25+</div><div class="stat-label">IoT sensors in my Digital Twin</div></div>
      <div class="stat-box"><div class="stat-num">2nd</div><div class="stat-label">State AutoCAD Competition, JH 2025</div></div>
    </div>
  </section>

  <section class="section reveal">
    <div class="section-label">TECH STACK</div>
    <h2 class="section-title">What I work with<br>(quite a lot, honestly)</h2>
    <div class="orbit-wrap"><canvas id="orbit-canvas"></canvas></div>
  </section>

  <section class="section reveal">
    <div class="section-label">EXPERIENCE</div>
    <h2 class="section-title">Where I've been breaking things</h2>
    <div class="exp-card">
      <div class="exp-role">BIM Automation Consultant</div>
      <div class="exp-meta">BMSI &middot; Gurugram &middot; 2026 &ndash; Present &middot; Part-time</div>
      <ul class="exp-list">
        <li>Build Python + Revit API tools that delete repetitive tasks from the engineering team's existence</li>
        <li>Maintain the Electrical Automation Suite (PyRevit) &mdash; it runs, nobody panics, I sleep well</li>
        <li>Sit with project engineers, find the boring stuff they do every day, and write scripts for it</li>
        <li>BIM model QA, clash detection and LOD checks on live projects &mdash; keeping things from literally colliding</li>
      </ul>
    </div>
  </section>

  <section class="section reveal">
    <div class="section-label">A QUICK INTRO</div>
    <h2 class="section-title">The kind of engineer I am</h2>
    <div class="terminal">
      <div class="terminal-bar">
        <div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div>
        <span style="margin-left:.5rem;color:#475569;">devendra@bim-lab ~ </span>
      </div>
      <div class="terminal-body">
        <span class="t-green">$</span> <span class="t-blue">whoami</span><br>
        &rarr; Civil Engg student &middot; BIM Automation Dev &middot; part-time consultant at BMSI<br><br>
        <span class="t-green">$</span> <span class="t-blue">cat philosophy.txt</span><br>
        &rarr; If I do the same task twice, I automate it on the third attempt.<br>
        &rarr; If Revit can do it with one click, it should do it with zero clicks.<br>
        &rarr; AI should write the RFI. I should drink tea and review it.<br><br>
        <span class="t-green">$</span> <span class="t-blue">ls skills/</span><br>
        <span class="t-purple">revit-api/</span>&nbsp;&nbsp;<span class="t-purple">pyrevit/</span>&nbsp;&nbsp;<span class="t-purple">python/</span>&nbsp;&nbsp;<span class="t-yellow">n8n/</span>&nbsp;&nbsp;<span class="t-yellow">langchain/</span>&nbsp;&nbsp;<span class="t-purple">react/</span><br>
        <span class="t-purple">three-js/</span>&nbsp;&nbsp;<span class="t-yellow">groq-ai/</span>&nbsp;&nbsp;<span class="t-purple">fastapi/</span>&nbsp;&nbsp;<span class="t-yellow">lora-finetuning/</span>&nbsp;&nbsp;<span class="t-purple">navisworks/</span><br><br>
        <span class="t-green">$</span> <span class="t-blue">echo $STATUS</span><br>
        &rarr; 2nd year B.Tech &middot; building things that shouldn't exist yet &middot; available for collabs<br><br>
        <span class="t-muted"># yes, I'm a 2025 engineering student doing this stuff. It surprised me too.</span>
      </div>
    </div>
  </section>

  <section class="section reveal">
    <div class="section-label">PROJECTS</div>
    <h2 class="section-title">Things I built that actually work</h2>
    <div class="projects-grid">

      <div class="proj-card">
        <div class="proj-title">&#9889; Clash-to-RFI Automation System</div>
        <p class="proj-desc">Navisworks spits out an XML clash report. My system reads it, figures out which discipline owns each clash, has Groq AI write a proper RFI citing IS 456 and NBC 2016, and emails it to the right engineer. What used to take 2&ndash;3 days of manual writing now takes minutes. 3D React dashboard with Three.js shows every clash location live. Also fine-tuned a model on real BIM RFI documents using LoRA.</p>
        <div class="proj-tags">
          <span class="tag">Python</span><span class="tag purple">Groq AI</span><span class="tag">n8n</span>
          <span class="tag">React</span><span class="tag purple">Three.js</span><span class="tag green">LoRA</span>
          <span class="tag">HuggingFace</span>
        </div>
      </div>

      <div class="proj-card">
        <div class="proj-title">&#128268; Electrical Automation Suite (PyRevit)</div>
        <p class="proj-desc">Three tools the BMSI electrical team uses daily. Conduit Bend Checker flags anything above the allowed angle. Conduit Spacing Checker highlights clearance violations. Excel-to-Schedule builds a Revit schedule from a spreadsheet with zero manual data entry. Used in production. Not a demo.</p>
        <div class="proj-tags">
          <span class="tag">PyRevit</span><span class="tag">Revit API</span><span class="tag">Python</span><span class="tag">Excel</span>
        </div>
      </div>

      <div class="proj-card">
        <div class="proj-title">&#129302; AI Copilot for Revit (v0.15)</div>
        <p class="proj-desc">30+ features living inside Revit itself. Model health score out of 100, one-click clash fix, smart renumbering, auto-dimension, auto-tag, auto-join cleanup. Generative MEP routing. Code-based clearance zones. And yes &mdash; it converts a 2D floor plan image into a 3D Revit model. Autodesk did not expect this.</p>
        <div class="proj-tags">
          <span class="tag">Python</span><span class="tag">Revit API</span><span class="tag purple">Gen AI</span>
        </div>
      </div>

      <div class="proj-card">
        <div class="proj-title">&#127970; AI Digital Twin Dashboard</div>
        <p class="proj-desc">G+2 building modeled in Revit, rebuilt in Three.js. 25 simulated IoT sensors over MQTT sending temperature, CO2, vibration and energy data. Isolation Forest anomaly detection fires alerts in real time. Live building health score. Natural-language query box. JUT Civil Department asked for it. I said sure.</p>
        <div class="proj-tags">
          <span class="tag">React</span><span class="tag purple">Three.js</span><span class="tag">Python</span>
          <span class="tag green">scikit-learn</span><span class="tag">MQTT</span>
        </div>
      </div>

      <div class="proj-card">
        <div class="proj-title">&#128218; BIM AI Assistant (RAG)</div>
        <p class="proj-desc">Ask it anything about ISO 19650, LOD Spec, Revit, or Navisworks. It has read 2,333 pages so you don't have to. Engineers can upload their own project PDFs too, stored locally. Built with LangChain + ChromaDB + Groq, deployed on Streamlit.</p>
        <div class="proj-tags">
          <span class="tag">LangChain</span><span class="tag">ChromaDB</span><span class="tag purple">Groq</span>
          <span class="tag green">RAG</span><span class="tag">Streamlit</span>
        </div>
      </div>

      <div class="proj-card">
        <div class="proj-title">&#128208; 12-Model Revit Portfolio</div>
        <p class="proj-desc">25-storey high-rise. 100-suite hotel. G+7 RCC building. G+3 house with full rebar detailing. All self-taught, all built to proper LOD specs. Sometimes you just have to sit down and go model things for three weeks straight.</p>
        <div class="proj-tags">
          <span class="tag">Autodesk Revit</span><span class="tag">LOD</span><span class="tag green">Rebar</span>
        </div>
      </div>

    </div>
  </section>

  <section class="section reveal">
    <div class="section-label">INTERACTIVE</div>
    <h2 class="section-title">Live Clash Visualizer<br><span style="font-size:.9rem;color:var(--muted);font-weight:400;">(mini demo of what the automation system shows)</span></h2>
    <div class="clash-wrap">
      <div class="clash-header">
        <div class="dot dot-red"></div><div class="dot dot-yellow"></div><div class="dot dot-green"></div>
        <span style="margin-left:.5rem;">clash_report_live.xml &mdash; 12 clashes detected</span>
      </div>
      <canvas id="clash-canvas"></canvas>
    </div>
  </section>

  <section class="section reveal">
    <div class="section-label">THE HUMAN BEHIND THE SCRIPTS</div>
    <h2 class="section-title">A few things worth knowing</h2>
    <div class="about-grid">
      <div class="about-item"><strong>&#127979; Currently studying</strong>B.Tech Civil Engineering at Jharkhand University of Technology (2025&ndash;2029). Member of the college technical club. Somehow also consulting part-time. Sleep is optional apparently.</div>
      <div class="about-item"><strong>&#127942; Won something</strong>2nd Runner-Up at the State-Level AutoCAD Competition, Jharkhand 2025. Not bad for someone who then immediately went and automated half of what the competition tested.</div>
      <div class="about-item"><strong>&#128205; Based in</strong>Jamshedpur, Jharkhand &mdash; the steel city. Appropriate given I spend most of my time dealing with structural models. Drop me an email or find me on LinkedIn.</div>
      <div class="about-item"><strong>&#128295; Current obsession</strong>Making Revit do things Autodesk didn't intend. If the API allows it and it saves someone an afternoon of clicking, I'm building it.</div>
    </div>
  </section>

  <footer class="footer">
    <span>Devendra Gupta &middot; Jamshedpur, Jharkhand</span>
    <span><a href="mailto:devendragupta56789@gmail.com">devendragupta56789@gmail.com</a> &middot; <a href="https://linkedin.com/in/devendra-gupta-b943b4377" target="_blank">LinkedIn</a></span>
  </footer>
</div>

<script>
const glow=document.getElementById('cursorGlow');
document.addEventListener('mousemove',e=>{glow.style.left=e.clientX+'px';glow.style.top=e.clientY+'px';});

const reveals=document.querySelectorAll('.reveal');
const observer=new IntersectionObserver(entries=>{entries.forEach(e=>{if(e.isIntersecting)e.target.classList.add('visible');});},{threshold:.1});
reveals.forEach(r=>observer.observe(r));

// PARTICLE BG
(function(){
  const canvas=document.getElementById('bg-canvas');
  const ctx=canvas.getContext('2d');
  let W,H,particles=[];
  function resize(){W=canvas.width=window.innerWidth;H=canvas.height=window.innerHeight;}
  function makeP(){return{x:Math.random()*W,y:Math.random()*H,vx:(Math.random()-.5)*.25,vy:(Math.random()-.5)*.25,r:Math.random()*1.5+.3,alpha:Math.random()*.4+.1};}
  function init(){resize();particles=Array.from({length:120},makeP);}
  function tick(){
    ctx.clearRect(0,0,W,H);
    for(let i=0;i<particles.length;i++){
      for(let j=i+1;j<particles.length;j++){
        const dx=particles[i].x-particles[j].x,dy=particles[i].y-particles[j].y;
        const d=Math.sqrt(dx*dx+dy*dy);
        if(d<130){ctx.beginPath();ctx.strokeStyle=`rgba(59,130,246,${.06*(1-d/130)})`;ctx.lineWidth=.5;ctx.moveTo(particles[i].x,particles[i].y);ctx.lineTo(particles[j].x,particles[j].y);ctx.stroke();}
      }
    }
    particles.forEach(p=>{
      p.x+=p.vx;p.y+=p.vy;
      if(p.x<0||p.x>W)p.vx*=-1;if(p.y<0||p.y>H)p.vy*=-1;
      ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,Math.PI*2);ctx.fillStyle=`rgba(59,130,246,${p.alpha})`;ctx.fill();
    });
    requestAnimationFrame(tick);
  }
  window.addEventListener('resize',resize);
  init();tick();
})();

// 3D BUILDING
(function(){
  const canvas=document.getElementById('building-canvas');
  if(!canvas)return;
  const ctx=canvas.getContext('2d');
  canvas.width=340;canvas.height=380;
  let angle=0;
  function project(x,y,z,a){
    const cos=Math.cos(a),sin=Math.sin(a);
    const rx=x*cos-z*sin,rz=x*sin+z*cos;
    const scale=200/(200+rz+60);
    return{sx:170+rx*scale*55,sy:310+y*scale*55,scale};
  }
  function drawFace(corners,fill,stroke){
    ctx.beginPath();ctx.moveTo(corners[0].sx,corners[0].sy);
    for(let i=1;i<corners.length;i++)ctx.lineTo(corners[i].sx,corners[i].sy);
    ctx.closePath();
    if(fill){ctx.fillStyle=fill;ctx.fill();}
    if(stroke){ctx.strokeStyle=stroke;ctx.lineWidth=.8;ctx.stroke();}
  }
  function tick(){
    ctx.clearRect(0,0,340,380);
    angle+=.008;
    const floors=12,bw=1.2,bd=.7,fh=.4;
    for(let f=0;f<floors;f++){
      const y0=-(f*fh),y1=-(f*fh+fh);
      const pts=[[-bw,y0,-bd],[bw,y0,-bd],[bw,y0,bd],[-bw,y0,bd],[-bw,y1,-bd],[bw,y1,-bd],[bw,y1,bd],[-bw,y1,bd]].map(([x,y,z])=>project(x,y,z,angle));
      const l=12+f*4;
      drawFace([pts[1],pts[2],pts[6],pts[5]],`rgb(${l},${l+8},${l+20})`,'rgba(59,130,246,.12)');
      drawFace([pts[0],pts[1],pts[5],pts[4]],`rgb(${l+8},${l+14},${l+28})`,'rgba(59,130,246,.1)');
      if(f===floors-1)drawFace([pts[4],pts[5],pts[6],pts[7]],'rgba(59,130,246,.3)','rgba(59,130,246,.25)');
      for(let w=0;w<3;w++){
        const wx=-.7+w*.7;
        const wp=[[wx-.18,y1+fh*.15,-bd-.01],[wx+.18,y1+fh*.15,-bd-.01],[wx+.18,y1+fh*.75,-bd-.01],[wx-.18,y1+fh*.75,-bd-.01]].map(([x,y,z])=>project(x,y,z,angle));
        drawFace(wp,f%3===0&&w%2===0?'rgba(250,204,21,.75)':'rgba(20,30,60,.95)',null);
      }
    }
    const grad=ctx.createRadialGradient(170,315,0,170,315,90);
    grad.addColorStop(0,'rgba(59,130,246,.12)');grad.addColorStop(1,'transparent');
    ctx.fillStyle=grad;ctx.fillRect(80,270,180,80);
    requestAnimationFrame(tick);
  }
  tick();
})();

// ORBIT
(function(){
  const canvas=document.getElementById('orbit-canvas');
  if(!canvas)return;
  const ctx=canvas.getContext('2d');
  function resize(){canvas.width=canvas.offsetWidth;canvas.height=canvas.offsetHeight||320;}
  resize();window.addEventListener('resize',resize);
  const skills=[{name:'Revit API',color:'#3b82f6'},{name:'PyRevit',color:'#8b5cf6'},{name:'Python',color:'#06d6a0'},{name:'n8n',color:'#f59e0b'},{name:'LangChain',color:'#3b82f6'},{name:'Three.js',color:'#8b5cf6'},{name:'React',color:'#06d6a0'},{name:'Groq AI',color:'#f59e0b'},{name:'Navisworks',color:'#3b82f6'},{name:'Dynamo',color:'#8b5cf6'},{name:'FastAPI',color:'#06d6a0'},{name:'LoRA ft.',color:'#f59e0b'}];
  const rings=[{r:90,speed:.004,items:skills.slice(0,4)},{r:148,speed:-.003,items:skills.slice(4,8)},{r:205,speed:.002,items:skills.slice(8,12)}];
  rings.forEach(ring=>{ring.items=ring.items.map((s,i)=>({...s,angle:(i/ring.items.length)*Math.PI*2}));});
  function tick(){
    const W=canvas.width,H=canvas.height;
    ctx.clearRect(0,0,W,H);
    const cx=W/2,cy=H/2;
    const cg=ctx.createRadialGradient(cx,cy,0,cx,cy,32);
    cg.addColorStop(0,'rgba(59,130,246,.5)');cg.addColorStop(1,'transparent');
    ctx.beginPath();ctx.arc(cx,cy,28,0,Math.PI*2);ctx.fillStyle=cg;ctx.fill();
    ctx.fillStyle='#e2e8f0';ctx.font='bold 9px Space Grotesk,sans-serif';ctx.textAlign='center';ctx.textBaseline='middle';
    ctx.fillText('DEVENDRA',cx,cy-5);ctx.fillText('GUPTA',cx,cy+7);
    rings.forEach(ring=>{
      ctx.beginPath();ctx.arc(cx,cy,ring.r,0,Math.PI*2);ctx.strokeStyle='rgba(59,130,246,.08)';ctx.lineWidth=1;ctx.stroke();
      ring.items.forEach(s=>{
        s.angle+=ring.speed;
        const x=cx+Math.cos(s.angle)*ring.r,y=cy+Math.sin(s.angle)*ring.r;
        ctx.beginPath();ctx.moveTo(cx,cy);ctx.lineTo(x,y);ctx.strokeStyle=s.color+'18';ctx.lineWidth=.7;ctx.stroke();
        ctx.beginPath();ctx.arc(x,y,26,0,Math.PI*2);ctx.fillStyle='#111827';ctx.fill();ctx.strokeStyle=s.color+'55';ctx.lineWidth=1;ctx.stroke();
        ctx.fillStyle=s.color;ctx.font='9px Space Grotesk,sans-serif';ctx.textAlign='center';ctx.textBaseline='middle';
        ctx.fillText(s.name,x,y);
      });
    });
    requestAnimationFrame(tick);
  }
  tick();
})();

// CLASH VISUALIZER
(function(){
  const canvas=document.getElementById('clash-canvas');
  if(!canvas)return;
  const ctx=canvas.getContext('2d');
  canvas.width=canvas.offsetWidth||860;canvas.height=280;
  window.addEventListener('resize',()=>{canvas.width=canvas.offsetWidth;});
  const clashes=[
    {x:.2,y:.35,z:.1,sev:'critical',label:'Duct vs Beam'},
    {x:.55,y:.5,z:.3,sev:'high',label:'Column vs Pipe'},
    {x:.75,y:.25,z:-.1,sev:'critical',label:'Conduit vs Slab'},
    {x:-.3,y:.45,z:.2,sev:'medium',label:'Rebar vs Pipe'},
    {x:.1,y:.6,z:-.4,sev:'high',label:'Cable Tray vs Beam'},
    {x:-.5,y:.3,z:.3,sev:'medium',label:'Wall vs Duct'},
    {x:.4,y:.7,z:.0,sev:'critical',label:'Sprinkler vs Column'},
    {x:-.1,y:.2,z:-.2,sev:'low',label:'Footing vs Pipe'},
    {x:.6,y:.4,z:.5,sev:'high',label:'Duct vs Duct'},
    {x:-.4,y:.65,z:-.3,sev:'medium',label:'Slab vs Cable'},
    {x:.3,y:.1,z:.4,sev:'low',label:'Grade Beam vs Pipe'},
    {x:-.2,y:.8,z:.1,sev:'critical',label:'Valve vs Beam'},
  ];
  const sc={critical:'#ef4444',high:'#f97316',medium:'#f59e0b',low:'#22c55e'};
  let angle=0,hovered=null,pulse=0;
  let projected=[];
  function project(x,y,z,a){
    const cos=Math.cos(a),sin=Math.sin(a);
    const rx=x*cos-z*sin,rz=x*sin+z*cos;
    const scale=180/(180+rz+2);
    const W=canvas.width,H=canvas.height;
    return{sx:W/2+rx*scale*120,sy:H/2+y*scale*90-30,scale,rz};
  }
  canvas.addEventListener('mousemove',e=>{
    const rect=canvas.getBoundingClientRect();
    const mx=e.clientX-rect.left,my=e.clientY-rect.top;
    hovered=null;
    projected.forEach((p,i)=>{const dx=p.sx-mx,dy=p.sy-my;if(Math.sqrt(dx*dx+dy*dy)<18)hovered=i;});
  });
  function tick(){
    const W=canvas.width,H=canvas.height;
    ctx.clearRect(0,0,W,H);
    angle+=.006;pulse+=.05;
    for(let gx=-3;gx<=3;gx++){for(let gz=-3;gz<=3;gz++){
      const a=project(gx*.25,.9,gz*.25,angle),b=project((gx+1)*.25,.9,gz*.25,angle),c=project(gx*.25,.9,(gz+1)*.25,angle);
      ctx.beginPath();ctx.moveTo(a.sx,a.sy);ctx.lineTo(b.sx,b.sy);ctx.strokeStyle='rgba(59,130,246,.07)';ctx.lineWidth=.7;ctx.stroke();
      ctx.beginPath();ctx.moveTo(a.sx,a.sy);ctx.lineTo(c.sx,c.sy);ctx.stroke();
    }}
    projected=clashes.map((cl,i)=>({...project(cl.x,cl.y-.7,cl.z,angle),i,cl})).sort((a,b)=>b.rz-a.rz);
    projected.forEach(p=>{
      const{sx,sy,scale,cl,i}=p;
      const r=(hovered===i?14:10)*scale;
      const color=sc[cl.sev];
      if(cl.sev==='critical'){
        const pr=r*(1.8+Math.sin(pulse+i)*.6);
        ctx.beginPath();ctx.arc(sx,sy,pr,0,Math.PI*2);ctx.fillStyle=color+'20';ctx.fill();
      }
      ctx.beginPath();ctx.arc(sx,sy,r,0,Math.PI*2);ctx.fillStyle=color+'cc';ctx.fill();
      ctx.strokeStyle=color;ctx.lineWidth=1.2;ctx.stroke();
      if(hovered===i){
        ctx.fillStyle='#1e293b';
        ctx.font='10px Space Grotesk,sans-serif';ctx.textAlign='center';ctx.textBaseline='middle';
        const tw=ctx.measureText(cl.label).width+16;
        const tx=Math.min(Math.max(sx-tw/2,4),W-tw-4),ty=sy-r-28;
        ctx.beginPath();ctx.roundRect(tx,ty,tw,22,4);ctx.fill();
        ctx.strokeStyle=color;ctx.lineWidth=1;ctx.stroke();
        ctx.fillStyle='#e2e8f0';ctx.fillText(cl.label,tx+tw/2,ty+11);
      }
    });
    ['critical','high','medium','low'].forEach((s,i)=>{
      const lx=12+i*90,ly=H-22;
      ctx.beginPath();ctx.arc(lx+6,ly,5,0,Math.PI*2);ctx.fillStyle=sc[s];ctx.fill();
      ctx.fillStyle='#64748b';ctx.font='10px Space Grotesk,sans-serif';ctx.textAlign='left';ctx.textBaseline='middle';
      ctx.fillText(s,lx+14,ly);
    });
    requestAnimationFrame(tick);
  }
  tick();
})();
</script>
</body>
</html>
