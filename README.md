<!DOCTYPE html>
<html>
<head>
<title>Secure Meeting Report System</title>
<meta charset="UTF-8">

<style>
/* Animated Background */
body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(-45deg, #1e3c72, #2a5298, #00c6ff, #0072ff);
    background-size: 400% 400%;
    animation: gradientBG 12s ease infinite;
    color: #fff;
}

@keyframes gradientBG {
    0% {background-position: 0% 50%;}
    50% {background-position: 100% 50%;}
    100% {background-position: 0% 50%;}
}

.hidden { display: none; }

.login-box {
    width: 350px;
    margin: 120px auto;
    background: rgba(0,0,0,0.85);
    padding: 30px;
    border-radius: 15px;
    text-align: center;
}

.container {
    width: 85%;
    margin: 40px auto;
    background: rgba(0,0,0,0.85);
    padding: 30px;
    border-radius: 15px;
}

.logo {
    text-align: center;
    margin-bottom: 15px;
}

.logo-img {
    width: 120px;
    margin-bottom: 10px;
}

.moving-text {
    width: 100%;
    overflow: hidden;
    white-space: nowrap;
    background: black;
    color: #00ffff;
    padding: 8px 0;
    font-weight: bold;
    animation: scrollText 12s linear infinite;
}

@keyframes scrollText {
    0% { transform: translateX(100%); }
    100% { transform: translateX(-100%); }
}

input, textarea {
    width: 100%;
    padding: 8px;
    margin: 6px 0 12px 0;
    border-radius: 6px;
    border: none;
}

button {
    padding: 8px 15px;
    border-radius: 6px;
    border: none;
    background: #00c6ff;
    font-weight: bold;
    cursor: pointer;
    margin-top: 5px;
}

button:hover { background: #fff; }

table {
    background: white;
    color: black;
    border-collapse: collapse;
    width: 100%;
}

th, td {
    padding: 6px;
    border: 1px solid #ccc;
}

h2 {
    border-bottom: 2px solid #00c6ff;
    padding-bottom: 4px;
    margin-top: 25px;
}

@media print {
    button { display: none; }
    body { background: white; color: black; }
    .container { background: white; }
}
</style>
</head>

<body>

<!-- LOGIN PAGE -->
<div id="loginPage" class="login-box">
    <h2>Institution Login</h2>
    <input type="text" id="username" placeholder="Username">
    <input type="password" id="password" placeholder="Password">
    <button onclick="login()">Login</button>
    <p id="loginError" style="color:red;"></p>
</div>

<!-- REPORT PAGE -->
<div id="reportPage" class="container hidden">

    <div class="moving-text">
        CONFIDENTIAL • AUTHORIZED ACCESS ONLY • INSTITUTION NAME • OFFICIAL REPORT •
    </div>

    <div class="logo">
        <img src="logo.png" alt="Institution Logo" class="logo-img">
        <h1>You Are Free!!!!
start....</h1>
    </div>

    <button onclick="logout()" style="float:right;">Logout</button>

    <!-- 1 Meeting Details -->
    <h2>1. Meeting Details 
        <button onclick="unlockSection('meetingSection')">Unlock</button>
    </h2>
    <div id="meetingSection" class="hidden">
        <input type="date">
        <input type="time">
        <input type="text" placeholder="Location">
        <input type="text" placeholder="Chairperson">
        <input type="text" placeholder="Secretary">
    </div>

    <!-- 2 Attendance -->
    <h2>2. Attendance 
        <button onclick="unlockSection('attendanceSection')">Unlock</button>
    </h2>
    <div id="attendanceSection" class="hidden">
        <textarea placeholder="Attendees"></textarea>
        <textarea placeholder="Absentees"></textarea>
        <textarea placeholder="Apologies"></textarea>
    </div>

    <!-- 3 Agenda -->
    <h2>3. Agenda 
        <button onclick="unlockSection('agendaSection')">Unlock</button>
    </h2>
    <div id="agendaSection" class="hidden">
        <textarea rows="4"></textarea>
    </div>

    <!-- 4 Discussion -->
    <h2>4. Discussion 
        <button onclick="unlockSection('discussionSection')">Unlock</button>
    </h2>
    <div id="discussionSection" class="hidden">
        <textarea rows="5"></textarea>
    </div>

    <!-- 5 Decisions -->
    <h2>5. Decisions 
        <button onclick="unlockSection('decisionSection')">Unlock</button>
    </h2>
    <div id="decisionSection" class="hidden">
        <textarea rows="4"></textarea>
    </div>

    <!-- 6 Action Items -->
    <h2>6. Action Items 
        <button onclick="unlockSection('actionSection')">Unlock</button>
    </h2>
    <div id="actionSection" class="hidden">
        <table>
            <thead>
                <tr>
                    <th>#</th>
                    <th>Task</th>
                    <th>Assigned To</th>
                    <th>Deadline</th>
                    <th>Remove</th>
                </tr>
            </thead>
            <tbody id="taskBody"></tbody>
        </table>
        <button onclick="addTask()">Add Task</button>
    </div>

    <br>
    <button onclick="window.print()">Save as PDF</button>

</div>

<script>
/* LOGIN CREDENTIALS */
const USERNAME = "admin";
const PASSWORD = "1234";

/* SAME PASSWORD FOR ALL SECTIONS */
const SECTION_PASSWORD = "secure2026";

function login() {
    let user = document.getElementById("username").value;
    let pass = document.getElementById("password").value;

    if (user === USERNAME && pass === PASSWORD) {
        document.getElementById("loginPage").classList.add("hidden");
        document.getElementById("reportPage").classList.remove("hidden");
    } else {
        document.getElementById("loginError").innerText = "Invalid login details!";
    }
}

function logout() {
    location.reload();
}

function unlockSection(sectionId) {
    let pass = prompt("Enter Section Password:");
    if (pass === SECTION_PASSWORD) {
        document.getElementById(sectionId).classList.remove("hidden");
    } else {
        alert("Incorrect password!");
    }
}

function addTask() {
    let table = document.getElementById("taskBody");
    let rowCount = table.rows.length + 1;
    let row = table.insertRow();

    row.innerHTML = `
        <td>${rowCount}</td>
        <td><input type="text"></td>
        <td><input type="text"></td>
        <td><input type="date"></td>
        <td><button onclick="removeTask(this)">X</button></td>
    `;
}

function removeTask(btn) {
    btn.parentElement.parentElement.remove();
    renumberTasks();
}

function renumberTasks() {
    let rows = document.querySelectorAll("#taskBody tr");
    rows.forEach((row, index) => {
        row.cells[0].innerText = index + 1;
    });
}
</script>

</body>
</html>
