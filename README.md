<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Аптечка Pro | GitHub Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap');
        body { font-family: 'Inter', sans-serif; -webkit-tap-highlight-color: transparent; background-color: #F8FAFC; color: #1e293b; }
        .glass { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px); }
        .btn-press:active { transform: scale(0.96); }
        .hide-scroll { -ms-overflow-style: none; scrollbar-width: none; }
        .hide-scroll::-webkit-scrollbar { display: none; }
        .shadow-soft { box-shadow: 0 10px 30px -5px rgba(0, 0, 0, 0.04), 0 4px 10px -2px rgba(0, 0, 0, 0.02); }
        .active-pill { box-shadow: 0 4px 20px rgba(99, 102, 241, 0.3); }
        @keyframes pulse-red { 0%, 100% { background-color: #ef4444; } 50% { background-color: #f87171; } }
        select, input { outline: none; border: none; background: transparent; width: 100%; font-weight: 700; font-size: 14px; color: #1e293b; }
        .input-box { background-color: #f1f5f9; border-radius: 1.25rem; padding: 0.85rem; transition: all 0.2s; border: 2px solid transparent; }
        .input-box:focus-within { background-color: #fff; border-color: #6366f1; box-shadow: 0 4px 12px rgba(99, 102, 241, 0.1); }
        .dashed-card { border: 2px dashed #cbd5e1; background: transparent; }
        label { font-size: 10px; font-weight: 800; color: #64748b; text-transform: uppercase; letter-spacing: 0.05em; margin-left: 4px; margin-bottom: 4px; display: block; }
    </style>
</head>
<body class="min-h-screen pb-32 text-slate-800">

    <div id="app" class="max-w-md mx-auto">
        <!-- Header -->
        <div class="sticky top-0 z-40 bg-[#F8FAFC]/80 backdrop-blur-md pt-8 px-5 pb-4 border-b border-slate-100">
            <div class="flex justify-between items-center mb-4">
                <div>
                    <div class="flex items-center gap-2 mb-1">
                        <button onclick="changeDate(-1)" class="text-indigo-400 p-1 btn-press">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M15 19l-7-7 7-7"/></svg>
                        </button>
                        <p id="current-date" class="text-[10px] font-black text-indigo-500 uppercase tracking-[0.2em] min-w-[140px] text-center"></p>
                        <button onclick="changeDate(1)" class="text-indigo-400 p-1 btn-press">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M9 5l7 7-7 7"/></svg>
                        </button>
                    </div>
                    <h1 id="view-title" class="text-3xl font-black tracking-tighter text-slate-900 uppercase italic">График</h1>
                </div>
                <div class="flex gap-2">
                    <button onclick="openDailyMealSettings()" class="w-11 h-11 flex items-center justify-center bg-white rounded-2xl shadow-soft btn-press text-amber-500 border border-amber-50">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364-6.364l-.707.707M6.343 17.657l-.707.707m12.728 0l-.707-.707M6.343 6.343l-.707-.707M12 5a7 7 0 100 14 7 7 0 000-14z"/></svg>
                    </button>
                    <button onclick="showModal('child-modal')" class="w-11 h-11 flex items-center justify-center bg-white rounded-2xl shadow-soft btn-press text-indigo-600 border border-indigo-50">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M18 9v3m0 0v3m0-3h3m-3 0h-3m-2-5a4 4 0 11-8 0 4 4 0 018 0zM3 20a6 6 0 0112 0v1H3v-1z"/></svg>
                    </button>
                </div>
            </div>
            <div id="child-switcher" class="flex gap-2 overflow-x-auto hide-scroll pb-1"></div>
        </div>

        <div id="content" class="px-5 mt-4"></div>

        <!-- Navbar -->
        <nav class="fixed bottom-6 left-5 right-5 glass rounded-[2.5rem] border border-white shadow-2xl p-2 flex gap-1 z-50 max-w-md mx-auto text-slate-800">
            <button onclick="setView('calendar')" id="nav-calendar" class="flex-1 flex flex-col items-center justify-center py-3 rounded-[2rem] gap-1 transition-all"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/></svg><span class="text-[8px] font-black uppercase">График</span></button>
            <button onclick="setView('list')" id="nav-list" class="flex-1 flex flex-col items-center justify-center py-3 rounded-[2rem] transition-all"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M4 6h16M4 12h16m-7 6h7"/></svg><span class="text-[8px] font-black uppercase">Список</span></button>
            <button onclick="setView('schemes')" id="nav-schemes" class="flex-1 flex flex-col items-center justify-center py-3 rounded-[2rem] transition-all"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/></svg><span class="text-[8px] font-black uppercase">Схемы</span></button>
            <button onclick="setView('archive')" id="nav-archive" class="flex-1 flex flex-col items-center justify-center py-3 rounded-[2rem] transition-all"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M5 8h14M5 12h14M5 16h14"/></svg><span class="text-[8px] font-black uppercase">Архив</span></button>
            <button onclick="openSettings()" id="nav-settings" class="flex-1 flex flex-col items-center justify-center py-3 rounded-[2rem] transition-all text-slate-400"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065z"/><path d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"/></svg><span class="text-[8px] font-black uppercase">Опции</span></button>
        </nav>
    </div>

    <!-- Medication Modal -->
    <div id="med-modal" class="fixed inset-0 glass hidden z-[60] flex items-center justify-center p-6" onclick="closeModal(event, 'med-modal')">
        <div class="bg-white w-full rounded-[3rem] p-8 shadow-2xl max-h-[90vh] overflow-y-auto hide-scroll border border-slate-50 text-slate-800" onclick="event.stopPropagation()">
            <h3 id="med-modal-title" class="text-2xl font-black mb-6 tracking-tight">Назначение</h3>
            
            <div class="space-y-4">
                <div id="med-child-row">
                    <label>Ребенок</label>
                    <div class="input-box"><select id="in-med-child" onchange="updateParentSelect()"></select></div>
                </div>

                <div class="p-5 bg-indigo-50/50 rounded-[2.5rem] border border-indigo-100/30 space-y-4">
                    <div class="flex gap-2 p-1 bg-white/50 rounded-2xl">
                        <button onclick="setMedType('drug')" id="type-drug" class="flex-1 py-2 text-[8px] font-black rounded-xl bg-white shadow-sm uppercase tracking-widest">Препарат</button>
                        <button onclick="setMedType('inhale')" id="type-inhale" class="flex-1 py-2 text-[8px] font-black rounded-xl text-slate-400 uppercase tracking-widest">Ингаляция</button>
                    </div>

                    <div>
                        <label>Название препарата 1</label>
                        <div class="input-box bg-white shadow-sm mb-3"><input type="text" id="in-med-name" placeholder="Что даем?"></div>
                        <div class="grid grid-cols-2 gap-3">
                            <div><label>Дозировка 1</label><div class="input-box bg-white shadow-sm"><input type="text" id="in-med-dose-val" placeholder="0"></div></div>
                            <div><label>Ед. изм. 1</label>
                                <div class="input-box bg-white shadow-sm"><select id="in-med-dose-unit">
                                    <option value="мл">мл</option><option value="кап.">кап</option><option value="тбл.">тбл</option><option value="мг">мг</option>
                                </select></div>
                            </div>
                        </div>
                    </div>

                    <div id="duo-section" class="hidden border-t border-indigo-100/50 pt-4 space-y-3">
                        <label class="text-indigo-600 font-bold">Название препарата 2</label>
                        <div class="input-box bg-white shadow-sm mb-3"><input type="text" id="in-med-name2" placeholder="Дополнительно"></div>
                        <div class="grid grid-cols-2 gap-3">
                            <div><label>Дозировка 2</label><div class="input-box bg-white shadow-sm"><input type="text" id="in-med-dose-val2" placeholder="0"></div></div>
                            <div><label>Ед. изм. 2</label>
                                <div class="input-box bg-white shadow-sm">
                                    <select id="in-med-dose-unit2">
                                        <option value="мл">мл</option><option value="кап.">кап</option><option value="тбл.">тбл</option><option value="мг">мг</option>
                                    </select>
                                </div>
                            </div>
                        </div>
                    </div>
                    <button onclick="toggleDuoInput()" id="btn-duo-toggle" class="w-full py-2 text-[8px] font-black text-indigo-600 uppercase bg-indigo-100/40 rounded-xl">+ Добавить 2-й препарат</button>

                    <div id="combine-row" class="flex items-center justify-between pt-2 border-t border-indigo-100/50">
                        <p class="text-[9px] font-black text-indigo-900 uppercase">Совместить прием</p>
                        <button onclick="toggleCombine()" id="btn-combine" class="w-10 h-5 rounded-full bg-slate-200 relative transition-all shadow-inner"><div id="combine-dot" class="absolute top-1 left-1 w-3 h-3 bg-white rounded-full transition-transform"></div></button>
                    </div>

                    <div id="parent-select-box" class="hidden space-y-2">
                        <label>В паре с лекарством:</label>
                        <div class="input-box bg-white shadow-sm"><select id="in-med-parent" onchange="syncWithParent()"></select></div>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div><label>Курс (дней)</label><div class="input-box"><input type="number" id="in-med-duration" value="5"></div></div>
                    <div id="med-start-row"><label>Дата начала</label><div class="input-box"><input type="date" id="in-med-start"></div></div>
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div><label>Частота (раз в день)</label><div class="input-box"><select id="in-med-count"><option value="1">1 р/д</option><option value="2">2 р/д</option><option value="3" selected>3 р/д</option><option value="4">4 р/д</option></select></div></div>
                    <div><label>Связь с едой</label><div class="input-box"><select id="in-med-food" onchange="toggleManualTime(this.value)">
                        <option value="before">До еды</option><option value="before_after">До или После</option><option value="with">С едой</option><option value="after_now">Сразу после</option><option value="after_1h">Через 1ч</option><option value="manual">Свое время</option>
                    </select></div></div>
                </div>
                <div id="manual-time-box" class="hidden"><label>Время 1-го приема</label><div class="input-box bg-indigo-50"><input type="time" id="in-med-manual-time" value="12:00"></div></div>
                <div class="flex items-center justify-between px-2 pt-2"><label>Цвет карточки</label><div class="input-box w-20"><input type="color" id="in-med-color" value="#6366f1" class="h-6 cursor-pointer"></div></div>
            </div>

            <div class="flex flex-col gap-3 mt-10">
                <button onclick="handleMedSave()" class="w-full bg-indigo-600 text-white py-5 rounded-[2.5rem] font-black btn-press shadow-xl uppercase text-xs tracking-widest">Сохранить</button>
                <button id="btn-delete-med" onclick="deleteCurrentMed()" class="w-full py-2 text-rose-400 font-bold text-[10px] hidden uppercase tracking-widest text-center">Удалить</button>
            </div>
        </div>
    </div>

    <!-- Modals (Child, Schemes, Settings) -->
    <div id="child-modal" class="fixed inset-0 glass hidden z-[70] flex items-center justify-center p-6" onclick="closeModal(event, 'child-modal')">
        <div class="bg-white w-full rounded-[3rem] p-10 shadow-2xl" onclick="event.stopPropagation()">
            <h3 class="text-2xl font-black mb-8 tracking-tighter">Новый ребенок</h3>
            <div class="input-box mb-8"><input type="text" id="in-child-name" placeholder="Имя ребенка"></div>
            <div class="flex gap-4">
                <button onclick="hideModal('child-modal')" class="flex-1 py-5 text-slate-400 font-black uppercase text-xs text-center">Отмена</button>
                <button onclick="saveChild()" class="flex-1 bg-slate-900 text-white py-5 rounded-3xl font-black btn-press shadow-xl uppercase text-xs">Создать</button>
            </div>
        </div>
    </div>

    <div id="settings-modal" class="fixed inset-0 glass hidden z-[60] flex items-center justify-center p-6" onclick="closeModal(event, 'settings-modal')">
        <div class="bg-white w-full rounded-[3rem] p-8 shadow-2xl max-h-[85vh] overflow-y-auto hide-scroll border border-slate-50 text-slate-800" onclick="event.stopPropagation()">
            <h3 class="text-2xl font-black mb-8 tracking-tight uppercase text-center text-slate-400 italic">Опции</h3>
            <div id="backup-section" class="mb-8">
                <p class="text-[10px] font-black text-indigo-500 uppercase tracking-widest mb-4 text-center">🔔 Уведомления</p>
                <button onclick="enableNotifications()" id="btn-notif" class="w-full p-4 bg-indigo-50 rounded-2xl text-[10px] font-black uppercase text-indigo-600 mb-6 border border-indigo-100 btn-press">Включить уведомления</button>

                <p class="text-[10px] font-black text-indigo-500 uppercase tracking-widest mb-4 text-center">Данные и отчеты</p>
                <div class="grid grid-cols-2 gap-3 mb-4">
                    <button onclick="exportDataJSON()" class="p-4 bg-slate-50 rounded-2xl text-[8px] font-black uppercase text-indigo-600 btn-press">💾 Export JSON</button>
                    <label class="p-4 bg-slate-50 rounded-2xl cursor-pointer text-center text-[8px] font-black uppercase text-indigo-600 btn-press">📂 Import JSON<input type="file" class="hidden" accept=".json" onchange="importDataJSON(event)"></label>
                </div>
                <button onclick="exportDailyExcel()" class="w-full p-4 bg-green-50 rounded-2xl text-[10px] font-black uppercase text-green-600 border border-green-100 btn-press shadow-sm">📊 Выгрузить график сегодня в Excel</button>
            </div>
            <div id="meal-settings-list" class="space-y-3 mb-8"></div>
            <div id="med-management-list" class="space-y-4 mb-6"></div>
            <button onclick="hideModal('settings-modal')" class="w-full bg-slate-900 text-white py-5 rounded-[2.5rem] font-black btn-press uppercase text-xs shadow-xl">Закрыть</button>
        </div>
    </div>

    <div id="scheme-modal" class="fixed inset-0 glass hidden z-[80] flex items-center justify-center p-6" onclick="closeModal(event, 'scheme-modal')">
        <div class="bg-white w-full rounded-[3rem] p-10 shadow-2xl" onclick="event.stopPropagation()">
            <h3 class="text-2xl font-black mb-2 text-slate-800 tracking-tight">Новая схема</h3>
            <p class="text-[10px] font-bold text-slate-400 uppercase mb-6 tracking-widest text-center">Лечение будет сохранено как шаблон</p>
            <div class="input-box mb-8"><input type="text" id="in-scheme-name" placeholder="Название (например: ОРВИ)"></div>
            <div class="flex gap-4">
                <button onclick="hideModal('scheme-modal')" class="flex-1 py-5 text-slate-400 font-black uppercase text-xs text-center text-center">Отмена</button>
                <button onclick="confirmSaveScheme()" class="flex-1 bg-indigo-600 text-white py-5 rounded-3xl font-black shadow-lg uppercase text-xs">Сохранить</button>
            </div>
        </div>
    </div>

    <div id="daily-meal-modal" class="fixed inset-0 glass hidden z-[65] flex items-center justify-center p-6" onclick="closeModal(event, 'daily-meal-modal')">
        <div class="bg-white w-full rounded-[3rem] p-8 shadow-2xl border border-slate-50" onclick="event.stopPropagation()">
            <h3 class="text-2xl font-black mb-2 tracking-tight text-center">Еда на сегодня</h3>
            <p id="meal-modal-date" class="text-[10px] font-bold text-indigo-400 uppercase mb-6 tracking-widest text-center"></p>
            <div class="space-y-3 mb-8" id="daily-meal-inputs"></div>
            <div class="flex gap-3">
                 <button onclick="resetDailyMeals()" class="flex-1 py-4 text-rose-400 font-bold text-[10px] uppercase border rounded-3xl">Сбросить</button>
                 <button onclick="hideModal('daily-meal-modal')" class="flex-1 bg-slate-900 text-white py-4 rounded-3xl font-black btn-press uppercase text-xs">Готово</button>
            </div>
        </div>
    </div>

    <script>
        // MASTER KEY FOR ALL DATA - DO NOT CHANGE
        const MASTER_DB_KEY = 'med_pro_master_sync_v_ultimate';

        let state = JSON.parse(localStorage.getItem(MASTER_DB_KEY)) || {
            children: [], meals: { b: "09:00", l: "13:30", d: "19:00" },
            dailyMeals: {}, history: {}, schemes: [], view: 'calendar', filterId: null, notified: []
        };

        let currentViewDate = new Date(), editingMedId = null, editingChildId = null, editingSchemeId = null;
        let isCombine = false, medType = 'drug', isDuo = false, lastScheduleString = "", isModalOpen = false, tempSchemeChildId = null;

        // --- Utils ---
        function safeVal(id, val = null) {
            const el = document.getElementById(id);
            if (!el) return '';
            if (val !== null) el.value = val;
            return el.value;
        }

        function calculateSchedule() {
            const now = new Date(), viewStr = currentViewDate.toISOString().split('T')[0], isToday = viewStr === now.toISOString().split('T')[0], nowMins = now.getHours() * 60 + now.getMinutes();
            let raw = [];
            const kids = state.filterId ? state.children.filter(c => c.id == state.filterId) : state.children;
            const currentMeals = getMealsForDate(viewStr);

            kids.forEach(child => {
                const base = [currentMeals.b, currentMeals.l, currentMeals.d, "21:00"];
                child.meds.forEach(med => {
                    const start = new Date(med.startDate); start.setHours(0,0,0,0);
                    const cur = new Date(currentViewDate); cur.setHours(0,0,0,0);
                    const dayNum = Math.floor((cur - start) / 86400000) + 1;
                    if(dayNum > 0 && dayNum <= (med.duration || 1)) {
                        for(let i = 0; i < (med.count || 1); i++) {
                            let mealMins = (med.food === 'manual') ? (parseInt(med.manualTime.split(':')[0])*60 + parseInt(med.manualTime.split(':')[1]) + (i*240)) : (parseInt(base[i % 4].split(':')[0])*60 + parseInt(base[i % 4].split(':')[1]));
                            raw.push({ ...med, childId: child.id, childName: child.name, mealMins, dayNum, doseIdx: i });
                        }
                    }
                });
            });

            let uiGroups = [];
            let groupsMap = {};
            raw.forEach(it => { const k = `${it.childId}-${it.mealMins}`; if(!groupsMap[k]) groupsMap[k] = []; groupsMap[k].push(it); });
            for(let key in groupsMap) {
                const meds = groupsMap[key]; const ref = meds[0].mealMins;
                meds.forEach(m => { m.effFood = m.food; if(m.food === 'before_after') { const id = `${m.id}-${m.doseIdx}`; if(isToday && !state.history[viewStr]?.[id] && nowMins > (ref - 15)) { m.effFood = 'after_now'; m.shifted = true; } else m.effFood = 'before'; } });
                ['before', 'with', 'after_now', 'after_1h', 'manual'].forEach(type => {
                    const g = meds.filter(m => m.effFood === type).sort((a,b) => a.id - b.id);
                    g.forEach((item, idx) => {
                        item.slot = item.parentId ? (g.find(p => p.id == item.parentId)?.slot || idx) : g.slice(0, idx).filter(x => !x.parentId).length;
                        const gap = 30; let time;
                        if(type === 'before') time = ref - ((g.filter(x => !x.parentId).length - item.slot) * gap);
                        else if(type === 'after_now') time = ref + 5 + (item.slot * gap);
                        else if(type === 'after_1h') time = ref + 60 + (item.slot * gap);
                        else time = ref + (item.slot * gap);
                        let gr = uiGroups.find(x => x.time === time && x.childId === item.childId);
                        if(gr) gr.items.push({ ...item, instId: `${item.id}-${item.doseIdx}`, done: !!state.history[viewStr]?.[`${item.id}-${item.doseIdx}`] });
                        else uiGroups.push({ time, childId: item.childId, childName: item.childName, items: [{ ...item, instId: `${item.id}-${item.doseIdx}`, done: !!state.history[viewStr]?.[`${item.id}-${item.doseIdx}`] }], color: item.color, shifted: item.shifted, type: item.type });
                    });
                });
            }
            return uiGroups.sort((a,b) => a.time - b.time);
        }

        function saveData() { localStorage.setItem(MASTER_DB_KEY, JSON.stringify(state)); render(); }
        function showModal(id) { const el = document.getElementById(id); if(el) { isModalOpen = true; el.classList.remove('hidden'); } }
        function hideModal(id) { const el = document.getElementById(id); if(el) { isModalOpen = false; el.classList.add('hidden'); } }
        function closeModal(e, id) { if(e.target.id === id) hideModal(id); }
        function setView(v) { state.view = v; lastScheduleString = ""; saveData(); }
        function changeDate(days) { currentViewDate.setDate(currentViewDate.getDate() + days); lastScheduleString = ""; render(); }
        function filterByChild(id) { state.filterId = id; lastScheduleString = ""; saveData(); }
        function getMealsForDate(dateStr) { return state.dailyMeals[dateStr] || state.meals; }

        function enableNotifications() {
            if (!("Notification" in window)) return alert("Браузер не поддерживает уведомления");
            Notification.requestPermission().then(p => { if (p === "granted") alert("Уведомления включены!"); });
        }

        function checkNotifications() {
            if (Notification.permission !== "granted") return;
            const now = new Date(), nowMins = now.getHours() * 60 + now.getMinutes(), todayStr = now.toISOString().split('T')[0];
            const groups = calculateSchedule();
            groups.forEach(g => {
                if (Math.abs(g.time - nowMins) <= 1) {
                    const notifId = `${todayStr}-${g.time}-${g.childId}`;
                    if (!state.notified) state.notified = [];
                    if (!state.notified.includes(notifId) && g.items.some(i => !i.done)) {
                        new Notification(`Прием лекарства: ${g.childName}`, { body: g.items.map(i => i.name).join(', ') });
                        state.notified.push(notifId);
                        saveData();
                    }
                }
            });
        }

        function toggleDuoInput(forced = null) {
            isDuo = forced !== null ? forced : !isDuo;
            const sec = document.getElementById('duo-section'); if(sec) sec.classList.toggle('hidden', !isDuo);
            const btn = document.getElementById('btn-duo-toggle'); if(btn) btn.innerText = isDuo ? '− Убрать 2-й препарат' : '+ Добавить 2-й препарат';
        }

        function setMedType(type) {
            medType = type;
            const dBtn = document.getElementById('type-drug'), iBtn = document.getElementById('type-inhale');
            if(dBtn) dBtn.className = type === 'drug' ? 'flex-1 py-2 text-[8px] font-black rounded-xl bg-white shadow-sm uppercase tracking-widest' : 'flex-1 py-2 text-[8px] font-black rounded-xl text-slate-400 uppercase tracking-widest';
            if(iBtn) iBtn.className = type === 'inhale' ? 'flex-1 py-2 text-[8px] font-black rounded-xl bg-white shadow-sm uppercase text-cyan-600 tracking-widest' : 'flex-1 py-2 text-[8px] font-black rounded-xl text-slate-400 uppercase tracking-widest';
            if(type === 'inhale' && safeVal('in-med-color') === '#6366f1') safeVal('in-med-color', '#0891b2');
        }

        function toggleCombine(val = null) {
            isCombine = val !== null ? val : !isCombine;
            const btn = document.getElementById('btn-combine'), dot = document.getElementById('combine-dot'), pBox = document.getElementById('parent-select-box');
            if(btn) btn.className = `w-10 h-5 rounded-full relative transition-all shadow-inner ${isCombine ? 'bg-indigo-500' : 'bg-slate-200'}`;
            if(dot) dot.style.transform = isCombine ? 'translateX(20px)' : 'translateX(0)';
            if(pBox) pBox.classList.toggle('hidden', !isCombine);
            if(isCombine) { updateParentSelect(); syncWithParent(); }
        }

        function syncWithParent() {
            const childId = safeVal('in-med-child'), parentId = safeVal('in-med-parent');
            const child = state.children.find(c => c.id == childId), parent = child?.meds.find(m => m.id == parentId);
            if (parent) { safeVal('in-med-count', parent.count); safeVal('in-med-food', parent.food); }
        }

        function updateParentSelect() {
            const cId = safeVal('in-med-child'), child = state.children.find(c => c.id == cId);
            const ps = document.getElementById('in-med-parent'); if(!child || !ps) return;
            const others = child.meds.filter(m => m.id != editingMedId && !m.parentId);
            ps.innerHTML = others.map(m => `<option value="${m.id}">${m.name}</option>`).join('') || '<option value="">Нет подходящих лекарств</option>';
        }

        function toggleManualTime(val) { const box = document.getElementById('manual-time-box'); if(box) box.classList.toggle('hidden', val !== 'manual'); }

        function showMedModal(targetId = null, medId = null, isScheme = false) {
            isModalOpen = true; 
            editingSchemeId = isScheme ? targetId : null;
            editingChildId = isScheme ? null : targetId;
            editingMedId = medId;

            showModal('med-modal');

            const source = isScheme ? state.schemes.find(s=>s.id == targetId) : state.children.find(c=>c.id == targetId);
            const med = (medId && source) ? source.meds.find(m => m.id == medId) : null;
            const todayStr = new Date().toISOString().split('T')[0];

            if(med) {
                document.getElementById('med-modal-title').innerText = "Изменить";
                safeVal('in-med-name', med.name || '');
                safeVal('in-med-name2', med.name2 || '');
                safeVal('in-med-dose-val', med.doseVal || '');
                safeVal('in-med-dose-unit', med.doseUnit || 'мл');
                safeVal('in-med-dose-val2', med.doseVal2 || '');
                safeVal('in-med-dose-unit2', med.doseUnit2 || 'мл');
                safeVal('in-med-duration', med.duration || 5);
                if(!isScheme) safeVal('in-med-start', med.startDate || todayStr);
                safeVal('in-med-food', med.food || 'before');
                safeVal('in-med-count', med.count || 3);
                safeVal('in-med-color', med.color || '#6366f1');
                setMedType(med.type || 'drug'); toggleCombine(false);
                toggleDuoInput(!!med.name2);
                toggleManualTime(med.food);
                document.getElementById('btn-delete-med').classList.remove('hidden');
            } else {
                document.getElementById('med-modal-title').innerText = "Назначение";
                safeVal('in-med-name', ''); safeVal('in-med-name2', '');
                safeVal('in-med-dose-val', ''); safeVal('in-med-dose-val2', '');
                safeVal('in-med-duration', '5');
                if(!isScheme) safeVal('in-med-start', todayStr);
                setMedType('drug'); toggleCombine(false); toggleDuoInput(false); toggleManualTime('before');
                document.getElementById('btn-delete-med').classList.add('hidden');
            }

            const childBox = document.getElementById('med-child-row'), startBox = document.getElementById('med-start-row'), combo = document.getElementById('combine-row');
            if(childBox) childBox.style.display = isScheme ? 'none' : 'block';
            if(startBox) startBox.style.display = isScheme ? 'none' : 'block';
            if(combo) combo.style.display = isScheme ? 'none' : 'flex';

            const select = document.getElementById('in-med-child');
            if(!isScheme && select) {
                select.innerHTML = state.children.map(c => `<option value="${c.id}">${c.name}</option>`).join('');
                if(targetId) select.value = targetId;
            }
        }

        function handleMedSave() {
            const medObj = {
                id: editingMedId || Date.now(),
                name: safeVal('in-med-name').trim(),
                name2: isDuo ? (safeVal('in-med-name2').trim() || null) : null,
                doseVal: safeVal('in-med-dose-val'),
                doseVal2: isDuo ? safeVal('in-med-dose-val2') : null,
                doseUnit: safeVal('in-med-dose-unit'),
                doseUnit2: isDuo ? safeVal('in-med-dose-unit2') : null,
                duration: parseInt(safeVal('in-med-duration')) || 1,
                food: safeVal('in-med-food'),
                manualTime: safeVal('in-med-manual-time') || '12:00',
                count: parseInt(safeVal('in-med-count')) || 1,
                color: safeVal('in-med-color'),
                type: medType
            };
            if(!medObj.name) return;
            if(editingSchemeId) { const sch = state.schemes.find(s => s.id == editingSchemeId); if(editingMedId) sch.meds = sch.meds.filter(m => m.id != editingMedId); sch.meds.push(medObj); }
            else { const cId = safeVal('in-med-child'), child = state.children.find(ch => ch.id == (editingMedId ? editingChildId : cId)); if(child) { medObj.startDate = safeVal('in-med-start'); medObj.parentId = isCombine ? safeVal('in-med-parent') : null; if(editingMedId) child.meds = child.meds.filter(m => m.id != editingMedId); child.meds.push(medObj); } }
            lastScheduleString = ""; hideModal('med-modal'); saveData();
        }

        function deleteCurrentMed() { if(editingSchemeId) { const sch = state.schemes.find(s => s.id == editingSchemeId); sch.meds = sch.meds.filter(m => m.id != editingMedId); } else { const c = state.children.find(ch => ch.id == editingChildId); if(c) c.meds = c.meds.filter(m => m.id != editingMedId); } lastScheduleString = ""; hideModal('med-modal'); saveData(); }
        function saveChild() { const n = safeVal('in-child-name'); if(n.trim()) { state.children.push({ id: Date.now(), name: n.trim(), meds: [] }); safeVal('in-child-name',''); lastScheduleString = ""; saveData(); } hideModal('child-modal'); }
        function openSchemeSaver(cId) { tempSchemeChildId = cId; const c = state.children.find(x=>x.id==cId); const today = new Date().toLocaleDateString('ru-RU', {day: '2-digit', month: '2-digit'}); safeVal('in-scheme-name', c ? `Схема ${c.name} (${today})` : `Схема (${today})`); showModal('scheme-modal'); }
        function openManualSchemeModal() { tempSchemeChildId = null; safeVal('in-scheme-name', ''); showModal('scheme-modal'); }
        function confirmSaveScheme() { const name = safeVal('in-scheme-name'); if(!name.trim()) return; const meds = tempSchemeChildId ? state.children.find(c => c.id == tempSchemeChildId)?.meds || [] : []; state.schemes.push({ id: Date.now(), title: name, meds: JSON.parse(JSON.stringify(meds)) }); hideModal('scheme-modal'); lastScheduleString = ""; saveData(); }
        function applyScheme(schId, cId) { const sch = state.schemes.find(s => s.id == schId), child = state.children.find(c => c.id == cId); if(!sch || !child) return; const today = new Date().toISOString().split('T')[0]; const newMeds = sch.meds.map(m => ({ ...m, id: Date.now() + Math.random(), startDate: today })); child.meds.push(...newMeds); lastScheduleString = ""; saveData(); setView('list'); }
        function deleteScheme(id) { state.schemes = state.schemes.filter(s => s.id !== id); lastScheduleString = ""; saveData(); }
        function toggleDoneGroup(dateStr, ids) { if(!state.history[dateStr]) state.history[dateStr] = {}; const allDone = ids.every(id => state.history[dateStr][id]); ids.forEach(id => state.history[dateStr][id] = !allDone); lastScheduleString = ""; saveData(); }
        function formatTimer(target) { const now = new Date(); const diff = target - (now.getHours() * 60 + now.getMinutes()); if (diff < 0) return `Опоздание`; return `Через ${Math.floor(diff/60) > 0 ? Math.floor(diff/60)+'ч ' : ''}${diff%60}м`; }

        function exportSchemeToExcel(schemeId) {
            const sch = state.schemes.find(s => s.id == schemeId); if(!sch) return;
            let csv = "\uFEFFНазвание;Дозировка 1;Дозировка 2;Частота;Курс;Привязка\n";
            sch.meds.forEach(m => {
                const foodMap = { before: "До еды", before_after: "До/После", with: "С едой", after_now: "Сразу после", after_1h: "Через час", manual: "Свое время" };
                csv += `${m.name}${m.type==='inhale'?' (Ингаляция)':''};${m.doseVal}${m.doseUnit};${m.name2 ? m.name2+' '+m.doseVal2+m.doseUnit2 : '-'};${m.count} р/д;${m.duration};${foodMap[m.food]}\n`;
            });
            const link = document.createElement("a"); link.href = URL.createObjectURL(new Blob([csv], { type: "text/csv;charset=utf-8;" })); link.download = `Схема_${sch.title}.csv`; link.click();
        }

        function exportDailyExcel() {
            const groups = calculateSchedule();
            let csv = "\uFEFFВремя;Ребенок;Лекарство;Тип;Статус\n";
            groups.forEach(g => {
                const h = Math.floor(g.time/60).toString().padStart(2,'0'), m = (g.time%60).toString().padStart(2,'0');
                const meds = g.items.map(i => `${i.name} (${i.doseVal}${i.doseUnit})${i.name2 ? ' + '+i.name2+' ('+i.doseVal2+i.doseUnit2+')' : ''}`).join(' • ');
                csv += `${h}:${m};${g.childName};${meds};${g.items[0].type==='inhale'?'Ингаляция':'Препарат'};${g.items.every(i => i.done) ? "Выполнено" : "Ожидает"}\n`;
            });
            const link = document.createElement("a"); link.href = URL.createObjectURL(new Blob([csv], { type: "text/csv;charset=utf-8;" })); link.download = `График_${currentViewDate.toLocaleDateString('ru-RU')}.csv`; link.click();
        }

        function render() {
            const now = new Date(), nowMins = now.getHours() * 60 + now.getMinutes(), viewStr = currentViewDate.toISOString().split('T')[0], isToday = viewStr === now.toISOString().split('T')[0];
            if(isModalOpen) return;
            const groups = calculateSchedule();
            const currentHash = JSON.stringify(groups.map(g => ({t: g.time, d: g.items.every(i=>i.done)})));
            const needsFull = currentHash !== lastScheduleString;

            const dateLabel = document.getElementById('current-date');
            if(dateLabel) dateLabel.innerText = isToday ? `Сегодня, ${currentViewDate.toLocaleDateString('ru-RU', { day: 'numeric', month: 'numeric' })}` : currentViewDate.toLocaleDateString('ru-RU', { day: 'numeric', month: 'short' });
            
            const sw = document.getElementById('child-switcher');
            if (sw) sw.innerHTML = state.children.length > 0 ? `<button onclick="filterByChild(null)" class="px-5 py-2 rounded-full text-[10px] font-black uppercase transition-all ${state.filterId === null ? 'bg-indigo-600 text-white shadow-lg active-pill' : 'bg-white text-slate-400 shadow-soft'}">Все</button>` + state.children.map(c => `<button onclick="filterByChild(${c.id})" class="px-5 py-2 rounded-full text-[10px] font-black uppercase transition-all whitespace-nowrap ${state.filterId == c.id ? 'bg-indigo-600 text-white shadow-lg active-pill' : 'bg-white text-slate-400 shadow-soft'}">${c.name}</button>`).join('') : '';

            if (!needsFull && state.view !== 'archive' && state.view !== 'schemes') {
                document.querySelectorAll('.timer-val').forEach(t => t.innerText = formatTimer(parseInt(t.dataset.mins)));
                return;
            }
            lastScheduleString = currentHash;
            const cont = document.getElementById('content'); if(!cont) return;

            if(state.view === 'list') {
                const upc = groups.filter(g => g.items.some(i => !i.done)), pst = groups.filter(g => g.items.every(i => i.done));
                let html = (state.filterId ? `<div onclick="showMedModal(${state.filterId})" class="p-6 rounded-[2.5rem] mb-4 dashed-card flex flex-col items-center justify-center gap-2 cursor-pointer btn-press"><span class="text-[10px] font-black text-indigo-400 uppercase tracking-widest">＋ Добавить назначение</span></div>` : '');
                
                html += upc.map(g => {
                    const h = Math.floor(g.time/60).toString().padStart(2,'0'), m = (g.time%60).toString().padStart(2,'0'), isInh = g.items.some(i => i.type === 'inhale');
                    return `<div class="bg-white p-6 rounded-[2.5rem] mb-4 shadow-soft border-l-[6px] ${isToday && g.time < nowMins ? 'bg-rose-50/10' : ''}" style="border-left-color: ${g.items[0].color}">
                        <div class="flex items-center justify-between mb-4"><div class="flex-1 pr-4"><p class="text-[9px] font-black text-indigo-500 uppercase mb-1">${g.childName} ${isInh ? '• 🌬️ Ингаляция' : ''}</p><div class="space-y-1">${g.items.map(i => {
                            let text = `${i.name} (${i.doseVal}${i.doseUnit})`;
                            if(i.name2) text += ` + ${i.name2} (${i.doseVal2}${i.doseUnit2})`;
                            return `<div onclick="showMedModal(${i.childId}, ${i.id})" class="cursor-pointer font-bold text-lg text-slate-800 line-clamp-1">${text}</div>`;
                        }).join('')}</div><p class="text-[10px] font-bold ${g.shifted ? 'text-amber-500' : 'text-slate-400'} uppercase mt-2">${g.shifted ? 'Перенос' : 'График'} • ${g.items[0].dayNum} из ${g.items[0].duration} дн</p></div><div class="text-right flex-shrink-0"><p class="text-xl font-black">${h}:${m}</p><p class="text-[9px] font-black timer-val" data-mins="${g.time}">${isToday ? formatTimer(g.time) : ''}</p></div></div>
                        <button onclick="toggleDoneGroup('${viewStr}', [${g.items.map(id=>`'${id.instId}'`)}])" class="w-full py-4 rounded-3xl font-black uppercase text-[10px] shadow-lg btn-press text-white" style="background-color: ${g.items[0].color}">${isInh ? 'СДЕЛАЛ' : 'ПРИНЯЛ'}</button>
                    </div>`;
                }).join('');
                
                const doneItems = pst.map(g => `<div class="bg-white/40 p-4 rounded-3xl mb-2 opacity-50 flex justify-between mx-2 border border-slate-100"><span class="text-[10px] font-bold text-slate-400 line-through">${g.items.map(i=> {
                    let t = (i.type==='inhale' ? 'Ингаляция сделана: ' : '') + i.name;
                    if(i.name2) t += ' + ' + i.name2;
                    return t;
                }).join(' • ')}</span><span class="text-[9px] font-black text-slate-300 uppercase">${Math.floor(g.time/60)}:${(g.time%60).toString().padStart(2,'0')}</span></div>`).join('');
                const saveBtn = (state.filterId && state.children.find(c=>c.id == state.filterId)?.meds.length > 0) ? `<div onclick="openSchemeSaver(${state.filterId})" class="p-4 rounded-3xl mt-8 mb-4 bg-indigo-50 border border-indigo-100 flex items-center justify-center gap-2 cursor-pointer btn-press text-indigo-600"><svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7H5a2 2 0 00-2 2v9a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-3m-1 4l-3 3m0 0l-3-3m3 3V4"/></svg><span class="text-[9px] font-black uppercase">💾 Сохранить это лечение как схему</span></div>` : '';
                
                cont.innerHTML = html + (pst.length ? `<h3 class="text-[10px] font-black text-slate-300 uppercase text-center mt-10 mb-4 tracking-widest">Выполнено сегодня</h3>` + doneItems : '') + saveBtn;
            } else if(state.view === 'calendar') {
                cont.innerHTML = `<div class="pt-6 px-2">${groups.map(g => {
                    const h = Math.floor(g.time/60).toString().padStart(2,'0'), m = (g.time%60).toString().padStart(2,'0'), isInh = g.items.some(i => i.type === 'inhale');
                    return `<div class="flex gap-6"><div class="w-10 text-right text-[11px] font-black text-slate-400 pt-1 tracking-tighter">${h}:${m}</div><div class="flex-1 pb-10 border-l-2 border-slate-100 pl-8 relative"><div class="absolute -left-[7px] top-1.5 w-3 h-3 rounded-full border-2 border-white shadow-sm" style="background:${g.items.every(i=>i.done) ? '#10b981' : g.items[0].color}"></div><div class="${g.items.every(i=>i.done) ? 'opacity-30' : ''}"><p class="text-[9px] font-black text-indigo-500 uppercase mb-1">${g.childName} ${isInh ? '• 🌬️' : ''}</p><h4 class="font-black text-slate-800 text-sm leading-tight">${g.items.map(i=> {
                        let t = i.name; if(i.name2) t += ' + ' + i.name2; return t;
                    }).join(' • ')}</h4></div></div></div>`;
                }).join('')}</div>`;
            } else if(state.view === 'schemes') {
                let sHtml = `<div onclick="openManualSchemeModal()" class="p-6 rounded-[2.5rem] mb-4 dashed-card flex flex-col items-center justify-center gap-2 cursor-pointer btn-press"><span class="text-[10px] font-black text-indigo-400 uppercase tracking-widest">＋ Создать новую схему</span></div>`;
                sHtml += `<div class="space-y-4">${state.schemes.map(sch => `
                    <div class="bg-white p-6 rounded-[3rem] shadow-soft border border-slate-50">
                        <div class="flex justify-between items-start mb-4"><div><h3 class="font-black text-lg">${sch.title}</h3><p class="text-[9px] font-bold text-slate-400 uppercase tracking-widest italic">Протокол лечения</p></div><div class="flex gap-1">
                            <button onclick="exportSchemeToExcel(${sch.id})" class="p-2 text-indigo-400 btn-press"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 17v-2a2 2 0 00-2-2H5a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2zm0 0h2a2 2 0 012 2v2a2 2 0 01-2 2H9a2 2 0 01-2-2v-2a2 2 0 012-2zM9 7V5a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2zm0 0h-2a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2V9a2 2 0 00-2-2z"/></svg></button>
                            <button onclick="deleteScheme(${sch.id})" class="p-2 text-rose-300 btn-press"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg></button>
                        </div></div>
                        <div onclick="showMedModal(${sch.id}, null, true)" class="p-4 rounded-[1.5rem] mb-4 dashed-card flex flex-col items-center justify-center gap-1 cursor-pointer btn-press text-indigo-500"><span class="text-[10px] font-black uppercase tracking-widest">＋ Добавить препарат в схему</span></div>
                        <div class="space-y-2 mb-6">${sch.meds.map(m => `
                            <div onclick="showMedModal(${sch.id}, ${m.id}, true)" class="bg-slate-50 p-4 rounded-[2rem] flex justify-between items-center cursor-pointer active:bg-slate-100 transition-all">
                                <div><div class="font-bold text-slate-800">${m.name} ${m.type==='inhale'?'🌬️':''}</div>${m.name2 ? `<div class="text-[10px] text-slate-500 mt-0.5">＋ ${m.name2}</div>` : ''}</div>
                                <div class="text-[10px] font-black text-indigo-500 uppercase text-right">
                                    ${m.doseVal}${m.doseUnit} ${m.name2 ? '/ '+m.doseVal2+m.doseUnit2 : ''}<br>
                                    <span class="text-[8px] text-slate-400 font-bold">${m.count}р/д • ${m.duration} дн.</span>
                                </div>
                            </div>`).join('')}</div>
                        <div class="relative">
                            <select onchange="if(this.value) { applyScheme(${sch.id}, this.value); this.value=''; }" class="w-full bg-indigo-600 text-white p-4 rounded-3xl font-black text-[10px] uppercase appearance-none text-center btn-press shadow-lg cursor-pointer italic">
                                <option value="" selected disabled>Применить к ребенку...</option>
                                ${state.children.map(c => `<option value="${c.id}">${c.name}</option>`).join('')}
                            </select>
                        </div>
                    </div>`).join('')}</div>`;
                cont.innerHTML = sHtml;
            } else if(state.view === 'archive') {
                const dates = Object.keys(state.history).sort((a,b) => new Date(b) - new Date(a));
                cont.innerHTML = dates.length ? dates.map(d => `<div onclick="currentViewDate=new Date('${d}'); setView('list');" class="bg-white p-6 rounded-[2.5rem] mb-4 shadow-soft text-center btn-press"><span class="text-[10px] font-black text-indigo-500 uppercase">${new Date(d).toLocaleDateString('ru-RU',{day:'numeric', month:'long'})}</span></div>`).join('') : `<div class="py-20 text-center text-slate-300 uppercase text-[10px] font-bold">История пуста</div>`;
            }
            ['calendar','list','archive','schemes'].forEach(v => { const el = document.getElementById('nav-'+v); if (el) el.className = `flex-1 flex flex-col items-center justify-center py-3 rounded-[2rem] transition-all ${state.view === v ? 'bg-indigo-600 text-white shadow-lg active-pill' : 'text-slate-400'}`; });
            const setBtn = document.getElementById('nav-settings'); if(setBtn) setBtn.className = `flex-1 flex flex-col items-center justify-center py-3 rounded-[2rem] transition-all text-slate-400`;
            const vt = document.getElementById('view-title'); if(vt) vt.innerText = state.view === 'list' ? 'Список' : state.view === 'calendar' ? 'График' : state.view === 'schemes' ? 'Схемы' : 'Архив';
        }

        function openSettings() {
            const ml = document.getElementById('meal-settings-list'); 
            if(ml) ml.innerHTML = `<div><label>Время приемов по умолчанию</label>` + Object.entries(state.meals).map(([k, v]) => `<div class="flex justify-between items-center bg-slate-50 p-3 rounded-2xl mb-2"><span class="text-xs font-bold uppercase text-slate-400 pl-2">${k === 'b' ? 'Завтрак' : k === 'l' ? 'Обед' : 'Ужин'}</span><input type="time" value="${v}" onchange="state.meals.${k}=this.value; lastScheduleString=''; saveData();" class="bg-white p-2 rounded-xl border-none font-black text-xs shadow-sm"></div>`).join('') + `</div>`;
            const cl = document.getElementById('med-management-list');
            if(cl) cl.innerHTML = `<p class="text-[10px] font-black text-indigo-500 uppercase tracking-widest text-center mb-4 mt-8">Профили детей</p>` + 
                state.children.map(child => `<div class="p-5 bg-slate-50 rounded-[3rem] border border-slate-100 mb-4">
                <div class="flex justify-between items-center mb-3"><span class="font-black text-[11px] text-slate-800 uppercase">${child.name}</span><button onclick="if(confirm('Удалить?')){state.children=state.children.filter(c=>c.id!==${child.id});saveData();openSettings();}" class="text-rose-400 font-bold text-[9px] uppercase text-center">Удалить</button></div>
                <button onclick="hideModal('settings-modal'); showMedModal(${child.id})" class="w-full py-3 bg-white text-indigo-600 rounded-2xl text-[9px] font-black uppercase tracking-widest border border-indigo-50 mb-3 btn-press shadow-sm">＋ Добавить назначение</button>
                <div class="space-y-1 mb-3">${child.meds.map(m => `<div onclick="hideModal('settings-modal'); showMedModal(${child.id}, ${m.id})" class="bg-white p-3 rounded-xl shadow-sm text-xs font-bold text-slate-600 flex justify-between items-center">${m.name}<svg class="w-3 h-3 text-slate-300 ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M9 5l7 7-7 7"/></svg></div>`).join('')}</div></div>`).join('');
            showModal('settings-modal');
        }

        function openDailyMealSettings() { const dt = currentViewDate.toISOString().split('T')[0], meals = getMealsForDate(dt); const dl = document.getElementById('meal-modal-date'); if(dl) dl.innerText = currentViewDate.toLocaleDateString('ru-RU', { day:'numeric', month:'long' }); const box = document.getElementById('daily-meal-inputs'); if(box) box.innerHTML = Object.entries(meals).map(([k,v])=>`<div class="flex justify-between items-center bg-slate-50 p-3 rounded-2xl mb-2"><span class="text-xs font-bold uppercase text-slate-400">${k==='b'?'Завтрак':k==='l'?'Обед':'Ужин'}</span><input type="time" value="${v}" onchange="updateDailyMeal('${dt}','${k}',this.value)" class="bg-white p-2 rounded-xl border-none font-black text-xs shadow-sm"></div>`).join(''); showModal('daily-meal-modal'); }
        function updateDailyMeal(dt,k,v){ if(!state.dailyMeals[dt]) state.dailyMeals[dt]={...state.meals}; state.dailyMeals[dt][k]=v; lastScheduleString=""; saveData(); }
        function resetDailyMeals(){ const dt=currentViewDate.toISOString().split('T')[0]; delete state.dailyMeals[dt]; lastScheduleString=""; saveData(); hideModal('daily-meal-modal'); }
        function exportDataJSON(){ const link = document.createElement("a"); link.href = URL.createObjectURL(new Blob([JSON.stringify(state, null, 2)], { type: "application/json" })); link.download = `med_pro_backup.json`; link.click(); }
        
        function importDataJSON(e){ 
            const f = e.target.files[0]; if(!f) return; 
            const r = new FileReader(); 
            r.onload = function(ev){ 
                try { 
                    const imp = JSON.parse(ev.target.result); 
                    if(imp && imp.children){ 
                        // Forced migration and sync
                        state = imp; 
                        state.view = 'calendar';
                        if(state.children.length > 0) state.filterId = state.children[0].id;
                        localStorage.setItem(APP_DB_KEY, JSON.stringify(state)); 
                        localStorage.setItem(MASTER_DB_KEY, JSON.stringify(state)); // Double sync fix
                        alert("Данные из файла успешно восстановлены!");
                        location.reload(); 
                    } else {
                        throw new Error("Invalid structure");
                    }
                } catch(err){ alert("Ошибка формата JSON файла. Убедитесь, что вы выбрали правильный файл резервной копии."); } 
            }; 
            r.readAsText(f); 
        }

        setInterval(checkNotifications, 60000);
        setInterval(render, 1000);
        setView(state.view);
    </script>
</body>
</html>
