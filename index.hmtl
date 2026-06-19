<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Samsung OQA Dashboard</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial,sans-serif}
body{background:#f4f7fb;color:#111827}
header{background:linear-gradient(135deg,#0f172a,#1e3a8a);color:white;padding:24px;text-align:center}
header h1{font-size:24px}
header p{color:#cbd5e1;margin-top:6px}
nav{display:flex;gap:8px;overflow:auto;background:white;padding:12px;border-bottom:1px solid #ddd}
nav button{border:0;background:#eef2ff;color:#1e3a8a;padding:10px 14px;border-radius:10px;font-weight:bold}
nav button.active{background:#2563eb;color:white}
.container{padding:18px;max-width:1200px;margin:auto}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:14px;margin-bottom:18px}
.card,.panel{background:white;border-radius:16px;padding:18px;box-shadow:0 6px 18px rgba(0,0,0,.08);border:1px solid #e5e7eb}
.card h3{font-size:13px;color:#6b7280;text-transform:uppercase}
.value{font-size:30px;font-weight:900;margin-top:8px}
.green{color:#16a34a}.red{color:#dc2626}.yellow{color:#f59e0b}.blue{color:#2563eb}.purple{color:#7c3aed}
.panel{margin-bottom:18px}
.panel h2{margin-bottom:14px}
.hidden{display:none}
.two{display:grid;grid-template-columns:1fr 1fr;gap:18px}
@media(max-width:800px){.two{grid-template-columns:1fr}header h1{font-size:20px}}
table{width:100%;border-collapse:collapse}
th{background:#f1f5f9;color:#475569}
td,th{padding:12px;border-bottom:1px solid #e5e7eb;text-align:left}
input,select,textarea{width:100%;padding:12px;margin:6px 0 12px;border:1px solid #ddd;border-radius:10px}
textarea{min-height:90px}
.btn{background:#2563eb;color:white;border:0;padding:12px 16px;border-radius:10px;font-weight:bold;margin:4px}
.btn.dark{background:#0f172a}.btn.greenbtn{background:#16a34a}
.status{padding:6px 10px;border-radius:20px;font-size:12px;font-weight:bold}
.done{background:#dcfce7;color:#166534}.hold{background:#fee2e2;color:#991b1b}.open{background:#fef3c7;color:#92400e}.str{background:#ede9fe;color:#5b21b6}
canvas{width:100%;height:260px;border:1px solid #ddd;border-radius:12px}
.report{white-space:pre-wrap;background:#0f172a;color:white;padding:16px;border-radius:12px;margin-top:12px;line-height:1.5}
footer{text-align:center;color:#6b7280;padding:25px}
</style>
</head>

<body>

<header>
<h1>Samsung OQA Operations Dashboard</h1>
<p>Shift Flow • Inventory • Quality • Safety • Maintenance • Reports</p>
</header>

<nav>
<button class="active" onclick="showPage('dashboard',this)">Dashboard</button>
<button onclick="showPage('technician',this)">Technician</button>
<button onclick="showPage('manager',this)">Manager</button>
<button onclick="showPage('inventory',this)">Inventory</button>
<button onclick="showPage('safety',this)">Safety</button>
<button onclick="showPage('email',this)">Email Report</button>
</nav>

<div class="container">

<section id="dashboard" class="page">
<div class="grid">
<div class="card"><h3>Pass Rate</h3><div class="value green" id="passRate">0%</div></div>
<div class="card"><h3>Lots Processed</h3><div class="value blue" id="lotsProcessed">0</div></div>
<div class="card"><h3>Failed Wafers</h3><div class="value red" id="failedTotal">0</div></div>
<div class="card"><h3>Open Holds</h3><div class="value yellow" id="openHolds">0</div></div>
<div class="card"><h3>QARS</h3><div class="value purple" id="qarsTotal">0</div></div>
<div class="card"><h3>Tool Issues</h3><div class="value red" id="toolIssues">0</div></div>
</div>

<div class="two">
<div class="panel">
<h2>Production Goal vs Actual</h2>
<canvas id="barChart" width="600" height="280"></canvas>
</div>
<div class="panel">
<h2>Wafer Pass / Fail</h2>
<canvas id="lineChart" width="600" height="280"></canvas>
</div>
</div>

<div class="panel">
<h2>Shift Status Today</h2>
<div class="grid" id="shiftCards"></div>
</div>
</section>

<section id="technician" class="page hidden">
<div class="panel">
<h2>Technician Daily Work Log</h2>
<div class="two">
<div>
<label>Shift</label>
<select id="shift"><option>A</option><option>B</option><option>C</option><option>D</option></select>

<label>Lot ID</label>
<input id="lotId" placeholder="Example: LOT-2026-001">

<label>Passed Wafers</label>
<input id="passed" type="number" placeholder="0">

<label>Failed Wafers</label>
<input id="failed" type="number" placeholder="0">
</div>

<div>
<label>Status</label>
<select id="status">
<option>Released</option>
<option>Hold</option>
<option>STR</option>
<option>QARS</option>
<option>Black Mushroom</option>
</select>

<label>Tool / Maintenance Issue</label>
<input id="toolIssue" placeholder="Example: Rudolph F30 PM due">

<label>Safety Note</label>
<textarea id="safetyNote" placeholder="Enter safety notes"></textarea>

<label>Pass-Down Notes</label>
<textarea id="passdownNote" placeholder="Enter pass-down notes"></textarea>
</div>
</div>

<button class="btn" onclick="addEntry()">Submit Work Log</button>
<button class="btn dark" onclick="clearForm()">Clear</button>
</div>

<div class="panel">
<h2>Recent Work Logs</h2>
<table>
<thead><tr><th>Time</th><th>Shift</th><th>Lot</th><th>Passed</th><th>Failed</th><th>Status</th></tr></thead>
<tbody id="logTable"></tbody>
</table>
</div>
</section>

<section id="manager" class="page hidden">
<div class="panel">
<h2>Manager Approval Queue</h2>
<table>
<thead><tr><th>Item</th><th>Shift</th><th>Status</th><th>Action</th></tr></thead>
<tbody id="approvalTable"></tbody>
</table>
</div>
</section>

<section id="inventory" class="page hidden">
<div class="grid">
<div class="card"><h3>QARS</h3><div class="value blue" id="qarsCount">0</div></div>
<div class="card"><h3>Hold Lots</h3><div class="value yellow" id="holdCount">0</div></div>
<div class="card"><h3>STR</h3><div class="value purple" id="strCount">0</div></div>
<div class="card"><h3>Black Mushroom</h3><div class="value red" id="bmCount">0</div></div>
</div>

<div class="panel">
<h2>Lot Inventory</h2>
<table>
<thead><tr><th>Lot ID</th><th>Status</th><th>Shift</th><th>Owner</th></tr></thead>
<tbody id="inventoryTable"></tbody>
</table>
</div>
</section>

<section id="safety" class="page hidden">
<div class="grid">
<div class="card"><h3>Days Since Incident</h3><div class="value green">128</div></div>
<div class="card"><h3>Safety Notes</h3><div class="value blue" id="safetyCount">0</div></div>
<div class="card"><h3>Restarts Pending</h3><div class="value yellow">1</div></div>
</div>

<div class="panel">
<h2>Daily Safety Reminders</h2>
<ul style="padding-left:20px;line-height:1.9">
<li>Verify PPE before entering the production area.</li>
<li>Confirm ESD strap and cleanroom requirements.</li>
<li>Report near misses immediately.</li>
<li>Follow SOP before tool restart or lot release.</li>
</ul>
</div>
</section>

<section id="email" class="page hidden">
<div class="panel">
<h2>Email Report Generator</h2>
<button class="btn greenbtn" onclick="generateReport()">Generate Shift Report</button>
<button class="btn dark" onclick="copyReport()">Copy Report</button>
<div class="report" id="reportBox">Click Generate Shift Report.</div>
</div>
</section>

</div>

<footer>Samsung OQA Operations Dashboard • Demo Web App</footer>

<script>
let entries = JSON.parse(localStorage.getItem("oqa_entries") || "[]");

function save(){
localStorage.setItem("oqa_entries", JSON.stringify(entries));
}

function showPage(id,btn){
document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
document.querySelectorAll("nav button").forEach(b=>b.classList.remove("active"));
btn.classList.add("active");
render();
}

function addEntry(){
const entry={
time:new Date().toLocaleString(),
shift:document.getElementById("shift").value,
lot:document.getElementById("lotId").value || "N/A",
passed:Number(document.getElementById("passed").value || 0),
failed:Number(document.getElementById("failed").value || 0),
status:document.getElementById("status").value,
tool:document.getElementById("toolIssue").value,
safety:document.getElementById("safetyNote").value,
notes:document.getElementById("passdownNote").value,
approved:false
};
entries.unshift(entry);
save();
clearForm();
render();
alert("Work log submitted.");
}

function clearForm(){
["lotId","passed","failed","toolIssue","safetyNote","passdownNote"].forEach(id=>{
document.getElementById(id).value="";
});
}

function countStatus(s){
return entries.filter(e=>e.status===s).length;
}

function badge(status){
let cls="done";
if(status==="Hold")cls="hold";
if(status==="STR")cls="str";
if(status==="QARS" || status==="Black Mushroom")cls="open";
return `<span class="status ${cls}">${status}</span>`;
}

function render(){
const passed=entries.reduce((a,e)=>a+e.passed,0);
const failed=entries.reduce((a,e)=>a+e.failed,0);
const total=passed+failed;

document.getElementById("passRate").textContent=total?Math.round((passed/total)*100)+"%":"0%";
document.getElementById("lotsProcessed").textContent=entries.length;
document.getElementById("failedTotal").textContent=failed;
document.getElementById("openHolds").textContent=countStatus("Hold");
document.getElementById("qarsTotal").textContent=countStatus("QARS");
document.getElementById("toolIssues").textContent=entries.filter(e=>e.tool).length;

document.getElementById("qarsCount").textContent=countStatus("QARS");
document.getElementById("holdCount").textContent=countStatus("Hold");
document.getElementById("strCount").textContent=countStatus("STR");
document.getElementById("bmCount").textContent=countStatus("Black Mushroom");
document.getElementById("safetyCount").textContent=entries.filter(e=>e.safety).length;

const shifts=["A","B","C","D"];
document.getElementById("shiftCards").innerHTML=shifts.map(s=>{
const list=entries.filter(e=>e.shift===s);
const p=list.reduce((a,e)=>a+e.passed,0);
const f=list.reduce((a,e)=>a+e.failed,0);
return `<div class="card"><h3>Shift ${s}</h3><div class="value">${p+f}</div><p>Passed: ${p}</p><p>Failed: ${f}</p></div>`;
}).join("");

document.getElementById("logTable").innerHTML=entries.slice(0,10).map(e=>`
<tr><td>${e.time}</td><td>${e.shift}</td><td>${e.lot}</td><td>${e.passed}</td><td>${e.failed}</td><td>${badge(e.status)}</td></tr>
`).join("");

document.getElementById("inventoryTable").innerHTML=entries.map(e=>`
<tr><td>${e.lot}</td><td>${badge(e.status)}</td><td>${e.shift}</td><td>OQA</td></tr>
`).join("");

document.getElementById("approvalTable").innerHTML=entries.filter(e=>!e.approved).slice(0,10).map((e,i)=>`
<tr><td>${e.lot} - ${e.status}</td><td>${e.shift}</td><td>Pending</td><td><button class="btn greenbtn" onclick="approve(${i})">Approve</button></td></tr>
`).join("");

drawCharts();
}

function approve(index){
const pending=entries.filter(e=>!e.approved);
const target=pending[index];
const realIndex=entries.indexOf(target);
entries[realIndex].approved=true;
save();
render();
}

function drawCharts(){
drawBar();
drawLine();
}

function drawBar(){
const c=document.getElementById("barChart");
const ctx=c.getContext("2d");
ctx.clearRect(0,0,c.width,c.height);
const actual=[120,150,180,130,210,170,entries.reduce((a,e)=>a+e.passed+e.failed,0)];
const days=["Mon","Tue","Wed","Thu","Fri","Sat","Sun"];
const goal=200,base=240,max=250,w=45,gap=35;
ctx.font="13px Arial";
days.forEach((d,i)=>{
let x=45+i*(w+gap);
ctx.fillStyle="#e5e7eb";
ctx.fillRect(x,base-goal/max*200,w,goal/max*200);
ctx.fillStyle="#2563eb";
ctx.fillRect(x,base-actual[i]/max*200,w,actual[i]/max*200);
ctx.fillStyle="#64748b";
ctx.fillText(d,x,265);
});
}

function drawLine(){
const c=document.getElementById("lineChart");
const ctx=c.getContext("2d");
ctx.clearRect(0,0,c.width,c.height);
const pass=[20,30,25,40,35,45,entries.reduce((a,e)=>a+e.passed,0)];
const fail=[2,3,1,5,4,2,entries.reduce((a,e)=>a+e.failed,0)];
drawSeries(ctx,pass,"#16a34a");
drawSeries(ctx,fail,"#dc2626");
ctx.fillStyle="#64748b";
ctx.font="13px Arial";
ctx.fillText("Green = Passed   Red = Failed",25,260);
}

function drawSeries(ctx,data,color){
const max=Math.max(...data,10);
const base=230;
ctx.strokeStyle=color;
ctx.lineWidth=3;
ctx.beginPath();
data.forEach((v,i)=>{
let x=35+i*80;
let y=base-v/max*180;
if(i===0)ctx.moveTo(x,y);
else ctx.lineTo(x,y);
});
ctx.stroke();
ctx.fillStyle=color;
data.forEach((v,i)=>{
let x=35+i*80;
let y=base-v/max*180;
ctx.beginPath();
ctx.arc(x,y,5,0,Math.PI*2);
ctx.fill();
});
}

function generateReport(){
const passed=entries.reduce((a,e)=>a+e.passed,0);
const failed=entries.reduce((a,e)=>a+e.failed,0);

const report=`SAMSUNG OQA SHIFT REPORT

Date: ${new Date().toLocaleString()}

Production Summary:
Passed Wafers: ${passed}
Failed Wafers: ${failed}
Total Lots Processed: ${entries.length}

Inventory / Quality:
QARS: ${countStatus("QARS")}
Hold Lots: ${countStatus("Hold")}
STR: ${countStatus("STR")}
Black Mushroom: ${countStatus("Black Mushroom")}

Safety:
Safety Notes Entered: ${entries.filter(e=>e.safety).length}
Days Since Last Incident: 128

Maintenance:
Tool Issues Open: ${entries.filter(e=>e.tool).length}

Recent Pass-Down:
${entries.slice(0,5).map(e=>`Shift ${e.shift} | ${e.lot} | ${e.status} | ${e.notes || "No notes"}`).join("\n")}

Prepared by:
Samsung OQA Operations Dashboard`;

document.getElementById("reportBox").textContent=report;
}

function copyReport(){
navigator.clipboard.writeText(document.getElementById("reportBox").textContent);
alert("Report copied.");
}

render();
</script>

</body>
</html>