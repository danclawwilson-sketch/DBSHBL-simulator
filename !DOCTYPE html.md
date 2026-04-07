# <!DOCTYPE html>  
  
<html lang="zh-TW">  
<head>  
<meta charset="UTF-8">  
<title>DB-SH vs BL-SH 飛輪模擬器 V6</title>  
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>  
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Noto+Sans+TC:wght@400;700&display=swap" rel="stylesheet">  
<style>  
:root{  
  --bg:#040d1a;  
  --s1:#071428;  
  --s2:#091830;  
  --border:#0d3060;  
  --border2:#164080;  
  --accent:#00ccff;  
  --accent-dim:rgba(0,204,255,0.07);  
  --red:#ff4466;  
  --red-dim:rgba(255,68,102,0.08);  
  --yellow:#ffcc00;  
  --yellow-dim:rgba(255,204,0,0.08);  
  --blue:#00ccff;  
  --blue-dim:rgba(0,204,255,0.08);  
  --purple:#bb88ff;  
  --orange:#ff9944;  
  --orange-dim:rgba(255,153,68,0.08);  
  --text:#4a7aaa;  
  --text2:#99ccee;  
  --text3:#ffffff;  
  --mono:'Share Tech Mono',monospace;  
  --sans:'Noto Sans TC',sans-serif;  
}  
*{box-sizing:border-box;margin:0;padding:0;}  
html{font-size:18px;}  
body{font-family:var(--mono);background:var(--bg);color:var(--text2);min-height:100vh;}  
body::after{content:'';position:fixed;inset:0;pointer-events:none;z-index:9999;  
  background:repeating-linear-gradient(0deg,transparent,transparent 3px,rgba(0,0,0,0.05) 3px,rgba(0,0,0,0.05) 4px);}  
.app{max-width:2200px;margin:0 auto;padding:14px;}  
  
/* HEADER */  
.header{border:1px solid var(–border2);border-top:2px solid var(–accent);background:var(–s1);margin-bottom:2px;}  
.header-top{display:flex;align-items:center;justify-content:center;padding:14px 24px 10px;position:relative;}  
.header-dot{position:absolute;left:24px;width:8px;height:8px;border-radius:50%;background:var(–border2);}  
.header-dot.on{background:var(–accent);box-shadow:0 0 12px var(–accent);animation:blink 2s infinite;}  
@keyframes blink{0%,100%{opacity:1}50%{opacity:.15}}  
.header-status{position:absolute;right:24px;font-size:.75rem;letter-spacing:.12em;color:var(–text);}  
.header-title{font-size:1.3rem;letter-spacing:.2em;color:var(–text3);}  
.header-title .db{color:var(–accent);}  
.header-title .bl{color:var(–orange);}  
.header-sub{text-align:center;font-size:.75rem;letter-spacing:.12em;color:var(–text);padding:0 24px 10px;}  
.header-sub span{margin:0 4px;}  
.header-sub .db{color:var(–accent);}  
.header-sub .bl{color:var(–orange);}  
.header-live{display:flex;align-items:center;justify-content:center;gap:24px;  
border-top:1px solid var(–border);background:rgba(0,255,157,0.02);  
padding:9px 24px;font-size:.75rem;letter-spacing:.1em;color:var(–text);}  
.live-lbl{color:var(–accent);}  
.live-price{font-size:1.5rem;color:var(–accent);}  
.live-time{font-size:.72rem;color:var(–text);}  
.sync-btn{border:1px solid var(–accent);color:var(–accent);background:transparent;  
padding:4px 12px;font-family:var(–mono);font-size:.72rem;letter-spacing:.1em;cursor:pointer;transition:all .15s;}  
.sync-btn:hover{background:var(–accent-dim);}  
  
/* LAYOUT */  
.main-layout{display:grid;grid-template-columns:300px 1fr;gap:2px;margin-top:3px;align-items:start;}  
.dashboard{display:grid;grid-template-columns:1fr 1fr;gap:2px;}  
  
/* ── LEFT PANEL ── */  
.left-panel{  
background:var(–s1);  
border:1px solid var(–border2);  
display:flex;flex-direction:column;  
}  
  
/* Section title — same as shark-fin .stitle */  
.lp-title{  
font-family:var(–mono);font-size:.62rem;  
text-transform:uppercase;letter-spacing:.2em;  
color:var(–accent);  
padding:14px 16px 10px;  
border-bottom:1px solid rgba(0,255,157,0.1);  
display:flex;align-items:center;gap:8px;  
}  
.lp-title::before{content:’//’;opacity:.5;}  
  
/* Field group */  
.lp-group{  
padding:14px 16px;  
border-bottom:1px solid var(–border);  
}  
.lp-group:last-of-type{border-bottom:none;}  
  
/* Two-col row inside group */  
.lp-row{  
display:flex;gap:10px;  
margin-bottom:10px;  
}  
.lp-row:last-child{margin-bottom:0;}  
  
/* Single field */  
.lp-f{  
display:flex;flex-direction:column;gap:6px;flex:1;  
}  
.lp-f label{  
font-family:var(–mono);font-size:.6rem;  
color:var(–text);letter-spacing:.1em;  
text-transform:uppercase;  
}  
.lp-f input,  
.lp-f select{  
background:rgba(0,0,0,0.35);  
border:1px solid var(–border2);  
color:var(–text3);  
padding:8px 10px;  
font-family:var(–mono);font-size:.84rem;  
width:100%;outline:none;  
transition:border-color .15s,box-shadow .15s;  
-webkit-appearance:none;  
}  
.lp-f input:focus,  
.lp-f select:focus{  
border-color:var(–accent);  
box-shadow:0 0 0 1px rgba(0,255,157,0.12);  
color:var(–accent);background:#0d1520;  
}  
.lp-f select{font-size:.72rem;}  
  
/* Pool counter */  
.pool-ctrls{  
display:flex;align-items:center;  
border:1px solid var(–border2);background:rgba(0,0,0,0.35);  
}  
.pool-ctrls button{  
background:transparent;border:none;color:var(–text2);  
padding:7px 12px;font-family:var(–mono);font-size:.9rem;  
cursor:pointer;border-right:1px solid var(–border2);  
transition:color .15s;  
}  
.pool-ctrls button:last-child{border-right:none;border-left:1px solid var(–border2);}  
.pool-ctrls button:hover{color:var(–accent);}  
.pool-ctrls .num{flex:1;text-align:center;color:var(–text3);font-size:.84rem;}  
  
/* Reserve note */  
.rsv-note{  
margin-top:10px;padding:7px 10px;  
font-size:.65rem;color:var(–text);  
background:rgba(255,197,0,0.04);  
border:1px solid rgba(255,197,0,0.12);  
border-left:2px solid var(–yellow);  
}  
.rsv-note span{color:var(–yellow);}  
  
/* Range buttons */  
.range-btns{display:flex;gap:4px;}  
.range-btn{  
flex:1;background:rgba(0,0,0,0.3);border:1px solid var(–border2);  
color:var(–text);font-family:var(–mono);  
font-size:.68rem;padding:7px 4px;  
cursor:pointer;transition:all .15s;text-align:center;  
}  
.range-btn:hover{color:var(–text3);}  
.range-btn.active{color:var(–accent);border-color:var(–accent);background:var(–accent-dim);}  
  
.custom-date{display:none;margin-top:10px;gap:8px;}  
.custom-date.show{display:flex;}  
  
/* Run button */  
.lp-run{padding:0;margin-top:auto;}  
.lp-run .btn-run{  
width:100%;font-size:.78rem;letter-spacing:.18em;  
border:none;padding:14px;  
}  
.btn-compare{  
width:100%;background:transparent;color:var(–blue);border:1px solid var(–blue);  
padding:10px;font-family:var(–mono);font-size:.72rem;letter-spacing:.12em;  
cursor:pointer;transition:all .2s;text-transform:uppercase;  
}  
.btn-compare:hover{background:var(–blue-dim);}  
.btn-compare:disabled{opacity:.3;cursor:not-allowed;}  
  
/* SECTION HEADER */  
  
/* SECTION HEADER */  
.shdr{font-size:.74rem;letter-spacing:.18em;color:var(–text2);padding:11px 14px 10px;border-bottom:1px solid var(–border);}  
.shdr::before{content:’// ’;color:var(–accent);}  
.shdr.bl::before{color:var(–orange);}  
  
/* INPUT ROWS */  
.input-row{display:grid;grid-template-columns:1fr 76px 40px;align-items:center;border-bottom:1px solid var(–border);}  
.input-row label{font-size:.74rem;letter-spacing:.06em;color:var(–text);padding:11px 12px;line-height:1.5;}  
.input-row input,.input-row select{width:100%;background:var(–s2);border:none;border-left:1px solid var(–border);  
color:var(–text3);padding:11px 8px;font-family:var(–mono);font-size:.82rem;outline:none;transition:background .15s;}  
.input-row input:focus,.input-row select:focus{background:#111a28;color:var(–accent);}  
.input-row .unit{font-size:.68rem;color:var(–text);padding:11px 6px;border-left:1px solid var(–border);text-align:center;background:var(–bg);white-space:nowrap;}  
.input-row.no-unit{grid-template-columns:1fr 118px;}  
.input-row.full-sel{grid-template-columns:1fr;}  
.input-row.full-sel select{border-left:none;background:var(–s1);padding:13px 10px;font-size:.82rem;}  
  
/* POOL COUNTER */  
.pool-counter{display:flex;align-items:center;border-bottom:1px solid var(–border);}  
.pool-counter label{flex:1;font-size:.74rem;letter-spacing:.06em;color:var(–text);padding:11px 12px;}  
.pool-counter .ctrls{display:flex;align-items:center;border-left:1px solid var(–border);}  
.pool-counter button{background:var(–bg);border:none;color:var(–text2);padding:11px 13px;font-family:var(–mono);font-size:.9rem;cursor:pointer;transition:color .15s;}  
.pool-counter button:hover{color:var(–accent);}  
.pool-counter .num{min-width:36px;text-align:center;color:var(–text3);font-size:.9rem;padding:11px 0;}  
  
/* RANGE BUTTONS */  
.range-btns{display:flex;border-bottom:1px solid var(–border);}  
.range-btn{flex:1;background:transparent;border:none;border-right:1px solid var(–border);  
color:var(–text);font-family:var(–mono);font-size:.74rem;padding:10px 4px;cursor:pointer;transition:all .15s;text-align:center;}  
.range-btn:last-child{border-right:none;}  
.range-btn:hover{color:var(–text3);}  
.range-btn.active{color:var(–accent);background:var(–accent-dim);}  
.custom-date{display:none;border-bottom:1px solid var(–border);}  
  
/* RESERVE NOTE */  
.reserve-note{padding:9px 12px;font-size:.72rem;color:var(–text);border-bottom:1px solid var(–border);}  
.reserve-note span{color:var(–yellow);}  
  
/* RUN BUTTON */  
.btn-run{width:100%;background:var(–accent);color:#000;border:none;  
padding:11px;font-family:var(–mono);font-weight:700;font-size:.8rem;  
letter-spacing:.14em;cursor:pointer;transition:all .2s;text-transform:uppercase;}  
.btn-run:hover{opacity:.85;}  
.btn-run:disabled{opacity:.3;cursor:not-allowed;}  
  
/* PROGRESS */  
.prog-wrap{padding:10px 12px;border-top:1px solid var(–border);display:none;}  
.prog-bg{background:var(–border);height:2px;}  
.prog-fill{height:100%;background:var(–accent);width:0%;transition:width .1s;box-shadow:0 0 6px var(–accent);}  
.prog-text{font-size:.68rem;color:var(–text);margin-top:5px;text-align:center;letter-spacing:.06em;}  
  
/* STATS GRID - 2x4 */  
.stats-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:2px;background:var(–border);border:1px solid var(–border2);}  
.stat{background:var(–s1);padding:11px 12px;position:relative;overflow:hidden;}  
.stat::before{content:’’;position:absolute;top:0;left:0;right:0;height:1px;  
background:linear-gradient(90deg,var(–accent),transparent 60%);opacity:.4;}  
.col-bl .stat::before{background:linear-gradient(90deg,var(–orange),transparent 60%);}  
.stat-lbl{font-size:.65rem;letter-spacing:.12em;color:var(–text);text-transform:uppercase;margin-bottom:6px;}  
.stat-val{font-size:1.15rem;color:var(–text3);}  
.stat-val.green{color:var(–accent);}  
.stat-val.red{color:var(–red);}  
.stat-val.yellow{color:var(–yellow);}  
.stat-val.orange{color:var(–orange);}  
.stat-sub{font-size:.66rem;color:var(–text);margin-top:3px;}  
  
/* CHART */  
.chart-box{border:1px solid var(–border2);background:var(–s1);}  
.chart-inner{padding:12px 14px;}  
.chart-title{font-size:.68rem;letter-spacing:.12em;color:var(–text);text-transform:uppercase;margin-bottom:10px;}  
.chart-title::before{content:’▸ ’;color:var(–accent);}  
  
/* TABS */  
.tab-row{display:flex;background:var(–s1);border:1px solid var(–border2);border-bottom:none;}  
.tab{flex:1;background:transparent;border:none;border-bottom:2px solid transparent;  
color:var(–text);font-family:var(–mono);font-size:.68rem;letter-spacing:.08em;  
padding:8px 6px;cursor:pointer;transition:all .15s;text-transform:uppercase;border-right:1px solid var(–border);}  
.tab:last-child{border-right:none;}  
.tab:hover{color:var(–text3);}  
.tab.active{color:var(–accent);border-bottom-color:var(–accent);}  
.tab-content{display:none;border:1px solid var(–border2);background:var(–s1);}  
.tab-content.active{display:block;}  
  
/* POOL CARDS */  
.pools-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(120px,1fr));gap:2px;background:var(–border);margin:2px;}  
.pool-card{background:var(–s2);padding:10px 11px;position:relative;}  
.pool-card::before{content:’’;position:absolute;top:0;left:0;width:2px;height:100%;background:var(–border2);}  
.pool-card.active::before{background:var(–accent);}  
.pool-card.bl-active::before{background:var(–orange);}  
.pool-card.eth-mode::before{background:var(–red);}  
.pool-card.idle{opacity:.35;}  
.pc-id{font-size:.65rem;letter-spacing:.12em;color:var(–text);margin-bottom:5px;text-transform:uppercase;}  
.pc-val{font-size:1rem;color:var(–text3);}  
.pc-sub{font-size:.65rem;color:var(–text);margin-top:2px;}  
.pc-badge{display:inline-block;padding:2px 5px;font-size:.62rem;font-weight:700;margin-top:5px;letter-spacing:.08em;border:1px solid;text-transform:uppercase;}  
.badge-db{border-color:var(–accent);color:var(–accent);}  
.badge-bl{border-color:var(–orange);color:var(–orange);}  
.badge-sh{border-color:var(–red);color:var(–red);}  
.badge-idle{border-color:var(–border2);color:var(–text);}  
  
