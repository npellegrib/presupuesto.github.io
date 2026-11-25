<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Mi Presupuesto">
<title>Mi Presupuesto</title>
<style>
:root {
  --bg: #0f1117;
  --card: #1a1d27;
  --card2: #232734;
  --accent: #4f8cff;
  --green: #34d399;
  --yellow: #fbbf24;
  --red: #f87171;
  --text: #e8eaf0;
  --text2: #9ca3b8;
  --radius: 14px;
  --shadow: 0 2px 12px rgba(0,0,0,.3);
}
* { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background: var(--bg); color: var(--text);
  max-width: 480px; margin: 0 auto;
  min-height: 100vh; padding-bottom: 80px;
}
/* ── Top bar ── */
.topbar {
  position: sticky; top: 0; z-index: 100;
  background: var(--bg); padding: 16px 18px 10px;
  display: flex; align-items: center; justify-content: space-between;
}
.topbar h1 { font-size: 20px; font-weight: 700; }
.month-nav { display: flex; align-items: center; gap: 10px; font-size: 15px; color: var(--text2); }
.month-nav button {
  background: var(--card2); border: none; color: var(--text);
  width: 32px; height: 32px; border-radius: 50%; font-size: 18px;
  cursor: pointer; display: grid; place-items: center;
}
.month-nav button:active { transform: scale(.9); }

/* ── Tabs (bottom bar) ── */
.tabbar {
  position: fixed; bottom: 0; left: 0; right: 0; z-index: 100;
  background: var(--card); border-top: 1px solid #2a2e3d;
  display: flex; justify-content: space-around; padding: 8px 0 26px;
}
.tab {
  display: flex; flex-direction: column; align-items: center; gap: 2px;
  font-size: 10px; color: var(--text2); cursor: pointer; padding: 4px 12px;
  transition: color .2s;
}
.tab.active { color: var(--accent); }
.tab .ic { font-size: 22px; }

/* ── Cards ── */
.page { display: none; padding: 4px 14px; }
.page.active { display: block; }
.card {
  background: var(--card); border-radius: var(--radius);
  padding: 16px; margin-bottom: 12px; box-shadow: var(--shadow);
}
.card h3 { font-size: 13px; color: var(--text2); text-transform: uppercase; letter-spacing: .5px; margin-bottom: 10px; }

/* ── Big numbers ── */
.big-num { font-size: 32px; font-weight: 800; }
.big-num.green { color: var(--green); }
.big-num.red { color: var(--red); }
.sub { font-size: 13px; color: var(--text2); margin-top: 2px; }

/* ── Progress bar ── */
.bar-wrap { height: 10px; background: var(--card2); border-radius: 6px; margin-top: 8px; overflow: hidden; }
.bar-fill { height: 100%; border-radius: 6px; transition: width .4s; }

/* ── Category row ── */
.cat-row {
  display: flex; align-items: center; gap: 12px;
  padding: 12px 0; border-bottom: 1px solid #2a2e3d;
}
.cat-row:last-child { border-bottom: none; }
.cat-icon {
  width: 38px; height: 38px; border-radius: 10px;
  display: grid; place-items: center; font-size: 18px; flex-shrink: 0;
}
.cat-info { flex: 1; min-width: 0; }
.cat-name { font-size: 15px; font-weight: 600; }
.cat-limit { font-size: 12px; color: var(--text2); }
.cat-amt { font-size: 15px; font-weight: 700; text-align: right; }
.cat-remain { font-size: 11px; color: var(--text2); text-align: right; }

/* ── Buttons ── */
.btn {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 10px 18px; border-radius: 10px; border: none;
  font-size: 14px; font-weight: 600; cursor: pointer;
  background: var(--accent); color: #fff;
}
.btn:active { transform: scale(.96); }
.btn-sm { padding: 6px 12px; font-size: 12px; border-radius: 8px; }
.btn-outline { background: transparent; border: 1.5px solid var(--card2); color: var(--text); }
.btn-danger { background: var(--red); }
.btn-green { background: var(--green); }

