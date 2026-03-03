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
            --header-bg: #0f4c75;
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
            gap: 1.5rem;
            padding: 1rem;
        }
        .week-card {
            background: var(--card-bg);
            padding: 1.5rem;
            border-radius: 0.75rem;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        .week-card h3 {
            margin: 0 0 1.5rem 0;
            color: var(--accent);
            font-size: 1.2rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .days-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1.5rem;
        }
        .day-schedule {
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 0.5rem;
            overflow: hidden;
        }
        .day-header {
            background: var(--header-bg);
            padding: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            font-size: 0.9rem;
            letter-spacing: 0.5px;
            border-bottom: 2px solid var(--accent);
        }
        .shifts-table {
            display: flex;
            flex-direction: column;
        }
        .shift-cell {
            display: grid;
            grid-template-columns: 0.8fr 1fr 1.2fr;
            gap: 0.5rem;
            padding: 0.75rem;
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
            align-items: center;
            cursor: pointer;
            transition: background 0.2s;
            font-size: 0.9rem;
        }
        .shift-cell:hover {
            background: rgba(59, 130, 246, 0.1);
        }
        .shift-cell:last-child {
            border-bottom: none;
        }
        .shift-label {
            font-weight: 600;
            color: var(--accent);
            font-size: 0.85rem;
        }
        .shift-time {
            font-size: 0.85rem;
            color: rgba(229, 231, 235, 0.7);
        }
        .agent-assignment {
            font-weight: 600;
            color: var(--text);
            min-height: 1.2rem;
        }
        .agent-assignment.empty {
            color: var(--error);
            font-weight: bold;
            opacity: 0.7;
        }
        .agent-info {
            font-size: 0.75rem;
            color: rgba(229, 231, 235, 0.6);
            margin-top: 0.2rem;
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
            border: 1px solid rgba(59, 130, 246, 0.3);
        }
        .modal-close {
            position: absolute;
            top: 0.5rem;
            right: 0.5rem;
            cursor: pointer;
            font-size: 1.2rem;
        }
        @media (max-width: 900px) {
            .days-container {
                grid-template-columns: 1fr;
            }
        }
        .week-card, .agent-card, .shift-cell {
            transition: background 0.3s, transform 0.2s;
        }
        .week-card:hover, .agent-card:hover, .shift-cell:hover {
            transform: translateY(-1px);
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
            background: rgba(16, 185, 129, 0.1);
            margin: 0 1rem 1rem 1rem;
            border-radius: 0.375rem;
            border: 1px solid rgba(16, 185, 129, 0.2);
        }
        .table-header-row {
            display: grid;
            grid-template-columns: 0.8fr 1fr 1.2fr;
            gap: 0.5rem;
            padding: 0.5rem 0.75rem;
            background: rgba(59, 130, 246, 0.1);
            font-weight: 700;
            font-size: 0.8rem;
            border-bottom: 1px solid var(--accent);
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
          - NOVA REGRA: quem trabalha sábado NÃO pode trabalhar domingo
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
            let d = end - start;
            if (d < 0) d += 24 * 60;
            return d;
        }
        // função para shuffle - melhor aleatoriedade
        function shuffle(array) {
            const arr = [...array];
            for (let i = arr.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [arr[i], arr[j]] = [arr[j], arr[i]];
            }
            return arr;
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
            // validação de descanso e regra de sábado/domingo
            if (choice !== '---') {
                const m = scheduleData[week][day][idx];
                const entry = timeToMinutes(m.entry);
                // NOVA REGRA: se tá em domingo, não pode ter trabalhado sábado
                if (day === 'sunday') {
                    const satWorked = scheduleData[week].saturday.find(s=>s.agent===choice);
                    if (satWorked) {
                        showModal('❌ Conflito: este agente trabalha SÁBADO desta semana. Não pode trabalhar DOMINGO também!');
                        return;
                    }
                }
                // NOVA REGRA: se tá em sábado, não pode trabalhar domingo
                if (day === 'saturday') {
                    const sunWorked = scheduleData[week].sunday.find(s=>s.agent===choice);
                    if (sunWorked) {
                        showModal('❌ Conflito: este agente trabalha DOMINGO desta semana. Não pode trabalhar SÁBADO também!');
                        return;
                    }
                }
                // verificar descanso de sexta para sábado
                if (day === 'saturday') {
                    const ag = AGENTS.find(a=>a.name===choice);
                    const diff = diffMinutes(timeToMinutes(ag.fridayExit), entry);
                    if (diff < 11*60) {
                        showModal('⚠️ Conflito: menos de 11h de descanso após saída de sexta (necesário ' + Math.ceil((11*60 - diff)/60) + 'h mais).');
                        return;
                    }
                }
                // verificar descanso de sexta para domingo
                if (day === 'sunday') {
                    const ag = AGENTS.find(a=>a.name===choice);
                    const difff = diffMinutes(timeToMinutes(ag.fridayExit), entry);
                    if (difff < 11*60) {
                        showModal('⚠️ Conflito: menos de 11h de descanso após saída de sexta.');
                        return;
                    }
                }
            }
            scheduleData[week][day][idx].agent = choice === '---' ? '' : choice;
            saveToStorage();
            renderSchedule();
        }
        function generateSchedule() {
            const fridayExitMinutes = {};
            AGENTS.forEach(a => {
                fridayExitMinutes[a.name] = timeToMinutes(a.fridayExit);
            });
            const result = {};
            let modalShown = false;
            // para cada semana
            Object.keys(MONTH_WEEKENDS_CONFIG).forEach(weekKey => {
                result[weekKey] = { saturday: [], sunday: [] };
                const workingSaturday = new Set(); // quem trabalha sábado
                // SATURDAY - shuffle para melhor aleatoriedade
                const satShifts = shuffle(WEEKEND_SHIFTS.saturday);
                satShifts.forEach(shift => {
                    const entryMin = timeToMinutes(shift.entry1);
                    let candidates = AGENTS.filter(a => {
                        if (agentStatus[a.name] !== 'Disponível') return false;
                        if (result[weekKey].saturday.some(r=>r.agent===a.name)) return false;
                        const diff = diffMinutes(fridayExitMinutes[a.name], entryMin);
                        return diff >= 11*60;
                    }).map(a=>a.name);
                    // shuffle para maior aleatoriedade na seleção
                    candidates = shuffle(candidates);
                    if (candidates.length === 0) {
                        if (!modalShown) {
                            showModal(`⚠️ Sem agentes disponíveis para ${weekKey} sábado ${shift.shift}.`);
                            modalShown = true;
                        }
                    }
                    const chosen = candidates[0];
                    result[weekKey].saturday.push({ 
                        shift: shift.shift, 
                        agent: chosen || '', 
                        entry: shift.entry1, 
                        exit: shift.exit2 
                    });
                    if (chosen) {
                        workingSaturday.add(chosen);
                    }
                });
                // SUNDAY - NOVA REGRA: não pode quem trabalhou sábado
                const sunShifts = shuffle(WEEKEND_SHIFTS.sunday);
                sunShifts.forEach(shift => {
                    const entryMin = timeToMinutes(shift.entry1);
                    let candidates = AGENTS.filter(a => {
                        if (agentStatus[a.name] !== 'Disponível') return false;
                        // REGRA PRINCIPAL: não pode se trabalhou sábado
                        if (workingSaturday.has(a.name)) return false;
                        if (result[weekKey].sunday.some(r=>r.agent===a.name)) return false;
                        const diff = diffMinutes(fridayExitMinutes[a.name], entryMin);
                        return diff >= 11*60;
                    }).map(a=>a.name);
                    // shuffle para maior aleatoriedade
                    candidates = shuffle(candidates);
                    if (candidates.length === 0) {
                        if (!modalShown) {
                            showModal(`⚠️ Sem agentes disponíveis para ${weekKey} domingo ${shift.shift}.`);
                            modalShown = true;
                        }
                    }
                    const chosen = candidates[0];
                    result[weekKey].sunday.push({ 
                        shift: shift.shift, 
                        agent: chosen || '', 
                        entry: shift.entry1, 
                        exit: shift.exit2 
                    });
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
                const daysContainer = document.createElement('div');
                daysContainer.className = 'days-container';
                ['saturday','sunday'].forEach(day => {
                    const daySchedule = document.createElement('div');
                    daySchedule.className = 'day-schedule';
                    const dayHeader = document.createElement('div');
                    dayHeader.className = 'day-header';
                    dayHeader.textContent = day === 'saturday' ? '📅 SÁBADO' : '📅 DOMINGO';
                    daySchedule.appendChild(dayHeader);
                    const shiftsTable = document.createElement('div');
                    shiftsTable.className = 'shifts-table';
                    // header das colunas
                    const headerRow = document.createElement('div');
                    headerRow.className = 'table-header-row';
                    headerRow.innerHTML = '<div>Turno</div><div>Horário</div><div>Agente</div>';
                    shiftsTable.appendChild(headerRow);
                    (scheduleData[weekKey][day]||[]).forEach((row, idx)=>{
                        const cell = document.createElement('div');
                        cell.className = 'shift-cell';
                        cell.dataset.week = weekKey;
                        cell.dataset.day = day;
                        cell.dataset.shiftIndex = idx;
                        const shiftLabel = document.createElement('div');
                        shiftLabel.className = 'shift-label';
                        shiftLabel.textContent = row.shift;
                        const shiftTime = document.createElement('div');
                        shiftTime.className = 'shift-time';
                        shiftTime.textContent = `${row.entry}–${row.exit}`;
                        const agentAssignment = document.createElement('div');
                        if (!row.agent) {
                            agentAssignment.className = 'agent-assignment empty conflict';
                            agentAssignment.innerHTML = '<div>⚠️ SEM AGENTE</div>';
                        } else {
                            agentAssignment.className = 'agent-assignment';
                            const ag = AGENTS.find(a=>a.name===row.agent);
                            agentAssignment.innerHTML = `<div>✓ ${row.agent}</div>`;
                            if (ag) {
                                const info = document.createElement('div');
                                info.className = 'agent-info';
                                info.textContent = `Sex: ${ag.fridayStart}–${ag.fridayExit}`;
                                agentAssignment.appendChild(info);
                            }
                        }
                        cell.appendChild(shiftLabel);
                        cell.appendChild(shiftTime);
                        cell.appendChild(agentAssignment);
                        cell.addEventListener('click', () => openManualEdit(cell));
                        shiftsTable.appendChild(cell);
                    });
                    daySchedule.appendChild(shiftsTable);
                    daysContainer.appendChild(daySchedule);
                });
                weekDiv.appendChild(daysContainer);
                container.appendChild(weekDiv);
            });
            applyFilter();
            updateGenerationInfo();
        }
        function applyFilter() {
            const term = document.getElementById('filterInput').value.toLowerCase();
            document.querySelectorAll('.shift-cell').forEach(row => {
                const text = row.textContent.toLowerCase();
                if (term && !text.includes(term)) row.classList.add('filter-hidden');
                else row.classList.remove('filter-hidden');
            });
        }
        function updateGenerationInfo() {
            const div = document.getElementById('generationInfo');
            if (lastGenMonth) {
                const d = new Date(lastGenMonth);
                div.textContent = `✓ Gerada em ${d.toLocaleString()} | 📌 Regras: Folga alternada (Sáb/Dom) + 11h min descanso`;
            } else {
                div.textContent = 'Clique em "Gerar Escala" para começar';
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
