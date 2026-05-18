LOGO_B64=$(cat /tmp/logo_b64.txt)

cat > /tmp/index_new.html << HTMLEOF
<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>TyrePrint — RacingZ</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Syne:wght@700;800&family=DM+Sans:wght@300;400;500;600&display=swap');

:root {
  --bg: #f0ede8;
  --surface: #faf8f5;
  --dark: #0f0e0c;
  --mid: #3a3830;
  --muted: #8a8580;
  --border: #d8d4ce;
  --radius: 10px;
}

* { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

body {
  font-family: 'DM Sans', sans-serif;
  background: var(--bg);
  color: var(--dark);
  min-height: 100vh;
  padding-bottom: 40px;
}

.topnav {
  background: #111;
  padding: 12px 16px;
  display: flex; align-items: center; gap: 10px;
  position: sticky; top: 0; z-index: 100;
}
.topnav-logo { height: 36px; width: auto; filter: invert(1); }
.topnav-text { font-family: 'Syne', sans-serif; font-size: 11px; font-weight: 700; color: #888; letter-spacing: 2px; text-transform: uppercase; }

.tabs {
  display: flex; background: var(--surface);
  border-bottom: 1px solid var(--border);
  position: sticky; top: 60px; z-index: 99;
}
.tab {
  flex: 1; padding: 12px 8px; text-align: center;
  font-size: 12px; font-weight: 600; color: var(--muted);
  letter-spacing: 0.5px; border-bottom: 2.5px solid transparent;
  cursor: pointer; transition: all 0.15s; user-select: none;
}
.tab.active { color: #111; border-bottom-color: #111; }

.page { display: none; padding: 16px; }
.page.active { display: block; }

.card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 16px; margin-bottom: 14px;
}
.card-title {
  font-family: 'DM Mono', monospace; font-size: 9px; font-weight: 500;
  letter-spacing: 2px; text-transform: uppercase; color: #555;
  margin-bottom: 12px; display: flex; align-items: center; gap: 6px;
}
.card-title::after { content:''; flex:1; height:1px; background:var(--border); }

.field { display: flex; flex-direction: column; gap: 5px; margin-bottom: 10px; }
.field:last-child { margin-bottom: 0; }
.field label { font-size: 11px; font-weight: 600; color: var(--mid); }
.field input, .field textarea {
  font-family: 'DM Sans', sans-serif; font-size: 15px;
  background: var(--bg); border: 1.5px solid var(--border);
  color: var(--dark); padding: 11px 13px; border-radius: 8px;
  outline: none; width: 100%; -webkit-appearance: none;
}
.field input:focus, .field textarea:focus {
  border-color: #111; box-shadow: 0 0 0 3px rgba(0,0,0,0.08);
}
.field textarea { resize: vertical; min-height: 70px; font-size: 14px; line-height: 1.5; }

.tyre-add-row { display: grid; grid-template-columns: 1fr 80px; gap: 8px; margin-bottom: 10px; }
.tyre-add-row input { font-family: 'DM Mono', monospace; font-size: 14px; }

.btn-add {
  width: 100%; background: #111; color: white; border: none;
  font-family: 'Syne', sans-serif; font-size: 14px; font-weight: 700;
  padding: 12px; border-radius: 8px; cursor: pointer; letter-spacing: 0.5px;
  transition: all 0.15s; margin-bottom: 10px;
}
.btn-add:active { opacity: 0.8; transform: scale(0.98); }

.tyre-list { display: flex; flex-direction: column; gap: 8px; }
.tyre-chip {
  display: flex; align-items: center; justify-content: space-between;
  background: var(--bg); border: 1.5px solid var(--border);
  border-radius: 8px; padding: 10px 13px;
  animation: pop 0.18s ease;
}
@keyframes pop { from { opacity:0; transform:scale(0.95); } to { opacity:1; transform:scale(1); } }
.tyre-chip-left { display: flex; align-items: center; gap: 10px; }
.tyre-chip-size { font-family: 'DM Mono', monospace; font-size: 14px; font-weight: 500; }
.tyre-chip-qty {
  background: #111; color: white;
  font-family: 'DM Mono', monospace; font-size: 11px;
  padding: 2px 9px; border-radius: 20px;
}
.tyre-chip-del { background: none; border: none; color: var(--muted); font-size: 18px; cursor: pointer; padding: 0 4px; }

.btn-generate {
  width: 100%; background: #111; color: white; border: none;
  font-family: 'Syne', sans-serif; font-size: 16px; font-weight: 800;
  padding: 16px; border-radius: var(--radius); cursor: pointer;
  margin-top: 4px; transition: all 0.15s;
  display: flex; align-items: center; justify-content: center; gap: 8px;
}
.btn-generate:active { opacity: 0.85; transform: scale(0.98); }

.preview-toolbar {
  display: flex; align-items: center; justify-content: space-between;
  margin-bottom: 16px;
}
.preview-toolbar h2 { font-family: 'Syne', sans-serif; font-size: 16px; font-weight: 800; }
.btn-print {
  background: #111; color: white; border: none;
  font-family: 'Syne', sans-serif; font-size: 13px; font-weight: 700;
  padding: 10px 18px; border-radius: 8px; cursor: pointer;
  display: flex; align-items: center; gap: 6px;
}

.empty-state { text-align: center; padding: 50px 20px; color: var(--muted); }
.empty-icon { font-size: 40px; margin-bottom: 12px; opacity: 0.4; }
.empty-state h3 { font-family: 'Syne', sans-serif; font-size: 16px; font-weight: 700; color: var(--mid); opacity: 0.5; margin-bottom: 6px; }
.empty-state p { font-size: 13px; line-height: 1.6; }

/* ── A6 LABEL — BLACK ONLY FOR THERMAL ── */
.label-wrap { overflow-x: auto; padding-bottom: 8px; }

.a6-label {
  width: 340px; background: white;
  border-radius: 6px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.15);
  overflow: hidden; display: flex; flex-direction: column;
  margin: 0 auto;
  color: #000;
}

/* thick top border */
.lbl-topbar { height: 5px; background: #000; }

.lbl-header {
  padding: 10px 12px 8px;
  display: flex; align-items: center; justify-content: space-between;
  border-bottom: 1.5px solid #000;
}
.lbl-logo-area { display: flex; align-items: center; gap: 8px; }
.lbl-logo-img { height: 38px; width: auto; }
.lbl-our-name {
  font-family: 'Syne', sans-serif; font-size: 11px; font-weight: 800;
  color: #000; letter-spacing: 0.5px; text-transform: uppercase;
  max-width: 90px; line-height: 1.2; border-left: 2px solid #000; padding-left: 8px;
}
.lbl-title-block { text-align: right; }
.lbl-title-big {
  font-family: 'Syne', sans-serif; font-size: 15px; font-weight: 800;
  color: #000; letter-spacing: 1px; text-transform: uppercase; line-height: 1;
}
.lbl-title-small { font-size: 7px; color: #555; letter-spacing: 1.5px; text-transform: uppercase; }

.lbl-dealer {
  padding: 8px 12px; border-bottom: 1px solid #000;
  background: #f5f5f5;
}
.lbl-sec-lbl {
  font-family: 'DM Mono', monospace; font-size: 7px; font-weight: 500;
  letter-spacing: 2px; text-transform: uppercase; color: #333; margin-bottom: 3px;
}
.lbl-dealer-name { font-family: 'Syne', sans-serif; font-size: 13px; font-weight: 700; color: #000; margin-bottom: 2px; }
.lbl-dealer-addr { font-size: 9px; color: #333; line-height: 1.5; margin-bottom: 2px; }
.lbl-dealer-tel { font-family: 'DM Mono', monospace; font-size: 9px; color: #333; }
.lbl-tyre-wrap { padding: 8px 12px; flex: 1; }
.lbl-table { width: 100%; border-collapse: collapse; border: 1px solid #000; }
.lbl-table thead tr { background: #000; }
.lbl-table thead th {
  font-family: 'DM Mono', monospace; font-size: 7.5px; font-weight: 500;
  letter-spacing: 1px; text-transform: uppercase;
  color: #fff; padding: 5px 7px; text-align: left;
}
.lbl-table thead th:last-child { text-align: center; }
.lbl-table tbody tr { border-bottom: 1px solid #ccc; }
.lbl-table tbody tr:last-child { border-bottom: none; }
.lbl-table td { padding: 6px 7px; vertical-align: middle; }

.lbl-size { font-family: 'DM Mono', monospace; font-size: 11px; font-weight: 500; color: #000; white-space: nowrap; }

/* Empty tick boxes only — warehouse staff marks with marker */
.lbl-ticks { display: flex; flex-wrap: wrap; gap: 2px; }
.lbl-tick {
  width: 13px; height: 13px;
  border: 1.5px solid #000; border-radius: 1px;
  display: inline-flex; align-items: center; justify-content: center;
  background: white; flex-shrink: 0;
}

.lbl-qty { text-align: center; font-family: 'DM Mono', monospace; font-size: 13px; font-weight: 700; color: #000; }

.lbl-total-row td { padding: 6px 7px 4px; border-top: 2px solid #000; }
.lbl-total-label { font-family: 'DM Mono', monospace; font-size: 8px; letter-spacing: 2px; text-transform: uppercase; color: #000; font-weight: 700; }
.lbl-total-val { font-family: 'Syne', sans-serif; font-size: 18px; font-weight: 800; color: #000; text-align: right; }

.lbl-footer {
  padding: 7px 12px 10px; display: flex; justify-content: space-between;
  border-top: 1px solid #000;
}
.lbl-footer-field { display: flex; flex-direction: column; gap: 3px; align-items: center; }
.lbl-footer-line { width: 65px; height: 1px; background: #000; margin-bottom: 2px; }
.lbl-footer-lbl { font-family: 'DM Mono', monospace; font-size: 6.5px; color: #555; letter-spacing: 1px; text-transform: uppercase; }

.lbl-bottombar { height: 4px; background: #000; }

/* ── PRINT ── */
@media print {
  @page { size: A6 portrait; margin: 0; }
  body { background: white !important; padding: 0 !important; }
  .topnav, .tabs, #formPage, .preview-toolbar { display: none !important; }
  #previewPage { display: block !important; padding: 0 !important; }
  .label-wrap { overflow: visible !important; }
  .a6-label {
    width: 105mm !important; min-height: 148mm !important;
    box-shadow: none !important; border-radius: 0 !important; margin: 0 !important;
  }
  .btn-print { display: none !important; }
  .lbl-table thead tr { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; }
  .lbl-topbar, .lbl-bottombar { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; }
  .lbl-dealer { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; }
}
</style>
</head>
<body>

<div class="topnav">
  <img class="topnav-logo" src="${LOGO_B64}" alt="RacingZ">
  <span class="topnav-text">Tyre Label System</span>
</div>

<div class="tabs">
  <div class="tab active" onclick="switchTab('form')">📋 填写资料</div>
  <div class="tab" onclick="switchTab('preview')">🏷️ 预览 & 打印</div>
</div>

<div class="page active" id="formPage">

  <div class="card">
    <div class="card-title">Dealer 资料</div>
    <div class="field">
      <label>公司名称</label>
      <input type="text" id="dealerName" placeholder="Dealer Sdn Bhd">
    </div>
    <div class="field">
      <label>地址</label>
      <textarea id="dealerAddr" placeholder="No. 12, Jalan Maju,&#10;47500 Subang Jaya"></textarea>
    </div>
    <div class="field">
      <label>电话号码</label>
      <input type="tel" id="dealerTel" placeholder="+60 12-345 6789">
    </div>
  </div>

  <div class="card">
    <div class="card-title">轮胎尺寸 & 数量</div>
    <div class="tyre-add-row">
      <div class="field" style="margin:0">
        <label>尺寸</label>
        <input type="text" id="newSize" placeholder="205/55R16" style="font-family:'DM Mono',monospace">
      </div>
      <div class="field" style="margin:0">
        <label>数量 (pcs)</label>
        <input type="number" id="newQty" placeholder="10" min="1" max="99" style="font-family:'DM Mono',monospace">
      </div>
    </div>
    <button class="btn-add" onclick="addTyre()">＋ 加入轮胎</button>
    <div class="tyre-list" id="tyreList"></div>
  </div>

  <button class="btn-generate" onclick="generateAndSwitch()">⚡ 生成标签</button>
</div>

<div class="page" id="previewPage">
  <div class="preview-toolbar">
    <h2>标签预览</h2>
    <button class="btn-print" onclick="window.print()">🖨 打印 A6</button>
  </div>
  <div class="label-wrap">
    <div id="labelOutput">
      <div class="empty-state">
        <div class="empty-icon">🏷️</div>
        <h3>还没有标签</h3>
        <p>填好资料后点「生成标签」</p>
      </div>
    </div>
  </div>
</div>

<script>
const LOGO = "${LOGO_B64}";
let tyres = [];

function switchTab(tab) {
  document.querySelectorAll('.tab').forEach((t,i) => {
    t.classList.toggle('active', (tab==='form'&&i===0)||(tab==='preview'&&i===1));
  });
  document.getElementById('formPage').classList.toggle('active', tab==='form');
  document.getElementById('previewPage').classList.toggle('active', tab==='preview');
  if(tab==='preview') window.scrollTo(0,0);
}

function addTyre() {
  const size = document.getElementById('newSize').value.trim().toUpperCase();
  const qty  = parseInt(document.getElementById('newQty').value)||0;
  if(!size){document.getElementById('newSize').focus();return;}
  if(qty<1){document.getElementById('newQty').focus();return;}
  tyres.push({size,qty});
  document.getElementById('newSize').value='';
  document.getElementById('newQty').value='';
  document.getElementById('newSize').focus();
  renderList();
}

document.addEventListener('DOMContentLoaded',()=>{
  document.getElementById('newSize').addEventListener('keydown',e=>{if(e.key==='Enter')document.getElementById('newQty').focus();});
  document.getElementById('newQty').addEventListener('keydown',e=>{if(e.key==='Enter')addTyre();});
});

function removeTyre(i){tyres.splice(i,1);renderList();}

function renderList(){
  document.getElementById('tyreList').innerHTML=tyres.map((t,i)=>\`
    <div class="tyre-chip">
      <div class="tyre-chip-left">
        <span class="tyre-chip-size">\${t.size}</span>
        <span class="tyre-chip-qty">\${t.qty} pcs</span>
      </div>
      <button class="tyre-chip-del" onclick="removeTyre(\${i})">×</button>
    </div>\`).join('');
}

function generateAndSwitch(){generateLabel();switchTab('preview');}

function generateLabel(){
  const dealerName = document.getElementById('dealerName').value.trim();
  const dealerAddr = document.getElementById('dealerAddr').value.trim();
  const dealerTel  = document.getElementById('dealerTel').value.trim();
  const output = document.getElementById('labelOutput');

  if(!dealerName && tyres.length===0){
    output.innerHTML=\`<div class="empty-state"><div class="empty-icon">⚠️</div><h3>请先填写资料</h3><p>填好 Dealer 资料和轮胎尺寸再生成</p></div>\`;
    return;
  }
const total = tyres.reduce((s,t)=>s+t.qty,0);

  const tyreRows = tyres.map(t=>{
    // Empty tick boxes equal to qty — staff marks with marker pen
    const boxes = Array.from({length: Math.min(t.qty, 25)}, ()=>\`<div class="lbl-tick"></div>\`).join('');
    return \`<tr>
      <td class="lbl-size">\${t.size}</td>
      <td><div class="lbl-ticks">\${boxes}</div></td>
      <td class="lbl-qty">\${t.qty}</td>
    </tr>\`;
  }).join('');

  output.innerHTML=\`
    <div class="a6-label">
      <div class="lbl-topbar"></div>
      <div class="lbl-header">
        <div class="lbl-logo-area">
          <img class="lbl-logo-img" src="\${LOGO}" alt="RacingZ">
        </div>
        <div class="lbl-title-block">
          <div class="lbl-title-big">TYRE ORDER</div>
          <div class="lbl-title-small">Dealer Delivery Form</div>
        </div>
      </div>
      <div class="lbl-dealer">
        <div class="lbl-sec-lbl">Dealer Detail</div>
        <div class="lbl-dealer-name">\${dealerName||'—'}</div>
        \${dealerAddr?\`<div class="lbl-dealer-addr">\${dealerAddr.replace(/\n/g,'<br>')}</div>\`:''}
        \${dealerTel?\`<div class="lbl-dealer-tel">Tel: \${dealerTel}</div>\`:''}
      </div>
      <div class="lbl-tyre-wrap">
        <div class="lbl-sec-lbl" style="margin-bottom:5px">Tyre Size & Quantity</div>
        <table class="lbl-table">
          <thead><tr>
            <th style="width:34%">Tyre Size</th>
            <th>QTY (Tick)</th>
            <th style="width:14%;text-align:center">PCS</th>
          </tr></thead>
          <tbody>\${tyreRows}</tbody>
          <tfoot><tr class="lbl-total-row">
            <td class="lbl-total-label">Total Pcs</td>
            <td></td>
            <td class="lbl-total-val">\${total}</td>
          </tr></tfoot>
        </table>
      </div>
      <div class="lbl-footer">
        <div class="lbl-footer-field"><div class="lbl-footer-line"></div><div class="lbl-footer-lbl">Date</div></div>
        <div class="lbl-footer-field"><div class="lbl-footer-line"></div><div class="lbl-footer-lbl">Received By</div></div>
        <div class="lbl-footer-field"><div class="lbl-footer-line"></div><div class="lbl-footer-lbl">Signature</div></div>
      </div>
      <div class="lbl-bottombar"></div>
    </div>\`;
}
</script>
</body>
</html>
HTMLEOF

echo "Done, size: $(wc -c < /tmp/index_new.html) bytes"