/* ── Alert banner ── */
.alert-banner {
  background: rgba(248,113,113,.15); border: 1px solid rgba(248,113,113,.3);
  border-radius: 10px; padding: 12px 14px; margin-bottom: 12px;
  font-size: 13px; color: var(--red); display: flex; align-items: center; gap: 8px;
}
.alert-banner.warn { background: rgba(251,191,36,.12); border-color: rgba(251,191,36,.3); color: var(--yellow); }

/* ── Modal ── */
.modal-bg {
  display: none; position: fixed; inset: 0; z-index: 200;
  background: rgba(0,0,0,.6); backdrop-filter: blur(4px);
  align-items: flex-end; justify-content: center;
}
.modal-bg.open { display: flex; }
.modal {
  background: var(--card); width: 100%; max-width: 480px;
  border-radius: 20px 20px 0 0; padding: 24px 20px 36px;
  animation: slideUp .25s ease;
}
@keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
.modal h2 { font-size: 18px; margin-bottom: 16px; }
.field { margin-bottom: 14px; }
.field label { font-size: 13px; color: var(--text2); display: block; margin-bottom: 5px; }
.field input, .field select, .field textarea {
  width: 100%; padding: 12px; border-radius: 10px; border: 1.5px solid var(--card2);
  background: var(--card2); color: var(--text); font-size: 16px; outline: none;
}
.field input:focus, .field select:focus { border-color: var(--accent); }
.modal-actions { display: flex; gap: 10px; margin-top: 8px; }
.modal-actions .btn { flex: 1; justify-content: center; }

/* ── Transaction list ── */
.txn {
  display: flex; align-items: center; gap: 12px;
  padding: 10px 0; border-bottom: 1px solid #2a2e3d;
}
.txn:last-child { border: none; }
.txn-cat { font-size: 12px; padding: 2px 8px; border-radius: 6px; background: var(--card2); color: var(--text2); }
.txn-amt { font-weight: 700; margin-left: auto; }
.txn-amt.exp { color: var(--red); }
.txn-amt.inc { color: var(--green); }
.txn-date { font-size: 11px; color: var(--text2); }

/* ── Misc ── */
.flex { display: flex; align-items: center; gap: 10px; }
.flex-between { display: flex; justify-content: space-between; align-items: center; }
.mt { margin-top: 12px; }
.mb { margin-bottom: 12px; }
.center { text-align: center; }
.fade { opacity: .5; }
.trip-badge {
  display: inline-block; font-size: 11px; background: var(--accent);
  color: #fff; padding: 2px 8px; border-radius: 6px; margin-left: 6px;
}
.section-label {
  font-size: 11px; text-transform: uppercase; letter-spacing: 1px;
  color: var(--text2); margin: 18px 0 8px; font-weight: 700;
}
.empty { text-align: center; padding: 40px 20px; color: var(--text2); font-size: 14px; }
.toggle-row { display: flex; justify-content: space-between; align-items: center; padding: 12px 0; }
.toggle {
  width: 48px; height: 28px; background: var(--card2); border-radius: 14px;
  position: relative; cursor: pointer; transition: background .2s;
}
.toggle.on { background: var(--accent); }
.toggle::after {
  content: ''; width: 22px; height: 22px; background: #fff;
  border-radius: 50%; position: absolute; top: 3px; left: 3px;
  transition: left .2s;
}
.toggle.on::after { left: 23px; }
.fab {
  position: fixed; bottom: 90px; right: 20px; z-index: 50;
  width: 56px; height: 56px; border-radius: 50%;
  background: var(--accent); color: #fff; border: none;
  font-size: 28px; display: grid; place-items: center;
  box-shadow: 0 4px 16px rgba(79,140,255,.4); cursor: pointer;
}
.fab:active { transform: scale(.92); }
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
  <h1>💰 Mi Presupuesto</h1>
  <div class="month-nav">
    <button onclick="changeMonth(-1)">‹</button>
    <span id="monthLabel"></span>
    <button onclick="changeMonth(1)">›</button>
  </div>
