<!DOCTYPE html>
<html>
<head>
<title>Tech Quiz Master 🚀</title>

<!-- Google Font -->
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500&display=swap" rel="stylesheet">

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
body{
    margin:0;
    font-family: 'Orbitron', sans-serif;
    text-align:center;
    color:white;
    background: linear-gradient(-45deg,#0f2027,#203a43,#2c5364,#000428);
    background-size: 400% 400%;
    animation: gradientBG 10s ease infinite;
}

@keyframes gradientBG{
    0%{background-position:0% 50%;}
    50%{background-position:100% 50%;}
    100%{background-position:0% 50%;}
}

.container{
    width:90%;
    max-width:700px;
    margin:50px auto;
    background:rgba(0,0,0,0.7);
    padding:25px;
    border-radius:20px;
    box-shadow:0 0 25px cyan;
}

h1{
    font-size:30px;
    color:#00f7ff;
}

button{
    padding:10px 15px;
    margin:8px;
    border:none;
    border-radius:10px;
    cursor:pointer;
    font-weight:bold;
}

.option{
    display:block;
    background:#111;
    color:white;
}

.option:hover{
    background:cyan;
    color:black;
}

#result{
    font-size:20px;
    margin-top:20px;
    color:yellow;
}

input{
    padding:10px;
    border-radius:8px;
    border:none;
}
table{
    width:100%;
    margin-top:20px;
    border-collapse:collapse;
}
th,td{
    border:1px solid cyan;
    padding:8px;
}
</style>
</head>

<body>

<div class="container" id="loginBox">
<h1>💻 TECH QUIZ MASTER 🚀</h1>
<p>Enter your name to begin:</p>
<input type="text" id="username" placeholder="Your Name">
<br><br>
<button onclick="startQuiz()">Start Quiz 🔥</button>
</div>

<div class="container" id="quizBox" style="display:none;">
<h1>🚀 Tech Challenge</h1>
<div id="quiz"></div>
<button onclick="submitQuiz()">Submit Quiz 🧠</button>
<div id="result"></div>
<button id="downloadBtn" style="display:none;" onclick="downloadCertificate()">Download Certificate 🏆</button>
</div>

<div class="container" id="leaderboardBox" style="display:none;">
<h1>🏆 Leaderboard</h1>
<table>
<thead>
<tr><th>Name</th><th>Score</th></tr>
</thead>
<tbody id="leaderboard"></tbody>
</table>
</div>

<script>

const questions=[
{
q:"1️⃣ What does CPU stand for?",
a:["Central Processing Unit","Computer Personal Unit","Central Print Unit","Control Processing User"],
correct:0
},
{
q:"2️⃣ Which language is used for web styling?",
a:["Python","HTML","CSS","C++"],
correct:2
},
{
q:"3️⃣ What does HTTP stand for?",
a:["Hyper Text Transfer Protocol","High Transfer Text Process","Hyperlink Text Protocol","Home Transfer Tool Protocol"],
correct:0
},
{
q:"4️⃣ Which company developed Java?",
a:["Microsoft","Sun Microsystems","Apple","Google"],
correct:1
},
{
q:"5️⃣ What is the brain of the computer?",
a:["Monitor","Keyboard","CPU","Mouse"],
correct:2
}
];

let score=0;
let player="";

function startQuiz(){
player=document.getElementById("username").value;
if(player===""){
alert("Please enter your name!");
return;
}
document.getElementById("loginBox").style.display="none";
document.getElementById("quizBox").style.display="block";
loadQuiz();
}

function loadQuiz(){
const quiz=document.getElementById("quiz");
questions.forEach((q,index)=>{
quiz.innerHTML+=`<p>${q.q}</p>`;
q.a.forEach((opt,i)=>{
quiz.innerHTML+=`<button class="option" onclick="selectAnswer(${index},${i})">${opt}</button>`;
});
});
}

let answers={};

function selectAnswer(q,i){
answers[q]=i;
}

function submitQuiz(){
score=0;
questions.forEach((q,i)=>{
if(answers[i]==q.correct){
score++;
}
});

document.getElementById("result").innerHTML=
`🎯 ${player}, Your Score: ${score}/5`;

saveScore(player,score);
showLeaderboard();

document.getElementById("downloadBtn").style.display="inline-block";
}

function saveScore(name,score){
let data=JSON.parse(localStorage.getItem("scores"))||[];
data.push({name,score});
localStorage.setItem("scores",JSON.stringify(data));
}

function showLeaderboard(){
document.getElementById("leaderboardBox").style.display="block";
let data=JSON.parse(localStorage.getItem("scores"))||[];
data.sort((a,b)=>b.score-a.score);

let board=document.getElementById("leaderboard");
board.innerHTML="";
data.forEach(d=>{
board.innerHTML+=`<tr><td>${d.name}</td><td>${d.score}</td></tr>`;
});
}

async function downloadCertificate(){
const {jsPDF}=window.jspdf;
const doc=new jsPDF();

doc.setFontSize(22);
doc.text("TECH QUIZ CERTIFICATE 🏆",20,30);

doc.setFontSize(16);
doc.text(`Awarded to: ${player}`,20,50);
doc.text(`Score: ${score}/5`,20,65);

doc.save("TechQuizCertificate.pdf");
}

</script>

</body>
</html>