/* LOG */  
.log-search-row{display:flex;gap:2px;padding:8px 10px;border-bottom:1px solid var(–border);}  
.log-search-row input{flex:1;background:var(–s2);border:1px solid var(–border);color:var(–text2);  
padding:5px 8px;font-family:var(–mono);font-size:.72rem;outline:none;}  
.log-search-row input:focus{border-color:var(–accent);}  
.log-search-row button{background:var(–bg);border:1px solid var(–border);color:var(–text);  
padding:5px 10px;font-family:var(–mono);font-size:.7rem;cursor:pointer;}  
.log-scroll{height:340px;overflow-y:auto;padding:4px 10px;}  
.log-scroll::-webkit-scrollbar{width:2px;}  
.log-scroll::-webkit-scrollbar-thumb{background:var(–border2);}  
.log-entry{display:flex;align-items:baseline;gap:7px;padding:3px 0;border-bottom:1px solid rgba(22,32,46,0.6);font-size:.7rem;}  
.log-entry::before{content:’>’;color:var(–border2);flex-shrink:0;font-size:.68rem;}  
.log-date{color:var(–text);flex-shrink:0;min-width:70px;}  
.log-badge{flex-shrink:0;padding:1px 4px;font-size:.62rem;font-weight:700;white-space:nowrap;}  
.lb-s1{background:rgba(0,255,157,0.12);color:var(–accent);}  
.lb-s2{background:rgba(255,197,0,0.12);color:var(–yellow);}  
.lb-s3{background:rgba(0,184,255,0.12);color:var(–blue);}  
.lb-sh1{background:rgba(255,40,85,0.12);color:var(–red);}  
.lb-sh2{background:rgba(176,136,255,0.12);color:var(–purple);}  
.lb-fill{background:rgba(255,197,0,0.06);color:var(–yellow);}  
.lb-warn{background:rgba(255,40,85,0.06);color:var(–red);}  
.lb-bl1{background:rgba(255,140,0,0.15);color:var(–orange);}  
.lb-bl2{background:rgba(0,255,157,0.12);color:var(–accent);}  
.log-msg{color:var(–text2);flex:1;}  
  
/* SUMMARY */  
.sum-wrap{padding:10px;}  
.sum-table{width:100%;border-collapse:collapse;font-size:.75rem;}  
.sum-table th{color:var(–text);text-align:left;padding:8px 10px;border-bottom:1px solid var(–border);font-weight:400;letter-spacing:.08em;}  
.sum-table td{padding:8px 10px;border-bottom:1px solid var(–border);color:var(–text2);}  
.sum-table td.num{color:var(–text3);text-align:right;}  
.sum-table td.green{color:var(–accent);text-align:right;}  
.sum-table td.red{color:var(–red);text-align:right;}  
.sum-table td.yellow{color:var(–yellow);text-align:right;}  
.sum-table td.orange{color:var(–orange);text-align:right;}  
.sum-table tr:hover td{background:rgba(30,45,64,0.3);}  
  
/* DIFF BADGE */  
.diff-badge{display:inline-block;padding:1px 6px;font-size:.65rem;font-weight:700;margin-left:6px;border:1px solid;}  
.diff-pos{border-color:var(–orange);color:var(–orange);}  
.diff-neg{border-color:var(–text);color:var(–text);}  
  
@media(max-width:1400px){.main-layout{grid-template-columns:280px 1fr 1fr;}}  
@media(max-width:1000px){.main-layout{grid-template-columns:1fr;}  
.result-col{min-width:0;}}  
</style>  
  
</head>  
<body>  
<div class="app">  
  
<!-- HEADER -->  
  
<div class="header">  
  <div class="header-top">  
    <div class="header-dot" id="statusDot"></div>  
    <div class="header-title">◌ <span class="db">DB-SH</span> FLYWHEEL SIMULATOR</div>  
    <div class="header-status" id="statusText">READY</div>  
  </div>  
  <div class="header-sub">  
    <span class="db">DISCOUNTED BUY</span> · <span style="color:var(--red)">SELL HIGH</span> · <span style="color:var(--orange)">BUY LOW</span> · <span style="color:var(--text2)">V6.2</span>  
  </div>  
  <div class="header-live">  
    <span class="live-lbl">ETH/USDT</span>  
    <span class="live-price" id="livePrice">--</span>  
    <span class="live-time" id="liveTime">--</span>  
    <button class="sync-btn" onclick="syncPrice()">⟳ SYNC</button>  
  </div>  
</div>  
  
<div class="main-layout">  
  
<!-- LEFT PANEL -->  
  
