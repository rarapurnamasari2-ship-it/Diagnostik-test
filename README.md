<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>English Diagnostic Assessment — Grade 11</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Bitter:ital,wght@0,500;0,700;0,800;1,500&family=Space+Grotesk:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
  :root{
    --paper: #F7F0E1;
    --paper-white: #FFFDF7;
    --ink-green: #24402F;
    --ink-green-2: #2F5240;
    --pencil: #38352C;
    --pen-red: #C1440E;
    --highlight: #E8B33D;
    --line: rgba(36,64,47,0.16);
    --shadow: rgba(36,64,47,0.25);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(circle at 15% 10%, rgba(232,179,61,0.10), transparent 40%),
      var(--ink-green);
    background-color:var(--ink-green);
    min-height:100vh;
    font-family:'Space Grotesk', sans-serif;
    color:var(--pencil);
    display:flex;
    align-items:flex-start;
    justify-content:center;
    padding:32px 16px 64px;
  }
  .stage{
    width:100%;
    max-width:760px;
  }
  .masthead{
    text-align:center;
    color:var(--paper);
    margin-bottom:20px;
  }
  .masthead .eyebrow{
    font-family:'IBM Plex Mono', monospace;
    letter-spacing:.18em;
    font-size:11px;
    text-transform:uppercase;
    color:var(--highlight);
    display:block;
    margin-bottom:6px;
  }
  .masthead h1{
    font-family:'Bitter', serif;
    font-weight:800;
    font-size:clamp(24px,4vw,34px);
    margin:0;
  }
  .sheet{
    background:var(--paper);
    border-radius:6px;
    box-shadow:0 24px 60px var(--shadow), 0 2px 0 rgba(255,255,255,0.4) inset;
    position:relative;
    overflow:hidden;
    padding:34px 30px 30px;
  }
  .sheet::before{
    content:"";
    position:absolute;
    left:0; top:0; bottom:0;
    width:38px;
    background:repeating-linear-gradient(
      to bottom, transparent 0 26px, var(--line) 26px 27px
    );
    border-right:2px solid var(--pen-red);
    opacity:.55;
  }
  .sheet-inner{ position:relative; padding-left:26px; }

  /* ---------- Bubble progress (signature element) ---------- */
  .bubble-track{
    display:flex;
    flex-wrap:wrap;
    gap:5px;
    margin-bottom:22px;
    padding-bottom:14px;
    border-bottom:1px dashed var(--line);
  }
  .bubble{
    width:19px; height:19px;
    border-radius:50%;
    border:1.6px solid var(--ink-green-2);
    display:flex; align-items:center; justify-content:center;
    font-family:'IBM Plex Mono',monospace;
    font-size:8px;
    color:var(--ink-green-2);
    background:transparent;
    transition:all .2s ease;
  }
  .bubble.answered{ background:var(--ink-green-2); color:var(--paper); }
  .bubble.current{ border-color:var(--pen-red); box-shadow:0 0 0 3px rgba(193,68,14,0.18); }

  /* ---------- Start screen ---------- */
  .field{ margin-bottom:18px; }
  .field label{
    display:block;
    font-family:'IBM Plex Mono', monospace;
    font-size:11px;
    letter-spacing:.1em;
    text-transform:uppercase;
    color:var(--ink-green-2);
    margin-bottom:6px;
  }
  .field input, .field select{
    width:100%;
    font-family:'Space Grotesk', sans-serif;
    font-size:17px;
    padding:11px 4px;
    border:none;
    border-bottom:2px solid var(--ink-green-2);
    background:transparent;
    color:var(--pencil);
    outline:none;
  }
  .field input:focus, .field select:focus{ border-bottom-color:var(--pen-red); }
  .intro-text{ line-height:1.6; font-size:15px; color:var(--pencil); margin:0 0 22px; }
  .intro-text strong{ color:var(--ink-green); }

  .btn{
    font-family:'IBM Plex Mono', monospace;
    letter-spacing:.05em;
    text-transform:uppercase;
    font-size:12.5px;
    font-weight:600;
    border:none;
    padding:14px 22px;
    border-radius:3px;
    cursor:pointer;
    transition:transform .12s ease, box-shadow .12s ease;
  }
  .btn:active{ transform:translateY(1px); }
  .btn-primary{ background:var(--pen-red); color:var(--paper-white); box-shadow:0 4px 0 #8f3208; }
  .btn-primary:hover{ box-shadow:0 5px 0 #8f3208; transform:translateY(-1px); }
  .btn-ghost{ background:transparent; color:var(--ink-green-2); border:1.6px solid var(--ink-green-2); }
  .btn-ghost:hover{ background:rgba(47,82,64,0.08); }
  .btn:disabled{ opacity:.4; cursor:not-allowed; box-shadow:none; }
  .btn-row{ display:flex; gap:10px; margin-top:24px; justify-content:space-between; }
  .btn-row .right{ display:flex; gap:10px; }

  /* ---------- Quiz screen ---------- */
  .q-meta{
    display:flex; justify-content:space-between; align-items:baseline;
    margin-bottom:10px;
  }
  .q-count{ font-family:'IBM Plex Mono',monospace; font-size:12px; color:var(--ink-green-2); }
  .q-tag{
    font-family:'IBM Plex Mono',monospace; font-size:10.5px;
    letter-spacing:.08em; text-transform:uppercase;
    background:var(--ink-green); color:var(--highlight);
    padding:3px 9px; border-radius:20px;
  }
  .passage{
    background:var(--paper-white);
    border-left:3px solid var(--highlight);
    padding:14px 16px;
    font-size:14.5px;
    line-height:1.65;
    margin-bottom:16px;
    border-radius:2px;
    font-style:italic;
    color:#413d31;
  }
  .q-text{
    font-family:'Bitter', serif;
    font-weight:600;
    font-size:19px;
    line-height:1.45;
    margin:0 0 18px;
    color:var(--ink-green);
  }
  .options{ display:flex; flex-direction:column; gap:10px; }
  .option{
    display:flex; align-items:center; gap:12px;
    padding:12px 14px;
    background:var(--paper-white);
    border:1.6px solid transparent;
    border-radius:4px;
    cursor:pointer;
    font-size:15px;
    transition:border-color .12s ease, background .12s ease;
  }
  .option:hover{ border-color:var(--ink-green-2); }
  .option.selected{ border-color:var(--pen-red); background:#fff8ee; }
  .option .letter{
    font-family:'IBM Plex Mono',monospace;
    font-weight:600;
    width:24px; height:24px;
    border-radius:50%;
    border:1.6px solid var(--ink-green-2);
    display:flex; align-items:center; justify-content:center;
    font-size:12px;
    flex-shrink:0;
    color:var(--ink-green-2);
  }
  .option.selected .letter{ background:var(--pen-red); border-color:var(--pen-red); color:#fff; }

  /* ---------- Result screen ---------- */
  .report-head{
    display:flex; justify-content:space-between; align-items:flex-start;
    border-bottom:2px solid var(--ink-green); padding-bottom:14px; margin-bottom:18px;
  }
  .report-head h2{ font-family:'Bitter',serif; font-size:22px; margin:0 0 4px; color:var(--ink-green); }
  .report-head .sub{ font-family:'IBM Plex Mono',monospace; font-size:11.5px; color:var(--ink-green-2); }
  .stamp{
    font-family:'IBM Plex Mono', monospace;
    font-weight:700;
    font-size:13px;
    letter-spacing:.05em;
    color:var(--pen-red);
    border:2.5px solid var(--pen-red);
    padding:7px 12px;
    border-radius:6px;
    transform:rotate(6deg);
    white-space:nowrap;
  }
  .score-row{ display:flex; gap:18px; margin-bottom:22px; flex-wrap:wrap; }
  .score-box{
    flex:1; min-width:140px;
    background:var(--paper-white);
    border-radius:5px;
    padding:16px 18px;
    text-align:center;
  }
  .score-box .num{ font-family:'Bitter',serif; font-weight:800; font-size:30px; color:var(--ink-green); }
  .score-box .lbl{ font-family:'IBM Plex Mono',monospace; font-size:10px; text-transform:uppercase; letter-spacing:.08em; color:var(--ink-green-2); margin-top:4px; }

  .cat-title{ font-family:'IBM Plex Mono',monospace; font-size:11px; text-transform:uppercase; letter-spacing:.1em; color:var(--ink-green-2); margin:22px 0 10px; }
  .cat-row{ margin-bottom:12px; }
  .cat-row .cat-name{ display:flex; justify-content:space-between; font-size:13.5px; margin-bottom:5px; }
  .cat-bar-bg{ background:var(--line); height:9px; border-radius:5px; overflow:hidden; }
  .cat-bar-fill{ height:100%; background:linear-gradient(90deg, var(--ink-green-2), var(--highlight)); border-radius:5px; }

  .note-box{
    background:var(--paper-white);
    border-left:3px solid var(--pen-red);
    padding:12px 16px;
    font-size:13.5px;
    line-height:1.6;
    margin-top:20px;
    color:#413d31;
  }
  .export-row{ display:flex; gap:10px; margin-top:24px; flex-wrap:wrap; }
  .export-row .btn{ flex:1; min-width:130px; }

  .footer-note{
    text-align:center; color:rgba(247,240,225,0.55);
    font-family:'IBM Plex Mono',monospace; font-size:10.5px;
    margin-top:18px; letter-spacing:.04em;
  }

  #captureArea{ background:var(--paper); }

  @media (max-width:520px){
    .sheet{ padding:26px 16px 22px; }
    .sheet::before{ width:22px; }
    .sheet-inner{ padding-left:12px; }
    .report-head{ flex-direction:column; gap:10px; }
    .stamp{ align-self:flex-end; }
  }
  [hidden]{ display:none !important; }
</style>
</head>
<body>

<div class="stage">
  <div class="masthead">
    <span class="eyebrow">Diagnostic Test · English Subject</span>
    <h1>English Diagnostic Assessment</h1>
  </div>

  <!-- START SCREEN -->
  <div class="sheet" id="screen-start">
    <div class="sheet-inner">
      <p class="intro-text">
        Welcome! This is a <strong>30-item diagnostic test</strong> for Grade 11 students (SMA/SMK) to
        check your current level of English. Answer honestly — this is not a formal exam, it simply helps
        your teacher understand where to start. It covers <strong>Grammar, Vocabulary, Language Function,
        and Reading Comprehension</strong>.
      </p>
      <div class="field">
        <label for="inputName">Full Name</label>
        <input type="text" id="inputName" placeholder="e.g. Andi Pratama" autocomplete="off">
      </div>
      <div class="field">
        <label for="inputClass">Class</label>
        <input type="text" id="inputClass" placeholder="e.g. XI TKJ 2 / XI IPA 1" autocomplete="off">
      </div>
      <div class="btn-row">
        <span></span>
        <div class="right">
          <button class="btn btn-primary" id="btnStart">Start Test →</button>
        </div>
      </div>
    </div>
  </div>

  <!-- QUIZ SCREEN -->
  <div class="sheet" id="screen-quiz" hidden>
    <div class="sheet-inner">
      <div class="bubble-track" id="bubbleTrack"></div>
      <div class="q-meta">
        <span class="q-count" id="qCount">Question 1 of 30</span>
        <span class="q-tag" id="qTag">GRAMMAR</span>
      </div>
      <div class="passage" id="passageBox" hidden></div>
      <p class="q-text" id="qText"></p>
      <div class="options" id="optionsBox"></div>
      <div class="btn-row">
        <button class="btn btn-ghost" id="btnPrev">← Previous</button>
        <div class="right">
          <button class="btn btn-primary" id="btnNext">Next →</button>
        </div>
      </div>
    </div>
  </div>

  <!-- RESULT SCREEN -->
  <div class="sheet" id="screen-result" hidden>
    <div class="sheet-inner" id="captureArea">
      <div class="report-head">
        <div>
          <h2 id="resName">—</h2>
          <span class="sub" id="resMeta">CLASS — · DATE —</span>
        </div>
        <div class="stamp" id="resStamp">LEVEL</div>
      </div>

      <div class="score-row">
        <div class="score-box">
          <div class="num" id="resScore">0/30</div>
          <div class="lbl">Correct Answers</div>
        </div>
        <div class="score-box">
          <div class="num" id="resPercent">0%</div>
          <div class="lbl">Overall Score</div>
        </div>
        <div class="score-box">
          <div class="num" id="resLevel">—</div>
          <div class="lbl">Proficiency Level</div>
        </div>
      </div>

      <div class="cat-title">Diagnostic Breakdown by Skill Area</div>
      <div id="catBreakdown"></div>

      <div class="note-box" id="resNote"></div>
    </div>

    <div class="sheet-inner" style="padding-top:0;">
      <div class="export-row">
        <button class="btn btn-primary" id="btnPng">⬇ Save as PNG</button>
        <button class="btn btn-primary" id="btnJpg">⬇ Save as JPG</button>
        <button class="btn btn-primary" id="btnPdf">⬇ Save as PDF</button>
      </div>
      <div class="btn-row" style="margin-top:14px;">
        <button class="btn btn-ghost" id="btnRetry">↻ Retake Test</button>
        <span></span>
      </div>
    </div>
  </div>

  <div class="footer-note">FOR TEACHER USE · GRADE 11 ENGLISH DIAGNOSTIC TEST · RESULT CAN BE EXPORTED &amp; SUBMITTED</div>
</div>

<script>
/* ============ QUESTION BANK (30 items) ============ */
const QUESTIONS = [
{ cat:"Grammar", q:"By the time the teacher arrived, the students ___ already ___ the exercise.", opts:["have / finished","had / finished","has / finish","had / finish"], ans:1 },
{ cat:"Grammar", q:"She usually ___ to school by bus, but yesterday she ___ by car.", opts:["goes / went","go / goes","went / goes","goes / go"], ans:0 },
{ cat:"Grammar", q:"Look! The kite ___ down. It's going to crash.", opts:["is falling","falls","fell","has fallen"], ans:0 },
{ cat:"Grammar", q:"If I ___ more free time, I would learn to play the guitar.", opts:["have","had","will have","has"], ans:1 },
{ cat:"Grammar", q:"The new bridge ___ by the workers next year.", opts:["will build","will be built","is built","built"], ans:1 },
{ cat:"Grammar", q:"This report ___ by the manager before it is sent to the client.", opts:["must check","must be checked","must be checking","must checked"], ans:1 },
{ cat:"Grammar", q:"You ___ smoke in the hospital area. It is strictly forbidden.", opts:["mustn't","don't have to","may","should"], ans:0 },
{ cat:"Grammar", q:"My brother is taller than me, but my sister is the ___ of all.", opts:["taller","tallest","more tall","most tall"], ans:1 },
{ cat:"Grammar", q:"This exercise is ___ than the previous one.", opts:["difficult","more difficult","most difficult","difficulter"], ans:1 },
{ cat:"Grammar", q:"He asked me where ___.", opts:["do I live","I live","did I live","I lived"], ans:3 },
{ cat:"Grammar", q:"She said that she ___ already finished her homework.", opts:["has","have","had","having"], ans:2 },
{ cat:"Grammar", q:"You have visited Bali before, ___?", opts:["haven't you","have you","don't you","didn't you"], ans:0 },
{ cat:"Vocabulary", q:"Choose the word closest in meaning to 'enormous'.", opts:["tiny","huge","narrow","quiet"], ans:1 },
{ cat:"Vocabulary", q:"Choose the antonym (opposite) of 'generous'.", opts:["kind","stingy","wealthy","friendly"], ans:1 },
{ cat:"Vocabulary", q:"The meeting has been postponed ___ next Monday.", opts:["to","at","in","on"], ans:0 },
{ cat:"Vocabulary", q:"She is very good ___ solving math problems.", opts:["at","in","on","for"], ans:0 },
{ cat:"Function", q:"'___ I help you carry those books?' — offering help politely.", opts:["Should","Shall","Must","Would"], ans:1 },
{ cat:"Function", q:"'___, I think online learning is more effective than offline learning.' (expressing opinion)", opts:["In my opinion","On the contrary","As a result","In addition"], ans:0 },
{ cat:"Function", q:"Someone says: 'I think school uniforms should be optional.' Which response shows DISAGREEMENT?", opts:["That's exactly what I think.","I couldn't agree more.","I'm afraid I disagree.","Absolutely right."], ans:2 },
{ cat:"Reading", passage:"Recycling is one of the easiest ways to protect our environment. By separating plastic, paper, and glass waste, we can reduce the amount of rubbish sent to landfills. Many schools in Indonesia now have recycling programs to teach students about the importance of caring for the planet.", q:"What is the main idea of the passage above?", opts:["Schools should stop using plastic.","Recycling helps protect the environment.","Landfills are dangerous places.","Glass is more valuable than paper."], ans:1 },
{ cat:"Reading", passage:"Recycling is one of the easiest ways to protect our environment. By separating plastic, paper, and glass waste, we can reduce the amount of rubbish sent to landfills. Many schools in Indonesia now have recycling programs to teach students about the importance of caring for the planet.", q:"According to the passage, what kinds of waste should be separated?", opts:["Food and drinks","Plastic, paper, and glass","Students and teachers","Schools and landfills"], ans:1 },
{ cat:"Reading", passage:"Recycling is one of the easiest ways to protect our environment. By separating plastic, paper, and glass waste, we can reduce the amount of rubbish sent to landfills. Many schools in Indonesia now have recycling programs to teach students about the importance of caring for the planet.", q:"The word 'rubbish' in the passage is closest in meaning to...", opts:["treasure","waste","equipment","furniture"], ans:1 },
{ cat:"Reading", passage:"How to Make Iced Tea: 1) Boil water. 2) Steep the tea bag for three minutes. 3) Add sugar and stir well. 4) Pour the tea over ice cubes. 5) Serve immediately.", q:"What should you do right after steeping the tea bag?", opts:["Boil water","Add sugar and stir","Pour over ice cubes","Serve immediately"], ans:1 },
{ cat:"Reading", passage:"How to Make Iced Tea: 1) Boil water. 2) Steep the tea bag for three minutes. 3) Add sugar and stir well. 4) Pour the tea over ice cubes. 5) Serve immediately.", q:"This text is an example of...", opts:["narrative text","procedure text","descriptive text","recount text"], ans:1 },
{ cat:"Reading", passage:"Once upon a time, a clever mouse lived near a farmer's house. One day, the mouse found a trap set by the farmer. Instead of running away, the mouse warned the other animals about the danger.", q:"What character trait does the mouse show in the story?", opts:["Lazy","Clever and caring","Greedy","Careless"], ans:1 },
{ cat:"Reading", passage:"Once upon a time, a clever mouse lived near a farmer's house. One day, the mouse found a trap set by the farmer. Instead of running away, the mouse warned the other animals about the danger.", q:"What is the main purpose of a narrative text like this one?", opts:["To describe a place","To entertain readers with a story","To give step-by-step instructions","To report a past event factually"], ans:1 },
{ cat:"Reading", passage:"Last weekend, my family and I went to Bandung. We visited a tea plantation and took a lot of pictures. In the afternoon, we enjoyed some local food before heading back home.", q:"What tense is mainly used in the text above?", opts:["Simple present","Simple past","Present perfect","Future"], ans:1 },
{ cat:"Reading", passage:"Last weekend, my family and I went to Bandung. We visited a tea plantation and took a lot of pictures. In the afternoon, we enjoyed some local food before heading back home.", q:"What is the purpose of a recount text such as this one?", opts:["To persuade readers","To retell past experiences","To explain a process","To argue an opinion"], ans:1 },
{ cat:"Grammar", q:"Neither the teacher nor the students ___ ready for the exam.", opts:["is","are","was","has"], ans:1 },
{ cat:"Grammar", q:"Choose the correct passive form of: 'They are building a new library.'", opts:["A new library is built by them.","A new library is being built by them.","A new library was being built by them.","A new library has been built by them."], ans:1 },
];

/* ============ STATE ============ */
let current = 0;
let answers = new Array(QUESTIONS.length).fill(null);
let studentName = "";
let studentClass = "";

/* ============ ELEMENTS ============ */
const screenStart = document.getElementById('screen-start');
const screenQuiz = document.getElementById('screen-quiz');
const screenResult = document.getElementById('screen-result');

document.getElementById('btnStart').addEventListener('click', startTest);
document.getElementById('btnPrev').addEventListener('click', ()=>navigate(-1));
document.getElementById('btnNext').addEventListener('click', ()=>navigate(1));
document.getElementById('btnRetry').addEventListener('click', resetAll);
document.getElementById('btnPng').addEventListener('click', ()=>exportImage('png'));
document.getElementById('btnJpg').addEventListener('click', ()=>exportImage('jpeg'));
document.getElementById('btnPdf').addEventListener('click', exportPdf);

function startTest(){
  const nameVal = document.getElementById('inputName').value.trim();
  const classVal = document.getElementById('inputClass').value.trim();
  if(!nameVal || !classVal){
    alert("Please fill in both your Name and Class before starting the test.");
    return;
  }
  studentName = nameVal;
  studentClass = classVal;
  screenStart.hidden = true;
  screenQuiz.hidden = false;
  buildBubbleTrack();
  renderQuestion();
}

function buildBubbleTrack(){
  const track = document.getElementById('bubbleTrack');
  track.innerHTML = "";
  QUESTIONS.forEach((_, i)=>{
    const b = document.createElement('div');
    b.className = 'bubble';
    b.textContent = i+1;
    b.id = 'bub-'+i;
    b.addEventListener('click', ()=>{ current = i; renderQuestion(); });
    track.appendChild(b);
  });
  updateBubbles();
}

function updateBubbles(){
  QUESTIONS.forEach((_, i)=>{
    const b = document.getElementById('bub-'+i);
    b.classList.toggle('answered', answers[i] !== null);
    b.classList.toggle('current', i === current);
  });
}

function renderQuestion(){
  const item = QUESTIONS[current];
  document.getElementById('qCount').textContent = `Question ${current+1} of ${QUESTIONS.length}`;
  document.getElementById('qTag').textContent = item.cat.toUpperCase();
  document.getElementById('qText').textContent = item.q;

  const passageBox = document.getElementById('passageBox');
  if(item.passage){
    passageBox.hidden = false;
    passageBox.textContent = item.passage;
  } else {
    passageBox.hidden = true;
    passageBox.textContent = "";
  }

  const optionsBox = document.getElementById('optionsBox');
  optionsBox.innerHTML = "";
  const letters = ['A','B','C','D'];
  item.opts.forEach((opt, i)=>{
    const div = document.createElement('div');
    div.className = 'option' + (answers[current] === i ? ' selected' : '');
    div.innerHTML = `<span class="letter">${letters[i]}</span><span>${opt}</span>`;
    div.addEventListener('click', ()=>{
      answers[current] = i;
      renderQuestion();
      updateBubbles();
    });
    optionsBox.appendChild(div);
  });

  document.getElementById('btnPrev').disabled = current === 0;
  const nextBtn = document.getElementById('btnNext');
  nextBtn.textContent = current === QUESTIONS.length - 1 ? "Finish Test ✓" : "Next →";
  updateBubbles();
}

function navigate(dir){
  if(dir === 1 && current === QUESTIONS.length - 1){
    const unanswered = answers.filter(a=>a===null).length;
    if(unanswered > 0){
      const proceed = confirm(`You still have ${unanswered} unanswered question(s). Submit anyway?`);
      if(!proceed) return;
    }
    finishTest();
    return;
  }
  current = Math.max(0, Math.min(QUESTIONS.length - 1, current + dir));
  renderQuestion();
}

/* ============ SCORING ============ */
function finishTest(){
  const catTotals = {};
  const catCorrect = {};
  let totalCorrect = 0;

  QUESTIONS.forEach((item, i)=>{
    catTotals[item.cat] = (catTotals[item.cat] || 0) + 1;
    if(answers[i] === item.ans){
      totalCorrect++;
      catCorrect[item.cat] = (catCorrect[item.cat] || 0) + 1;
    }
  });

  const percent = Math.round((totalCorrect / QUESTIONS.length) * 100);
  let level, note;
  if(percent >= 85){
    level = "ADVANCED";
    note = "Excellent! This student demonstrates strong command of grammar, vocabulary, and reading comprehension. Ready for enrichment or advanced-level materials.";
  } else if(percent >= 70){
    level = "INTERMEDIATE";
    note = "Good performance overall. The student has a solid foundation but may benefit from targeted practice in the weaker skill areas shown below.";
  } else if(percent >= 50){
    level = "ELEMENTARY";
    note = "The student shows basic understanding of English but needs structured reinforcement, especially in grammar and reading comprehension, before moving to more advanced material.";
  } else {
    level = "BEGINNER";
    note = "The student requires foundational support across most skill areas. Recommended: start with basic grammar structures and core vocabulary building.";
  }

  document.getElementById('resName').textContent = studentName;
  const dateStr = new Date().toLocaleDateString('en-GB', {day:'2-digit', month:'long', year:'numeric'});
  document.getElementById('resMeta').textContent = `CLASS ${studentClass.toUpperCase()} · ${dateStr}`;
  document.getElementById('resStamp').textContent = level;
  document.getElementById('resScore').textContent = `${totalCorrect}/${QUESTIONS.length}`;
  document.getElementById('resPercent').textContent = `${percent}%`;
  document.getElementById('resLevel').textContent = level;
  document.getElementById('resNote').textContent = note;

  const catBreakdown = document.getElementById('catBreakdown');
  catBreakdown.innerHTML = "";
  Object.keys(catTotals).forEach(cat=>{
    const correct = catCorrect[cat] || 0;
    const total = catTotals[cat];
    const pct = Math.round((correct/total)*100);
    const row = document.createElement('div');
    row.className = 'cat-row';
    row.innerHTML = `
      <div class="cat-name"><span>${cat}</span><span>${correct}/${total} · ${pct}%</span></div>
      <div class="cat-bar-bg"><div class="cat-bar-fill" style="width:${pct}%"></div></div>
    `;
    catBreakdown.appendChild(row);
  });

  screenQuiz.hidden = true;
  screenResult.hidden = false;
  window.scrollTo({top:0, behavior:'smooth'});
}

function resetAll(){
  current = 0;
  answers = new Array(QUESTIONS.length).fill(null);
  document.getElementById('inputName').value = "";
  document.getElementById('inputClass').value = "";
  screenResult.hidden = true;
  screenQuiz.hidden = true;
  screenStart.hidden = false;
}

/* ============ EXPORT ============ */
function safeFileName(){
  const base = (studentName || "student") + "_" + (studentClass || "class") + "_english_diagnostic";
  return base.replace(/[^a-z0-9_\-]+/gi, "_");
}

function exportImage(type){
  const target = document.getElementById('captureArea');
  html2canvas(target, {scale:2, backgroundColor:'#F7F0E1'}).then(canvas=>{
    const link = document.createElement('a');
    const ext = type === 'jpeg' ? 'jpg' : 'png';
    link.download = `${safeFileName()}.${ext}`;
    link.href = canvas.toDataURL(`image/${type}`, 0.95);
    link.click();
  });
}

function exportPdf(){
  const target = document.getElementById('captureArea');
  html2canvas(target, {scale:2, backgroundColor:'#F7F0E1'}).then(canvas=>{
    const { jsPDF } = window.jspdf;
    const imgData = canvas.toDataURL('image/png');
    const pdfWidth = 210;
    const pdfHeight = (canvas.height * pdfWidth) / canvas.width;
    const pdf = new jsPDF('p', 'mm', 'a4');
    pdf.addImage(imgData, 'PNG', 0, 12, pdfWidth, pdfHeight);
    pdf.save(`${safeFileName()}.pdf`);
  });
}
</script>
</body>
</html>
