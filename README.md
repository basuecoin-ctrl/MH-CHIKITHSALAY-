<!DOCTYPE html>
<html>
<head>
<title>Homoeopathy Clinic Pro</title>

<style>

body{
font-family:Arial;
background:#f3f3f3;
padding:20px;
}

h1{
color:darkgreen;
}

h2{
background:#e8ffe8;
padding:5px;
}

input,textarea,select{
width:100%;
padding:8px;
margin:5px 0;
}

button{
padding:8px 15px;
margin-top:5px;
cursor:pointer;
}

table{
width:100%;
border-collapse:collapse;
background:white;
margin-top:20px;
}

th,td{
border:1px solid #ccc;
padding:8px;
text-align:center;
}

.container{
background:white;
padding:20px;
border-radius:10px;
}

</style>
</head>

<body>

<div class="container">

<h1>Homoeopathy Clinic Professional System</h1>

<h2>Patient Information</h2>

<input id="name" placeholder="Patient Name">
<input id="age" placeholder="Age">

<select id="gender">
<option>Male</option>
<option>Female</option>
</select>

<input id="mobile" placeholder="Mobile">

<textarea id="complaint" placeholder="Main Complaint"></textarea>

<textarea id="mental" placeholder="Mental Symptoms"></textarea>

<textarea id="history" placeholder="Past History"></textarea>

<textarea id="medicine" placeholder="Prescription"></textarea>

<input id="followup" type="date">

<button onclick="savePatient()">Save Patient</button>

<hr>

<h2>Search Patient</h2>

<input id="search" placeholder="Search patient" onkeyup="searchPatient()">

<h2>Patient List</h2>

<table id="table">

<tr>
<th>Name</th>
<th>Age</th>
<th>Mobile</th>
<th>Complaint</th>
<th>Medicine</th>
<th>FollowUp</th>
<th>Print</th>
<th>Delete</th>
</tr>

</table>

<hr>

<h2>Simple Repertory</h2>

<select id="symptom">
<option value="">Select Symptom</option>
<option value="fever">Fever sudden</option>
<option value="injury">Injury / trauma</option>
<option value="cold">Cold with sneezing</option>
<option value="anger">Anger / irritability</option>
<option value="joint">Joint pain worse movement</option>
</select>

<button onclick="suggestRemedy()">Suggest Remedy</button>

<h3 id="remedyResult"></h3>

</div>

<script>

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

table.innerHTML=`
<tr>
<th>Name</th>
<th>Age</th>
<th>Mobile</th>
<th>Complaint</th>
<th>Medicine</th>
<th>FollowUp</th>
<th>Print</th>
<th>Delete</th>
</tr>
`;

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
w.document.write("Name: "+p.name+"<br>");
w.document.write("Age: "+p.age+"<br>");
w.document.write("Complaint: "+p.complaint+"<br><br>");
w.document.write("Medicine: "+p.medicine+"<br>");

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

function suggestRemedy(){

let s=document.getElementById("symptom").value;

let remedy="";

if(s=="fever") remedy="Aconite / Belladonna";
if(s=="injury") remedy="Arnica";
if(s=="cold") remedy="Allium Cepa";
if(s=="anger") remedy="Nux Vomica";
if(s=="joint") remedy="Rhus Toxicodendron";

document.getElementById("remedyResult").innerHTML="Suggested Remedy: "+remedy;

}

showPatients();

</script>

</body>
</html><!DOCTYPE html>
<html>
<head>
<title>Homoeopathy AI Assistant</title>

<style>

body{
font-family:Arial;
background:#f2f2f2;
padding:20px;
}

.container{
background:white;
padding:20px;
border-radius:10px;
max-width:700px;
margin:auto;
box-shadow:0 0 10px gray;
}

input,button{
padding:10px;
margin:5px;
font-size:16px;
}

button{
background:#2e86de;
color:white;
border:none;
border-radius:5px;
}

table{
width:100%;
margin-top:20px;
border-collapse:collapse;
}

table,th,td{
border:1px solid gray;
padding:10px;
text-align:center;
}

</style>

</head>

<body>

<div class="container">

<h2>Homoeopathy AI Case Taking</h2>

Patient Name  
<input type="text" id="name">

<br>

Age  
<input type="number" id="age">

<br>

Symptom  
<input list="symptoms" id="symptomInput">

<datalist id="symptoms">

<option value="fever">
<option value="cold">
<option value="cough">
<option value="headache">
<option value="anger">
<option value="joint pain">
<option value="injury">
<option value="diarrhea">
<option value="vomiting">
<option value="constipation">
<option value="fear">
<option value="anxiety">
<option value="weakness">
<option value="sleep problem">

</datalist>

<button onclick="addSymptom()">Add Symptom</button>

<h3>Selected Symptoms</h3>

<ul id="symptomList"></ul>

<button onclick="analyze()">AI Remedy Suggestion</button>

<h3>Suggested Remedies</h3>

<table id="resultTable">

<tr>
<th>Remedy</th>
<th>Score</th>
<th>Advice</th>
</tr>

</table>

</div>

<script>

let patientSymptoms=[];

const remedyDB={

"Aconite":{
symptoms:["fever","fear","anxiety"],
advice:"Sudden fever after cold exposure"
},

"Belladonna":{
symptoms:["fever","headache"],
advice:"High fever with throbbing headache"
},

"Nux Vomica":{
symptoms:["anger","constipation","cold"],
advice:"Digestive disturbance with irritability"
},

"Arnica":{
symptoms:["injury","pain"],
advice:"Best remedy for trauma and injury"
},

"Rhus Tox":{
symptoms:["joint pain","stiffness"],
advice:"Joint pain better by movement"
},

"Arsenicum Album":{
symptoms:["vomiting","diarrhea","weakness"],
advice:"Food poisoning and weakness"
}

};

function addSymptom(){

let sym=document.getElementById("symptomInput").value;

if(sym!=""){

patientSymptoms.push(sym);

let li=document.createElement("li");
li.innerText=sym;

document.getElementById("symptomList").appendChild(li);

document.getElementById("symptomInput").value="";
}

}

function analyze(){

let scores={};

for(let remedy in remedyDB){

scores[remedy]=0;

remedyDB[remedy].symptoms.forEach(sym=>{

if(patientSymptoms.includes(sym)){

scores[remedy]+=1;

}

});

}

let sorted=Object.keys(scores).sort((a,b)=>scores[b]-scores[a]);

let table=document.getElementById("resultTable");

table.innerHTML=`
<tr>
<th>Remedy</th>
<th>Score</th>
<th>Advice</th>
</tr>
`;

sorted.slice(0,5).forEach(remedy=>{

let row=table.insertRow();

row.insertCell(0).innerText=remedy;
row.insertCell(1).innerText=scores[remedy];
row.insertCell(2).innerText=remedyDB[remedy].advice;

});

}

</script>

</body>
</html>

