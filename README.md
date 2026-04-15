<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Life Log</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400;700&family=Klee+One:wght@400;600&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/fullcalendar@6.1.10/index.global.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        morandi: {
                            'dark-gray': '#3A4750',
                            'blue-light': '#E2EBF0',
                            'blue-dark': '#8DAAB9',
                            'green-light': '#E2E8E8',
                            'green-dark': '#7F9F9F',
                            'pink': '#EBC4B9',
                            'soft-red': '#E8D1D1',
                            'soft-orange': '#EEDFD2',
                            'soft-yellow': '#EBE5D1',
                            'soft-green': '#D5E0D5',
                            'soft-gray': '#E5E5E5'
                        }
                    }
                }
            }
        }
    </script>
    
    <style>
        :root { --font-en: 'Dancing Script', cursive; --font-zh: 'Klee One', sans-serif; }
        body { font-family: var(--font-zh); overflow: hidden; background-color: #F8F8F8; margin: 0; }
        .font-cursive { font-family: var(--font-en); }

        .fc { font-family: var(--font-zh) !important; height: 100% !important; border: none !important; }
        .fc-theme-standard td, .fc-theme-standard th { border: 1px solid #EEEEEE !important; }
        .fc-scrollgrid { border: none !important; }
        .fc .fc-view-harness { border-radius: 20px; overflow: hidden; background-color: #fff; }
        .fc-header-toolbar { margin-bottom: 0.25rem !important; padding: 0 5px !important; }
        .fc .fc-toolbar-title { font-family: var(--font-en); font-size: 1.8rem !important; font-weight: 700; color: #3A4750; }
        .fc .fc-button { padding: 2px 6px !important; font-size: 0.65rem !important; border-radius: 8px !important; background-color: #C6D8E2 !important; border: none !important; color: #3A4750 !important; }
        .fc .fc-yearDisplay-button { background-color: transparent !important; border: none !important; color: #3A4750 !important; font-size: 1rem !important; font-family: var(--font-en); font-weight: 600 !important; padding: 0 8px !important; cursor: default !important; }

        /* 修改：將日期與標籤的佈局調整為兩端對齊 */
.fc .fc-daygrid-day-top {
    display: flex !important;
    flex-direction: row-reverse !important; /* 關鍵：讓內容反向排列，確保日期在右 */
    justify-content: space-between !important; /* 讓兩者分開至左右兩端 */
    align-items: flex-start !important;
    padding: 2px 4px !important;
    min-height: 30px;
    width: 100%;
}

/* 確保標籤（國定假日、節氣）靠左 */
.special-label { 
    font-size: 10px; 
    padding: 1px 5px; 
    border-radius: 4px; 
    line-height: 1.4; 
    white-space: nowrap; 
    flex-shrink: 0; 
    margin-top: 2px;
    margin-right: auto; /* 強制靠左推 */
}

/* 確保日期數字靠右 */
.fc .fc-daygrid-day-number { 
    padding: 0 2px !important; 
    text-decoration: none !important; 
    color: #555; 
    font-size: 0.8rem; 
    float: none !important; 
    flex-shrink: 0; 
    margin-top: 1px;
    text-align: right; /* 文字對齊向右 */
}
        .special-label { font-size: 10px; padding: 1px 5px; border-radius: 4px; line-height: 1.4; white-space: nowrap; flex-shrink: 0; margin-top: 2px; }
        .label-holiday { background-color: #E8D1D1; color: #A64444; font-weight: 600; }
        .label-solar { color: #2D3748; background: transparent !important; font-weight: 600; }
        .fc .fc-daygrid-day-number { padding: 0 2px !important; text-decoration: none !important; color: #555; font-size: 0.8rem; float: none !important; flex-shrink: 0; margin-top: 1px; }
        .fc-day-sat .fc-daygrid-day-number, .fc-day-sun .fc-daygrid-day-number { color: #D19A9A !important; font-weight: 600; }
        .fc-day-sat, .fc-day-sun { background-color: rgba(232,209,209,0.08) !important; }
        th.fc-day-sat a, th.fc-day-sun a { color: #D19A9A !important; }
        .fc .fc-day-today { background-color: #F2F2F2 !important; border-radius: 15px !important; }
        .fc-event { border: none !important; background: transparent !important; }

        .modal-fixed-card { background-color: #FFFFFF !important; border-radius: 40px !important; width: 100%; max-width: 460px; height: 680px !important; box-shadow: 0 25px 50px -12px rgba(0,0,0,0.25); position: relative; overflow: hidden; display: flex; flex-direction: column; }
        .tab-inner-row { display: flex; padding-left: 32px; margin-top: 20px; height: 32px; align-items: flex-end; flex-shrink: 0; }
        .sticker-tab { transition: all 0.2s; font-size: 1.1rem; border: none; outline: none; border-top-left-radius: 12px !important; border-top-right-radius: 12px !important; min-width: 75px; height: 30px; display: flex; align-items: center; justify-content: center; cursor: pointer; margin-right: -12px; }
        #tab-schedule { background-color: #E2EBF0; color: #A0B0B9; z-index: 5; }
        #tab-schedule.active { background-color: #8DAAB9; color: white; z-index: 10; }
        #tab-reading { background-color: #E2E8E8; color: #A0B0B0; z-index: 4; }
        #tab-reading.active { background-color: #7F9F9F; color: white; z-index: 10; }

        .category-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 6px; margin-top: 8px; width: 100%; }
        .cat-btn { transition: all 0.2s; border: none; font-family: var(--font-en); opacity: 0.8; text-align: center; height: 30px; border-radius: 12px !important; font-size: 15px !important; cursor: pointer; display: flex; align-items: center; justify-content: center; width: 100%; }
        .cat-btn.selected { opacity: 1; box-shadow: 0 2px 6px rgba(0,0,0,0.15); font-weight: 700; transform: scale(1.05); color: #3A4750; }

        .field-group { display: flex; flex-direction: column; width: 100%; }
        .custom-input { background-color: #F9FAFB; border: none; border-radius: 12px; padding: 10px 12px; outline: none; font-size: 0.85rem; height: 42px; width: 100%; margin-top: 4px; font-family: var(--font-zh); box-sizing: border-box; }
        .custom-input:focus { box-shadow: 0 0 0 1px #E5E7EB; }
        textarea.custom-input { resize: none; min-height: 70px; height: auto !important; }

        .fab-container { position: fixed; bottom: 24px; right: 24px; z-index: 9999; }
        .fab-main { background-color: #EBC4B9; color: #3A4750; width: 56px; height: 56px; border-radius: 50%; display: flex; align-items: center; justify-content: center; box-shadow: 0 4px 12px rgba(0,0,0,0.15); cursor: pointer; border: none; transition: transform 0.2s; }

        /* Reading module */
        .read-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px; }
        .add-book-btn { background: none; border: none; cursor: pointer; width: 26px; height: 26px; border-radius: 50%; background: #E2E8E8; display: flex; align-items: center; justify-content: center; color: #7F9F9F; transition: background 0.2s; flex-shrink: 0; }
        .add-book-btn:hover { background: #C6D8D8; }

        .book-row { background: #F9FAFB; border-radius: 16px; padding: 12px 14px; margin-bottom: 8px; }
        .book-title-line { display: flex; align-items: center; gap: 8px; }
        .book-title-input { background: transparent; border: none; outline: none; font-size: 0.88rem; font-family: var(--font-zh); flex: 1; color: #3A4750; font-weight: 600; min-width: 0; }
        .book-title-input::placeholder { color: #CCCCCC; font-weight: 400; }
        .book-done-cb { width: 16px; height: 16px; accent-color: #7F9F9F; cursor: pointer; flex-shrink: 0; }

        .book-start-row { display: flex; align-items: center; gap: 6px; margin-top: 6px; }
        .start-label { font-size: 10px; color: #BBBBBB; white-space: nowrap; }
        .start-date-input { background: transparent; border: none; border-bottom: 1px dashed #DDDDDD; font-size: 0.75rem; color: #AAAAAA; outline: none; font-family: var(--font-zh); padding: 1px 4px; flex: 1; }

        .page-row { display: flex; align-items: center; gap: 5px; margin-top: 8px; }
        .page-input { background: #EEEEEE; border: none; border-radius: 8px; padding: 4px 8px; width: 62px; font-size: 0.82rem; text-align: center; outline: none; font-family: var(--font-zh); color: #3A4750; height: 28px; box-sizing: border-box; }
        .page-sep { color: #CCCCCC; font-size: 0.9rem; }
        .page-total-input { background: transparent; border: none; border-bottom: 1px dashed #DDDDDD; padding: 2px 4px; width: 58px; font-size: 0.82rem; text-align: center; outline: none; font-family: var(--font-zh); color: #BBBBBB; height: 28px; box-sizing: border-box; }
        .pages-label { font-size: 10px; color: #AAAAAA; }

        .progress-bar-wrap { margin-top: 8px; height: 4px; background: #EBEBEB; border-radius: 4px; overflow: hidden; }
        .progress-bar-fill { height: 100%; background: #7F9F9F; border-radius: 4px; transition: width 0.4s; }

        .book-meta { display: flex; gap: 6px; margin-top: 6px; flex-wrap: wrap; }
        .meta-chip { font-size: 10px; color: #888; background: #EEEEEE; border-radius: 20px; padding: 2px 8px; white-space: nowrap; }
        .meta-chip.pct { background: #D5E0D5; color: #3A6B3A; }

        .ai-section { margin-top: 10px; padding-top: 10px; border-top: 1px solid #F0F0F0; }
        .ai-btn { display: flex; align-items: center; justify-content: center; gap: 6px; background: #EDF3ED; border: none; border-radius: 20px; padding: 7px 16px; cursor: pointer; font-size: 0.75rem; font-family: var(--font-zh); color: #3A4750; width: 100%; transition: opacity 0.2s; }
        .ai-btn:hover { opacity: 0.8; }
        .ai-result { margin-top: 8px; background: #F5F8F5; border-radius: 12px; padding: 10px 12px; font-size: 0.73rem; color: #555; line-height: 1.65; display: none; max-height: 110px; overflow-y: auto; }
        .ai-result.show { display: block; }
        .ai-spin { display: none; width: 13px; height: 13px; border: 2px solid #7F9F9F; border-top-color: transparent; border-radius: 50%; animation: spin 0.7s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }
    </style>
</head>
<body class="bg-morandi-white text-gray-700 antialiased">

<div class="flex h-screen overflow-hidden">
    <nav class="w-52 bg-morandi-dark-gray text-morandi-gray p-5 flex flex-col rounded-r-[30px] shadow-2xl shrink-0 z-10">
        <button onclick="location.reload()" class="mb-8 text-left">
            <div class="flex items-center gap-2">
                <h1 class="text-lg font-cursive text-morandi-green">Life Log</h1>
                <i class="fa-solid fa-seedling text-morandi-green text-[12px]"></i>
            </div>
        </button>
        <ul class="space-y-2 flex-grow">
            <li><button onclick="showSection('calendar')" class="flex items-center gap-3 hover:text-white w-full text-left p-2 rounded-xl font-cursive text-base text-morandi-blue"><i class="fa-solid fa-pen-nib text-xs"></i> Journal</button></li>
            <li><button onclick="showSection('tigps')" class="flex items-center gap-3 hover:text-white w-full text-left p-2 rounded-xl font-cursive text-base"><i class="fa-solid fa-layer-group text-xs"></i> Projects</button></li>
            <li><button onclick="showSection('phd')" class="flex items-center gap-3 hover:text-white w-full text-left p-2 rounded-xl font-cursive text-base"><i class="fa-solid fa-graduation-cap text-xs"></i> Thesis</button></li>
            <li><button onclick="showSection('finance')" class="flex items-center gap-3 hover:text-white w-full text-left p-2 rounded-xl font-cursive text-base"><i class="fa-solid fa-calculator text-xs"></i> Accounting</button></li>
        </ul>
    </nav>

    <main class="flex-1 overflow-hidden relative">
        <div class="p-4 h-full flex flex-col">
            <div class="mb-3 ml-2">
                <h2 id="daily-quote-en" class="text-xl font-cursive font-bold text-morandi-dark-gray leading-tight"></h2>
                <div class="flex items-baseline gap-2">
                    <p id="daily-quote-zh" class="text-[10px] text-gray-400 font-medium"></p>
                    <span id="quote-author" class="text-[10px] font-cursive text-morandi-dark-gray italic font-bold"></span>
                </div>
            </div>
            <section id="calendar-section" class="flex-1 bg-morandi-gray/5 p-4 rounded-[20px] border border-white/30 shadow-sm overflow-hidden">
                <div id="calendar"></div>
            </section>
        </div>
    </main>
</div>

<div class="fab-container">
    <button onclick="openUnifiedModal()" class="fab-main"><i class="fa-solid fa-plus text-xl"></i></button>
</div>

<div id="unifiedModal" class="hidden fixed inset-0 bg-black/50 backdrop-blur-md flex justify-center items-center z-[10000] p-4">
    <div class="modal-fixed-card">
        <div class="tab-inner-row">
            <button onclick="switchType('schedule')" id="tab-schedule" class="sticker-tab active"><i class="fa-solid fa-calendar-days"></i></button>
            <button onclick="switchType('reading')" id="tab-reading" class="sticker-tab"><i class="fa-solid fa-book"></i></button>
        </div>

        <div class="flex-1 p-8 pt-4 flex flex-col justify-between overflow-y-auto">
            <button onclick="closeUnifiedModal()" class="absolute top-6 right-8 text-gray-300 hover:text-gray-500 transition-colors">
                <i class="fa-solid fa-times text-xs"></i>
            </button>

            <!-- SCHEDULE -->
            <div id="schedule-fields-static" class="space-y-4 text-left">
                <div class="field-group">
                    <label class="text-lg font-bold text-morandi-dark-gray font-cursive">Schedule</label>
                    <input type="text" id="inputMain" placeholder="What's your plan?" class="custom-input">
                </div>
                <div class="field-group">
                    <label id="dateLabel" class="text-lg font-bold text-morandi-dark-gray font-cursive">Date & Time</label>
                    <div class="flex items-center gap-2">
                        <input type="date" id="inputDate" class="custom-input flex-[2]">
                        <div class="flex flex-1 items-center gap-1 bg-gray-50 rounded-xl px-2 h-[42px] mt-1">
                            <select id="selectHour" class="bg-transparent text-xs flex-1 outline-none"></select>
                            <span class="text-gray-300">:</span>
                            <select id="selectMinute" class="bg-transparent text-xs flex-1 outline-none"></select>
                        </div>
                    </div>
                </div>
                <div id="conditional-fields">
                    <div id="travel-end-container" class="field-group" style="display:none;">
                        <label class="text-lg font-bold text-morandi-dark-gray font-cursive">End Date & Time</label>
                        <div class="flex items-center gap-2">
                            <input type="date" id="inputEndDate" class="custom-input flex-[2]">
                            <div class="flex flex-1 items-center gap-1 bg-gray-50 rounded-xl px-2 h-[42px] mt-1">
                                <select id="selectEndHour" class="bg-transparent text-xs flex-1 outline-none"></select>
                                <span class="text-gray-300">:</span>
                                <select id="selectEndMinute" class="bg-transparent text-xs flex-1 outline-none"></select>
                            </div>
                        </div>
                    </div>
                    <div id="work-hours-container" class="field-group" style="display:none;">
                        <label class="text-lg font-bold text-morandi-dark-gray font-cursive">Work Hours</label>
                        <input type="number" step="0.1" id="workHours" placeholder="Hours" class="custom-input">
                    </div>
                </div>
                <div class="field-group">
                    <label class="text-lg font-bold text-morandi-dark-gray font-cursive">Category</label>
                    <div class="category-grid">
                        <button onclick="setCategory('Work')" id="cat-Work" class="cat-btn" style="background-color:#E8D1D1;">Work</button>
                        <button onclick="setCategory('Exercise')" id="cat-Exercise" class="cat-btn" style="background-color:#D5E0D5;">Exercise</button>
                        <button onclick="setCategory('Travel')" id="cat-Travel" class="cat-btn" style="background-color:#EBE5D1;">Travel</button>
                        <button onclick="setCategory('Tutor')" id="cat-Tutor" class="cat-btn" style="background-color:#EEDFD2;">Tutor</button>
                        <button onclick="setCategory('Private')" id="cat-Private" class="cat-btn" style="background-color:#E5E5E5;">Private</button>
                    </div>
                </div>
                <div class="field-group">
                    <label class="text-lg font-bold text-morandi-dark-gray font-cursive">Notes</label>
                    <textarea id="inputNotes" placeholder="Notes..." class="custom-input"></textarea>
                </div>
            </div>

            <!-- READING -->
            <div id="read-fields" style="display:none;" class="text-left">
                <div class="read-header">
                    <label class="text-lg font-bold text-morandi-dark-gray font-cursive">Reading</label>
                    <button onclick="addBookRow()" class="add-book-btn" title="新增書籍">
                        <i class="fa-solid fa-plus" style="font-size:10px;"></i>
                    </button>
                </div>
                <div id="books-container"></div>
                <div class="ai-section">
                    <button class="ai-btn" onclick="runAIAnalysis()">
                        <div id="ai-spin" class="ai-spin"></div>
                        <i id="ai-icon" class="fa-solid fa-wand-magic-sparkles" style="font-size:11px;color:#7F9F9F;"></i>
                        <span>AI 閱讀分析與建議</span>
                    </button>
                    <div id="ai-result" class="ai-result"></div>
                </div>
            </div>

            <div class="pt-4">
                <button onclick="handleSave()" id="saveBtn" class="w-1/2 mx-auto block py-2 text-white rounded-full font-cursive text-sm shadow-sm active:scale-95 transition-all" style="background-color:#8DAAB9;">Keep it</button>
            </div>
        </div>
    </div>
</div>

<script>
const quotes = [
    { en: "The only way to do great work is to love what you do.", zh: "成就偉業的唯一途徑是熱愛自己的工作。", author: "Steve Jobs" },
    { en: "Quality is not an act, it is a habit.", zh: "優秀不是一種行為，而是一種習慣。", author: "Aristotle" }
];

const TAIWAN_SPECIAL_DAYS = [
    { date: '2026-01-01', title: '元旦', type: 'holiday' }, { date: '2026-02-16', title: '除夕', type: 'holiday' },
    { date: '2026-02-17', title: '初一', type: 'holiday' }, { date: '2026-02-18', title: '初二', type: 'holiday' },
    { date: '2026-02-19', title: '初三', type: 'holiday' }, { date: '2026-02-28', title: '和平紀念日', type: 'holiday' },
    { date: '2026-04-04', title: '兒童節', type: 'holiday' }, { date: '2026-04-05', title: '清明節', type: 'holiday' },
    { date: '2026-05-01', title: '勞動節', type: 'holiday' }, { date: '2026-06-19', title: '端午節', type: 'holiday' },
    { date: '2026-09-25', title: '中秋節', type: 'holiday' }, { date: '2026-10-10', title: '國慶日', type: 'holiday' },
    { date: '2026-02-04', title: '立春', type: 'solar' }, { date: '2026-03-20', title: '春分', type: 'solar' },
    { date: '2026-05-05', title: '立夏', type: 'solar' }, { date: '2026-06-21', title: '夏至', type: 'solar' },
    { date: '2026-08-07', title: '立秋', type: 'solar' }, { date: '2026-09-23', title: '秋分', type: 'solar' },
    { date: '2026-11-07', title: '立冬', type: 'solar' }, { date: '2026-12-22', title: '冬至', type: 'solar' }
];

let currentType = 'schedule', currentCategory = 'Private', calendar;
const STORAGE_KEY = 'ashley_events_v8_reorder';
const BOOKS_KEY = 'ashley_books_v1';

document.addEventListener('DOMContentLoaded', () => { updateQuote(); initTimeDropdowns(); initCalendar(); });

function initTimeDropdowns() {
    const h = document.getElementById('selectHour'), m = document.getElementById('selectMinute');
    const eh = document.getElementById('selectEndHour'), em = document.getElementById('selectEndMinute');
    for (let i = 0; i < 24; i++) {
        const o = `<option value="${String(i).padStart(2,'0')}">${String(i).padStart(2,'0')}</option>`;
        h.innerHTML += o; eh.innerHTML += o;
    }
    for (let i = 0; i < 60; i += 5) {
        const o = `<option value="${String(i).padStart(2,'0')}">${String(i).padStart(2,'0')}</option>`;
        m.innerHTML += o; em.innerHTML += o;
    }
}

function updateQuote() {
    const q = quotes[Math.floor(Math.random() * quotes.length)];
    document.getElementById('daily-quote-en').innerText = q.en;
    document.getElementById('daily-quote-zh').innerText = q.zh;
    document.getElementById('quote-author').innerText = `— ${q.author}`;
}

function initCalendar() {
    const userEvents = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
    calendar = new FullCalendar.Calendar(document.getElementById('calendar'), {
        initialView: 'dayGridMonth', locale: 'en', height: '100%',
        headerToolbar: { left: 'today yearDisplay', center: 'title', right: 'prev,next' },
        customButtons: { yearDisplay: { text: new Date().getFullYear().toString() } },
        titleFormat: { month: 'long' },
        dayCellContent: (arg) => {
            const dateStr = arg.date.toLocaleDateString('en-CA');
            const special = TAIWAN_SPECIAL_DAYS.find(d => d.date === dateStr);
            const labelHtml = special
                ? `<span class="special-label ${special.type === 'holiday' ? 'label-holiday' : 'label-solar'}">${special.title}</span>`
                : `<span></span>`;
            const dayNumber = arg.dayNumberText.replace('日', '');
            return { html: `${labelHtml}<a class="fc-daygrid-day-number">${dayNumber}</a>` };
        },
        events: userEvents,
        eventContent: (arg) => {
            let div = document.createElement('div');
            div.style.cssText = 'font-size:7.5px;padding:1px 3px;margin-bottom:1px;border-radius:4px;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;color:#444;box-shadow:2px 2px 5px rgba(0,0,0,0.08);';
            const cc = arg.event.extendedProps.colorClass || '';
            if (cc.includes('soft-red')) div.style.backgroundColor = '#E8D1D1';
            else if (cc.includes('soft-green')) div.style.backgroundColor = '#D5E0D5';
            else if (cc.includes('soft-yellow')) div.style.backgroundColor = '#EBE5D1';
            else if (cc.includes('soft-orange')) div.style.backgroundColor = '#EEDFD2';
            else div.style.backgroundColor = '#E5E5E5';
            div.innerText = arg.event.title;
            return { domNodes: [div] };
        }
    });
    calendar.render();
}

// ── BOOKS ──────────────────────────────────────────────────

function loadBooks() { return JSON.parse(localStorage.getItem(BOOKS_KEY) || '[]'); }
function saveBooks(b) { localStorage.setItem(BOOKS_KEY, JSON.stringify(b)); }

function calcMeta(book) {
    const pct = book.totalPages > 0 ? Math.min(100, Math.round((book.currentPage / book.totalPages) * 100)) : 0;
    let daysRead = 0, eta = null;
    if (book.startDate) {
        const start = new Date(book.startDate);
        const today = new Date();
        daysRead = Math.max(1, Math.round((today - start) / 86400000));
        const ppd = book.currentPage / daysRead;
        const rem = (book.totalPages || 0) - (book.currentPage || 0);
        if (ppd > 0 && rem > 0) eta = Math.ceil(rem / ppd);
    }
    return { pct, daysRead, eta };
}

function renderBooks() {
    const books = loadBooks();
    const container = document.getElementById('books-container');
    container.innerHTML = '';

    const active = books.filter(b => !b.done);
    const list = active.length > 0 ? active : [];

    // Always show at least one empty row if no books
    if (list.length === 0) {
        const tempId = 'new_' + Date.now();
        renderBookRow(container, { id: tempId, title: '', currentPage: 0, totalPages: 0, startDate: today(), done: false }, true);
    } else {
        list.forEach(b => renderBookRow(container, b, false));
    }
}

function today() { return new Date().toISOString().split('T')[0]; }

function renderBookRow(container, book, isTemp) {
    const meta = calcMeta(book);
    const row = document.createElement('div');
    row.className = 'book-row';
    row.dataset.id = book.id;
    row.dataset.temp = isTemp ? '1' : '0';
    row.innerHTML = `
        <div class="book-title-line">
            <input class="book-title-input" type="text" placeholder="書名 Book Title" value="${book.title || ''}"
                oninput="updateField(this,'title')" />
            <input type="checkbox" class="book-done-cb" title="完成閱讀" ${book.done ? 'checked' : ''}
                onchange="toggleDone(this)" />
        </div>
        <div class="book-start-row">
            <span class="start-label">開始閱讀</span>
            <input type="date" class="start-date-input" value="${book.startDate || today()}"
                oninput="updateField(this,'startDate')" />
        </div>
        <div class="page-row">
            <input type="number" class="page-input" placeholder="0" min="0" value="${book.currentPage || ''}"
                oninput="updateField(this,'currentPage')" />
            <span class="page-sep">/</span>
            <input type="number" class="page-total-input" placeholder="總頁數" min="0" value="${book.totalPages || ''}"
                oninput="updateField(this,'totalPages')" />
            <span class="pages-label" style="margin-left:4px;">pages</span>
        </div>
        <div class="progress-bar-wrap"><div class="progress-bar-fill" style="width:${meta.pct}%"></div></div>
        <div class="book-meta">
            <span class="meta-chip pct">${meta.pct}% 完成</span>
            ${meta.eta !== null ? `<span class="meta-chip">預計還需 ${meta.eta} 天</span>` : ''}
            ${meta.daysRead > 0 ? `<span class="meta-chip">已讀 ${meta.daysRead} 天</span>` : ''}
        </div>
    `;
    container.appendChild(row);
}

function getOrCreateBook(row) {
    const books = loadBooks();
    const rawId = row.dataset.id;
    const isTemp = row.dataset.temp === '1';
    let book = books.find(b => String(b.id) === String(rawId));
    if (!book) {
        book = { id: rawId, title: '', currentPage: 0, totalPages: 0, startDate: today(), done: false };
        books.push(book);
        row.dataset.temp = '0';
        saveBooks(books);
    }
    return { books, book };
}

function updateField(el, field) {
    const row = el.closest('.book-row');
    const { books, book } = getOrCreateBook(row);
    book[field] = (field === 'title' || field === 'startDate') ? el.value : (parseInt(el.value) || 0);
    saveBooks(books);

    // Refresh meta display
    const meta = calcMeta(book);
    const fill = row.querySelector('.progress-bar-fill');
    if (fill) fill.style.width = meta.pct + '%';
    const chipsEl = row.querySelector('.book-meta');
    if (chipsEl) chipsEl.innerHTML = `
        <span class="meta-chip pct">${meta.pct}% 完成</span>
        ${meta.eta !== null ? `<span class="meta-chip">預計還需 ${meta.eta} 天</span>` : ''}
        ${meta.daysRead > 0 ? `<span class="meta-chip">已讀 ${meta.daysRead} 天</span>` : ''}
    `;
}

function toggleDone(el) {
    const row = el.closest('.book-row');
    const { books, book } = getOrCreateBook(row);
    book.done = el.checked;
    saveBooks(books);
    renderBooks();
}

function addBookRow() {
    const books = loadBooks();
    const nb = { id: Date.now(), title: '', currentPage: 0, totalPages: 0, startDate: today(), done: false };
    books.push(nb);
    saveBooks(books);
    renderBooks();
}

// ── AI ──────────────────────────────────────────────────────

async function runAIAnalysis() {
    const books = loadBooks();
    const spin = document.getElementById('ai-spin');
    const icon = document.getElementById('ai-icon');
    const resultEl = document.getElementById('ai-result');

    spin.style.display = 'block'; icon.style.display = 'none';
    resultEl.classList.remove('show');

    const info = books.length > 0
        ? books.map(b => {
            const m = calcMeta(b);
            return `《${b.title || '未命名'}》已讀${b.currentPage}/${b.totalPages}頁（${m.pct}%），開始日${b.startDate||'未知'}，${b.done?'已完成':'進行中'}`;
          }).join('\n')
        : '（目前無閱讀紀錄）';

    const prompt = `你是一位閱讀教練，以下是使用者的閱讀紀錄：\n${info}\n\n請用繁體中文，語氣溫暖簡潔地給出：1) 閱讀狀況分析（2句），2) 具體閱讀建議（2-3點）。不要使用markdown格式或星號。`;

    try {
        const res = await fetch('https://api.anthropic.com/v1/messages', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                model: 'claude-sonnet-4-20250514',
                max_tokens: 1000,
                messages: [{ role: 'user', content: prompt }]
            })
        });
        const data = await res.json();
        const text = (data.content || []).map(c => c.text || '').join('') || '無法取得分析。';
        resultEl.innerText = text;
        resultEl.classList.add('show');
    } catch (e) {
        resultEl.innerText = '分析失敗，請稍後再試。';
        resultEl.classList.add('show');
    } finally {
        spin.style.display = 'none'; icon.style.display = 'inline';
    }
}

// ── MODAL ───────────────────────────────────────────────────

function openUnifiedModal() {
    document.getElementById('unifiedModal').classList.remove('hidden');
    document.getElementById('inputDate').valueAsDate = new Date();
    document.getElementById('inputEndDate').valueAsDate = new Date();
    currentCategory = 'Private';
    switchType('schedule');
}

function closeUnifiedModal() { document.getElementById('unifiedModal').classList.add('hidden'); }

function switchType(type) {
    currentType = type;
    const s = document.getElementById('schedule-fields-static');
    const r = document.getElementById('read-fields');
    const btn = document.getElementById('saveBtn');
    document.getElementById('tab-schedule').classList.toggle('active', type === 'schedule');
    document.getElementById('tab-reading').classList.toggle('active', type === 'reading');
    if (type === 'schedule') {
        s.style.display = 'block'; r.style.display = 'none';
        btn.style.backgroundColor = '#8DAAB9';
        setCategory(currentCategory);
    } else {
        s.style.display = 'none'; r.style.display = 'block';
        btn.style.backgroundColor = '#7F9F9F';
        renderBooks();
    }
}

function setCategory(cat) {
    currentCategory = cat;
    ['Work','Exercise','Travel','Tutor','Private'].forEach(c => {
        const b = document.getElementById(`cat-${c}`); if (b) b.classList.remove('selected');
    });
    const sel = document.getElementById(`cat-${cat}`); if (sel) sel.classList.add('selected');
    document.getElementById('work-hours-container').style.display = (cat === 'Tutor') ? 'flex' : 'none';
    document.getElementById('travel-end-container').style.display = (cat === 'Travel') ? 'flex' : 'none';
    document.getElementById('dateLabel').innerText = (cat === 'Travel') ? 'Start Date & Time' : 'Date & Time';
}

function handleSave() {
    const date = document.getElementById('inputDate').value; if (!date) return;
    const hour = document.getElementById('selectHour').value;
    const min = document.getElementById('selectMinute').value;
    let title, colorClass, end;

    if (currentType === 'schedule') {
        const main = document.getElementById('inputMain').value; if (!main) return;
        const colors = { Work:'#E8D1D1', Exercise:'#D5E0D5', Travel:'#EBE5D1', Tutor:'#EEDFD2', Private:'#E5E5E5' };
        colorClass = 'soft-' + currentCategory.toLowerCase();
        if (currentCategory === 'Tutor') {
            const wh = document.getElementById('workHours').value;
            title = `${hour}:${min} ${main}${wh?' ('+wh+'h)':''}`;
        } else if (currentCategory === 'Travel') {
            title = `${hour}:${min} ${main}`;
            const ed = document.getElementById('inputEndDate').value;
            const eh = document.getElementById('selectEndHour').value;
            const em = document.getElementById('selectEndMinute').value;
            end = `${ed}T${eh}:${em}:00`;
        } else {
            title = `${hour}:${min} ${main}`;
        }
        const colors2 = { Work:'bg-morandi-soft-red', Exercise:'bg-morandi-soft-green', Travel:'bg-morandi-soft-yellow', Tutor:'bg-morandi-soft-orange', Private:'bg-morandi-soft-gray' };
        colorClass = colors2[currentCategory];
    } else {
        const active = loadBooks().filter(b => !b.done);
        const b = active[0];
        title = b ? `📖 ${hour}:${min} ${b.title||'Reading'} (p.${b.currentPage||0}/${b.totalPages||0})` : `📖 ${hour}:${min} Reading`;
        colorClass = 'bg-morandi-soft-green';
    }

    const ev = { title, start: `${date}T${hour}:${min}:00`, end, extendedProps: { colorClass } };
    const storage = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
    storage.push(ev); localStorage.setItem(STORAGE_KEY, JSON.stringify(storage));
    calendar.addEvent(ev);
    closeUnifiedModal();
}

function showSection(id) { if (id === 'calendar') setTimeout(() => calendar.render(), 10); }
</script>
</body>
</html>
