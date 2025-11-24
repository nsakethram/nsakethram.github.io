<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Sakethram's Hyper Habit & Task Console</title>
<style>
  :root{
    --bg1:#060616;
    --bg2:#020617;
    --muted:#9aa4b2;
    --text:#e6eef8;
    --accent:#9b5cff;
    --accent2:#06b6d4;
    --success:#34d399;
    --danger:#fb7185;
    --card: rgba(15,23,42,0.96);
    --glass-border: rgba(148,163,184,0.35);
    --radius:14px;
  }
  *{box-sizing:border-box}
  body{
    margin:0;
    font-family:Inter,system-ui,Arial,sans-serif;
    color:var(--text);
    background:
      radial-gradient(800px 300px at 6% 10%, rgba(155,92,255,0.09), transparent 40%),
      radial-gradient(600px 200px at 92% 90%, rgba(6,182,212,0.10), transparent 55%),
      linear-gradient(180deg,var(--bg1),var(--bg2));
    padding:26px;
  }

  .app-shell{
    max-width:1180px;
    margin:0 auto;
    display:flex;
    flex-direction:column;
    gap:18px;
  }

  .top {
    display:flex;
    justify-content:space-between;
    align-items:center;
    gap:12px;
    padding:12px 14px;
    border-radius:12px;
    background:linear-gradient(180deg, rgba(15,23,42,0.85), rgba(15,23,42,0.95));
    border:1px solid var(--glass-border);
    box-shadow:0 16px 40px rgba(0,0,0,0.6);
  }
  .brand { display:flex; gap:12px; align-items:center }
  .orb{
    width:44px;height:44px;border-radius:999px;
    background:conic-gradient(#ffd166,#9b5cff,#06b6d4,#fb7185,#ffd166);
    box-shadow:0 6px 26px rgba(155,92,255,0.35);
  }
  .title{font-weight:700;font-size:18px}
  .subtitle{font-size:12px;color:var(--muted)}
  .controls{display:flex;gap:8px;align-items:center;flex-wrap:wrap}

  .grid{display:grid;grid-template-columns:1fr 360px;gap:18px}
  @media (max-width:980px){.grid{grid-template-columns:1fr}}

  .card{
    border-radius:var(--radius);
    padding:16px;
    background:var(--card);
    border:1px solid var(--glass-border);
    box-shadow:0 16px 40px rgba(0,0,0,0.75);
  }

  .small{font-size:13px;color:var(--muted)}

  button.primary{
    background:linear-gradient(90deg,var(--accent),#ec4899);
    color:#fff;
    padding:8px 12px;
    border-radius:999px;
    border:none;
    cursor:pointer;
    font-weight:700;
    font-size:13px;
  }
  button.ghost{
    background:rgba(15,23,42,0.9);
    border:1px solid var(--glass-border);
    color:var(--muted);
    padding:8px 10px;
    border-radius:10px;
    cursor:pointer;
    font-size:13px;
  }

  input[type="date"]{
    padding:8px 10px;
    border-radius:8px;
    background:#020617;
    border:1px solid rgba(148,163,184,0.45);
    color:var(--text);
    font-size:13px;
  }

  /* Habit layout */
  .habit-controls{
    display:flex;
    gap:8px;
    align-items:center;
    margin-bottom:12px;
    flex-wrap:wrap;
  }
  .habit-head{
    display:grid;
    grid-template-columns: 1fr repeat(7,56px);
    gap:8px;
    align-items:center;
    margin-bottom:6px;
    color:var(--muted);
    font-size:13px;
  }
  .dates-row{
    display:grid;
    grid-template-columns:1fr repeat(7,56px);
    gap:8px;
    margin-bottom:12px;
    align-items:center;
  }
  .date-box{
    text-align:center;
    padding:6px 4px;
    border-radius:8px;
    background:rgba(15,23,42,0.9);
    color:var(--accent2);
    font-size:12px;
    border:1px solid rgba(148,163,184,0.45);
  }

  .habit-rows{
    display:flex;
    flex-direction:column;
    gap:10px;
    max-height:420px;
    overflow:auto;
    padding-right:6px;
    margin-bottom:12px;
  }

  .habit-row{
    display:grid;
    grid-template-columns:1fr repeat(7,56px);
    gap:8px;
    align-items:center;
    padding:10px;
    border-radius:10px;
    background:linear-gradient(180deg, rgba(30,64,175,0.38), rgba(15,23,42,0.98));
    border:1px solid rgba(148,163,184,0.6);
  }

  input.habit-title{
    padding:10px 12px;
    border-radius:10px;
    background:#020617;
    border:1px solid rgba(148,163,184,0.5);
    color:var(--text);
    width:100%;
    font-size:14px;
  }

  .cell-toggle{
    width:56px;
    height:44px;
    border-radius:10px;
    display:flex;
    align-items:center;
    justify-content:center;
    cursor:pointer;
    border:1px solid rgba(148,163,184,0.8);
    background:#020617;
    color:var(--muted);
    font-size:18px;
    transition:all .12s;
    box-shadow:0 4px 10px rgba(0,0,0,0.6);
  }
  .cell-toggle:hover{
    transform:translateY(-2px);
    box-shadow:0 10px 22px rgba(0,0,0,0.8);
  }
  .cell-toggle.done{
    background:linear-gradient(180deg, rgba(34,197,94,0.5), rgba(21,128,61,0.96));
    color:#ecfdf5;
    border-color:rgba(34,197,94,0.95);
  }
  .cell-toggle.not-done{
    background:linear-gradient(180deg, rgba(248,113,113,0.5), rgba(127,29,29,0.98));
    color:#fef2f2;
    border-color:rgba(248,113,113,0.9);
  }

  .habit-actions{
    display:flex;
    gap:8px;
    align-items:center;
    justify-content:space-between;
    flex-wrap:wrap;
  }

  /* Task layout */
  .tasks-head{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-bottom:10px;
    gap:12px;
    flex-wrap:wrap;
  }
  .task-rows{
    display:flex;
    flex-direction:column;
    gap:10px;
    max-height:340px;
    overflow:auto;
  }
  .task-row{
    display:grid;
    grid-template-columns:28px 1fr 120px 90px;
    gap:8px;
    align-items:center;
    padding:8px;
    border-radius:10px;
    background:linear-gradient(180deg, rgba(30,64,175,0.22), rgba(15,23,42,0.98));
    border:1px solid rgba(148,163,184,0.5);
  }

  .task-row input,
  .task-row select{
    padding:8px;
    border-radius:8px;
    background:#020617;
    border:1px solid rgba(148,163,184,0.7);
    color:#e5e7eb;
    font-size:13px;
  }

  .task-row select{
    appearance:auto;
    -webkit-appearance:menulist;
    -moz-appearance:menulist;
    font-size:14px;
  }

  /* right column */
  .summary .ring{
    width:92px;
    height:92px;
    border-radius:999px;
    background:linear-gradient(180deg, rgba(148,163,184,0.25), rgba(15,23,42,0.98));
    border:1px solid rgba(148,163,184,0.7);
    position:relative;
    display:grid;
    place-items:center;
    overflow:hidden;
  }
  .ring .fill{
    position:absolute;
    left:0;
    bottom:0;
    width:100%;
    height:30%;
    background:linear-gradient(90deg,var(--success),var(--accent2));
    transition:height .5s ease;
    z-index:0;
  }
  .ring span{z-index:1;font-weight:700}

  @media (max-width:720px){
    .habit-head,.dates-row,.habit-row{grid-template-columns:1fr repeat(7,44px)}
    .cell-toggle{width:44px}
    .task-row{grid-template-columns:24px 1fr}
    .task-row input[type="date"],.task-row select{margin-top:4px}
  }

  /* modal */
  .modal{
    position:fixed;
    inset:0;
    display:none;
    align-items:center;
    justify-content:center;
    background:rgba(0,0,0,0.6);
    z-index:2000;
  }
  .modal .panel{
    background:linear-gradient(180deg, rgba(15,23,42,0.96), rgba(15,23,42,0.98));
    padding:18px;
    border-radius:12px;
    width:90%;
    max-width:720px;
    border:1px solid var(--glass-border);
  }

</style>
</head>
<body>
<div class="app-shell">
  <div class="top">
    <div class="brand">
      <div class="orb" aria-hidden="true"></div>
      <div>
        <div class="title">Sakethram's Hyper Habit & Task Console</div>
        <div class="subtitle">Daily wins • Weekly rhythm • Import & Export tools</div>
      </div>
    </div>

    <div class="controls">
      <button class="primary" id="downloadWeekBtn">⬇ Download week</button>
      <button class="ghost" id="exportCSVWeek">Export week CSV</button>
      <label class="ghost" style="display:inline-flex;align-items:center;gap:6px;cursor:pointer">
        ⬆ Upload JSON
        <input type="file" id="uploadJSON" accept="application/json" style="display:none">
      </label>
    </div>
  </div>

  <div class="grid">
    <!-- LEFT: habit + tasks -->
    <div>
      <div class="card">
        <h3 style="margin:0 0 6px 0">Habit Tracker</h3>
        <div class="small">Pick a week — the weekday headers and the ISO dates will update.</div>

        <div class="habit-controls" style="margin-top:12px">
          <label class="small">Week of</label>
          <input type="date" id="weekOf">
          <button class="ghost" id="clearHabitsBtn">Clear marks</button>
          <button class="ghost" id="saveWeekBtn">Save week</button>
        </div>

        <div class="habit-head">
          <div>Habit</div>
          <div class="dayLabel">Mon</div>
          <div class="dayLabel">Tue</div>
          <div class="dayLabel">Wed</div>
          <div class="dayLabel">Thu</div>
          <div class="dayLabel">Fri</div>
          <div class="dayLabel">Sat</div>
          <div class="dayLabel">Sun</div>
        </div>

        <div class="dates-row" id="habit-date-row">
          <div>Dates</div>
          <div class="date-box" data-pos="0">-</div>
          <div class="date-box" data-pos="1">-</div>
          <div class="date-box" data-pos="2">-</div>
          <div class="date-box" data-pos="3">-</div>
          <div class="date-box" data-pos="4">-</div>
          <div class="date-box" data-pos="5">-</div>
          <div class="date-box" data-pos="6">-</div>
        </div>

        <div class="habit-rows" id="habit-rows" aria-live="polite"></div>

        <div class="habit-actions">
          <div style="display:flex;gap:8px">
            <button class="ghost" id="addHabitBtn">+ Add habit</button>
            <button class="ghost" id="removeHabitBtn">- Remove habit</button>
          </div>
          <button class="primary" id="downloadWeekDataTop">Download week data</button>
        </div>
      </div>

      <div style="height:14px"></div>

      <div class="card">
        <h3 style="margin:0 0 6px 0">Task Tracker</h3>
        <div class="small">Tasks saved per date. Click a task checkbox to cycle status.</div>

        <div style="display:flex;gap:8px;align-items:center;margin-top:10px;margin-bottom:8px;flex-wrap:wrap">
          <label class="small">Day</label>
          <input type="date" id="taskDayPicker">
          <button class="ghost" id="clearTasksBtn">Clear day</button>
          <button class="ghost" id="saveDayBtn">Save day</button>
        </div>

        <div class="tasks-head">
          <div class="small">Tasks</div>
          <div style="display:flex;gap:8px">
            <button class="ghost" id="addTaskBtn">+ Add task</button>
            <button class="ghost" id="removeTaskBtn">- Remove task</button>
          </div>
        </div>

        <div class="task-rows" id="task-rows" aria-live="polite"></div>
      </div>
    </div>

    <!-- RIGHT: summary -->
    <aside class="card summary">
      <h3 style="margin:0 0 6px 0">Weekly Summary</h3>
      <div style="display:flex;gap:12px;align-items:center;margin-bottom:12px">
        <div class="ring">
          <div class="fill" id="todayFill" style="height:30%"></div>
          <span id="todayPct">30%</span>
        </div>
        <div style="flex:1">
          <div class="small">Today progress</div>
          <div style="height:10px;background:#020617;border-radius:8px;margin-top:10px;border:1px solid rgba(148,163,184,0.4)">
            <div id="todayBar" style="width:30%;height:10px;background:linear-gradient(90deg,var(--success),var(--accent2));border-radius:8px"></div>
          </div>
          <div style="margin-top:8px;font-weight:700" id="todayDoneSmall">0/0</div>
        </div>
      </div>

      <div style="border-top:1px dashed rgba(148,163,184,0.4);padding-top:12px;margin-top:6px">
        <div class="small">Weekly completion</div>
        <div style="font-weight:700;margin-top:6px" id="weeklyPct">0%</div>
      </div>

      <div style="height:12px"></div>

      <div style="margin-top:auto;display:flex;gap:8px;align-items:center;justify-content:space-between">
        <button class="primary" id="openSummary">Open report</button>
        <button class="ghost" id="restoreBtn">Restore</button>
      </div>
    </aside>
  </div>
</div>

<!-- modal -->
<div class="modal" id="modal">
  <div class="panel">
    <div id="modalContent">...</div>
    <div style="text-align:right;margin-top:12px">
      <button class="ghost" id="closeModal">Close</button>
    </div>
  </div>
</div>

<!-- confetti -->
<canvas id="confetti" style="position:fixed;inset:0;pointer-events:none"></canvas>

<script>
const $ = s => document.querySelector(s);
const $$ = s => Array.from(document.querySelectorAll(s));

const weekInput = $('#weekOf');
const habitRows = $('#habit-rows');
const dateBoxes = $$('#habit-date-row .date-box');
const dayLabels = $$('.dayLabel');

const addHabitBtn = $('#addHabitBtn'), removeHabitBtn = $('#removeHabitBtn');
const saveWeekBtn = $('#saveWeekBtn'), clearHabitsBtn = $('#clearHabitsBtn');
const downloadWeekBtn = $('#downloadWeekBtn'), downloadWeekDataTop = $('#downloadWeekDataTop');
const uploadJSON = $('#uploadJSON'), restoreBtn = $('#restoreBtn'), exportCSVWeek = $('#exportCSVWeek');

const taskPicker = $('#taskDayPicker'), addTaskBtn = $('#addTaskBtn'), removeTaskBtn = $('#removeTaskBtn');
const saveDayBtn = $('#saveDayBtn'), clearTasksBtn = $('#clearTasksBtn');
const taskRows = $('#task-rows');

const todayFill = $('#todayFill'), todayPct = $('#todayPct'), todayBar = $('#todayBar'), todayDoneSmall = $('#todayDoneSmall');
const weeklyPct = $('#weeklyPct');

const modal = $('#modal'), modalContent = $('#modalContent'), openSummary = $('#openSummary'), closeModal = $('#closeModal');
const confettiCanvas = document.getElementById('confetti');
confettiCanvas.width = innerWidth; confettiCanvas.height = innerHeight;
window.addEventListener('resize', ()=>{confettiCanvas.width=innerWidth;confettiCanvas.height=innerHeight;});

function iso(d){return d.toISOString().slice(0,10);}
function weekdayShort(d){return ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'][d.getDay()];}
function toast(txt){
  const el=document.createElement('div');
  el.textContent=txt;
  Object.assign(el.style,{
    position:'fixed',right:'18px',bottom:'18px',
    padding:'10px 14px',
    background:'linear-gradient(90deg,#9b5cff,#ec4899)',
    color:'#fff',borderRadius:'10px',zIndex:9999,
    fontSize:'13px'
  });
  document.body.appendChild(el);
  setTimeout(()=>el.style.opacity=0,2000);
  setTimeout(()=>el.remove(),2600);
}

/* habit row */
function createHabitRow(name=''){
  const wrapper=document.createElement('div');
  wrapper.className='habit-row';
  const title=document.createElement('input');
  title.className='habit-title';
  title.placeholder='Habit title';
  title.value=name;
  wrapper.appendChild(title);
  for(let i=0;i<7;i++){
    const btn=document.createElement('button');
    btn.className='cell-toggle';
    btn.type='button';
    btn.dataset.dayIndex=i;
    btn.textContent='';
    btn.addEventListener('click',()=>{
      if(btn.textContent===''){btn.textContent='✅';btn.classList.add('done');btn.classList.remove('not-done');}
      else if(btn.textContent==='✅'){btn.textContent='❌';btn.classList.remove('done');btn.classList.add('not-done');}
      else{btn.textContent='';btn.classList.remove('done','not-done');}
      updateProgress();
    });
    wrapper.appendChild(btn);
  }
  return wrapper;
}

/* task row */
function createTaskRow(data){
  const wrapper=document.createElement('div');
  wrapper.className='task-row';
  wrapper.innerHTML=`
    <div style="text-align:center;color:var(--muted);cursor:pointer">☐</div>
    <input placeholder="Task">
    <input type="date">
    <select><option>High</option><option>Med</option><option>Low</option></select>`;
  const icon=wrapper.children[0];
  const taskInput=wrapper.children[1];
  const dateInput=wrapper.children[2];
  const priority=wrapper.children[3];

  if(data){
    taskInput.value=data.task||'';
    dateInput.value=data.date||'';
    priority.value=data.priority||'Med';
    icon.textContent=data.doneState||'☐';
    icon.style.color=(icon.textContent==='✅')?'var(--success)':(icon.textContent==='❌')?'var(--danger)':'var(--muted)';
  }

  icon.addEventListener('click',()=>{
    if(icon.textContent==='☐'){icon.textContent='✅';icon.style.color='var(--success)';}
    else if(icon.textContent==='✅'){icon.textContent='❌';icon.style.color='var(--danger)';}
    else{icon.textContent='☐';icon.style.color='var(--muted)';}
    updateProgress();
  });
  return wrapper;
}

/* date headers */
function updateWeekHeaders(){
  if(!weekInput.value){
    const defaultLabels=['Mon','Tue','Wed','Thu','Fri','Sat','Sun'];
    dayLabels.forEach((el,idx)=>el.textContent=defaultLabels[idx]);
    dateBoxes.forEach(d=>d.textContent='-');
    return;
  }
  const base=new Date(weekInput.value);
  for(let i=0;i<7;i++){
    const d=new Date(base);
    d.setDate(base.getDate()+i);
    dayLabels[i].textContent=weekdayShort(d);
    dateBoxes[i].textContent=iso(d);
  }
}

/* storage keys */
function habitKey(week){return 'habitWeek__'+(week||weekInput.value||iso(new Date()));}
function taskKey(day){return 'taskDay__'+(day||taskPicker.value||iso(new Date()));}

/* save / load habits */
function saveHabits(){
  const week=weekInput.value;
  if(!week){alert('Select a Week date first');return;}
  const rows=[];
  habitRows.querySelectorAll('.habit-row').forEach(r=>{
    const title=r.querySelector('.habit-title').value||'';
    const cells=Array.from(r.querySelectorAll('.cell-toggle')).map(c=>c.textContent||'');
    rows.push({title,cells});
  });
  localStorage.setItem(habitKey(week),JSON.stringify(rows));
  toast('Week saved');
  updateProgress();
}

function loadHabits(){
  habitRows.innerHTML='';
  const week=weekInput.value;
  updateWeekHeaders();
  if(!week){
    ['Workout','Reading','Meditate'].forEach(h=>habitRows.appendChild(createHabitRow(h)));
    updateProgress();return;
  }
  const raw=localStorage.getItem(habitKey(week));
  if(!raw){
    ['Workout','Reading','Meditate'].forEach(h=>habitRows.appendChild(createHabitRow(h)));
    updateProgress();return;
  }
  try{
    const parsed=JSON.parse(raw);
    parsed.forEach(r=>{
      const node=createHabitRow(r.title||'');
      const cells=node.querySelectorAll('.cell-toggle');
      (r.cells||[]).forEach((val,idx)=>{
        if(!cells[idx])return;
        const v=String(val||'');
        cells[idx].textContent=v;
        cells[idx].classList.toggle('done',v==='✅');
        cells[idx].classList.toggle('not-done',v==='❌');
      });
      habitRows.appendChild(node);
    });
  }catch(e){
    console.error('invalid habit data',e);
    ['Workout','Reading','Meditate'].forEach(h=>habitRows.appendChild(createHabitRow(h)));
  }
  updateProgress();
}

/* save / load tasks */
function saveTasksForDay(){
  const day=taskPicker.value;
  if(!day){alert('Pick a day');return;}
  const list=[];
  taskRows.querySelectorAll('.task-row').forEach(r=>{
    const icon=r.children[0].textContent||'☐';
    const task=r.children[1].value||'';
    const date=r.children[2].value||'';
    const pr=r.children[3].value||'Med';
    list.push({task,date,priority:pr,doneState:icon});
  });
  localStorage.setItem(taskKey(day),JSON.stringify(list));
  toast('Day saved');
  updateProgress();
}

function loadTasksForDay(){
  taskRows.innerHTML='';
  const day=taskPicker.value;
  if(!day){taskRows.appendChild(createTaskRow());updateProgress();return;}
  const raw=localStorage.getItem(taskKey(day));
  if(!raw){taskRows.appendChild(createTaskRow());updateProgress();return;}
  try{
    const parsed=JSON.parse(raw);
    parsed.forEach(t=>taskRows.appendChild(createTaskRow(t)));
  }catch(e){
    console.error('invalid task data',e);
    taskRows.appendChild(createTaskRow());
  }
  updateProgress();
}

/* week export bundle */
function buildWeekBundle(){
  const week=weekInput.value||iso(new Date());
  const base=new Date(week);
  const weekDates=Array.from({length:7},(_,i)=>{const d=new Date(base);d.setDate(base.getDate()+i);return iso(d);});
  const habitData=JSON.parse(localStorage.getItem(habitKey(week))||'[]');
  const tasksByDay={};
  weekDates.forEach(dt=>tasksByDay[dt]=JSON.parse(localStorage.getItem(taskKey(dt))||'[]'));
  return {weekOf:week,weekDates,habits:habitData,tasksByDay};
}

function downloadWeekBundle(){
  const bundle=buildWeekBundle();
  const week=bundle.weekOf;
  const blob=new Blob([JSON.stringify(bundle,null,2)],{type:'application/json'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download=`habit_task_week_${week}.json`;
  document.body.appendChild(a);a.click();a.remove();
}

/* CSV export */
function exportWeekCSV(){
  const {weekOf,weekDates,habits,tasksByDay}=buildWeekBundle();
  let rows=[];
  rows.push(['Habit'].concat(weekDates).join(','));
  habits.forEach(h=>{
    const row=[`"${(h.title||'').replace(/"/g,'""')}"`].concat((h.cells||[]).map(c=>`"${(c||'')}"`));
    rows.push(row.join(','));
  });
  rows.push('');
  rows.push('Date,Task,Priority,Status');
  weekDates.forEach(dt=>{
    (tasksByDay[dt]||[]).forEach(t=>{
      rows.push([
        dt,
        `"${(t.task||'').replace(/"/g,'""')}"`,
        t.priority||'',
        `"${(t.doneState||'')}"`,
      ].join(','));
    });
  });
  const blob=new Blob([rows.join('\n')],{type:'text/csv'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download=`habit_week_${weekOf}.csv`;
  document.body.appendChild(a);a.click();a.remove();
}

/* upload / restore */
uploadJSON.addEventListener('change',e=>{
  const f=e.target.files[0];if(!f)return;
  const r=new FileReader();
  r.onload=ev=>{
    try{
      const json=JSON.parse(ev.target.result);
      if(json.weekOf && Array.isArray(json.habits)){
        localStorage.setItem(habitKey(json.weekOf),JSON.stringify(json.habits));
        if(json.tasksByDay&&typeof json.tasksByDay==='object'){
          Object.entries(json.tasksByDay).forEach(([date,arr])=>{
            localStorage.setItem(taskKey(date),JSON.stringify(arr));
          });
        }
        weekInput.value=json.weekOf;
        updateWeekHeaders();
        loadHabits();
        toast('Week restored');
      }else toast('JSON format not recognized');
    }catch(err){toast('Invalid JSON');console.error(err);}
  };
  r.readAsText(f);
  e.target.value='';
});

/* progress + confetti */
function updateProgress(){
  const today=iso(new Date());
  const tasksToday=JSON.parse(localStorage.getItem(taskKey(today))||'[]');
  const totalTasks=tasksToday.length;
  const doneTasks=tasksToday.filter(t=>t.doneState==='✅').length;
  const pct=totalTasks?Math.round(doneTasks/totalTasks*100):0;
  todayFill.style.height=pct+'%';
  todayPct.textContent=pct+'%';
  todayBar.style.width=pct+'%';
  todayDoneSmall.textContent=`${doneTasks}/${totalTasks}`;

  const week=weekInput.value||iso(new Date());
  const habits=JSON.parse(localStorage.getItem(habitKey(week))||'[]');
  let checks=0,total=0;
  habits.forEach(h=>(h.cells||[]).forEach(c=>{if(c!=='')total++;if(c==='✅')checks++;}));
  const wPct=total?Math.round(checks/total*100):0;
  weeklyPct.textContent=wPct+'%';

  if(pct===100 && totalTasks>0)launchConfetti();
}

function launchConfetti(){
  const ctx=confettiCanvas.getContext('2d');
  const W=confettiCanvas.width,H=confettiCanvas.height;
  const pieces=Array.from({length:80}).map(()=>({
    x:Math.random()*W,y:-20-Math.random()*H*0.3,
    vx:(Math.random()-0.5)*6,vy:2+Math.random()*4,
    rot:Math.random()*360,vr:(Math.random()-0.5)*12,
    size:6+Math.random()*8,
    color:['#ffd166','#06b6d4','#9b5cff','#fb7185'][Math.floor(Math.random()*4)]
  }));
  let t=0;
  const id=setInterval(()=>{
    t++;ctx.clearRect(0,0,W,H);
    pieces.forEach(p=>{
      p.x+=p.vx;p.y+=p.vy;p.vy+=0.06;p.rot+=p.vr;
      ctx.save();ctx.translate(p.x,p.y);ctx.rotate(p.rot*Math.PI/180);
      ctx.fillStyle=p.color;ctx.fillRect(-p.size/2,-p.size/2,p.size,p.size*0.6);ctx.restore();
    });
    if(t>100){clearInterval(id);setTimeout(()=>ctx.clearRect(0,0,W,H),200);}
  },16);
}

/* summary modal */
openSummary.addEventListener('click',()=>{
  modal.style.display='flex';
  const {weekOf,weekDates,habits,tasksByDay}=buildWeekBundle();
  let habitChecks=0,habitTotal=0;
  habits.forEach(h=>(h.cells||[]).forEach(c=>{if(c!=='')habitTotal++;if(c==='✅')habitChecks++;}));
  let taskDone=0,taskTotal=0;
  weekDates.forEach(d=>{
    (tasksByDay[d]||[]).forEach(t=>{taskTotal++;if(t.doneState==='✅')taskDone++;});
  });
  modalContent.innerHTML=`
    <h3 style="margin:0 0 8px 0">Report — Week of ${weekOf}</h3>
    <div class="small">Habits: ${habitChecks}/${habitTotal} checks</div>
    <div class="small">Tasks: ${taskDone}/${taskTotal} done</div>
    <div style="margin-top:10px">
      <strong>Per-day task counts</strong>
      <ul>${weekDates.map(d=>`<li>${d} — ${(tasksByDay[d]||[]).length} tasks</li>`).join('')}</ul>
    </div>`;
});
closeModal.addEventListener('click',()=>modal.style.display='none');

/* wiring */
addHabitBtn.addEventListener('click',()=>{habitRows.appendChild(createHabitRow(''));updateProgress();});
removeHabitBtn.addEventListener('click',()=>{if(habitRows.lastChild)habitRows.lastChild.remove();updateProgress();});
saveWeekBtn.addEventListener('click',saveHabits);
clearHabitsBtn.addEventListener('click',()=>{
  habitRows.querySelectorAll('.cell-toggle').forEach(c=>{c.textContent='';c.classList.remove('done','not-done');});
  updateProgress();
});

addTaskBtn.addEventListener('click',()=>{taskRows.appendChild(createTaskRow());updateProgress();});
removeTaskBtn.addEventListener('click',()=>{if(taskRows.lastChild)taskRows.lastChild.remove();updateProgress();});
saveDayBtn.addEventListener('click',saveTasksForDay);
clearTasksBtn.addEventListener('click',()=>{taskRows.innerHTML='';updateProgress();});

downloadWeekBtn.addEventListener('click',downloadWeekBundle);
downloadWeekDataTop.addEventListener('click',downloadWeekBundle);
exportCSVWeek.addEventListener('click',exportWeekCSV);
restoreBtn.addEventListener('click',()=>uploadJSON.click());

weekInput.addEventListener('change',()=>{updateWeekHeaders();loadHabits();});
taskPicker.addEventListener('change',()=>{loadTasksForDay();});

/* init */
window.addEventListener('load',()=>{
  const today=new Date();const t=iso(today);
  if(!weekInput.value)weekInput.value=t;
  if(!taskPicker.value)taskPicker.value=t;
  habitRows.innerHTML='';
  ['Workout','Reading','Meditate'].forEach(h=>habitRows.appendChild(createHabitRow(h)));
  taskRows.innerHTML='';taskRows.appendChild(createTaskRow());
  updateWeekHeaders();
  loadHabits();
  loadTasksForDay();
  updateProgress();
});

/* shortcuts */
document.addEventListener('keydown',e=>{
  if(['INPUT','TEXTAREA'].includes(document.activeElement.tagName))return;
  if(e.key.toLowerCase()==='s'){e.preventDefault();saveTasksForDay();}
  if(e.key.toLowerCase()==='w'){e.preventDefault();saveHabits();}
});
</script>
</body>
</html>
