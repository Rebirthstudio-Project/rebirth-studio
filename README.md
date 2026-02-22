<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rebirth Studio</title>

<style>
body{
  margin:0;
  font-family:Arial, sans-serif;
  background:#0a0a12;
  color:white;
}

/* TOPBAR */
.topbar{
  display:flex;
  justify-content:space-between;
  align-items:center;
  background:#111;
  padding:12px 15px;
  position:fixed;
  width:100%;
  top:0;
  z-index:1000;
}

.logo{
  font-weight:bold;
}

.icon{
  font-size:20px;
  cursor:pointer;
  margin-left:15px;
}

/* SIDEMENU */
.sidemenu{
  position:fixed;
  left:-250px;
  top:0;
  width:250px;
  height:100%;
  background:#1a1a2e;
  padding-top:60px;
  transition:0.3s;
  z-index:999;
}

.sidemenu a{
  display:block;
  padding:12px 20px;
  text-decoration:none;
  color:white;
}

.sidemenu a:hover{
  background:#8a2be2;
}

/* SEARCH */
.searchbox{
  display:none;
  padding:10px;
  background:#111;
  margin-top:55px;
}

.searchbox input{
  width:100%;
  padding:8px;
  border-radius:8px;
  border:none;
}

/* CONTENT */
.content{
  padding:80px 20px 20px 20px;
}

.page{
  display:none;
}

.active{
  display:block;
}

/* LOGIN POPUP */
.popup{
  display:none;
  position:fixed;
  top:50%;
  left:50%;
  transform:translate(-50%,-50%);
  background:#1a1a2e;
  padding:20px;
  border-radius:15px;
  z-index:2000;
  width:250px;
}

.popup input{
  width:100%;
  padding:8px;
  margin:5px 0;
  border-radius:8px;
  border:none;
}

.popup button{
  width:100%;
  padding:8px;
  background:#8a2be2;
  border:none;
  border-radius:8px;
  color:white;
  cursor:pointer;
}
</style>
</head>

<body>

<div class="sidemenu" id="menu">
  <a onclick="showPage('home')">Home</a>
  <a onclick="showPage('characters')">Characters</a>
  <a onclick="showPage('world')">World</a>
  <a onclick="showPage('gallery')">Gallery</a>
  <a onclick="showPage('news')">News</a>
  <a onclick="showPage('community')">Community</a>
</div>

<div class="topbar">
  <div>
    <span class="icon" onclick="toggleMenu()">☰</span>
    <span class="logo">Rebirth Studio</span>
  </div>
  <div>
    <span class="icon" onclick="toggleSearch()">🔍</span>
    <span class="icon" onclick="openLogin()">👤</span>
  </div>
</div>

<div class="searchbox" id="search">
  <input type="text" placeholder="Search...">
</div>

<div class="content">

  <div id="home" class="page active">
    <h2>Welcome</h2>
    <p>จักรวาล Rebirth Studio เริ่มต้นใหม่ทุกมิติ</p>
  </div>

  <div id="characters" class="page">
    <h2>Characters</h2>
    <p>Ken • Feilong • Sun Void</p>
  </div>

  <div id="world" class="page">
    <h2>World</h2>
    <p>Celestial Realm • Royal Academy • Void Dimension</p>
  </div>

  <div id="gallery" class="page">
    <h2>Gallery</h2>
    <p>Artwork & Illustrations</p>
  </div>

  <div id="news" class="page">
    <h2>News</h2>
    <p>Season ใหม่กำลังพัฒนา</p>
  </div>

  <div id="community" class="page">
    <h2>Community</h2>
    <p>Discord • Instagram • Fandom Wiki</p>
  </div>

</div>

<!-- LOGIN POPUP -->
<div class="popup" id="loginPopup">
  <h3>Login</h3>
  <input type="text" placeholder="Username">
  <input type="password" placeholder="Password">
  <button onclick="closeLogin()">Login</button>
</div>

<script>
function toggleMenu(){
  let menu=document.getElementById("menu");
  menu.style.left = menu.style.left==="0px" ? "-250px":"0px";
}

function toggleSearch(){
  let s=document.getElementById("search");
  s.style.display = s.style.display==="block" ? "none":"block";
}

function showPage(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
  document.getElementById(id).classList.add("active");
  toggleMenu();
}

function openLogin(){
  document.getElementById("loginPopup").style.display="block";
}

function closeLogin(){
  document.getElementById("loginPopup").style.display="none";
}
</script>

</body>
</html>