<div class="left-panel">  
  
  <div class="lp-title">策略參數設定</div>  
  
  <!-- 資金設定 -->  
  
  <div class="lp-group">  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>初始本金</label>  
        <input type="number" id="initCapital" value="10000" oninput="updateReserve()">  
      </div>  
    </div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>每池金額</label>  
        <input type="number" id="poolSize" value="1000" oninput="updateReserve()">  
      </div>  
      <div class="lp-f">  
        <label>水池數量</label>  
        <div class="pool-ctrls">  
          <button onclick="changePool(-1)">－</button>  
          <span class="num" id="poolCountDisplay">5</span>  
          <button onclick="changePool(1)">＋</button>  
        </div>  
      </div>  
    </div>  
    <div class="rsv-note">初始備用金：<span id="reservePreview">--</span> USDT</div>  
  </div>  
  
  <!-- Discounted Buy -->  
  
  <div class="lp-group">  
    <div class="lp-title" style="padding:0 0 8px;margin-bottom:12px;font-size:.58rem;">Discounted Buy</div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>DB 週期天數</label>  
        <input type="number" id="dbDays" value="7" min="1">  
      </div>  
      <div class="lp-f">  
        <label>DB APR %</label>  
        <input type="number" id="dbAPR" value="30">  
      </div>  
    </div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>Target Price −%</label>  
        <input type="number" id="dbTarget" value="4.5" step="0.1">  
      </div>  
      <div class="lp-f">  
        <label>Knock Out +%</label>  
        <input type="number" id="dbKO" value="1" step="0.1">  
      </div>  
    </div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>S2 策略模式</label>  
        <select id="s2Mode">  
          <option value="A">S2-A：買ETH市價賣出，全回水池</option>  
          <option value="B">S2-B：買ETH進SH，備用金補水池</option>  
        </select>  
      </div>  
    </div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>S3 策略模式</label>  
        <select id="s3Mode">  
          <option value="reinvest">S3-繼續投入：本金＋利息繼續跑DB</option>  
          <option value="reserve">S3-超出進備用金：超出部分存備用金</option>  
        </select>  
      </div>  
    </div>  
  </div>  
  
  <!-- Sell High -->  
  
  <div class="lp-group">  
    <div class="lp-title" style="padding:0 0 8px;margin-bottom:12px;font-size:.58rem;">Sell High</div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>SH 週期天數</label>  
        <input type="number" id="shDays" value="1" min="1">  
      </div>  
      <div class="lp-f">  
        <label>SH APR %</label>  
        <input type="number" id="shAPR" value="350">  
      </div>  
    </div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>SH Knock Out +%</label>  
        <input type="number" id="shKO" value="3" step="0.1">  
      </div>  
    </div>  
  </div>  
  
  <!-- Buy Low -->  
  
  <div class="lp-group">  
    <div class="lp-title" style="padding:0 0 8px;margin-bottom:12px;font-size:.58rem;color:var(--orange);">Buy Low</div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>BL 週期天數</label>  
        <input type="number" id="blDays" value="1" min="1">  
      </div>  
      <div class="lp-f">  
        <label>BL APR %</label>  
        <input type="number" id="blAPR" value="333">  
      </div>  
    </div>  
    <div class="lp-row">  
      <div class="lp-f">  
        <label>買入價 (市價 −%)</label>  
        <input type="number" id="blTarget" value="0.56" step="0.01">  
      </div>  
    </div>  
  </div>  
  
  <!-- 回測區間 -->  
  
  <div class="lp-group">  
    <div class="lp-title" style="padding:0 0 8px;margin-bottom:12px;font-size:.58rem;">回測區間</div>  
    <div class="range-btns">  
      <button class="range-btn" onclick="setRange('8d',this)">8D</button>  
      <button class="range-btn" onclick="setRange('15d',this)">15D</button>  
      <button class="range-btn active" onclick="setRange('1y',this)">1Y</button>  
      <button class="range-btn" onclick="setRange('2y',this)">2Y</button>  
      <button class="range-btn" onclick="setRange('custom',this)">自訂</button>  
    </div>  
    <div class="custom-date" id="customDateWrap">  
      <div class="lp-f"><label>開始日期</label><input type="date" id="startDate" value="2025-01-01"></div>  
      <div class="lp-f"><label>結束日期</label><input type="date" id="endDate" value="2026-01-01"></div>  
    </div>  
  </div>  
  
  <!-- 執行 -->  
  
  <div class="lp-run">  
    <button class="btn-run" id="btnRun" onclick="startSimulation()">▶ 開始回測</button>  
    <button class="btn-compare" id="btnMatrix" onclick="startMatrixSim()" style="margin-top:2px;">⊞ DB 矩陣比較</button>  
    <div class="prog-wrap" id="progWrap" style="display:none;padding:8px 14px;">  
      <div class="prog-bg"><div class="prog-fill" id="progBar"></div></div>  
      <div class="prog-text" id="progText">0%</div>  
    </div>  
  </div>  
  
</div><!-- /left-panel -->  
  
<!-- DASHBOARD -->  
  
<div class="dashboard">  
  
  <!-- ── DB 欄 ── -->  
  
  <div class="result-col col-db" style="display:flex;flex-direction:column;gap:2px;">  
    <div style="padding:8px 14px;font-size:.65rem;letter-spacing:.18em;color:var(--accent);background:var(--s1);border:1px solid var(--border2);border-top:2px solid var(--accent);">// DB + SH 策略</div>  
    <div class="stats-grid">  
      <div class="stat"><div class="stat-lbl">總資產淨值</div><div class="stat-val" id="db-total">--</div><div class="stat-sub" id="db-roi">--</div></div>  
      <div class="stat"><div class="stat-lbl">備用金餘額</div><div class="stat-val yellow" id="db-reserve">--</div><div class="stat-sub" id="db-reservePct">--</div></div>  
      <div class="stat"><div class="stat-lbl">ETH 倉位價值</div><div class="stat-val" id="db-eth">--</div><div class="stat-sub" id="db-ethQty">--</div></div>  
      <div class="stat"><div class="stat-lbl">DB 水池總值</div><div class="stat-val green" id="db-pools">--</div><div class="stat-sub" id="db-poolActive">--</div></div>  
      <div class="stat"><div class="stat-lbl">最大回撤 MDD</div><div class="stat-val red" id="db-mdd">--</div><div class="stat-sub">歷史最大回撤</div></div>  
      <div class="stat"><div class="stat-lbl">DB 勝率 (S3)</div><div class="stat-val" id="db-wr">--</div><div class="stat-sub" id="db-wrc">--</div></div>  
      <div class="stat"><div class="stat-lbl">SH 成功賣出</div><div class="stat-val" id="db-shSell">--</div><div class="stat-sub" id="db-shRoll">--</div></div>  
      <div class="stat"><div class="stat-lbl">ETH 均買成本</div><div class="stat-val" id="db-cost">--</div><div class="stat-sub" id="db-costVs">--</div></div>  
    </div>  
    <div class="chart-box"><div class="chart-inner">  
      <div class="chart-title">總資產 vs ETH 走勢</div>  
      <canvas id="db-chartMain" height="90"></canvas>  
    </div></div>  
    <div class="tab-row">  
      <button class="tab active" onclick="switchTab('db','charts')">圖表</button>  
      <button class="tab" onclick="switchTab('db','pools')">水池</button>  
      <button class="tab" onclick="switchTab('db','summary')">統計</button>  
      <button class="tab" onclick="switchTab('db','log')">日誌</button>  
    </div>  
    <div class="tab-content active" id="db-tab-charts">  
      <div class="chart-box"><div class="chart-inner">  
        <div class="chart-title">資產組成（備用金 / DB水池 / ETH倉位）</div>  
        <canvas id="db-chartStack" height="130"></canvas>  
      </div></div>  
    </div>  
    <div class="tab-content" id="db-tab-pools">  
      <div class="shdr" style="margin:0;">PORTFOLIO</div>  
      <div class="pools-grid" id="db-poolsGrid"><div style="padding:16px;color:var(--text);font-size:.75rem;">尚未執行回測</div></div>  
    </div>  
    <div class="tab-content" id="db-tab-summary">  
      <div class="shdr" style="margin:0;">統計總結</div>  
      <div class="sum-wrap"><table class="sum-table" id="db-sumTable"><tr><td colspan="2" style="color:var(--text);font-size:.75rem;">尚未執行回測</td></tr></table></div>  
    </div>  
    <div class="tab-content" id="db-tab-log">  
      <div class="shdr" style="margin:0;" id="db-logTitle">EXECUTION LOG</div>  
      <div class="log-search-row">  
        <input type="text" id="db-logSearch" placeholder="搜尋..." oninput="renderLog('db',this.value)">  
        <button onclick="document.getElementById('db-logSearch').value='';renderLog('db','')">CLR</button>  
      </div>  
      <div class="log-scroll" id="db-logWrap"><div style="color:var(--text);font-size:.75rem;padding:12px;">等待執行...</div></div>  
    </div>  
  </div>  
  
  <!-- ── BL 欄 ── -->  
  
  <div class="result-col col-bl" style="display:flex;flex-direction:column;gap:2px;">  
    <div style="padding:8px 14px;font-size:.65rem;letter-spacing:.18em;color:var(--orange);background:var(--s1);border:1px solid var(--border2);border-top:2px solid var(--orange);">// BL + SH 策略</div>  
    <div class="stats-grid">  
      <div class="stat" style="--accent:var(--orange);"><div class="stat-lbl">總資產淨值</div><div class="stat-val" id="bl-total">--</div><div class="stat-sub" id="bl-roi">--</div></div>  
      <div class="stat"><div class="stat-lbl">備用金餘額</div><div class="stat-val yellow" id="bl-reserve">--</div><div class="stat-sub" id="bl-reservePct">--</div></div>  
      <div class="stat"><div class="stat-lbl">ETH 倉位價值</div><div class="stat-val" id="bl-eth">--</div><div class="stat-sub" id="bl-ethQty">--</div></div>  
      <div class="stat"><div class="stat-lbl">BL 水池總值</div><div class="stat-val" style="color:var(--orange)" id="bl-pools">--</div><div class="stat-sub" id="bl-poolActive">--</div></div>  
      <div class="stat"><div class="stat-lbl">最大回撤 MDD</div><div class="stat-val red" id="bl-mdd">--</div><div class="stat-sub">歷史最大回撤</div></div>  
      <div class="stat"><div class="stat-lbl">BL 成交率</div><div class="stat-val" id="bl-wr">--</div><div class="stat-sub" id="bl-wrc">--</div></div>  
      <div class="stat"><div class="stat-lbl">SH 成功賣出</div><div class="stat-val" id="bl-shSell">--</div><div class="stat-sub" id="bl-shRoll">--</div></div>  
      <div class="stat"><div class="stat-lbl">ETH 均買成本</div><div class="stat-val" id="bl-cost">--</div><div class="stat-sub" id="bl-costVs">--</div></div>  
    </div>  
    <div class="chart-box"><div class="chart-inner">  
      <div class="chart-title">總資產 vs ETH 走勢</div>  
      <canvas id="bl-chartMain" height="90"></canvas>  
    </div></div>  
    <div class="tab-row">  
      <button class="tab active" onclick="switchTab('bl','charts')">圖表</button>  
      <button class="tab" onclick="switchTab('bl','pools')">水池</button>  
      <button class="tab" onclick="switchTab('bl','summary')">統計</button>  
      <button class="tab" onclick="switchTab('bl','log')">日誌</button>  
    </div>  
    <div class="tab-content active" id="bl-tab-charts">  
      <div class="chart-box"><div class="chart-inner">  
        <div class="chart-title">資產組成（備用金 / BL水池 / ETH倉位）</div>  
        <canvas id="bl-chartStack" height="130"></canvas>  
      </div></div>  
    </div>  
    <div class="tab-content" id="bl-tab-pools">  
      <div class="shdr" style="margin:0;">PORTFOLIO</div>  
      <div class="pools-grid" id="bl-poolsGrid"><div style="padding:16px;color:var(--text);font-size:.75rem;">尚未執行回測</div></div>  
    </div>  
    <div class="tab-content" id="bl-tab-summary">  
      <div class="shdr" style="margin:0;">統計總結</div>  
      <div class="sum-wrap"><table class="sum-table" id="bl-sumTable"><tr><td colspan="2" style="color:var(--text);font-size:.75rem;">尚未執行回測</td></tr></table></div>  
    </div>  
    <div class="tab-content" id="bl-tab-log">  
      <div class="shdr" style="margin:0;" id="bl-logTitle">EXECUTION LOG</div>  
      <div class="log-search-row">  
        <input type="text" id="bl-logSearch" placeholder="搜尋..." oninput="renderLog('bl',this.value)">  
        <button onclick="document.getElementById('bl-logSearch').value='';renderLog('bl','')">CLR</button>  
      </div>  
      <div class="log-scroll" id="bl-logWrap"><div style="color:var(--text);font-size:.75rem;padding:12px;">等待執行...</div></div>  
    </div>  
  </div>  
  
