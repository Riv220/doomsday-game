<!doctype html>
<html lang="he" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
  <title>Doomsday Master</title>
  <style>
    :root{
      --bg:#f4f6f9;
      --card:#ffffff;
      --border:#d1d5db;
      --text:#0b0f19;
      --muted:#6b7280;
      --green:#2ecc71;
      --green2:#27ae60;
      --red:#e74c3c;
      --yellow:#f1c40f;
      --blue:#3498db;
      --dark:#34495e;
    }
    *{ box-sizing:border-box; }
    body{
      margin:0;
      font-family: system-ui, -apple-system, Segoe UI, Arial;
      background:var(--bg);
      color:var(--text);
    }
    .wrap{
      max-width:720px;
      margin:0 auto;
      padding:14px;
      padding-bottom:28px;
    }
    .title{
      font-size:22px;
      font-weight:800;
      margin:6px 0 10px 0;
    }
    .topbar{
      display:flex;
      align-items:center;
      gap:10px;
      margin-bottom:10px;
    }
    .topbar .spacer{ flex:1; }
    .btn{
      border:0;
      border-radius:10px;
      padding:10px 12px;
      font-weight:800;
      cursor:pointer;
      user-select:none;
      -webkit-tap-highlight-color: transparent;
    }
    .btn.dark{ background:var(--dark); color:#fff; }
    .pill{
      font-weight:800;
      padding:8px 10px;
      border-radius:999px;
      background:#fff;
      border:1px solid var(--border);
      display:flex;
      align-items:center;
      gap:8px;
      white-space:nowrap;
    }
    .pill.coins{ color:#d35400; }
    .pill.streak{ color:var(--red); }

    .progressWrap{
      background:#fff;
      border:1px solid var(--border);
      border-radius:999px;
      overflow:hidden;
      height:14px;
      margin:10px 0;
    }
    .progressBar{
      height:100%;
      width:100%;
      background:var(--blue);
      transition:width .25s linear;
    }
    .row{
      display:flex;
      gap:10px;
      align-items:center;
      flex-wrap:wrap;
    }
    select{
      width:100%;
      padding:10px 12px;
      border-radius:10px;
      border:1px solid var(--border);
      font-weight:700;
      background:#fff;
      font-size:14px;
    }
    .card{
      margin-top:12px;
      background:var(--card);
      border:1px solid var(--border);
      border-radius:16px;
      padding:12px;
    }
    .cardTop{
      display:flex;
      align-items:center;
    }
    .cardTop .spacer{ flex:1; }
    .hintBtn{
      background:var(--yellow);
      color:#111;
      border:1px solid #d4ac0d;
      padding:8px 10px;
      border-radius:10px;
      font-weight:900;
    }
    .hintBtn.disabled{
      background:#95a5a6;
      border-color:#7f8c8d;
      color:#ecf0f1;
      cursor:not-allowed;
    }
    .context{
      text-align:center;
      color:var(--muted);
      font-size:14px;
      margin-top:6px;
    }
    .question{
      text-align:center;
      font-size:34px;
      font-weight:900;
      margin:8px 0 6px;
      color:#2c3e50;
    }
    .hintBox{
      display:none;
      margin-top:10px;
      padding:10px;
      border-radius:10px;
      background:#e8f6f3;
      border:1px solid #1abc9c;
      color:#2c3e50;
      font-size:14px;
      line-height:1.35;
    }
    .grid{
      margin-top:12px;
      display:grid;
      grid-template-columns: repeat(4, 1fr);
      gap:10px;
    }
    .dayBtn{
      background:#ecf0f1;
      border:1px solid #bdc3c7;
      border-radius:12px;
      padding:14px 10px;
      font-weight:900;
      font-size:18px;
      cursor:pointer;
      color:#2c3e50;
    }
    .dayBtn.correct{
      background:var(--green);
      border:2px solid var(--green2);
      color:#fff;
    }
    .dayBtn.wrong{
      background:var(--red);
      border:2px solid #c0392b;
      color:#fff;
    }
    .feedback{
      display:none;
      margin-top:12px;
      background:#dfe6e9;
      border-radius:12px;
      padding:12px;
      font-size:14px;
      line-height:1.4;
      text-align:right;
    }
    .nextBtn{
      display:none;
      margin-top:10px;
      width:100%;
      background:var(--green2);
      color:#fff;
      font-size:16px;
      padding:12px 12px;
      border-radius:12px;
    }

    .modalBackdrop{
      display:none;
      position:fixed;
      inset:0;
      background:rgba(0,0,0,.45);
      padding:18px;
      z-index:9999;
    }
    .modal{
      max-width:720px;
      margin:0 auto;
      background:#fff;
      border-radius:16px;
      border:1px solid var(--border);
      overflow:hidden;
    }
    .modalHeader{
      padding:12px 14px;
      font-weight:900;
      font-size:18px;
      background:#f8fafc;
      border-bottom:1px solid var(--border);
      display:flex;
      align-items:center;
      gap:10px;
    }
    .modalHeader .spacer{ flex:1; }
    .modalBody{
      padding:14px;
      font-size:14px;
      line-height:1.45;
      text-align:right;
    }
    .modalBody h3{ margin:8px 0 6px; }
    .modalClose{
      background:#111827;
      color:#fff;
      border-radius:10px;
      padding:8px 10px;
      font-weight:900;
    }
    .small{ color:var(--muted); font-size:12px; }
    @media (max-width:520px){
      .grid{ grid-template-columns: repeat(2, 1fr); }
      .question{ font-size:30px; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="title">Doomsday Master 🧠</div>

    <div class="topbar">
      <button id="btnCheat" class="btn dark">📜 טבלה</button>
      <div class="spacer"></div>
      <div class="pill coins" id="lblCoins">💰 10</div>
      <div class="pill streak" id="lblStreak">רצף: 0 🔥</div>
    </div>

    <div class="progressWrap" aria-label="timer">
      <div class="progressBar" id="progressBar"></div>
    </div>

    <div class="row">
      <select id="modeSelect">
        <option value="0">רמה 1: אימון שנים (+1)</option>
        <option value="1">רמה 2: אימון עוגנים (+2)</option>
        <option value="2">רמה 3: מלא (+3)</option>
      </select>
    </div>

    <div class="card">
      <div class="cardTop">
        <div class="spacer"></div>
        <button id="btnHint" class="hintBtn">💡 רמז (10)</button>
      </div>
      <div class="context" id="lblContext"></div>
      <div class="question" id="lblQuestion">...</div>
      <div class="hintBox" id="hintBox"></div>
    </div>

    <div class="grid" id="daysGrid"></div>

    <div class="feedback" id="feedbackBox"></div>
    <button class="btn nextBtn" id="btnNext">המשך</button>

    <div class="small" style="margin-top:10px;">
      באייפון: Share → Add to Home Screen
    </div>
  </div>

  <div class="modalBackdrop" id="modalBackdrop">
    <div class="modal" role="dialog" aria-modal="true">
      <div class="modalHeader">
        📜 טבלת עוגנים
        <div class="spacer"></div>
        <button class="btn modalClose" id="modalClose">סגור</button>
      </div>
      <div class="modalBody" id="modalBody"></div>
    </div>
  </div>

<script>
  const DISPLAY_NAMES = ["ראשון", "שני", "שלישי", "רביעי", "חמישי", "שישי", "שבת"];
  const MONTH_NAMES = ["", "ינואר", "פברואר", "מרץ", "אפריל", "מאי", "יוני", "יולי", "אוגוסט", "ספטמבר", "אוקטובר", "נובמבר", "דצמבר"];

  function isLeapYear(y){
    return (y % 4 === 0 && y % 100 !== 0) || (y % 400 === 0);
  }
  function utcDate(y, m, d){
    return new Date(Date.UTC(y, m-1, d, 12, 0, 0));
  }
  function addDaysUTC(dt, days){
    return new Date(dt.getTime() + days * 86400000);
  }
  function weekdayUTC(dt){
    return dt.getUTCDay(); // 0..6 (Sun..Sat)
  }

  // Century anchors (classic doomsday):
  // 1800s Friday(5), 1900s Wednesday(3), 2000s Tuesday(2), 2100s Sunday(0)
  function centuryAnchorClassic(year){
    const century = Math.floor(year / 100) * 100;
    const mod = (century % 400 + 400) % 400;
    if (mod === 0) return 2;
    if (mod === 100) return 0;
    if (mod === 200) return 5;
    return 3;
  }
  function yearOffsetClassic(year){
    const yy = year % 100;
    const a = Math.floor(yy / 12);
    const b = yy % 12;
    const c = Math.floor(b / 4);
    return (a + b + c) % 7;
  }
  function doomsdayOfYear(year){
    return (centuryAnchorClassic(year) + yearOffsetClassic(year)) % 7;
  }

  function monthAnchorDay(year, month){
    const leap = isLeapYear(year);
    if (month === 1) return leap ? 4 : 3;
    if (month === 2) return leap ? 29 : 28;
    const map = {3:14,4:4,5:9,6:6,7:11,8:8,9:5,10:10,11:7,12:12};
    return map[month];
  }

  // UI refs
  const lblCoins = document.getElementById("lblCoins");
  const lblStreak = document.getElementById("lblStreak");
  const progressBar = document.getElementById("progressBar");
  const modeSelect = document.getElementById("modeSelect");
  const lblContext = document.getElementById("lblContext");
  const lblQuestion = document.getElementById("lblQuestion");
  const hintBox = document.getElementById("hintBox");
  const btnHint = document.getElementById("btnHint");
  const daysGrid = document.getElementById("daysGrid");
  const feedbackBox = document.getElementById("feedbackBox");
  const btnNext = document.getElementById("btnNext");

  const modalBackdrop = document.getElementById("modalBackdrop");
  const modalClose = document.getElementById("modalClose");
  const btnCheat = document.getElementById("btnCheat");
  const modalBody = document.getElementById("modalBody");

  // Game state
  let coins = 10;
  let streak = 0;

  const ROUND_TIME = 60;
  let timeLeft = ROUND_TIME;
  let timerId = null;

  let current = null; // {type, year, ansDayIdx, targetUTC?}
  let locked = false;

  function renderCheatSheet(){
    modalBody.innerHTML = `
      <h3 style="color:#2980b9;margin-top:0;">1) עוגני המאות</h3>
      <ul>
        <li>1800–1899: <b>שישי</b></li>
        <li>1900–1999: <b>רביעי</b></li>
        <li>2000–2099: <b>שלישי</b></li>
        <li>2100–2199: <b>ראשון</b></li>
      </ul>
      <hr>
      <h3 style="color:#27ae60;">2) שיטת ה-12</h3>
      <ol>
        <li>yy = שתי ספרות אחרונות</li>
        <li>a = ⌊yy/12⌋</li>
        <li>b = yy mod 12</li>
        <li>c = ⌊b/4⌋</li>
        <li>סה״כ = a+b+c (mod 7) + עוגן המאה</li>
      </ol>
      <hr>
      <h3 style="color:#c0392b;">3) עוגני חודשים</h3>
      <p>
        שנה רגילה: 1/3, 2/28, 3/14, 4/4, 5/9, 6/6, 7/11, 8/8, 9/5, 10/10, 11/7, 12/12<br>
        שנה מעוברת: 1/4, 2/29 (השאר אותו דבר)
      </p>
      <div class="small">אחרי כל שאלה מוצג פתרון והסבר – גם אם צדקת.</div>
    `;
  }

  function updateLabels(){
    lblCoins.textContent = `💰 ${coins}`;
    lblStreak.textContent = `רצף: ${streak} 🔥`;

    if (coins >= 10){
      btnHint.classList.remove("disabled");
      btnHint.textContent = "💡 רמז (10)";
      btnHint.disabled = false;
    } else {
      btnHint.classList.add("disabled");
      btnHint.textContent = "אין כסף (10)";
      btnHint.disabled = true;
    }
  }

  function setProgress(){
    const pct = Math.max(0, Math.min(1, timeLeft / ROUND_TIME));
    progressBar.style.width = (pct * 100).toFixed(2) + "%";
  }

  function stopTimer(){
    if (timerId){
      clearInterval(timerId);
      timerId = null;
    }
  }

  function startTimer(){
    stopTimer();
    timeLeft = ROUND_TIME;
    setProgress();
    timerId = setInterval(() => {
      timeLeft -= 1;
      setProgress();
      if (timeLeft <= 0){
        stopTimer();
        handleTimeout();
      }
    }, 1000);
  }

  function lockButtons(state){
    locked = state;
    const buttons = daysGrid.querySelectorAll("button.dayBtn");
    buttons.forEach(b => b.disabled = state);
    btnHint.disabled = state || coins < 10;
  }

  function resetRoundUI(){
    feedbackBox.style.display = "none";
    feedbackBox.innerHTML = "";
    btnNext.style.display = "none";
    hintBox.style.display = "none";
    hintBox.innerHTML = "";
    const buttons = daysGrid.querySelectorAll("button.dayBtn");
    buttons.forEach(b => {
      b.classList.remove("correct","wrong");
      b.disabled = false;
    });
  }

  function buildDaysButtons(){
    daysGrid.innerHTML = "";
    for (let i=0;i<7;i++){
      const btn = document.createElement("button");
      btn.className = "dayBtn";
      btn.textContent = DISPLAY_NAMES[i];
      btn.addEventListener("click", () => checkAnswer(i));
      daysGrid.appendChild(btn);
    }
  }

  function randInt(a,b){
    return Math.floor(Math.random()*(b-a+1))+a;
  }

  function startNewRound(){
    resetRoundUI();
    lockButtons(false);
    updateLabels();

    const mode = Number(modeSelect.value);
    const year = randInt(1950, 2050);

    if (mode === 0){
      const ans = doomsdayOfYear(year);
      lblContext.innerHTML = "חשב את יום העוגן השנתי:";
      lblQuestion.textContent = String(year);
      current = { type: "year", year: year, ansDayIdx: ans };

    } else if (mode === 1){
      const anchorIdx = doomsdayOfYear(year);
      const month = randInt(1,12);
      const baseDay = monthAnchorDay(year, month);
      const shiftOptions = [-2,-1,1,2];
      const shift = shiftOptions[randInt(0, shiftOptions.length-1)];

      let target = utcDate(year, month, baseDay);
      target = addDaysUTC(target, shift);

      const anchorName = DISPLAY_NAMES[anchorIdx];
      lblContext.innerHTML = `בשנת ${year} העוגן הוא <b>${anchorName}</b>.`;
      lblQuestion.textContent = `${target.getUTCDate()} ב${MONTH_NAMES[target.getUTCMonth()+1]}`;
      current = { type: "anchor", year: year, targetUTC: target, ansDayIdx: weekdayUTC(target) };

    } else {
      const y = year;
      const start = utcDate(y,1,1);
      const d = addDaysUTC(start, randInt(0, 364));
      lblContext.innerHTML = "חשב מאפס:";
      lblQuestion.textContent = `${d.getUTCDate()} ב${MONTH_NAMES[d.getUTCMonth()+1]} ${d.getUTCFullYear()}`;
      current = { type: "full", year: d.getUTCFullYear(), targetUTC: d, ansDayIdx: weekdayUTC(d) };
    }

    startTimer();
  }

  function useHint(){
    if (locked) return;
    if (coins < 10) return;
    coins -= 10;
    updateLabels();

    hintBox.style.display = "block";

    const y = current.year;
    if (current.type === "year"){
      const cen = centuryAnchorClassic(y);
      const cenName = DISPLAY_NAMES[cen];

      const yy = y % 100;
      const dozens = Math.floor(yy / 12);
      const rem = yy % 12;
      const fours = Math.floor(rem / 4);
      const total = dozens + rem + fours;

      hintBox.innerHTML =
        `<b>רמז לשנת ${y}:</b><br>` +
        `• yy=${yy} → ${dozens} תריסרים, שארית ${rem}, רביעיות ${fours}.<br>` +
        `• תוספת = ${total} (mod 7 = ${total % 7}).<br>` +
        `• עוגן המאה = <b>${cenName}</b>.<br>` +
        `<b>משימה:</b> ${cenName} + ${total % 7} = ?`;

    } else {
      const yearAnchor = doomsdayOfYear(y);
      const yearAnchorName = DISPLAY_NAMES[yearAnchor];

      const t = current.targetUTC;
      const m = t.getUTCMonth()+1;
      const mAnchor = monthAnchorDay(y, m);
      const diff = t.getUTCDate() - mAnchor;

      hintBox.innerHTML =
        `<b>רמז:</b><br>` +
        `• עוגן השנה: <b>${yearAnchorName}</b>.<br>` +
        `• עוגן החודש: יום <b>${mAnchor}</b>.<br>` +
        `• הפרש: <b>${diff >= 0 ? "+" : ""}${diff}</b> ימים.<br>` +
        `<b>משימה:</b> ${yearAnchorName} ${diff >= 0 ? "+" : ""}${diff} ימים = ?`;
    }
  }

  function detailedExplanation(correctIdx){
    const y = current.year;
    const correctName = DISPLAY_NAMES[correctIdx];

    const cen = centuryAnchorClassic(y);
    const cenName = DISPLAY_NAMES[cen];

    const yy = y % 100;
    const dozens = Math.floor(yy / 12);
    const rem = yy % 12;
    const fours = Math.floor(rem / 4);
    const total = dozens + rem + fours;

    const yearAnchor = doomsdayOfYear(y);
    const yearAnchorName = DISPLAY_NAMES[yearAnchor];

    if (current.type === "year"){
      return (
        `פתרון: <b>${y} → ${yearAnchorName}</b><br><br>` +
        `1) עוגן המאה: <b>${cenName}</b><br>` +
        `2) שיטת ה-12 על ${yy}:<br>` +
        `• a=⌊${yy}/12⌋=${dozens}, b=${rem}, c=⌊${rem}/4⌋=${fours}<br>` +
        `3) a+b+c=${total} → mod7=${total%7}<br>` +
        `4) ${cenName} + ${total%7} = <b>${yearAnchorName}</b>`
      );
    }

    const t = current.targetUTC;
    const m = t.getUTCMonth()+1;
    const mAnchor = monthAnchorDay(y, m);
    const diff = t.getUTCDate() - mAnchor;

    const intro = (current.type === "full")
      ? `1) קודם מחשבים עוגן שנה: <b>${y} → ${yearAnchorName}</b><br>`
      : "";

    return (
      `פתרון: <b>${t.getUTCDate()}.${m}.${y} → ${correctName}</b><br><br>` +
      `${intro}` +
      `2) עוגן חודש: ה-${mAnchor}.${m} נופל על ${yearAnchorName}<br>` +
      `3) הפרש: ${t.getUTCDate()} − ${mAnchor} = ${diff >= 0 ? "+" : ""}${diff}<br>` +
      `4) ${yearAnchorName} ${diff >= 0 ? "+" : ""}${diff} ימים = <b>${correctName}</b>`
    );
  }

  function checkAnswer(userIdx){
    if (locked) return;
    stopTimer();
    lockButtons(true);

    const correctIdx = current.ansDayIdx;
    const isCorrect = (userIdx === correctIdx);

    const buttons = daysGrid.querySelectorAll("button.dayBtn");
    buttons.forEach((b, idx) => {
      if (idx === correctIdx) b.classList.add("correct");
      else if (idx === userIdx && !isCorrect) b.classList.add("wrong");
    });

    const modeLevel = Number(modeSelect.value) + 1;
    let header = "";
    if (isCorrect){
      coins += modeLevel;
      streak += 1;
      header = "✅ נכון!";
    } else {
      streak = 0;
      header = "❌ לא נכון";
    }
    updateLabels();

    const expl = detailedExplanation(correctIdx);
    feedbackBox.innerHTML = `<b>${header}</b><br><br>${expl}`;
    feedbackBox.style.display = "block";
    btnNext.style.display = "block";
  }

  function handleTimeout(){
    lockButtons(true);
    const correctIdx = current.ansDayIdx;
    const buttons = daysGrid.querySelectorAll("button.dayBtn");
    buttons.forEach((b, idx) => {
      if (idx === correctIdx) b.classList.add("correct");
    });

    streak = 0;
    updateLabels();

    const expl = detailedExplanation(correctIdx);
    feedbackBox.innerHTML = `<b>⏱️ נגמר הזמן</b><br><br>${expl}`;
    feedbackBox.style.display = "block";
    btnNext.style.display = "block";
  }

  btnHint.addEventListener("click", useHint);
  btnNext.addEventListener("click", startNewRound);
  modeSelect.addEventListener("change", () => {
    streak = 0;
    updateLabels();
    startNewRound();
  });

  btnCheat.addEventListener("click", () => {
    renderCheatSheet();
    modalBackdrop.style.display = "block";
  });
  modalClose.addEventListener("click", () => modalBackdrop.style.display = "none");
  modalBackdrop.addEventListener("click", (e) => {
    if (e.target === modalBackdrop) modalBackdrop.style.display = "none";
  });

  buildDaysButtons();
  updateLabels();
  startNewRound();
</script>
</body>
</html>
