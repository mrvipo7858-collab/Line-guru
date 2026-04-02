<!DOCTYPE html>
<html lang="gu">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Line Guru Pro - Premium Cricket Contest</title>
    <!-- Confetti Library for Celebration -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
    <style>
        :root {
            --primary: #0f172a;
            --secondary: #1e293b;
            --accent: #facc15;
            --success: #22c55e;
            --danger: #ef4444;
            --text-light: #f8fafc;
        }

        body { font-family: 'Inter', sans-serif; background-color: #0f172a; margin: 0; padding-bottom: 100px; color: var(--text-light); }
        
        /* Header & Logo */
        .header { background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%); padding: 30px 20px; text-align: center; border-bottom: 2px solid var(--accent); position: relative; }
        .logo { font-size: 36px; font-weight: 900; color: var(--accent); font-style: italic; letter-spacing: 3px; text-shadow: 0 0 15px rgba(250, 204, 21, 0.4); }
        .round-tag { background: var(--accent); color: #000; padding: 4px 12px; border-radius: 20px; font-size: 12px; font-weight: 800; display: inline-block; margin-top: 10px; }

        .admin-login-btn { position: absolute; top: 15px; right: 15px; background: rgba(255,255,255,0.1); color: white; border: 1px solid rgba(255,255,255,0.3); padding: 5px 10px; border-radius: 6px; cursor: pointer; font-size: 11px; }

        /* Progress Bar */
        .progress-section { background: var(--secondary); margin: 15px; padding: 15px; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.3); }
        .progress-label { display: flex; justify-content: space-between; font-size: 13px; margin-bottom: 8px; font-weight: bold; }
        .bar-bg { background: #334155; height: 10px; border-radius: 10px; overflow: hidden; }
        .bar-fill { background: var(--accent); height: 100%; width: 0%; transition: 0.5s; }

        /* Tab System */
        .tabs { display: flex; margin: 15px; gap: 10px; }
        .tab { flex: 1; padding: 12px; text-align: center; background: var(--secondary); border-radius: 8px; cursor: pointer; font-weight: bold; border: 1px solid #334155; }
        .tab.active { background: var(--accent); color: #000; border-color: var(--accent); }

        .container { padding: 0 15px; max-width: 600px; margin: auto; }
        .content-section { display: none; }
        .content-section.active { display: block; }

        /* Premium Team Card */
        .team-card { background: var(--secondary); margin-bottom: 15px; padding: 18px; border-radius: 15px; display: flex; align-items: center; justify-content: space-between; position: relative; border: 1px solid #334155; transition: 0.3s; }
        .is-top { border: 2px solid #3b82f6; box-shadow: 0 0 15px rgba(59, 130, 246, 0.3); }
        .is-winner { border: 2px solid var(--accent); background: linear-gradient(145deg, #1e293b, #2d3748); transform: scale(1.02); }

        .team-id { background: #334155; color: var(--accent); width: 35px; height: 35px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-weight: 800; margin-right: 15px; }
        .runs { font-size: 26px; font-weight: 900; color: var(--accent); margin-right: 15px; }

        .btn-join { background: var(--success); color: white; border: none; padding: 12px 20px; border-radius: 10px; font-weight: bold; cursor: pointer; box-shadow: 0 4px 10px rgba(34, 197, 94, 0.3); }
        .btn-full { background: #475569; color: #94a3b8; cursor: not-allowed; }

        /* Modal Customization */
        .modal { display: none; position: fixed; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.9); backdrop-filter: blur(8px); justify-content: center; align-items: center; z-index: 9999; }
        .modal-body { background: white; color: #000; padding: 30px; border-radius: 25px; width: 85%; max-width: 350px; text-align: center; }
        .copy-box { background: #f1f5f9; padding: 10px; border-radius: 10px; margin: 15px 0; border: 2px dashed #cbd5e1; display: flex; justify-content: space-between; align-items: center; }

        /* Admin Controls */
        #admin-panel { background: #fff; color: #000; padding: 20px; margin: 15px; border-radius: 15px; display: none; }
        .admin-box { border: 1px solid #eee; padding: 15px; margin-bottom: 15px; border-radius: 10px; }
        input, select { width: 100%; padding: 12px; margin-bottom: 10px; border-radius: 8px; border: 1px solid #ccc; box-sizing: border-box; font-weight: bold; }

        .floating-wa { position: fixed; bottom: 20px; right: 20px; background: var(--success); color: white; padding: 15px 25px; border-radius: 50px; text-decoration: none; font-weight: bold; box-shadow: 0 10px 20px rgba(0,0,0,0.4); z-index: 100; border: 2px solid white; }
    </style>
</head>
<body>

<div class="header">
    <button class="admin-login-btn" onclick="adminLogin()">ADMIN</button>
    <div class="logo">LINE GURU</div>
    <div id="round-tag" class="round-tag">ROUND 1</div>
    <div id="prize-tag" style="margin-top:15px; font-weight:bold; color: #94a3b8; font-size: 14px;">
        PRIZE <span style="color:var(--accent)">₹1300</span> | ENTRY <span style="color:var(--accent)">₹100</span>
    </div>
</div>

<div class="progress-section">
    <div class="progress-label">
        <span>Tickets Sold</span>
        <span id="sold-count">0/15</span>
    </div>
    <div class="bar-bg">
        <div id="bar-fill" class="bar-fill"></div>
    </div>
</div>

<div class="tabs">
    <div class="tab active" onclick="switchTab('teams', this)">TEAMS</div>
    <div class="tab" onclick="switchTab('entries', this)">MY ENTRIES</div>
</div>

<div class="container">
    <!-- Teams Section -->
    <div id="teams-section" class="content-section active">
        <div id="team-list"></div>
    </div>

    <!-- Entries Section -->
    <div id="entries-section" class="content-section">
        <div id="entry-list"></div>
    </div>

    <!-- Admin Panel -->
    <div id="admin-panel">
        <h2 style="margin:0 0 20px 0; border-bottom:2px solid #000;">Admin Panel Pro</h2>
        
        <div class="admin-box">
            <h4>1. મેચ અને રકમ સેટિંગ્સ</h4>
            <input type="text" id="tA" placeholder="Team A Name">
            <input type="text" id="tB" placeholder="Team B Name">
            <input type="number" id="tP" placeholder="Prize Amount">
            <input type="number" id="tE" placeholder="Entry Fee">
            <button onclick="saveMatchSettings()" style="width:100%; background:#3b82f6; color:white; padding:12px; border:none; border-radius:8px; font-weight:bold;">Save Match Settings</button>
        </div>

        <div class="admin-box">
            <h4>2. લાઈવ રન અપડેટ</h4>
            <button onclick="toggleMatch()" id="liveBtn" style="width:100%; padding:12px; border:none; border-radius:8px; font-weight:bold; color:white; margin-bottom:10px;"></button>
            <div id="admin-runs" style="display:grid; grid-template-columns:1fr 1fr; gap:10px;">
                <!-- Run inputs generated here -->
            </div>
            <button onclick="updateRuns()" style="width:100%; background:#22c55e; color:white; padding:12px; border:none; border-radius:8px; font-weight:bold; margin-top:10px;">Update Runs Now</button>
        </div>

        <div class="admin-box">
            <h4>3. રાઉન્ડ અને વિજેતા</h4>
            <select id="winSel"><option value="">-- Select Winner --</option></select>
            <button onclick="setWinner()" style="width:100%; background:var(--accent); color:#000; padding:12px; border:none; border-radius:8px; font-weight:bold;">Declare Winner 🏆</button>
            <hr>
            <button onclick="startNewRound()" style="width:100%; background:#ef4444; color:white; padding:12px; border:none; border-radius:8px; font-weight:bold;">Next Round (Clean Entries)</button>
        </div>

        <button onclick="adminLogout()" style="width:100%; padding:10px; background:#64748b; color:white; border:none; border-radius:8px;">Logout</button>
    </div>
</div>

<a href="https://wa.me/917567470149" class="floating-wa" target="_blank">WhatsApp Support</a>

<!-- Premium Payment Modal -->
<div id="payModal" class="modal">
    <div class="modal-body">
        <h2 style="margin:0">Secure Checkout</h2>
        <div id="pay-round-info" style="color:var(--danger); font-weight:800; margin-top:10px;"></div>
        <p id="pay-team" style="font-weight:bold; margin:10px 0;"></p>
        
        <div class="copy-box">
            <span id="upi-id" style="font-weight:bold; color:#1e293b;">mrvipo7858@okicici</span>
            <button onclick="copyUPI()" style="background:var(--primary); color:white; border:none; padding:5px 10px; border-radius:5px; font-size:10px; cursor:pointer;">COPY</button>
        </div>
        
        <img id="qr-img" src="" style="width:200px; height:200px; border:4px solid #f1f5f9; border-radius:15px;" alt="QR">
        
        <p style="font-size:14px; color:#64748b;">Pay <b>₹<span id="pay-amt"></span></b> and click below</p>
        
        <button onclick="confirmPayment()" style="width:100%; background:var(--success); color:white; padding:15px; border:none; border-radius:12px; font-weight:900; font-size:16px; cursor:pointer; box-shadow: 0 10px 20px rgba(34,197,94,0.3);">✅ I HAVE PAID</button>
        <button onclick="closeModal()" style="margin-top:15px; color:#94a3b8; background:none; border:none; cursor:pointer;">Cancel</button>
    </div>
</div>

<script>
    // State Management
    let state = {
        round: JSON.parse(localStorage.getItem('lg_pro_round')) || 1,
        entries: JSON.parse(localStorage.getItem('lg_pro_entries')) || [],
        runs: JSON.parse(localStorage.getItem('lg_pro_runs')) || {a1:0, a2:0, a3:0, b1:0, b2:0, b3:0},
        names: JSON.parse(localStorage.getItem('lg_pro_names')) || {a: 'GT', b: 'PBKS'},
        money: JSON.parse(localStorage.getItem('lg_pro_money')) || {prize: 1300, entry: 100},
        isLive: JSON.parse(localStorage.getItem('lg_pro_live')) || false,
        winner: JSON.parse(localStorage.getItem('lg_pro_winner')) || null,
        isAdmin: false
    };

    const ADMIN_PASS = "BHAVU9686";
    const PAY_WA = "918690447858";

    // Initialize
    window.onload = () => {
        render();
        initAdminInputs();
    };

    function render() {
        updateHeaders();
        renderTeams();
        renderEntries();
        updateProgressBar();
        
        // Admin UI
        document.getElementById('admin-panel').style.display = state.isAdmin ? 'block' : 'none';
        const liveBtn = document.getElementById('liveBtn');
        liveBtn.innerText = state.isLive ? "STOP LIVE MATCH" : "START LIVE MATCH";
        liveBtn.style.background = state.isLive ? "var(--danger)" : "var(--success)";
    }

    function updateHeaders() {
        document.getElementById('round-tag').innerText = `ROUND ${state.round}`;
        document.getElementById('prize-tag').innerHTML = `PRIZE <span style="color:var(--accent)">₹${state.money.prize}</span> | ENTRY <span style="color:var(--accent)">₹${state.money.entry}</span>`;
    }

    function renderTeams() {
        const list = document.getElementById('team-list');
        list.innerHTML = "";
        
        const configs = getConfigs();
        const scored = configs.map(c => ({...c, total: parseInt(state.runs[c.keys[0]]) + parseInt(state.runs[c.keys[1]])}));
        const max = Math.max(...scored.map(s => s.total));

        scored.forEach(t => {
            const isSold = state.entries.some(e => e.team === t.name && e.status === 'Verified');
            const isTop = state.isLive && t.total > 0 && t.total === max && !state.winner;
            const isWinner = state.winner == t.id;

            list.innerHTML += `
                <div class="team-card ${isTop ? 'is-top' : ''} ${isWinner ? 'is-winner' : ''}">
                    ${isWinner ? '<div style="position:absolute; top:-10px; left:15px; background:var(--accent); color:#000; padding:2px 10px; border-radius:4px; font-weight:900; font-size:10px;">🏆 WINNER</div>' : ''}
                    ${isTop ? '<div style="position:absolute; top:-10px; left:15px; background:#3b82f6; color:#fff; padding:2px 10px; border-radius:4px; font-weight:900; font-size:10px;">🔥 LEADING</div>' : ''}
                    
                    <div style="display:flex; align-items:center;">
                        <div class="team-id">${t.id}</div>
                        <div>
                            <div style="font-weight:800; font-size:15px;">${t.name}</div>
                            <div style="font-size:10px; color:${isSold ? 'var(--danger)' : 'var(--success)'}; font-weight:bold;">${isSold ? 'SOLD OUT' : 'AVAILABLE'}</div>
                        </div>
                    </div>

                    <div style="display:flex; align-items:center;">
                        ${(state.isLive || isWinner) ? `<div class="runs">${t.total}</div>` : ''}
                        ${!isSold ? `<button class="btn-join" onclick="openJoin('${t.name}')">JOIN</button>` : `<button class="btn-join btn-full" disabled>FULL</button>`}
                    </div>
                </div>
            `;
        });
    }

    function renderEntries() {
        const list = document.getElementById('entry-list');
        list.innerHTML = state.entries.length ? "" : "<p style='text-align:center; color:#64748b;'>No entries yet.</p>";
        state.entries.forEach(e => {
            list.innerHTML += `
                <div class="team-card" style="border-left:4px solid var(--accent)">
                    <div>
                        <div style="font-weight:bold">${e.name}</div>
                        <div style="font-size:12px; color:#94a3b8;">${e.team} | ${e.mobile}</div>
                    </div>
                    <div style="text-align:right">
                        <div style="font-size:10px; font-weight:900; color:${e.status==='Verified'?'var(--success)':'var(--danger)'}">${e.status}</div>
                        ${state.isAdmin ? `
                            <div style="margin-top:5px;">
                                <button onclick="verifyEntry(${e.id})" style="background:var(--success); border:none; color:white; border-radius:4px; padding:4px 8px; cursor:pointer;">V</button>
                                <button onclick="deleteEntry(${e.id})" style="background:var(--danger); border:none; color:white; border-radius:4px; padding:4px 8px; cursor:pointer;">X</button>
                            </div>
                        ` : ''}
                    </div>
                </div>
            `;
        });
    }

    function updateProgressBar() {
        const soldCount = state.entries.filter(e => e.status === 'Verified').length;
        document.getElementById('sold-count').innerText = `${soldCount}/15`;
        document.getElementById('bar-fill').style.width = `${(soldCount / 15) * 100}%`;
    }

    function switchTab(id, el) {
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
        el.classList.add('active');
        document.getElementById(id + '-section').classList.add('active');
    }

    // Logic Functions
    let currentPendingEntry = null;
    function openJoin(teamName) {
        const name = prompt("તમારું નામ:");
        const mobile = prompt("મોબાઈલ નંબર:");
        if(!name || !mobile) return;

        currentPendingEntry = { id: Date.now(), name, mobile, team: teamName, status: 'Checking' };
        
        document.getElementById('pay-round-info').innerText = `CONTEST ROUND ${state.round}`;
        document.getElementById('pay-team').innerText = `Team Selected: ${teamName}`;
        document.getElementById('pay-amt').innerText = state.money.entry;
        
        const upiLink = `upi://pay?pa=mrvipo7858@okicici&pn=Line Guru&am=${state.money.entry}`;
        document.getElementById('qr-img').src = `https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=${encodeURIComponent(upiLink)}`;
        
        document.getElementById('payModal').style.display = 'flex';
    }

    function confirmPayment() {
        state.entries.push(currentPendingEntry);
        save();
        const msg = `*New Contest Entry*%0A*Round:* ${state.round}%0A*Name:* ${currentPendingEntry.name}%0A*Team:* ${currentPendingEntry.team}%0A*Payment:* ₹${state.money.entry} Done ✅`;
        window.open(`https://wa.me/${PAY_WA}?text=${msg}`, '_blank');
        closeModal();
        render();
    }

    function copyUPI() {
        navigator.clipboard.writeText("mrvipo7858@okicici");
        alert("UPI ID Copied!");
    }

    // Admin Functions
    function setWinner() {
        const id = document.getElementById('winSel').value;
        if(!id) return;
        state.winner = id;
        save();
        render();
        // Fire Confetti!
        confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
    }

    function startNewRound() {
        if(!confirm("Are you sure? All entries will be deleted!")) return;
        state.round++;
        state.entries = [];
        state.runs = {a1:0, a2:0, a3:0, b1:0, b2:0, b3:0};
        state.winner = null;
        state.isLive = false;
        save();
        location.reload();
    }

    function saveMatchSettings() {
        state.names.a = document.getElementById('tA').value.toUpperCase();
        state.names.b = document.getElementById('tB').value.toUpperCase();
        state.money.prize = document.getElementById('tP').value;
        state.money.entry = document.getElementById('tE').value;
        save();
        render();
        alert("Settings Saved!");
    }

    function updateRuns() {
        state.runs.a1 = document.getElementById('in-a1').value;
        state.runs.a2 = document.getElementById('in-a2').value;
        state.runs.a3 = document.getElementById('in-a3').value;
        state.runs.b1 = document.getElementById('in-b1').value;
        state.runs.b2 = document.getElementById('in-b2').value;
        state.runs.b3 = document.getElementById('in-b3').value;
        save();
        render();
        alert("Runs Updated!");
    }

    function toggleMatch() {
        state.isLive = !state.isLive;
        save();
        render();
    }

    function verifyEntry(id) {
        state.entries = state.entries.map(e => e.id === id ? {...e, status: 'Verified'} : e);
        save();
        render();
    }

    function deleteEntry(id) {
        state.entries = state.entries.filter(e => e.id !== id);
        save();
        render();
    }

    // Helpers
    function save() {
        localStorage.setItem('lg_pro_round', JSON.stringify(state.round));
        localStorage.setItem('lg_pro_entries', JSON.stringify(state.entries));
        localStorage.setItem('lg_pro_runs', JSON.stringify(state.runs));
        localStorage.setItem('lg_pro_names', JSON.stringify(state.names));
        localStorage.setItem('lg_pro_money', JSON.stringify(state.money));
        localStorage.setItem('lg_pro_live', JSON.stringify(state.isLive));
        localStorage.setItem('lg_pro_winner', JSON.stringify(state.winner));
    }

    function getConfigs() {
        const {a, b} = state.names;
        return [
            { id: 1, name: `${a}1 + ${b}3`, keys: ['a1', 'b3'] },
            { id: 2, name: `${a}1 + ${b}2`, keys: ['a1', 'b2'] },
            { id: 3, name: `${a}1 + ${b}1`, keys: ['a1', 'b1'] },
            { id: 4, name: `${a}2 + ${b}1`, keys: ['a2', 'b1'] },
            { id: 5, name: `${a}2 + ${b}2`, keys: ['a2', 'b2'] },
            { id: 6, name: `${a}2 + ${b}3`, keys: ['a2', 'b3'] },
            { id: 7, name: `${a}3 + ${b}3`, keys: ['a3', 'b3'] },
            { id: 8, name: `${a}3 + ${b}2`, keys: ['a3', 'b2'] },
            { id: 9, name: `${a}3 + ${b}1`, keys: ['a3', 'b1'] },
            { id: 10, name: `${a}1 + ${a}2`, keys: ['a1', 'a2'] },
            { id: 11, name: `${a}2 + ${a}3`, keys: ['a2', 'a3'] },
            { id: 12, name: `${a}3 + ${a}1`, keys: ['a3', 'a1'] },
            { id: 13, name: `${b}1 + ${b}2`, keys: ['b1', 'b2'] },
            { id: 14, name: `${b}2 + ${b}3`, keys: ['b2', 'b3'] },
            { id: 15, name: `${b}3 + ${b}1`, keys: ['b3', 'b1'] }
        ];
    }

    function initAdminInputs() {
        document.getElementById('tA').value = state.names.a;
        document.getElementById('tB').value = state.names.b;
        document.getElementById('tP').value = state.money.prize;
        document.getElementById('tE').value = state.money.entry;
        
        const runDiv = document.getElementById('admin-runs');
        runDiv.innerHTML = `
            <div><small>${state.names.a} 1</small><input type="number" id="in-a1" value="${state.runs.a1}"></div>
            <div><small>${state.names.b} 1</small><input type="number" id="in-b1" value="${state.runs.b1}"></div>
            <div><small>${state.names.a} 2</small><input type="number" id="in-a2" value="${state.runs.a2}"></div>
            <div><small>${state.names.b} 2</small><input type="number" id="in-b2" value="${state.runs.b2}"></div>
            <div><small>${state.names.a} 3</small><input type="number" id="in-a3" value="${state.runs.a3}"></div>
            <div><small>${state.names.b} 3</small><input type="number" id="in-b3" value="${state.runs.b3}"></div>
        `;

        const winSel = document.getElementById('winSel');
        for(let i=1; i<=15; i++) {
            let opt = document.createElement('option');
            opt.value = i; opt.innerText = "Team " + i;
            winSel.appendChild(opt);
        }
    }

    function adminLogin() { if(prompt("Pass:") === ADMIN_PASS) { state.isAdmin = true; render(); } }
    function adminLogout() { state.isAdmin = false; render(); }
    function closeModal() { document.getElementById('payModal').style.display = 'none'; }
</script>
</body>
</html>