</div><!-- /dashboard -->  
</div><!-- /main-layout -->  
  
<!-- MATRIX RESULT -->  
  
<div id="matrixPanel" style="display:none;margin-top:3px;border:1px solid var(--border2);background:var(--s1);"></div>  
</div><!-- /app -->  
  
<script>  
let poolCount=5, _simRange='1y', _allLog={db:[],bl:[]};  
const charts={db:{},bl:{}};  
  
function setRange(v,btn){_simRange=v;document.querySelectorAll('.range-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');document.getElementById('customDateWrap').classList.toggle('show',v==='custom');}  
function changePool(d){const n=poolCount+d;if(n<1||n>20)return;poolCount=n;document.getElementById('poolCountDisplay').textContent=poolCount;updateReserve();}  
function updateReserve(){const cap=parseFloat(document.getElementById('initCapital').value)||10000,ps=parseFloat(document.getElementById('poolSize').value)||1000,res=cap-poolCount*ps,el=document.getElementById('reservePreview');el.textContent=res.toLocaleString('zh-TW',{maximumFractionDigits:0});el.style.color=res<0?'var(--red)':'var(--yellow)';}  
updateReserve();  
  
function switchTab(side,name){  
  const tabs=['charts','pools','summary','log'];  
  const col=document.querySelector('.col-'+side);  
  col.querySelectorAll('.tab').forEach((t,i)=>t.classList.toggle('active',tabs[i]===name));  
  tabs.forEach(n=>{const el=document.getElementById(side+'-tab-'+n);if(el)el.classList.toggle('active',n===name);});  
}  
  
const fmt=(n,d=2)=>(+n).toLocaleString('zh-TW',{minimumFractionDigits:d,maximumFractionDigits:d});  
const fmtU=n=>'$'+fmt(n,0);  
  
async function syncPrice(){const btn=document.querySelector('.sync-btn');btn.textContent='...';try{const r=await fetch('https://api.binance.com/api/v3/ticker/price?symbol=ETHUSDT');const d=await r.json();document.getElementById('livePrice').textContent='$'+parseFloat(d.price).toLocaleString('zh-TW',{minimumFractionDigits:2,maximumFractionDigits:2});document.getElementById('liveTime').textContent=new Date().toLocaleTimeString('zh-TW');}catch(e){document.getElementById('livePrice').textContent='ERROR';}btn.textContent='⟳ SYNC';}  
syncPrice();  
  
// ── MAIN SIMULATION ──  
async function startSimulation(){  
  const btn=document.getElementById('btnRun');  
  btn.disabled=true;  
  document.getElementById('progWrap').style.display='block';  
  document.getElementById('progBar').style.width='0%';  
  document.getElementById('statusDot').classList.add('on');  
  document.getElementById('statusText').textContent='FETCHING';  
  
  let startMs,endMs;const now=Date.now();  
  if(_simRange==='8d'){startMs=now-8*86400000;endMs=now;}  
  else if(_simRange==='15d'){startMs=now-15*86400000;endMs=now;}  
  else if(_simRange==='1y'){startMs=new Date('2025-01-01T00:00:00Z').getTime();endMs=new Date('2026-01-01T00:00:00Z').getTime();}  
  else if(_simRange==='2y'){startMs=new Date('2024-01-01T00:00:00Z').getTime();endMs=new Date('2026-01-01T00:00:00Z').getTime();}  
  else{startMs=new Date(document.getElementById('startDate').value+'T00:00:00Z').getTime();endMs=new Date(document.getElementById('endDate').value+'T00:00:00Z').getTime();}  
  
  let priceData=[];  
  try{  
    document.getElementById('statusText').textContent='DAILY K';  
    document.getElementById('progText').textContent='取得日K...';  
    let rawDaily=[],batchStart=startMs;  
    while(batchStart<endMs){const rd=await fetch(`https://api.binance.com/api/v3/klines?symbol=ETHUSDT&interval=1d&startTime=${batchStart}&endTime=${endMs}&limit=1000`);const chunk=await rd.json();if(!Array.isArray(chunk)||chunk.length===0)break;rawDaily=rawDaily.concat(chunk);batchStart=chunk[chunk.length-1][6]+1;if(chunk.length<1000)break;}  
  
    document.getElementById('statusText').textContent='16:00';  
    const totalHours=Math.ceil((rawDaily[rawDaily.length-1][6]-rawDaily[0][0])/3600000)+24;  
    const batches=Math.ceil(totalHours/1000);  
    const batchPromises=[];  
    for(let b=0;b<batches;b++){const bStart=rawDaily[0][0]+b*1000*3600000;batchPromises.push(fetch(`https://api.binance.com/api/v3/klines?symbol=ETHUSDT&interval=1h&limit=1000&startTime=${bStart}`).then(r=>r.json()).then(raw=>{document.getElementById('progBar').style.width=Math.round((b+1)/batches*35)+'%';return Array.isArray(raw)?raw:[];}));}  
    const allHourly=(await Promise.all(batchPromises)).flat();  
    const p1600={};  
    for(const h of allHourly){const dt=new Date(h[0]);if(dt.getUTCHours()===8){const k=new Date(h[0]+8*3600000).toLocaleDateString('zh-TW');p1600[k]=parseFloat(h[4]);}}  
  
    document.getElementById('progBar').style.width='40%';  
    priceData=rawDaily.map(d=>{const ds=new Date(d[0]).toLocaleDateString('zh-TW'),cl=parseFloat(d[4]);return{date:ds,open:parseFloat(d[1]),high:parseFloat(d[2]),low:parseFloat(d[3]),close:cl,price1600:p1600[ds]||cl};});  
    window.priceData=priceData;  
  }catch(e){alert('無法取得幣安資料: '+e);btn.disabled=false;document.getElementById('statusDot').classList.remove('on');document.getElementById('statusText').textContent='ERROR';return;}  
  
  document.getElementById('statusText').textContent='RUNNING';  
  
  // Common params  
  const initCapital=parseFloat(document.getElementById('initCapital').value);  
  const poolSize=parseFloat(document.getElementById('poolSize').value);  
  const shDays=parseInt(document.getElementById('shDays').value);  
  const shKOPct=parseFloat(document.getElementById('shKO').value)/100;  
  const shAPR=parseFloat(document.getElementById('shAPR').value)/100;  
  
  // ── DB SIM ──  
  document.getElementById('progText').textContent='模擬中...';  
  await new Promise(r=>setTimeout(r,0));  
  document.getElementById('progText').textContent='DB 模擬中...';  
  const dbResult=await runDB(priceData,initCapital,poolSize,shDays,shKOPct,shAPR);  
  document.getElementById('progBar').style.width='70%';  
  document.getElementById('progText').textContent='BL 模擬中...';  
  const blResult=await runBL(priceData,initCapital,poolSize,shDays,shKOPct,shAPR);  
  document.getElementById('progBar').style.width='100%';  
  document.getElementById('progText').textContent='完成';  
  document.getElementById('statusDot').classList.remove('on');  
  document.getElementById('statusText').textContent='DONE';  
  
  renderResults('db',priceData,dbResult,initCapital);  
  renderResults('bl',priceData,blResult,initCapital);  
  btn.disabled=false;  
}  
  