</div>

<!-- ─── PAGE: DASHBOARD ─── -->
<div class="page active" id="page-dash">
  <div id="alertArea"></div>
  <div class="card center">
    <h3>Ingreso del mes</h3>
    <div class="big-num" id="incomeDisplay">0</div>
    <div class="sub" id="incomeSub"></div>
    <div class="mt flex-between">
      <div>
        <div class="big-num green" id="savedDisplay" style="font-size:26px">0</div>
        <div class="sub">Ahorro estimado</div>
      </div>
      <div style="text-align:right">
        <div class="big-num" id="spentDisplay" style="font-size:26px;color:var(--text2)">0</div>
        <div class="sub">Gastado</div>
      </div>
    </div>
    <div class="bar-wrap mt" style="height:12px">
      <div class="bar-fill" id="mainBar" style="width:0%;background:var(--green)"></div>
    </div>
    <div class="sub mt" id="mainBarLabel"></div>
  </div>

  <div class="card">
    <h3>Resumen por categoría</h3>
    <div id="catSummary"></div>
  </div>
</div>

<!-- ─── PAGE: GASTOS ─── -->
<div class="page" id="page-txn">
  <div class="flex-between mb">
    <h3 style="font-size:16px">Movimientos</h3>
    <button class="btn btn-sm btn-outline" onclick="showFilter()">🔍 Filtro</button>
  </div>
  <div class="card" id="txnList"></div>
</div>

<!-- ─── PAGE: PRESUPUESTO ─── -->
<div class="page" id="page-budget">
  <div class="flex-between mb">
    <h3 style="font-size:16px">Presupuesto</h3>
    <button class="btn btn-sm" onclick="openCatModal()">+ Categoría</button>
  </div>
  <div class="card" id="budgetList"></div>
  <div class="card">
    <h3>Opciones</h3>
    <div class="toggle-row">
      <span>Modo Viaje España (−2.000 QAR)</span>
      <div class="toggle" id="tripToggle" onclick="toggleTrip()"></div>
    </div>
    <div class="field mt">
      <label>Umbral de alerta</label>
      <input type="range" id="alertSlider" min="50" max="100" value="80"
             oninput="document.getElementById('alertVal').textContent=this.value+'%';
                     state.settings.alertThreshold=+this.value/100; save();">
      <span id="alertVal" style="font-size:13px;color:var(--text2)">80%</span>
    </div>
  </div>
</div>

<!-- ─── PAGE: AJUSTES ─── -->
<div class="page" id="page-settings">
  <div class="card">
    <h3>Respaldo de datos</h3>
    <div class="flex" style="gap:8px;flex-wrap:wrap">
      <button class="btn btn-sm btn-outline" onclick="exportData()">📤 Exportar</button>
      <button class="btn btn-sm btn-outline" onclick="document.getElementById('importFile').click()">📥 Importar</button>
      <input type="file" id="importFile" accept=".json" style="display:none" onchange="importData(event)">
    </div>
    <p class="sub mt">Descarga un JSON y guárdalo. Así no pierdes nada.</p>
  </div>
  <div class="card">
    <h3>Instalar como app</h3>
    <p class="sub">En iPhone (Safari): toca el botón <b>Compartir</b> (cuadrado con flecha ↑) → <b>"Añadir a pantalla de inicio"</b>. Se abrirá como una app con icono.</p>
  </div>
  <div class="card">
    <h3>Peligro zona</h3>
    <button class="btn btn-sm btn-danger" onclick="if(confirm('¿Borrar TODOS los datos?')){localStorage.removeItem('mp_data');location.reload();}">🗑 Borrar todo</button>
  </div>
