# toeic-answer-sheet[ToeicAnw.html](https://github.com/user-attachments/files/23956263/ToeicAnw.html)
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TOEIC Pro: Answer Sheet & History</title>
    <style>
        :root {
            --primary: #0056D2;
            --success: #28a745;
            --bg: #F2F5F9;
            --text: #333;
            --white: #ffffff;
            --danger: #dc3545;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg);
            margin: 0;
            color: var(--text);
            user-select: none;
            padding-bottom: 40px;
        }

        .hidden { display: none !important; }
        .screen { min-height: 100vh; width: 100%; }

        /* --- HOME SCREEN --- */
        .home-header {
            background: linear-gradient(135deg, #0056D2, #0044A5);
            color: white;
            padding: 50px 20px 40px;
            border-bottom-left-radius: 30px;
            border-bottom-right-radius: 30px;
            text-align: center;
            box-shadow: 0 10px 20px rgba(0,86,210,0.2);
        }
        
        .menu-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            max-width: 500px;
            margin: -30px auto 20px;
            padding: 0 20px;
        }

        .menu-card {
            background: white;
            border-radius: 20px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
            cursor: pointer;
            transition: transform 0.1s;
            border: 2px solid transparent;
        }
        .menu-card:active { transform: scale(0.96); }
        .menu-card.primary { border-color: var(--primary); background: #fff; }
        
        .icon-large { font-size: 2.5rem; margin-bottom: 10px; display: block; }
        .menu-title { font-weight: 800; font-size: 1.1rem; color: var(--primary); }
        .menu-desc { font-size: 0.8rem; color: #888; margin-top: 5px; }

        /* --- HISTORY LIST --- */
        .history-container {
            max-width: 600px;
            margin: 20px auto;
            padding: 0 20px;
        }
        .section-header { margin: 20px 0 10px; font-weight: bold; color: #555; display: flex; justify-content: space-between; align-items: center; }
        
        .history-item {
            background: white;
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.04);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .h-score { font-size: 1.4rem; font-weight: 800; color: var(--primary); }
        .h-date { font-size: 0.75rem; color: #999; margin-bottom: 4px; }
        .h-details { font-size: 0.85rem; color: #555; }
        
        .review-btn {
            background: var(--active);
            color: var(--primary);
            border: 1px solid var(--primary);
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 0.8rem;
            cursor: pointer;
            font-weight: 600;
        }
        .review-btn:hover { background: var(--primary); color: white; }

        /* --- TEST SCREEN --- */
        .header {
            position: fixed; top: 0; left: 0; right: 0;
            background: rgba(255,255,255,0.95); backdrop-filter: blur(10px);
            padding: 15px 20px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            z-index: 1000; display: flex; justify-content: space-between; align-items: center;
        }
        .timer { font-size: 1.4rem; font-weight: 800; color: var(--primary); font-variant-numeric: tabular-nums; }
        
        .nav-scroller {
            position: fixed; top: 60px; left: 0; right: 0;
            background: var(--bg); padding: 10px 0;
            z-index: 900;
        }
        .part-nav { display: flex; overflow-x: auto; gap: 8px; padding: 0 20px; scrollbar-width: none; }
        .part-btn { background: white; border: 1px solid #ddd; padding: 6px 14px; border-radius: 20px; font-size: 0.85rem; white-space: nowrap; }
        .part-btn.active { background: var(--primary); color: white; border-color: var(--primary); }

        .questions-wrapper { max-width: 600px; margin: 120px auto 100px; padding: 0 15px; }

        /* Questions */
        .section-title { font-weight: bold; margin: 30px 0 15px; color: var(--primary); border-bottom: 2px solid #ddd; padding-bottom: 5px; }
        
        .q-row {
            background: white; padding: 12px 15px; margin-bottom: 10px; border-radius: 12px;
            display: flex; align-items: center; justify-content: space-between;
        }
        .q-num { width: 30px; font-weight: 700; color: #555; }
        .choices { display: flex; gap: 10px; }
        
        .bubble {
            width: 36px; height: 36px; border-radius: 50%; border: 2px solid #e0e0e0;
            display: flex; align-items: center; justify-content: center;
            font-weight: 600; color: #bbb; transition: 0.2s; font-size: 0.9rem;
        }
        
        /* Interactive Inputs */
        input[type="radio"] { display: none; }
        input[type="radio"]:checked + .bubble {
            background: var(--primary); border-color: var(--primary); color: white; transform: scale(1.1);
        }
        
        /* Checkbox for Grading */
        .grade-check { display: none; margin-right: 15px; }
        .check-box { width: 22px; height: 22px; border: 2px solid #ddd; border-radius: 6px; display: flex; align-items: center; justify-content: center; color: white; }
        .grade-check input { display: none; }
        .grade-check input:checked + .check-box { background: var(--success); border-color: var(--success); }
        .grade-check input:checked + .check-box::after { content: '✓'; font-size: 14px; }

        /* --- Footer --- */
        .footer {
            position: fixed; bottom: 0; left: 0; right: 0;
            background: white; padding: 15px; border-top: 1px solid #eee;
            text-align: center; z-index: 1000; box-shadow: 0 -5px 20px rgba(0,0,0,0.05);
        }
        .btn-main {
            background: var(--primary); color: white; border: none; padding: 14px 0;
            border-radius: 30px; font-size: 1rem; font-weight: bold; width: 100%; max-width: 400px;
            cursor: pointer; box-shadow: 0 4px 15px rgba(0,86,210,0.3);
        }
        .btn-calc { background: var(--success); box-shadow: 0 4px 15px rgba(40,167,69,0.3); }

        /* --- MODES --- */
        /* Grading Mode */
        .mode-grading .grade-check { display: block; }
        .mode-grading input[type="radio"] { pointer-events: none; } /* Lock radio */
        .mode-grading .q-row.correct { border: 2px solid var(--success); background: #f0fff4; }

        /* Review Mode (Read Only) */
        .mode-review .header { background: #333; color: white; }
        .mode-review .timer { display: none; }
        .mode-review .grade-check { display: block; pointer-events: none; } /* Show marks but lock */
        .mode-review .q-row.correct { border: 2px solid var(--success); background: #f0fff4; }
        .mode-review input[type="radio"] { pointer-events: none; }
        
        /* Modal */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); z-index: 2000; align-items: center; justify-content: center; backdrop-filter: blur(5px); }
        .modal-card { background: white; padding: 30px; border-radius: 20px; width: 85%; max-width: 320px; text-align: center; }

    </style>
</head>
<body>

    <div id="screen-home" class="screen">
        <div class="home-header">
            <h1 style="margin:0; font-size: 2rem;">TOEIC Master</h1>
            <p style="opacity:0.8; margin-top:5px;">Your Personal Answer Sheet</p>
        </div>

        <div class="menu-grid">
            <div class="menu-card primary" onclick="startNewTest()">
                <span class="icon-large">✏️</span>
                <div class="menu-title">Start Test</div>
                <div class="menu-desc">New answer sheet</div>
            </div>
            <div class="menu-card" onclick="scrollToHistory()">
                <span class="icon-large">📊</span>
                <div class="menu-title">History</div>
                <div class="menu-desc">View past scores</div>
            </div>
        </div>

        <div class="history-container" id="historySection">
            <div class="section-header">
                <span>Recent Activity</span>
                <span style="font-size:0.8rem; color:var(--danger); cursor:pointer;" onclick="clearHistory()">Clear All</span>
            </div>
            <div id="historyList">
                </div>
        </div>
    </div>

    <div id="screen-test" class="screen hidden">
        <div class="header">
            <div id="headerTitle" style="font-weight:bold;">Answer Sheet</div>
            <div style="display:flex; align-items:center; gap:15px;">
                <button onclick="exitTest()" style="background:none; border:none; color:#666; cursor:pointer;">Exit</button>
                <div class="timer" id="timer">02:00:00</div>
            </div>
        </div>

        <div class="nav-scroller">
            <div class="part-nav" id="partNav"></div>
        </div>

        <div class="questions-wrapper" id="questionsArea"></div>

        <div class="footer" id="testFooter">
            <button id="btnFinish" class="btn-main" onclick="enterGrading()">Finish & Check</button>
            <button id="btnSave" class="btn-main btn-calc hidden" onclick="calculateAndSave()">Confirm Score</button>
        </div>
    </div>

    <div id="modalResult" class="modal">
        <div class="modal-card">
            <h2 style="margin:0; color:#555;">Test Result</h2>
            <div style="font-size:4rem; font-weight:800; color:var(--primary); margin:10px 0;" id="modalScore">0</div>
            <p style="color:#888;">Estimated Score</p>
            
            <div style="background:#f5f5f5; padding:15px; border-radius:10px; margin-bottom:20px;">
                <div style="display:flex; justify-content:space-between; margin-bottom:5px;">
                    <span>Listening</span> <b id="modalL">0</b>
                </div>
                <div style="display:flex; justify-content:space-between;">
                    <span>Reading</span> <b id="modalR">0</b>
                </div>
            </div>
            <button class="btn-main" onclick="closeModal()">Back to Home</button>
        </div>
    </div>

    <script>
        // --- DATA ---
        const PARTS = [
            { id: 1, start: 1, end: 6, name: 'Photographs' },
            { id: 2, start: 7, end: 31, name: 'Q&A' },
            { id: 3, start: 32, end: 70, name: 'Conversations' },
            { id: 4, start: 71, end: 100, name: 'Talks' },
            { id: 5, start: 101, end: 130, name: 'Incomplete Sentences' },
            { id: 6, start: 131, end: 146, name: 'Text Completion' },
            { id: 7, start: 147, end: 200, name: 'Reading Comprehension' }
        ];

        let timerInterval;
        let timeLeft = 7200;
        let isReviewMode = false;

        // --- INIT ---
        window.onload = function() {
            renderHistory();
            renderTestStructure();
        };

        // --- NAVIGATION ---
        function startNewTest() {
            isReviewMode = false;
            resetUI();
            document.getElementById('screen-home').classList.add('hidden');
            document.getElementById('screen-test').classList.remove('hidden');
            document.body.className = ''; // Reset classes
            
            // Start Timer
            timeLeft = 7200;
            updateTimerDisplay();
            timerInterval = setInterval(() => {
                if(timeLeft <= 0) { enterGrading(); return; }
                timeLeft--;
                updateTimerDisplay();
            }, 1000);
            
            showPart(0);
        }

        function reviewTest(index) {
            const history = JSON.parse(localStorage.getItem('toeic_history') || '[]');
            const data = history[index];
            if(!data) return;

            isReviewMode = true;
            document.getElementById('screen-home').classList.add('hidden');
            document.getElementById('screen-test').classList.remove('hidden');
            
            // Set Review UI
            document.body.className = 'mode-review'; // Locks inputs
            document.getElementById('testFooter').classList.add('hidden'); // Hide buttons
            document.getElementById('headerTitle').innerText = `Review: ${data.date}`;

            // Restore Answers
            restoreAnswers(data.answers, data.checks);
            
            showPart(0);
        }

        function exitTest() {
            if(!isReviewMode && !confirm("Exit? Progress will be lost.")) return;
            clearInterval(timerInterval);
            document.getElementById('screen-test').classList.add('hidden');
            document.getElementById('screen-home').classList.remove('hidden');
            renderHistory();
        }

        function scrollToHistory() {
            document.getElementById('historySection').scrollIntoView({behavior: 'smooth'});
        }

        // --- RENDERING ---
        function renderTestStructure() {
            const nav = document.getElementById('partNav');
            const area = document.getElementById('questionsArea');
            nav.innerHTML = ''; area.innerHTML = '';

            PARTS.forEach((p, i) => {
                // Nav Button
                const btn = document.createElement('button');
                btn.className = `part-btn ${i===0?'active':''}`;
                btn.innerText = `Part ${p.id}`;
                btn.onclick = () => showPart(i);
                nav.appendChild(btn);

                // Section
                const section = document.createElement('div');
                section.id = `sec-${i}`;
                section.className = i===0 ? '' : 'hidden';
                
                const title = document.createElement('div');
                title.className = 'section-title';
                title.innerText = `Part ${p.id}: ${p.name}`;
                section.appendChild(title);

                // Questions
                for(let q=p.start; q<=p.end; q++) {
                    section.appendChild(createQuestionRow(q));
                }
                area.appendChild(section);
            });
        }

        function createQuestionRow(num) {
            const row = document.createElement('div');
            row.className = 'q-row';
            row.id = `row-${num}`;
            
            // HTML Structure
            row.innerHTML = `
                <label class="grade-check">
                    <input type="checkbox" id="chk-${num}" onchange="toggleCorrect(${num})">
                    <div class="check-box"></div>
                </label>
                <div class="q-num">${num}</div>
                <div class="choices">
                    ${['A','B','C','D'].map(val => `
                        <label>
                            <input type="radio" name="q${num}" value="${val}">
                            <div class="bubble">${val}</div>
                        </label>
                    `).join('')}
                </div>
            `;
            return row;
        }

        function showPart(index) {
            document.querySelectorAll('.part-btn').forEach((b, i) => {
                if(i===index) b.classList.add('active');
                else b.classList.remove('active');
            });
            PARTS.forEach((_, i) => {
                const el = document.getElementById(`sec-${i}`);
                if(i===index) el.classList.remove('hidden');
                else el.classList.add('hidden');
            });
            window.scrollTo({top:0});
        }

        // --- LOGIC ---
        function toggleCorrect(num) {
            const row = document.getElementById(`row-${num}`);
            if(document.getElementById(`chk-${num}`).checked) row.classList.add('correct');
            else row.classList.remove('correct');
        }

        function enterGrading() {
            clearInterval(timerInterval);
            document.body.classList.add('mode-grading');
            document.getElementById('timer').innerText = "GRADING";
            document.getElementById('timer').style.color = "#333";
            document.getElementById('btnFinish').classList.add('hidden');
            document.getElementById('btnSave').classList.remove('hidden');
            
            alert("หมดเวลา! \nเข้าสู่โหมดตรวจคำตอบ\nโปรดติ๊ก ✓ หน้าข้อที่ถูก");
            showPart(0);
        }

        function calculateAndSave() {
            let userAnswers = {};
            let userChecks = {};
            let l_raw = 0, r_raw = 0;

            // Collect Data
            for(let i=1; i<=200; i++) {
                // Get Selected Choice
                const radios = document.getElementsByName(`q${i}`);
                for(let r of radios) if(r.checked) userAnswers[`q${i}`] = r.value;
                
                // Get Check Status
                const isCorrect = document.getElementById(`chk-${i}`).checked;
                if(isCorrect) {
                    userChecks[`chk-${i}`] = true;
                    if(i<=100) l_raw++; else r_raw++;
                }
            }

            // Calc Score
            const l_score = convertScore(l_raw, 'L');
            const r_score = convertScore(r_raw, 'R');
            const total = l_score + r_score;

            // Save to LocalStorage
            const history = JSON.parse(localStorage.getItem('toeic_history') || '[]');
            history.unshift({
                date: new Date().toLocaleString('th-TH'),
                score: total,
                l: l_score,
                r: r_score,
                answers: userAnswers,
                checks: userChecks
            });
            localStorage.setItem('toeic_history', JSON.stringify(history));

            // Show Result
            document.getElementById('modalScore').innerText = total;
            document.getElementById('modalL').innerText = l_score;
            document.getElementById('modalR').innerText = r_score;
            document.getElementById('modalResult').style.display = 'flex';
        }

        // --- UTILS ---
        function resetUI() {
            document.querySelectorAll('input').forEach(i => { i.checked = false; i.disabled = false; });
            document.querySelectorAll('.q-row').forEach(r => r.classList.remove('correct'));
            document.getElementById('testFooter').classList.remove('hidden');
            document.getElementById('btnFinish').classList.remove('hidden');
            document.getElementById('btnSave').classList.add('hidden');
            document.getElementById('headerTitle').innerText = "Answer Sheet";
        }

        function restoreAnswers(answers, checks) {
            // Restore Radio selections
            for(let key in answers) {
                const els = document.getElementsByName(key);
                for(let el of els) if(el.value === answers[key]) el.checked = true;
            }
            // Restore Checks
            for(let key in checks) {
                if(checks[key]) {
                    const el = document.getElementById(key);
                    if(el) {
                        el.checked = true;
                        // Trigger visual update
                        const num = key.split('-')[1];
                        document.getElementById(`row-${num}`).classList.add('correct');
                    }
                }
            }
        }

        function renderHistory() {
            const list = document.getElementById('historyList');
            const history = JSON.parse(localStorage.getItem('toeic_history') || '[]');
            
            if(history.length === 0) {
                list.innerHTML = `<div style="text-align:center; padding:20px; color:#aaa;">No history yet</div>`;
                return;
            }

            list.innerHTML = history.map((item, index) => `
                <div class="history-item">
                    <div>
                        <div class="h-date">${item.date}</div>
                        <div class="h-score">${item.score}</div>
                        <div class="h-details">Listening: ${item.l} | Reading: ${item.r}</div>
                    </div>
                    <button class="review-btn" onclick="reviewTest(${index})">📄 Review</button>
                </div>
            `).join('');
        }

        function closeModal() {
            document.getElementById('modalResult').style.display = 'none';
            exitTest();
        }

        function clearHistory() {
            if(confirm("Delete all history?")) {
                localStorage.removeItem('toeic_history');
                renderHistory();
            }
        }

        function updateTimerDisplay() {
            const h = Math.floor(timeLeft/3600);
            const m = Math.floor((timeLeft%3600)/60);
            const s = timeLeft%60;
            const el = document.getElementById('timer');
            el.innerText = `${h}:${m<10?'0'+m:m}:${s<10?'0'+s:s}`;
            el.style.color = timeLeft < 300 ? '#dc3545' : 'var(--primary)';
        }

        function convertScore(raw, type) {
            if(raw===0) return 5;
            if(type==='L') {
                if(raw>=90) return 495;
                if(raw>=75) return 400 + (raw-75)*6;
                if(raw>=50) return 250 + (raw-50)*6;
                return 5 + raw*5;
            } else {
                if(raw>=90) return 495;
                if(raw>=80) return 395 + (raw-80)*10;
                if(raw>=50) return 200 + (raw-50)*6;
                return 5 + raw*4;
            }
        }
    </script>
</body>
</html>