// ── DB ENGINE ──  
async function runDB(priceData,initCapital,poolSize,shDays,shKOPct,shAPR,overrideTarget=null,overrideKO=null){  
  const dbDays=parseInt(document.getElementById('dbDays').value);  
  const dbTargetPct=overrideTarget!==null?overrideTarget:parseFloat(document.getElementById('dbTarget').value)/100;  
  const dbKOPct=overrideKO!==null?overrideKO:parseFloat(document.getElementById('dbKO').value)/100;  
  const dbAPR=parseFloat(document.getElementById('dbAPR').value)/100;  
  const s2Mode=document.getElementById('s2Mode').value;  
  const s3Mode=document.getElementById('s3Mode').value;  
  
  let reserve=initCapital-poolCount*poolSize;  
  let settleCycle=dbDays; // counts down: when 0, it's settle day; P1 starts day 0, settles on day dbDays  
  let pools=Array(poolCount).fill(0).map((_,idx)=>({id:idx+1,balance:poolSize,mode:'wait',startDelay:idx,daysInProduct:0,basePrice:0,targetPrice:0,koPrice:0,refillDelay:0}));  
  let shPos=[],dbS1=0,dbS2=0,dbS3=0,shS1=0,shS2=0,totalEthBought=0,totalEthCostUsdt=0;  
  let peakEquity=initCapital,maxDrawdown=0,logEntries=[];  
  let cDates=[],cEquity=[],cReserve=[],cPools=[],cEthVal=[],cEthQty=[],cEthPrice=[];  
  
  for(let i=0;i<priceData.length;i++){  
    const today=priceData[i],prevClose=i===0?today.open:priceData[i-1].close;  
    if(i%20===0){document.getElementById('progBar').style.width=(40+Math.round(i/priceData.length*30))+'%';await new Promise(r=>setTimeout(r,0));}  
  
    // SH  
    const nextSh=[];  
    for(const pos of shPos){  
      pos.cycleDays--;  
      if(pos.cycleDays>0){nextSh.push(pos);continue;}  
      const intEth=pos.ethAmt*(shAPR/365)*shDays,totalEth=pos.ethAmt+intEth;  
      if(today.price1600>=pos.koPrice){reserve+=totalEth*pos.koPrice;shS2++;logEntries.push({date:today.date,badge:'SH-S2',bc:'lb-sh2',msg:`SH成功 KO@${fmt(pos.koPrice)} +${fmtU(totalEth*pos.koPrice)}→備用金`});}  
      else{const nk=today.price1600*(1+shKOPct);nextSh.push({ethAmt:totalEth,koPrice:nk,cycleDays:shDays});shS1++;logEntries.push({date:today.date,badge:'SH-S1',bc:'lb-sh1',msg:`SH展期 ETH×${fmt(totalEth,4)} 新KO@${fmt(nk)}`});}  
    }  
    shPos=nextSh;  
  
    // DB：共用結算日，各水池TP/KO依申購時價格各自不同  
    settleCycle--;const isSettle=(settleCycle===0);if(isSettle)settleCycle=dbDays;  
    for(const p of pools){  
      if(p.mode==='wait'){if(p.startDelay>0){p.startDelay--;continue;}p.basePrice=today.open;p.targetPrice=today.open*(1-dbTargetPct);p.koPrice=today.open*(1+dbKOPct);p.daysInProduct=0;p.mode='active';logEntries.push({date:today.date,badge:'DB-ON',bc:'lb-s3',msg:`P${p.id}投入 TP@${fmt(p.targetPrice,0)} KO@${fmt(p.koPrice,0)}`});continue;}  
      if(p.mode==='empty'){if(p.refillDelay>0){p.refillDelay--;continue;}if(reserve>=poolSize){reserve-=poolSize;p.balance=poolSize;p.basePrice=today.open;p.targetPrice=today.open*(1-dbTargetPct);p.koPrice=today.open*(1+dbKOPct);p.daysInProduct=0;p.mode='active';logEntries.push({date:today.date,badge:'DB-ON',bc:'lb-s3',msg:`P${p.id}補充 TP@${fmt(p.targetPrice,0)} KO@${fmt(p.koPrice,0)}`});}continue;}  
      p.daysInProduct++;if(!isSettle)continue;  
      const principal=p.balance,actualDays=p.daysInProduct,earned=principal*(dbAPR/365)*actualDays;p.balance+=earned;  
      if(today.price1600>=p.koPrice){  
        dbS3++;logEntries.push({date:today.date,badge:'DB-S3',bc:'lb-s3',msg:`P${p.id} S3 ${actualDays}天 +${fmtU(earned)}`});  
        if(s3Mode==='reserve'){if(p.balance>poolSize){const ov=p.balance-poolSize;reserve+=ov;p.balance=poolSize;}}  
        const sf=poolSize-p.balance;if(sf>0&&reserve>=sf){reserve-=sf;p.balance=poolSize;}else if(sf>0){logEntries.push({date:today.date,badge:'LOW',bc:'lb-warn',msg:`P${p.id}備用金不足`});}  
        if(p.id===1){p.basePrice=today.open;p.targetPrice=today.open*(1-dbTargetPct);p.koPrice=today.open*(1+dbKOPct);p.daysInProduct=0;p.mode='active';}  
        else{reserve+=p.balance;p.balance=0;p.mode='empty';p.refillDelay=Math.max(0,[p.id](http://p.id)-2);}  
      }else if(today.price1600<=p.targetPrice){  
        dbS1++;const ethBought=principal/p.targetPrice;totalEthBought+=ethBought;totalEthCostUsdt+=principal;  
        const nk=today.price1600*(1+shKOPct);shPos.push({ethAmt:ethBought,koPrice:nk,cycleDays:shDays});p.balance=0;  
        logEntries.push({date:today.date,badge:'DB-S1',bc:'lb-s1',msg:`P${[p.id](http://p.id)} S1 ETH×${fmt(ethBought,4)}@${fmt(p.targetPrice,0)}→SH`});  
        if(p.id===1&&reserve>=poolSize){reserve-=poolSize;p.balance=poolSize;p.basePrice=today.open;p.targetPrice=today.open*(1-dbTargetPct);p.koPrice=today.open*(1+dbKOPct);p.daysInProduct=0;p.mode='active';}  
        else{p.mode='empty';p.refillDelay=Math.max(0,[p.id](http://p.id)-2);}  
      }else{  
        dbS2++;const halfUsdt=principal/2,ethBought=halfUsdt/p.targetPrice;  
        if(s2Mode==='A'){const sv=ethBought*today.price1600;p.balance=halfUsdt+sv;logEntries.push({date:today.date,badge:'DB-S2A',bc:'lb-s2',msg:`P${[p.id](http://p.id)} S2-A 回收${fmtU(p.balance)}`});}  
        else{const nk=today.price1600*(1+shKOPct);shPos.push({ethAmt:ethBought,koPrice:nk,cycleDays:shDays});totalEthBought+=ethBought;totalEthCostUsdt+=halfUsdt;p.balance=halfUsdt;logEntries.push({date:today.date,badge:'DB-S2B',bc:'lb-s2',msg:`P${[p.id](http://p.id)} S2-B ETH×${fmt(ethBought,4)}→SH 半倉${fmtU(halfUsdt)}留池`});}  
        if(p.id===1){const sf=poolSize-p.balance;if(sf>0&&reserve>=sf){reserve-=sf;p.balance=poolSize;}p.basePrice=today.open;p.targetPrice=today.open*(1-dbTargetPct);p.koPrice=today.open*(1+dbKOPct);p.daysInProduct=0;p.mode='active';}  
        else{reserve+=p.balance;p.balance=0;p.mode='empty';p.refillDelay=Math.max(0,[p.id](http://p.id)-2);}  
      }  
    }  
    const ethQty=shPos.reduce((s,p)=>s+p.ethAmt,0),ethVal=ethQty*today.close,poolsVal=pools.reduce((s,p)=>s+p.balance,0),equity=reserve+poolsVal+ethVal;  
    if(equity>peakEquity)peakEquity=equity;const dd=peakEquity>0?(peakEquity-equity)/peakEquity*100:0;if(dd>maxDrawdown)maxDrawdown=dd;  
    cDates.push(today.date);cEquity.push(+equity.toFixed(2));cReserve.push(+reserve.toFixed(2));cPools.push(+poolsVal.toFixed(2));cEthVal.push(+ethVal.toFixed(2));cEthQty.push(+ethQty.toFixed(4));cEthPrice.push(+today.close.toFixed(2));  
  }  
  return{cDates,cEquity,cReserve,cPools,cEthVal,cEthQty,cEthPrice,reserve,pools,shPos,maxDrawdown,logEntries,dbS1,dbS2,dbS3,shS1,shS2,totalEthBought,totalEthCostUsdt,type:'db'};  
}  
  
// ── BL ENGINE ──  
async function runBL(priceData,initCapital,poolSize,shDays,shKOPct,shAPR){  
  const blDays=parseInt(document.getElementById('blDays').value);  
  const blTargetPct=parseFloat(document.getElementById('blTarget').value)/100;  
  const blAPR=parseFloat(document.getElementById('blAPR').value)/100;  
  
  let reserve=initCapital-poolCount*poolSize;  
  let pools=Array(poolCount).fill(0).map((_,idx)=>({id:idx+1,balance:poolSize,mode:'wait',startDelay:idx,daysInProduct:0,buyPrice:0,refillDelay:0}));  
  let shPos=[],blHit=0,blMiss=0,shS1=0,shS2=0,totalEthBought=0,totalEthCostUsdt=0;  
  let peakEquity=initCapital,maxDrawdown=0,logEntries=[];  
  let cDates=[],cEquity=[],cReserve=[],cPools=[],cEthVal=[],cEthQty=[],cEthPrice=[];  
  // BL uses per-pool day counter (each pool settles every blDays)  
  // BL: each pool settles independently after blDays  
  
  for(let i=0;i<priceData.length;i++){  
    const today=priceData[i],prevClose=i===0?today.open:priceData[i-1].close;  
    if(i%20===0){document.getElementById('progBar').style.width=(70+Math.round(i/priceData.length*28))+'%';await new Promise(r=>setTimeout(r,0));}  
  
    // SH  
    const nextSh=[];  
    for(const pos of shPos){  
      pos.cycleDays--;  
      if(pos.cycleDays>0){nextSh.push(pos);continue;}  
      const intEth=pos.ethAmt*(shAPR/365)*shDays,totalEth=pos.ethAmt+intEth;  
      if(today.price1600>=pos.koPrice){reserve+=totalEth*pos.koPrice;shS2++;logEntries.push({date:today.date,badge:'SH-S2',bc:'lb-sh2',msg:`SH成功 KO@${fmt(pos.koPrice,0)} +${fmtU(totalEth*pos.koPrice)}→備用金`});}  
      else{const nk=today.price1600*(1+shKOPct);nextSh.push({ethAmt:totalEth,koPrice:nk,cycleDays:shDays});shS1++;logEntries.push({date:today.date,badge:'SH-S1',bc:'lb-sh1',msg:`SH展期 ETH×${fmt(totalEth,4)} 新KO@${fmt(nk,0)}`});}  
    }  
    shPos=nextSh;  
  
    // BL  
    // BL settlement handled per-pool below  
    for(const p of pools){  
      if(p.mode==='wait'){if(p.startDelay>0){p.startDelay--;continue;}p.buyPrice=prevClose*(1-blTargetPct);p.daysInProduct=0;p.mode='active';logEntries.push({date:today.date,badge:'BL-ON',bc:'lb-bl1',msg:`P${[p.id](http://p.id)}投入 買@${fmt(p.buyPrice,0)}`});continue;}  
      if(p.mode==='empty'){if(p.refillDelay>0){p.refillDelay--;continue;}if(reserve>=poolSize){reserve-=poolSize;p.balance=poolSize;p.buyPrice=prevClose*(1-blTargetPct);p.daysInProduct=0;p.mode='active';logEntries.push({date:today.date,badge:'BL-ON',bc:'lb-bl1',msg:`P${[p.id](http://p.id)}補充 買@${fmt(p.buyPrice,0)}`});}continue;}  
      p.daysInProduct++;  
      const isSettle=(p.daysInProduct>=blDays);  
      if(!isSettle)continue;  
      const principal=p.balance,actualDays=p.daysInProduct;  
      const earned=principal*(blAPR/365)*actualDays;  
      // BL 成交判斷：當日low ≤ 買入價（模擬成交）  
      if(today.low<=p.buyPrice){  
        // 成交：全倉買ETH → 進SH  
        blHit++;  
        const ethBought=principal/p.buyPrice;  
        totalEthBought+=ethBought;totalEthCostUsdt+=principal;  
        const nk=today.price1600*(1+shKOPct);  
        shPos.push({ethAmt:ethBought,koPrice:nk,cycleDays:shDays});  
        logEntries.push({date:today.date,badge:'BL-HIT',bc:'lb-bl2',msg:`P${[p.id](http://p.id)} 成交 ETH×${fmt(ethBought,4)}@${fmt(p.buyPrice,0)}→SH KO@${fmt(nk,0)}`});  
        p.balance=0;  
        if(p.id===1&&reserve>=poolSize){reserve-=poolSize;p.balance=poolSize;p.buyPrice=prevClose*(1-blTargetPct);p.daysInProduct=0;p.mode='active';}  
        else{p.mode='empty';p.refillDelay=Math.max(0,[p.id](http://p.id)-2);}  
      }else{  
        // 未成交：本金+利息回池繼續跑  
        blMiss++;  
        p.balance=principal+earned;  
        logEntries.push({date:today.date,badge:'BL-MISS',bc:'lb-bl1',msg:`P${[p.id](http://p.id)} 未成交 本金+息${fmtU(p.balance)} 繼續 買入@${fmt(p.buyPrice,0)}`});  
        // 更新買入價為最新市價  
        if(p.id===1){const sf=poolSize-p.balance;if(sf>0&&reserve>=sf){reserve-=sf;p.balance=poolSize;}p.buyPrice=prevClose*(1-blTargetPct);p.daysInProduct=0;p.mode='active';}  
        else{reserve+=p.balance;p.balance=0;p.mode='empty';p.refillDelay=Math.max(0,[p.id](http://p.id)-2);}  
      }  
    }  
    const ethQty=shPos.reduce((s,p)=>s+p.ethAmt,0),ethVal=ethQty*today.close,poolsVal=pools.reduce((s,p)=>s+p.balance,0),equity=reserve+poolsVal+ethVal;  
    if(equity>peakEquity)peakEquity=equity;const dd=peakEquity>0?(peakEquity-equity)/peakEquity*100:0;if(dd>maxDrawdown)maxDrawdown=dd;  
    cDates.push(today.date);cEquity.push(+equity.toFixed(2));cReserve.push(+reserve.toFixed(2));cPools.push(+poolsVal.toFixed(2));cEthVal.push(+ethVal.toFixed(2));cEthQty.push(+ethQty.toFixed(4));cEthPrice.push(+today.close.toFixed(2));  
  }  
  return{cDates,cEquity,cReserve,cPools,cEthVal,cEthQty,cEthPrice,reserve,pools,shPos,maxDrawdown,logEntries,blHit,blMiss,shS1,shS2,totalEthBought,totalEthCostUsdt,type:'bl'};  
}  
  
// ── RENDER RESULTS ──  
function renderResults(side,priceData,res,initCapital){  
  const{cDates,cEquity,cReserve,cPools,cEthVal,cEthQty,cEthPrice,reserve,pools,shPos,maxDrawdown,logEntries}=res;  
  const lastPrice=priceData[priceData.length-1].close;  
  const equity=cEquity[cEquity.length-1];  
  const roi=(equity-initCapital)/initCapital*100;  
  const ann=roi/priceData.length*365;  
  const ethQty=cEthQty[cEthQty.length-1];  
  const ethVal=cEthVal[cEthVal.length-1];  
  const poolsVal=cPools[cPools.length-1];  
  const avgCost=res.totalEthBought>0?res.totalEthCostUsdt/res.totalEthBought:0;  
  const roiC=roi>=0?'green':'red';  
  
  // Stats  
  document.getElementById(side+'-total').textContent=fmtU(equity);  
  document.getElementById(side+'-total').className='stat-val '+roiC;  
  document.getElementById(side+'-roi').textContent='ROI '+(roi>=0?'+':'')+fmt(roi)+'% | 年化'+(ann>=0?'+':'')+fmt(ann)+'%';  
  document.getElementById(side+'-reserve').textContent=fmtU(reserve);  
  document.getElementById(side+'-reservePct').textContent='佔總資產 '+fmt(equity>0?reserve/equity*100:0)+'%';  
  document.getElementById(side+'-eth').textContent=fmtU(ethVal);  
  document.getElementById(side+'-ethQty').textContent='ETH 數量 '+fmt(ethQty,4);  
  document.getElementById(side+'-pools').textContent=fmtU(poolsVal);  
  document.getElementById(side+'-poolActive').textContent='活躍 '+pools.filter(p=>p.mode==='active').length+'/'+pools.length;  
  document.getElementById(side+'-mdd').textContent='-'+fmt(maxDrawdown)+'%';  
  document.getElementById(side+'-cost').textContent=avgCost>0?'$'+fmt(avgCost):'--';  
  document.getElementById(side+'-costVs').textContent=avgCost>0?'vs市價 '+(lastPrice>=avgCost?'+':'')+fmt((lastPrice-avgCost)/avgCost*100)+'%':'';  
  
  if(side==='db'){  
    const tDB=res.dbS1+res.dbS2+res.dbS3;  
    document.getElementById('db-wr').textContent=tDB>0?fmt(res.dbS3/tDB*100)+'%':'--';  
    document.getElementById('db-wrc').textContent=`S3 ${res.dbS3}次 / 共 ${tDB}次`;  
    document.getElementById('db-shSell').textContent=res.shS2+'次';  
    document.getElementById('db-shRoll').textContent='展期 '+res.shS1+'次';  
  }else{  
    const tBL=res.blHit+res.blMiss;  
    document.getElementById('bl-wr').textContent=tBL>0?fmt(res.blHit/tBL*100)+'%':'--';  
    document.getElementById('bl-wrc').textContent=`成交 ${res.blHit}次 / 共 ${tBL}次`;  
    document.getElementById('bl-shSell').textContent=res.shS2+'次';  
    document.getElementById('bl-shRoll').textContent='展期 '+res.shS1+'次';  
  }  
  
  // Charts  
  const skip=Math.max(1,Math.floor(cDates.length/80));  
  const sl=arr=>arr.filter((_,i)=>i%skip===0);  
  const d=sl(cDates),tc='rgba(184,208,223,0.7)',gc='rgba(22,32,46,0.8)';  
  const base={responsive:true,animation:false,plugins:{legend:{display:false}},scales:{x:{ticks:{color:tc,font:{size:9,family:'Share Tech Mono'},maxRotation:45,minRotation:45},grid:{color:gc}},y:{ticks:{color:tc,font:{size:9,family:'Share Tech Mono'}},grid:{color:gc}}}};  
  const mainColor=side==='db'?'#00ff9d':'#ff8c00';  
  const poolColor=side==='db'?'rgba(0,255,157,0.06)':'rgba(255,140,0,0.06)';  
  const poolBorder=side==='db'?'#00ff9d':'#ff8c00';  
  
  Object.values(charts[side]).forEach(c=>c&&c.destroy());  
  const maxEq=Math.max(...cEquity),maxEp=Math.max(...cEthPrice),scale=maxEq/maxEp;  
  charts[side].main=new Chart(document.getElementById(side+'-chartMain').getContext('2d'),{  
    type:'line',data:{labels:d,datasets:[  
      {label:'總資產',data:sl(cEquity),borderColor:mainColor,borderWidth:2,pointRadius:0,fill:false,tension:.3},  
      {label:'ETH(縮放)',data:sl(cEthPrice).map(v=>+(v*scale).toFixed(2)),borderColor:'rgba(0,184,255,0.3)',borderWidth:1.5,pointRadius:0,fill:false,tension:.3,borderDash:[5,4]},  
      {label:'本金',data:Array(d.length).fill(initCapital),borderColor:'rgba(255,255,255,0.08)',borderWidth:1,pointRadius:0,fill:false,borderDash:[2,6]},  
    ]},options:{...base,plugins:{legend:{display:true,labels:{color:'rgba(106,144,168,0.8)',font:{size:9,family:'Share Tech Mono'},boxWidth:10}}}}  
  });  
  if(document.getElementById(side+'-chartEth')){  
    charts[side].eth=new Chart(document.getElementById(side+'-chartEth').getContext('2d'),{  
      type:'line',data:{labels:d,datasets:[{data:sl(cEthQty),borderColor:'#ff2855',borderWidth:1.5,fill:true,backgroundColor:'rgba(255,40,85,0.07)',pointRadius:0,tension:.3}]},options:base  
    });  
  }  
  charts[side].stack=new Chart(document.getElementById(side+'-chartStack').getContext('2d'),{  
    type:'line',data:{labels:d,datasets:[  
      {label:'備用金',data:sl(cReserve),borderColor:'#ffc500',borderWidth:1.5,fill:true,backgroundColor:'rgba(255,197,0,0.06)',pointRadius:0,tension:.3},  
      {label:side==='db'?'DB水池':'BL水池',data:sl(cPools),borderColor:poolBorder,borderWidth:1.5,fill:true,backgroundColor:poolColor,pointRadius:0,tension:.3},  
      {label:'ETH倉位',data:sl(cEthVal),borderColor:'#ff2855',borderWidth:1.5,fill:true,backgroundColor:'rgba(255,40,85,0.06)',pointRadius:0,tension:.3},  
    ]},options:{...base,plugins:{legend:{display:true,labels:{color:'rgba(106,144,168,0.8)',font:{size:9,family:'Share Tech Mono'},boxWidth:9}}}}  
  });  
  
  // Pool status  
  buildPoolStatus(side,pools,shPos,lastPrice,reserve);  
  
  // Log  
  _allLog[side]=logEntries;  
  document.getElementById(side+'-logTitle').textContent=`// LOG（${logEntries.length} 筆）`;  
  renderLog(side,'');  
  
  // Summary  
  buildSummary(side,{initCapital,simDays:priceData.length,finalEquity:equity,reserveFinal:reserve,ethValFinal:ethVal,poolsValFinal:poolsVal,maxDrawdown,avgCost,firstClose:priceData[0].close,lastClose:lastPrice,...res});  
}  
  
function renderLog(side,filter){  
  const entries=_allLog[side]||[];  
  if(!entries.length){document.getElementById(side+'-logWrap').innerHTML='<div style="color:var(--text);font-size:.72rem;padding:10px;">無記錄</div>';return;}  
  const kw=filter.trim().toUpperCase();  
  const show=kw?entries.filter(e=>e.badge.includes(kw)||e.msg.toUpperCase().includes(kw)||e.date.includes(kw)):entries;  
  document.getElementById(side+'-logTitle').textContent=kw?`// LOG 篩選 "${filter}" → ${show.length} 筆`:`// LOG（${entries.length} 筆）`;  
  document.getElementById(side+'-logWrap').innerHTML=show.map(e=>`<div class="log-entry"><span class="log-date">${e.date}</span><span class="log-badge ${e.bc}">${e.badge}</span><span class="log-msg">${e.msg}</span></div>`).join('');  
}  
  
function buildPoolStatus(side,pools,shPos,lastPrice,finalReserve){  
  const grid=document.getElementById(side+'-poolsGrid');grid.innerHTML='';  
  const isDB=side==='db';  
  pools.forEach(p=>{  
    const c=document.createElement('div');  
    let cls='pool-card',badge,bc='badge-idle',val='--',sub='--';  
    if(p.mode==='active'){cls+=' '+(isDB?'active':'bl-active');badge=isDB?'DB':'BL';bc=isDB?'badge-db':'badge-bl';val='$'+p.balance.toLocaleString(undefined,{maximumFractionDigits:0});sub=isDB?`${p.daysInProduct}天 TP:$${fmt(p.targetPrice,0)}`:`${p.daysInProduct}天 買@$${fmt(p.buyPrice,0)}`;}  
    else if(p.mode==='empty'){cls+=' eth-mode';badge='WAIT→SH';bc='badge-sh';val='等補充';sub=p.refillDelay>0?`${p.refillDelay}天後`:finalReserve>=1000?'備用金足':'等備用金';}  
    else{badge='WAIT';val='$'+p.balance.toLocaleString(undefined,{maximumFractionDigits:0});sub=`${p.startDelay}天後啟動`;}  
    c.className=cls;  
    c.innerHTML=`<div class="pc-id">P${[p.id](http://p.id)}</div><div class="pc-val">${val}</div><div class="pc-sub">${sub}</div><span class="pc-badge ${bc}">${badge}</span>`;  
    grid.appendChild(c);  
  });  
  const c=document.createElement('div');  
  if(shPos.length>0){const te=shPos.reduce((s,p)=>s+p.ethAmt,0),tv=Math.round(te*lastPrice),ak=shPos.reduce((s,p)=>s+p.koPrice,0)/shPos.length;c.className='pool-card eth-mode';c.innerHTML=`<div class="pc-id">SH 池</div><div class="pc-val">≈$${tv.toLocaleString()}</div><div class="pc-sub">ETH×${fmt(te,4)} | ${shPos.length}筆</div><div class="pc-sub">均KO $${fmt(ak,0)}</div><span class="pc-badge badge-sh">SELL HIGH</span>`;}  
  else{c.className='pool-card idle';c.innerHTML=`<div class="pc-id">SH 池</div><div class="pc-val" style="color:var(--text)">無倉位</div><div class="pc-sub">--</div><span class="pc-badge badge-idle">IDLE</span>`;}  
  grid.appendChild(c);  
}  
  
function buildSummary(side,s){  
  const roi=(s.finalEquity-s.initCapital)/s.initCapital*100;  
  const ann=roi/s.simDays*365;  
  const ethChg=(s.lastClose-s.firstClose)/s.firstClose*100;  
  const isDB=side==='db';  
  let rows=[  
    ['回測天數',s.simDays+'天',''],['初始本金',fmtU(s.initCapital),''],  
    ['最終資產',fmtU(s.finalEquity),roi>=0?'green':'red'],  
    ['總報酬率',(roi>=0?'+':'')+fmt(roi)+'%',roi>=0?'green':'red'],  
    ['年化報酬率',(ann>=0?'+':'')+fmt(ann)+'%',ann>=0?'green':'red'],  
    ['最大回撤 MDD','-'+fmt(s.maxDrawdown)+'%','red'],['──','',''],  
    ['ETH 期初價','$'+fmt(s.firstClose,0),''],['ETH 期末價','$'+fmt(s.lastClose,0),''],  
    ['ETH 漲跌幅',(ethChg>=0?'+':'')+fmt(ethChg)+'%',ethChg>=0?'green':'red'],  
    ['ETH 累計買入',fmt(s.totalEthBought,4)+' ETH',''],  
    ['ETH 均買成本',s.avgCost>0?'$'+fmt(s.avgCost):'--',''],  
    ['ETH 倉位現值',fmtU(s.ethValFinal),''],['──','',''],  
  ];  
  if(isDB){  
    const tDB=s.dbS1+s.dbS2+s.dbS3;  
    rows=rows.concat([  
      ['DB 總結算',tDB+'次',''],  
      ['DB S1 (全倉買ETH)',s.dbS1+'次',''],  
      ['DB S2 (半倉買ETH)',s.dbS2+'次','yellow'],  
      ['DB S3 (KO拿息)',s.dbS3+'次','green'],  
      ['DB S3 勝率',tDB>0?fmt(s.dbS3/tDB*100)+'%':'--','green'],  
    ]);  
  }else{  
    const tBL=s.blHit+s.blMiss;  
    rows=rows.concat([  
      ['BL 總結算',tBL+'次',''],  
      ['BL 成交 (買ETH→SH)',s.blHit+'次','orange'],  
      ['BL 未成交 (拿息回池)',s.blMiss+'次','green'],  
      ['BL 成交率',tBL>0?fmt(s.blHit/tBL*100)+'%':'--','orange'],  
    ]);  
  }  
  rows=rows.concat([  
    ['──','',''],  
    ['SH 成功賣出',s.shS2+'次','green'],  
    ['SH 展期',s.shS1+'次',''],['──','',''],  
    ['備用金餘額',fmtU(s.reserveFinal),'yellow'],  
    [isDB?'DB水池總值':'BL水池總值',fmtU(s.poolsValFinal),''],  
  ]);  
  document.getElementById(side+'-sumTable').innerHTML=  
    `<tr><th>指標</th><th style="text-align:right">數值</th></tr>`+  
    rows.map(([k,v,c])=>{  
      if(k==='──')return`<tr><td colspan="2" style="padding:3px 8px;border-bottom:1px solid var(--border)"></td></tr>`;  
      return`<tr><td>${k}</td><td class="num ${c}">${v}</td></tr>`;  
    }).join('');  
}  
  
// ── MATRIX SIMULATION ──  
async function startMatrixSim(){  
  const priceData=window.priceData;  
  if(!priceData||!priceData.length){alert('請先執行一次回測載入價格資料');return;}  
  
  const btn=document.getElementById('btnMatrix');  
  btn.disabled=true;btn.textContent='⊞ 計算中...';  
  document.getElementById('progWrap').style.display='block';  
  
  const initCapital=parseFloat(document.getElementById('initCapital').value)||10000;  
  const poolSize=parseFloat(document.getElementById('poolSize').value)||1000;  
  const nPools=poolCount;  
  const dbDays=parseInt(document.getElementById('dbDays').value)||4;  
  const dbAPR=parseFloat(document.getElementById('dbAPR').value)/100;  
  const shDays=parseInt(document.getElementById('shDays').value)||1;  
  const shKOPct=parseFloat(document.getElementById('shKO').value)/100;  
  const shAPR=parseFloat(document.getElementById('shAPR').value)/100;  
  const s2Mode=document.getElementById('s2Mode').value;  
  const s3Mode=document.getElementById('s3Mode').value;  
  
  const targets=[1,1.5,2,2.5,3,3.5,4,4.5,5];   // Target Price -%  
  
  // KO: 0.5 steps from 0.5 up to user's KO value  
  const userKO=parseFloat(document.getElementById('dbKO').value)||1;  
  const kos=[];  
  for(let v=0.5;v<userKO;v=Math.round((v+0.5)*10)/10) kos.push(Math.round(v*10)/10);  
  kos.push(Math.round(userKO*10)/10);  
  
  const results=[];  
  let done=0,total=targets.length*kos.length;  
  
  for(const tPct of targets){  
    const row=[];  
    for(const koPct of kos){  
      await new Promise(r=>setTimeout(r,0));  
      const res=await runDB(priceData,initCapital,poolSize,shDays,shKOPct,shAPR,tPct/100,koPct/100);  
      const finalEquity=res.cEquity[res.cEquity.length-1];  
      const roi=(finalEquity-initCapital)/initCapital*100;  
      row.push({finalEquity,roi,maxDrawdown:res.maxDrawdown,dbS1:res.dbS1,dbS2:res.dbS2,dbS3:res.dbS3,shS1:res.shS1,shS2:res.shS2});  
      done++;  
      document.getElementById('progText').textContent=`矩陣計算 ${done}/${total}...`;  
    }  
    results.push(row);  
  }  
  
  renderMatrix(results,targets,kos,initCapital,nPools,dbDays);  
  btn.disabled=false;btn.textContent='⊞ DB 矩陣比較';  
  document.getElementById('progWrap').style.display='none';  
  document.getElementById('progText').textContent='完成';  
}  
  
function runDBCore(priceData,initCapital,poolSize,nPools,dbDays,dbTargetPct,dbKOPct,dbAPR,shDays,shKOPct,shAPR,s2Mode,s3Mode){  
  let reserve=initCapital-nPools*poolSize;  
  if(reserve<0)return null;  
  let settleCycle=dbDays+1;  
  let pools=Array(nPools).fill(0).map((_,idx)=>({id:idx+1,balance:poolSize,mode:'wait',startDelay:idx,daysInProduct:0,targetPrice:0,koPrice:0,refillDelay:0}));  
  let shPos=[],dbS1=0,dbS2=0,dbS3=0,shS1=0,shS2=0,totalEthBought=0,totalEthCostUsdt=0;  
  let peakEquity=initCapital,maxDrawdown=0,cEquity=[];  
  
  for(let i=0;i<priceData.length;i++){  
    const today=priceData[i],prevClose=i===0?today.open:priceData[i-1].close;  
    // SH  
    const nextSh=[];  
    for(const pos of shPos){  
      pos.cycleDays--;  
      if(pos.cycleDays>0){nextSh.push(pos);continue;}  
      const intEth=pos.ethAmt*(shAPR/365)*shDays,totalEth=pos.ethAmt+intEth;  
      if(today.price1600>=pos.koPrice){reserve+=totalEth*pos.koPrice;shS2++;}  
      else{nextSh.push({ethAmt:totalEth,koPrice:today.price1600*(1+shKOPct),cycleDays:shDays});shS1++;}  
    }  
    shPos=nextSh;  
    // DB  
    settleCycle--;const isSettle=(settleCycle===0);if(isSettle)settleCycle=dbDays;  
    for(const p of pools){  
      if(p.mode==='wait'){if(p.startDelay>0){p.startDelay--;continue;}p.targetPrice=prevClose*(1-dbTargetPct);p.koPrice=prevClose*(1+dbKOPct);p.daysInProduct=0;p.mode='active';continue;}  
      if(p.mode==='empty'){if(p.refillDelay>0){p.refillDelay--;continue;}if(reserve>=poolSize){reserve-=poolSize;p.balance=poolSize;p.targetPrice=prevClose*(1-dbTargetPct);p.koPrice=prevClose*(1+dbKOPct);p.daysInProduct=0;p.mode='active';}continue;}  
      p.daysInProduct++;if(!isSettle)continue;  
      const principal=p.balance,actualDays=p.daysInProduct,earned=principal*(dbAPR/365)*actualDays;p.balance+=earned;  
      if(today.price1600>=p.koPrice){  
        dbS3++;  
        if(s3Mode==='reserve'&&p.balance>poolSize){reserve+=p.balance-poolSize;p.balance=poolSize;}  
        const sf=poolSize-p.balance;if(sf>0&&reserve>=sf){reserve-=sf;p.balance=poolSize;}  
        if(p.id===1){p.targetPrice=prevClose*(1-dbTargetPct);p.koPrice=prevClose*(1+dbKOPct);p.daysInProduct=0;p.mode='active';}  
        else{reserve+=p.balance;p.balance=0;p.mode='empty';p.refillDelay=Math.max(0,[p.id](http://p.id)-2);}  
      }else if(today.price1600<=p.targetPrice){  
        dbS1++;const ethBought=principal/p.targetPrice;totalEthBought+=ethBought;totalEthCostUsdt+=principal;  
        shPos.push({ethAmt:ethBought,koPrice:today.price1600*(1+shKOPct),cycleDays:shDays});p.balance=0;  
        if(p.id===1&&reserve>=poolSize){reserve-=poolSize;p.balance=poolSize;p.targetPrice=prevClose*(1-dbTargetPct);p.koPrice=prevClose*(1+dbKOPct);p.daysInProduct=0;p.mode='active';}  
        else{p.mode='empty';p.refillDelay=Math.max(0,[p.id](http://p.id)-2);}  
      }else{  
        dbS2++;const halfUsdt=principal/2,ethBought=halfUsdt/p.targetPrice;  
        if(s2Mode==='A'){p.balance=halfUsdt+ethBought*today.price1600;}  
        else{shPos.push({ethAmt:ethBought,koPrice:today.price1600*(1+shKOPct),cycleDays:shDays});totalEthBought+=ethBought;totalEthCostUsdt+=halfUsdt;p.balance=halfUsdt;}  
        if(p.id===1){const sf=poolSize-p.balance;if(sf>0&&reserve>=sf){reserve-=sf;p.balance=poolSize;}p.targetPrice=prevClose*(1-dbTargetPct);p.koPrice=prevClose*(1+dbKOPct);p.daysInProduct=0;p.mode='active';}  
        else{reserve+=p.balance;p.balance=0;p.mode='empty';p.refillDelay=Math.max(0,p.id-2);}  
      }  
    }  
    const ethQty=shPos.reduce((s,p)=>s+p.ethAmt,0),ethVal=ethQty*today.close,poolsVal=pools.reduce((s,p)=>s+p.balance,0),equity=reserve+poolsVal+ethVal;  
    if(equity>peakEquity)peakEquity=equity;const dd=peakEquity>0?(peakEquity-equity)/peakEquity*100:0;if(dd>maxDrawdown)maxDrawdown=dd;  
    cEquity.push(equity);  
  }  
  const finalEquity=cEquity[cEquity.length-1];  
  const roi=(finalEquity-initCapital)/initCapital*100;  
  return{finalEquity,roi,maxDrawdown,dbS1,dbS2,dbS3,shS1,shS2};  
}  
  
function renderMatrix(results,targets,kos,initCapital,nPools,dbDays){  
  const panel=document.getElementById('matrixPanel');  
  
  const allRoi=results.flat().filter(r=>r).map(r=>r.roi);  
  const minRoi=Math.min(...allRoi),maxRoi=Math.max(...allRoi);  
  
  function roiColor(roi){  
    const t=(roi-minRoi)/(maxRoi-minRoi||1);  
    if(t<0.5){const g=Math.round(t*2*180);return`rgba(255,${g},0,0.22)`;}  
    else{const r=Math.round((1-(t-0.5)*2)*255);return`rgba(${r},200,0,0.22)`;}  
  }  
  
  let h=`<div style="padding:10px 16px 8px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;">`;  
  h+=`<span style="font-size:.65rem;letter-spacing:.2em;color:var(--accent);font-family:var(--mono);">// DB 矩陣回測 — 總資產結果</span>`;  
  h+=`<span style="font-size:.6rem;color:var(--text3);font-family:var(--mono);">初始本金 $${initCapital.toLocaleString()} · 水池 ${nPools} 個 · DB 週期 ${dbDays} 天</span>`;  
  h+=`<span style="font-size:.6rem;color:var(--text);font-family:var(--mono);">色彩：綠=高報酬　紅=低報酬</span>`;  
  h+=`</div>`;  
  
  h+=`<div style="overflow:auto;padding:12px 16px;">`;  
  h+=`<table style="border-collapse:collapse;font-family:var(--mono);font-size:.72rem;white-space:nowrap;">`;  
  
  // Header: Target % across columns  
  h+=`<tr>`;  
  h+=`<th style="padding:7px 14px;color:var(--text);text-align:center;border-bottom:1px solid var(--border2);border-right:1px solid var(--border2);background:var(--s2);">KO → / Target ↓</th>`;  
  for(const t of targets){  
    h+=`<th style="padding:7px 14px;color:var(--accent);text-align:center;border-bottom:1px solid var(--border2);border-right:1px solid var(--border);background:var(--s2);">−${t}%</th>`;  
  }  
  h+=`</tr>`;  
  
  // Rows: KO %  
  for(let ki=0;ki<kos.length;ki++){  
    h+=`<tr>`;  
    h+=`<td style="padding:7px 14px;color:var(--text2);text-align:center;border-right:1px solid var(--border2);border-bottom:1px solid var(--border);background:var(--s2);font-weight:700;">+${kos[ki]}%</td>`;  
    for(let ti=0;ti<targets.length;ti++){  
      const r=results[ti][ki];  
      if(!r){h+=`<td style="padding:7px 14px;text-align:center;border-right:1px solid var(--border);border-bottom:1px solid var(--border);color:var(--text);">—</td>`;continue;}  
      const bg=roiColor(r.roi);  
      const roiC=r.roi>=0?'var(--accent)':'var(--red)';  
      const tDB=r.dbS1+r.dbS2+r.dbS3;  
      const s3wr=tDB>0?Math.round(r.dbS3/tDB*100):0;  
      h+=`<td style="padding:7px 14px;text-align:right;background:${bg};border-right:1px solid var(--border);border-bottom:1px solid var(--border);">`;  
      h+=`<div style="color:var(--text3);font-weight:700;">$${Math.round(r.finalEquity).toLocaleString()}</div>`;  
      h+=`<div style="color:${roiC};font-size:.65rem;">${r.roi>=0?'+':''}${r.roi.toFixed(1)}%</div>`;  
      h+=`<div style="color:var(--text);font-size:.6rem;">MDD -${r.maxDrawdown.toFixed(1)}% | S3 ${s3wr}%</div>`;  
      h+=`</td>`;  
    }  
    h+=`</tr>`;  
  }  
  h+=`</table></div>`;  
  
  panel.innerHTML=h;  
  panel.style.display='block';  
  panel.scrollIntoView({behavior:'smooth',block:'nearest'});  
}  
  
</script>  
  
</body>  
</html>  