</div>

<!-- FAB -->
<button class="fab" onclick="openTxnModal()">+</button>

<!-- ─── MODAL: Transaction ─── -->
<div class="modal-bg" id="modalTxn">
  <div class="modal">
    <h2 id="modalTxnTitle">Nuevo gasto</h2>
    <div class="field">
      <label>Monto (QAR)</label>
      <input type="number" id="txAmount" placeholder="0.00" step="0.01" inputmode="decimal">
    </div>
    <div class="field">
      <label>Categoría</label>
      <select id="txCat"></select>
    </div>
    <div class="field">
      <label>Concepto (opcional)</label>
      <input type="text" id="txNote" placeholder="Ej: Cena con amigos">
    </div>
    <div class="field">
      <label>Fecha</label>
      <input type="date" id="txDate">
    </div>
    <div class="field">
      <label>Tipo</label>
      <select id="txType">
        <option value="expense">Gasto</option>
        <option value="income">Ingreso extra</option>
      </select>
    </div>
    <div class="modal-actions">
      <button class="btn btn-outline" onclick="closeModals()">Cancelar</button>
      <button class="btn btn-green" onclick="saveTxn()">Guardar</button>
    </div>
  </div>
</div>

<!-- ─── MODAL: Category ─── -->
<div class="modal-bg" id="modalCat">
  <div class="modal">
    <h2 id="modalCatTitle">Nueva categoría</h2>
    <div class="field">
      <label>Nombre</label>
      <input type="text" id="catName" placeholder="Ej: Películas">
    </div>
    <div class="field">
      <label>Límite mensual (QAR)</label>
      <input type="number" id="catLimit" placeholder="0" step="1" inputmode="decimal">
    </div>
    <div class="field">
      <label>Grupo</label>
      <select id="catGroup">
        <option value="Fijo">Fijo</option>
        <option value="Variable" selected>Variable</option>
        <option value="Apartado">Apartado</option>
        <option value="Otro">Otro</option>
      </select>
    </div>
    <div class="field">
      <label>Icono (emoji)</label>
      <input type="text" id="catIcon" value="📌" maxlength="4" style="width:80px;text-align:center;font-size:22px">
    </div>
    <div class="field">
      <label>Color</label>
      <input type="color" id="catColor" value="#4f8cff" style="width:60px;height:44px;padding:2px">
    </div>
    <div class="modal-actions">
      <button class="btn btn-outline" onclick="closeModals()">Cancelar</button>
      <button class="btn" onclick="saveCat()">Guardar</button>
    </div>
  </div>
</div>

<!-- ─── MODAL: Filter ─── -->
<div class="modal-bg" id="modalFilter">
  <div class="modal">
    <h2>Filtrar movimientos</h2>
    <div class="field">
      <label>Categoría</label>
      <select id="filterCat"><option value="">Todas</option></select>
    </div>
    <div class="field">
      <label>Tipo</label>
      <select id="filterType">
        <option value="">Todos</option>
        <option value="expense">Gastos</option>
        <option value="income">Ingresos</option>
      </select>
    </div>
    <div class="modal-actions">
      <button class="btn btn-outline" onclick="closeModals()">Cancelar</button>
      <button class="btn" onclick="applyFilter()">Aplicar</button>
    </div>
  </div>
</div>

<!-- ─── BOTTOM TABS ─── -->
<div class="tabbar">
  <div class="tab active" onclick="showPage('dash')"><span class="ic">🏠</span>Inicio</div>
  <div class="tab" onclick="showPage('txn')"><span class="ic">📋</span>Gastos</div>
  <div class="tab" onclick="showPage('budget')"><span class="ic">⚙️</span>Presupuesto</div>
  <div class="tab" onclick="showPage('settings')"><span class="ic">🔧</span>Ajustes</div>
</div>

