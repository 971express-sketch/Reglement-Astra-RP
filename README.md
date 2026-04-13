astra-rp-reglement/
│── index.html        (site principal)
│── admin.html        (panel admin)
│── style.css
│── script.js
│── firebase.js
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Astra RP</title>
<link rel="stylesheet" href="style.css">
</head>

<body>

<button onclick="toggleMenu()" class="menu">☰</button>

<div class="sidebar" id="sidebar">
  <input type="text" id="search" placeholder="Rechercher..." onkeyup="searchRule()">
  <div onclick="showPage('reglement')">Règlement</div>
</div>

<div class="content" id="reglement"></div>

<script src="firebase.js"></script>
<script src="script.js"></script>
</body>
</html>
body{margin:0;background:#0a0a0f;color:white;font-family:sans-serif}
.sidebar{width:250px;background:#111;position:fixed;height:100%}
.content{margin-left:260px;padding:20px}
.menu{position:fixed;top:10px;left:10px}
input{width:90%;margin:10px;padding:8px}
function toggleMenu(){
  document.getElementById("sidebar").classList.toggle("hide")
}

function showPage(page){
  loadRules()
}

function searchRule(){
  let input = document.getElementById("search").value.toLowerCase()
  document.querySelectorAll(".rule").forEach(r=>{
    r.style.display = r.innerText.toLowerCase().includes(input) ? "block":"none"
  })
}

function loadRules(){
  db.collection("rules").get().then(snapshot=>{
    let html=""
    snapshot.forEach(doc=>{
      html += `<div class="rule">${doc.data().text}</div>`
    })
    document.getElementById("reglement").innerHTML = html
  })
}

loadRules()
const firebaseConfig = {
  apiKey: "TA_CLE",
  authDomain: "TON_APP",
  projectId: "TON_ID"
};

firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();
<!DOCTYPE html>
<html>
<head><title>Admin Astra</title></head>
<body>

<h2>Ajouter une règle</h2>
<input id="ruleText" placeholder="Nouvelle règle">
<button onclick="addRule()">Ajouter</button>

<h2>Liste</h2>
<div id="list"></div>

<script src="firebase.js"></script>

<script>
function addRule(){
  let text=document.getElementById("ruleText").value
  db.collection("rules").add({text})
}

function load(){
  db.collection("rules").onSnapshot(snap=>{
    let html=""
    snap.forEach(doc=>{
      html+=`<div>${doc.data().text}
      <button onclick="del('${doc.id}')">X</button></div>`
    })
    document.getElementById("list").innerHTML=html
  })
}

function del(id){
  db.collection("rules").doc(id).delete()
}

load()
</script>

</body>
</html>
astra_reglement/
│── fxmanifest.lua
│── index.html
fx_version 'cerulean'
game 'gta5'

ui_page 'index.html'

files {
  'index.html'
}

client_script 'client.lua'
RegisterCommand("reglement", function()
    SetNuiFocus(true, true)
    SendNUIMessage({type="open"})
end)
window.addEventListener("message", function(e){
  if(e.data.type==="open"){
    document.body.style.display="block"
  }
})
