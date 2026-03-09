<!DOCTYPE html>
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