<script>
// ═══════════════════════════════════════
//  STATE
// ═══════════════════════════════════════
const DEFAULT_STATE = {
  settings: {
    monthlyIncome: 23600,
    incomeDay: 25,
    alertThreshold: 0.80,
    tripMode: false,
    tripReduction: 2000,
    currency: 'QAR'
  },
  categories: [
    {id:'c1', name:'Arriendo Doha',   limit:3750, group:'Fijo',    icon:'🏠', color:'#f87171'},
    {id:'c2', name:'Datos',           limit:125,  group:'Fijo',    icon:'📡', color:'#a78bfa'},
    {id:'c3', name:'Comida',          limit:1400, group:'Variable', icon:'🍽️', color:'#fbbf24'},
    {id:'c4', name:'Transporte',      limit:400,  group:'Variable', icon:'🚌', color:'#34d399'},
    {id:'c5', name:'Salidas / Eventos',limit:600, group:'Variable', icon:'🎉', color:'#f472b6'},
    {id:'c6', name:'Deporte',         limit:400,  group:'Variable', icon:'⚽', color:'#4f8cff'},
    {id:'c7', name:'Imprevistos',     limit:200,  group:'Variable', icon:'🩹', color:'#fbbf24'},
    {id:'c8', name:'Familia',         limit:1000, group:'Apartado', icon:'👨‍👩‍👧', color:'#34d399'},
    {id:'c9', name:'Bolsillo personal',limit:1500,group:'Apartado', icon:'👕', color:'#a78bfa'},
    {id:'c10',name:'Viaje España',    limit:2000, group:'Apartado', icon:'✈️', color:'#4f8cff'},
  ],
  transactions: [],
  activeMonth: new Date().toISOString().slice(0,7)
};

let state = load();
let editingCatId = null;
let currentFilter = {cat:'', type:''};

function load(){
  const d = localStorage.getItem('mp_data');
  if(d) return JSON.parse(d);
  return structuredClone(DEFAULT_STATE);
}
function save(){ localStorage.setItem('mp_data', JSON.stringify(state)); }

// ═══════════════════════════════════════
//  HELPERS
// ═══════════════════════════════════════
const $ = s => document.querySelector(s);
const fmt = n => new Intl.NumberFormat('es-ES',{minimumFractionDigits:0,maximumFractionDigits:2}).format(n);
const monthName = m => {
  const d = new Date(m+'-01');
  return d.toLocaleDateString('es-ES',{month:'long',year:'numeric'});
};
function catById(id){ return state.categories.find(c=>c.id===id); }
function txnsForMonth(m){
  return state.transactions.filter(t=>t.date.startsWith(m));
}
function spentForCat(catId, m){
  return txnsForMonth(m)
    .filter(t=>t.categoryId===catId && t.type==='expense')
    .reduce((s,t)=>s+t.amount, 0);
}
function totalSpent(m){
  return txnsForMonth(m).filter(t=>t.type==='expense').reduce((s,t)=>s+t.amount,0);
}
function totalIncome(m){
  let base = state.settings.monthlyIncome;
  if(state.settings.tripMode) base -= state.settings.tripReduction;
  const extras = txnsForMonth(m).filter(t=>t.type==='income').reduce((s,t)=>s+t.amount,0);
  return base + extras;
}
function totalBudget(m){
  return state.categories.reduce((s,c)=>s+c.limit,0);
}
function uid(){ return 'x'+Date.now()+Math.random().slice(2,7); }

// ═══════════════════════════════════════
//  NAVIGATION
// ═══════════════════════════════════════
function showPage(p){
  document.querySelectorAll('.page').forEach(el=>el.classList.remove('active'));
  document.getElementById('page-'+p).classList.add('active');
  document.querySelectorAll('.tab').forEach((t,i)=>{
    t.classList.toggle('active', ['dash','txn','budget','settings'][i]===p);
  });
  render();
}
function changeMonth(d){
  const [y,mo] = state.activeMonth.split('-').map(Number);
  const dt = new Date(y, mo-1+d, 1);
  state.activeMonth = dt.toISOString().slice(0,7);
  save(); render();
}

