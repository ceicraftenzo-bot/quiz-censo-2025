<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Censo Tech 2025 — Quiz & Painel</title>

<!-- Chart.js CDN -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
:root{
  --bg:#0f1724; --card:#0b1220; --accent:#7c3aed; --muted:#9ca3af; --text:#e6eef8;
  --card-2:#0f1727;
}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,Arial;background:linear-gradient(180deg,#071029 0%,#071827 100%);color:var(--text);padding:32px;-webkit-font-smoothing:antialiased}
.container{max-width:1100px;margin:0 auto;display:grid;grid-template-columns:1fr 420px;gap:22px}
.header{display:flex;justify-content:space-between;align-items:center;margin-bottom:14px}
.brand{display:flex;gap:12px;align-items:center}
.brand .logo{width:54px;height:54px;background:linear-gradient(135deg,#7c3aed,#06b6d4);border-radius:10px;display:flex;align-items:center;justify-content:center;font-weight:700;color:white}
.header h1{margin:0;font-size:20px}
.header small{display:block;color:var(--muted)}

.card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:18px;border-radius:12px;box-shadow:0 8px 30px rgba(2,6,23,0.6)}
.quiz-area{padding:4px}
.question{margin-bottom:12px}
.q-title{font-weight:600;margin-bottom:6px}
.input, select, textarea{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:var(--text)}
.row{display:flex;gap:10px}
.col{flex:1}
.actions{display:flex;gap:10px;margin-top:12px}
.btn{background:var(--accent);border:none;color:white;padding:10px 14px;border-radius:8px;cursor:pointer;font-weight:600}
.btn.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--muted)}
.small{font-size:13px;color:var(--muted)}
.right-col{position:sticky;top:28px;align-self:start}
.stat{display:flex;justify-content:space-between;gap:10px}
.stat strong{font-size:18px}
.result-area{margin-top:12px}
.footer{margin-top:14px;text-align:center;color:var(--muted);font-size:13px}

/* dashboard modal */
.modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(2,6,23,0.6);z-index:1000}
.modal-inner{width:90%;max-width:1100px;max-height:92vh;overflow:auto;background:var(--card-2);padding:18px;border-radius:12px}
.grid-2{display:grid;grid-template-columns:1fr 360px;gap:14px}
.chart-box{background:rgba(255,255,255,0.02);padding:12px;border-radius:10px}
.table{width:100%;border-collapse:collapse;margin-top:8px}
.table th,.table td{padding:8px;border-bottom:1px dashed rgba(255,255,255,0.03);text-align:left}
.search{display:flex;gap:8px;margin-bottom:8px}
.kv{font-size:13px;color:var(--muted)}
@media (max-width:980px){.container{grid-template-columns:1fr;padding:12px}.right-col{position:static}.grid-2{grid-template-columns:1fr}}
</style>
</head>
<body>

<div class="header container" style="grid-template-columns:1fr">
  <div class="brand">
    <div class="logo">CT</div>
    <div>
      <h1>Censo Tech 2025</h1>
      <small class="small">Quiz interativo • Dados salvos localmente • Painel admin com PIN</small>
    </div>
  </div>
  <div style="display:flex;gap:10px;align-items:center">
    <button class="btn ghost" id="openPanelBtn">Painel (admin)</button>
    <button class="btn" id="exportAllBtn">Exportar Tudo (JSON)</button>
  </div>
</div>

