<!doctype html>
<html lang="ur" dir="rtl">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Peacock Blue — Download ZIP</title>
  <style>
    body{font-family:Segoe UI, Tahoma, sans-serif;display:flex;align-items:center;justify-content:center;height:100vh;background:#f1f4ff;margin:0}
    .box{background:#fff;padding:24px;border-radius:10px;box-shadow:0 8px 30px rgba(60,70,120,0.08);max-width:720px;width:90%;text-align:center}
    button{background:linear-gradient(90deg,#4f5bff,#7a6bff);color:#fff;border:0;padding:12px 18px;border-radius:8px;font-size:16px;cursor:pointer}
    pre{max-height:220px;overflow:auto;text-align:left;background:#f7f9ff;padding:12px;border-radius:8px}
    .small{font-size:13px;color:#555;margin-top:8px}
  </style>
</head>
<body>
  <div class="box">
    <h2>Peacock Blue — Download Project ZIP</h2>
    <p class="small">نیچے بٹن سے تمام فائلیں ایک ZIP میں ڈاؤن لوڈ کریں۔ پھر extract کریں اور index.html کھولیں یا server کے لیے Node انسٹال کرکے server.js چلائیں۔</p>
    <div style="margin:16px 0">
      <button id="dl">Download ZIP</button>
    </div>
    <div class="small">اگر ZIP خود بخود نہیں بنتی تو بتا دیں، میں متبادل طریقہ بتا دوں گا۔</div>
  </div>

  <!-- JSZip + FileSaver via CDN -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>

  <script>
    // === Put file contents here ===
    const files = {
      "index.html": `<!doctype html>
<html lang="ur" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Peacock Blue - Projects Dashboard</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.rtl.min.css" rel="stylesheet">
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
  <style>
    :root{--peacock-1:#4f5bff;--peacock-2:#7a6bff;--accent:#ff6b6b;--card-radius:12px}
    body{background:linear-gradient(180deg,#5f6ce6 0%, #7c7ff3 100%);font-family: "Segoe UI", Tahoma, sans-serif;padding:20px}
    .app-header{background:linear-gradient(90deg,var(--peacock-1),var(--peacock-2));color:#fff;padding:18px;border-radius:12px;box-shadow:0 6px 18px rgba(20,20,60,0.15)}
    .panel{background:#fff;border-radius:12px;padding:18px;box-shadow:0 6px 18px rgba(20,20,60,0.08)}
    .btn-gradient{background: linear-gradient(90deg,#6b66ff,#9e63d9);color:#fff}
    .project-card{border-left:6px solid rgba(100,100,255,0.12);margin-bottom:18px}
    .logo-placeholder{width:48px;height:48px;border-radius:8px;background:#fff3;padding:6px;display:inline-block;vertical-align:middle}
    @media print{.no-print{display:none !important}; body{background:#fff}}
  </style>
</head>
<body>
  <div class="container-fluid">
    <div class="app-header mb-4 d-flex justify-content-between align-items-center">
      <div>
        <div class="d-flex align-items-center">
          <img src="logo.png" alt="logo" onerror="this.style.display='none'" style="height:48px;margin-left:12px;border-radius:6px;background:#fff;padding:6px">
          <div>
            <h3 class="mb-0">Peacock Blue Air Conditioner & Refrigerators</h3>
            <small>Professional Project Management System - Dubai Edition (AED)</small>
          </div>
        </div>
      </div>
      <div class="no-print text-end">
        <button class="btn btn-light me-2" onclick="printDashboard()"><i class="fa-solid fa-print"></i> پرنٹ</button>
        <button class="btn btn-light me-2" id="exportAllPdfBtn"><i class="fa-regular fa-file-pdf"></i> سیو PDF</button>
        <button class="btn btn-outline-light me-2" onclick="exportAllExcel()"><i class="fa-solid fa-file-excel"></i> ایکسل برآمد</button>
        <button class="btn btn-outline-light" onclick="downloadBackup()"><i class="fa-solid fa-download"></i> بیک اپ (JSON)</button>
      </div>
    </div>

    <div class="row mb-3">
      <div class="col-md-8">
        <div class="panel d-flex align-items-center gap-3">
          <div>
            <small class="text-muted">Server Mode</small><br/>
            <select id="apiMode" class="form-select" onchange="toggleMode()">
              <option value="">Local (localStorage)</option>
              <option value="http://localhost:3000">Use Server API (http://localhost:3000)</option>
            </select>
          </div>
          <div class="ms-auto text-end">
            <small class="text-muted">Search projects</small>
            <div class="d-flex gap-2">
              <input id="searchInput" class="form-control" placeholder="پراجیکٹ تلاش کریں..." oninput="renderProjects()" />
              <button class="btn btn-outline-secondary" onclick="clearSearch()">ختم</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="row card-stats mb-4">
      <div class="col-6 col-md-3">
        <div class="panel text-center">
          <small class="text-muted">کل پروجیکٹس</small>
          <h3 id="statTotalProjects">0</h3>
        </div>
      </div>
      <div class="col-6 col-md-3">
        <div class="panel text-center">
          <small class="text-muted">مجموعی وصولی</small>
          <h3 id="statTotalReceived">AED 0</h3>
        </div>
      </div>
      <div class="col-6 col-md-3">
        <div class="panel text-center">
          <small class="text-muted">کل اخراجات</small>
          <h3 id="statTotalExpenses">AED 0</h3>
        </div>
      </div>
      <div class="col-6 col-md-3">
        <div class="panel text-center">
          <small class="text-muted">کل منافع / نقصان</small>
          <h3 id="statTotalProfit">AED 0</h3>
        </div>
      </div>
    </div>

    <div class="row g-4">
      <div class="col-lg-4">
        <div class="panel">
          <h5><i class="fa-solid fa-plus" style="color:var(--peacock-1);margin-left:8px"></i> Add New Project</h5><hr/>
          <form id="projectForm" onsubmit="event.preventDefault(); addProject();">
            <div class="mb-3">
              <label class="form-label">Project Name *</label>
              <input id="pName" required type="text" class="form-control" placeholder="e.g., AC Installation" />
            </div>
            <div class="mb-3">
              <label class="form-label">Company Name</label>
              <input id="pCompany" type="text" class="form-control" placeholder="Company / Client name" />
            </div>
            <div class="mb-3">
              <label class="form-label">Project Description</label>
              <textarea id="pDesc" class="form-control" rows="3" placeholder="Project details, location, client info..."></textarea>
            </div>
            <div class="mb-3">
              <label class="form-label">Total Project Value (AED) *</label>
              <input id="pValue" required type="number" min="0" class="form-control" placeholder="Enter total project value" />
            </div>
            <div class="d-grid">
              <button class="btn btn-gradient">✓ Add Project</button>
            </div>
            <div class="mt-3">
              <input type="file" id="restoreFile" accept=".json" style="display:none" onchange="restoreFromFile(event)">
              <button type="button" class="btn btn-outline-secondary mt-2" onclick="document.getElementById('restoreFile').click()">Restore Backup (JSON)</button>
            </div>
          </form>
        </div>
      </div>

      <div class="col-lg-8">
        <div class="panel">
          <div class="d-flex justify-content-between align-items-center mb-3">
            <h5><i class="fa-regular fa-clipboard" style="margin-left:8px"></i> Projects List</h5>
            <div class="text-muted small">کل پروجیکٹس دکھائے جا رہے ہیں</div>
          </div>
          <div id="projectsContainer"></div>
        </div>
      </div>
    </div>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

  <script>
    let API_BASE = '';
    const STORAGE_KEY = 'peacock_projects_v1';
    function uid(){ return Date.now().toString(36) + Math.random().toString(36).slice(2,7); }
    function fmtAED(n){ return 'AED ' + Number(n||0).toLocaleString(); }
    function escapeHtml(s){ if(!s) return ''; return String(s).replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;'); }

    async function loadData(){
      if(API_BASE){
        try{
          const res = await fetch(API_BASE + '/projects');
          if(!res.ok) throw new Error('API error');
          return await res.json();
        }catch(e){
          alert('Server error: ' + e.message + '. Reverting to local data.');
          API_BASE = ''; document.getElementById('apiMode').value = '';
        }
      }
      try{
        const raw = localStorage.getItem(STORAGE_KEY);
        if(!raw){
          const sample = [
            { id: uid(), name:'ABDULLAH NAJEM AL HAMOODI', company:'UNITED ELECTRONICS', description:'AC maintenance', totalValue:1, entries: [{id:uid
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              
              # mm-g
PEACOCK BLUE AIR CONDITIONR 23