// ═══════════════════════════════════════
//  RENDER
// ═══════════════════════════════════════
function render(){
  const m = state.activeMonth;
  const income = totalIncome(m);
  const spent = totalSpent(m);
  const budget = totalBudget(m);
  const saved = income - budget;
  const pct = budget>0 ? Math.min(spent/budget*100,100) : 0;

  // Top bar
  $('#monthLabel').textContent = monthName(m);

  // Dashboard
  $('#incomeDisplay').textContent = fmt(income)+' '+state.settings.currency;
  $('#incomeSub').textContent = 'El día '+state.settings.incomeDay+' de cada mes' + (state.settings.tripMode?' · Modo viaje ✈️':'');
  $('#spentDisplay').textContent = fmt(spent)+' '+state.settings.currency;
  $('#savedDisplay').textContent = fmt(saved)+' '+state.settings.currency;
  $('#savedDisplay').style.color = saved>=0 ? 'var(--green)' : 'var(--red)';

  const bar = $('#mainBar');
  bar.style.width = pct+'%';
  bar.style.background = pct<70?'var(--green)':pct<90?'var(--yellow)':'var(--red)';
  $('#mainBarLabel').textContent = fmt(pct)+'% del presupuesto gastado';

  // Alerts
  renderAlerts();

  // Category summary on dashboard
  let catHTML = '';
  state.categories.forEach(c=>{
    const sp = spentForCat(c.id, m);
    const p = c.limit>0 ? Math.min(sp/c.limit*100,100) : 0;
    const col = p<70?'var(--green)':p<90?'var(--yellow)':'var(--red)';
    catHTML += `
      <div class="cat-row">
        <div class="cat-icon" style="background:${c.color}22;color:${c.color}">${c.icon}</div>
        <div class="cat-info">
          <div class="cat-name">${c.name}</div>
          <div class="cat-limit">Límite: ${fmt(c.limit)} ${state.settings.currency}</div>
        </div>
        <div>
          <div class="cat-amt" style="color:${col}">${fmt(sp)}</div>
          <div class="cat-remain">${fmt(c.limit-sp)} libre</div>
        </div>
      </div>
      <div class="bar-wrap"><div class="bar-fill" style="width:${p}%;background:${col}"></div></div>`;
  });
  $('#catSummary').innerHTML = catHTML || '<div class="empty">Sin categorías</div>';

  // Transactions page
  let filtered = txnsForMonth(m);
  if(currentFilter.cat) filtered = filtered.filter(t=>t.categoryId===currentFilter.cat);
  if(currentFilter.type) filtered = filtered.filter(t=>t.type===currentFilter.type);
  filtered.sort((a,b)=>b.date.localeCompare(a.date));

  let txHTML = '';
  if(!filtered.length){
    txHTML = '<div class="empty">📭 No hay movimientos este mes.<br>Toca el botón + para añadir.</div>';
  } else {
    filtered.forEach(t=>{
      const c = catById(t.categoryId);
      const dObj = new Date(t.date);
      const ds = dObj.toLocaleDateString('es-ES',{day:'numeric',month:'short'});
      txHTML += `
        <div class="txn">
          <div class="cat-icon" style="background:${c?c.color:'#555'}22;color:${c?c.color:'#555'};width:34px;height:34px;font-size:15px">${c?c.icon:'❓'}</div>
          <div class="cat-info">
            <div class="cat-name" style="font-size:14px">${c?c.name:'—'}</div>
            <div class="txn-date">${ds}${t.note?' · '+t.note:''}</div>
          </div>
          <div>
            <div class="txn-amt ${t.type==='income'?'inc':'exp'}">${t.type==='income'?'+':'−'}${fmt(t.amount)}</div>
            <button class="btn btn-sm btn-outline" style="padding:2px 8px;font-size:10px;margin-top:4px" onclick="delTxn('${t.id}')">✕</button>
          </div>
        </div>`;
    });
  }
  $('#txnList').innerHTML = txHTML;

  // Budget page
  const groups = ['Fijo','Variable','Apartado','Otro'];
  let bHTML = '';
  groups.forEach(g=>{
    const cats = state.categories.filter(c=>c.group===g);
    if(!cats.length) return;
    bHTML += `<div class="section-label">${g}</div>`;
    cats.forEach(c=>{
      const sp = spentForCat(c.id, m);
      bHTML += `
        <div class="cat-row">
          <div class="cat-icon" style="background:${c.color}22;color:${c.color}">${c.icon}</div>
          <div class="cat-info">
            <div class="cat-name">${c.name}</div>
            <div class="cat-limit">${fmt(sp)} / ${fmt(c.limit)} ${state.settings.currency}</div>
          </div>
          <div class="flex" style="gap:4px">
            <button class="btn btn-sm btn-outline" onclick="openCatModal('${c.id}')">✏️</button>
            <button class="btn btn-sm btn-outline" onclick="delCat('${c.id}')">🗑</button>
          </div>
        </div>`;
    });
  });
  $('#budgetList').innerHTML = bHTML || '<div class="empty">Añade categorías</div>';

  // Trip toggle
  $('#tripToggle').classList.toggle('on', state.settings.tripMode);

  // Filter select
  let fOpts = '<option value="">Todas</option>';
  state.categories.forEach(c=>{ fOpts += `<option value="${c.id}">${c.icon} ${c.name}</option>`; });
  $('#filterCat').innerHTML = fOpts;
  if(currentFilter.cat) $('#filterCat').value = currentFilter.cat;
}

