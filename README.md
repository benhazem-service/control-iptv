<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IPTV Pro Manager</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <style>
        :root {
            --primary: #4f46e5;
            --primary-dark: #4338ca;
            --bg: #f8fafc;
            --surface: #ffffff;
            --text: #1e293b;
            --text-light: #64748b;
            --border: #e2e8f0;
            --danger: #ef4444;
            --success: #10b981;
            --warning: #f59e0b;
            --info: #3b82f6;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --radius: 0.75rem;
        }

        body.dark-mode {
            --bg: #0f172a;
            --surface: #1e293b;
            --text: #f8fafc;
            --text-light: #94a3b8;
            --border: #334155;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.5);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, sans-serif; }
        body { background-color: var(--bg); color: var(--text); transition: 0.3s; min-height: 100vh; padding-bottom: 60px; }

        /* Auth Styles */
        #auth-container { display: flex; justify-content: center; align-items: center; height: 100vh; background: linear-gradient(135deg, var(--primary), #818cf8); }
        .auth-card { background: var(--surface); padding: 2.5rem; border-radius: 1rem; width: 100%; max-width: 400px; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1); text-align: center; }
        .password-wrapper { position: relative; width: 100%; margin-bottom: 1rem; }
        .password-wrapper input { width: 100%; padding: 0.8rem 1rem; padding-left: 40px; border: 1px solid var(--border); border-radius: 0.5rem; outline: none; background: var(--bg); color: var(--text); }
        .password-wrapper i { position: absolute; left: 10px; top: 50%; transform: translateY(-50%); color: var(--text-light); cursor: pointer; z-index: 10; }
        .auth-input { width: 100%; padding: 0.8rem 1rem; margin-bottom: 1rem; border: 1px solid var(--border); border-radius: 0.5rem; outline: none; background: var(--bg); color: var(--text); }
        .auth-btn { width: 100%; padding: 0.8rem; background: var(--primary); color: white; border: none; border-radius: 0.5rem; font-weight: bold; cursor: pointer; transition: 0.2s; }
        .auth-btn:hover { background: var(--primary-dark); }
        .forgot-link { display: block; margin-bottom: 1rem; font-size: 0.85rem; color: var(--primary); text-decoration: underline; cursor: pointer; text-align: right; }
        .remember-me-container { display: flex; align-items: center; gap: 8px; margin-bottom: 1rem; font-size: 0.9rem; color: var(--text); }
        .remember-me-container input { width: 16px; height: 16px; cursor: pointer; }

        /* App Layout */
        #app-container { display: none; }
        nav { background: var(--surface); padding: 1rem 2rem; display: flex; justify-content: space-between; align-items: center; box-shadow: var(--shadow); position: sticky; top: 0; z-index: 100; }
        .logo { font-size: 1.4rem; font-weight: 800; color: var(--primary); display: flex; align-items: center; gap: 8px; }
        .nav-links button { background: transparent; border: none; padding: 0.6rem 1.2rem; cursor: pointer; color: var(--text-light); font-weight: 600; font-size: 0.95rem; transition: 0.2s; border-radius: 0.5rem; display: inline-flex; align-items: center; gap: 6px; }
        .nav-links button:hover { background: rgba(79, 70, 229, 0.1); color: var(--primary); }
        .nav-links button.active { background: var(--primary); color: white; }
        .nav-links button.hidden { display: none; } 
        .container { max-width: 1400px; margin: 2rem auto; padding: 0 1rem; }
        .section { display: none; animation: fadeIn 0.4s; }
        .section.active { display: block; }

        /* Dashboard */
        .dashboard-grid { display: grid; grid-template-columns: 3fr 1fr; gap: 1.5rem; transition: 0.3s ease; }
        .dashboard-grid.full-width { grid-template-columns: 1fr; }
        .chart-container { background: var(--surface); padding: 1rem; border-radius: var(--radius); box-shadow: var(--shadow); border: 1px solid var(--border); height: 320px; position: sticky; top: 100px; transition: 0.3s ease; }
        .chart-container.hidden { display: none; }

        /* Controls Area */
        .controls-area { display: flex; gap: 10px; align-items: center; margin-bottom: 1rem; flex-wrap: wrap; }
        .search-box { flex: 1; min-width: 200px; }
        .filter-box { width: 200px; }

        /* Components */
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 1.5rem; margin-bottom: 2rem; }
        .stat-card { background: var(--surface); padding: 1.5rem; border-radius: var(--radius); box-shadow: var(--shadow); border: 1px solid var(--border); display: flex; flex-direction: column; gap: 5px; }
        .stat-title { font-size: 0.9rem; color: var(--text-light); }
        .stat-value { font-size: 1.8rem; font-weight: 800; color: var(--text); }

        /* Forms */
        .form-section-title { font-size: 1.1rem; font-weight: 700; color: var(--primary); margin-bottom: 1rem; border-bottom: 2px solid var(--border); padding-bottom: 0.5rem; display: flex; align-items: center; gap: 8px; }
        .elegant-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1.5rem; }
        .form-card { background: var(--surface); padding: 1.5rem; border-radius: var(--radius); box-shadow: var(--shadow); border: 1px solid var(--border); }
        .form-group { margin-bottom: 1rem; }
        .form-group label { display: block; margin-bottom: 0.4rem; font-weight: 500; font-size: 0.9rem; color: var(--text-light); }
        .form-control { width: 100%; padding: 0.7rem; border: 1px solid var(--border); border-radius: 0.5rem; background: var(--bg); color: var(--text); font-size: 0.95rem; transition: 0.2s; }
        .form-control:focus { border-color: var(--primary); outline: none; box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.1); }

        /* Tables */
        .table-wrapper { background: var(--surface); border-radius: var(--radius); box-shadow: var(--shadow); overflow: hidden; border: 1px solid var(--border); }
        table { width: 100%; border-collapse: collapse; white-space: nowrap; }
        th { background: var(--bg); color: var(--text-light); font-weight: 600; padding: 1rem; text-align: right; }
        td { padding: 1rem; border-top: 1px solid var(--border); color: var(--text); vertical-align: middle; }
        tr:hover td { background: rgba(0,0,0,0.02); }
        
        .status-badge { padding: 0.35rem 0.85rem; border-radius: 2rem; font-size: 0.85rem; font-weight: 700; display: inline-block; min-width: 60px; text-align: center; cursor: pointer; transition: transform 0.2s; box-shadow: 0 1px 2px rgba(0,0,0,0.1); }
        .status-badge:hover { transform: scale(1.1); filter: brightness(95%); }
        .status-active { background: #dcfce7; color: #166534; }
        .status-warning { background: #fef9c3; color: #854d0e; }
        .status-expired { background: #fee2e2; color: #991b1b; }
        .status-none { background: #e2e8f0; color: #475569; cursor: default; }

        .btn { padding: 0.5rem 0.8rem; border: none; border-radius: 0.5rem; cursor: pointer; font-weight: 600; display: inline-flex; align-items: center; justify-content: center; gap: 5px; transition: 0.2s; font-size: 0.9rem; min-width: 35px; }
        .btn-primary { background: var(--primary); color: white; }
        .btn-success { background: var(--success); color: white; }
        .btn-danger { background: var(--danger); color: white; }
        .btn-warning { background: var(--warning); color: #fff; } 
        .btn-sm { padding: 0.4rem 0.6rem; font-size: 0.85rem; }
        .btn i { pointer-events: none; }

        /* Modals */
        .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); display: none; place-items: center; z-index: 1000; backdrop-filter: blur(2px); }
        .modal { background: var(--surface); padding: 2rem; border-radius: var(--radius); width: 90%; max-width: 600px; max-height: 90vh; overflow-y: auto; box-shadow: 0 20px 25px -5px rgba(0,0,0,0.2); }
        .detail-row { display: flex; justify-content: space-between; border-bottom: 1px solid var(--border); padding: 8px 0; align-items: center;}
        .detail-row span:first-child { color: var(--text-light); font-weight: bold; }
        .date-popover { position: fixed; background: #333; color: #fff; padding: 10px 15px; border-radius: 8px; z-index: 2000; font-size: 0.9rem; display: none; pointer-events: none; box-shadow: 0 4px 10px rgba(0,0,0,0.3); text-align: center; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        @media (max-width: 900px) { .dashboard-grid { grid-template-columns: 1fr; } .chart-container { order: -1; margin-bottom: 1rem; height: 250px; position: static; } }
    </style>
</head>
<body>

    <!-- AUTHENTICATION -->
    <div id="auth-container">
        <div class="auth-card">
            <h2 style="color: var(--primary); margin-bottom: 0.5rem;">IPTV Pro</h2>
            <p style="color: var(--text-light); margin-bottom: 1.5rem;">إدارة اشتراكاتك باحترافية</p>
            <form id="auth-form">
                <input type="email" id="auth-email" class="auth-input" placeholder="البريد الإلكتروني" required>
                <div class="password-wrapper">
                    <input type="password" id="auth-password" placeholder="كلمة المرور" required>
                    <i class="fas fa-eye" onclick="togglePasswordVisibility('auth-password', this)"></i>
                </div>
                <div class="password-wrapper" id="confirm-password-wrapper" style="display: none;">
                    <input type="password" id="auth-confirm-password" placeholder="تأكيد كلمة المرور">
                    <i class="fas fa-eye" onclick="togglePasswordVisibility('auth-confirm-password', this)"></i>
                </div>
                <div class="remember-me-container" id="remember-me-wrapper">
                    <input type="checkbox" id="remember-me">
                    <label for="remember-me">تذكرني</label>
                </div>
                <span id="forgot-password-link" class="forgot-link" onclick="resetPassword()">نسيت كلمة المرور؟</span>
                <button type="submit" class="auth-btn" id="auth-submit-btn">تسجيل الدخول</button>
            </form>
            <p style="margin-top:1rem; font-size:0.9rem; color:var(--text-light); cursor:pointer;" onclick="toggleAuthMode()" id="auth-switch-text">ليس لديك حساب؟ إنشاء حساب</p>
            <p id="auth-error" style="color:var(--danger); font-size:0.85rem; margin-top:1rem; display:none;"></p>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="app-container">
        <nav>
            <div class="logo"><i class="fas fa-layer-group"></i> IPTV Pro</div>
            <div class="nav-links">
                <button onclick="toggleChartVisibility()" title="إظهار/إخفاء المبيان"><i class="fas fa-chart-bar"></i></button>
                <button onclick="showSection('dashboard')" class="active" id="nav-dashboard"><i class="fas fa-home"></i> <span>الرئيسية</span></button>
                <button onclick="showSection('add'); resetForm();" id="nav-add"><i class="fas fa-user-plus"></i> <span>إضافة</span></button>
                <button onclick="showSection('finance')" id="nav-finance"><i class="fas fa-wallet"></i> <span>المالية</span></button>
                <button onclick="showSection('trash')" id="nav-trash"><i class="fas fa-trash"></i> <span>المحذوفات</span></button>
                <button onclick="showSection('settings')" id="nav-settings" class="hidden"><i class="fas fa-cogs"></i> <span>الإعدادات</span></button>
                <button onclick="logout()" style="color: var(--danger);"><i class="fas fa-sign-out-alt"></i></button>
            </div>
        </nav>

        <div class="container">
            <div id="loading" style="text-align: center; padding: 20px; display: none;"><i class="fas fa-circle-notch fa-spin fa-2x" style="color: var(--primary);"></i></div>

            <!-- 1. DASHBOARD -->
            <div id="dashboard" class="section active">
                <div class="dashboard-grid full-width" id="dashboardGrid">
                    <div class="card" style="background: var(--surface); padding: 1.5rem; border-radius: var(--radius); border: 1px solid var(--border);">
                        
                        <!-- Controls Area: Search & Filter -->
                        <div class="controls-area">
                            <h3 style="margin-left: 15px;">قائمة المشتركين</h3>
                            
                            <!-- Filter Dropdown -->
                            <select id="filterSelect" class="form-control filter-box" onchange="renderTable()">
                                <option value="all">الكل (أقل وقت IPTV)</option>
                                <option value="iptv_asc">أقل وقت متبقي (IPTV)</option>
                                <option value="iptv_desc">أكثر وقت متبقي (IPTV)</option>
                                <option value="ibo_lifetime">إيبو مدى الحياة</option>
                                <option value="ibo_asc">أقل وقت متبقي (إيبو)</option>
                            </select>

                            <input type="text" id="searchInput" placeholder="بحث بالاسم..." class="form-control search-box" onkeyup="renderTable()">
                        </div>

                        <div class="table-wrapper">
                            <table>
                                <thead>
                                    <tr>
                                        <th>الاسم</th>
                                        <th>IPTV (يوم)</th>
                                        <th>IBO (يوم)</th>
                                        <th>الجهاز</th>
                                        <th>إجراءات</th>
                                    </tr>
                                </thead>
                                <tbody id="subscribersTableBody"></tbody>
                            </table>
                        </div>
                    </div>
                    <div class="chart-container hidden" id="chartContainer">
                        <canvas id="salesChart"></canvas>
                    </div>
                </div>
            </div>

            <!-- 2. ADD SUBSCRIBER -->
            <div id="add" class="section">
                <h2 id="formTitle" style="margin-bottom: 1.5rem;">إضافة مشترك جديد</h2>
                <form id="subscriberForm">
                    <input type="hidden" id="editId">
                    <div class="elegant-form">
                        <!-- Basic Info -->
                        <div class="form-card">
                            <div class="form-section-title"><i class="fas fa-user"></i> البيانات الأساسية</div>
                            <div class="form-group"><label>اسم المشترك</label><input type="text" id="name" class="form-control" required></div>
                            <div class="form-group"><label>رقم الهاتف</label><input type="tel" id="phone" class="form-control"></div>
                            <div class="form-group"><label>نوع الجهاز</label><select id="deviceType" class="form-control"><option value="">-- اختر الجهاز --</option></select></div>
                        </div>
                        <!-- IPTV -->
                        <div class="form-card">
                            <div class="form-section-title"><i class="fas fa-tv"></i> اشتراك IPTV</div>
                            <div class="form-group"><label>تاريخ البدء</label><input type="date" id="startDate" class="form-control" required></div>
                            <div class="form-group"><label>المدة (أشهر)</label><input type="number" id="duration" class="form-control" required min="1"></div>
                            <div class="form-group"><label>سعر البيع (IPTV)</label><input type="number" id="iptvPrice" class="form-control" placeholder="0.00"></div>
                        </div>
                        <!-- IBO -->
                        <div class="form-card">
                            <div class="form-section-title"><i class="fas fa-play-circle"></i> تفعيل IBO Player</div>
                            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                                <div class="form-group"><label>MAC Address</label><input type="text" id="iboMac" class="form-control" placeholder="MAC"></div>
                                <div class="form-group"><label>Device Key</label><input type="text" id="iboKey" class="form-control" placeholder="Key"></div>
                            </div>
                            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                                <div class="form-group"><label>تاريخ التفعيل</label><input type="date" id="iboStartDate" class="form-control"></div>
                                <div class="form-group"><label>مدة التفعيل</label><select id="iboDuration" class="form-control"><option value="">بدون تفعيل</option><option value="year">سنة واحدة</option><option value="lifetime">مدى الحياة</option></select></div>
                            </div>
                            <div class="form-group"><label>سعر البيع (IBO)</label><input type="number" id="iboPrice" class="form-control" placeholder="0.00"></div>
                        </div>
                        <!-- Server -->
                        <div class="form-card">
                            <div class="form-section-title"><i class="fas fa-server"></i> بيانات السيرفر</div>
                            <div class="form-group"><label>Host / URL</label><input type="text" id="host" class="form-control"></div>
                            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                                <div class="form-group"><label>Username</label><input type="text" id="username" class="form-control"></div>
                                <div class="form-group"><label>Password</label><input type="text" id="password" class="form-control"></div>
                            </div>
                            <div class="form-group"><label>M3U URL</label><textarea id="m3u" class="form-control" rows="3" placeholder="http://..."></textarea></div>
                        </div>
                    </div>
                    <div style="margin-top: 2rem; text-align: left;">
                        <button type="button" class="btn btn-warning" onclick="showSection('dashboard');">إلغاء</button>
                        <button type="submit" class="btn btn-primary" style="padding: 0.7rem 2rem;">حفظ المشترك</button>
                    </div>
                </form>
            </div>

            <!-- 3. FINANCE -->
            <div id="finance" class="section">
                <div class="stats-grid">
                    <div class="stat-card" style="border-right: 4px solid var(--primary);"><span class="stat-title">مجموع المبيعات</span><span class="stat-value" id="totalSales">0.00</span></div>
                    <div class="stat-card" style="border-right: 4px solid var(--warning);"><span class="stat-title">مجموع التعبئة</span><span class="stat-value" id="totalRecharge">0.00</span></div>
                    <div class="stat-card" style="border-right: 4px solid var(--success);"><span class="stat-title">الصافي</span><span class="stat-value" id="netProfit">0.00</span></div>
                </div>
                <div class="form-card" style="margin-bottom: 2rem;">
                    <div class="form-section-title">إضافة تعبئة رصيد</div>
                    <div style="display: flex; gap: 10px; align-items: flex-end;">
                        <div style="flex: 1;"><label>المبلغ</label><input type="number" id="rechargeAmount" class="form-control"></div>
                        <div style="flex: 2;"><label>ملاحظة</label><input type="text" id="rechargeNote" class="form-control"></div>
                        <button onclick="addRecharge()" class="btn btn-success" style="height: 42px;">إضافة</button>
                    </div>
                </div>
                <div class="table-wrapper">
                    <h4 style="padding: 1rem;">سجل التعبئات</h4>
                    <table><thead><tr><th>المبلغ</th><th>التاريخ</th><th>الملاحظة</th><th>حذف</th></tr></thead><tbody id="rechargeTableBody"></tbody></table>
                </div>
            </div>

            <!-- 4. TRASH -->
            <div id="trash" class="section">
                <div class="table-wrapper" style="border-color: var(--danger);">
                    <h3 style="padding:1rem; color:var(--danger)">سلة المهملات</h3>
                    <table><tbody id="trashTableBody"></tbody></table>
                </div>
            </div>

            <!-- 5. SETTINGS -->
            <div id="settings" class="section">
                <div class="form-card">
                    <div class="form-section-title">إدارة الأجهزة</div>
                    <div style="display: flex; gap: 10px; margin-bottom: 1rem;">
                        <input type="text" id="newDeviceInput" class="form-control" placeholder="اسم الجهاز">
                        <button class="btn btn-primary" onclick="addDevice()">إضافة</button>
                    </div>
                    <div id="devicesList" style="display: flex; gap: 10px; flex-wrap: wrap;"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modals -->
    <div id="detailsModal" class="modal-overlay">
        <div class="modal">
            <div style="display:flex; justify-content:space-between; margin-bottom:1rem;">
                <h3>تفاصيل المشترك الكاملة</h3>
                <button onclick="document.getElementById('detailsModal').style.display='none'" style="border:none;background:none;font-size:1.5rem;">&times;</button>
            </div>
            <div id="detailsContent"></div>
            <div style="margin-top: 20px; text-align: left; border-top: 1px solid #eee; padding-top: 15px;">
                <button id="modalEditBtn" class="btn btn-warning">تعديل المشترك</button>
            </div>
        </div>
    </div>

    <div id="dateModal" class="modal-overlay">
        <div class="modal" style="max-width: 300px; text-align: center; padding: 1.5rem;">
            <h4 id="dateModalTitle" style="color:var(--primary); margin-bottom:1rem;"></h4>
            <div style="margin-bottom:0.5rem"><strong>البداية:</strong> <span id="dateModalStart"></span></div>
            <div style="margin-bottom:1.5rem"><strong>الانتهاء:</strong> <span id="dateModalEnd"></span></div>
            <button class="btn btn-primary" onclick="document.getElementById('dateModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <!-- Firebase -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut, sendPasswordResetEmail, setPersistence, browserLocalPersistence, browserSessionPersistence } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, updateDoc, deleteDoc, doc, onSnapshot, query, where, getDocs, setDoc, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyA3EcwcPbdJo5j79fS0j2Hhw2W7LLEWxEc",
            authDomain: "control-iptv-49b63.firebaseapp.com",
            projectId: "control-iptv-49b63",
            storageBucket: "control-iptv-49b63.firebasestorage.app",
            messagingSenderId: "60685582544",
            appId: "1:60685582544:web:81c0cb70ba16512b449ed5",
            measurementId: "G-RZD7YKRDKZ"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        let currentUser = null;
        let isAdmin = false;
        let subscribers = [];
        let recharges = [];
        let trash = [];
        let managedDevices = [];
        let salesChartInstance = null;
        let isLoginMode = true;

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                currentUser = user;
                document.getElementById('auth-container').style.display = 'none';
                document.getElementById('app-container').style.display = 'block';
                await loadData(user.uid);
            } else {
                currentUser = null;
                document.getElementById('auth-container').style.display = 'flex';
                document.getElementById('app-container').style.display = 'none';
            }
        });

        async function loadData(uid) {
            document.getElementById('loading').style.display = 'block';
            onSnapshot(doc(db, "users", uid), (snap) => {
                if (snap.exists()) {
                    isAdmin = (snap.data().role === 'admin');
                    if (isAdmin) {
                        document.getElementById('nav-settings').classList.remove('hidden');
                        claimOrphanedSubscribers(uid); 
                    } else {
                        document.getElementById('nav-settings').classList.add('hidden');
                    }
                }
            });
            const subQuery = query(collection(db, "subscribers"), where("userId", "==", uid));
            onSnapshot(subQuery, (snap) => {
                subscribers = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                renderTable();
                updateChart();
                calculateFinances();
            });
            const rechQuery = query(collection(db, "recharges"), where("userId", "==", uid));
            onSnapshot(rechQuery, (snap) => {
                recharges = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                recharges.sort((a,b) => new Date(b.date) - new Date(a.date));
                renderRechargeTable();
                calculateFinances();
            });
            const trashQuery = query(collection(db, "trash"), where("userId", "==", uid));
            onSnapshot(trashQuery, (snap) => {
                trash = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                renderTrash();
            });
            onSnapshot(collection(db, "managed_devices"), (snap) => {
                managedDevices = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                renderDeviceSelect();
                if (isAdmin) renderAdminDevices();
            });
            document.getElementById('loading').style.display = 'none';
        }

        async function claimOrphanedSubscribers(adminUid) {
            const allSubs = await getDocs(collection(db, "subscribers"));
            allSubs.forEach(async (d) => {
                const data = d.data();
                if (!data.userId) { await updateDoc(d.ref, { userId: adminUid }); }
            });
        }

        window.toggleAuthMode = () => {
            isLoginMode = !isLoginMode;
            document.getElementById('auth-submit-btn').innerText = isLoginMode ? "تسجيل الدخول" : "إنشاء حساب";
            document.getElementById('auth-switch-text').innerText = isLoginMode ? "ليس لديك حساب؟ إنشاء حساب" : "لديك حساب؟ تسجيل الدخول";
            
            const confirmWrapper = document.getElementById('confirm-password-wrapper');
            const forgotLink = document.getElementById('forgot-password-link');
            const rememberWrapper = document.getElementById('remember-me-wrapper');
            
            if (isLoginMode) {
                confirmWrapper.style.display = 'none';
                document.getElementById('auth-confirm-password').removeAttribute('required');
                forgotLink.style.display = 'block';
                rememberWrapper.style.display = 'flex';
            } else {
                confirmWrapper.style.display = 'block';
                document.getElementById('auth-confirm-password').setAttribute('required', 'true');
                forgotLink.style.display = 'none';
                rememberWrapper.style.display = 'none';
            }
            document.getElementById('auth-error').style.display = 'none';
        };

        window.togglePasswordVisibility = (fieldId, icon) => {
            const input = document.getElementById(fieldId);
            if (input.type === "password") {
                input.type = "text";
                icon.classList.remove('fa-eye');
                icon.classList.add('fa-eye-slash');
            } else {
                input.type = "password";
                icon.classList.remove('fa-eye-slash');
                icon.classList.add('fa-eye');
            }
        };

        window.resetPassword = async () => {
            const email = document.getElementById('auth-email').value;
            if(!email) { alert("الرجاء إدخال البريد الإلكتروني أولاً"); return; }
            try { await sendPasswordResetEmail(auth, email); alert("تم إرسال رابط إعادة التعيين"); } catch(e) { alert("خطأ: " + e.message); }
        };

        document.getElementById('auth-form').addEventListener('submit', async (e) => {
            e.preventDefault();
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-password').value;
            const confirmPass = document.getElementById('auth-confirm-password').value;
            const rememberMe = document.getElementById('remember-me').checked;
            const errorEl = document.getElementById('auth-error');
            errorEl.style.display = 'none';

            try {
                if (isLoginMode) {
                    const persistence = rememberMe ? browserLocalPersistence : browserSessionPersistence;
                    await setPersistence(auth, persistence);
                    await signInWithEmailAndPassword(auth, email, pass);
                } else {
                    if(pass !== confirmPass) { throw new Error("كلمة المرور غير متطابقة"); }
                    if(pass.length < 6) { throw new Error("كلمة المرور ضعيفة"); }
                    const cred = await createUserWithEmailAndPassword(auth, email, pass);
                    const usersSnap = await getDocs(collection(db, 'users'));
                    const role = usersSnap.empty ? 'admin' : 'user';
                    await setDoc(doc(db, 'users', cred.user.uid), { email, role, createdAt: new Date().toISOString() });
                }
            } catch (err) { errorEl.innerText = err.message; errorEl.style.display = 'block'; }
        });
        window.logout = () => signOut(auth);

        window.toggleChartVisibility = () => {
            const chart = document.getElementById('chartContainer');
            const grid = document.getElementById('dashboardGrid');
            if (chart.classList.contains('hidden')) { chart.classList.remove('hidden'); grid.classList.remove('full-width'); } 
            else { chart.classList.add('hidden'); grid.classList.add('full-width'); }
        };

        window.showSection = (id) => {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-links button').forEach(b => b.classList.remove('active'));
            document.getElementById('nav-'+id).classList.add('active');
        }

        // --- FILTER & SORT LOGIC ---
        window.renderTable = () => {
            const tbody = document.getElementById('subscribersTableBody');
            const term = document.getElementById('searchInput').value.toLowerCase();
            const filter = document.getElementById('filterSelect').value;
            
            tbody.innerHTML = '';
            
            // 1. Copy array for sorting
            let displayData = [...subscribers];

            // 2. Sorting / Filtering Logic
            if(filter === 'iptv_asc' || filter === 'all') {
                displayData.sort((a,b) => new Date(a.endDate) - new Date(b.endDate));
            } else if (filter === 'iptv_desc') {
                displayData.sort((a,b) => new Date(b.endDate) - new Date(a.endDate));
            } else if (filter === 'ibo_lifetime') {
                displayData = displayData.filter(s => s.iboDuration === 'lifetime');
            } else if (filter === 'ibo_asc') {
                displayData = displayData.filter(s => s.iboDuration === 'year' && s.iboStartDate);
                displayData.sort((a,b) => {
                    let endA = new Date(a.iboStartDate); endA.setFullYear(endA.getFullYear()+1);
                    let endB = new Date(b.iboStartDate); endB.setFullYear(endB.getFullYear()+1);
                    return endA - endB;
                });
            }

            displayData.forEach(sub => {
                if(sub.name.toLowerCase().includes(term)) {
                    const iptvDays = getDays(sub.endDate); 
                    const iptvClass = iptvDays > 30 ? 'status-active' : (iptvDays > 0 ? 'status-warning' : 'status-expired');
                    const iptvText = iptvDays > 0 ? `${iptvDays} يوم` : "منتهي";

                    let iboText = "-";
                    let iboClass = "status-none";
                    let iboEndDate = "";
                    let iboStartDate = sub.iboStartDate || "";

                    if (sub.iboDuration === 'lifetime') { iboText = "∞"; iboClass = "status-active"; iboEndDate = "مدى الحياة"; } 
                    else if (sub.iboDuration === 'year' && sub.iboStartDate) {
                        let start = new Date(sub.iboStartDate);
                        let end = new Date(start);
                        end.setFullYear(end.getFullYear() + 1);
                        iboEndDate = end.toISOString().split('T')[0];
                        let diff = Math.ceil((end - new Date()) / (1000 * 60 * 60 * 24));
                        iboText = diff > 0 ? `${diff} يوم` : "منتهي";
                        iboClass = diff > 30 ? 'status-active' : (diff > 0 ? 'status-warning' : 'status-expired');
                    }

                    tbody.innerHTML += `
                        <tr>
                            <td><span style="color:var(--primary);font-weight:bold;cursor:pointer" onclick="showDetails('${sub.id}')">${sub.name}</span></td>
                            <td><span class="status-badge ${iptvClass}" onclick="showDateDetails('${sub.startDate}', '${sub.endDate}', 'IPTV')">${iptvText}</span></td>
                            <td><span class="status-badge ${iboClass}" onclick="showDateDetails('${iboStartDate}', '${iboEndDate}', 'IBO Player')">${iboText}</span></td>
                            <td>${sub.device || '-'}</td>
                            <td style="white-space:nowrap;">
                                <button class="btn btn-warning btn-sm" onclick="editSub('${sub.id}')" title="تعديل"><i class="fas fa-edit"></i></button>
                                <button class="btn btn-danger btn-sm" onclick="moveToTrash('${sub.id}')" title="حذف"><i class="fas fa-trash"></i></button>
                            </td>
                        </tr>
                    `;
                }
            });
        }

        window.showDateDetails = (start, end, title) => {
            if(!start) return; 
            document.getElementById('dateModalTitle').innerText = title;
            document.getElementById('dateModalStart').innerText = start;
            document.getElementById('dateModalEnd').innerText = end;
            document.getElementById('dateModal').style.display = 'flex';
        };

        function calculateFinances() {
            let sales = subscribers.reduce((acc, s) => acc + (parseFloat(s.iptvPrice)||0) + (parseFloat(s.iboPrice)||0), 0);
            let rechargeTotal = recharges.reduce((acc, r) => acc + (parseFloat(r.amount)||0), 0);
            document.getElementById('totalSales').innerText = sales.toFixed(2);
            document.getElementById('totalRecharge').innerText = rechargeTotal.toFixed(2);
            document.getElementById('netProfit').innerText = (sales - rechargeTotal).toFixed(2);
        }

        window.addRecharge = async () => {
            const amt = document.getElementById('rechargeAmount').value;
            const note = document.getElementById('rechargeNote').value;
            if(amt) {
                await addDoc(collection(db, "recharges"), { amount: parseFloat(amt), note: note, date: new Date().toISOString(), userId: currentUser.uid });
                document.getElementById('rechargeAmount').value = '';
                document.getElementById('rechargeNote').value = '';
            }
        };
        window.deleteRecharge = async (id) => { if(confirm("حذف؟")) await deleteDoc(doc(db, "recharges", id)); };
        function renderRechargeTable() {
            const t = document.getElementById('rechargeTableBody'); t.innerHTML = '';
            recharges.forEach(r => {
                t.innerHTML += `<tr><td>${r.amount}</td><td>${r.date.split('T')[0]}</td><td>${r.note}</td><td><button class="btn btn-danger btn-sm" onclick="deleteRecharge('${r.id}')">&times;</button></td></tr>`;
            });
        }

        document.getElementById('subscriberForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const id = document.getElementById('editId').value;
            const startDate = document.getElementById('startDate').value;
            const duration = parseInt(document.getElementById('duration').value);
            let d = new Date(startDate); d.setMonth(d.getMonth() + duration);
            const endDate = d.toISOString().split('T')[0];

            const data = {
                name: document.getElementById('name').value,
                phone: document.getElementById('phone').value,
                device: document.getElementById('deviceType').value,
                startDate, duration, endDate,
                iptvPrice: document.getElementById('iptvPrice').value,
                iboMac: document.getElementById('iboMac').value,
                iboKey: document.getElementById('iboKey').value,
                iboStartDate: document.getElementById('iboStartDate').value,
                iboDuration: document.getElementById('iboDuration').value,
                iboPrice: document.getElementById('iboPrice').value,
                host: document.getElementById('host').value,
                username: document.getElementById('username').value,
                password: document.getElementById('password').value,
                m3u: document.getElementById('m3u').value,
                userId: currentUser.uid
            };

            if(id) await updateDoc(doc(db, "subscribers", id), data);
            else await addDoc(collection(db, "subscribers"), data);
            showSection('dashboard');
        });

        window.editSub = (id) => {
            document.getElementById('detailsModal').style.display='none';
            showSection('add');
            const s = subscribers.find(x => x.id === id);
            document.getElementById('formTitle').innerText = 'تعديل مشترك: ' + s.name;
            document.getElementById('editId').value = s.id;
            document.getElementById('name').value = s.name || '';
            document.getElementById('phone').value = s.phone || '';
            document.getElementById('deviceType').value = s.device || '';
            document.getElementById('startDate').value = s.startDate || '';
            document.getElementById('duration').value = s.duration || '';
            document.getElementById('iptvPrice').value = s.iptvPrice || '';
            document.getElementById('iboMac').value = s.iboMac || '';
            document.getElementById('iboKey').value = s.iboKey || '';
            document.getElementById('iboStartDate').value = s.iboStartDate || '';
            document.getElementById('iboDuration').value = s.iboDuration || '';
            document.getElementById('iboPrice').value = s.iboPrice || '';
            document.getElementById('host').value = s.host || '';
            document.getElementById('username').value = s.username || '';
            document.getElementById('password').value = s.password || '';
            document.getElementById('m3u').value = s.m3u || '';
        };

        window.resetForm = function() { 
            document.getElementById('subscriberForm').reset(); 
            document.getElementById('editId').value=''; 
            document.getElementById('formTitle').innerText = 'إضافة مشترك جديد';
        }

        window.showDetails = (id) => {
            const s = subscribers.find(x => x.id === id);
            const content = document.getElementById('detailsContent');
            const total = (parseFloat(s.iptvPrice)||0) + (parseFloat(s.iboPrice)||0);
            let iboStatus = "غير مفعل", iboEndDisplay = "-";
            if (s.iboDuration && s.iboStartDate) {
                if (s.iboDuration === 'lifetime') { iboStatus = "مدى الحياة"; iboEndDisplay = "∞"; } 
                else if (s.iboDuration === 'year') {
                    let start = new Date(s.iboStartDate);
                    let end = new Date(start);
                    end.setFullYear(end.getFullYear() + 1);
                    iboEndDisplay = end.toISOString().split('T')[0];
                    let diff = Math.ceil((end - new Date()) / (1000 * 60 * 60 * 24));
                    iboStatus = diff > 0 ? `${diff} يوم` : "منتهي";
                }
            }
            const iptvDays = getDays(s.endDate);
            const iptvStatus = iptvDays > 0 ? `${iptvDays} يوم` : "منتهي";

            content.innerHTML = `
                <div class="detail-row"><span>الاسم:</span> <span>${s.name}</span></div>
                <div class="detail-row"><span>الهاتف:</span> <span>${s.phone || '-'}</span></div>
                <div class="detail-row"><span>الجهاز:</span> <span>${s.device || '-'}</span></div>
                <h4 style="color:var(--primary);margin-top:10px;border-bottom:1px solid #eee;">معلومات IPTV</h4>
                <div class="detail-row"><span>تاريخ البدء:</span> <span>${s.startDate}</span></div>
                <div class="detail-row"><span>تاريخ الانتهاء:</span> <span>${s.endDate}</span></div>
                <div class="detail-row" style="color:var(--success)"><span>المتبقي:</span> <span>${iptvStatus}</span></div>
                <h4 style="color:var(--primary);margin-top:10px;border-bottom:1px solid #eee;">IBO Player</h4>
                <div class="detail-row"><span>MAC:</span> <span>${s.iboMac || '-'}</span></div>
                <div class="detail-row"><span>Key:</span> <span>${s.iboKey || '-'}</span></div>
                <div class="detail-row"><span>تاريخ التفعيل:</span> <span>${s.iboStartDate || '-'}</span></div>
                <div class="detail-row"><span>تاريخ الانتهاء:</span> <span>${iboEndDisplay}</span></div>
                <div class="detail-row" style="color:var(--info)"><span>المتبقي:</span> <span>${iboStatus}</span></div>
                <h4 style="color:var(--primary);margin-top:10px;border-bottom:1px solid #eee;">السيرفر</h4>
                <div class="detail-row"><span>Host:</span> <span>${s.host || '-'}</span></div>
                <div class="detail-row"><span>User:</span> <span>${s.username || '-'}</span></div>
                <div class="detail-row"><span>Pass:</span> <span>${s.password || '-'}</span></div>
                <div style="margin-top:5px"><strong>M3U:</strong><textarea class="form-control" rows="2" readonly>${s.m3u || ''}</textarea></div>
                <h4 style="color:var(--primary);margin-top:10px;border-bottom:1px solid #eee;">المالية</h4>
                <div class="detail-row"><span>سعر IPTV:</span> <span>${s.iptvPrice || 0}</span></div>
                <div class="detail-row"><span>سعر IBO:</span> <span>${s.iboPrice || 0}</span></div>
                <div class="detail-row" style="background:#f0f9ff; padding:5px; margin-top:5px; font-weight:bold;"><span>الإجمالي:</span> <span>${total}</span></div>
            `;
            const editBtn = document.getElementById('modalEditBtn');
            editBtn.onclick = function() { editSub(s.id); };
            document.getElementById('detailsModal').style.display = 'flex';
        };

        window.moveToTrash = async (id) => { if(confirm("نقل للسلة؟")) { const s = subscribers.find(x=>x.id===id); await addDoc(collection(db,"trash"),{...s, deletedAt:new Date().toISOString()}); await deleteDoc(doc(db,"subscribers",id)); }};
        window.renderTrash = () => { const t = document.getElementById('trashTableBody'); t.innerHTML = ''; trash.forEach(i => t.innerHTML += `<tr><td>${i.name}</td><td><button class="btn btn-success btn-sm" onclick="restore('${i.id}')">استعادة</button></td><td><button class="btn btn-danger btn-sm" onclick="permDelete('${i.id}')">&times;</button></td></tr>`); };
        
        // FIXED RESTORE LOGIC
        window.restore = async (id) => { 
            const i = trash.find(x=>x.id===id); 
            if(i) {
                const {deletedAt, id:old,...d}=i; 
                await addDoc(collection(db,"subscribers"),d); 
                await deleteDoc(doc(db,"trash"),id); 
            }
        };
        window.permDelete = async (id) => { if(confirm("نهائي؟")) await deleteDoc(doc(db,"trash"),id); };

        window.addDevice = async () => { const v=document.getElementById('newDeviceInput').value; if(v) await addDoc(collection(db,"managed_devices"),{name:v}); document.getElementById('newDeviceInput').value=''; };
        window.deleteDevice = async (id) => { if(confirm("حذف؟")) await deleteDoc(doc(db,"managed_devices"),id); };
        function renderAdminDevices() { const d=document.getElementById('devicesList'); d.innerHTML=''; managedDevices.forEach(i=>d.innerHTML+=`<span style="background:#ddd;padding:5px 10px;border-radius:10px">${i.name} <b style="color:red;cursor:pointer" onclick="deleteDevice('${i.id}')">&times;</b></span>`); }
        window.renderDeviceSelect = () => { const s=document.getElementById('deviceType'); const old=s.value; s.innerHTML='<option value="">اختر</option>'; managedDevices.forEach(i=>s.innerHTML+=`<option value="${i.name}">${i.name}</option>`); s.value=old; };
        
        function getDays(e) { return Math.ceil((new Date(e)-new Date())/(1000*60*60*24)); }
        
        function updateChart() {
            const ctx = document.getElementById('salesChart').getContext('2d');
            const months = {};
            subscribers.forEach(s => { const m = s.startDate.substring(0,7); months[m] = (months[m]||0)+1; });
            const labels = Object.keys(months).sort();
            if(salesChartInstance) salesChartInstance.destroy();
            salesChartInstance = new Chart(ctx, { 
                type:'bar', 
                data:{
                    labels, 
                    datasets:[{
                        label:'عدد الاشتراكات', 
                        data:labels.map(l=>months[l]), 
                        backgroundColor:'#4f46e5',
                        borderRadius: 5
                    }]
                }, 
                options:{
                    maintainAspectRatio:false,
                    plugins: { legend: { display: false } },
                    scales: { y: { beginAtZero: true, grid: { display: false } }, x: { grid: { display: false } } }
                } 
            });
        }
    </script>
</body>
</html>
