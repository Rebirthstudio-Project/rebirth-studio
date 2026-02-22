<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rebirth Studio</title>

<style>
:root{
 --bg:#0b0f1a;
 --card:rgba(255,255,255,0.06);
 --text:#fff;
 --accent:#9d4edd;
 --border:rgba(255,255,255,0.08);
}

.light{
 --bg:#f4f6fb;
 --card:rgba(255,255,255,0.8);
 --text:#111;
 --accent:#ff4081;
 --border:rgba(0,0,0,0.08);
}

*{box-sizing:border-box;margin:0;padding:0;}

body{
 font-family:Segoe UI;
 background:var(--bg);
 color:var(--text);
 transition:0.3s;
}

/* HEADER */
.topbar{
 display:flex;
 align-items:center;
 justify-content:space-between;
 padding:12px 15px;
 background:var(--card);
 backdrop-filter:blur(10px);
 border-bottom:1px solid var(--border);
 position:sticky;
 top:0;
 z-index:1000;
}

.menu-btn{
 font-size:22px;
 cursor:pointer;
}

/* SIDEBAR */
.sidebar{
 position:fixed;
 top:0;
 left:0;
 height:100%;
 width:280px;
 background:var(--card);
 backdrop-filter:blur(15px);
 border-right:1px solid var(--border);
 transform:translateX(-100%);
 transition:transform 0.4s cubic-bezier(.77,0,.18,1);
 padding-top:70px;
 z-index:999;
 overflow:auto;
}

.sidebar.open{
 transform:translateX(0);
}

.sidebar button{
 display:block;
 width:90%;
 margin:10px auto;
 padding:10px;
 border:none;
 border-radius:10px;
 background:var(--accent);
 color:white;
 cursor:pointer;
}

.container{
 padding:20px;
}

.card{
 background:var(--card);
 padding:15px;
 border-radius:15px;
 margin-bottom:15px;
 border:1px solid var(--border);
}

/* SETTINGS SLIDE PANEL */
.settings{
 display:none;
 margin-top:15px;
}

.setting-item{
 margin-bottom:15px;
}

input[type="range"]{
 width:100%;
}

input[type="file"]{
 margin-top:5px;
}

img{
 width:100%;
 border-radius:12px;
 margin-top:10px;
}
</style>
</head>

<body>

<div class="topbar">
 <span class="menu-btn" onclick="toggleMenu()">☰</span>
 <span id="siteTitle">Rebirth Studio</span>
</div>

<div class="sidebar" id="sidebar">
 <button onclick="toggleTheme()">🌙 Toggle Theme</button>
 <button onclick="showSettings()">⚙ Settings</button>
 <button onclick="loginOwner()">👑 Owner Login</button>
</div>

<div class="container">

 <div id="bannerArea"></div>

 <div class="card">
   <h2>Welcome</h2>
   <p>Simple premium mobile layout.</p>
 </div>

 <!-- SETTINGS PANEL -->
 <div class="card settings" id="settingsPanel">
   <h3>Settings</h3>

   <div class="setting-item">
     <label>Accent Strength</label>
     <input type="range" min="0" max="20" value="9" oninput="changeAccent(this.value)">
   </div>

   <div class="setting-item">
     <label>Font Size</label>
     <input type="range" min="14" max="22" value="16" oninput="changeFont(this.value)">
   </div>

   <div class="setting-item" id="ownerUpload" style="display:none;">
     <label>Upload Banner (Owner)</label>
     <input type="file" onchange="uploadBanner(event)">
   </div>

 </div>

</div>

<script>
let owner=false;

function toggleMenu(){
 sidebar.classList.toggle("open");
}

function toggleTheme(){
 document.body.classList.toggle("light");
}

function showSettings(){
 settingsPanel.style.display=
 settingsPanel.style.display==="block"?"none":"block";
}

function changeAccent(value){
 document.documentElement.style.setProperty("--accent",
 `hsl(${value*18},70%,55%)`);
}

function changeFont(size){
 document.body.style.fontSize=size+"px";
}

function loginOwner(){
 let pass=prompt("Enter owner password:");
 if(pass==="Rebirth999"){
  owner=true;
  alert("Owner mode enabled");
  ownerUpload.style.display="block";
 }else{
  alert("Wrong password");
 }
}

function uploadBanner(event){
 let reader=new FileReader();
 reader.onload=function(e){
  localStorage.setItem("bannerImage",e.target.result);
  showBanner();
 };
 reader.readAsDataURL(event.target.files[0]);
}

function showBanner(){
 let imgData=localStorage.getItem("bannerImage");
 if(imgData){
  bannerArea.innerHTML=
  `<img src="${imgData}">`;
 }
}

showBanner();
</script>

</body>
</html>