function renderAlerts(){
  const m = state.activeMonth;
  const th = state.settings.alertThreshold;
  let alerts = [];
  state.categories.forEach(c=>{
    const sp = spentForCat(c.id, m);
    if(c.limit>0 && sp/c.limit >= th){
      const pct = Math.round(sp/c.limit*100);
      const over = sp > c.limit;
      alerts.push({
        type: over?'error':'warn',
        msg: `${c.icon} ${c.name}: ${pct}% del límite ${over?'SUPERADO ⚠️':'· casi lleno'}`
      });
    }
  });
  const totalSp = totalSpent(m);
  const totalB = totalBudget(m);
  if(totalB>0 && totalSp/totalB >= th){
    alerts.push({type: totalSp>totalB?'error':'warn', msg:`📊 Presupuesto total: ${Math.round(totalSp/totalB*100)}% usado`});
  }
  const html = alerts.map(a=>`
    <div class="alert-banner ${a.type==='warn'?'warn':''}">⚡ ${a.msg}</div>
  `).join('');
  $('#alertArea').innerHTML = html;
}

// ═══════════════════════════════════════
//  TRANSACTIONS CRUD
// ═══════════════════════════════════════
function openTxnModal(){
  const sel = $('#txCat');
  sel.innerHTML = '';
  state.categories.forEach(c=>{
    sel.innerHTML += `<option value="${c.id}">${c.icon} ${c.name} (${c.group})</option>`;
  });
  $('#txAmount').value = '';
  $('#txNote').value = '';
  $('#txDate').value = state.activeMonth+'-01';
  $('#txType').value = 'expense';
  $('#modalTxnTitle').textContent = 'Nuevo movimiento';
  openModal('#modalTxn');
}
function saveTxn(){
  const amt = parseFloat($('#txAmount').value);
  if(!amt || amt<=0){ alert('Ingresa un monto válido'); return; }
  const t = {
    id: uid(),
    date: $('#txDate').value || state.activeMonth+'-01',
    amount: amt,
    categoryId: $('#txCat').value,
    type: $('#txType').value,
    note: $('#txNote').value.trim()
  };
  state.transactions.push(t);
  // make sure activeMonth matches
  const m = t.date.slice(0,7);
  if(m !== state.activeMonth){ state.activeMonth = m; }
  save(); closeModals(); render();
  // haptic-ish feedback
  if(navigator.vibrate) navigator.vibrate(15);
}
function delTxn(id){
  if(!confirm('¿Eliminar este movimiento?')) return;
  state.transactions = state.transactions.filter(t=>t.id!==id);
  save(); render();
}

