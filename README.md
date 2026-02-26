# Fitness-app
<!DOCTYPE html>
<html lang="hr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FitZona Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif}
body{
background:linear-gradient(135deg,#0f172a,#111827);
color:white;
overflow-x:hidden;
}
.hidden{display:none}

header{
position:fixed;
width:100%;
background:rgba(15,23,42,.8);
backdrop-filter:blur(15px);
padding:20px 40px;
display:flex;
justify-content:space-between;
align-items:center;
border-bottom:1px solid #1e293b;
z-index:1000;
}

header h1{
color:#22c55e;
font-weight:800;
}

nav a{
margin-left:20px;
color:white;
text-decoration:none;
cursor:pointer;
transition:.3s;
}

nav a:hover{color:#22c55e}

section{
padding:120px 40px;
min-height:100vh;
}

.hero{
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
text-align:center;
background:linear-gradient(rgba(0,0,0,.7),rgba(0,0,0,.7)),
url('https://images.unsplash.com/photo-1599058917765-a780eda07a3e?auto=format&fit=crop&w=1600&q=80')
center/cover;
}

.hero h2{
font-size:50px;
margin-bottom:20px;
font-weight:800;
}

.btn{
background:#22c55e;
padding:12px 25px;
border:none;
border-radius:30px;
cursor:pointer;
font-weight:600;
margin-top:15px;
transition:.3s;
}

.btn:hover{
background:#16a34a;
transform:scale(1.05);
}

.cards{
display:flex;
flex-wrap:wrap;
justify-content:center;
gap:25px;
}

.card{
background:rgba(30,41,59,.7);
padding:25px;
border-radius:20px;
width:300px;
backdrop-filter:blur(15px);
transition:.3s;
}

.card:hover{
transform:translateY(-5px);
box-shadow:0 0 25px rgba(34,197,94,.3);
}

input,select{
padding:10px;
margin:8px 0;
width:100%;
border-radius:8px;
border:none;
}

canvas{margin-top:30px}

footer{
text-align:center;
padding:30px;
background:#0f172a;
border-top:1px solid #1e293b;
}

@media(max-width:768px){
.hero h2{font-size:32px}
.cards{flex-direction:column;align-items:center}
}
</style>
</head>

<body>

<header>
<h1>FitZona Pro</h1>
<nav>
<a onclick="showSection('home')">Početna</a>
<a onclick="showSection('workouts')">Vježbe</a>
<a onclick="showSection('nutrition')">Prehrana</a>
<a onclick="showSection('progress')">Napredak</a>
<a onclick="showSection('dashboard')">Dashboard</a>
</nav>
</header>

<!-- HOME -->
<section id="home" class="hero">
<h2>Postani Najbolja Verzija Sebe</h2>
<p>Trening, prehrana, AI plan i praćenje napretka.</p>
<button class="btn" onclick="showSection('dashboard')">Kreni Sada</button>
</section>

<!-- WORKOUTS -->
<section id="workouts" class="hidden">
<h2>Vježbe po Mišićnim Skupinama</h2>
<div class="cards">
<div class="card"><h3>Prsa</h3>Bench press<br>Incline DB press<br>Chest fly<br>Sklekovi</div>
<div class="card"><h3>Leđa</h3>Deadlift<br>Pull-ups<br>Barbell row<br>Lat pulldown</div>
<div class="card"><h3>Noge</h3>Čučanj<br>Leg press<br>Iskoraci<br>RDL</div>
<div class="card"><h3>Ramena</h3>Shoulder press<br>Lateral raise<br>Rear delt fly</div>
<div class="card"><h3>Ruke</h3>Biceps curl<br>Hammer curl<br>Dips<br>Pushdown</div>
<div class="card"><h3>Core</h3>Plank<br>Leg raises<br>Russian twist<br>Ab wheel</div>
</div>
</section>

<!-- NUTRITION -->
<section id="nutrition" class="hidden">
<h2>Prehrana i Kalorije</h2>
<div class="cards">

<div class="card">
<h3>BMR Kalkulator</h3>
<input type="number" id="weightCalc" placeholder="Težina (kg)">
<input type="number" id="heightCalc" placeholder="Visina (cm)">
<input type="number" id="ageCalc" placeholder="Godine">
<select id="goal">
<option value="maintain">Održavanje</option>
<option value="bulk">Masa</option>
<option value="cut">Definicija</option>
</select>
<button class="btn" onclick="calculateCalories()">Izračunaj</button>
<p id="calorieResult"></p>
</div>

<div class="card">
<h3>Primjer Plana Prehrane</h3>
Doručak: Zobene + whey<br>
Ručak: Piletina + riža<br>
Večera: Losos + povrće<br>
Snack: Orašasti plodovi
</div>

</div>
</section>

<!-- PROGRESS -->
<section id="progress" class="hidden">
<h2>Praćenje Težine</h2>
<input type="number" id="weightInput" placeholder="Unesi težinu">
<button class="btn" onclick="addWeight()">Spremi</button>
<canvas id="weightChart"></canvas>
</section>

<!-- DASHBOARD -->
<section id="dashboard" class="hidden">
<h2>Dashboard</h2>
<div class="cards">

<div class="card">
<h3>AI Plan Treninga</h3>
<select id="aiGoal">
<option value="masa">Masa</option>
<option value="definicija">Definicija</option>
<option value="snaga">Snaga</option>
</select>
<button class="btn" onclick="generatePlan()">Generiraj</button>
<p id="aiPlan"></p>
</div>

<div class="card">
<h3>Korisnik (Opcionalno)</h3>
<input type="text" id="name" placeholder="Ime">
<input type="email" id="email" placeholder="Email">
<button class="btn" onclick="saveUser()">Spremi</button>
<p id="userStatus"></p>
</div>

</div>
</section>

<footer>
© 2026 FitZona Pro
</footer>

<script>

function showSection(id){
document.querySelectorAll("section").forEach(sec=>sec.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}

function calculateCalories(){
let w=weightCalc.value;
let h=heightCalc.value;
let a=ageCalc.value;
let goal=document.getElementById("goal").value;
if(w && h && a){
let bmr=10*w+6.25*h-5*a+5;
if(goal==="bulk") bmr+=300;
if(goal==="cut") bmr-=300;
calorieResult.innerText="Preporučeni unos: "+Math.round(bmr)+" kcal";
}
}

function generatePlan(){
let goal=document.getElementById("aiGoal").value;
let text="";
if(goal==="masa"){
text="5 treninga tjedno, 8-12 ponavljanja, progresivno opterećenje.";
}
if(goal==="definicija"){
text="4 treninga + kardio 3x tjedno, kalorijski deficit.";
}
if(goal==="snaga"){
text="Compound pokreti 3-5 ponavljanja, duži odmor.";
}
document.getElementById("aiPlan").innerText=text;
}

function saveUser(){
let name=document.getElementById("name").value;
if(name){
localStorage.setItem("fitzonaUser",name);
document.getElementById("userStatus").innerText="Korisnik spremljen.";
}
}

let weights=JSON.parse(localStorage.getItem("weights"))||[];

function addWeight(){
let w=document.getElementById("weightInput").value;
if(w){
weights.push(w);
localStorage.setItem("weights",JSON.stringify(weights));
updateChart();
}
}

let ctx=document.getElementById("weightChart").getContext("2d");
let chart=new Chart(ctx,{
type:"line",
data:{labels:[],datasets:[{label:"Težina (kg)",data:[],borderWidth:2,tension:.3}]}
});

function updateChart(){
chart.data.labels=weights.map((_,i)=>"Unos "+(i+1));
chart.data.datasets[0].data=weights;
chart.update();
}

updateChart();

</script>

</body>
</html>
