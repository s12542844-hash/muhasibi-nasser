<!DOCTYPE html>
<html lang="ar" dir="rtl" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>محاسبي - المنصة المحاسبية المتخصصة الشاملة</title>
    
    <!-- Google Fonts: Cairo for sleek Arabic typography -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;800;900&display=swap" rel="stylesheet">
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Cairo', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#ecfdf5',
                            100: '#d1fae5',
                            500: '#10b981',
                            600: '#059669',
                            700: '#047857',
                            800: '#065f46',
                            900: '#064e3b',
                        }
                    }
                }
            }
        }
    </script>
    
    <!-- FontAwesome Icons CDN -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #0f172a;
            color: #f8fafc;
            min-height: 100vh;
        }

        /* Glassmorphism utility classes */
        .glass-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        .glass-nav {
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(16px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.08);
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #10b981;
        }

        /* Print Mode Setup */
        @media print {
            body * {
                visibility: hidden;
            }
            #printable-section, #printable-section * {
                visibility: visible;
            }
            #printable-section {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                color: #000 !important;
                background: #fff !important;
            }
            .no-print {
                display: none !important;
            }
        }
    </style>
</head>
<body class="flex flex-col min-h-screen antialiased selection:bg-emerald-500 selection:text-white">

    <!-- Custom Toast Notification Box -->
    <div id="toast-container" class="fixed top-5 left-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <!-- PIN Security Lock Screen Modal -->
    <div id="pin-modal" class="fixed inset-0 bg-slate-950/90 backdrop-blur-lg z-50 flex items-center justify-center hidden">
        <div class="glass-card p-8 rounded-2xl max-w-sm w-full mx-4 text-center border border-emerald-500/30 shadow-2xl shadow-emerald-950/50">
            <div class="w-16 h-16 bg-emerald-500/10 text-emerald-400 rounded-full flex items-center justify-center mx-auto mb-4 border border-emerald-500/20">
                <i class="fa-solid fa-lock text-2xl"></i>
            </div>
            <h3 class="text-xl font-bold mb-1 text-white">محاسبي - النظام مقفل</h3>
            <p class="text-xs text-slate-400 mb-6">يرجى إدخال رمز الأمان PIN للمتابعة</p>
            <input type="password" id="pin-input" maxlength="6" placeholder="• • • •" class="w-full text-center text-2xl tracking-widest py-3 bg-slate-900/80 border border-slate-700 rounded-xl mb-4 focus:outline-none focus:border-emerald-500 text-emerald-400 font-mono">
            <button onclick="checkPin()" class="w-full py-3 bg-emerald-600 hover:bg-emerald-500 text-white font-bold rounded-xl transition shadow-lg shadow-emerald-900/30">
                فتح النظام <i class="fa-solid fa-key mr-2"></i>
            </button>
        </div>
    </div>

    <!-- Top Header Navigation -->
    <header class="glass-nav sticky top-0 z-40 px-4 lg:px-8 py-3.5 flex flex-wrap items-center justify-between gap-4">
        <!-- Logo & Branding -->
        <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-gradient-to-tr from-emerald-600 to-teal-400 rounded-xl flex items-center justify-center shadow-lg shadow-emerald-500/20">
                <i class="fa-solid fa-calculator text-slate-950 text-xl"></i>
            </div>
            <div>
                <div class="flex items-center gap-2">
                    <h1 class="text-xl font-extrabold tracking-wide text-white">محاسبي</h1>
                    <span id="current-activity-badge" class="px-2.5 py-0.5 text-xs font-semibold rounded-full bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                        تجارة وتجزئة
                    </span>
                </div>
                <p class="text-[11px] text-emerald-400 font-medium flex items-center gap-1">
                    <i class="fa-solid fa-user-check text-[10px]"></i>
                    تطوير وإعداد المحاسب: براء نادر توفيق ناصر
                </p>
            </div>
        </div>

        <!-- Dynamic Header Controls -->
        <div class="flex items-center gap-2 flex-wrap">
            <!-- Active Activity Selector -->
            <div class="flex items-center bg-slate-800/80 border border-slate-700 rounded-xl px-3 py-1.5 text-xs">
                <i class="fa-solid fa-briefcase text-slate-400 ml-2"></i>
                <select id="header-activity-select" onchange="changeActivity(this.value)" class="bg-transparent text-slate-200 focus:outline-none cursor-pointer font-semibold">
                    <option value="retail" class="bg-slate-900 text-slate-200">نشاط: تجارة وتجزئة</option>
                    <option value="realestate" class="bg-slate-900 text-slate-200">نشاط: عقارات ومخازن</option>
                    <option value="manufacturing" class="bg-slate-900 text-slate-200">نشاط: تصنيع وإنتاج</option>
                    <option value="services" class="bg-slate-900 text-slate-200">نشاط: خدمات واستشارات</option>
                </select>
            </div>

            <!-- Demo Data Button -->
            <button onclick="loadDemoData()" class="px-3 py-1.5 bg-indigo-600/20 hover:bg-indigo-600/30 text-indigo-300 border border-indigo-500/30 rounded-xl text-xs font-bold transition flex items-center gap-1.5">
                <i class="fa-solid fa-wand-magic-sparkles"></i>
                <span>بيانات تجريبية 🚀</span>
            </button>

            <!-- Quick Print Button -->
            <button onclick="switchTab('reports'); window.print();" class="px-3 py-1.5 bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 rounded-xl text-xs font-semibold transition flex items-center gap-1.5">
                <i class="fa-solid fa-print"></i>
                <span class="hidden sm:inline">طباعة رسمية</span>
            </button>

            <!-- Lock Button -->
            <button id="lock-btn" onclick="toggleLock()" class="p-2 bg-slate-800 hover:bg-slate-700 text-slate-400 hover:text-amber-400 border border-slate-700 rounded-xl text-xs transition" title="قفل النظام">
                <i class="fa-solid fa-lock"></i>
            </button>
        </div>
    </header>

    <!-- Tab Navigation Bar -->
    <nav class="bg-slate-900/90 border-b border-slate-800 px-4 lg:px-8 overflow-x-auto">
        <div class="flex space-x-1 space-x-reverse min-w-max py-2 text-sm">
            <button onclick="switchTab('dashboard')" id="nav-btn-dashboard" class="tab-btn active px-4 py-2 rounded-xl font-bold flex items-center gap-2 transition text-emerald-400 bg-emerald-500/10 border border-emerald-500/20">
                <i class="fa-solid fa-chart-pie"></i>
                <span>اللوحة الرئيسية والمعاملات</span>
            </button>
            <button onclick="switchTab('activity')" id="nav-btn-activity" class="tab-btn px-4 py-2 rounded-xl font-bold flex items-center gap-2 transition text-slate-400 hover:text-slate-200 hover:bg-slate-800/60">
                <i class="fa-solid fa-sliders"></i>
                <span>أدوات النشاط المخصص</span>
            </button>
            <button onclick="switchTab('advanced')" id="nav-btn-advanced" class="tab-btn px-4 py-2 rounded-xl font-bold flex items-center gap-2 transition text-slate-400 hover:text-slate-200 hover:bg-slate-800/60">
                <i class="fa-solid fa-scale-balanced"></i>
                <span>العمليات المحاسبية المتقدمة</span>
            </button>
            <button onclick="switchTab('reports')" id="nav-btn-reports" class="tab-btn px-4 py-2 rounded-xl font-bold flex items-center gap-2 transition text-slate-400 hover:text-slate-200 hover:bg-slate-800/60">
                <i class="fa-solid fa-file-invoice-dollar"></i>
                <span>التقارير والطباعة الرسمية</span>
            </button>
            <button onclick="switchTab('assistant')" id="nav-btn-assistant" class="tab-btn px-4 py-2 rounded-xl font-bold flex items-center gap-2 transition text-slate-400 hover:text-slate-200 hover:bg-slate-800/60">
                <i class="fa-solid fa-robot text-indigo-400"></i>
                <span>المساعد الذكي والدليل</span>
            </button>
            <button onclick="switchTab('settings')" id="nav-btn-settings" class="tab-btn px-4 py-2 rounded-xl font-bold flex items-center gap-2 transition text-slate-400 hover:text-slate-200 hover:bg-slate-800/60">
                <i class="fa-solid fa-gear"></i>
                <span>الإعدادات والأمان</span>
            </button>
        </div>
    </nav>

    <main class="flex-1 p-4 lg:p-8 max-w-7xl w-full mx-auto space-y-6">

        <!-- ========================================== -->
        <!-- TAB 1: DASHBOARD & TRANSACTIONS            -->
        <!-- ========================================== -->
        <section id="tab-dashboard" class="tab-content space-y-6">
            <!-- Welcome Banner -->
            <div class="glass-card p-6 rounded-2xl relative overflow-hidden flex flex-col md:flex-row items-center justify-between gap-4 border-r-4 border-r-emerald-500">
                <div>
                    <h2 class="text-2xl font-black text-white mb-1">مرحباً بك في أداة "محاسبي" المتقدمة 👋</h2>
                    <p class="text-sm text-slate-300">نظام محاسبي متخصص صُمم ليناسب نشاطك العملي بدقة وأمان عالٍ.</p>
                    <p class="text-xs text-emerald-400 font-semibold mt-2">
                        <i class="fa-solid fa-certificate ml-1"></i> إعداد وتطوير المحاسب: براء نادر توفيق ناصر
                    </p>
                </div>
                <div class="flex items-center gap-3">
                    <div class="text-left bg-slate-900/60 px-4 py-2.5 rounded-xl border border-slate-700">
                        <span class="text-xs text-slate-400 block">النشاط التجاري المفعل</span>
                        <span id="banner-activity-name" class="font-bold text-emerald-400 text-sm">تجارة وتجزئة</span>
                    </div>
                    <button onclick="switchTab('settings')" class="px-3 py-2 bg-slate-800 hover:bg-slate-700 text-xs rounded-xl border border-slate-700 transition">
                        تغيير <i class="fa-solid fa-chevron-left mr-1"></i>
                    </button>
                </div>
            </div>

            <!-- Financial Cards Summary -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <!-- Total Income Card -->
                <div class="glass-card p-5 rounded-2xl">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs font-bold text-slate-400">إجمالي الإيرادات المقبوضة</span>
                        <div class="w-8 h-8 rounded-lg bg-emerald-500/10 text-emerald-400 flex items-center justify-center">
                            <i class="fa-solid fa-arrow-down-left text-sm"></i>
                        </div>
                    </div>
                    <div class="text-2xl font-black text-emerald-400 font-mono" id="stat-total-income">0.00</div>
                    <span class="text-[11px] text-slate-500 mt-1 block">تتضمن المبيعات والتحصيلات</span>
                </div>

                <!-- Total Expense Card -->
                <div class="glass-card p-5 rounded-2xl">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs font-bold text-slate-400">إجمالي المصاريف والنفقات</span>
                        <div class="w-8 h-8 rounded-lg bg-rose-500/10 text-rose-400 flex items-center justify-center">
                            <i class="fa-solid fa-arrow-up-right text-sm"></i>
                        </div>
                    </div>
                    <div class="text-2xl font-black text-rose-400 font-mono" id="stat-total-expense">0.00</div>
                    <span class="text-[11px] text-slate-500 mt-1 block">تشمل التشغيل والتكاليف</span>
                </div>

                <!-- Net Profit/Loss Card -->
                <div class="glass-card p-5 rounded-2xl">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs font-bold text-slate-400">صافي الأرباح / الخسائر</span>
                        <div class="w-8 h-8 rounded-lg bg-indigo-500/10 text-indigo-400 flex items-center justify-center">
                            <i class="fa-solid fa-chart-line text-sm"></i>
                        </div>
                    </div>
                    <div class="text-2xl font-black text-white font-mono" id="stat-net-profit">0.00</div>
                    <span class="text-[11px] text-slate-500 mt-1 block">الفرق الفعلي الصافي</span>
                </div>

                <!-- Total Transactions Count -->
                <div class="glass-card p-5 rounded-2xl">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs font-bold text-slate-400">إجمالي القيود والسجلات</span>
                        <div class="w-8 h-8 rounded-lg bg-amber-500/10 text-amber-400 flex items-center justify-center">
                            <i class="fa-solid fa-receipt text-sm"></i>
                        </div>
                    </div>
                    <div class="text-2xl font-black text-amber-400 font-mono" id="stat-count">0</div>
                    <span class="text-[11px] text-slate-500 mt-1 block">معاملة محفوظة بالنظام</span>
                </div>
            </div>

            <!-- Main Grid: Add Entry Form & Visual Chart -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- New Transaction Form -->
                <div class="glass-card p-6 rounded-2xl lg:col-span-1">
                    <h3 class="text-base font-bold mb-4 flex items-center gap-2 text-white border-b border-slate-700/60 pb-3">
                        <i class="fa-solid fa-circle-plus text-emerald-400"></i>
                        تسجيل معاملة مالية جديدة
                    </h3>
                    <form id="transaction-form" onsubmit="handleAddTransaction(event)" class="space-y-4">
                        <div>
                            <label class="block text-xs font-bold text-slate-300 mb-1">نوع المعاملة</label>
                            <div class="grid grid-cols-2 gap-2">
                                <label class="flex items-center justify-center gap-2 p-2.5 rounded-xl border border-slate-700 bg-slate-900/60 cursor-pointer hover:bg-slate-800 text-xs font-bold text-slate-200">
                                    <input type="radio" name="tx-type" value="income" checked class="accent-emerald-500">
                                    <i class="fa-solid fa-arrow-down text-emerald-400"></i> إيراد / قبض
                                </label>
                                <label class="flex items-center justify-center gap-2 p-2.5 rounded-xl border border-slate-700 bg-slate-900/60 cursor-pointer hover:bg-slate-800 text-xs font-bold text-slate-200">
                                    <input type="radio" name="tx-type" value="expense" class="accent-rose-500">
                                    <i class="fa-solid fa-arrow-up text-rose-400"></i> مصروف / صرف
                                </label>
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-300 mb-1">المبلغ (<span class="currency-symbol">$</span>)</label>
                            <input type="number" step="0.01" min="0.01" required id="tx-amount" placeholder="0.00" class="w-full bg-slate-900/80 border border-slate-700 rounded-xl px-3 py-2.5 text-sm font-mono text-white focus:outline-none focus:border-emerald-500">
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-300 mb-1">التصنيف / البيان</label>
                            <input type="text" id="tx-category" required placeholder="مثلاً: مبيعات تجزئة، إيجار، رواتب..." class="w-full bg-slate-900/80 border border-slate-700 rounded-xl px-3 py-2.5 text-sm text-white focus:outline-none focus:border-emerald-500">
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-300 mb-1">التاريخ</label>
                            <input type="date" id="tx-date" required class="w-full bg-slate-900/80 border border-slate-700 rounded-xl px-3 py-2.5 text-sm text-white focus:outline-none focus:border-emerald-500">
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-300 mb-1">ملاحظات / معلومات إضافية</label>
                            <textarea id="tx-notes" rows="2" placeholder="اختياري..." class="w-full bg-slate-900/80 border border-slate-700 rounded-xl px-3 py-2.5 text-xs text-white focus:outline-none focus:border-emerald-500"></textarea>
                        </div>

                        <button type="submit" class="w-full py-3 bg-emerald-600 hover:bg-emerald-500 text-white font-bold rounded-xl transition shadow-lg shadow-emerald-900/40 text-sm flex items-center justify-center gap-2">
                            <i class="fa-solid fa-floppy-disk"></i> حفظ المعاملة بالنظام
                        </button>
                    </form>
                </div>

                <!-- Visual Chart & Trend Analysis -->
                <div class="glass-card p-6 rounded-2xl lg:col-span-2 flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-4 border-b border-slate-700/60 pb-3">
                            <h3 class="text-base font-bold flex items-center gap-2 text-white">
                                <i class="fa-solid fa-chart-column text-indigo-400"></i>
                                الرسم البياني للمقارنة المالية
                            </h3>
                            <span class="text-xs text-slate-400">الإيرادات vs المصاريف</span>
                        </div>
                        <div class="relative w-full h-64">
                            <canvas id="financial-chart"></canvas>
                        </div>
                    </div>
                    <div class="mt-4 p-3 bg-slate-900/50 rounded-xl border border-slate-800 text-xs text-slate-400 flex items-center justify-between">
                        <span><i class="fa-solid fa-shield-halved text-emerald-400 ml-1"></i> حفظ تلقائي محلي يضمن سرية بياناتك كاملة</span>
                        <button onclick="renderDashboard()" class="text-emerald-400 hover:underline">تحديث الرسم <i class="fa-solid fa-rotate-right mr-1"></i></button>
                    </div>
                </div>
            </div>

            <!-- Transactions Table View -->
            <div class="glass-card p-6 rounded-2xl space-y-4">
                <div class="flex flex-col sm:flex-row items-center justify-between gap-3 border-b border-slate-700/60 pb-4">
                    <h3 class="text-base font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-list-check text-emerald-400"></i>
                        سجل المعاملات والقيود المالية
                    </h3>
                    <div class="flex items-center gap-2 w-full sm:w-auto">
                        <input type="text" id="tx-search" oninput="renderTransactionsTable()" placeholder="بحث في المعاملات..." class="bg-slate-900/80 border border-slate-700 rounded-xl px-3 py-1.5 text-xs text-white focus:outline-none focus:border-emerald-500 w-full sm:w-48">
                        <select id="tx-filter-type" onchange="renderTransactionsTable()" class="bg-slate-900/80 border border-slate-700 rounded-xl px-3 py-1.5 text-xs text-white focus:outline-none focus:border-emerald-500">
                            <option value="all">كل الأنواع</option>
                            <option value="income">إيرادات فقط</option>
                            <option value="expense">مصاريف فقط</option>
                        </select>
                    </div>
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-right text-xs">
                        <thead>
                            <tr class="text-slate-400 border-b border-slate-700 bg-slate-900/40">
                                <th class="p-3">التاريخ</th>
                                <th class="p-3">النوع</th>
                                <th class="p-3">التصنيف/البيان</th>
                                <th class="p-3">المبلغ</th>
                                <th class="p-3">ملاحظات</th>
                                <th class="p-3 text-center">إجراءات</th>
                            </tr>
                        </thead>
                        <tbody id="transactions-tbody" class="divide-y divide-slate-800 text-slate-200">
                            <!-- Dynamic Content -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- ========================================== -->
        <!-- TAB 2: INDUSTRY-SPECIFIC TOOLS             -->
        <!-- ========================================== -->
        <section id="tab-activity" class="tab-content space-y-6 hidden">
            <!-- Dynamic Sub-Header based on chosen Activity -->
            <div class="glass-card p-6 rounded-2xl flex flex-col md:flex-row items-center justify-between gap-4 border-l-4 border-l-teal-500">
                <div>
                    <span class="text-xs font-bold text-teal-400 uppercase tracking-wider">الأدوات التخصصية</span>
                    <h2 id="activity-title" class="text-xl font-bold text-white">تجارة وتجزئة</h2>
                    <p id="activity-desc" class="text-xs text-slate-400 mt-1">أدوات حساسية هامش الربح، الخصم التجاري وتتبع الهالك والضائع.</p>
                </div>
                <div class="text-xs bg-slate-900/80 px-3 py-2 rounded-xl border border-slate-700 text-slate-300">
                    يمكنك تغيير النشاط من القائمة العلوية في أي وقت
                </div>
            </div>

            <!-- Container for dynamic tool views -->
            <div id="activity-tools-container">
                <!-- Activity tool panels will be dynamically injected here by JS -->
            </div>
        </section>

        <!-- ========================================== -->
        <!-- TAB 3: ADVANCED ACCOUNTING OPERATIONS      -->
        <!-- ========================================== -->
        <section id="tab-advanced" class="tab-content space-y-6 hidden">
            <!-- Advanced Header -->
            <div class="glass-card p-6 rounded-2xl">
                <h2 class="text-xl font-bold text-white mb-1 flex items-center gap-2">
                    <i class="fa-solid fa-scale-balanced text-amber-400"></i>
                    العمليات المحاسبية المتقدمة والقيود الخاصة
                </h2>
                <p class="text-xs text-slate-400">إدارة الشيكات الراجعة، إهلاك الأصول الثابتة، سجل الهالك والتالف، ومتابعة الديون والذمم.</p>
            </div>

            <!-- Sub Navigation Tabs for Advanced Ops -->
            <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
                <button onclick="switchAdvTab('checks')" id="adv-tab-checks" class="adv-btn active p-3 glass-card rounded-xl text-center text-xs font-bold text-amber-400 border-amber-500/30">
                    <i class="fa-solid fa-money-check-dollar text-base block mb-1"></i> الشيكات الراجعة
                </button>
                <button onclick="switchAdvTab('damaged')" id="adv-tab-damaged" class="adv-btn p-3 glass-card rounded-xl text-center text-xs font-bold text-slate-400 hover:text-white">
                    <i class="fa-solid fa-dumpster text-base block mb-1"></i> البضاعة التالفة/الهالك
                </button>
                <button onclick="switchAdvTab('assets')" id="adv-tab-assets" class="adv-btn p-3 glass-card rounded-xl text-center text-xs font-bold text-slate-400 hover:text-white">
                    <i class="fa-solid fa-building-columns text-base block mb-1"></i> إهلاك الأصول الثابتة
                </button>
                <button onclick="switchAdvTab('debts')" id="adv-tab-debts" class="adv-btn p-3 glass-card rounded-xl text-center text-xs font-bold text-slate-400 hover:text-white">
                    <i class="fa-solid fa-hand-holding-dollar text-base block mb-1"></i> الذمم والديون
                </button>
            </div>

            <!-- 1. Bounced Checks Panel -->
            <div id="adv-panel-checks" class="adv-panel space-y-4">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="glass-card p-5 rounded-2xl">
                        <h3 class="text-sm font-bold text-white mb-3">تسجيل شيك مرتجع / راجع</h3>
                        <form onsubmit="handleCheckSubmit(event)" class="space-y-3">
                            <input type="text" id="check-num" placeholder="رقم الشيك" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <input type="text" id="check-bank" placeholder="اسم البنك" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <input type="text" id="check-client" placeholder="اسم الساحب / العميل" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <input type="number" step="0.01" id="check-amount" placeholder="مبلغ الشيك" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white font-mono">
                            <input type="date" id="check-date" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <button type="submit" class="w-full py-2.5 bg-amber-600 hover:bg-amber-500 text-white font-bold rounded-xl text-xs transition">
                                إدراج الشيك الراجع
                            </button>
                        </form>
                    </div>

                    <div class="glass-card p-5 rounded-2xl lg:col-span-2 overflow-x-auto">
                        <h3 class="text-sm font-bold text-white mb-3">جدول متابعة الشيكات المرتجعة</h3>
                        <table class="w-full text-right text-xs">
                            <thead>
                                <tr class="text-slate-400 border-b border-slate-700">
                                    <th class="p-2">رقم الشيك</th>
                                    <th class="p-2">البنك</th>
                                    <th class="p-2">العميل</th>
                                    <th class="p-2">المبلغ</th>
                                    <th class="p-2">التاريخ</th>
                                    <th class="p-2 text-center">إجراء</th>
                                </tr>
                            </thead>
                            <tbody id="checks-tbody" class="divide-y divide-slate-800"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- 2. Damaged Goods Panel -->
            <div id="adv-panel-damaged" class="adv-panel space-y-4 hidden">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="glass-card p-5 rounded-2xl">
                        <h3 class="text-sm font-bold text-white mb-3">إثبات بضاعة تالفة / هالك</h3>
                        <form onsubmit="handleDamagedSubmit(event)" class="space-y-3">
                            <input type="text" id="dmg-item" placeholder="اسم الصنف / البضاعة" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <input type="number" id="dmg-qty" placeholder="الكمية التالفة" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white font-mono">
                            <input type="number" step="0.01" id="dmg-cost" placeholder="تكلفة الوحدة الواحدة" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white font-mono">
                            <input type="text" id="dmg-reason" placeholder="سبب التلف (سوء تخزين، انتهاء صلاحية...)" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <button type="submit" class="w-full py-2.5 bg-rose-600 hover:bg-rose-500 text-white font-bold rounded-xl text-xs transition">
                                إثبات الهالك وقيد المصروف
                            </button>
                        </form>
                    </div>

                    <div class="glass-card p-5 rounded-2xl lg:col-span-2 overflow-x-auto">
                        <h3 class="text-sm font-bold text-white mb-3">سجل تلفيات المخزون الهالك</h3>
                        <table class="w-full text-right text-xs">
                            <thead>
                                <tr class="text-slate-400 border-b border-slate-700">
                                    <th class="p-2">الصنف</th>
                                    <th class="p-2">الكمية</th>
                                    <th class="p-2">ت. الوحدة</th>
                                    <th class="p-2">الإجمالي</th>
                                    <th class="p-2">السبب</th>
                                    <th class="p-2 text-center">إجراء</th>
                                </tr>
                            </thead>
                            <tbody id="damaged-tbody" class="divide-y divide-slate-800"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- 3. Fixed Asset Depreciation Panel -->
            <div id="adv-panel-assets" class="adv-panel space-y-4 hidden">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="glass-card p-5 rounded-2xl">
                        <h3 class="text-sm font-bold text-white mb-3">حاسبة واحتساب إهلاك أصل ثابت</h3>
                        <form onsubmit="handleAssetSubmit(event)" class="space-y-3">
                            <input type="text" id="asset-name" placeholder="اسم الأصل (سيارة، جهاز، ميكنة...)" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <input type="number" step="0.01" id="asset-cost" placeholder="تكلفة الشراء الأصلية" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white font-mono">
                            <input type="number" step="0.01" id="asset-salvage" placeholder="القيمة الخردة المتوقعة" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white font-mono">
                            <input type="number" id="asset-years" placeholder="العمر الإنتاجي (بالسنوات)" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white font-mono">
                            <button type="submit" class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-bold rounded-xl text-xs transition">
                                احتساب قسط الإهلاك السنوي
                            </button>
                        </form>
                    </div>

                    <div class="glass-card p-5 rounded-2xl lg:col-span-2 overflow-x-auto">
                        <h3 class="text-sm font-bold text-white mb-3">جدول إهلاك الأصول الثابتة (القسط الثابت)</h3>
                        <table class="w-full text-right text-xs">
                            <thead>
                                <tr class="text-slate-400 border-b border-slate-700">
                                    <th class="p-2">الأصل</th>
                                    <th class="p-2">التكلفة</th>
                                    <th class="p-2">الخردة</th>
                                    <th class="p-2">العمر</th>
                                    <th class="p-2">الإهلاك السنوي</th>
                                    <th class="p-2 text-center">إجراء</th>
                                </tr>
                            </thead>
                            <tbody id="assets-tbody" class="divide-y divide-slate-800"></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- 4. Debts Panel -->
            <div id="adv-panel-debts" class="adv-panel space-y-4 hidden">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="glass-card p-5 rounded-2xl">
                        <h3 class="text-sm font-bold text-white mb-3">تسجيل دين / ذمة جديدة</h3>
                        <form onsubmit="handleDebtSubmit(event)" class="space-y-3">
                            <input type="text" id="debt-name" placeholder="اسم الشريك / العميل / المورد" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <select id="debt-type" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                                <option value="debtor">لنا عليه (لنا فلوس / ذمم مدينة)</option>
                                <option value="creditor">علينا له (علينا فلوس / ذمم دائنة)</option>
                            </select>
                            <input type="number" step="0.01" id="debt-amount" placeholder="المبلغ" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white font-mono">
                            <input type="date" id="debt-date" required class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                            <button type="submit" class="w-full py-2.5 bg-teal-600 hover:bg-teal-500 text-white font-bold rounded-xl text-xs transition">
                                حفظ الدين بالنظام
                            </button>
                        </form>
                    </div>

                    <div class="glass-card p-5 rounded-2xl lg:col-span-2 overflow-x-auto">
                        <h3 class="text-sm font-bold text-white mb-3">سجل الذمم المالية المتبقية</h3>
                        <table class="w-full text-right text-xs">
                            <thead>
                                <tr class="text-slate-400 border-b border-slate-700">
                                    <th class="p-2">الاسم</th>
                                    <th class="p-2">النوع</th>
                                    <th class="p-2">المبلغ</th>
                                    <th class="p-2">التاريخ</th>
                                    <th class="p-2 text-center">إجراء</th>
                                </tr>
                            </thead>
                            <tbody id="debts-tbody" class="divide-y divide-slate-800"></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

        <!-- ========================================== -->
        <!-- TAB 4: OFFICIAL REPORTS & PRINTING         -->
        <!-- ========================================== -->
        <section id="tab-reports" class="tab-content space-y-6 hidden">
            <!-- Report Actions Bar -->
            <div class="glass-card p-6 rounded-2xl flex flex-col sm:flex-row items-center justify-between gap-4">
                <div>
                    <h2 class="text-xl font-bold text-white mb-1">التقرير المالي الرسمي المعتمد</h2>
                    <p class="text-xs text-slate-400">ملخص البيان المالي موثق بهوية المطور لطباعته أو تصديره للمراجعة.</p>
                </div>
                <button onclick="window.print()" class="w-full sm:w-auto px-6 py-3 bg-emerald-600 hover:bg-emerald-500 text-white font-bold rounded-xl transition shadow-lg shadow-emerald-900/40 text-sm flex items-center justify-center gap-2">
                    <i class="fa-solid fa-print"></i> طباعة التقرير المالي الآن
                </button>
            </div>

            <!-- Printable Statement View Container -->
            <div id="printable-section" class="glass-card p-8 rounded-2xl space-y-6 text-slate-100">
                <!-- Formal Header -->
                <div class="flex items-center justify-between border-b-2 border-emerald-500/40 pb-6">
                    <div>
                        <h1 class="text-3xl font-black text-emerald-400 mb-1">محاسبي - Muhasibi</h1>
                        <p class="text-xs text-slate-300 font-semibold">تقرير البيان المالي والحسابات الختامية</p>
                        <p class="text-[11px] text-emerald-400 mt-1 font-bold">
                            تطوير وإعداد المحاسب: براء نادر توفيق ناصر
                        </p>
                    </div>
                    <div class="text-left text-xs text-slate-400 space-y-1">
                        <div><strong class="text-slate-200">النشاط التجاري:</strong> <span id="report-activity">تجارة وتجزئة</span></div>
                        <div><strong class="text-slate-200">تاريخ الإصدار:</strong> <span id="report-date">--</span></div>
                        <div><strong class="text-slate-200">حالة المستند:</strong> <span class="text-emerald-400">مصدق ومحدث</span></div>
                    </div>
                </div>

                <!-- Income Statement Grid -->
                <div class="grid grid-cols-3 gap-4 text-center my-6">
                    <div class="p-4 rounded-xl bg-slate-900/60 border border-slate-700">
                        <span class="text-xs text-slate-400 block mb-1">مجموع المقبوضات (الإيراد)</span>
                        <span id="report-income" class="text-xl font-black text-emerald-400 font-mono">0.00</span>
                    </div>
                    <div class="p-4 rounded-xl bg-slate-900/60 border border-slate-700">
                        <span class="text-xs text-slate-400 block mb-1">مجموع المدفوعات (المصروف)</span>
                        <span id="report-expense" class="text-xl font-black text-rose-400 font-mono">0.00</span>
                    </div>
                    <div class="p-4 rounded-xl bg-slate-900/60 border border-slate-700">
                        <span class="text-xs text-slate-400 block mb-1">صافي نتيجة الفترة</span>
                        <span id="report-net" class="text-xl font-black text-white font-mono">0.00</span>
                    </div>
                </div>

                <!-- Summary Breakdown Table -->
                <div>
                    <h4 class="text-sm font-bold text-slate-200 mb-3 border-r-2 border-emerald-500 pr-2">تفاصيل المعاملات المالية المعتمدة</h4>
                    <table class="w-full text-right text-xs border border-slate-700">
                        <thead>
                            <tr class="bg-slate-900 text-slate-300">
                                <th class="p-2.5 border-b border-slate-700">التاريخ</th>
                                <th class="p-2.5 border-b border-slate-700">النوع</th>
                                <th class="p-2.5 border-b border-slate-700">التصنيف</th>
                                <th class="p-2.5 border-b border-slate-700">المبلغ</th>
                                <th class="p-2.5 border-b border-slate-700">الملاحظات</th>
                            </tr>
                        </thead>
                        <tbody id="report-table-body" class="divide-y divide-slate-800">
                            <!-- Dynamic Content -->
                        </tbody>
                    </table>
                </div>

                <!-- Official Signature Footer -->
                <div class="pt-8 border-t border-slate-700 flex justify-between items-end text-xs text-slate-400">
                    <div>
                        <p class="font-bold text-slate-200">ختم الاعتماد المحاسبي النظامي</p>
                        <p class="text-[10px] text-slate-400">تطبيق محاسبي - تطوير المحاسب براء نادر توفيق ناصر</p>
                    </div>
                    <div class="text-center">
                        <div class="h-10 border-b border-dashed border-slate-600 mb-1 w-40"></div>
                        <p>توقيع المحاسب المسؤول</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- ========================================== -->
        <!-- TAB 5: AI ASSISTANT & GUIDE                -->
        <!-- ========================================== -->
        <section id="tab-assistant" class="tab-content space-y-6 hidden">
            <div class="glass-card p-6 rounded-2xl flex items-center justify-between gap-4 border-r-4 border-r-indigo-500">
                <div>
                    <h2 class="text-xl font-bold text-white mb-1 flex items-center gap-2">
                        <i class="fa-solid fa-robot text-indigo-400"></i>
                        المساعد المحاسبي الذكي ودليل الاستخدام
                    </h2>
                    <p class="text-xs text-slate-300">اسأل المساعد الذكي عن أي استفسار محاسبي أو كيفية التعامل مع أدوات الموقع.</p>
                </div>
            </div>

            <!-- AI Chat Wrapper -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Suggested Quick Chips -->
                <div class="glass-card p-5 rounded-2xl space-y-3">
                    <h3 class="text-sm font-bold text-white mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-lightbulb text-amber-400"></i>
                        أسئلة سريعة وشائعة
                    </h3>
                    <div class="space-y-2">
                        <button onclick="askAI('كيف أقيد شيك مرتجع؟')" class="w-full text-right p-2.5 rounded-xl bg-slate-900/80 hover:bg-slate-800 border border-slate-700/80 text-xs text-slate-300 hover:text-white transition block">
                            📌 كيف أقيد شيك مرتجع؟
                        </button>
                        <button onclick="askAI('كيف يتم حساب إهلاك الأصول؟')" class="w-full text-right p-2.5 rounded-xl bg-slate-900/80 hover:bg-slate-800 border border-slate-700/80 text-xs text-slate-300 hover:text-white transition block">
                            📌 كيف يتم حساب إهلاك الأصول؟
                        </button>
                        <button onclick="askAI('كيف أتعامل مع البضاعة التالفة؟')" class="w-full text-right p-2.5 rounded-xl bg-slate-900/80 hover:bg-slate-800 border border-slate-700/80 text-xs text-slate-300 hover:text-white transition block">
                            📌 كيف أتعامل مع البضاعة التالفة؟
                        </button>
                        <button onclick="askAI('شرح عن نشاط العقارات والمخازن')" class="w-full text-right p-2.5 rounded-xl bg-slate-900/80 hover:bg-slate-800 border border-slate-700/80 text-xs text-slate-300 hover:text-white transition block">
                            📌 شرح عن نشاط العقارات والمخازن
                        </button>
                        <button onclick="askAI('طريقة تصدير واسترجاع البيانات')" class="w-full text-right p-2.5 rounded-xl bg-slate-900/80 hover:bg-slate-800 border border-slate-700/80 text-xs text-slate-300 hover:text-white transition block">
                            📌 طريقة تصدير واسترجاع البيانات
                        </button>
                    </div>
                </div>

                <!-- Chat Box Window -->
                <div class="glass-card p-5 rounded-2xl lg:col-span-2 flex flex-col h-[480px]">
                    <div id="ai-chat-history" class="flex-1 overflow-y-auto space-y-4 p-2">
                        <!-- Welcome message -->
                        <div class="flex gap-3 items-start">
                            <div class="w-8 h-8 rounded-full bg-indigo-600 flex items-center justify-center text-white shrink-0 text-xs">
                                <i class="fa-solid fa-robot"></i>
                            </div>
                            <div class="bg-slate-800 p-3.5 rounded-2xl rounded-tr-none text-xs text-slate-200 max-w-[85%] leading-relaxed">
                                مرحباً بك! أنا مساعدك المحاسبي الذكي الخاص بأداة <strong>محاسبي</strong> (تطوير المحاسب براء نادر توفيق ناصر). 
                                يمكنك طرح أي سؤال عن القيود، كيفية التعامل مع الأنشطة، أو طريقة الحسابات، وسأجيبك فوراً!
                            </div>
                        </div>
                    </div>

                    <!-- Input Bar -->
                    <form onsubmit="handleAISubmit(event)" class="mt-4 flex gap-2">
                        <input type="text" id="ai-input" placeholder="اكتب سؤالك أو استفسارك المحاسبي هنا..." class="flex-1 bg-slate-900/90 border border-slate-700 rounded-xl px-4 py-3 text-xs text-white focus:outline-none focus:border-indigo-500">
                        <button type="submit" class="px-5 py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-bold rounded-xl text-xs transition">
                            إرسال <i class="fa-solid fa-paper-plane mr-1"></i>
                        </button>
                    </form>
                </div>
            </div>
        </section>

        <!-- ========================================== -->
        <!-- TAB 6: SETTINGS & SECURITY                -->
        <!-- ========================================== -->
        <section id="tab-settings" class="tab-content space-y-6 hidden">
            <div class="glass-card p-6 rounded-2xl">
                <h2 class="text-xl font-bold text-white mb-1">الإعدادات والأمان وضبط النظام</h2>
                <p class="text-xs text-slate-400">تخصيص النشاط التجاري، رمز PIN للخصوصية، والنسخ الاحتياطي للبيانات.</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Activity Switcher Setting -->
                <div class="glass-card p-5 rounded-2xl space-y-3">
                    <h3 class="text-sm font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-briefcase text-emerald-400"></i> النشاط التجاري المعتمد
                    </h3>
                    <p class="text-xs text-slate-400">حدد نوع النشاط التجاري لتشغيل الأدوات المخصصة المناسبة لطبيعة عملك (بدون اختيار عشوائي).</p>
                    <select id="setting-activity-select" onchange="changeActivity(this.value)" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-slate-200">
                        <option value="retail">تجارة وتجزئة (مبيعات، خصومات، هالك)</option>
                        <option value="realestate">عقارات ومخازن (مستأجرين، عقود، إشغال)</option>
                        <option value="manufacturing">تصنيع وإنتاج (تكلفة مواد خام، عمالة، قطعة)</option>
                        <option value="services">خدمات واستشارات (ساعات عمل، هامش الربح)</option>
                    </select>
                </div>

                <!-- Currency Setting -->
                <div class="glass-card p-5 rounded-2xl space-y-3">
                    <h3 class="text-sm font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-coins text-amber-400"></i> العملة الأساسية
                    </h3>
                    <p class="text-xs text-slate-400">اختر رمز العملة الافتراضي للتقارير والمعاملات.</p>
                    <select id="setting-currency-select" onchange="changeCurrency(this.value)" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-slate-200">
                        <option value="USD">دولار أمريكي ($)</option>
                        <option value="ILS">شيكل جديد (₪)</option>
                        <option value="JOD">دينار أردني (JD)</option>
                        <option value="EUR">يورو (€)</option>
                    </select>
                </div>

                <!-- Security PIN Setting -->
                <div class="glass-card p-5 rounded-2xl space-y-3">
                    <h3 class="text-sm font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-shield-halved text-indigo-400"></i> حماية النظام برمز PIN
                    </h3>
                    <p class="text-xs text-slate-400">تعيين رمز أمان لمنع أي شخص آخر من الاطلاع على بياناتك المحاسبية عند فتح الموقع.</p>
                    <div class="flex gap-2">
                        <input type="password" id="setting-pin-input" maxlength="6" placeholder="أدخل رمز جديد..." class="flex-1 bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-xs text-white">
                        <button onclick="savePinSetting()" class="px-4 py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white text-xs font-bold rounded-xl transition">حفظ الرمز</button>
                    </div>
                </div>

                <!-- Data Backup & Reset -->
                <div class="glass-card p-5 rounded-2xl space-y-3">
                    <h3 class="text-sm font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-database text-rose-400"></i> النسخ الاحتياطي والبيانات
                    </h3>
                    <p class="text-xs text-slate-400">تصدير جميع معاملاتك لملف JSON خارجي أو استعادتها.</p>
                    <div class="flex flex-wrap gap-2">
                        <button onclick="exportDataJSON()" class="px-3 py-2 bg-slate-800 hover:bg-slate-700 border border-slate-700 rounded-xl text-xs font-bold text-slate-200 transition">
                            <i class="fa-solid fa-download ml-1"></i> تصدير البيانات
                        </button>
                        <button onclick="clearAllData()" class="px-3 py-2 bg-rose-950/60 hover:bg-rose-900 border border-rose-800 rounded-xl text-xs font-bold text-rose-300 transition">
                            <i class="fa-solid fa-trash ml-1"></i> مسح البيانات بالكامل
                        </button>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer Branding -->
    <footer class="mt-auto border-t border-slate-800 py-6 px-4 text-center text-xs text-slate-400 bg-slate-950/60">
        <div class="max-w-7xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-3">
            <div class="flex items-center gap-2">
                <span class="font-extrabold text-white">محاسبي</span>
                <span>— جميع الحقوق محفوظة © 2026</span>
            </div>
            <div class="text-emerald-400 font-bold bg-emerald-500/10 px-3 py-1 rounded-full border border-emerald-500/20">
                تطوير وإعداد المحاسب: براء نادر توفيق ناصر
            </div>
        </div>
    </footer>

    <script>
        /* ==========================================================================
           STATE MANAGEMENT & INITIALIZATION
           ========================================================================== */
        const STORAGE_KEY = 'muhasibi_app_state_v3';

        // Core App State (Never picks random activity!)
        let appState = {
            activity: 'retail', // Default explicitly set to 'retail'
            currency: 'USD',
            currencySymbol: '$',
            pin: null,
            transactions: [],
            checks: [],
            damaged: [],
            assets: [],
            debts: []
        };

        let financialChartInstance = null;

        // On Application Load
        window.onload = function() {
            loadStateFromStorage();
            
            // Check if PIN protection is active
            if (appState.pin) {
                document.getElementById('pin-modal').classList.remove('hidden');
            }

            // Sync Header & Selects
            document.getElementById('header-activity-select').value = appState.activity;
            document.getElementById('setting-activity-select').value = appState.activity;
            document.getElementById('setting-currency-select').value = appState.currency;
            
            // Set default date picker to today
            document.getElementById('tx-date').value = new Date().toISOString().split('T')[0];
            
            // Refresh UI
            updateActivityUI();
            renderDashboard();
            renderAdvancedOperations();
            
            showToast('أهلاً بك في منصة محاسبي!', 'info');
        };

        function saveStateToStorage() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(appState));
        }

        function loadStateFromStorage() {
            const data = localStorage.getItem(STORAGE_KEY);
            if (data) {
                try {
                    appState = Object.assign({}, appState, JSON.parse(data));
                } catch(e) {
                    console.error("Error reading localStorage", e);
                }
            }
        }

        /* ==========================================================================
           NAVIGATION & TAB SWITCHING
           ========================================================================== */
        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.tab-btn').forEach(el => {
                el.classList.remove('active', 'text-emerald-400', 'bg-emerald-500/10', 'border', 'border-emerald-500/20');
                el.classList.add('text-slate-400');
            });

            const targetTab = document.getElementById(`tab-${tabName}`);
            const targetBtn = document.getElementById(`nav-btn-${tabName}`);

            if (targetTab && targetBtn) {
                targetTab.classList.remove('hidden');
                targetBtn.classList.add('active', 'text-emerald-400', 'bg-emerald-500/10', 'border', 'border-emerald-500/20');
                targetBtn.classList.remove('text-slate-400');
            }

            if (tabName === 'reports') {
                renderReports();
            }
        }

        function switchAdvTab(advName) {
            document.querySelectorAll('.adv-panel').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.adv-btn').forEach(el => {
                el.classList.remove('active', 'text-amber-400', 'border-amber-500/30');
                el.classList.add('text-slate-400');
            });

            document.getElementById(`adv-panel-${advName}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`adv-tab-${advName}`);
            activeBtn.classList.add('active', 'text-amber-400', 'border-amber-500/30');
            activeBtn.classList.remove('text-slate-400');
        }

        /* ==========================================================================
           ACTIVITY SWITCHER LOGIC (Deterministic & Fixed)
           ========================================================================== */
        const activityNames = {
            retail: 'تجارة وتجزئة',
            realestate: 'عقارات ومخازن',
            manufacturing: 'تصنيع وإنتاج',
            services: 'خدمات واستشارات'
        };

        function changeActivity(newActivity) {
            appState.activity = newActivity;
            saveStateToStorage();

            document.getElementById('header-activity-select').value = newActivity;
            document.getElementById('setting-activity-select').value = newActivity;

            updateActivityUI();
            showToast(`تم تغيير النشاط التجاري إلى: ${activityNames[newActivity]}`, 'success');
        }

        function updateActivityUI() {
            const name = activityNames[appState.activity] || 'تجارة وتجزئة';
            
            // Update Badges
            document.getElementById('current-activity-badge').innerText = name;
            document.getElementById('banner-activity-name').innerText = name;
            document.getElementById('activity-title').innerText = name;

            // Render Dynamic Tools inside Tab 2
            const container = document.getElementById('activity-tools-container');
            
            if (appState.activity === 'retail') {
                container.innerHTML = `
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="glass-card p-5 rounded-2xl">
                            <h3 class="text-sm font-bold text-white mb-3">حاسبة هامش الربح والنسبة المئوية</h3>
                            <div class="space-y-3 text-xs">
                                <div>
                                    <label class="block text-slate-400 mb-1">تكلفة الشراء (سعر التكلفة)</label>
                                    <input type="number" id="rt-cost" oninput="calcRetailMargin()" placeholder="0.00" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2 text-white font-mono">
                                </div>
                                <div>
                                    <label class="block text-slate-400 mb-1">سعر البيع المقترح</label>
                                    <input type="number" id="rt-price" oninput="calcRetailMargin()" placeholder="0.00" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2 text-white font-mono">
                                </div>
                                <div class="p-3 bg-slate-900/80 rounded-xl border border-slate-700 space-y-1">
                                    <div class="flex justify-between"><span>ربح القطعة:</span> <strong id="rt-res-profit" class="text-emerald-400 font-mono">0.00</strong></div>
                                    <div class="flex justify-between"><span>نسبة الهامش:</span> <strong id="rt-res-margin" class="text-teal-400 font-mono">0%</strong></div>
                                </div>
                            </div>
                        </div>

                        <div class="glass-card p-5 rounded-2xl">
                            <h3 class="text-sm font-bold text-white mb-3">حاسبة الخصم التجاري والتنزيلات</h3>
                            <div class="space-y-3 text-xs">
                                <div>
                                    <label class="block text-slate-400 mb-1">السعر الأصلي للمنتج</label>
                                    <input type="number" id="disc-price" oninput="calcDiscount()" placeholder="0.00" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2 text-white font-mono">
                                </div>
                                <div>
                                    <label class="block text-slate-400 mb-1">نسبة الخصم (%)</label>
                                    <input type="number" id="disc-pct" oninput="calcDiscount()" placeholder="15" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2 text-white font-mono">
                                </div>
                                <div class="p-3 bg-slate-900/80 rounded-xl border border-slate-700 space-y-1">
                                    <div class="flex justify-between"><span>قيمة الخصم:</span> <strong id="disc-res-amount" class="text-rose-400 font-mono">0.00</strong></div>
                                    <div class="flex justify-between"><span>السعر بعد الخصم:</span> <strong id="disc-res-final" class="text-emerald-400 font-mono">0.00</strong></div>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            } else if (appState.activity === 'realestate') {
                container.innerHTML = `
                    <div class="glass-card p-5 rounded-2xl space-y-4">
                        <h3 class="text-sm font-bold text-white">إدارة العقارات والوحدات المؤجرة</h3>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-3 text-xs">
                            <input type="text" id="re-unit" placeholder="رقم / اسم المحل أو المخزن" class="bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-white">
                            <input type="text" id="re-tenant" placeholder="اسم المستأجر" class="bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-white">
                            <input type="number" id="re-rent" placeholder="الإيجار الشهري" class="bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-white font-mono">
                        </div>
                        <p class="text-xs text-slate-400">تتيح لك هذه الأداة متابعة الشواغر وإيرادات العقارات المباشرة.</p>
                    </div>
                `;
            } else if (appState.activity === 'manufacturing') {
                container.innerHTML = `
                    <div class="glass-card p-5 rounded-2xl space-y-4">
                        <h3 class="text-sm font-bold text-white">حاسبة تكلفة المنتج المصنع (تكلفة الوحدة)</h3>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-3 text-xs">
                            <div>
                                <label class="block text-slate-400 mb-1">تكلفة المواد الخام المباشرة</label>
                                <input type="number" id="mfg-raw" oninput="calcMfgCost()" placeholder="0.00" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-white font-mono">
                            </div>
                            <div>
                                <label class="block text-slate-400 mb-1">تكلفة أجور العمالة المباشرة</label>
                                <input type="number" id="mfg-labor" oninput="calcMfgCost()" placeholder="0.00" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-white font-mono">
                            </div>
                            <div>
                                <label class="block text-slate-400 mb-1">مصاريف صناعية غير مباشرة</label>
                                <input type="number" id="mfg-overhead" oninput="calcMfgCost()" placeholder="0.00" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-white font-mono">
                            </div>
                        </div>
                        <div class="p-3 bg-slate-900/80 rounded-xl border border-slate-700 text-xs flex justify-between items-center">
                            <span>إجمالي تكلفة إنتاج القطعة الواحدة:</span>
                            <strong id="mfg-res-total" class="text-lg font-bold text-emerald-400 font-mono">0.00</strong>
                        </div>
                    </div>
                `;
            } else if (appState.activity === 'services') {
                container.innerHTML = `
                    <div class="glass-card p-5 rounded-2xl space-y-4">
                        <h3 class="text-sm font-bold text-white">حاسبة تسعير الخدمة والاستشارة</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-xs">
                            <div>
                                <label class="block text-slate-400 mb-1">عدد الساعات المتوقعة للمشروع</label>
                                <input type="number" id="srv-hours" oninput="calcServiceCost()" placeholder="10" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-white font-mono">
                            </div>
                            <div>
                                <label class="block text-slate-400 mb-1">تكلفة/سعر الساعة الواحدة</label>
                                <input type="number" id="srv-rate" oninput="calcServiceCost()" placeholder="25.00" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-2.5 text-white font-mono">
                            </div>
                        </div>
                        <div class="p-3 bg-slate-900/80 rounded-xl border border-slate-700 text-xs flex justify-between items-center">
                            <span>القيمة الإجمالية المقترحة للعقد:</span>
                            <strong id="srv-res-total" class="text-lg font-bold text-indigo-400 font-mono">0.00</strong>
                        </div>
                    </div>
                `;
            }
        }

        // Retail Calculations
        function calcRetailMargin() {
            const cost = parseFloat(document.getElementById('rt-cost').value) || 0;
            const price = parseFloat(document.getElementById('rt-price').value) || 0;
            const profit = price - cost;
            const margin = price > 0 ? ((profit / price) * 100).toFixed(1) : 0;

            document.getElementById('rt-res-profit').innerText = `${profit.toFixed(2)} ${appState.currencySymbol}`;
            document.getElementById('rt-res-margin').innerText = `${margin}%`;
        }

        function calcDiscount() {
            const price = parseFloat(document.getElementById('disc-price').value) || 0;
            const pct = parseFloat(document.getElementById('disc-pct').value) || 0;
            const discountAmt = (price * pct) / 100;
            const finalPrice = price - discountAmt;

            document.getElementById('disc-res-amount').innerText = `${discountAmt.toFixed(2)} ${appState.currencySymbol}`;
            document.getElementById('disc-res-final').innerText = `${finalPrice.toFixed(2)} ${appState.currencySymbol}`;
        }

        function calcMfgCost() {
            const raw = parseFloat(document.getElementById('mfg-raw').value) || 0;
            const labor = parseFloat(document.getElementById('mfg-labor').value) || 0;
            const overhead = parseFloat(document.getElementById('mfg-overhead').value) || 0;
            const total = raw + labor + overhead;
            document.getElementById('mfg-res-total').innerText = `${total.toFixed(2)} ${appState.currencySymbol}`;
        }

        function calcServiceCost() {
            const hours = parseFloat(document.getElementById('srv-hours').value) || 0;
            const rate = parseFloat(document.getElementById('srv-rate').value) || 0;
            const total = hours * rate;
            document.getElementById('srv-res-total').innerText = `${total.toFixed(2)} ${appState.currencySymbol}`;
        }

        /* ==========================================================================
           TRANSACTION MANAGEMENT
           ========================================================================== */
        function handleAddTransaction(e) {
            e.preventDefault();
            const type = document.querySelector('input[name="tx-type"]:checked').value;
            const amount = parseFloat(document.getElementById('tx-amount').value);
            const category = document.getElementById('tx-category').value;
            const date = document.getElementById('tx-date').value;
            const notes = document.getElementById('tx-notes').value;

            if (!amount || amount <= 0) {
                showToast('يرجى إدخال مبلغ صحيح', 'error');
                return;
            }

            const newTx = {
                id: Date.now(),
                type,
                amount,
                category,
                date,
                notes,
                activity: appState.activity
            };

            appState.transactions.unshift(newTx);
            saveStateToStorage();
            renderDashboard();

            // Reset form
            document.getElementById('tx-amount').value = '';
            document.getElementById('tx-category').value = '';
            document.getElementById('tx-notes').value = '';
            
            showToast('تم تسجيل المعاملة بنجاح ✅', 'success');
        }

        function deleteTransaction(id) {
            appState.transactions = appState.transactions.filter(t => t.id !== id);
            saveStateToStorage();
            renderDashboard();
            showToast('تم حذف المعاملة', 'info');
        }

        function renderDashboard() {
            // Symbols
            document.querySelectorAll('.currency-symbol').forEach(el => el.innerText = appState.currencySymbol);

            // Calculations
            let income = 0;
            let expense = 0;

            appState.transactions.forEach(t => {
                if (t.type === 'income') income += t.amount;
                if (t.type === 'expense') expense += t.amount;
            });

            const net = income - expense;

            document.getElementById('stat-total-income').innerText = `${income.toFixed(2)} ${appState.currencySymbol}`;
            document.getElementById('stat-total-expense').innerText = `${expense.toFixed(2)} ${appState.currencySymbol}`;
            
            const netEl = document.getElementById('stat-net-profit');
            netEl.innerText = `${net.toFixed(2)} ${appState.currencySymbol}`;
            netEl.className = `text-2xl font-black font-mono ${net >= 0 ? 'text-emerald-400' : 'text-rose-400'}`;

            document.getElementById('stat-count').innerText = appState.transactions.length;

            renderTransactionsTable();
            renderChart(income, expense);
        }

        function renderTransactionsTable() {
            const tbody = document.getElementById('transactions-tbody');
            const search = (document.getElementById('tx-search').value || '').toLowerCase();
            const filterType = document.getElementById('tx-filter-type').value;

            const filtered = appState.transactions.filter(t => {
                const matchesSearch = t.category.toLowerCase().includes(search) || (t.notes && t.notes.toLowerCase().includes(search));
                const matchesType = filterType === 'all' || t.type === filterType;
                return matchesSearch && matchesType;
            });

            if (filtered.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center py-6 text-slate-500">لا توجد معاملات مسجلة حالياً</td>
                    </tr>
                `;
                return;
            }

            tbody.innerHTML = filtered.map(t => `
                <tr class="hover:bg-slate-800/40 transition">
                    <td class="p-3 text-slate-400 font-mono">${t.date}</td>
                    <td class="p-3">
                        <span class="px-2 py-0.5 rounded-full text-[10px] font-bold ${t.type === 'income' ? 'bg-emerald-500/10 text-emerald-400' : 'bg-rose-500/10 text-rose-400'}">
                            ${t.type === 'income' ? 'إيراد / قبض' : 'مصروف / صرف'}
                        </span>
                    </td>
                    <td class="p-3 font-semibold text-white">${escapeHtml(t.category)}</td>
                    <td class="p-3 font-mono font-bold ${t.type === 'income' ? 'text-emerald-400' : 'text-rose-400'}">
                        ${t.type === 'income' ? '+' : '-'}${t.amount.toFixed(2)} ${appState.currencySymbol}
                    </td>
                    <td class="p-3 text-slate-400">${escapeHtml(t.notes || '-')}</td>
                    <td class="p-3 text-center">
                        <button onclick="deleteTransaction(${t.id})" class="p-1 text-slate-500 hover:text-rose-400 transition" title="حذف">
                            <i class="fa-solid fa-trash-can"></i>
                        </button>
                    </td>
                </tr>
            `).join('');
        }

        /* ==========================================================================
           CHART.JS RENDERING
           ========================================================================== */
        function renderChart(income, expense) {
            const ctx = document.getElementById('financial-chart').getContext('2d');
            
            if (financialChartInstance) {
                financialChartInstance.destroy();
            }

            financialChartInstance = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['الإيرادات المقبوضة', 'المصاريف والنفقات', 'صافي النتيجة'],
                    datasets: [{
                        label: 'المبلغ المالي',
                        data: [income, expense, income - expense],
                        backgroundColor: [
                            'rgba(16, 185, 129, 0.7)',
                            'rgba(244, 63, 94, 0.7)',
                            'rgba(99, 102, 241, 0.7)'
                        ],
                        borderColor: [
                            '#10b981',
                            '#f43f5e',
                            '#6366f1'
                        ],
                        borderWidth: 1.5,
                        borderRadius: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    },
                    scales: {
                        y: {
                            grid: { color: 'rgba(255,255,255,0.05)' },
                            ticks: { color: '#94a3b8', font: { family: 'Cairo' } }
                        },
                        x: {
                            grid: { display: false },
                            ticks: { color: '#f8fafc', font: { family: 'Cairo', weight: 'bold' } }
                        }
                    }
                }
            });
        }

        /* ==========================================================================
           ADVANCED OPERATIONS HANDLERS
           ========================================================================== */
        function handleCheckSubmit(e) {
            e.preventDefault();
            const item = {
                id: Date.now(),
                num: document.getElementById('check-num').value,
                bank: document.getElementById('check-bank').value,
                client: document.getElementById('check-client').value,
                amount: parseFloat(document.getElementById('check-amount').value),
                date: document.getElementById('check-date').value
            };
            appState.checks.unshift(item);
            saveStateToStorage();
            renderAdvancedOperations();
            e.target.reset();
            showToast('تم إدراج الشيك المرتجع', 'success');
        }

        function handleDamagedSubmit(e) {
            e.preventDefault();
            const item = {
                id: Date.now(),
                item: document.getElementById('dmg-item').value,
                qty: parseInt(document.getElementById('dmg-qty').value),
                cost: parseFloat(document.getElementById('dmg-cost').value),
                reason: document.getElementById('dmg-reason').value
            };
            appState.damaged.unshift(item);
            saveStateToStorage();
            renderAdvancedOperations();
            e.target.reset();
            showToast('تم تسجيل البضاعة التالفة', 'warning');
        }

        function handleAssetSubmit(e) {
            e.preventDefault();
            const item = {
                id: Date.now(),
                name: document.getElementById('asset-name').value,
                cost: parseFloat(document.getElementById('asset-cost').value),
                salvage: parseFloat(document.getElementById('asset-salvage').value),
                years: parseInt(document.getElementById('asset-years').value)
            };
            appState.assets.unshift(item);
            saveStateToStorage();
            renderAdvancedOperations();
            e.target.reset();
            showToast('تم احتساب إهلاك الأصل', 'success');
        }

        function handleDebtSubmit(e) {
            e.preventDefault();
            const item = {
                id: Date.now(),
                name: document.getElementById('debt-name').value,
                type: document.getElementById('debt-type').value,
                amount: parseFloat(document.getElementById('debt-amount').value),
                date: document.getElementById('debt-date').value
            };
            appState.debts.unshift(item);
            saveStateToStorage();
            renderAdvancedOperations();
            e.target.reset();
            showToast('تم تسجيل الذمة المالية', 'success');
        }

        function renderAdvancedOperations() {
            // Render Checks
            const checksTbody = document.getElementById('checks-tbody');
            checksTbody.innerHTML = appState.checks.map(c => `
                <tr>
                    <td class="p-2 font-mono font-bold text-amber-400">${escapeHtml(c.num)}</td>
                    <td class="p-2">${escapeHtml(c.bank)}</td>
                    <td class="p-2 text-white font-semibold">${escapeHtml(c.client)}</td>
                    <td class="p-2 font-mono text-rose-400 font-bold">${c.amount.toFixed(2)} ${appState.currencySymbol}</td>
                    <td class="p-2 text-slate-400 font-mono">${c.date}</td>
                    <td class="p-2 text-center">
                        <button onclick="deleteAdvItem('checks', ${c.id})" class="text-slate-500 hover:text-rose-400"><i class="fa-solid fa-trash"></i></button>
                    </td>
                </tr>
            `).join('') || '<tr><td colspan="6" class="text-center py-4 text-slate-500">لا توجد شيكات مرتجعة</td></tr>';

            // Render Damaged
            const dmgTbody = document.getElementById('damaged-tbody');
            dmgTbody.innerHTML = appState.damaged.map(d => `
                <tr>
                    <td class="p-2 font-semibold text-white">${escapeHtml(d.item)}</td>
                    <td class="p-2 font-mono">${d.qty}</td>
                    <td class="p-2 font-mono">${d.cost.toFixed(2)}</td>
                    <td class="p-2 font-mono text-rose-400 font-bold">${(d.qty * d.cost).toFixed(2)} ${appState.currencySymbol}</td>
                    <td class="p-2 text-slate-400">${escapeHtml(d.reason)}</td>
                    <td class="p-2 text-center">
                        <button onclick="deleteAdvItem('damaged', ${d.id})" class="text-slate-500 hover:text-rose-400"><i class="fa-solid fa-trash"></i></button>
                    </td>
                </tr>
            `).join('') || '<tr><td colspan="6" class="text-center py-4 text-slate-500">لا توجد سائل هالك مسجلة</td></tr>';

            // Render Assets
            const assetTbody = document.getElementById('assets-tbody');
            assetTbody.innerHTML = appState.assets.map(a => {
                const dep = (a.cost - a.salvage) / a.years;
                return `
                <tr>
                    <td class="p-2 font-semibold text-white">${escapeHtml(a.name)}</td>
                    <td class="p-2 font-mono">${a.cost.toFixed(2)}</td>
                    <td class="p-2 font-mono">${a.salvage.toFixed(2)}</td>
                    <td class="p-2 font-mono">${a.years} سنوات</td>
                    <td class="p-2 font-mono text-indigo-400 font-bold">${dep.toFixed(2)} ${appState.currencySymbol} / سنة</td>
                    <td class="p-2 text-center">
                        <button onclick="deleteAdvItem('assets', ${a.id})" class="text-slate-500 hover:text-rose-400"><i class="fa-solid fa-trash"></i></button>
                    </td>
                </tr>
            `}).join('') || '<tr><td colspan="6" class="text-center py-4 text-slate-500">لا توجد أصول مسجلة</td></tr>';

            // Render Debts
            const debtsTbody = document.getElementById('debts-tbody');
            debtsTbody.innerHTML = appState.debts.map(d => `
                <tr>
                    <td class="p-2 font-semibold text-white">${escapeHtml(d.name)}</td>
                    <td class="p-2">
                        <span class="px-2 py-0.5 rounded-full text-[10px] ${d.type === 'debtor' ? 'bg-emerald-500/10 text-emerald-400' : 'bg-rose-500/10 text-rose-400'}">
                            ${d.type === 'debtor' ? 'لنا عليه (مدينة)' : 'علينا له (دائنة)'}
                        </span>
                    </td>
                    <td class="p-2 font-mono font-bold ${d.type === 'debtor' ? 'text-emerald-400' : 'text-rose-400'}">${d.amount.toFixed(2)} ${appState.currencySymbol}</td>
                    <td class="p-2 text-slate-400 font-mono">${d.date}</td>
                    <td class="p-2 text-center">
                        <button onclick="deleteAdvItem('debts', ${d.id})" class="text-slate-500 hover:text-rose-400"><i class="fa-solid fa-trash"></i></button>
                    </td>
                </tr>
            `).join('') || '<tr><td colspan="5" class="text-center py-4 text-slate-500">لا توجد ذمم دائنة أو مدينة</td></tr>';
        }

        function deleteAdvItem(key, id) {
            appState[key] = appState[key].filter(x => x.id !== id);
            saveStateToStorage();
            renderAdvancedOperations();
            showToast('تم الحذف', 'info');
        }

        /* ==========================================================================
           REPORT GENERATION & PRINT
           ========================================================================== */
        function renderReports() {
            document.getElementById('report-activity').innerText = activityNames[appState.activity] || 'تجارة وتجزئة';
            document.getElementById('report-date').innerText = new Date().toLocaleDateString('ar-EG');

            let income = 0;
            let expense = 0;

            appState.transactions.forEach(t => {
                if (t.type === 'income') income += t.amount;
                if (t.type === 'expense') expense += t.amount;
            });

            const net = income - expense;

            document.getElementById('report-income').innerText = `${income.toFixed(2)} ${appState.currencySymbol}`;
            document.getElementById('report-expense').innerText = `${expense.toFixed(2)} ${appState.currencySymbol}`;
            document.getElementById('report-net').innerText = `${net.toFixed(2)} ${appState.currencySymbol}`;

            const tbody = document.getElementById('report-table-body');
            tbody.innerHTML = appState.transactions.map(t => `
                <tr class="border-b border-slate-800">
                    <td class="p-2 text-slate-400 font-mono">${t.date}</td>
                    <td class="p-2">${t.type === 'income' ? 'إيراد' : 'مصروف'}</td>
                    <td class="p-2 font-bold">${escapeHtml(t.category)}</td>
                    <td class="p-2 font-mono font-bold ${t.type === 'income' ? 'text-emerald-400' : 'text-rose-400'}">${t.amount.toFixed(2)} ${appState.currencySymbol}</td>
                    <td class="p-2 text-slate-400">${escapeHtml(t.notes || '-')}</td>
                </tr>
            `).join('') || '<tr><td colspan="5" class="text-center py-4 text-slate-500">لا توجد بيانات للعرض</td></tr>';
        }

        /* ==========================================================================
           INTERACTIVE AI ASSISTANT LOGIC
           ========================================================================== */
        const aiKnowledgeBase = [
            {
                keywords: ['شيك', 'شيكات', 'مرتجع', 'راجع', 'مرتجة'],
                response: 'لتسجيل شيك مرتجع في برنامج "محاسبي": اذهب إلى تبويب (العمليات المحاسبية المتقدمة) ➔ واختر قسم (الشيكات الراجعة)، قم بتعبئة رقم الشيك، اسم البنك، اسم الساحب والمبلغ. محاسبياً: يُقيد الشيك المرتجع بإعادة إثبات الذمة على العميل والخصم من البنك.'
            },
            {
                keywords: ['إهلاك', 'الاهلاك', 'أصل', 'اصول', 'ثابتة'],
                response: 'طريقة حساب إهلاك الأصول الثابتة عبر طريقة القسط الثابت هي: (تكلفة الشراء الأصلية - القيمة الخردة) ÷ العمر الإنتاجي بالسنوات. يمكنك استخدام الحاسبة التلقائية المدمجة في تبويب (العمليات المحاسبية المتقدمة ➔ إهلاك الأصول الثابتة).'
            },
            {
                keywords: ['تالف', 'تالفة', 'بضاعة', 'هالك', 'تلف'],
                response: 'لإثبات البضاعة التالفة: اذهب إلى (العمليات المحاسبية المتقدمة) ➔ (البضاعة التالفة/الهالك). ادخل اسم الصنف، الكمية، وتكلفة الوحدة. سيتم قيد إجمالي المبلغ كمصروف هالك خاسر ينقص من صافي الربح.'
            },
            {
                keywords: ['عقارات', 'مخازن', 'مستأجر', 'إيجار'],
                response: 'قسم (عقارات ومخازن) يتيح لك تسجيل الوحدات المؤجرة، متابعة المستأجرين، حساب متوسط الإشغال وتتبع تحصيل الإيجارات الدورية بسهولة عند اختيار هذا النشاط من القائمة العلوية.'
            },
            {
                keywords: ['تصدير', 'استرجاع', 'نسخ', 'بيانات', 'حفظ'],
                response: 'يمكنك تصدير بياناتك أو مسحها بالكامل من تبويب (الإعدادات والأمان) في أسفل الصفحة. البيانات تُحفظ محلياً وبشكل تلقائي وآمن داخل متصفحك عبر LocalStorage.'
            }
        ];

        function askAI(questionText) {
            document.getElementById('ai-input').value = questionText;
            handleAISubmit(new Event('submit'));
        }

        function handleAISubmit(e) {
            e.preventDefault();
            const inputEl = document.getElementById('ai-input');
            const query = inputEl.value.trim();
            if (!query) return;

            const chatHistory = document.getElementById('ai-chat-history');

            // Render User Question
            chatHistory.innerHTML += `
                <div class="flex gap-3 items-start justify-end">
                    <div class="bg-indigo-600 p-3 rounded-2xl rounded-tl-none text-xs text-white max-w-[80%]">
                        ${escapeHtml(query)}
                    </div>
                </div>
            `;

            // Find matching response
            let reply = "شكراً لاستفسارك! أداة محاسبي تم تصميمها وتطويرها بواسطة المحاسب **براء نادر توفيق ناصر** لتبسيط كافة القيود المحاسبية. يمكنك تنقل التبويبات المخصصة لإنجاز معاملتك بدقة.";
            
            const lowerQuery = query.toLowerCase();
            for (let kb of aiKnowledgeBase) {
                if (kb.keywords.some(k => lowerQuery.includes(k))) {
                    reply = kb.response;
                    break;
                }
            }

            // Render AI Response
            setTimeout(() => {
                chatHistory.innerHTML += `
                    <div class="flex gap-3 items-start">
                        <div class="w-8 h-8 rounded-full bg-indigo-600 flex items-center justify-center text-white shrink-0 text-xs">
                            <i class="fa-solid fa-robot"></i>
                        </div>
                        <div class="bg-slate-800 p-3.5 rounded-2xl rounded-tr-none text-xs text-slate-200 max-w-[85%] leading-relaxed border border-indigo-500/20">
                            ${reply}
                        </div>
                    </div>
                `;
                chatHistory.scrollTop = chatHistory.scrollHeight;
            }, 300);

            inputEl.value = '';
            chatHistory.scrollTop = chatHistory.scrollHeight;
        }

        /* ==========================================================================
           SETTINGS, DEMO DATA & SECURITY
           ========================================================================== */
        function changeCurrency(curr) {
            appState.currency = curr;
            const symbols = { USD: '$', ILS: '₪', JOD: 'JD', EUR: '€' };
            appState.currencySymbol = symbols[curr] || '$';
            saveStateToStorage();
            renderDashboard();
            showToast(`تم تغيير العملة إلى: ${curr}`, 'success');
        }

        function savePinSetting() {
            const val = document.getElementById('setting-pin-input').value.trim();
            if (val.length < 4) {
                showToast('رمز PIN يجب أن يتكون من 4 إلى 6 أرقام', 'error');
                return;
            }
            appState.pin = val;
            saveStateToStorage();
            document.getElementById('setting-pin-input').value = '';
            showToast('تم تعيين رمز الأمان PIN بنجاح 🔒', 'success');
        }

        function toggleLock() {
            if (!appState.pin) {
                showToast('يرجى تعيين رمز PIN من الإعدادات أولاً لقفل النظام', 'warning');
                switchTab('settings');
                return;
            }
            document.getElementById('pin-modal').classList.remove('hidden');
        }

        function checkPin() {
            const input = document.getElementById('pin-input').value;
            if (input === appState.pin) {
                document.getElementById('pin-modal').classList.add('hidden');
                document.getElementById('pin-input').value = '';
                showToast('تم فتح النظام بنجاح', 'success');
            } else {
                showToast('رمز PIN غير صحيح!', 'error');
            }
        }

        function loadDemoData() {
            appState.transactions = [
                { id: 101, type: 'income', amount: 3500.00, category: 'مبيعات بضائع تجزئة', date: '2026-09-25', notes: 'دفعة نقدية صندوق', activity: 'retail' },
                { id: 102, type: 'expense', amount: 1200.00, category: 'إيجار المحل التجاري', date: '2026-09-26', notes: 'عن شهر سبتمبر', activity: 'retail' },
                { id: 103, type: 'income', amount: 1800.00, category: 'خدمات واستشارات متخصصة', date: '2026-09-28', notes: 'تحصيل بموجب فاتورة', activity: 'services' },
                { id: 104, type: 'expense', amount: 450.00, category: 'فاتورة كهرباء ومرافق', date: '2026-09-29', notes: 'تشغيل المعرض', activity: 'retail' }
            ];
            
            appState.checks = [
                { id: 201, num: 'CHK-9940', bank: 'البنك العربي', client: 'شركة الأمل للتجارة', amount: 850.00, date: '2026-09-20' }
            ];

            appState.damaged = [
                { id: 301, item: 'كرتونة مواد غذائية', qty: 5, cost: 20.00, reason: 'سوء تخزين ورطوبة' }
            ];

            appState.assets = [
                { id: 401, name: 'سيارة توزيع البضائع', cost: 15000.00, salvage: 3000.00, years: 5 }
            ];

            appState.debts = [
                { id: 501, name: 'مؤسسة النور', type: 'debtor', amount: 1400.00, date: '2026-09-15' }
            ];

            saveStateToStorage();
            renderDashboard();
            renderAdvancedOperations();
            showToast('تم تحميل البيانات التجريبية بنجاح 🚀', 'success');
        }

        function exportDataJSON() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(appState, null, 2));
            const downloadAnchor = document.createElement('a');
            downloadAnchor.setAttribute("href", dataStr);
            downloadAnchor.setAttribute("download", `muhasibi_backup_${new Date().toISOString().split('T')[0]}.json`);
            document.body.appendChild(downloadAnchor);
            downloadAnchor.click();
            downloadAnchor.remove();
            showToast('تم تصدير نسخة البيانات الاحتياطية', 'success');
        }

        function clearAllData() {
            if (confirm('هل أنت تأكد من مسح جميع البيانات بشكل نهائي؟')) {
                localStorage.removeItem(STORAGE_KEY);
                location.reload();
            }
        }

        /* Helper Utilities */
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            
            const bgClasses = {
                success: 'bg-emerald-600 text-white',
                error: 'bg-rose-600 text-white',
                warning: 'bg-amber-600 text-white',
                info: 'bg-slate-800 text-slate-100 border border-slate-700'
            };

            toast.className = `px-4 py-2.5 rounded-xl text-xs font-bold shadow-xl pointer-events-auto flex items-center gap-2 ${bgClasses[type] || bgClasses.info} transition-all duration-300`;
            toast.innerHTML = `<span>${message}</span>`;

            container.appendChild(toast);

            setTimeout(() => {
                toast.style.opacity = '0';
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        function escapeHtml(str) {
            if (!str) return '';
            return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;").replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