// ═══════════════════════════════════════
//  CATEGORIES CRUD
// ═══════════════════════════════════════
function openCatModal(id){
  editingCatId = id || null;
  if(id){
    const c = catById(id);
    $('#modalCatTitle').textContent = 'Editar categoría';
    $('#catName').value = c.name;
    $('#catLimit').value = c.limit;
    $('#catGroup').value = c.group;
    $('#catIcon').value = c.icon;
    $('#catColor').value = c.color;
  } else {
    $('#modalCatTitle').textContent = 'Nueva categoría';
    $('#catName').value = '';
    $('#catLimit').value = '';
    $('#catGroup').value = 'Variable';
    $('#catIcon').value = '📌';
    $('#catColor').value = '#4f8cff';
  }
  openModal('#modalCat');
}
function saveCat(){
  const name = $('#catName').value.trim();
  const limit = parseFloat($('#catLimit').value) || 0;
  if(!name){ alert('Ponle un nombre'); return; }
  const data = {
    name, limit,
    group: $('#catGroup').value,
    icon: $('#catIcon').value || '📌',
    color: $('#catColor').value
  };
  if(editingCatId){
    const c = catById(editingCatId);
    Object.assign(c, data);
  } else {
    data.id = uid();
    state.categories.push(data);
  }
  save(); closeModals(); render();
}
function delCat(id){
  const c = catById(id);
  if(!confirm(`¿Eliminar "${c.name}"? Los movimientos de esa categoría se quedan sin categoría.`)) return;
  state.categories = state.categories.filter(x=>x.id!==id);
  save(); render();
}

// ═══════════════════════════════════════
//  FILTER
// ═══════════════════════════════════════
function showFilter(){ openModal('#modalFilter'); }
function applyFilter(){
  currentFilter = { cat:$('#filterCat').value, type:$('#filterType').value };
  closeModals(); render();
}

// ═══════════════════════════════════════
//  TRIP MODE
// ═══════════════════════════════════════
function toggleTrip(){
  state.settings.tripMode = !state.settings.tripMode;
  save(); render();
}

// ═══════════════════════════════════════
//  EXPORT / IMPORT
// ═══════════════════════════════════════
function exportData(){
  const blob = new Blob([JSON.stringify(state,null,2)],{type:'application/json'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'presupuesto_'+state.activeMonth+'.json';
  a.click();
}
function importData(e){
  const file = e.target.files[0];
  if(!file) return;
  const r = new FileReader();
  r.onload = ()=>{
    try{
      state = JSON.parse(r.result);
      save(); render();
      alert('Datos importados ✓');
    }catch(err){ alert('Archivo no válido'); }
  };
  r.readAsText(file);
}

// ═══════════════════════════════════════
//  MODAL HELPERS
// ═══════════════════════════════════════
function openModal(sel){ $(sel).classList.add('open'); }
function closeModals(){ document.querySelectorAll('.modal-bg').forEach(m=>m.classList.remove('open')); }
// close on backdrop tap
document.querySelectorAll('.modal-bg').forEach(m=>{
  m.addEventListener('click', e=>{ if(e.target===m) closeModals(); });
});

// ═══════════════════════════════════════
//  INIT
// ═══════════════════════════════════════
render();

// Optional: browser notification for alerts
if('Notification' in window && Notification.permission==='default'){
  setTimeout(()=>Notification.requestPermission(), 3000);
}
</script>
</body>
</html>
