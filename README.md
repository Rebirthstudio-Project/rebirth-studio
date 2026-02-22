<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rebirth Studio</title>

<style>
:root{
 --bg:#0f0f18;
 --card:#1a1a2e;
 --text:#fff;
 --accent:#8a2be2;
 --glow:0 0 12px #8a2be2;
 transition:0.3s;
}
.light{
 --bg:#f5f5fb;
 --card:#ffffff;
 --text:#222;
 --accent:#ff4081;
 --glow:none;
}

body{
 margin:0;
 font-family:Arial;
 background:var(--bg);
 color:var(--text);
}

.topbar{
 display:flex;
 align-items:center;
 justify-content:space-between;
 padding:10px;
 background:var(--card);
 box-shadow:var(--glow);
 position:sticky;
 top:0;
 z-index:100;
}

.left-section{
 display:flex;
 align-items:center;
 gap:10px;
}

.center-section{
 flex:1;
 text-align:center;
}

.right-section{
 display:flex;
 align-items:center;
 gap:8px;
}

.menu-btn{
 font-size:24px;
 cursor:pointer;
}

.search{
 width:70%;
 max-width:250px;
 padding:6px;
 border-radius:20px;
 border:none;
}

button{
 padding:6px 10px;
 border:none;
 border-radius:20px;
 background:var(--accent);
 color:white;
 cursor:pointer;
 font-size:14px;
}

button:hover{
 box-shadow:var(--glow);
}

.sidebar{
 position:fixed;
 left:-250px;
 top:0;
 width:250px;
 height:100%;
 background:var(--card);
 padding-top:60px;
 transition:0.3s;
 overflow:auto;
}

.sidebar a{
 display:block;
 padding:12px;
 text-decoration:none;
 color:var(--text);
}

.sidebar a:hover{
 background:var(--accent);
 color:white;
}

.container{
 padding:15px;
}

.card{
 background:var(--card);
 padding:15px;
 border-radius:15px;
 margin-bottom:15px;
 box-shadow:var(--glow);
 animation:fade 0.3s ease;
}

@keyframes fade{
 from{opacity:0;transform:translateY(10px);}
 to{opacity:1;transform:translateY(0);}
}

img{
 width:100%;
 border-radius:10px;
 margin-top:10px;
}

.popup{
 display:none;
 position:fixed;
 top:50%;
 left:50%;
 transform:translate(-50%,-50%);
 background:var(--card);
 padding:20px;
 border-radius:15px;
 width:90%;
 max-width:350px;
 box-shadow:var(--glow);
 z-index:200;
}

input,textarea{
 width:100%;
 padding:8px;
 border-radius:10px;
 border:none;
 margin:5px 0;
}
</style>
</head>

<body>

<div class="sidebar" id="sidebar">
 <h3 style="padding-left:15px;">Categories</h3>
 <div id="categoryList"></div>
</div>

<div class="topbar">

 <div class="left-section">
   <span class="menu-btn" onclick="toggleMenu()">☰</span>
   <b>Rebirth Studio</b>
 </div>

 <div class="center-section">
   <input class="search" placeholder="Search..." oninput="search(this.value)">
 </div>

 <div class="right-section">
   <button onclick="toggleTheme()">🌙</button>
   <button id="loginBtn" onclick="openAuth()">Login</button>
   <button id="logoutBtn" style="display:none" onclick="logout()">Logout</button>
 </div>

</div>

<div class="container">
 <button id="addBtn" style="display:none" onclick="openPagePopup()">+ Add Page</button>
 <div id="contentArea"></div>
</div>

<!-- Auth -->
<div class="popup" id="authPopup">
 <h3>Login</h3>
 <input id="user" placeholder="Username">
 <input id="pass" type="password" placeholder="Password">
 <button onclick="login()">Login</button>
 <button onclick="closeAuth()">Cancel</button>
</div>

<!-- Page -->
<div class="popup" id="pagePopup">
 <h3>Create Page</h3>
 <input id="title" placeholder="Title">
 <textarea id="desc" placeholder="Description"></textarea>
 <input type="file" id="img">
 <button onclick="savePage()">Save</button>
 <button onclick="closePagePopup()">Cancel</button>
</div>

<script>
const OWNER_PASSWORD="Rebirth999";

let currentUser=null;
let data=JSON.parse(localStorage.getItem("rebirthData"))||{};
let currentCategory=null;

function saveAll(){
 localStorage.setItem("rebirthData",JSON.stringify(data));
}

function toggleMenu(){
 sidebar.style.left=sidebar.style.left==="0px"?"-250px":"0px";
}

function toggleTheme(){
 document.body.classList.toggle("light");
}

function openAuth(){authPopup.style.display="block";}
function closeAuth(){authPopup.style.display="none";}

function login(){
 if(user.value==="owner"&&pass.value===OWNER_PASSWORD){
  currentUser="owner";
  addBtn.style.display="block";
 }
 loginBtn.style.display="none";
 logoutBtn.style.display="block";
 closeAuth();
}

function logout(){
 currentUser=null;
 loginBtn.style.display="block";
 logoutBtn.style.display="none";
 addBtn.style.display="none";
}

function openPagePopup(){
 if(!currentUser)return alert("Login first");
 pagePopup.style.display="block";
}

function closePagePopup(){
 pagePopup.style.display="none";
}

function savePage(){
 let file=img.files[0];
 let reader=new FileReader();
 reader.onload=function(e){
  if(!data["General"])data["General"]={items:[]};
  data["General"].items.push({
   title:title.value,
   desc:desc.value,
   img:file?e.target.result:null
  });
  saveAll();
  display();
  closePagePopup();
 };
 if(file)reader.readAsDataURL(file);
 else reader.onload({target:{result:null}});
}

function display(){
 contentArea.innerHTML="";
 if(!data["General"])return;
 data["General"].items.forEach(item=>{
  contentArea.innerHTML+=`
   <div class="card">
    <h2>${item.title}</h2>
    <p>${item.desc}</p>
    ${item.img?`<img src="${item.img}">`:""}
   </div>`;
 });
}

function search(text){
 if(!data["General"])return;
 contentArea.innerHTML="";
 data["General"].items
 .filter(p=>p.title.toLowerCase().includes(text.toLowerCase()))
 .forEach(p=>{
  contentArea.innerHTML+=`
   <div class="card">
    <h2>${p.title}</h2>
    <p>${p.desc}</p>
   </div>`;
 });
}

display();
</script>

</body>
</html>
