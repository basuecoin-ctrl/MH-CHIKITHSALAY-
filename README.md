<Dr.R.K.Basu Basu>
<html>
<head>
<title>Homoeopathy Clinic AI Level 7</title>
<style>
body{font-family:Arial;background:#f4f4f4;padding:20px;}
h1{color:darkgreen;}
h2{background:#e8ffe8;padding:5px;}
input,textarea,select{width:100%;padding:8px;margin:5px 0;}
button{padding:8px 15px;margin-top:5px;cursor:pointer;}
table{width:100%;border-collapse:collapse;background:white;margin-top:20px;}
th,td{border:1px solid #ccc;padding:8px;text-align:center;}
.container{background:white;padding:20px;border-radius:10px;}
</style>
</head>
<body>
<div class="container">
<h1>Homoeopathy Clinic AI - Level 7</h1>

<h2>Patient Information</h2>
<input id="name" placeholder="Patient Name">
<input id="age" placeholder="Age">
<select id="gender"><option>Male</option><option>Female</option></select>
<input id="mobile" placeholder="Mobile">
<textarea id="address" placeholder="Address"></textarea>

<h2>Chief Complaint</h2>
<textarea id="complaint" placeholder="Main Complaint"></textarea>
<textarea id="mental" placeholder="Mental Symptoms"></textarea>
<textarea id="history" placeholder="Past History"></textarea>

<h2>Prescription</h2>
<textarea id="medicine" placeholder="Medicine / Potency / Dose"></textarea>
<input id="followup" type="date">
<button onclick="savePatient()">Save Patient</button>

<hr>
<h2>Search Patient</h2>
<input id="search" placeholder="Search patient" onkeyup="searchPatient()">

<h2>Patient List</h2>
<table id="table">
<tr>
<th>Name</th><th>Age</th><th>Mobile</th><th>Complaint</th><th>Medicine</th><th>FollowUp</th><th>Print</th><th>Delete</th>
</tr>
</table>

<hr>
<h2>AI Symptom → Remedy Suggestion</h2>
<p>Enter symptoms separated by comma (e.g., fever,cold,anger)</p>
<input id="symptoms" placeholder="Enter symptoms">
<button onclick="aiSuggest()">Suggest Remedies</button>
<h3 id="aiResult"></h3>
</div>

<script>
// Patient Database
let patients = JSON.parse(localStorage.getItem("patients")) || [];

function savePatient(){
let p={
name:document.getElementById("name").value,
age:document.getElementById("age").value,
mobile:document.getElementById("mobile").value,
complaint:document.getElementById("complaint").value,
medicine:document.getElementById("medicine").value,
followup:document.getElementById("followup").value
};
patients.push(p);
localStorage.setItem("patients",JSON.stringify(patients));
showPatients();
}

function showPatients(){
let table=document.getElementById("table");
table.innerHTML=`<tr><th>Name</th><th>Age</th><th>Mobile</th><th>Complaint</th><th>Medicine</th><th>FollowUp</th><th>Print</th><th>Delete</th></tr>`;
patients.forEach((p,i)=>{
let row=table.insertRow();
row.insertCell(0).innerHTML=p.name;
row.insertCell(1).innerHTML=p.age;
row.insertCell(2).innerHTML=p.mobile;
row.insertCell(3).innerHTML=p.complaint;
row.insertCell(4).innerHTML=p.medicine;
row.insertCell(5).innerHTML=p.followup;

let printBtn=document.createElement("button");
printBtn.innerHTML="Print";
printBtn.onclick=function(){
let w=window.open();
w.document.write("<h2>Homoeopathy Prescription</h2>");
w.document.write("Name: "+p.name+"<br>Age: "+p.age+"<br>Mobile: "+p.mobile+"<br>");
w.document.write("Complaint: "+p.complaint+"<br>Medicine: "+p.medicine+"<br>FollowUp: "+p.followup);
w.print();
}
row.insertCell(6).appendChild(printBtn);

let delBtn=document.createElement("button");
delBtn.innerHTML="Delete";
delBtn.onclick=function(){
patients.splice(i,1);
localStorage.setItem("patients",JSON.stringify(patients));
showPatients();
}
row.insertCell(7).appendChild(delBtn);
});
}

function searchPatient(){
let text=document.getElementById("search").value.toLowerCase();
let filtered=patients.filter(p=>p.name.toLowerCase().includes(text));
let table=document.getElementById("table");
table.innerHTML="";
filtered.forEach(p=>{
let row=table.insertRow();
row.insertCell(0).innerHTML=p.name;
row.insertCell(1).innerHTML=p.age;
row.insertCell(2).innerHTML=p.mobile;
row.insertCell(3).innerHTML=p.complaint;
row.insertCell(4).innerHTML=p.medicine;
row.insertCell(5).innerHTML=p.followup;
});
}

// AI Symptom → Remedy
const remedyDB = {
"fever":["Aconite 30C","Belladonna 30C"],
"cold":["Allium Cepa 30C","Nux Vomica 30C"],
"injury":["Arnica 200C"],
"anger":["Nux Vomica 30C","Chamomilla 30C"],
"joint":["Rhus Tox 30C","Bryonia 30C"],
"sleep":["Coffea 30C","Nux Vomica 30C"],
"headache":["Belladonna 30C","Gelsemium 30C"],
"diarrhea":["Arsenicum Album 30C","Podophyllum 30C"]
};

function aiSuggest(){
let input=document.getElementById("symptoms").value.toLowerCase();
let symptoms=input.split(",").map(s=>s.trim());
let suggestion={};
symptoms.forEach(s=>{
if(remedyDB[s]){
remedyDB[s].forEach(r=>{
suggestion[r]=(suggestion[r]||0)+1;
});
}
});
// sort by score
let sorted=Object.keys(suggestion).sort((a,b)=>suggestion[b]-suggestion[a]);
document.getElementById("aiResult").innerHTML="Top Remedies: "+sorted.join(", ");
}

showPatients();
</script>
</body>
</html>