<div class="container">
  <!-- LEFT: Quiz -->
  <div class="card quiz-area">
    <h2 style="margin-top:0">Responder Censo</h2>
    <p class="small">Responda com atenção. Seus dados ficarão salvos localmente e podem ser exportados.</p>

    <div id="formArea">
      <!-- Perguntas -->
      <div class="question">
        <div class="q-title">1. Nome completo</div>
        <input id="p_name" class="input" placeholder="Seu nome">
      </div>

      <div class="row">
        <div class="col question">
          <div class="q-title">2. Idade</div>
          <input id="p_age" class="input" type="number" min="6" max="120" placeholder="Ex: 17">
        </div>
        <div class="col question">
          <div class="q-title">3. Sexo (opcional)</div>
          <select id="p_gender" class="input">
            <option value="">-- prefiro não informar --</option>
            <option>Masculino</option>
            <option>Feminino</option>
            <option>Outro</option>
          </select>
        </div>
      </div>

      <div class="question">
        <div class="q-title">4. Dispositivo principal</div>
        <select id="p_device" class="input">
          <option>Celular</option>
          <option>Computador</option>
          <option>Tablet</option>
          <option>Console</option>
          <option>Smartwatch</option>
          <option>Outro</option>
        </select>
      </div>

      <div class="question">
        <div class="q-title">5. Quantas horas usa a internet por dia?</div>
        <select id="p_hours" class="input">
          <option>menos de 1h</option>
          <option>1–3h</option>
          <option>3–5h</option>
          <option>5–8h</option>
          <option>mais de 8h</option>
        </select>
      </div>

      <div class="question">
        <div class="q-title">6. Principais apps que usa (separe por vírgula, até 5)</div>
        <input id="p_apps" class="input" placeholder="Ex: WhatsApp, Instagram, TikTok">
      </div>

      <div class="question">
        <div class="q-title">7. Quanto tempo jogando por dia (em horas)</div>
        <input id="p_games" class="input" type="number" min="0" step="0.1" placeholder="Ex: 1.5">
      </div>

      <div class="question">
        <div class="q-title">8. Quanto gasta com tecnologia por ano (aprox.)</div>
        <select id="p_spend" class="input">
          <option>R$ 0 - R$ 200</option>
          <option>R$ 200 - R$ 600</option>
          <option>R$ 600 - R$ 1500</option>
          <option>R$ 1500 - R$ 5000</option>
          <option>Mais de R$ 5000</option>
        </select>
      </div>

      <div class="question">
        <div class="q-title">9. Como você avalia seu conhecimento em tecnologia?</div>
        <select id="p_skill" class="input">
          <option>Iniciante</option>
          <option>Intermediário</option>
          <option>Avançado</option>
          <option>Profissional</option>
        </select>
      </div>

      <div class="question">
        <div class="q-title">10. Observações ou comentário (opcional)</div>
        <textarea id="p_notes" class="input" placeholder="Algo que queira registrar..."></textarea>
      </div>

      <div class="actions">
        <button class="btn" id="submitBtn">Enviar Resposta</button>
        <button class="btn ghost" id="clearBtn" title="Limpar formulário">Limpar</button>
      </div>

      <div id="msg" class="small" style="margin-top:10px"></div>
    </div>

    <div class="footer small">Dica: use o botão "Exportar" no painel para baixar todos os dados. As respostas são privadas e ficam no seu navegador até exportar.</div>
  </div>

  <!-- RIGHT: Resumo rápido -->
  <div class="right-col">
    <div class="card">
      <h3 style="margin-top:0">Resumo rápido</h3>
      <div class="stat"><div class="kv">Total de respostas</div><strong id="stat_total">0</strong></div>
      <div class="stat"><div class="kv">Média de idade</div><strong id="stat_age">—</strong></div>
      <div class="stat"><div class="kv">Dispositivo mais usado</div><strong id="stat_device">—</strong></div>
      <div class="stat"><div class="kv">Tempo médio (categoria)</div><strong id="stat_hours">—</strong></div>
      <div style="margin-top:12px" class="small">Você pode acessar o painel com o PIN para ver gráficos detalhados.</div>
    </div>

    <div style="height:14px"></div>

    <div class="card">
      <h3 style="margin-top:0">Ações rápidas</h3>
      <button class="btn" id="openPanelBtn2">Abrir painel (admin)</button>
      <button class="btn ghost" id="exportLocalBtn">Exportar (JSON)</button>
      <button class="btn ghost" id="exportCSVLocal">Exportar (CSV)</button>
      <button class="btn ghost" id="deleteLocal">Apagar todas (local)</button>
      <div style="margin-top:8px" class="small">PIN padrão: <strong>1975</strong>. Mude no código se quiser.</div>
    </div>
  </div>
</div>

<!-- Modal / Dashboard -->
<div class="modal" id="modal">
  <div class="modal-inner">
    <div style="display:flex;justify-content:space-between;align-items:center;gap:8px">
      <h2 style="margin:0">Painel de Estatísticas</h2>
      <div><button class="btn ghost" id="closeModal">Fechar</button></div>
    </div>
    <div style="margin-top:12px" class="small">Protegido por PIN. Insira o PIN para visualizar os gráficos.</div>
    <div style="margin-top:10px;display:flex;gap:8px;align-items:center">
      <input id="pinInput" class="input" placeholder="Digite o PIN (ex: 1975)" style="max-width:180px">
      <button class="btn" id="unlockBtn">Entrar</button>
      <button class="btn ghost" id="exportAllJSON2">Exportar JSON</button>
      <button class="btn ghost" id="exportAllCSV2">Exportar CSV</button>
    </div>
    <div id="dashContent" style="display:none;margin-top:12px">
      <div class="grid-2" style="margin-top:8px">
        <div>
          <div class="chart-box"><canvas id="chartDevice" height="140"></canvas></div>
          <div style="height:12px"></div>
          <div class="chart-box"><canvas id="chartHours" height="120"></canvas></div>
        </div>
        <div>
          <div class="chart-box"><canvas id="chartSpend" height="220"></canvas></div>
        </div>
      </div>
      <div style="margin-top:12px" class="chart-box">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <h3 style="margin:0">Respostas (últimas 200)</h3>
          <input id="searchBox" class="input" placeholder="Pesquisar por nome ou app" style="max-width:280px">
        </div>
        <table class="table" id="answersTable">
          <thead><tr><th>Nome</th><th>Idade</th><th>Dispositivo</th><th>Horas</th><th>Apps</th><th>Gasto</th></tr></thead>
          <tbody></tbody>
        </table>
      </div>
    </div>
  </div>
