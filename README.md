<!doctype html>
<html lang="he" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
  <title>Doomsday Master</title>
  <style>
    :root{
      --bg:#f4f6f9; --card:#ffffff; --border:#d1d5db;
      --text:#0b0f19; --muted:#6b7280;
      --green:#2ecc71; --green2:#27ae60; --red:#e74c3c;
      --yellow:#f1c40f; --blue:#3498db; --dark:#34495e;
    }
    *{ box-sizing:border-box; }
    body{ margin:0; font-family:system-ui,-apple-system,Segoe UI,Arial; background:var(--bg); color:var(--text); }
    .wrap{ max-width:820px; margin:0 auto; padding:14px; padding-bottom:28px; }
    .title{ font-size:22px; font-weight:900; margin:6px 0 10px; display:flex; align-items:center; gap:10px; }
    .title .spacer{ flex:1; }
    .btn{ border:0; border-radius:10px; padding:10px 12px; font-weight:900; cursor:pointer; user-select:none; -webkit-tap-highlight-color:transparent; }
    .btn.dark{ background:var(--dark); color:#fff; }
    .btn.green{ background:var(--green2); color:#fff; }
    .btn.gray{ background:#95a5a6; color:#fff; cursor:not-allowed; }
    .pill{ font-weight:900; padding:8px 10px; border-radius:999px; background:#fff; border:1px solid var(--border); display:flex; align-items:center; gap:8px; white-space:nowrap; }
    .pill.coins{ color:#d35400; }
    .pill.streak{ color:var(--red); }
    .pill.user{ color:#2c3e50; }
    .topbar{ display:flex; align-items:center; gap:10px; flex-wrap:wrap; margin-bottom:10px; }
    .topbar .spacer{ flex:1; }

    .progressWrap{ background:#fff; border:1px solid var(--border); border-radius:999px; overflow:hidden; height:14px; margin:10px 0; }
    .progressBar{ height:100%; width:100%; background:var(--blue); transition:width .25s linear; }

    .row{ display:flex; gap:10px; align-items:center; flex-wrap:wrap; }
    select, input{
      width:100%; padding:10px 12px; border-radius:10px; border:1px solid var(--border);
      font-weight:800; background:#fff; font-size:14px;
    }

    .card{ margin-top:12px; background:var(--card); border:1px solid var(--border); border-radius:16px; padding:12px; }
    .cardTop{ display:flex; align-items:center; gap:10px; }
    .cardTop .spacer{ flex:1; }
    .hintBtn{ background:var(--yellow); color:#111; border:1px solid #d4ac0d; padding:8px 10px; border-radius:10px; font-weight:900; }
    .hintBtn.disabled{ background:#95a5a6; border-color:#7f8c8d; color:#ecf0f1; cursor:not-allowed; }
    .context{ text-align:center; color:var(--muted); font-size:14px; margin-top:6px; }
    .question{ text-align:center; font-size:34px; font-weight:900; margin:8px 0 6px; color:#2c3e50; }
    .hintBox{ display:none; margin-top:10px; padding:10px; border-radius:10px; background:#e8f6f3; border:1px solid #1abc9c; color:#2c3e50; font-size:14px; line-height:1.35; }

    .grid{ margin-top:12px; display:grid; grid-template-columns:repeat(4,1fr); gap:10px; }
    .dayBtn{ background:#ecf0f1; border:1px solid #bdc3c7; border-radius:12px; padding:14px 10px; font-weight:900; font-size:18px; cursor:pointer; color:#2c3e50; }
    .dayBtn.correct{ background:var(--green); border:2px solid var(--green2); color:#fff; }
    .dayBtn.wrong{ background:var(--red); border:2px solid #c0392b; color:#fff; }

    .feedback{ display:none; margin-top:12px; background:#dfe6e9; border-radius:12px; padding:12px; font-size:14px; line-height:1.4; text-align:right; }
    .nextBtn{ display:none; margin-top:10px; width:100%; background:var(--green2); color:#fff; font-size:16px; padding:12px 12px; border-radius:12px; }

    .small{ color:var(--muted); font-size:12px; }
    @media (max-width:520px){
      .grid{ grid-template-columns:repeat(2,1fr); }
      .question{ font-size:30px; }
    }

    .modalBackdrop{
      display:none; position:fixed; inset:0; background:rgba(0,0,0,.45);
      padding:18px; z-index:9999;
    }
    .modal{
      max-width:820px; margin:0 auto; background:#fff; border-radius:16px;
      border:1px solid var(--border); overflow:hidden;
    }
    .modalHeader{
      padding:12px 14px; font-weight:900; font-size:18px; background:#f8fafc;
      border-bottom:1px solid var(--border); display:flex; align-items:center; gap:10px;
    }
    .modalHeader .spacer{ flex:1; }
    .modalBody{ padding:14px; font-size:14px; line-height:1.45; text-align:right; }
    .tileGrid{ display:grid; grid-template-columns:repeat(2,1fr); gap:10px; margin-top:10px; }
    .tile{
      border:1px solid var(--border); border-radius:14px; padding:12px;
      background:#fff; text-align:right;
    }
    .tile .name{ font-weight:900; font-size:16px; margin-bottom:6px; }
    .tile .desc{ color:var(--muted); font-size:13px; margin-bottom:10px; }
    .tile .row2{ display:flex; gap:8px; align-items:center; }
    .badge{
      background:#fff7ed; border:1px solid #fdba74; color:#9a3412;
      padding:4px 8px; border-radius:999px; font-weight:900; font-size:12px;
    }
    .activeBadge{
      background:#ecfdf5; border:1px solid #10b981; color:#065f46;
      padding:4px 8px; border-radius:999px; font-weight:900; font-size:12px;
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="title">
      Doomsday Master 🧠
      <div class="spacer"></div>
      <button id="btnStore" class="btn dark">🛒 חנות</button>
      <button id="btnCheat" class="btn dark">📜 טבלה</button>
    </div>

    <div class="topbar">
      <div class="pill user" id="lblUser">👤 אורח</div>
      <div class="pill coins" id="lblCoins">💰 10</div>
      <div class="pill streak" id="lblStreak">רצף: 0 🔥</div>
      <div class="spacer"></div>
      <div class="pill" id="lblCentury">מאה: 2000–2099</div>
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
      באייפון: Safari → Share → Add to Home Screen
    </div>
  </div>

  <!-- CHEAT MODAL -->
  <div class="modalBackdrop" id="modalCheat">
    <div class="modal" role="dialog" aria-modal="true">
      <div class="modalHeader">
        📜 טבלת עוגנים
        <div class="spacer"></div>
        <button class="btn dark" id="cheatClose">סגור</button>
      </div>
      <div class="modalBody" id="cheatBody"></div>
    </div>
  </div>

  <!-- STORE MODAL -->
  <div class="modalBackdrop" id="modalStore">
    <div class="modal" role="dialog" aria-modal="true">
      <div class="modalHeader">
        🛒 חנות + משתמש
        <div class="spacer"></div>
        <button class="btn dark" id="storeClose">סגור</button>
      </div>
      <div class="modalBody">
        <div style="font-weight:900;margin-bottom:6px;">שם משתמש</div>
        <div class="row">
          <input id="usernameInput" placeholder="הכנס שם (למשל: roma)" />
          <button id="btnSaveUser" class="btn green" style="width:100%;">שמור</button>
        </div>
        <div class="small" style="margin-top:6px;">השם נשמר מקומית בטלפון/דפדפן (לא נשלח לשום מקום).</div>

        <hr style="border:none;border-top:1px solid var(--border);margin:14px 0;" />

        <div style="font-weight:900;margin-bottom:6px;">Century Packs (קונים פעם אחת)</div>
        <div class="small">כל קנייה פותחת טווח שנים חדש לתרגול. אפשר לבחור “מאה פעילה”.</div>

        <div class="tileGrid" id="storeGrid"></div>
      </div>
    </div>
  </div>

<script>
/* =========================
   Storage (local only)
========================= */
const STORAGE_KEY = "doomsday_master_profile_v1";

function loadProfile(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if (!raw) return null;
    return JSON.parse(raw);
  }catch(e){
    return null;
  }
}
function saveProfile(p){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(p));
}
function defaultProfile(){
  return {
    username: "",
    coins: 10,
    streak: 0,
    activeCentury: 2000,
    ownedCenturies: { "2000": true } // default unlocked
  };
}
let profile = loadProfile() || defaultProfile();
saveProfile(profile);

/* =========================
   Helpers (calendar)
========================= */
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
  return dt.getUTCDay();
}
function centuryAnchorClassic(year){
  const century = Math.floor(year / 100) * 100;
  const mod = (century % 400 + 400) % 400;
  if (mod === 0) return 2;   // 2000 -> Tue
  if (mod === 100) return 0; // 2100 -> Sun
  if (mod === 200) return 5; // 2200 -> Fri
  return 3;                  // 1900 -> Wed
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
function randInt(a,b){
  return Math.floor(Math.random()*(b-a+1))+a;
}

/* =========================
   UI refs
========================= */
const lblUser = document.getElementById("lblUser");
const lblCoins = document.getElementById("lblCoins");
const lblStreak = document.getElementById("lblStreak");
const lblCentury = document.getElementById("lblCentury");

const progressBar = document.getElementById("progressBar");
const modeSelect = document.getElementById("modeSelect");
const lblContext = document.getElementById("lblContext");
const lblQuestion = document.getElementById("lblQuestion");
const hintBox = document.getElementById("hintBox");
const btnHint = document.getElementById("btnHint");
const daysGrid = document.getElementById("daysGrid");
const feedbackBox = document.getElementById("feedbackBox");
const btnNext = document.getElementById("btnNext");

const btnCheat = document.getElementById("btnCheat");
const modalCheat = document.getElementById("modalCheat");
const cheatClose = document.getElementById("cheatClose");
const cheatBody = document.getElementById("cheatBody");

const btnStore = document.getElementById("btnStore");
const modalStore = document.getElementById("modalStore");
const storeClose = document.getElementById("storeClose");
const storeGrid = document.getElementById("storeGrid");
const usernameInput = document.getElementById("usernameInput");
const btnSaveUser = document.getElementById("btnSaveUser");

/* =========================
   Store items
========================= */
const STORE_CENTURIES = [
  { century: 1800, label: "1800–1899", price: 25 },
  { century: 1900, label: "1900–1999", price: 25 },
  { century: 2000, label: "2000–2099", price: 0  },
  { century: 2100, label: "2100–2199", price: 25 }
];

function centuryRange(c){
  return { start: c, end: c + 99 };
}
function activeRange(){
  const c = profile.activeCentury;
  return centuryRange(c);
}

/* =========================
   Game state
========================= */
const ROUND_TIME = 60;
let timeLeft = ROUND_TIME;
let timerId = null;
let current = null;
let locked = false;

/* =========================
   Rendering
========================= */
function renderCheatSheet(){
  cheatBody.innerHTML = `
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
    <div class="small">הכל נשמר מקומית (localStorage). אין שרת.</div>
  `;
}
function updateTop(){
  lblUser.textContent = profile.username ? `👤 ${profile.username}` : "👤 אורח";
  lblCoins.textContent = `💰 ${profile.coins}`;
  lblStreak.textContent = `רצף: ${profile.streak} 🔥`;
  const r = activeRange();
  lblCentury.textContent = `מאה: ${r.start}–${r.end}`;
  btnHint.disabled = locked || profile.coins < 10;
  if (profile.coins >= 10){
    btnHint.classList.remove("disabled");
    btnHint.textContent = "💡 רמז (10)";
  } else {
    btnHint.classList.add("disabled");
    btnHint.textContent = "אין כסף (10)";
  }
}
function setProgress(){
  const pct = Math.max(0, Math.min(1, timeLeft / ROUND_TIME));
  progressBar.style.width = (pct * 100).toFixed(2) + "%";
}
function stopTimer(){
  if (timerId){ clearInterval(timerId); timerId = null; }
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
  daysGrid.querySelectorAll("button.dayBtn").forEach(b => b.disabled = state);
  updateTop();
}
function resetRoundUI(){
  feedbackBox.style.display = "none";
  feedbackBox.innerHTML = "";
  btnNext.style.display = "none";
  hintBox.style.display = "none";
  hintBox.innerHTML = "";
  daysGrid.querySelectorAll("button.dayBtn").forEach(b => b.classList.remove("correct","wrong"));
}

/* =========================
   Store UI
========================= */
function renderStore(){
  usernameInput.value = profile.username || "";
  storeGrid.innerHTML = "";

  STORE_CENTURIES.forEach(item => {
    const owned = !!profile.ownedCenturies[String(item.century)];
    const active = profile.activeCentury === item.century;

    const tile = document.createElement("div");
    tile.className = "tile";
    tile.innerHTML = `
      <div class="name">${item.label}</div>
      <div class="desc">פותח תרגול בטווח השנים של המאה הזאת.</div>
      <div class="row2">
        ${active ? `<span class="activeBadge">פעיל</span>` : ""}
        <span class="badge">מחיר: ${item.price} 💰</span>
      </div>
      <div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;">
        <button class="btn ${owned ? "gray" : "dark"}" ${owned ? "disabled" : ""} data-buy="${item.century}">
          ${owned ? "נקנה" : "קנה"}
        </button>
        <button class="btn ${owned ? "green" : "gray"}" ${owned ? "" : "disabled"} data-use="${item.century}">
          השתמש
        </button>
      </div>
    `;
    storeGrid.appendChild(tile);
  });

  storeGrid.querySelectorAll("button[data-buy]").forEach(b => {
    b.addEventListener("click", () => {
      const c = Number(b.getAttribute("data-buy"));
      buyCentury(c);
    });
  });
  storeGrid.querySelectorAll("button[data-use]").forEach(b => {
    b.addEventListener("click", () => {
      const c = Number(b.getAttribute("data-use"));
      setActiveCentury(c);
    });
  });
}
function buyCentury(c){
  const item = STORE_CENTURIES.find(x => x.century === c);
  if (!item) return;
  if (profile.ownedCenturies[String(c)]) return;

  if (profile.coins < item.price){
    alert("אין מספיק כסף 💰");
    return;
  }
  profile.coins -= item.price;
  profile.ownedCenturies[String(c)] = true;
  saveProfile(profile);
  updateTop();
  renderStore();
}
function setActiveCentury(c){
  if (!profile.ownedCenturies[String(c)]) return;
  profile.activeCentury = c;
  saveProfile(profile);
  updateTop();
  renderStore();
  profile.streak = 0;
  saveProfile(profile);
  startNewRound();
}
btnSaveUser.addEventListener("click", () => {
  const name = (usernameInput.value || "").trim().slice(0, 18);
  profile.username = name;
  saveProfile(profile);
  updateTop();
  alert("נשמר ✅");
});

/* =========================
   Rounds (use active century)
========================= */
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

function pickYearFromOwnedCenturies(){
  const owned = Object.keys(profile.ownedCenturies)
    .filter(k => profile.ownedCenturies[k])
    .map(k => Number(k))
    .sort((a,b)=>a-b);

  if (owned.length === 0) return randInt(2000, 2099);

  const c = owned[randInt(0, owned.length - 1)];
  return randInt(c, c + 99);
}

}

function startNewRound(){
  resetRoundUI();
  lockButtons(false);

  const mode = Number(modeSelect.value);
  const year = pickYearFromOwnedCenturies();


  if (mode === 0){
    const ans = doomsdayOfYear(year);
    lblContext.innerHTML = "חשב את יום העוגן השנתי:";
    lblQuestion.textContent = String(year);
    current = { type:"year", year, ansDayIdx: ans };

  } else if (mode === 1){
    const anchorIdx = doomsdayOfYear(year);
    const month = randInt(1,12);
    const baseDay = monthAnchorDay(year, month);
    const shiftOptions = [-2,-1,1,2];
    const shift = shiftOptions[randInt(0, shiftOptions.length-1)];
    let target = utcDate(year, month, baseDay);
    target = addDaysUTC(target, shift);

    lblContext.innerHTML = `בשנת ${year} העוגן הוא <b>${DISPLAY_NAMES[anchorIdx]}</b>.`;
    lblQuestion.textContent = `${target.getUTCDate()} ב${MONTH_NAMES[target.getUTCMonth()+1]}`;
    current = { type:"anchor", year, targetUTC: target, ansDayIdx: weekdayUTC(target) };

  } else {
    const y = year;
    const start = utcDate(y,1,1);
    const d = addDaysUTC(start, randInt(0, 364));
    lblContext.innerHTML = "חשב מאפס:";
    lblQuestion.textContent = `${d.getUTCDate()} ב${MONTH_NAMES[d.getUTCMonth()+1]} ${d.getUTCFullYear()}`;
    current = { type:"full", year: d.getUTCFullYear(), targetUTC: d, ansDayIdx: weekdayUTC(d) };
  }

  updateTop();
  startTimer();
}

/* =========================
   Hint + Explanation
========================= */
function useHint(){
  if (locked) return;
  if (profile.coins < 10) return;

  profile.coins -= 10;
  saveProfile(profile);
  updateTop();

  hintBox.style.display = "block";
  const y = current.year;

  if (current.type === "year"){
    const cen = centuryAnchorClassic(y);
    const yy = y % 100;
    const a = Math.floor(yy / 12);
    const b = yy % 12;
    const c = Math.floor(b / 4);
    const total = a + b + c;

    hintBox.innerHTML =
      `<b>Hint (Year ${y}):</b><br>` +
      `• yy=${yy} → a=${a}, b=${b}, c=${c}<br>` +
      `• add=(a+b+c) mod 7 = ${total % 7}<br>` +
      `• century anchor = <b>${DISPLAY_NAMES[cen]}</b><br>` +
      `<b>Task:</b> ${DISPLAY_NAMES[cen]} + ${total % 7} days = ?`;
    return;
  }

  const yearAnchor = doomsdayOfYear(y);
  const t = current.targetUTC;
  const m = t.getUTCMonth()+1;
  const mAnchor = monthAnchorDay(y, m);
  const diff = t.getUTCDate() - mAnchor;

  hintBox.innerHTML =
    `<b>Hint:</b><br>` +
    `• year doomsday = <b>${DISPLAY_NAMES[yearAnchor]}</b><br>` +
    `• month anchor = day <b>${mAnchor}</b><br>` +
    `• shift = <b>${diff >= 0 ? "+" : ""}${diff}</b> days<br>` +
    `<b>Task:</b> ${DISPLAY_NAMES[yearAnchor]} ${diff >= 0 ? "+" : ""}${diff} = ?`;
}

function detailedExplanation(correctIdx){
  const y = current.year;
  const correctName = DISPLAY_NAMES[correctIdx];

  const yy = y % 100;
  const a = Math.floor(yy / 12);
  const b = yy % 12;
  const c = Math.floor(b / 4);
  const total = a + b + c;

  const cen = centuryAnchorClassic(y);
  const yearAnchor = doomsdayOfYear(y);

  if (current.type === "year"){
    return (
      `Solution: <b>${y} → ${DISPLAY_NAMES[yearAnchor]}</b><br><br>` +
      `1) Century anchor: <b>${DISPLAY_NAMES[cen]}</b><br>` +
      `2) yy=${yy}: a=${a}, b=${b}, c=${c}<br>` +
      `3) (a+b+c) mod 7 = ${total % 7}<br>` +
      `4) ${DISPLAY_NAMES[cen]} + ${total % 7} = <b>${DISPLAY_NAMES[yearAnchor]}</b>`
    );
  }

  const t = current.targetUTC;
  const m = t.getUTCMonth()+1;
  const mAnchor = monthAnchorDay(y, m);
  const diff = t.getUTCDate() - mAnchor;

  const intro = (current.type === "full")
    ? `1) First find year doomsday: <b>${y} → ${DISPLAY_NAMES[yearAnchor]}</b><br>`
    : "";

  return (
    `Solution: <b>${t.getUTCDate()}.${m}.${y} → ${correctName}</b><br><br>` +
    `${intro}` +
    `2) Month anchor: ${mAnchor}.${m} is <b>${DISPLAY_NAMES[yearAnchor]}</b><br>` +
    `3) Shift: ${t.getUTCDate()} - ${mAnchor} = ${diff >= 0 ? "+" : ""}${diff}<br>` +
    `4) ${DISPLAY_NAMES[yearAnchor]} ${diff >= 0 ? "+" : ""}${diff} = <b>${correctName}</b>`
  );
}

/* =========================
   Answer / timeout
========================= */
function checkAnswer(userIdx){
  if (locked) return;
  stopTimer();
  lockButtons(true);

  const correctIdx = current.ansDayIdx;
  const isCorrect = userIdx === correctIdx;

  daysGrid.querySelectorAll("button.dayBtn").forEach((b, idx) => {
    if (idx === correctIdx) b.classList.add("correct");
    else if (idx === userIdx && !isCorrect) b.classList.add("wrong");
  });

  const reward = Number(modeSelect.value) + 1;
  let header = "";
  if (isCorrect){
    profile.coins += reward;
    profile.streak += 1;
    header = "✅ נכון!";
  } else {
    profile.streak = 0;
    header = "❌ לא נכון";
  }
  saveProfile(profile);
  updateTop();

  feedbackBox.innerHTML = `<b>${header}</b><br><br>${detailedExplanation(correctIdx)}`;
  feedbackBox.style.display = "block";
  btnNext.style.display = "block";
}

function handleTimeout(){
  lockButtons(true);

  const correctIdx = current.ansDayIdx;
  daysGrid.querySelectorAll("button.dayBtn").forEach((b, idx) => {
    if (idx === correctIdx) b.classList.add("correct");
  });

  profile.streak = 0;
  saveProfile(profile);
  updateTop();

  feedbackBox.innerHTML = `<b>⏱️ נגמר הזמן</b><br><br>${detailedExplanation(correctIdx)}`;
  feedbackBox.style.display = "block";
  btnNext.style.display = "block";
}

/* =========================
   Wire events
========================= */
btnHint.addEventListener("click", useHint);
btnNext.addEventListener("click", startNewRound);
modeSelect.addEventListener("change", () => {
  profile.streak = 0;
  saveProfile(profile);
  updateTop();
  startNewRound();
});

btnCheat.addEventListener("click", () => {
  renderCheatSheet();
  modalCheat.style.display = "block";
});
cheatClose.addEventListener("click", () => modalCheat.style.display = "none");
modalCheat.addEventListener("click", (e) => { if (e.target === modalCheat) modalCheat.style.display = "none"; });

btnStore.addEventListener("click", () => {
  renderStore();
  modalStore.style.display = "block";
});
storeClose.addEventListener("click", () => modalStore.style.display = "none");
modalStore.addEventListener("click", (e) => { if (e.target === modalStore) modalStore.style.display = "none"; });

/* =========================
   Boot
========================= */
buildDaysButtons();
updateTop();
renderCheatSheet();
startNewRound();
</script>
</body>
</html>
