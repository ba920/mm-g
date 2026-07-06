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
              
              
              <!doctype html>
<html lang="ur" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Project - Peacock Blue</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.rtl.min.css" rel="stylesheet">
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
  <style>
    :root{--peacock-1:#4f5bff;--card-radius:12px}
    body{background:#f4f6fb;font-family: "Segoe UI", Tahoma, sans-serif;padding:20px}
    .header{background:linear-gradient(90deg,var(--peacock-1),#7a6bff);color:#fff;padding:18px;border-radius:12px;margin-bottom:18px}
    .panel{background:#fff;border-radius:12px;padding:18px;box-shadow:0 6px 18px rgba(20,20,60,0.06)}
    @media print{.no-print{display:none !important} body{padding:0}}
  </style>
</head>
<body>
  <div class="container-fluid">
    <div class="header d-flex justify-content-between align-items-center">
      <div>
        <h3 id="projName">Project</h3>
        <small id="projCompany">Company</small>
      </div>
      <div class="no-print">
        <button class="btn btn-light me-2" onclick="window.open('index.html','_self')"><i class="fa-solid fa-arrow-right-from-bracket"></i> واپس ڈیشبورڈ</button>
        <button class="btn btn-light me-2" onclick="printProject()"><i class="fa-solid fa-print"></i> پرنٹ</button>
        <button class="btn btn-outline-light me-2" onclick="exportProjectExcel()"><i class="fa-solid fa-file-excel"></i> ایکسل ایکسپورٹ</button>
        <button class="btn btn-danger" id="exportPdfBtn"><i class="fa-regular fa-file-pdf"></i> سیو PDF</button>
      </div>
    </div>

    <div class="row g-4">
      <div class="col-md-8">
        <div class="panel mb-3">
          <h5>Project Summary</h5><hr/>
          <div class="d-flex flex-wrap gap-3">
            <div class="p-3 bg-light rounded" style="min-width:140px"><small class="text-muted">TOTAL VALUE</small><div><strong id="p_total">AED 0</strong></div></div>
            <div class="p-3 bg-light rounded" style="min-width:140px"><small class="text-muted">RECEIVED</small><div><strong id="p_received">AED 0</strong></div></div>
            <div class="p-3 bg-light rounded" style="min-width:140px"><small class="text-muted">PENDING</small><div><strong id="p_pending">AED 0</strong></div></div>
            <div class="p-3 bg-light rounded" style="min-width:140px"><small class="text-muted">EXPENSES</small><div><strong id="p_expenses">AED 0</strong></div></div>
            <div class="p-3 bg-light rounded" style="min-width:140px"><small class="text-muted">PROFIT/LOSS</small><div><strong id="p_profit" style="color:#1e9b57">AED 0</strong></div></div>
          </div>
          <p class="mt-3" id="p_desc"></p>
        </div>

        <div class="panel mb-3">
          <div class="d-flex justify-content-between align-items-center">
            <h5>Daily Records <small id="entriesCount" class="text-muted"></small></h5>
            <div class="no-print">
              <button class="btn btn-success" onclick="showEntryForm()"><i class="fa-solid fa-plus"></i> + Add Daily Entry</button>
            </div>
          </div>
          <hr />
          <div id="entriesContainer">
            <div class="text-muted">No daily entries yet.</div>
          </div>
        </div>
      </div>

      <div class="col-md-4">
        <div class="panel mb-3">
          <h6>Project Details</h6><hr />
          <div><strong>Company:</strong> <span id="detCompany"></span></div>
          <div><strong>Project Value:</strong> <span id="detValue"></span></div>
          <div><strong>Description:</strong> <div id="detDesc"></div></div>
        </div>

        <div class="panel no-print">
          <h6>Quick Actions</h6><hr/>
          <button class="btn btn-outline-secondary w-100 mb-2" onclick="editProjectName()"><i class="fa-solid fa-edit"></i> Edit Project</button>
          <button class="btn btn-outline-danger w-100" onclick="removeProject()"><i class="fa-solid fa-trash"></i> Delete Project</button>
        </div>
      </div>
    </div>
  </div>

  <!-- Entry form modal -->
  <div id="entryForm" style="position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,0.5);z-index:9999">
    <div style="background:#fff;border-radius:10px;padding:18px;width:480px;max-width:95%">
      <h5>Add Daily Entry</h5>
      <form id="dailyForm" onsubmit="event.preventDefault(); addDailyEntry();">
        <div class="mb-2"><label class="form-label">Date</label><input id="eDate" type="date" class="form-control" required /></div>
        <div class="mb-2"><label class="form-label">Members (comma separated)</label><input id="eMembers" class="form-control" placeholder="Ali, Omar" /></div>
        <div class="mb-2"><label class="form-label">Expenses (AED)</label><input id="eExpense" type="number" min="0" class="form-control" value="0" /></div>
        <div class="mb-2"><label class="form-label">Received (AED)</label><input id="eReceived" type="number" min="0" class="form-control" value="0" /></div>
        <div class="mb-2"><label class="form-label">Description</label><textarea id="eDesc" rows="2" class="form-control"></textarea></div>
        <div class="d-flex justify-content-end gap-2"><button type="button" class="btn btn-outline-secondary" onclick="hideEntryForm()">Cancel</button><button class="btn btn-primary">Save Entry</button></div>
      </form>
    </div>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

  <script>
    let API_BASE = ''; // اگر آپ server استعمال کرنا چاہیں تو 'http://localhost:3000' سیٹ کریں
    const STORAGE_KEY = 'peacock_projects_v1';
    function loadDataLocal(){ try{ return JSON.parse(localStorage.getItem(STORAGE_KEY)||'[]'); }catch{return []} }
    function saveDataLocal(d){ localStorage.setItem(STORAGE_KEY, JSON.stringify(d)); }
    function uid(){ return Date.now().toString(36)+Math.random().toString(36).slice(2,7); }
    function fmtAED(n){ return 'AED ' + Number(n||0).toLocaleString(); }
    function escapeHtml(s){ if(!s) return ''; return s.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;'); }

    const params = new URLSearchParams(location.search);
    const projId = params.get('id') || location.hash.slice(1);

    let projects = [];
    let project = null;

    async function init(){
      if(API_BASE){
        try{
          const res = await fetch(API_BASE + '/projects/' + encodeURIComponent(projId));
          if(!res.ok) throw new Error('Project not found on server');
          project = await res.json();
          projects = null;
        }catch(e){
          alert('Server error: ' + e.message);
          project = null;
        }
      } else {
        projects = loadDataLocal();
        project = projects.find(p=>p.id===projId);
      }
      if(!project){
        document.body.innerHTML = '<div class="container"><div class="alert alert-warning">پروجیکٹ مل نہیں سکا۔ براہِ مہربانی ڈیشبورڈ سے پروجیکٹ کھولیں۔ <a href="index.html">واپس جائیں</a></div></div>';
        return;
      }
      renderProject();
    }

    function renderProject(){
      document.getElementById('projName').textContent = project.name;
      document.getElementById('projCompany').textContent = project.company||'';
      document.getElementById('p_desc').textContent = project.description||'';
      document.getElementById('detCompany').textContent = project.company||'—';
      document.getElementById('detValue').textContent = fmtAED(project.totalValue);
      document.getElementById('detDesc').textContent = project.description||'—';
      const entries = project.entries || [];
      const received = entries.reduce((s,e)=>s+Number(e.received||0),0);
      const expenses = entries.reduce((s,e)=>s+Number(e.expense||0),0);
      const pending = Number(project.totalValue||0) - received;
      const profit = received - expenses;
      document.getElementById('p_total').textContent = fmtAED(project.totalValue);
      document.getElementById('p_received').textContent = fmtAED(received);
      document.getElementById('p_expenses').textContent = fmtAED(expenses);
      document.getElementById('p_pending').textContent = fmtAED(pending);
      const profitEl = document.getElementById('p_profit');
      profitEl.textContent = fmtAED(profit);
      profitEl.style.color = profit < 0 ? '#e74c3c' : '#1e9b57';

      const container = document.getElementById('entriesContainer');
      container.innerHTML = '';
      if(entries.length === 0){
        container.innerHTML = '<div class="text-muted">No daily entries yet. Click "Add Daily Entry" to start tracking.</div>';
      }else{
        document.getElementById('entriesCount').textContent = '(' + entries.length + ' entries)';
        entries.slice().reverse().forEach(e=>{
          const card = document.createElement('div');
          card.className = 'mb-3 p-3 border rounded';
          card.innerHTML = `
            <div class="d-flex justify-content-between align-items-start">
              <div>
                <strong>${e.date}</strong> <small class="text-muted">— ${e.members || ''}</small>
                <div class="mt-2">${escapeHtml(e.desc||'')}</div>
              </div>
              <div class="text-end no-print">
                <div><small class="text-muted">Received</small><div><strong>${fmtAED(e.received)}</strong></div></div>
                <div class="mt-2"><small class="text-muted">Expenses</small><div><strong>${fmtAED(e.expense)}</strong></div></div>
                <div class="mt-3"><button class="btn btn-sm btn-outline-danger" onclick="deleteEntry('${e.id}')">Delete</button></div>
              </div>
            </div>
          `;
          container.appendChild(card);
        });
      }
    }

    function showEntryForm(){ document.getElementById('entryForm').style.display = 'flex'; document.getElementById('dailyForm').reset(); document.getElementById('eDate').valueAsDate = new Date(); }
    function hideEntryForm(){ document.getElementById('entryForm').style.display = 'none'; }

    async function addDailyEntry(){
      const date = document.getElementById('eDate').value;
      const members = document.getElementById('eMembers').value;
      const expense = Number(document.getElementById('eExpense').value || 0);
      const received = Number(document.getElementById('eReceived').value || 0);
      const desc = document.getElementById('eDesc').value;
      if(!date) return alert('تاریخ ضروری ہے');
      const entry = { id: uid(), date, members, expense, received, desc };
      if(API_BASE){
        // POST to server
        const res = await fetch(API_BASE + '/projects/' + encodeURIComponent(project.id) + '/entries', { method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify(entry) });
        if(!res.ok) return alert('Server error adding entry');
        project = await res.json();
      } else {
        project.entries = project.entries || [];
        project.entries.push(entry);
        // save to local
        projects = projects.map(p => p.id === project.id ? project : p);
        saveDataLocal(projects);
      }
      hideEntryForm(); renderProject();
    }

    async function deleteEntry(id){
      if(!confirm('کیا آپ اس اندراج کو حذف کرنا چاہتے ہیں؟')) return;
      if(API_BASE){
        await fetch(API_BASE + '/projects/' + encodeURIComponent(project.id) + '/entries/' + encodeURIComponent(id), { method:'DELETE' });
        project = await (await fetch(API_BASE + '/projects/' + encodeURIComponent(project.id))).json();
      } else {
        project.entries = (project.entries||[]).filter(e=>e.id!==id);
        projects = projects.map(p=>p.id===project.id?project:p);
        saveDataLocal(projects);
      }
      renderProject();
    }

    function printProject(){ window.print(); }
    document.getElementById('exportPdfBtn').addEventListener('click', function(){ html2pdf().set({margin:0.3,filename:(project.name||'project')+'.pdf',image:{type:'jpeg',quality:0.98},html2canvas:{scale:2}}).from(document.body).save(); });

    // Export entries of this project to Excel
    function exportProjectExcel(){
      const wb = XLSX.utils.book_new();
      const projInfo = [{ id: project.id, name: project.name, company: project.company||'', description: project.description||'', totalValue: project.totalValue }];
      XLSX.utils.book_append_sheet(wb, XLSX.utils.json_to_sheet(projInfo), 'Project');
      const entries = (project.entries||[]).map(e=>({ date:e.date, members:e.members, expense:e.expense, received:e.received, desc:e.desc }));
      XLSX.utils.book_append_sheet(wb, XLSX.utils.json_to_sheet(entries), 'Entries');
      XLSX.writeFile(wb, (project.name||'project') + '.xlsx');
    }

    async function editProjectName(){
      const newName = prompt('Edit project name', project.name);
      if(newName === null) return;
      project.name = newName.trim() || project.name;
      if(API_BASE){
        await fetch(API_BASE + '/projects/' + encodeURIComponent(project.id), { method:'PUT', headers:{'Content-Type':'application/json'}, body: JSON.stringify(project) });
        project = await (await fetch(API_BASE + '/projects/' + encodeURIComponent(project.id))).json();
      } else {
        projects = projects.map(p=>p.id===project.id?project:p);
        saveDataLocal(projects);
      }
      renderProject();
    }

    async function removeProject(){
      if(!confirm('پراجیکٹ مستقل حذف ہو جائے گا۔ جاری رکھیں؟')) return;
      if(API_BASE){
        await fetch(API_BASE + '/projects/' + encodeURIComponent(project.id), { method:'DELETE' });
        window.location.href = 'index.html';
      } else {
        projects = projects.filter(p=>p.id!==project.id);
        saveDataLocal(projects);
        window.location.href = 'index.html';
      }
    }

    init();
  </script>
</body>
</html>
              
              
              
              
              
              
              
              
              
              
              
              
              
              # mm-g
PEACOCK BLUE AIR CONDITIONR 23