</div>

<script>
// ---------- SCRIPT DO QUIZ E PAINEL ----------
const ADMIN_PIN="1975";
const STORAGE_KEY="censo_tech_responses";

function getResponses(){try{return JSON.parse(localStorage.getItem(STORAGE_KEY)||"[]");}catch(e){return[]}}
function saveResponses(arr){localStorage.setItem(STORAGE_KEY,JSON.stringify(arr))}
function appendResponse(obj){const a=getResponses();a.push(obj);saveResponses(a);updateSummary()}

// UI refs
const submitBtn=document.getElementById("submitBtn");
const clearBtn=document.getElementById("clearBtn");
const msg=document.getElementById("msg");
const exportLocalBtn=document.getElementById("exportLocalBtn");
const exportCSVLocal=document.getElementById("exportCSVLocal");
const deleteLocal=document.getElementById("deleteLocal");
const stat_total=document.getElementById("stat_total");
const stat_age=document.getElementById("stat_age");
const stat_device=document.getElementById("stat_device");
const stat_hours=document.getElementById("stat_hours");
const openPanelBtn=document.getElementById("openPanelBtn");
const openPanelBtn2=document.getElementById("openPanelBtn2");
const modal=document.getElementById("modal");
const unlockBtn=document.getElementById("unlockBtn");
const pinInput=document.getElementById("pinInput");
const dashContent=document.getElementById("dashContent");
const closeModal=document.getElementById("closeModal");
const answersTable=document.querySelector("#answersTable tbody");
const searchBox=document.getElementById("searchBox");
const exportAllJSON2=document.getElementById("exportAllJSON2");
const exportAllCSV2=document.getElementById("exportAllCSV2");

// Submit
submitBtn.addEventListener("click",e=>{
  e.preventDefault();
  const data={
    name:document.getElementById("p_name").value.trim(),
    age:parseInt(document.getElementById("p_age").value)||null,
    gender:document.getElementById("p_gender").value||"",
    device:document.getElementById("p_device").value,
    hours_category:document.getElementById("p_hours").value,
    apps:document.getElementById("p_apps").value.split(",").map(s=>s.trim()).filter(Boolean).slice(0,5),
    games_hours:parseFloat(document.getElementById("p_games").value)||0,
    spend:document.getElementById("p_spend").value,
    skill:document.getElementById("p_skill").value,
    notes:document.getElementById("p_notes").value.trim(),
    created_at:new Date().toISOString()
  };
  if(!data.name){msg.innerText="Digite seu nome.";msg.style.color="#ffbaba";return;}
  if(!data.age){msg.innerText="Informe a idade.";msg.style.color="#ffbaba";return;}
  appendResponse(data);
  msg.innerText="✔ Obrigado! Resposta salva localmente.";msg.style.color="#a3f7c2";
  document.getElementById("p_name").value="";
  document.getElementById("p_age").value="";
  document.getElementById("p_apps").value="";
  document.getElementById("p_games").value="";
  document.getElementById("p_notes").value="";
  updateSummary();
});

// Clear
clearBtn.addEventListener("click",()=>{
  document.getElementById("p_name").value="";
  document.getElementById("p_age").value="";
  document.getElementById("p_apps").value="";
  document.getElementById("p_games").value="";
  document.getElementById("p_notes").value="";
  msg.innerText="";
});

// Export JSON/CSV
function downloadFile(filename,content,type="application/json"){
  const blob=new Blob([content],{type});
  const url=URL.createObjectURL(blob);
  const a=document.createElement("a");a.href=url;a.download=filename;document.body.appendChild(a);a.click();
  setTimeout(()=>{URL.revokeObjectURL(url);a.remove();},500);
}
exportLocalBtn.addEventListener("click",()=>{downloadFile("censo_tech_2025.json",JSON.stringify(getResponses(),null,2))});
exportCSVLocal.addEventListener("click",()=>{
  let csv="name,age,device,hours_category,apps,games_hours,spend,skill,created_at\n";
  getResponses().forEach(r=>{const apps=(r.apps||[]).join(";").replace(/"/g,'""');csv+=`"${r.name||""}",${r.age||""},"${r.device}","${r.hours_category}","${apps}",${r.games_hours||0},"${r.spend}","${r.skill}","${r.created_at}"\n`;});
  downloadFile("censo_tech_2025.csv",csv,"text/csv");
});
exportAllJSON2.addEventListener("click",()=>exportLocalBtn.click());
exportAllCSV2.addEventListener





