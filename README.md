<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Presupuesto">
<title>Mi Presupuesto</title>
<style>
:root{--bg:#0f1117;--card:#1a1d27;--card2:#232734;--accent:#4f8cff;--green:#34d399;--yellow:#fbbf24;--red:#f87171;--text:#e8eaf0;--text2:#9ca3b8;--radius:14px}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;background:var(--bg);color:var(--text);max-width:480px;margin:0 auto;min-height:100vh;padding-bottom:80px}

.topbar{position:sticky;top:0;z-index:100;background:var(--bg);padding:16px 18px 10px;display:flex;align-items:center;justify-content:space-between}
.topbar h1{font-size:20px;font-weight:700}
.month-nav{display:flex;align-items:center;gap:10px;font-size:15px;color:var(--text2)}
.month-nav button{background:var(--card2);border:none;color:var(--text);width:34px;height:34px;border-radius:50%;font-size:18px;cursor:pointer;display:grid;place-items:center}

.tabbar{position:fixed;bottom:0;left:0;right:0;z-index:100;background:var(--card);border-top:1px solid #2a2e3d;display:flex;justify-content:space-around;padding:8px 0 max(26px,env(safe-area-inset-bottom))}
.tab{display:flex;flex-direction:column;align-items:center;gap:2px;font-size:10px;color:var(--text2);cursor:pointer;padding:4px 12px}
.tab.active{color:var(--accent)}
.tab .ic{font-size:22px}

.page{display:none;padding:4px 14px}
.page.active{display:block}
.card{background:var(--card);border-radius:var(--radius);padding:16px;margin-bottom:12px}
.card h3{font-size:13px;color:var(--text2);text-transform:uppercase;letter-spacing:.5px;margin-bottom:10px}
.big-num{font-size:32px;font-weight:800}
.big-num.green{color:var(--green)}.big-num.red{color:var(--red)}
.sub{font-size:13px;color:var(--text2);margin-top:2px}
.bar-wrap{height:10px;background:var(--card2);border-radius:6px;margin-top:8px;overflow:hidden}
.bar-fill{height:100%;border-radius:6px;transition:width .4s}

.cat-row{display:flex;align-items:center;gap:12px;padding:12px 0;border-bottom:1px solid #2a2e3d}
.cat-row:last-child{border:none}
.cat-icon{width:38px;height:38px;border-radius:10px;display:grid;place-items:center;font-size:18px;flex-shrink:0}
.cat-info{flex:1;min-width:0}
.cat-name{font-size:15px;font-weight:600}
.cat-limit{font-size:12px;color:var(--text2)}
.cat-amt{font-size:15px;font-weight:700;text-align:right}
.cat-remain{font-size:11px;color:var(--text2);text-align:right}

.btn{display:inline-flex;align-items:center;justify-content:center;gap:6px;padding:12px 18px;border-radius:10px;border:none;font-size:15px;font-weight:600;cursor:pointer;background:var(--accent);color:#fff;min-height:44px;-webkit-appearance:none}
.btn:active{transform:scale(.96);opacity:.8}
.btn-sm{padding:8px 14px;font-size:13px;min-height:36px}
.btn-outline{background:transparent;border:1.5px solid var(--card2);color:var(--text)}
.btn-danger{background:var(--red)}
.btn-green{background:var(--green)}

.alert-banner{background:rgba(248,113,113,.15);border:1px solid rgba(248,113,113,.3);border-radius:10px;padding:12px 14px;margin-bottom:12px;font-size:13px;color:var(--red);display:flex;align-items:center;gap:8px}
.alert-banner.warn{background:rgba(251,191,36,.12);border-color:rgba(251,191,36,.3);color:var(--yellow)}

/* ── MODAL: scrollable, sticky footer, iOS-safe ── */
.modal-bg{display:none;position:fixed;inset:0;z-index:200;background:rgba(0,0,0,.65);backdrop-filter:blur(4px);align-items:flex-end;justify-content:center}
.modal-bg.open{display:flex}
.modal{background:var(--card);width:100%;max-width:480px;border-radius:20px 20px 0 0;
  max-height:85vh;display:flex;flex-direction:column;animation:slideUp .25s ease}
@keyframes slideUp{from{transform:translateY(100%)}to{transform:translateY(0)}}
.modal-head{padding:20px 20px 0;font-size:18px;font-weight:700;flex-shrink:0}
.modal-body{padding:16px 20px;overflow-y:auto;flex:1;-webkit-overflow-scrolling:touch}
.modal-foot{padding:14px 20px max(20px,env(safe-area-inset-bottom));display:flex;gap:10px;flex-shrink:0;border-top:1px solid #2a2e3d}
.modal-foot .btn{flex:1}

.field{margin-bottom:14px}
.field label{font-size:13px;color:var(--text2);display:block;margin-bottom:5px}
.field input,.field select,.field textarea{width:100%;padding:13px;border-radius:10px;border:1.5px solid var(--card2);background:var(--card2);color:var(--text);font-size:17px;outline:none;-webkit-appearance:none}
.field input:focus,.field select:focus{border-color:var(--accent)}
.field .err{color:var(--red);font-size:12px;margin-top:4px;display:none}
.field .err.show{display:block}

.flex{display:flex;align-items:center;gap:10px}
.flex-between{display:flex;justify-content:space-between;align-items:center}
.mt{margin-top:12px}.mb{margin-bottom:12px}.center{text-align:center}
.section-label{font-size:11px;text-transform:uppercase;letter-spacing:1px;color:var(--text2);margin:18px 0 8px;font-weight:700}
.empty{text-align:center;padding:40px 20px;color:var(--text2);font-size:14px}
.toggle-row{display:flex;justify-content:space-between;align-items:center;padding:12px 0}
.toggle{width:48px;height:28px;background:var(--card2);border-radius:14px;position:relative;cursor:pointer;transition:background .2s;flex-shrink:0}
.toggle.on{background:var(--accent)}
.toggle::after{content:'';width:22px;height:22px;background:#fff;border-radius:50%;position:absolute;top:3px;left:3px;transition:left .2s}
.toggle.on::after{left:23px}
.fab{position:fixed;bottom:90px;right:18px;z-index:50;width:58px;height:58px;border-radius:50%;background:var(--accent);color:#fff;border:none;font-size:30px;display:grid;place-items:center;box-shadow:0 4px 16px rgba(79,140,255,.4);cursor:pointer}
.fab:active{transform:scale(.92)}
.trip-badge{display:inline-block;font-size:11px;background:var(--accent);color:#fff;padding:2px 8px;border-radius:6px;margin-left:6px}
</style>
</head>
<body>

<div class="topbar">
  <h1>💰 Mi Presupuesto</h1>
  <div class="month-nav">
    <button type="button" onclick="changeMonth(-1)">‹</button>
    <span id="monthLabel"></span>
    <button type="button" onclick="changeMonth(1)">›</button>
  </div>
</div>

<!-- DASHBOARD -->
<div class="page active" id="page-dash">
  <div id="alertArea"></div>
  <div class="card center">
    <h3>Ingreso del mes</h3>
    <div class="big-num" id="incomeDisplay">0</div>
    <div class="sub" id="incomeSub"></div>
    <div class="mt flex-between">
      <div><div class="big-num green" id="savedDisplay" style="font-size:26px">0</div><div class="sub">Ahorro estimado</div></div>
      <div style="text-align:right"><div class="big-num" id="spentDisplay" style="font-size:26px;color:var(--text2)">0</div><div class="sub">Gastado</div></div>
    </div>
    <div class="bar-wrap mt" style="height:12px"><div class="bar-fill" id="mainBar" style="width:0%;background:var(--green)"></div></div>
    <div class="sub mt" id="mainBarLabel"></div>
  </div>
  <div class="card"><h3>Resumen por categoría</h3><div id="catSummary"></div></div>
</div>

<!-- GASTOS -->
<div class="page" id="page-txn">
  <div class="flex-between mb"><h3 style="font-size:16px">Movimientos</h3><button class="btn btn-sm btn-outline" type="button" onclick="showFilter()">🔍 Filtro</button></div>
  <div class="card" id="txnList"></div>
</div>

<!-- PRESUPUESTO -->
<div class="page" id="page-budget">
  <div class="flex-between mb"><h3 style="font-size:16px">Presupuesto</h3><button class="btn btn-sm" type="button" onclick="openCatModal()">+ Categoría</button></div>
  <div class="card" id="budgetList"></div>
  <div class="card">
    <h3>Opciones</h3>
    <div class="toggle-row"><span>Modo Viaje España (−2.000 QAR)</span><div class="toggle" id="tripToggle" onclick="toggleTrip()"></div></div>
    <div class="field mt"><label>Umbral de alerta</label>
      <input type="range" id="alertSlider" min="50" max="100" value="80" oninput="document.getElementById('alertVal').textContent=this.value+'%';state.settings.alertThreshold=+this.value/100;save()">
      <span id="alertVal" style="font-size:13px;color:var(--text2)">80%</span>
    </div>
  </div>
</div>

<!-- AJUSTES -->
<div class="page" id="page-settings">
  <div class="card">
    <h3>Respaldo de datos</h3>
    <div class="flex" style="gap:8px;flex-wrap:wrap">
      <button class="btn btn-sm btn-outline" type="button" onclick="exportData()">📤 Exportar</button>
      <button class="btn btn-sm btn-outline" type="button" onclick="document.getElementById('importFile').click()">📥 Importar</button>
      <input type="file" id="importFile" accept=".json" style="display:none" onchange="importData(event)">
    </div>
    <p class="sub mt">Descarga un JSON y guárdalo como respaldo.</p>
  </div>
  <div class="card"><h3>Instalar como app</h3>
    <p class="sub">En Safari: botón Compartir (⬆) → <b>"Añadir a pantalla de inicio"</b>.</p>
  </div>
  <div class="card"><h3>Zona de peligro</h3>
    <button class="btn btn-sm btn-danger" type="button" onclick="if(confirm('¿Borrar TODO?')){localStorage.removeItem('mp_data');location.reload()}">🗑 Borrar todo</button>
  </div>
</div>

<button class="fab" type="button" onclick="openTxnModal()">+</button>

<!-- MODAL: Nuevo movimiento -->
<div class="modal-bg" id="modalTxn">
  <div class="modal">
    <div class="modal-head" id="modalTxnTitle">Nuevo movimiento</div>
    <div class="modal-body">
      <div class="field">
        <label>Monto (QAR)</label>
        <input type="text" id="txAmount" inputmode="decimal" autocapitalize="off" autocomplete="off" placeholder="0.00" enterkeyhint="done">
        <div class="err" id="txAmountErr">Escribe un monto mayor a 0</div>
      </div>
      <div class="field">
        <label>Categoría</label>
        <select id="txCat"></select>
      </div>
      <div class="field">
        <label>Concepto (opcional)</label>
        <input type="text" id="txNote" placeholder="Ej: Cena con amigos" autocapitalize="sentences" autocomplete="off">
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
    </div>
    <div class="modal-foot">
      <button class="btn btn-outline" type="button" onclick="closeModals()">Cancelar</button>
      <button class="btn btn-green" type="button" onclick="saveTxn()">💾 Guardar</button>
    </div>
  </div>
</div>

<!-- MODAL: Categoría -->
<div class="modal-bg" id="modalCat">
  <div class="modal">
    <div class="modal-head" id="modalCatTitle">Nueva categoría</div>
    <div class="modal-body">
      <div class="field"><label>Nombre</label><input type="text" id="catName" placeholder="Ej: Películas" autocomplete="off"></div>
      <div class="field"><label>Límite mensual (QAR)</label><input type="text" id="catLimit" inputmode="decimal" placeholder="0" autocomplete="off"></div>
      <div class="field"><label>Grupo</label>
        <select id="catGroup"><option value="Fijo">Fijo</option><option value="Variable" selected>Variable</option><option value="Apartado">Apartado</option><option value="Otro">Otro</option></select>
      </div>
      <div class="field"><label>Icono (emoji)</label><input type="text" id="catIcon" value="📌" maxlength="4" style="width:80px;text-align:center;font-size:22px"></div>
      <div class="field"><label>Color</label><input type="color" id="catColor" value="#4f8cff" style="width:60px;height:44px;padding:2px"></div>
    </div>
    <div class="modal-foot">
      <button class="btn btn-outline" type="button" onclick="closeModals()">Cancelar</button>
      <button class="btn" type="button" onclick="saveCat()">Guardar</button>
    </div>
  </div>
</div>

<!-- MODAL: Filtro -->
<div class="modal-bg" id="modalFilter">
  <div class="modal">
    <div class="modal-head">Filtrar movimientos</div>
    <div class="modal-body">
      <div class="field"><label>Categoría</label><select id="filterCat"><option value="">Todas</option></select></div>
      <div class="field"><label>Tipo</label><select id="filterType"><option value="">Todos</option><option value="expense">Gastos</option><option value="income">Ingresos</option></select></div>
    </div>
    <div class="modal-foot">
      <button class="btn btn-outline" type="button" onclick="closeModals()">Cancelar</button>
      <button class="btn" type="button" onclick="applyFilter()">Aplicar</button>
    </div>
  </div>
</div>

<div class="tabbar">
  <div class="tab active" onclick="showPage('dash')"><span class="ic">🏠</span>Inicio</div>
  <div class="tab" onclick="showPage('txn')"><span class="ic">📋</span>Gastos</div>
  <div class="tab" onclick="showPage('budget')"><span class="ic">⚙️</span>Presupuesto</div>
  <div class="tab" onclick="showPage('settings')"><span class="ic">🔧</span>Ajustes</div>
</div>

<script>
/* ════════ STATE ════════ */
var DEFAULT = {
  settings:{monthlyIncome:23600,incomeDay:25,alertThreshold:0.80,tripMode:false,tripReduction:2000,currency:'QAR'},
  categories:[
    {id:'c1',name:'Arriendo Doha',limit:3750,group:'Fijo',icon:'\u{1F3E0}',color:'#f87171'},
    {id:'c2',name:'Datos',limit:125,group:'Fijo',icon:'\u{1F4E1}',color:'#a78bfa'},
    {id:'c3',name:'Comida',limit:1400,group:'Variable',icon:'\u{1F37D}\u{FE0F}',color:'#fbbf24'},
    {id:'c4',name:'Transporte',limit:400,group:'Variable',icon:'\u{1F68C}',color:'#34d399'},
    {id:'c5',name:'Salidas / Eventos',limit:600,group:'Variable',icon:'\u{1F389}',color:'#f472b6'},
    {id:'c6',name:'Deporte',limit:400,group:'Variable',icon:'\u{26BD}',color:'#4f8cff'},
    {id:'c7',name:'Imprevistos',limit:200,group:'Variable',icon:'\u{1FA79}',color:'#fbbf24'},
    {id:'c8',name:'Familia',limit:1000,group:'Apartado',icon:'\u{1F468}\u{200D}\u{1F469}\u{200D}\u{1F467}',color:'#34d399'},
    {id:'c9',name:'Bolsillo personal',limit:1500,group:'Apartado',icon:'\u{1F455}',color:'#a78bfa'},
    {id:'c10',name:'Viaje España',limit:2000,group:'Apartado',icon:'\u2708\u{FE0F}',color:'#4f8cff'}
  ],
  transactions:[],
  activeMonth: new Date().toISOString().slice(0,7)
};

var state, editingCatId = null, currentFilter = {cat:'',type:''};

function load(){
  try{
    var d = localStorage.getItem('mp_data');
    if(d){ state = JSON.parse(d); return; }
  }catch(e){}
  /* NO usar structuredClone: no soportado en iOS < 15.4 */
  state = JSON.parse(JSON.stringify(DEFAULT));
}
function save(){
  try{ localStorage.setItem('mp_data', JSON.stringify(state)); }
  catch(e){ console.warn('save failed', e); }
}

/* ════════ HELPERS ════════ */
var $ = function(s){ return document.getElementById(s); };
var fmt = function(n){
  return new Intl.NumberFormat('es-ES',{minimumFractionDigits:0,maximumFractionDigits:2}).format(n);
};
function monthName(m){
  var d = new Date(m+'-01T00:00:00');
  return d.toLocaleDateString('es-ES',{month:'long',year:'numeric'});
}
function catById(id){ for(var i=0;i<state.categories.length;i++){if(state.categories[i].id===id)return state.categories[i];} }
function txnsForMonth(m){ return state.transactions.filter(function(t){return t.date.slice(0,7)===m;}); }
function spentForCat(cid,m){
  var s=0; txnsForMonth(m).forEach(function(t){if(t.categoryId===cid&&t.type==='expense')s+=t.amount;}); return s;
}
function totalSpent(m){ var s=0; txnsForMonth(m).forEach(function(t){if(t.type==='expense')s+=t.amount;}); return s; }
function totalIncome(m){
  var b=state.settings.monthlyIncome;
  if(state.settings.tripMode) b-=state.settings.tripReduction;
  var x=0; txnsForMonth(m).forEach(function(t){if(t.type==='income')x+=t.amount;});
  return b+x;
}
function totalBudget(m){ var s=0; state.categories.forEach(function(c){s+=c.limit;}); return s; }
function uid(){ return 'x'+Date.now()+Math.random().toString(36).slice(2,7); }

/* ════════ NAV ════════ */
function showPage(p){
  var pages=document.querySelectorAll('.page');
  for(var i=0;i<pages.length;i++)pages[i].classList.remove('active');
  $(('page-'+p)).classList.add('active');
  var tabs=document.querySelectorAll('.tab');
  var ids=['dash','txn','budget','settings'];
  for(var i=0;i<tabs.length;i++)tabs[i].classList.toggle('active',ids[i]===p);
  render();
}
function changeMonth(d){
  var parts=state.activeMonth.split('-');
  var dt=new Date(+parts[0],+parts[1]-1+d,1);
  state.activeMonth=dt.getFullYear()+'-'+String(dt.getMonth()+1).padStart(2,'0');
  save();render();
}

/* ════════ RENDER ════════ */
function render(){
  var m=state.activeMonth;
  var inc=totalIncome(m), sp=totalSpent(m), bud=totalBudget(m), sav=inc-bud;
  var pct=bud>0?Math.min(sp/bud*100,100):0;

  $('monthLabel').textContent=monthName(m);
  $('incomeDisplay').textContent=fmt(inc)+' '+state.settings.currency;
  $('incomeSub').textContent='El día '+state.settings.incomeDay+' de cada mes'+(state.settings.tripMode?' · ✈️ Modo viaje':'');
  $('spentDisplay').textContent=fmt(sp)+' '+state.settings.currency;
  $('savedDisplay').textContent=fmt(sav)+' '+state.settings.currency;
  $('savedDisplay').style.color=sav>=0?'var(--green)':'var(--red)';

  var bar=$('mainBar');
  bar.style.width=pct+'%';
  bar.style.background=pct<70?'var(--green)':pct<90?'var(--yellow)':'var(--red)';
  $('mainBarLabel').textContent=Math.round(pct)+'% del presupuesto gastado';

  renderAlerts();

  /* cat summary */
  var ch='';
  state.categories.forEach(function(c){
    var s=spentForCat(c.id,m), p=c.limit>0?Math.min(s/c.limit*100,100):0;
    var col=p<70?'var(--green)':p<90?'var(--yellow)':'var(--red)';
    ch+='<div class="cat-row"><div class="cat-icon" style="background:'+c.color+'22;color:'+c.color+'">'+c.icon+'</div>'+
      '<div class="cat-info"><div class="cat-name">'+c.name+'</div><div class="cat-limit">Límite: '+fmt(c.limit)+' '+state.settings.currency+'</div></div>'+
      '<div><div class="cat-amt" style="color:'+col+'">'+fmt(s)+'</div><div class="cat-remain">'+fmt(c.limit-s)+' libre</div></div></div>'+
      '<div class="bar-wrap"><div class="bar-fill" style="width:'+p+'%;background:'+col+'"></div></div>';
  });
  $('catSummary').innerHTML=ch||'<div class="empty">Sin categorías</div>';

  /* transactions */
  var fl=txnsForMonth(m);
  if(currentFilter.cat)fl=fl.filter(function(t){return t.categoryId===currentFilter.cat;});
  if(currentFilter.type)fl=fl.filter(function(t){return t.type===currentFilter.type;});
  fl.sort(function(a,b){return b.date.localeCompare(a.date);});
  var th='';
  if(!fl.length){
    th='<div class="empty">📭 No hay movimientos.<br>Toca el botón + para añadir.</div>';
  }else{
    fl.forEach(function(t){
      var c=catById(t.categoryId);
      var ds=new Date(t.date+'T00:00:00').toLocaleDateString('es-ES',{day:'numeric',month:'short'});
      th+='<div class="txn"><div class="cat-icon" style="background:'+(c?c.color:'#555')+'22;color:'+(c?c.color:'#555')+';width:34px;height:34px;font-size:15px">'+(c?c.icon:'❓')+'</div>'+
        '<div class="cat-info"><div class="cat-name" style="font-size:14px">'+(c?c.name:'—')+'</div><div class="txn-date">'+ds+(t.note?' · '+t.note:'')+'</div></div>'+
        '<div><div class="txn-amt '+(t.type==='income'?'inc':'exp')+'">'+(t.type==='income'?'+':'−')+fmt(t.amount)+'</div>'+
        '<button class="btn btn-sm btn-outline" type="button" style="padding:3px 10px;font-size:10px;margin-top:4px" onclick="delTxn(\''+t.id+'\')">✕</button></div></div>';
    });
  }
  $('txnList').innerHTML=th;

  /* budget page */
  var groups=['Fijo','Variable','Apartado','Otro'],bh='';
  groups.forEach(function(g){
    var cs=state.categories.filter(function(c){return c.group===g;});
    if(!cs.length)return;
    bh+='<div class="section-label">'+g+'</div>';
    cs.forEach(function(c){
      var s=spentForCat(c.id,m);
      bh+='<div class="cat-row"><div class="cat-icon" style="background:'+c.color+'22;color:'+c.color+'">'+c.icon+'</div>'+
        '<div class="cat-info"><div class="cat-name">'+c.name+'</div><div class="cat-limit">'+fmt(s)+' / '+fmt(c.limit)+' '+state.settings.currency+'</div></div>'+
        '<div class="flex" style="gap:4px"><button class="btn btn-sm btn-outline" type="button" onclick="openCatModal(\''+c.id+'\')">✏️</button>'+
        '<button class="btn btn-sm btn-outline" type="button" onclick="delCat(\''+c.id+'\')">🗑</button></div></div>';
    });
  });
  $('budgetList').innerHTML=bh||'<div class="empty">Añade categorías</div>';
  $('tripToggle').className='toggle'+(state.settings.tripMode?' on':'');

  var fo='<option value="">Todas</option>';
  state.categories.forEach(function(c){fo+='<option value="'+c.id+'">'+c.icon+' '+c.name+'</option>';});
  $('filterCat').innerHTML=fo;
  if(currentFilter.cat)$('filterCat').value=currentFilter.cat;
}

function renderAlerts(){
  var m=state.activeMonth,th=state.settings.alertThreshold,al=[];
  state.categories.forEach(function(c){
    var s=spentForCat(c.id,m);
    if(c.limit>0&&s/c.limit>=th){
      var p=Math.round(s/c.limit*100),over=s>c.limit;
      al.push({t:over?'error':'warn',m:c.icon+' '+c.name+': '+p+'% '+(over?'SUPERADO ⚠️':'· casi lleno')});
    }
  });
  var ts=totalSpent(m),tb=totalBudget(m);
  if(tb>0&&ts/tb>=th)al.push({t:ts>tb?'error':'warn',m:'📊 Total: '+Math.round(ts/tb*100)+'% usado'});
  $('alertArea').innerHTML=al.map(function(a){
    return '<div class="alert-banner'+(a.t==='warn'?' warn':'')+'">⚡ '+a.m+'</div>';
  }).join('');
}

/* ════════ TXN CRUD ════════ */
function openTxnModal(){
  var sel=$('txCat');
  sel.innerHTML='';
  state.categories.forEach(function(c){
    sel.innerHTML+='<option value="'+c.id+'">'+c.icon+' '+c.name+' ('+c.group+')</option>';
  });
  $('txAmount').value='';
  $('txNote').value='';
  $('txDate').value=state.activeMonth+'-01';
  $('txType').value='expense';
  $('txAmountErr').className='err';
  $('modalTxnTitle').textContent='Nuevo movimiento';
  openModal('modalTxn');
  /* focus after animation */
  setTimeout(function(){ $('txAmount').focus(); },350);
}

function saveTxn(){
  var raw=$('txAmount').value.trim().replace(',','.');
  var amt=parseFloat(raw);
  if(!amt||amt<=0){
    $('txAmountErr').className='err show';
    $('txAmount').focus();
    return;
  }
  /* cerrar teclado en iOS */
  $('txAmount').blur();
  $('txNote').blur();

  var t={
    id:uid(),
    date:$('txDate').value||state.activeMonth+'-01',
    amount:amt,
    categoryId:$('txCat').value,
    type:$('txType').value,
    note:$('txNote').value.trim()
  };
  state.transactions.push(t);
  if(t.date.slice(0,7)!==state.activeMonth)state.activeMonth=t.date.slice(0,7);
  save();
  closeModals();
  render();
  /* feedback */
  if(navigator.vibrate)navigator.vibrate(15);
}

function delTxn(id){
  if(!confirm('¿Eliminar este movimiento?'))return;
  state.transactions=state.transactions.filter(function(t){return t.id!==id;});
  save();render();
}

/* ════════ CAT CRUD ════════ */
function openCatModal(id){
  editingCatId=id||null;
  if(id){
    var c=catById(id);
    $('modalCatTitle').textContent='Editar categoría';
    $('catName').value=c.name;
    $('catLimit').value=c.limit;
    $('catGroup').value=c.group;
    $('catIcon').value=c.icon;
    $('catColor').value=c.color;
  }else{
    $('modalCatTitle').textContent='Nueva categoría';
    $('catName').value='';$('catLimit').value='';
    $('catGroup').value='Variable';$('catIcon').value='📌';$('catColor').value='#4f8cff';
  }
  openModal('modalCat');
}
function saveCat(){
  var name=$('catName').value.trim();
  var raw=$('catLimit').value.trim().replace(',','.');
  var limit=parseFloat(raw)||0;
  if(!name){alert('Ponle un nombre');return;}
  var data={name:name,limit:limit,group:$('catGroup').value,icon:$('catIcon').value||'📌',color:$('catColor').value};
  if(editingCatId){
    var c=catById(editingCatId);
    for(var k in data)c[k]=data[k];
  }else{
    data.id=uid();
    state.categories.push(data);
  }
  save();closeModals();render();
}
function delCat(id){
  var c=catById(id);
  if(!confirm('¿Eliminar "'+c.name+'"? Los movimientos quedan sin categoría.'))return;
  state.categories=state.categories.filter(function(x){return x.id!==id;});
  save();render();
}

/* ════════ FILTER / TRIP ════════ */
function showFilter(){openModal('modalFilter');}
function applyFilter(){
  currentFilter={cat:$('filterCat').value,type:$('filterType').value};
  closeModals();render();
}
function toggleTrip(){
  state.settings.tripMode=!state.settings.tripMode;
  save();render();
}

/* ════════ EXPORT / IMPORT ════════ */
function exportData(){
  var blob=new Blob([JSON.stringify(state,null,2)],{type:'application/json'});
  var a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download='presupuesto_'+state.activeMonth+'.json';
  document.body.appendChild(a);a.click();document.body.removeChild(a);
}
function importData(e){
  var f=e.target.files[0];if(!f)return;
  var r=new FileReader();
  r.onload=function(){
    try{state=JSON.parse(r.result);save();render();alert('✓ Datos importados');}
    catch(err){alert('Archivo no válido');}
  };
  r.readAsText(f);
}

/* ════════ MODALS ════════ */
function openModal(id){ $(id).classList.add('open'); document.body.style.overflow='hidden'; }
function closeModals(){
  var ms=document.querySelectorAll('.modal-bg');
  for(var i=0;i<ms.length;i++)ms[i].classList.remove('open');
  document.body.style.overflow='';
}
document.querySelectorAll('.modal-bg').forEach(function(m){
  m.addEventListener('click',function(e){if(e.target===m)closeModals();});
});

/* ════════ INIT ════════ */
load();
render();
</script>
</body>
</html>
