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
 --glow:0 0 15px #8a2be2;
 transition:0.4s;
}
.light{
 --bg:#f6f6fb;
 --card:#ffffff;
 --text:#222;
 --accent:#ff4081;
 --glow:0 0 0 transparent;
}
body{
 margin:0;
 font-family:Arial;
 background:var(--bg);
 color:var(--text);
 transition:0.4s;
}
.topbar{
 display:flex;
 justify-content:space-between;
 align-items:center;
 padding:10px;
 background:var(--card);
 box-shadow:var(--glow);
 position:sticky;
 top:0;
 z-index:100;
}
.menu-btn{font-size:24px;cursor:pointer;}
button{
 padding:6px 10px;
 border:none;
 border-radius:20px;
 background:var(--accent);
 color:white;
 cursor:pointer;
 margin:2px;
}
button:hover{box-shadow:var(--glow);}
.sidebar{
 position:fixed;
 left:-260px;
 top:0;
 width:260px;
 height:100%;
 background:var(--card);
 padding-top:60px;
 transition:0.3s;
 overflow:auto;
 box-shadow:var(--glow);
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
.container{padding:15px;}
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
img{width:100%;border-radius:12px;margin-top:10px;}
input,textarea{
 width:100%;
 padding:8px;
 border-radius:10px;
 border:none;
 margin:5px 0;
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
.badge{
 background:gold;
 color:black;
 padding:2px 6px;
 border-radius:6px;
 font-size:12px;
}
.search{width:120px;}
.theme-toggle{
 width:50px;
 height:25px;
 background:#ccc;
 border-radius:20px;
 position:relative;
 cursor:pointer;
}
.theme-toggle::after{
 content:"";
 width:20px;
 height:20px;
 background:white;
 border-radius:50%;
 position:absolute;
 top:2.5px;
 left:3px;
 transition:0.4s;
}
.light .theme-toggle{background:var(--accent);}
.light .theme-toggle::after{left:26px;}
</style>
</head>

<body>

<div class="sidebar" id="sidebar">
 <h3 style="padding-left:15px;">Categories</h3>
 <div id="categoryList"></div>
 <div id="ownerTools" style="display:none;padding:10px;">
  <button onclick="openCategoryPopup()">+ Category</button>
 </div>
</div>

<div class="topbar">
 <div>
  <span class="menu-btn" onclick="toggleMenu()">☰</span>
  <b>Rebirth Studio</b>
  <span id="roleLabel"></span>
 </div>
 <div>
  <input class="search" placeholder="Search" oninput="search(this.value)">
  <div class="theme-toggle" onclick="toggleTheme()" style="display:inline-block;"></div>
  <button onclick="openAuth()">Login</button>
  <button id="logoutBtn" style="display:none" onclick="logout()">Logout</button>
  <button id="addBtn" style="display:none" onclick="openPagePopup()">+ Add</button>
 </div>
</div>

<div class="container">
 <div id="contentArea"></div>
</div>

<!-- Auth -->
<div class="popup" id="authPopup">
 <h3>Login / Register</h3>
 <input id="user" placeholder="Username">
 <input id="pass" type="password" placeholder="Password">
 <button onclick="login()">Login</button>
 <button onclick="register()">Register</button>
 <button onclick="closeAuth()">Cancel</button>
</div>

<!-- Category -->
<div class="popup" id="catPopup">
 <h3>Create Category</h3>
 <input id="catName" placeholder="Category Name">
 <button onclick="saveCategory()">Save</button>
 <button onclick="closeCategoryPopup()">Cancel</button>
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

let users=JSON.parse(localStorage.getItem("users"))||{};
let data=JSON.parse(localStorage.getItem("wikiData"))||{};
let currentUser=null;
let currentCategory=null;

function saveAll(){
 localStorage.setItem("users",JSON.stringify(users));
 localStorage.setItem("wikiData",JSON.stringify(data));
}

function toggleMenu(){
 sidebar.style.left=sidebar.style.left==="0px"?"-260px":"0px";
}

function toggleTheme(){
 document.body.classList.toggle("light");
}

function openAuth(){authPopup.style.display="block";}
function closeAuth(){authPopup.style.display="none";}

function register(){
 if(!user.value||!pass.value)return alert("Fill all");
 users[user.value]={password:pass.value,role:"member"};
 saveAll();
 alert("Registered!");
}

function login(){
 if(user.value==="owner"&&pass.value===OWNER_PASSWORD){
  currentUser={username:"owner",role:"owner"};
 }else if(users[user.value]&&users[user.value].password===pass.value){
  currentUser={username:user.value,role:"member"};
 }else return alert("Wrong login");

 roleLabel.innerHTML=currentUser.role==="owner"?
  "<span class='badge'>OWNER</span>":
  "<span class='badge'>MEMBER</span>";

 addBtn.style.display="inline-block";
 logoutBtn.style.display="inline-block";
 if(currentUser.role==="owner")
  ownerTools.style.display="block";

 closeAuth();
}

function logout(){
 currentUser=null;
 roleLabel.innerHTML="";
 addBtn.style.display="none";
 logoutBtn.style.display="none";
 ownerTools.style.display="none";
}

function openCategoryPopup(){catPopup.style.display="block";}
function closeCategoryPopup(){catPopup.style.display="none";}

function saveCategory(){
 if(!currentUser||currentUser.role!=="owner")
  return alert("Owner only");
 if(!catName.value)return;
 data[catName.value]={items:[]};
 saveAll();
 renderCategories();
 closeCategoryPopup();
}

function renderCategories(){
 categoryList.innerHTML="";
 for(let c in data){
  categoryList.innerHTML+=
   `<a onclick="openCategory('${c}')">${c}</a>`;
 }
}
function openCategory(c){
 currentCategory=c;
 display();
 toggleMenu();
}

function openPagePopup(){
 if(!currentUser)return alert("Login first");
 if(!currentCategory)return alert("Select category");
 pagePopup.style.display="block";
}
function closePagePopup(){pagePopup.style.display="none";}

function savePage(){
 let reader=new FileReader();
 let file=img.files[0];
 reader.onload=function(e){
  data[currentCategory].items.push({
   title:title.value,
   desc:desc.value,
   img:file?e.target.result:null,
   owner:currentUser.username
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
 if(!currentCategory)return;
 data[currentCategory].items.forEach((item,i)=>{
  let del="";
  if(currentUser&&(currentUser.role==="owner"||currentUser.username===item.owner)){
   del=`<button onclick="deletePage(${i})">Delete</button>`;
  }
  contentArea.innerHTML+=`
   <div class="card">
    <h2>${item.title}</h2>
    <p>${item.desc}</p>
    ${item.img?`<img src="${item.img}">`:""}
    <small>By ${item.owner}</small><br>
    ${del}
   </div>
  `;
 });
}

function deletePage(i){
 data[currentCategory].items.splice(i,1);
 saveAll();
 display();
}

function search(text){
 if(!currentCategory)return;
 contentArea.innerHTML="";
 data[currentCategory].items
 .filter(p=>p.title.toLowerCase().includes(text.toLowerCase()))
 .forEach(p=>{
  contentArea.innerHTML+=`
   <div class="card">
    <h2>${p.title}</h2>
    <p>${p.desc}</p>
   </div>`;
 });
}

renderCategories();
</script>

</body>
</html>
