# Fit4Liffe
<!DOCTYPE html>
<html lang="hr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FitZona Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Inter',sans-serif}

body{
background:#0f172a;
color:white;
display:flex;
min-height:100vh;
}

/* SIDEBAR */
.sidebar{
width:240px;
background:#111827;
padding:30px 20px;
display:flex;
flex-direction:column;
border-right:1px solid #1f2937;
}

.sidebar h1{
color:#22c55e;
margin-bottom:40px;
font-size:22px;
}

.sidebar button{
background:none;
border:none;
color:#cbd5e1;
text-align:left;
padding:12px 10px;
margin-bottom:10px;
cursor:pointer;
border-radius:8px;
transition:.2s;
}

.sidebar button:hover{
background:#1f2937;
color:white;
}

/* MAIN */
.main{
flex:1;
padding:40px;
}

.hidden{display:none}

.card{
background:#1e293b;
padding:25px;
border-radius:16px;
margin-bottom:25px;
}

input,select{
width:100%;
padding:10px;
margin:8px 0;
border-radius:8px;
border:none;
}

.btn{
background:#22c55e;
border:none;
padding:10px 20px;
border-radius:8px;
cursor:pointer;
margin-top:10px;
font-weight:600;
}

.btn:hover{background:#16a34a}

.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
gap:20px;
}

canvas{margin-top:20px}

@media(max-width:768px){
.sidebar{display:none}
}
</style>
</head>

<body>

<div class="sidebar">
<h1>FitZona Pro</h1>
<button onclick="show('dashboard')">Dashboard</button>
<button onclick="show('workouts')">Vježbe</button>
<button onclick="show('nutrition')">Prehrana</button>
<button onclick="show('progress')">Napredak</button>
<button onclick="show('profile')">Profil</button>
</div>

<div class="main">

<!-- DASHBOARD -->
<div id="dashboard">
<h2>Dashboard</h2>
<div class="grid">

<div class="card">
<h3>AI Plan Treninga</h3>
<select id="goal">
<option value="masa">Masa</option>
<option value="definicija">Definicija</option>
<option value="snaga">Snaga</option>
</select>
<button class="btn" onclick="generatePlan()">Generiraj</button>
<p id="planOutput"></p>
</div>

<div class="card">
<h3>Brzi Kalorijski Kalkulator</h3>
<input type="number" id="weight" placeholder="Težina (kg)">
<input type="number" id="height" placeholder="Visina (cm)">
<input type="number" id="age" placeholder="Godine">
<select id="calGoal">
<option value="maintain">Održavanje</option>
<option value="bulk">Masa (+300)</option>
<option value="cut">Definicija (-300)</option>
</select>
<button class="btn" onclick="calculate()">Izračunaj</button>
<p id="calResult"></p>
</div>

</div>
</div>

<!-- WORKOUTS -->
<div id="workouts" class="hidden">
<h2>Baza Vježbi</h2>
<div class="grid">

<div class="card"><h3>Prsa</h3>Bench press<br>Incline press<br>Chest fly<br>Push-ups</div>
<div class="card"><h3>Leđa</h3>Deadlift<br>Pull-ups<br>Barbell row<br>Lat pulldown</div>
<div class="card"><h3>Noge</h3>Squat<br>Leg press<br>Iskoraci<br>RDL</div>
<div class="card"><h3>Ramena</h3>Shoulder press<br>Lateral raise<br>Rear delt fly</div>
<div class="card"><h3>Ruke</h3>Biceps curl<br>Hammer curl<br>Dips<br>Pushdown</div>
<div class="card"><h3>Core</h3>Plank<br>Leg raises<br>Russian twist<br>Ab wheel</div>

</div>
</div>

<!-- NUTRITION -->
<div id="nutrition" class="hidden">
<h2>Praćenje Kalorija</h2>

<div class="card">
<input type="number" id="foodCalories" placeholder="Unesi kalorije obroka">
<button class="btn" onclick="addCalories()">Dodaj</button>
<p id="totalCalories"></p>
</div>

</div>

<!-- PROGRESS -->
<div id="progress" class="hidden">
<h2>Praćenje Težine</h2>
<div class="card">
<input type="number" id="weightInput" placeholder="Unesi težinu">
<button class="btn" onclick="addWeight()">Spremi</button>
<canvas id="chart"></canvas>
</div>
</div>

<!-- PROFILE -->
<div id="profile" class="hidden">
<h2>Profil (nije obavezno)</h2>
<div class="card">
<input type="text" id="username" placeholder="Ime">
<button class="btn" onclick="saveUser()">Spremi profil</button>
<p id="profileStatus"></p>
</div>
</div>

</div>

<script>
function show(id){
document.querySelectorAll(".main > div").forEach(d=>d.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}

/* AI PLAN */
function generatePlan(){
let g=document.getElementById("goal").value;
let text="";
if(g==="masa") text="5 treninga tjedno, 8-12 ponavljanja, kalorijski suficit.";
if(g==="definicija") text="4 treninga + kardio, kalorijski deficit.";
if(g==="snaga") text="Teški compound pokreti 3-5 ponavljanja.";
document.getElementById("planOutput").innerText=text;
}

/* KALORIJE */
function calculate(){
let w=weight.value,h=height.value,a=age.value;
let goal=calGoal.value;
let bmr=10*w+6.25*h-5*a+5;
if(goal==="bulk") bmr+=300;
if(goal==="cut") bmr-=300;
calResult.innerText="Preporučeni unos: "+Math.round(bmr)+" kcal";
}

/* PRAĆENJE HRANE */
let total=0;
function addCalories(){
let c=parseInt(foodCalories.value);
if(c){
total+=c;
totalCalories.innerText="Današnji unos: "+total+" kcal";
foodCalories.value="";
}
}

/* TEŽINA + GRAF */
let weights=JSON.parse(localStorage.getItem("weights"))||[];
let ctx=document.getElementById("chart").getContext("2d");
let chart=new Chart(ctx,{
type:"line",
data:{labels:[],datasets:[{label:"Težina",data:[],borderWidth:2,tension:.3}]}
});

function updateChart(){
chart.data.labels=weights.map((_,i)=>"Unos "+(i+1));
chart.data.datasets[0].data=weights;
chart.update();
}

function addWeight(){
let w=weightInput.value;
if(w){
weights.push(w);
localStorage.setItem("weights",JSON.stringify(weights));
updateChart();
weightInput.value="";
}
}

updateChart();

/* PROFIL */
function saveUser(){
let name=username.value;
if(name){
localStorage.setItem("user",name);
profileStatus.innerText="Profil spremljen.";
}
}
</script>

</body>
</html>
