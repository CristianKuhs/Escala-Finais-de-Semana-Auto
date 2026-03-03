<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Escala de Finais de Semana</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        /* Base colors */
        :root {
            --bg: #0f172a;
            --card-bg: #1e293b;
            --accent: #3b82f6;
            --success: #10b981;
            --error: #ef4444;
            --warning: #f59e0b;
            --text: #e5e7eb;
            --glass: rgba(255, 255, 255, 0.05);
            --font: 'Inter', sans-serif;
        }
        * {
            box-sizing: border-box;
        }
        body {
            margin: 0;
            font-family: var(--font);
            color: var(--text);
            background: var(--bg);
        }
        header {
            text-align: center;
            padding: 1rem;
            background: var(--card-bg);
            box-shadow: 0 2px 4px rgba(0,0,0,.5);
        }
        h1 {
            margin: 0;
        }
        .controls {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
            padding: 1rem;
            justify-content: center;
        }
        .btn {
            padding: 0.5rem 1rem;
            border: none;
            border-radius: 0.375rem;
            cursor: pointer;
            transition: background 0.2s ease;
            font-weight: 600;
        }
        .btn.primary { background: var(--accent); color: #fff; }
        .btn.primary:hover { background: #2563eb; }
        .btn.danger { background: var(--error); color: #fff; }
        .btn.danger:hover { background: #b91c1c; }
        #filterInput {
            padding: 0.5rem;
            border-radius: 0.375rem;
            border: 1px solid #334155;
            background: var(--card-bg);
            color: var(--text);
            flex: 1;
            max-width: 200px;
        }
        .status-panel {
            padding: 1rem;
            background: var(--card-bg);
            margin: 1rem;
            border-radius: 0.5rem;
            backdrop-filter: blur(5px);
        }
        .agents-list {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
        }
        .agent-card {
            background: var(--glass);
            padding: 0.5rem;
            border-radius: 0.5rem;
            min-width: 180px;
            display: flex;
            flex-direction: column;
            gap: 0.25rem;
        }
        .agent-card.status-ferias { border: 2px solid var(--warning); }
        .agent-card.status-folga { border: 2px solid var(--accent); }
        .agent-card.status-dayoff { border: 2px solid var(--error); }
        .agent-card select {
            width: 100%;
            padding: 0.25rem;
            border-radius: 0.25rem;
            background: #334155;
            color: var(--text);
            border: none;
        }
        .agent-card .remove-status {
            margin-top: 0.25rem;
            font-size: 0.8rem;
            color: var(--accent);
            background: none;
            border: none;
            cursor: pointer;
            text-decoration: underline;
            align-self: start;
        }
        .schedule {
            display: flex;
            flex-direction: column;
            gap: 1rem;
            padding: 1rem;
        }
        .week-card {
            background: var(--card-bg);
            padding: 1rem;
            border-radius: 0.5rem;
            backdrop-filter: blur(5px);
        }
        .week-card h3 {
            margin-top: 0;
        }
        .day-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            margin-top: 1rem;
        }
        .day-group h4 {
            margin: 0.5rem 0 0.25rem 0;
            color: var(--accent);
            text-transform: uppercase;
            font-size: 0.9rem;
        }
        .shift-row {
            display: flex;
            justify-content: space-between;
            padding: 0.5rem;
            border-radius: 0.25rem;
            transition: background 0.2s;
            cursor: pointer;
        }
        .shift-row:hover {
            background: var(--glass);
        }
        .shift-info {
            flex: 1;
        }
        .agent-name {
            flex: 1;
            font-weight: 600;
            min-width: 150px;
        }
        .agent-observation {
            margin-top: 0.25rem;
            color: var(--text);
            font-size: 0.75rem;
            opacity: 0.8;
        }
        .conflict {
            color: var(--error);
            font-weight: bold;
        }
        .filter-hidden {
            display: none !important;
        }
        /* modal styles */
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .modal.hidden {
            display: none;
        }
        .modal-content {
            background: var(--card-bg);
            padding: 1rem 1.5rem;
            border-radius: 0.5rem;
            max-width: 400px;
            text-align: center;
            position: relative;
            animation: fadeIn 0.3s ease;
        }
        .modal-close {
            position: absolute;
            top: 0.5rem;
            right: 0.5rem;
            cursor: pointer;
            font-size: 1.2rem;
        }
        @media (min-width: 768px) {
            .schedule {
                flex-direction: row;
                flex-wrap: wrap;
            }
            .week-card {
                flex: 1 1 45%;
            }
        }
        .week-card, .agent-card, .shift-row {
            transition: background 0.3s, transform 0.2s;
        }
        .week-card:hover, .agent-card:hover, .shift-row:hover {
            transform: translateY(-2px);
        }
        /* modal animation */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .generation-info {
            text-align: center;
            color: var(--text);
            font-size: 0.9rem;
            padding: 0.5rem;
        }
    </style>
</head>
<body>
    <header>
        <h1>Escala de Finais de Semana</h1>
    </header>
    <main>
        <section id="controls" class="controls">
            <button id="generateBtn" class="btn primary">Gerar Escala</button>
            <button id="resetBtn" class="btn danger">Resetar Escala</button>
            <input type="text" id="filterInput" placeholder="Filtrar por agente...">
        </section>
        <div id="generationInfo" class="generation-info"></div>
        <section id="statusPanel" class="status-panel">
            <h2>Controle de Agentes</h2>
            <div id="agentsList" class="agents-list"></div>
        </section>
        <section id="schedule" class="schedule"></section>
    </main>
    <!-- Modal explicativo -->
    <div id="modal" class="modal hidden">
        <div class="modal-content">
            <span id="modalClose" class="modal-close">&times;</span>
            <p id="modalMessage"></p>
        </div>
    </div>
    <script>
        /*
          script.js - lógica do sistema de geração de escala
          - cálculos de horários
          - geração aleatória respeitando 11h de descanso
          - painel de controle de agentes com status
          - persistência via localStorage
          - edição manual simples
          - validações e modal explicativo
        */
        // utilitário de cálculo de horas
        function timeToMinutes(t) {
            const [h, m] = t.split(':').map(Number);
            return h * 60 + m;
        }
        function minutesToTime(min) {
            const h = Math.floor(min / 60) % 24;
            const m = min % 60;
            return String(h).padStart(2, '0') + ':' + String(m).padStart(2, '0');
        }
        function diffMinutes(start, end) {
            // assume start/end em minutos do mesmo dia ou end no dia seguinte
            let d = end - start;
            if (d < 0) d += 24 * 60;
            return d;
        }
        // constantes fornecidas
        const WEEKEND_SHIFTS = {
          saturday: [
            { shift: "Turno 1", entry1: "07:30", exit1: "11:30", entry2: "12:30", exit2: "15:50" },
            { shift: "Turno 2", entry1: "08:00", exit1: "12:30", entry2: "13:30", exit2: "16:20" },
            { shift: "Turno 3", entry1: "09:00", exit1: "13:30", entry2: "14:30", exit2: "17:20" },
            { shift: "Turno 4", entry1: "10:00", exit1: "14:00", entry2: "15:00", exit2: "18:20" },
            { shift: "Turno 5", entry1: "12:00", exit1: "15:00", entry2: "16:00", exit2: "20:20" },
            { shift: "Turno 6", entry1: "12:00", exit1: "16:00", entry2: "17:00", exit2: "21:20" },
          ],
          sunday: [
            { shift: "Turno 1", entry1: "08:00", exit1: "12:00", entry2: "13:00", exit2: "16:20" },
            { shift: "Turno 2", entry1: "08:00", exit1: "12:30", entry2: "13:30", exit2: "16:20" },
            { shift: "Turno 3", entry1: "12:00", exit1: "14:00", entry2: "15:00", exit2: "20:20" },
            { shift: "Turno 4", entry1: "12:00", exit1: "15:00", entry2: "16:00", exit2: "20:20" }
          ]
        };
        const MONTH_WEEKENDS_CONFIG = {
          week1: { label: "Semana 1", weekend: "1º Fim de Semana" },
          week2: { label: "Semana 2", weekend: "2º Fim de Semana" },
          week3: { label: "Semana 3", weekend: "3º Fim de Semana" },
          week4: { label: "Semana 4", weekend: "4º Fim de Semana" }
        };
        // agentes e horários base de sexta-feira (entrada e saída)
        const AGENTS = [
          { name: 'Charlote', fridayStart: '13:00', fridayExit: '21:20' },
          { name: 'Eduardo', fridayStart: '13:40', fridayExit: '22:00' },
          { name: 'Emilly', fridayStart: '13:40', fridayExit: '22:00' },
          { name: 'Fernando', fridayStart: '13:00', fridayExit: '21:20' },
          { name: 'João', fridayStart: '13:00', fridayExit: '21:20' },
          { name: 'João Lucas', fridayStart: '13:00', fridayExit: '21:20' },
          { name: 'Kaua', fridayStart: '13:40', fridayExit: '22:00' },
          { name: 'Pamela', fridayStart: '07:30', fridayExit: '15:50' },
          { name: 'Richard', fridayStart: '08:00', fridayExit: '16:20' }
        ];
        // possible statuses
        const STATUSES = ['Disponível','FÉRIAS','FOLGA','DAY OFF'];
        // dados persistidos
        let agentStatus = {}; // name -> status
        let scheduleData = null; // generated schedule
        let lastGenMonth = null;
        function loadFromStorage() {
            const s = localStorage.getItem('agentStatus');
            if (s) agentStatus = JSON.parse(s);
            const sch = localStorage.getItem('scheduleData');
            if (sch) scheduleData = JSON.parse(sch);
            lastGenMonth = localStorage.getItem('lastGenMonth');
        }
        function saveToStorage() {
            localStorage.setItem('agentStatus', JSON.stringify(agentStatus));
            if (scheduleData) localStorage.setItem('scheduleData', JSON.stringify(scheduleData));
            if (lastGenMonth) localStorage.setItem('lastGenMonth', lastGenMonth);
        }
        function resetSchedule() {
            scheduleData = null;
            lastGenMonth = null;
            localStorage.removeItem('scheduleData');
            localStorage.removeItem('lastGenMonth');
            renderSchedule();
        }
        // remove agentes que não estão mais disponíveis da escala já gerada
        function sanitizeSchedule() {
            if (!scheduleData) return;
            Object.keys(scheduleData).forEach(weekKey => {
                ['saturday','sunday'].forEach(day => {
                    scheduleData[weekKey][day].forEach(row => {
                        if (row.agent && agentStatus[row.agent] !== 'Disponível') {
                            row.agent = '';
                        }
                    });
                });
            });
            saveToStorage();
        }
        function initStatuses() {
            AGENTS.forEach(a => {
                if (!agentStatus[a.name]) agentStatus[a.name] = 'Disponível';
            });
        }
        function renderAgentsPanel() {
            const container = document.getElementById('agentsList');
            container.innerHTML = '';
            AGENTS.forEach(agent => {
                const card = document.createElement('div');
                card.className = 'agent-card';
                function refreshCardColor() {
                    card.classList.remove('status-ferias','status-folga','status-dayoff');
                    const s = agentStatus[agent.name];
                    if (s === 'FÉRIAS') card.classList.add('status-ferias');
                    if (s === 'FOLGA') card.classList.add('status-folga');
                    if (s === 'DAY OFF') card.classList.add('status-dayoff');
                }
                refreshCardColor();
                const nameEl = document.createElement('div');
                nameEl.textContent = agent.name;
                nameEl.style.fontWeight = '600';
                const select = document.createElement('select');
                STATUSES.forEach(s => {
                    const opt = document.createElement('option');
                    opt.value = s;
                    opt.textContent = s;
                    if (agentStatus[agent.name] === s) opt.selected = true;
                    select.appendChild(opt);
                });
                select.addEventListener('change', () => {
                    agentStatus[agent.name] = select.value;
                    sanitizeSchedule();
                    saveToStorage();
                    refreshCardColor();
                    renderSchedule();
                });
                const removeBtn = document.createElement('button');
                removeBtn.className = 'remove-status';
                removeBtn.textContent = 'Remover Status';
                removeBtn.addEventListener('click', () => {
                    agentStatus[agent.name] = 'Disponível';
                    select.value = 'Disponível';
                    saveToStorage();
                    refreshCardColor();
                });
                card.appendChild(nameEl);
                card.appendChild(select);
                card.appendChild(removeBtn);
                container.appendChild(card);
            });
        }
        function showModal(msg) {
            const modal = document.getElementById('modal');
            document.getElementById('modalMessage').textContent = msg;
            modal.classList.remove('hidden');
        }
        function hideModal() {
            document.getElementById('modal').classList.add('hidden');
        }
        function openManualEdit(rowEl) {
            const week = rowEl.dataset.week;
            const day = rowEl.dataset.day;
            const idx = parseInt(rowEl.dataset.shiftIndex, 10);
            const current = scheduleData[week][day][idx].agent || '---';
            const options = ['---', ...AGENTS.map(a=>a.name)];
            let choice = prompt(`Agente atual: ${current}\nDigite novo agente ou '---' para remover:\n${options.join(', ')}`);
            if (choice === null) return;
            choice = choice.trim();
            if (!options.includes(choice)) {
                alert('Opção inválida');
                return;
            }
            if (choice !== '---' && agentStatus[choice] !== 'Disponível') {
                alert('Agente não está marcado como disponível. Ajuste o status primeiro.');
                return;
            }
            // validação de descanso
            if (choice !== '---') {
                const m = scheduleData[week][day][idx];
                const entry = timeToMinutes(m.entry);
                // se for sábado, verificar descanso de sexta
                if (day === 'saturday') {
                    const ag = AGENTS.find(a=>a.name===choice);
                    const diff = diffMinutes(timeToMinutes(ag.fridayExit), entry);
                    if (diff < 11*60) {
                        showModal('Conflito: menos de 11h após saída de sexta.');
                        return;
                    }
                }
                // se for domingo, verificar descanso de sábado
                if (day === 'sunday') {
                    // encontre a saída do mesmo agente no sábado
                    const sat = scheduleData[week].saturday.find(s=>s.agent===choice);
                    if (sat) {
                        const lastExit = timeToMinutes(sat.exit);
                        const diff = (entry + 24*60) - lastExit;
                        if (diff < 11*60) {
                            showModal('Conflito: menos de 11h entre sábado e domingo.');
                            return;
                        }
                    }
                    // também verificar sexta caso tenha não trabalhado sábado
                    const ag = AGENTS.find(a=>a.name===choice);
                    const difff = diffMinutes(timeToMinutes(ag.fridayExit), entry);
                    if (difff < 11*60) {
                        showModal('Conflito: menos de 11h após saída de sexta.');
                        return;
                    }
                }
            }
            scheduleData[week][day][idx].agent = choice === '---' ? '' : choice;
            saveToStorage();
            renderSchedule();
        }
        function generateSchedule() {
            // inicializa saídas de sexta
            const fridayExitMinutes = {};
            AGENTS.forEach(a => {
                fridayExitMinutes[a.name] = timeToMinutes(a.fridayExit);
            });
            const result = {};
            let modalShown = false;
            const conflicts = [];
            // para cada semana
            Object.keys(MONTH_WEEKENDS_CONFIG).forEach(weekKey => {
                result[weekKey] = { saturday: [], sunday: [] };
                let lastExitSat = {}; // nome -> minutos
                // saturday
                WEEKEND_SHIFTS.saturday.forEach(shift => {
                    const entryMin = timeToMinutes(shift.entry1);
                    // candidatos
                    let candidates = AGENTS.filter(a => {
                        if (agentStatus[a.name] !== 'Disponível') return false;
                        // não ter trabalhado outro turno no mesmo sábado
                        if (result[weekKey].saturday.some(r=>r.agent===a.name)) return false;
                        // descanso de sexta
                        const diff = diffMinutes(fridayExitMinutes[a.name], entryMin);
                        return diff >= 11*60;
                    }).map(a=>a.name);
                    if (candidates.length === 0) {
                        if (!modalShown) {
                            showModal(`Não há agentes disponíveis para ${weekKey} sábado ${shift.shift} devido às regras de descanso.`);
                            modalShown = true;
                        }
                    }
                    // seleção aleatória
                    const chosen = candidates[Math.floor(Math.random()*candidates.length)];
                    result[weekKey].saturday.push({ shift: shift.shift, agent: chosen || '---', entry: shift.entry1, exit: shift.exit2 });
                    if (chosen) lastExitSat[chosen] = timeToMinutes(shift.exit2);
                });
                // sunday
                WEEKEND_SHIFTS.sunday.forEach(shift => {
                    const entryMin = timeToMinutes(shift.entry1);
                    let candidates = AGENTS.filter(a => {
                        if (agentStatus[a.name] !== 'Disponível') return false;
                        // não ter trabalhado outro turno no mesmo domingo
                        if (result[weekKey].sunday.some(r=>r.agent===a.name)) return false;
                        // descanso de sábado
                        const lastExit = lastExitSat[a.name];
                        if (lastExit === undefined) {
                            // não trabalhou no sábado, então só cheque descanso friday->sunday?
                            const diff = diffMinutes(fridayExitMinutes[a.name], entryMin);
                            return diff >= 11*60;
                        }
                        const diff = (entryMin + 24*60) - lastExit;
                        return diff >= 11*60;
                    }).map(a=>a.name);
                    if (candidates.length === 0) {
                        if (!modalShown) {
                            showModal(`Não há agentes disponíveis para ${weekKey} domingo ${shift.shift} devido às regras de descanso.`);
                            modalShown = true;
                        }
                    }
                    const chosen = candidates[Math.floor(Math.random()*candidates.length)];
                    result[weekKey].sunday.push({ shift: shift.shift, agent: chosen || '---', entry: shift.entry1, exit: shift.exit2 });
                });
            });
            scheduleData = result;
            lastGenMonth = new Date().toISOString();
            saveToStorage();
            renderSchedule();
        }
        function renderSchedule() {
            const container = document.getElementById('schedule');
            container.innerHTML = '';
            if (!scheduleData) return;
            Object.entries(MONTH_WEEKENDS_CONFIG).forEach(([weekKey, weekCfg]) => {
                const weekDiv = document.createElement('div');
                weekDiv.className = 'week-card';
                const title = document.createElement('h3');
                title.textContent = `${weekCfg.label} - ${weekCfg.weekend}`;
                weekDiv.appendChild(title);
                ['saturday','sunday'].forEach(day => {
                    const dayGroup = document.createElement('div');
                    dayGroup.className = 'day-group';
                    const dayTitle = document.createElement('h4');
                    dayTitle.textContent = day.charAt(0).toUpperCase() + day.slice(1);
                    dayGroup.appendChild(dayTitle);
                    (scheduleData[weekKey][day]||[]).forEach((row, idx)=>{
                        const rowEl = document.createElement('div');
                        rowEl.className = 'shift-row';
                        rowEl.dataset.week = weekKey;
                        rowEl.dataset.day = day;
                        rowEl.dataset.shiftIndex = idx;
                        const info = document.createElement('div');
                        info.className = 'shift-info';
                        info.textContent = `${row.shift}: ${row.entry}–${row.exit}`;
                        const agentEl = document.createElement('div');
                        agentEl.className = 'agent-name';
                        agentEl.textContent = row.agent || '---';
                        if (!row.agent) {
                            rowEl.classList.add('conflict');
                        }
                        rowEl.appendChild(info);
                        rowEl.appendChild(agentEl);
                        // observação automática com horário base do agente
                        if (row.agent) {
                            const ag = AGENTS.find(a=>a.name===row.agent);
                            if (ag) {
                                const obs = document.createElement('div');
                                obs.className = 'agent-observation';
                                obs.textContent = `${ag.fridayStart} até ${ag.fridayExit} (${ag.fridayStart} / ${ag.fridayExit})`;
                                rowEl.appendChild(obs);
                            }
                        }
                        // click to modify manually
                        rowEl.addEventListener('click', () => {
                            openManualEdit(rowEl);
                        });
                        dayGroup.appendChild(rowEl);
                    });
                    weekDiv.appendChild(dayGroup);
                });
                container.appendChild(weekDiv);
            });
            applyFilter();
            updateGenerationInfo();
        }
        function applyFilter() {
            const term = document.getElementById('filterInput').value.toLowerCase();
            document.querySelectorAll('.shift-row').forEach(row => {
                const text = row.textContent.toLowerCase();
                if (term && !text.includes(term)) row.classList.add('filter-hidden');
                else row.classList.remove('filter-hidden');
            });
        }
        function updateGenerationInfo() {
            const div = document.getElementById('generationInfo');
            if (lastGenMonth) {
                const d = new Date(lastGenMonth);
                div.textContent = `Última geração: ${d.toLocaleString()}`;
            } else {
                div.textContent = 'Nenhuma escala gerada ainda.';
            }
        }
        // listeners
        window.addEventListener('load', () => {
            loadFromStorage();
            initStatuses();
            renderAgentsPanel();
            renderSchedule();
            document.getElementById('generateBtn').addEventListener('click', generateSchedule);
            document.getElementById('resetBtn').addEventListener('click', resetSchedule);
            document.getElementById('filterInput').addEventListener('input', applyFilter);
            document.getElementById('modalClose').addEventListener('click', hideModal);
        });
    </script>
</body>
</html>
