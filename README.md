# ALOMARI
MASSAR
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>برنامج مسار - ترقيات ضباط الصف</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;800&display=swap" rel="stylesheet">
<style>
:root{--main:#1f4e78;--accent:#e2b372;--bg:#f4f7fa;--shadow:0 4px 20px rgba(0,0,0,.06);}
*{box-sizing:border-box;}
body{background:var(--bg);font-family:'Cairo',sans-serif;min-height:100vh;display:flex;flex-direction:column;overflow-x:hidden;}

/* Navbar */
.main-nav{background:linear-gradient(135deg,#0f2b46,#1f4e78);color:#fff;padding:15px 20px;box-shadow:0 4px 15px rgba(0,0,0,0.1);}
.brand-title{font-size:1.4rem;font-weight:800;margin:0;}
.brand-sub{font-size:0.8rem;color:var(--accent);font-weight:600;letter-spacing:1px;}

/* Main Portal Layout */
.portal-wrap{max-width:1000px;margin:40px auto;padding:0 20px;flex:1;width:100%;}
.card-custom{background:#fff;border-radius:20px;box-shadow:var(--shadow);border:none;padding:30px;margin-bottom:25px;position:relative;overflow:hidden;}
.card-custom::before{content:'';position:absolute;top:0;right:0;width:6px;height:100%;background:var(--main);}

/* Dashboard Components */
.stat-box{background:#f8fbfd;border:1px solid #e2eaf2;border-radius:14px;padding:15px;text-align:center;}
.stat-box h3{color:var(--main);font-weight:800;margin-bottom:2px;}
.stat-box p{font-size:12px;color:#6c757d;margin:0;font-weight:700;}

/* Points Badge */
.points-badge{background:#eef3fa;color:var(--main);padding:8px 15px;border-radius:30px;font-weight:700;font-size:0.9rem;display:inline-block;}
.rank-number{font-size:3rem;font-weight:900;color:var(--accent);text-shadow:2px 2px 0px rgba(0,0,0,0.05);line-height:1;}

/* Sidebar Admin */
.admin-panel{display:none;}
.table-responsive{border-radius:12px;overflow:hidden;}
</style>
</head>
<body>

<nav class="main-nav d-flex justify-content-between align-items-center flex-wrap gap-2">
  <div>
    <h1 class="brand-title"><i class="fa-solid fa-layer-group text-warning me-2"></i>برنامج (مسار) الإلكتروني</h1>
    <div class="brand-sub">نظام ترقيات ضباط الصف - جناح الإدارة</div>
  </div>
  <div class="d-flex gap-2">
    <button id="btnPortalView" class="btn btn-sm btn-light fw-bold" onclick="switchView('portal')"><i class="fa-solid fa-user"></i> بوابة الاستعلام</button>
    <button id="btnAdminLoginView" class="btn btn-sm btn-outline-light fw-bold" onclick="switchView('admin-login')"><i class="fa-solid fa-lock"></i> لوحة التحكم</button>
    <button id="btnAdminDashboardView" class="btn btn-sm btn-warning fw-bold d-none" onclick="switchView('admin-dashboard')"><i class="fa-solid fa-gauge"></i> الإدارة المتقدمة</button>
    <button id="btnLogout" class="btn btn-sm btn-danger fw-bold d-none" onclick="logoutAdmin()"><i class="fa-solid fa-power-off"></i> خروج</button>
  </div>
</nav>

<div class="portal-wrap">

  <div id="view-portal" class="view-section">
    <div class="card-custom text-center py-5">
      <div class="mx-auto mb-4" style="width:80px;height:80px;background:#eef3fa;border-radius:50%;display:flex;align-items:center;justify-content:center;color:var(--main);font-size:2rem;">
        <i class="fa-solid fa-id-card-clip"></i>
      </div>
      <h3 class="fw-bold mb-2">الاستعلام المباشر عن مسار الترقية</h3>
      <p class="text-muted small mb-4">أدخل رقمك العسكري للاطلاع على ترتيبك الحالي في السرا وتفاصيل نقاطك بكل شفافية</p>
      
      <div class="row justify-content-center">
        <div class="col-md-7">
          <form onsubmit="event.preventDefault(); searchMilitaryId();">
            <div class="input-group input-group-lg mb-3 shadow-sm">
              <input id="searchMilId" type="text" class="form-control text-center fw-bold" placeholder="أدخل الرقم العسكري" required>
              <button class="btn text-white fw-bold" style="background:var(--main);" type="submit"><i class="fa-solid fa-magnifying-glass"></i> استعلام</button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <div id="searchResultCard" class="card-custom d-none animate__animated animate__fadeIn">
      <div class="row align-items-center">
        <div class="col-md-8 border-start-0">
          <h4 class="fw-bold text-primary mb-3" id="resName">-</h4>
          <div class="row g-3 mb-3">
            <div class="col-6 col-sm-4">
              <small class="text-muted d-block">الرقم العسكري</small>
              <strong id="resMilId">-</strong>
            </div>
            <div class="col-6 col-sm-4">
              <small class="text-muted d-block">الرتبة الحالية</small>
              <strong id="resRank">-</strong>
            </div>
            <div class="col-6 col-sm-4">
              <small class="text-muted d-block">الرتبة المستحقة</small>
              <strong id="resNextRank" class="text-success">-</strong>
            </div>
          </div>
          
          <hr>
          
          <h5 class="fw-bold mb-3 text-secondary"><i class="fa-solid fa-chart-pie me-1"></i> تفاصيل معايير الترقية</h5>
          <div class="row g-2">
            <div class="col-6 col-sm-3"><div class="stat-box"><h3 id="pSeniority">0</h3><p>نقاط الأقدمية</p></div></div>
            <div class="col-6 col-sm-3"><div class="stat-box"><h3 id="pEdu">0</h3><p>المؤهلات العلمية</p></div></div>
            <div class="col-6 col-sm-3"><div class="stat-box"><h3 id="pCourses">0</h3><p>الدورات التدريبية</p></div></div>
            <div class="col-6 col-sm-3"><div class="stat-box"><h3 id="pPerf">0</h3><p>تقييم الأداء</p></div></div>
          </div>
        </div>
        <div class="col-md-4 text-center mt-4 mt-md-0 bg-light py-4 rounded">
          <p class="text-muted fw-bold mb-1">ترتيبك الحالي في السرا</p>
          <div class="rank-number" id="resQueueNum">-</div>
          <div class="points-badge mt-2">إجمالي النقاط: <span id="resTotalPoints">0</span></div>
        </div>
      </div>
    </div>
  </div>

  <div id="view-admin-login" class="view-section d-none">
    <div class="card-custom mx-auto" style="max-width:450px;">
      <h4 class="fw-bold mb-3 text-center"><i class="fa-solid fa-lock-open text-warning me-2"></i>دخول جناح الإدارة</h4>
      <form onsubmit="event.preventDefault(); loginAdmin();">
        <div class="mb-3">
          <label class="form-label small fw-bold">رمز المسؤول</label>
          <input id="adminUser" type="text" class="form-control" placeholder="أدخل اسم المستخدم الافتراضي: admin" required>
        </div>
        <div class="mb-3">
          <label class="form-label small fw-bold">الرقم السري</label>
          <input id="adminPass" type="password" class="form-control" placeholder="أدخل الرقم السري الافتراضي: 123" required>
        </div>
        <button type="submit" class="btn text-white fw-bold w-100 py-2" style="background:var(--main);">تسجيل الدخول</button>
      </form>
    </div>
  </div>

  <div id="view-admin-dashboard" class="view-section d-none">
    <div class="d-flex justify-content-between align-items-center mb-4 flex-wrap gap-2">
      <h4 class="fw-bold m-0"><i class="fa-solid fa-users-gear text-primary"></i> إدارة مسارات المنسوبين</h4>
      <button class="btn btn-success fw-bold btn-sm" onclick="openAddModal()"><i class="fa-solid fa-plus"></i> إضافة ضابط صف جديد</button>
    </div>

    <div class="card-custom p-2">
      <div class="table-responsive">
        <table class="table table-hover align-middle text-center mb-0">
          <thead class="table-light">
            <tr>
              <th>ترتيب السرا</th>
              <th>الرقم العسكري</th>
              <th>الاسم</th>
              <th>الرتبة الحالية</th>
              <th>الرتبة المستحقة</th>
              <th>إجمالي النقاط</th>
              <th>الإجراءات</th>
            </tr>
          </thead>
          <tbody id="adminStaffTbody">
            </tbody>
        </table>
      </div>
    </div>
  </div>

</div>

<div class="modal fade" id="staffModal" tabindex="-1" aria-hidden="true">
  <div class="modal-dialog modal-lg">
    <div class="modal-content">
      <div class="modal-header bg-light">
        <h5 class="modal-title fw-bold" id="modalTitle">إضافة ملف منسوب</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <form onsubmit="event.preventDefault(); saveStaffForm();">
        <div class="modal-body p-4">
          <input type="hidden" id="formId">
          <div class="row g-3">
            <div class="col-md-6">
              <label class="form-label small fw-bold">الرقم العسكري *</label>
              <input type="text" id="formMilId" class="form-control" required>
            </div>
            <div class="col-md-6">
              <label class="form-label small fw-bold">الاسم الكامل *</label>
              <input type="text" id="formName" class="form-control" required>
            </div>
            <div class="col-md-6">
              <label class="form-label small fw-bold">الرتبة الحالية *</label>
              <select id="formRank" class="form-select" required>
                <option value="جندي أول">جندي أول</option>
                <option value="عريف">عريف</option>
                <option value="وكيل رقيب">وكيل رقيب</option>
                <option value="رقيب">رقيب</option>
                <option value="رقيب أول">رقيب أول</option>
              </select>
            </div>
            <div class="col-md-6">
              <label class="form-label small fw-bold">الرتبة المستحقة (القادمة) *</label>
              <select id="formNextRank" class="form-select" required>
                <option value="عريف">عريف</option>
                <option value="وكيل رقيب">وكيل رقيب</option>
                <option value="رقيب">رقيب</option>
                <option value="رقيب أول">رقيب أول</option>
                <option value="رئيس رقباء">رئيس رقباء</option>
              </select>
            </div>
            <hr>
            <h6 class="fw-bold text-secondary">توزيع النقاط التفصيلي لكل مرحلة</h6>
            <div class="col-6 col-md-3">
              <label class="form-label small fw-bold">نقاط الأقدمية</label>
              <input type="number" step="0.01" id="formPSeniority" class="form-control" value="0">
            </div>
            <div class="col-6 col-md-3">
              <label class="form-label small fw-bold">نقاط المؤهلات</label>
              <input type="number" step="0.01" id="formPEdu" class="form-control" value="0">
            </div>
            <div class="col-6 col-md-3">
              <label class="form-label small fw-bold">نقاط الدورات</label>
              <input type="number" step="0.01" id="formPCourses" class="form-control" value="0">
            </div>
            <div class="col-6 col-md-3">
              <label class="form-label small fw-bold">نقاط تقييم الأداء</label>
              <input type="number" step="0.01" id="formPPerf" class="form-control" value="0">
            </div>
          </div>
        </div>
        <div class="modal-footer bg-light">
          <button type="button" class="btn btn-secondary fw-bold" data-bs-dismiss="modal">إلغاء</button>
          <button type="submit" class="btn text-white fw-bold" style="background:var(--main);">حفظ البيانات</button>
        </div>
      </form>
    </div>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-database-compat.js"></script>

<script>
// ============ CONFIG FIREBASE (نفس الحساب الحالي الخاص بك) ============
const firebaseConfig = {
  apiKey: "AIzaSyAzyO4oN07WOfqoVzXzNCNWMTvrvNGWFMg",
  authDomain: "surveymoamar.firebaseapp.com",
  databaseURL: "https://surveymoamar-default-rtdb.firebaseio.com",
  projectId: "surveymoamar",
  storageBucket: "surveymoamar.firebasestorage.app",
  messagingSenderId: "944748990071",
  appId: "1:944748990071:web:e89fb772f86b0583e50072",
  measurementId: "G-E6796F3RES"
};
firebase.initializeApp(firebaseConfig);
const db = firebase.database();

let staffList = [];
let modalInstance = null;
let isAdminLoggedIn = false;

// INITIAL LOAD
window.addEventListener('DOMContentLoaded', () => {
  modalInstance = new bootstrap.Modal(document.getElementById('staffModal'));
  
  // الاستماع المباشر للتعديلات في قاعدة البيانات
  db.ref('masar_staff').on('value', (snapshot) => {
    const data = snapshot.val();
    staffList = data ? Object.values(data) : [];
    
    // ترتيب السرا تلقائياً بناءً على إجمالي النقاط (الأعلى أولاً)
    staffList.sort((a, b) => b.totalPoints - a.totalPoints);
    
    renderAdminTable();
  });
});

// NAVIGATION VIEWS
function switchView(viewId) {
  document.querySelectorAll('.view-section').forEach(section => section.classList.add('d-none'));
  document.getElementById('view-' + viewId).classList.remove('d-none');
}

// SEARCH FUNCTION (PORTAL VIEW)
function searchMilitaryId() {
  const milIdInput = document.getElementById('searchMilId').value.trim();
  
  // البحث عن الرقم العسكري ضمن القائمة المرتبة
  const index = staffList.findIndex(item => item.militaryId === milIdInput);
  const resultCard = document.getElementById('searchResultCard');
  
  if (index !== -1) {
    const person = staffList[index];
    
    document.getElementById('resName').innerText = person.name;
    document.getElementById('resMilId').innerText = person.militaryId;
    document.getElementById('resRank').innerText = person.rank;
    document.getElementById('resNextRank').innerText = person.nextRank;
    
    document.getElementById('pSeniority').innerText = person.pSeniority || 0;
    document.getElementById('pEdu').innerText = person.pEdu || 0;
    document.getElementById('pCourses').innerText = person.pCourses || 0;
    document.getElementById('pPerf').innerText = person.pPerf || 0;
    
    document.getElementById('resTotalPoints').innerText = person.totalPoints;
    document.getElementById('resQueueNum').innerText = index + 1; // رقم السرا المباشر
    
    resultCard.classList.remove('d-none');
  } else {
    resultCard.classList.add('d-none');
    alert('الرقم العسكري غير مسجل بالنظام حالياً. يرجى مراجعة جناح الإدارة.');
  }
}

// ADMIN AUTHENTICATION
function loginAdmin() {
  const user = document.getElementById('adminUser').value;
  const pass = document.getElementById('adminPass').value;
  
  if(user === 'admin' && pass === '123') {
    isAdminLoggedIn = true;
    document.getElementById('btnAdminLoginView').classList.add('d-none');
    document.getElementById('btnAdminDashboardView').classList.remove('d-none');
    document.getElementById('btnLogout').classList.remove('d-none');
    switchView('admin-dashboard');
  } else {
    alert('خطأ في رمز المسؤول أو الرقم السري!');
  }
}

function logoutAdmin() {
  isAdminLoggedIn = false;
  document.getElementById('btnAdminLoginView').classList.remove('d-none');
  document.getElementById('btnAdminDashboardView').classList.add('d-none');
  document.getElementById('btnLogout').classList.add('d-none');
  switchView('portal');
}

// RENDER ADMIN TABLE
function renderAdminTable() {
  const tbody = document.getElementById('adminStaffTbody');
  if(!tbody) return;
  
  if(staffList.length === 0) {
    tbody.innerHTML = `<tr><td colspan="7" class="text-muted p-4">لا توجد سجلات حالياً في مسار الترقيات</td></tr>`;
    return;
  }
  
  tbody.innerHTML = staffList.map((staff, idx) => `
    <tr>
      <td><span class="badge bg-secondary p-2 fs-6"># ${idx + 1}</span></td>
      <td><strong>${staff.militaryId}</strong></td>
      <td>${staff.name}</td>
      <td>${staff.rank}</td>
      <td><span class="text-success fw-bold">${staff.nextRank}</span></td>
      <td><span class="badge bg-primary p-2">${staff.totalPoints}</span></td>
      <td>
        <button class="btn btn-sm btn-outline-primary me-1" onclick="openEditModal('${staff.militaryId}')"><i class="fa-solid fa-pen"></i></button>
        <button class="btn btn-sm btn-outline-danger" onclick="deleteStaff('${staff.militaryId}')"><i class="fa-solid fa-trash"></i></button>
      </td>
    </tr>
  `).join('');
}

// DATA MANAGEMENT (CRUD)
function openAddModal() {
  document.getElementById('modalTitle').innerText = "إضافة ملف منسوب جديد";
  document.getElementById('formId').value = "";
  document.getElementById('formMilId').removeAttribute('readonly');
  document.getElementById('formMilId').value = "";
  document.getElementById('formName').value = "";
  document.getElementById('formPSeniority').value = 0;
  document.getElementById('formPEdu').value = 0;
  document.getElementById('formPCourses').value = 0;
  document.getElementById('formPPerf').value = 0;
  modalInstance.show();
}

function openEditModal(milId) {
  const staff = staffList.find(s => s.militaryId === milId);
  if(!staff) return;
  
  document.getElementById('modalTitle').innerText = "تعديل بيانات مسار الترقية";
  document.getElementById('formId').value = staff.militaryId;
  document.getElementById('formMilId').value = staff.militaryId;
  document.getElementById('formMilId').setAttribute('readonly', 'true');
  document.getElementById('formName').value = staff.name;
  document.getElementById('formRank').value = staff.rank;
  document.getElementById('formNextRank').value = staff.nextRank;
  
  document.getElementById('formPSeniority').value = staff.pSeniority || 0;
  document.getElementById('formPEdu').value = staff.pEdu || 0;
  document.getElementById('formPCourses').value = staff.pCourses || 0;
  document.getElementById('formPPerf').value = staff.pPerf || 0;
  
  modalInstance.show();
}

async function saveStaffForm() {
  const milId = document.getElementById('formMilId').value.trim();
  const name = document.getElementById('formName').value.trim();
  const rank = document.getElementById('formRank').value;
  const nextRank = document.getElementById('formNextRank').value;
  
  const pSeniority = parseFloat(document.getElementById('formPSeniority').value) || 0;
  const pEdu = parseFloat(document.getElementById('formPEdu').value) || 0;
  const pCourses = parseFloat(document.getElementById('formPCourses').value) || 0;
  const pPerf = parseFloat(document.getElementById('formPPerf').value) || 0;
  
  const totalPoints = parseFloat((pSeniority + pEdu + pCourses + pPerf).toFixed(2));
  
  const record = {
    militaryId: milId,
    name: name,
    rank: rank,
    nextRank: nextRank,
    pSeniority: pSeniority,
    pEdu: pEdu,
    pCourses: pCourses,
    pPerf: pPerf,
    totalPoints: totalPoints
  };
  
  await db.ref('masar_staff/' + milId).set(record);
  modalInstance.hide();
  alert('تم حفظ البيانات بنجاح في مسار الترقية.');
}

async function deleteStaff(milId) {
  if(confirm('هل أنت متأكد من حذف هذا السجل نهائياً من سرا الترقيات؟')) {
    await db.ref('masar_staff/' + milId).remove();
    alert('تم حذف السجل.');
  }
}
</script>
</body>
</html>
