<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>School Portal — Final (Attendance, Calendar, Editable Grades)</title>

<!-- Font Awesome for icons -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css"/>

<style>
  :root{
    --bg1: #F6FFF9;
    --bg2: #E8F8F6;
    --accent: #3AAFA9;
    --muted: #6B6B7A;
    --card: #ffffff;
    --success: #2ECC71;
    --danger: #FF6B6B;
    --glass: rgba(255,255,255,0.95);
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:"Bookman Old Style","Bookman",serif;background:linear-gradient(135deg,var(--bg1),var(--bg2));color:#122}
  a{color:inherit}

  /* Layout */
  .split{min-height:100vh;display:grid;grid-template-columns:1fr 460px;gap:36px;align-items:center;padding:46px}
  .welcome{padding:34px;border-radius:16px;background:linear-gradient(180deg,#fff,#f8fffe);box-shadow:0 18px 40px rgba(18,25,40,0.06)}
  .logoMark{width:64px;height:64px;border-radius:12px;background:linear-gradient(135deg,var(--accent),#6ad9c8);display:flex;align-items:center;justify-content:center;color:#fff;font-weight:900}
  h1{margin:10px 0 6px;font-size:30px}
  .muted{color:var(--muted)}

  /* Login card */
  .card{padding:22px;border-radius:12px;background:var(--card);box-shadow:0 16px 36px rgba(18,25,40,0.06)}
  label{display:block;margin-top:12px;color:var(--muted);font-size:13px}
  input,select{width:100%;padding:12px;border-radius:10px;border:1px solid rgba(18,25,40,0.06);margin-top:8px;background:transparent;font-size:15px}
  .actions{display:flex;gap:10px;margin-top:14px}
  .btn{padding:10px 14px;border-radius:10px;border:none;cursor:pointer;font-weight:800}
  .btn-primary{background:var(--accent);color:#fff}
  .btn-ghost{background:transparent;border:1px solid rgba(18,25,40,0.06)}

  /* App */
  .app{display:none;min-height:100vh;padding:20px}
  .wrap{display:flex;gap:18px;align-items:flex-start}
  .sidebar{width:260px;border-radius:12px;padding:14px;background:var(--card);box-shadow:0 18px 40px rgba(18,25,40,0.06)}
  .avatar{width:56px;height:56px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#8ce6d8);display:flex;align-items:center;justify-content:center;color:white;font-weight:900}
  .nav{display:flex;flex-direction:column;gap:8px;margin-top:12px}
  .nav button{display:flex;gap:10px;align-items:center;padding:10px;border-radius:10px;border:none;background:transparent;cursor:pointer;font-weight:800;color:var(--muted)}
  .nav button.active{background:linear-gradient(90deg, rgba(58,175,169,0.08), rgba(127,90,240,0.03));color:var(--accent)}
  .main{flex:1}
  .topbar{display:flex;justify-content:space-between;align-items:center;padding:12px;border-radius:10px;background:var(--card);box-shadow:0 10px 30px rgba(18,25,40,0.04)}
  .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:16px;margin-top:16px}
  .card-panel{padding:14px;border-radius:12px;background:var(--card);box-shadow:0 10px 30px rgba(18,25,40,0.04);min-height:100px}
  h2.section-title{margin:0 0 8px}

  /* Attendance card grid */
  .att-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px;margin-top:14px}
  .student-card{
    display:flex;gap:12px;align-items:center;padding:12px;border-radius:12px;background:linear-gradient(180deg,#fff,#fbfffe);
    box-shadow:0 8px 20px rgba(18,25,40,0.04);transition:transform .18s ease,box-shadow .18s ease;
  }
  .student-card:hover{transform:translateY(-6px);box-shadow:0 18px 30px rgba(18,25,40,0.06)}
  .stu-avatar{width:56px;height:56px;border-radius:10px;display:flex;align-items:center;justify-content:center;color:white;font-weight:800}
  .stu-meta{flex:1}
  .stu-meta .name{font-weight:900}
  .status-btns{display:flex;gap:8px}
  .present, .absent {padding:8px 10px;border-radius:8px;border:none;cursor:pointer;font-weight:800}
  .present{background:#e8f9f1;color:var(--accent);border:2px solid rgba(58,175,169,0.12)}
  .absent{background:#fff0f0;color:var(--danger);border:2px solid rgba(255,107,107,0.08)}
  .badge {padding:6px 8px;border-radius:999px;font-weight:800;font-size:13px}

  .ind-present{background:var(--success);color:white}
  .ind-absent{background:var(--danger);color:white}
  .ind-none{background:rgba(0,0,0,0.06);color:var(--muted)}

  /* Calendar mini UI (Option 3) */
  .mini-cal {width:320px;padding:12px;border-radius:12px;background:var(--card);box-shadow:0 12px 30px rgba(18,25,40,0.06)}
  .cal-header{display:flex;align-items:center;justify-content:space-between}
  .cal-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:6px;margin-top:10px}
  .cal-day{padding:8px;border-radius:8px;text-align:center;cursor:pointer;position:relative}
  .cal-day .dot {width:7px;height:7px;border-radius:50%;position:absolute;left:50%;transform:translateX(-50%);bottom:6px;opacity:0.95}
  .cal-day.today{box-shadow:0 6px 18px rgba(58,175,169,0.08);border:1px solid rgba(58,175,169,0.08)}
  .cal-day.dim{color:#bbb}
  .cal-nav{display:flex;gap:8px;align-items:center}

  /* modal */
  .modal-backdrop{position:fixed;left:0;top:0;width:100%;height:100%;background:rgba(0,0,0,0.35);display:none;align-items:center;justify-content:center;z-index:120}
  .modal{width:920px;max-width:96%;background:var(--card);padding:18px;border-radius:12px;box-shadow:0 40px 80px rgba(18,25,40,0.12);max-height:86vh;overflow:auto}

  /* grades table */
  table{width:100%;border-collapse:collapse;margin-top:12px}
  th,td{padding:10px;border-bottom:1px solid rgba(18,25,40,0.06);text-align:left}
  input.grade-input{width:80px;padding:6px;border-radius:6px;border:1px solid rgba(18,25,40,0.06)}

  /* small */
  .muted-sm{font-size:13px;color:var(--muted)}

  /* logout */
  .logout{position:fixed;right:22px;top:18px;background:var(--danger);color:white;padding:8px 12px;border-radius:8px;border:none;cursor:pointer;display:none;z-index:50}

  /* responsive */
  @media (max-width:980px){
    .split{grid-template-columns:1fr;padding:20px;gap:18px}
    .sidebar{width:100%;display:flex;flex-direction:row;gap:12px;align-items:center}
    .nav{flex-direction:row;overflow:auto}
    .mini-cal{width:100%}
  }
</style>
</head>
<body>

<!-- LOGIN SPLIT -->
<div id="loginScreen" class="split">
  <div class="welcome">
    <div style="display:flex;gap:12px;align-items:center">
      <div class="logoMark">SM</div>
      <div>
        <div style="font-weight:900">St. Mary School</div>
        <div class="muted">Student & Teacher Portal</div>
      </div>
    </div>

    <h1>Welcome — Learn beautifully</h1>
    <p class="muted">A gentle place for study, assessment, attendance and reporting.</p>
    <blockquote>"MISTAKES ARE PROOFS THAT YOU'RE LEARNING"</blockquote>

    <div style="margin-top:10px" class="muted-sm">
      <strong>IDs (examples):</strong> alice01, rahul02, meera03, ... aryan40 • <strong>teacher:</strong> teacher01 • <strong>password:</strong> 123
    </div>
  </div>

  <div class="card" role="form">
    <h3 id="loginTitle">Sign in</h3>
    <label>Role</label>
    <select id="role">
      <option value="student">Student</option>
      <option value="teacher">Teacher</option>
    </select>

    <label>User ID</label>
    <input id="userid" type="text" placeholder="e.g., alice01">

    <label>Password</label>
    <input id="password" type="password" placeholder="123">

    <div class="actions" style="margin-top:14px">
      <button class="btn btn-primary" onclick="doLogin()">Sign in</button>
      <button class="btn btn-ghost" onclick="demoFill()">Demo Fill</button>
    </div>
  </div>
</div>

<!-- APP -->
<button id="logoutBtn" class="logout" onclick="logout()">Logout</button>

<div id="app" class="app" style="display:none">
  <div class="wrap">
    <aside class="sidebar">
      <div style="display:flex;gap:12px;align-items:center">
        <div class="avatar" id="avatarMark">A</div>
        <div>
          <div id="userName" style="font-weight:900">User</div>
          <div class="muted-sm" id="userRole">Role</div>
        </div>
      </div>

      <nav class="nav" id="nav"></nav>
    </aside>

    <main class="main">
      <div class="topbar">
        <div>
          <div style="font-size:18px;font-weight:900" id="pageTitle">Dashboard</div>
          <div class="muted-sm" id="pageSub">Overview</div>
        </div>

        <div style="display:flex;gap:10px;align-items:center">
          <div class="muted-sm" id="nextExamInfo">Next exam: Sat 09:30–11:30</div>
        </div>
      </div>

      <!-- Dashboard panel -->
      <section id="panel-dashboard" class="card-panel" style="display:none">
        <h2 class="section-title">Welcome</h2>
        <div class="muted-sm" id="welcomeText"></div>

        <div class="grid" style="margin-top:14px">
          <div class="card-panel">
            <h3 style="margin:0 0 8px">Academics</h3>
            <div id="academicsBlock" class="muted-sm"></div>
          </div>

          <div class="card-panel">
            <h3 style="margin:0 0 8px">Timetable</h3>
            <div id="timetableBlock" class="muted-sm"></div>
          </div>

          <div class="card-panel">
            <h3 style="margin:0 0 8px">Today's Tasks</h3>
            <div id="todayBlock" class="muted-sm"></div>
          </div>
        </div>
      </section>

      <!-- Student pages -->
      <section id="panel-academics" class="card-panel" style="display:none"><h2 class="section-title">Academics</h2><div id="academicsList"></div></section>
      <section id="panel-timetable" class="card-panel" style="display:none"><h2 class="section-title">Timetable</h2><table id="tableTimetable"><thead><tr><th>Day</th><th>Subjects</th></tr></thead><tbody></tbody></table></section>
      <section id="panel-assignments" class="card-panel" style="display:none"><h2 class="section-title">Assignments</h2><div id="assignmentsList" class="muted-sm"></div></section>
      <section id="panel-exams" class="card-panel" style="display:none"><h2 class="section-title">Exams & Internals</h2><div id="examsList" class="muted-sm"></div></section>
      <section id="panel-results" class="card-panel" style="display:none"><h2 class="section-title">Exam Results</h2><div id="resultsBlock" class="muted-sm"></div><div style="margin-top:12px"><button class="btn btn-primary" onclick="generateReportCard()">Print Report Card</button></div></section>

      <!-- Teacher pages -->
      <section id="panel-teacher-timetable" class="card-panel" style="display:none"><h2 class="section-title">Teacher Timetable</h2><table id="teacherTable"><thead><tr><th>Day</th><th>No. Periods</th><th>Subjects (section - room)</th></tr></thead><tbody></tbody></table></section>

      <section id="panel-attendance" class="card-panel" style="display:none">
        <h2 class="section-title">Attendance — Card View</h2>
        <div style="display:flex;gap:8px;align-items:center;margin-top:8px">
          <div class="mini-cal" id="miniCal">
            <div class="cal-header">
              <div class="cal-nav">
                <button class="btn btn-ghost" onclick="prevMonth()">‹</button>
                <div style="width:12px"></div>
                <div id="calMonthYear" style="font-weight:800"></div>
                <div style="width:12px"></div>
                <button class="btn btn-ghost" onclick="nextMonth()">›</button>
              </div>
              <div class="muted-sm">Click day to edit attendance</div>
            </div>
            <div class="cal-grid" id="calGrid"></div>
          </div>

          <div style="flex:1"></div>

          <div style="margin-left:auto;display:flex;gap:8px">
            <button class="btn btn-primary" onclick="markAllPresentToday()">Mark All Present</button>
            <button class="btn btn-ghost" onclick="clearTodayAttendance()">Clear Today's</button>
          </div>
        </div>

        <div class="att-grid" id="attendanceGrid"></div>
      </section>

      <section id="panel-grades" class="card-panel" style="display:none">
        <h2 class="section-title">Grades — Edit Inline</h2>
        <div style="display:flex;gap:10px;align-items:center">
          <button class="btn btn-primary" onclick="saveAllGrades()">Save All Grades</button>
          <div class="muted-sm" style="margin-left:auto">Changes saved to localStorage</div>
        </div>
        <div id="gradesList" style="margin-top:12px"></div>
      </section>

      <section id="panel-failed" class="card-panel" style="display:none">
        <h2 class="section-title">Failed Students (Below 40%)</h2>
        <div id="failedList" class="muted-sm"></div>
      </section>
    </main>
  </div>
</div>

<!-- Modal for date attendance editing -->
<div class="modal-backdrop" id="modalBackdrop">
  <div class="modal" id="dateModal">
    <div style="display:flex;justify-content:space-between;align-items:center">
      <h3 id="modalTitle">Attendance for DATE</h3>
      <div><button class="btn btn-ghost" onclick="closeModal()">Close</button></div>
    </div>
    <div style="margin-top:12px" id="modalContent"></div>
  </div>
</div>

<!-- Print area -->
<div id="printArea" style="display:none"></div>

<script>
/* ================= DATA ================= */
/* 40 students (IDs + names + grades) */
const STUDENTS = [
  {id:'alice01', name:'Alice Thomas', grade:84},
  {id:'rahul02', name:'Rahul Menon', grade:78},
  {id:'meera03', name:'Meera Nair', grade:67},
  {id:'harsha04', name:'Harsha Reddy', grade:92},
  {id:'kavya05', name:'Kavya S', grade:88},
  {id:'rohan06', name:'Rohan Gupta', grade:73},
  {id:'aditi07', name:'Aditi Sharma', grade:81},
  {id:'vedant08', name:'Vedant Rao', grade:59},
  {id:'sahana09', name:'Sahana Pillai', grade:95},
  {id:'joel10', name:'Joel Fernandes', grade:62},
  {id:'varun11', name:'Varun Kumar', grade:37},   // fail
  {id:'laya12', name:'Laya Prabhu', grade:46},
  {id:'nitin13', name:'Nitin Paul', grade:28},    // fail
  {id:'srushti14', name:'Srushti K', grade:53},
  {id:'adarsh15', name:'Adarsh Mohan', grade:33}, // fail
  {id:'simran16', name:'Simranjeet Singh', grade:77},
  {id:'gaurav17', name:'Gaurav Desai', grade:41},
  {id:'angel18', name:'Angel Maria', grade:96},
  {id:'faizan19', name:'Faizan Khan', grade:22},  // fail
  {id:'bhavana20', name:'Bhavana Joshi', grade:48},
  {id:'sahil21', name:'Sahil Kapoor', grade:36},  // fail
  {id:'tanya22', name:'Tanya George', grade:72},
  {id:'rishi23', name:'Rishi Bhandari', grade:56},
  {id:'christina24', name:"Christina D'Silva", grade:91},
  {id:'zaid25', name:'Zaid Mohammed', grade:63},
  {id:'ananya26', name:'Ananya Prasad', grade:45},
  {id:'kevin27', name:'Kevin Mathew', grade:39},  // fail
  {id:'sneha28', name:'Sneha Iyer', grade:74},
  {id:'devanand29', name:'Devanand A', grade:52},
  {id:'janhavi30', name:'Janhavi R', grade:27},   // fail
  {id:'pranav31', name:'Pranav Krishna', grade:58},
  {id:'mohanraj32', name:'Mohanraj P', grade:86},
  {id:'ritika33', name:'Ritika Shetty', grade:44},
  {id:'vishal34', name:'Vishal Raj', grade:18},   // fail
  {id:'srushti35', name:'Srushti Patil', grade:97},
  {id:'nihal36', name:'Nihal Jacob', grade:31},   // fail
  {id:'pooja37', name:'Pooja Reddy', grade:69},
  {id:'rudra38', name:'Rudra Shah', grade:25},    // fail
  {id:'sandra39', name:'Sandra Jose', grade:76},
  {id:'aryan40', name:'Aryan Chakraborty', grade:82}
];

const TEACHER = {id:'teacher01', name:'Ms. Sharma', pass:'123'};
const PASS_THRESHOLD = 40;

/* Storage key */
const STORE_KEY = 'portal_final_store_v1';

/* load/store */
function loadStore(){
  const raw = localStorage.getItem(STORE_KEY);
  if(!raw){
    const init = { attendance: {}, grades: {} };
    STUDENTS.forEach(s=> init.grades[s.id] = s.grade);
    localStorage.setItem(STORE_KEY, JSON.stringify(init));
    return init;
  }
  try { return JSON.parse(raw); } catch(e){ return { attendance:{}, grades:{} }; }
}
function saveStore(s){ localStorage.setItem(STORE_KEY, JSON.stringify(s)); }
let STORE = loadStore();

/* session */
let SESSION = {role:null, id:null, name:null};

/* UI refs */
const loginScreen = document.getElementById('loginScreen');
const app = document.getElementById('app');
const nav = document.getElementById('nav');
const avatarMark = document.getElementById('avatarMark');
const userNameEl = document.getElementById('userName');
const userRoleEl = document.getElementById('userRole');
const pageTitle = document.getElementById('pageTitle');
const pageSub = document.getElementById('pageSub');
const logoutBtn = document.getElementById('logoutBtn');

/* ---------- Demo fill ---------- */
function demoFill(){ document.getElementById('role').value='teacher'; document.getElementById('userid').value='teacher01'; document.getElementById('password').value='123'; }

/* ---------- Login ---------- */
function doLogin(){
  const role = document.getElementById('role').value;
  const id = (document.getElementById('userid').value||'').trim();
  const pass = (document.getElementById('password').value||'').trim();
  if(pass !== '123'){ alert('Incorrect password — demo uses 123'); return; }

  if(role === 'student'){
    const found = STUDENTS.find(s=>s.id === id);
    if(!found){ alert('Student ID not found. Check the ID list.'); return; }
    SESSION = {role:'student', id:found.id, name:found.name};
    enterStudent();
  } else {
    if(id !== TEACHER.id){ alert('Teacher ID is teacher01'); return; }
    SESSION = {role:'teacher', id:TEACHER.id, name:TEACHER.name};
    enterTeacher();
  }
}

/* ---------- Enter student / teacher ---------- */
function enterStudent(){
  loginScreen.style.display='none'; app.style.display='block'; logoutBtn.style.display='block';
  avatarMark.textContent = SESSION.name.split(' ').map(n=>n[0]).slice(0,2).join('');
  userNameEl.textContent = SESSION.name; userRoleEl.textContent='Student — Grade 12 A';
  buildNav('student'); showPanel('dashboard'); renderStudentOverview();
}
function enterTeacher(){
  loginScreen.style.display='none'; app.style.display='block'; logoutBtn.style.display='block';
  avatarMark.textContent = 'TS'; userNameEl.textContent = SESSION.name; userRoleEl.textContent='Teacher';
  buildNav('teacher'); showPanel('dashboard'); renderTeacherOverview();
}

/* ---------- Navigation ---------- */
function buildNav(role){
  nav.innerHTML='';
  const common = [{k:'dashboard',t:'Dashboard'}];
  if(role==='student'){
    const items = [...common, {k:'academics',t:'Academics'},{k:'timetable',t:'Timetable'},{k:'assignments',t:'Assignments'},{k:'exams',t:'Exams'},{k:'results',t:'Results'}];
    items.forEach(it=> navButton(it.k,it.t));
  } else {
    const items = [...common, {k:'teacher-timetable',t:'Timetable'},{k:'attendance',t:'Attendance'},{k:'grades',t:'Grades'},{k:'failed',t:'Failed'}];
    items.forEach(it=> navButton(it.k,it.t));
  }
  nav.children[0]?.classList.add('active');
}
function navButton(key,title){
  const btn = document.createElement('button'); btn.textContent = title;
  btn.onclick = ()=>{ document.querySelectorAll('.nav button').forEach(b=>b.classList.remove('active')); btn.classList.add('active'); showPanel(key); };
  nav.appendChild(btn);
}
function showPanel(key){
  pageTitle.textContent = {dashboard:'Dashboard',academics:'Academics',timetable:'Timetable',assignments:'Assignments',exams:'Exams & Internals',results:'Exam Results','teacher-timetable':'Teacher Timetable',attendance:'Attendance',grades:'Grades',failed:'Failed Students'}[key] || 'Dashboard';
  document.querySelectorAll('main section').forEach(s=>s.style.display='none');
  const panel = document.getElementById('panel-' + key);
  if(panel) panel.style.display='block';
  // render on demand
  if(key==='dashboard'){ SESSION.role==='student' ? renderStudentOverview() : renderTeacherOverview(); }
  if(key==='academics') renderAcademics();
  if(key==='timetable') renderTimetable();
  if(key==='assignments') renderAssignments();
  if(key==='exams') renderExams();
  if(key==='results') renderResults();
  if(key==='teacher-timetable') renderTeacherTimetable();
  if(key==='attendance') renderAttendanceGrid();
  if(key==='grades') renderGradesTable();
  if(key==='failed') renderFailedList();
}

/* ---------- Student renderers ---------- */
function renderStudentOverview(){
  document.getElementById('welcomeText').textContent = `Hello ${SESSION.name}. Grade 12 A — St. Mary School.`;
  document.getElementById('academicsBlock').textContent = 'Physics · Maths · Biology · Circuit Design · English';
  document.getElementById('timetableBlock').textContent = 'Mon: Circuit Design, Physics · Tue: Maths, Biology · Wed: OFF · Thu: Circuit Design, Biology · Fri: English, Physics · Sat: Maths, English';
  document.getElementById('todayBlock').textContent = todaysAssignmentsText();
  renderAcademics(); renderTimetable(); renderAssignments(); renderExams(); renderResults();
}
function renderAcademics(){
  const el = document.getElementById('academicsList'); el.innerHTML = '';
  ['Physics','Maths','Biology','Circuit Design','English'].forEach(s=>{
    const d = document.createElement('div'); d.className='card-panel'; d.style.marginBottom='8px'; d.innerHTML = `<strong>${s}</strong><div class="muted-sm">Teacher: Ms. Sharma</div>`;
    el.appendChild(d);
  });
}
function renderTimetable(){
  const tbody = document.querySelector('#tableTimetable tbody'); tbody.innerHTML='';
  const TT = {Monday:['Circuit Design','Physics'],Tuesday:['Maths','Biology'],Wednesday:['OFF'],Thursday:['Circuit Design','Biology'],Friday:['English','Physics'],Saturday:['Maths','English']};
  for(const d of ['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday']){
    const row = tbody.insertRow(); row.insertCell().textContent = d; row.insertCell().textContent = TT[d].join(', ');
  }
}
function todaysAssignmentsText(){
  const day = new Date().toLocaleString('en-US',{weekday:'long'});
  const tasks = {Monday:"Physics numericals + Circuit Design worksheet",Tuesday:"Maths algebra problems + Biology labeling",Wednesday:"OFF — Rest",Thursday:"Circuit Design project + Biology notes",Friday:"English essay + Physics revision",Saturday:"Maths practice test + English grammar"};
  return `Today is ${day}. ${tasks[day] || 'No assignments.'}`;
}
function renderAssignments(){ document.getElementById('assignmentsList').textContent = todaysAssignmentsText(); }
function renderExams(){
  const el = document.getElementById('examsList'); el.innerHTML='';
  const tests = getStudentTests(SESSION.id);
  tests.forEach(t=>{ const d = document.createElement('div'); d.className='muted-sm'; d.innerHTML=`<strong>${t.subject}</strong> — ${t.marks}/100 on ${t.date}`; el.appendChild(d); });
}
function renderResults(){
  const el = document.getElementById('resultsBlock'); el.innerHTML = '';
  const grade = STORE.grades[SESSION.id] ?? findStudentGrade(SESSION.id);
  el.innerHTML = `<div><strong>${SESSION.name}</strong></div><div style="margin-top:8px">Overall Grade: ${grade}% — ${grade< PASS_THRESHOLD? '<span style="color:var(--danger);font-weight:800">FAILED</span>' : '<span style="color:var(--success);font-weight:800">PASS</span>'}</div>`;
}
function getStudentTests(id){
  const g = STORE.grades[id] ?? findStudentGrade(id);
  return [{subject:'Mathematics',date:'2025-11-10',marks:Math.round(g*0.9)},{subject:'Physics',date:'2025-10-22',marks:Math.round(g*0.85)}];
}

/* ---------- Teacher renderers ---------- */
function renderTeacherOverview(){
  document.getElementById('welcomeText').textContent = `Welcome ${TEACHER.name}. Manage attendance & grades below.`;
  populateTeacherTimetable();
}
function populateTeacherTimetable(){
  const tbody = document.querySelector('#teacherTable tbody'); tbody.innerHTML='';
  const tt = {Monday:[{subject:'Mathematics',section:'12A',room:'201'},{subject:'Physics',section:'12B',room:'105'}],Thursday:[{subject:'Circuit Design',section:'12A',room:'LAB2'},{subject:'Biology',section:'12C',room:'LAB1'},{subject:'English',section:'11A',room:'103'}],Friday:[{subject:'Revision',section:'12A',room:'201'}]};
  for(const d of ['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday']){
    const arr = tt[d]||[]; const row = tbody.insertRow();
    row.insertCell().textContent = d; row.insertCell().textContent = arr.length; row.insertCell().innerHTML = arr.map(x=>`${x.subject} — ${x.section} (${x.room})`).join('<br>') || '-';
  }
}

/* ---------- Attendance grid (card view) ---------- */
function renderAttendanceGrid(){
  // render calendar mini
  renderMiniCalendar();
  // grid
  const container = document.getElementById('attendanceGrid'); container.innerHTML='';
  const dateKey = todayDate();
  const dayAttendance = (STORE.attendance && STORE.attendance[dateKey]) ? STORE.attendance[dateKey] : {};
  STUDENTS.forEach(s=>{
    const card = document.createElement('div'); card.className='student-card';
    card.innerHTML = `
      <div class="stu-avatar" style="background:${avatarColor(s.id)}">${avatarInitials(s.name)}</div>
      <div class="stu-meta"><div class="name">${s.name}</div><div class="muted-sm">${s.id}</div></div>
      <div style="display:flex;flex-direction:column;align-items:flex-end;gap:8px">
        <div class="badge ${dayAttendance[s.id] === 'Present' ? 'ind-present' : (dayAttendance[s.id] === 'Absent' ? 'ind-absent' : 'ind-none')}" id="badge_${s.id}">
          ${dayAttendance[s.id] || 'Not marked'}
        </div>
        <div class="status-btns">
          <button class="present" onclick="markPresent('${s.id}')">Present</button>
          <button class="absent" onclick="markAbsent('${s.id}')">Absent</button>
        </div>
      </div>
    `;
    container.appendChild(card);
  });
}

/* ---------- Mini calendar (Option 3) ---------- */
let calDate = new Date(); // current shown month
function renderMiniCalendar(){
  const calGrid = document.getElementById('calGrid');
  const monthYear = document.getElementById('calMonthYear');
  calGrid.innerHTML = '';
  const year = calDate.getFullYear(), month = calDate.getMonth();
  monthYear.textContent = calDate.toLocaleString('default',{month:'long', year:'numeric'});

  // first day of month
  const first = new Date(year, month, 1);
  const startDay = first.getDay(); // 0 Sun .. 6 Sat
  // days in month
  const daysInMonth = new Date(year, month+1, 0).getDate();

  // add blank cells for startDay (convert to Monday-first or keep Sunday-first — keep Sunday-first)
  for(let i=0;i<startDay;i++){ const d=document.createElement('div'); d.className='cal-day dim'; calGrid.appendChild(d); }

  // render days
  for(let day=1; day<=daysInMonth; day++){
    const dCell = document.createElement('div'); dCell.className='cal-day';
    const dDate = new Date(year, month, day);
    const iso = dDate.toISOString().slice(0,10);
    dCell.innerHTML = `<div style="font-weight:800">${day}</div>`;
    // mark dot(s) if attendance exists
    const att = STORE.attendance && STORE.attendance[iso] ? STORE.attendance[iso] : {};
    const presentCount = Object.values(att).filter(v=>v==='Present').length;
    const absentCount = Object.values(att).filter(v=>v==='Absent').length;
    if(presentCount) {
      const dot = document.createElement('div'); dot.className='dot'; dot.style.background = 'var(--success)'; dCell.appendChild(dot);
    } else if(absentCount) {
      const dot = document.createElement('div'); dot.className='dot'; dot.style.background = 'var(--danger)'; dCell.appendChild(dot);
    }
    // today highlight
    const todayIso = todayDate();
    if(iso === todayIso) dCell.classList.add('today');
    // click opens modal to edit attendance of that date
    dCell.onclick = ()=> openDateModal(iso);
    calGrid.appendChild(dCell);
  }
}

function prevMonth(){ calDate.setMonth(calDate.getMonth()-1); renderMiniCalendar(); }
function nextMonth(){ calDate.setMonth(calDate.getMonth()+1); renderMiniCalendar(); }

/* ---------- Modal for date attendance ---------- */
function openDateModal(dateIso){
  const modalBackdrop = document.getElementById('modalBackdrop');
  const modalTitle = document.getElementById('modalTitle');
  const modalContent = document.getElementById('modalContent');
  modalTitle.textContent = `Attendance — ${dateIso}`;
  // generate list of students with toggles for that date
  const dayAttendance = (STORE.attendance && STORE.attendance[dateIso]) ? {...STORE.attendance[dateIso]} : {};
  let html = `<div style="display:flex;gap:10px;align-items:center;margin-bottom:8px"><button class="btn btn-primary" onclick="markAllForDate('${dateIso}','Present')">Mark All Present</button><button class="btn btn-ghost" onclick="markAllForDate('${dateIso}','Absent')">Mark All Absent</button><button class="btn btn-ghost" style="margin-left:auto" onclick="closeModal()">Close</button></div>`;
  html += `<table><thead><tr><th>#</th><th>Student</th><th>ID</th><th>Status</th></tr></thead><tbody>`;
  STUDENTS.forEach((s,idx)=>{
    const cur = dayAttendance[s.id] || 'Not marked';
    html += `<tr><td>${idx+1}</td><td>${s.name}</td><td>${s.id}</td><td>
      <button class="present" onclick="setAttendanceForDate('${dateIso}','${s.id}','Present')">Present</button>
      <button class="absent" onclick="setAttendanceForDate('${dateIso}','${s.id}','Absent')">Absent</button>
      <span style="margin-left:8px;font-weight:800" id="modal_badge_${dateIso}_${s.id}">${cur}</span>
    </td></tr>`;
  });
  html += `</tbody></table>`;
  modalContent.innerHTML = html;
  modalBackdrop.style.display = 'flex';
}

function closeModal(){ document.getElementById('modalBackdrop').style.display = 'none'; renderMiniCalendar(); renderAttendanceGrid(); }

function setAttendanceForDate(dateIso, studentId, status){
  if(!STORE.attendance) STORE.attendance = {};
  if(!STORE.attendance[dateIso]) STORE.attendance[dateIso] = {};
  STORE.attendance[dateIso][studentId] = status;
  saveStore(STORE);
  // update modal badge
  const badge = document.getElementById(`modal_badge_${dateIso}_${studentId}`);
  if(badge) badge.textContent = status;
  // update today's badge if editing today
  if(dateIso === todayDate()){
    const bd = document.getElementById('badge_' + studentId);
    if(bd){ bd.className = 'badge ' + (status==='Present' ? 'ind-present' : 'ind-absent'); bd.textContent = status; }
  }
}

function markAllForDate(dateIso,status){
  if(!STORE.attendance) STORE.attendance = {};
  STORE.attendance[dateIso] = STORE.attendance[dateIso] || {};
  STUDENTS.forEach(s=> STORE.attendance[dateIso][s.id] = status );
  saveStore(STORE);
  // refresh modal content
  openDateModal(dateIso);
}

/* ---------- Helper: mark present/absent today ---------- */
function ensureTodayNode(){
  const d = todayDate();
  if(!STORE.attendance) STORE.attendance = {};
  if(!STORE.attendance[d]) STORE.attendance[d] = {};
  return d;
}
function markPresent(id){
  const d = ensureTodayNode();
  STORE.attendance[d][id] = 'Present';
  saveStore(STORE);
  updateBadge(id,'Present');
  renderMiniCalendar();
}
function markAbsent(id){
  const d = ensureTodayNode();
  STORE.attendance[d][id] = 'Absent';
  saveStore(STORE);
  updateBadge(id,'Absent');
  renderMiniCalendar();
}
function updateBadge(id,status){
  const b = document.getElementById('badge_' + id);
  if(!b) return;
  b.className = 'badge ' + (status==='Present' ? 'ind-present' : 'ind-absent');
  b.textContent = status;
}
function markAllPresentToday(){
  const d = ensureTodayNode();
  STUDENTS.forEach(s=> STORE.attendance[d][s.id] = 'Present');
  saveStore(STORE); renderAttendanceGrid(); renderMiniCalendar();
}
function clearTodayAttendance(){
  const d = todayDate();
  if(STORE.attendance && STORE.attendance[d]) delete STORE.attendance[d];
  saveStore(STORE); renderAttendanceGrid(); renderMiniCalendar();
}

/* ---------- Grades: inline editing ---------- */
function renderGradesTable(){
  const container = document.getElementById('gradesList'); container.innerHTML = '';
  const tbl = document.createElement('table');
  tbl.innerHTML = `<thead><tr><th>#</th><th>Name</th><th>ID</th><th>Marks (%)</th><th>Result</th></tr></thead>`;
  const tbody = document.createElement('tbody');
  STUDENTS.forEach((s,idx)=>{
    const current = STORE.grades[s.id] ?? s.grade;
    const tr = document.createElement('tr');
    const resHtml = current < PASS_THRESHOLD ? `<span style="color:var(--danger);font-weight:800">FAIL</span>` : `<span style="color:var(--success);font-weight:800">PASS</span>`;
    tr.innerHTML = `<td>${idx+1}</td><td>${s.name}</td><td>${s.id}</td><td><input class="grade-input" type="number" min="0" max="100" id="g_${s.id}" value="${current}"></td><td id="r_${s.id}">${resHtml}</td>`;
    tbody.appendChild(tr);
  });
  tbl.appendChild(tbody); container.appendChild(tbl);
}
function saveAllGrades(){
  STUDENTS.forEach(s=>{
    const el = document.getElementById('g_' + s.id);
    if(!el) return;
    let v = parseInt(el.value);
    if(isNaN(v)) v = 0;
    STORE.grades[s.id] = Math.max(0,Math.min(100,v));
    const resEl = document.getElementById('r_' + s.id);
    resEl.innerHTML = STORE.grades[s.id] < PASS_THRESHOLD ? `<span style="color:var(--danger);font-weight:800">FAIL</span>` : `<span style="color:var(--success);font-weight:800">PASS</span>`;
  });
  saveStore(STORE);
  alert('Grades saved.');
  renderFailedList();
}

/* ---------- Failed list ---------- */
function renderFailedList(){
  const fail = STUDENTS.filter(s => (STORE.grades[s.id] ?? s.grade) < PASS_THRESHOLD);
  const el = document.getElementById('failedList'); el.innerHTML='';
  if(!fail.length) el.textContent = 'No failed students';
  else {
    const ul = document.createElement('ul');
    fail.forEach(f => { const li = document.createElement('li'); li.innerHTML = `${f.name} — ${(STORE.grades[f.id] ?? f.grade)}%`; ul.appendChild(li); });
    el.appendChild(ul);
  }
}

/* ---------- Report card ---------- */
function generateReportCard(){
  if(SESSION.role !== 'student'){ alert('Report card available for student login only'); return; }
  const grade = STORE.grades[SESSION.id] ?? findStudentGrade(SESSION.id);
  const student = STUDENTS.find(s=>s.id===SESSION.id);
  const html = `
    <div style="padding:22px;font-family:inherit;">
      <h2 style="margin:0 0 6px">St. Mary School</h2>
      <div style="margin-bottom:12px">Report Card — ${student.name}</div>
      <table style="width:100%;border-collapse:collapse">
        <thead><tr style="background:#f6f6f6"><th style="padding:8px;border:1px solid #ddd">Subject</th><th style="padding:8px;border:1px solid #ddd">Marks (%)</th></tr></thead>
        <tbody>
          <tr><td style="padding:8px;border:1px solid #ddd">Physics</td><td style="padding:8px;border:1px solid #ddd">${Math.max(0,Math.round(grade*0.95))}</td></tr>
          <tr><td style="padding:8px;border:1px solid #ddd">Maths</td><td style="padding:8px;border:1px solid #ddd">${grade}</td></tr>
          <tr><td style="padding:8px;border:1px solid #ddd">Biology</td><td style="padding:8px;border:1px solid #ddd">${Math.max(0,Math.round(grade*0.9))}</td></tr>
          <tr><td style="padding:8px;border:1px solid #ddd">Circuit Design</td><td style="padding:8px;border:1px solid #ddd">${Math.max(0,Math.round(grade*0.92))}</td></tr>
          <tr><td style="padding:8px;border:1px solid #ddd">English</td><td style="padding:8px;border:1px solid #ddd">${Math.max(0,Math.round(grade*0.96))}</td></tr>
        </tbody>
      </table>
      <div style="margin-top:12px"><strong>Overall:</strong> ${grade}% — ${grade < PASS_THRESHOLD ? 'FAILED' : 'PASS'}</div>
      <div style="margin-top:18px">Teacher remark: ${grade < PASS_THRESHOLD ? 'Needs improvement' : 'Good performance'}</div>
    </div>
  `;
  const printArea = document.getElementById('printArea'); printArea.innerHTML = html; printArea.style.display = 'block';
  setTimeout(()=>window.print(),200);
}

/* ---------- Utilities ---------- */
function todayDate(){ return new Date().toISOString().slice(0,10); }
function findStudentGrade(id){ const s = STUDENTS.find(x=>x.id===id); return s ? s.grade : 0; }

/* Static avatar helpers (deterministic color per id) */
function avatarColor(id){
  const colors = ['#FFB3BA','#FFDFBA','#FFFFBA','#BAFFC9','#BAE1FF','#E0BBFF','#FFCCE5','#D1F2EB'];
  let code = 0; for(let i=0;i<id.length;i++) code = code*31 + id.charCodeAt(i);
  return colors[Math.abs(code) % colors.length];
}
function avatarInitials(name){
  const parts = name.split(' ').filter(Boolean);
  if(parts.length===1) return parts[0].slice(0,2).toUpperCase();
  return (parts[0][0]+parts[1][0]).toUpperCase();
}

/* ---------- Logout ---------- */
function logout(){
  SESSION = {role:null,id:null,name:null}; app.style.display='none'; loginScreen.style.display='grid'; logoutBtn.style.display='none'; nav.innerHTML=''; document.getElementById('userid').value=''; document.getElementById('password').value='';
}

/* ---------- On load / initial renderers ---------- */
function showHomeAfterLogin(){
  buildNav(SESSION.role); showPanel('dashboard');
  if(SESSION.role === 'teacher'){ renderGradesTable(); renderFailedList(); }
}
function renderGradesTable(){ renderGradesTableInner(); renderFailedList(); }
function renderGradesTableInner(){
  // reuse function used above renderGradesTable
  renderGradesTable(); // call the defined function
}

/* Small wrappers to avoid duplicate names collision */
function renderGradesTable(){ renderGradesTableImpl(); }
function renderGradesTableImpl(){
  const container = document.getElementById('gradesList'); container.innerHTML = '';
  const tbl = document.createElement('table');
  tbl.innerHTML = `<thead><tr><th>#</th><th>Name</th><th>ID</th><th>Marks (%)</th><th>Result</th></tr></thead>`;
  const tbody = document.createElement('tbody');
  STUDENTS.forEach((s,idx)=>{
    const cur = STORE.grades[s.id] ?? s.grade;
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${idx+1}</td><td>${s.name}</td><td>${s.id}</td><td><input class="grade-input" id="g_${s.id}" type="number" min="0" max="100" value="${cur}"></td><td id="r_${s.id}">${ cur < PASS_THRESHOLD ? '<span style="color:var(--danger);font-weight:800">FAIL</span>' : '<span style="color:var(--success);font-weight:800">PASS</span>' }</td>`;
    tbody.appendChild(tr);
  });
  tbl.appendChild(tbody); container.appendChild(tbl);
}

/* Expose a few handlers used earlier (names) */
function renderGrades(){ renderGradesTableImpl(); }
function renderAttendanceGridWrapper(){ renderAttendanceGrid(); }
function renderFailedListWrapper(){ renderFailedList(); }

/* ---------- Initial execution for unknown: nothing until login ---------- */
document.addEventListener('DOMContentLoaded', ()=>{
  // nothing automatically
});

/* Provide some named functions used in HTML */
window.prevMonth = prevMonth; window.nextMonth = nextMonth;
window.openDateModal = openDateModal; window.closeModal = closeModal;
window.markPresent = markPresent; window.markAbsent = markAbsent;
window.markAllPresentToday = markAllPresentToday; window.clearTodayAttendance = clearTodayAttendance;
window.setAttendanceForDate = setAttendanceForDate; window.markAllForDate = markAllForDate;
window.saveAllGrades = saveAllGrades; window.generateReportCard = generateReportCard;

/* Fix: render functions collisions - reassign properly used above */
function renderGradesTable(){ renderGradesTableImpl(); }
function renderFailedList(){ renderFailedList(); }

/* Keep a few aliases consistent */
window.renderAttendanceGrid = renderAttendanceGrid;
window.renderMiniCalendar = renderMiniCalendar;

/* SMALL: after login, when teacher opens grades, user should hit Save */
</script>

</body>
</html>
