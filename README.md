<!DOCTYPE html>
<html>
<head>
  <title>Rebirth Studio Fandom</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body{
      margin:0;
      font-family:sans-serif;
      background:linear-gradient(135deg,#000,#200033,#000);
      color:white;
    }
    header{
      background:#111;
      padding:15px;
      text-align:center;
      font-size:22px;
      font-weight:bold;
    }
    nav{
      display:flex;
      flex-wrap:wrap;
      justify-content:center;
      background:#1a1a1a;
      padding:10px;
    }
    nav button{
      margin:5px;
      padding:8px 12px;
      border:none;
      border-radius:8px;
      background:#8a2be2;
      color:white;
      cursor:pointer;
    }
    section{
      padding:20px;
      display:none;
    }
    .active{
      display:block;
    }
    .box{
      background:#111;
      padding:15px;
      border-radius:15px;
      margin:10px 0;
    }
    input,button{
      padding:8px;
      margin:5px;
      border-radius:8px;
      border:none;
    }
    .hidden{display:none;}
  </style>
</head>
<body>

<header>Rebirth Studio Fandom</header>

<div id="loginArea">

  <div class="box">
    <h3>👑 Creator Login</h3>
    <input type="password" id="creatorPass" placeholder="Creator Password">
    <button onclick="creatorLogin()">Login</button>
  </div>

  <div class="box">
    <h3>🌙 Member Login</h3>
    <input type="text" id="user" placeholder="Username">
    <input type="password" id="pass" placeholder="Password">
    <button onclick="memberLogin()">Login</button>
  </div>

</div>

<div id="site" class="hidden">

<nav>
  <button onclick="showPage('home')">Home</button>
  <button onclick="showPage('characters')">Characters</button>
  <button onclick="showPage('world')">World</button>
  <button onclick="showPage('gallery')">Gallery</button>
  <button onclick="showPage('news')">News</button>
  <button onclick="showPage('projects')">Projects</button>
  <button onclick="showPage('community')">Community</button>
  <button onclick="showPage('contact')">Contact</button>
  <button id="adminBtn" class="hidden" onclick="showPage('admin')">Admin</button>
  <button onclick="logout()">Logout</button>
</nav>

<section id="home" class="active">
  <h2>Welcome</h2>
  <div class="box">จักรวาล Rebirth Studio เริ่มต้นใหม่ทุกมิติ</div>
</section>

<section id="characters">
  <h2>Characters</h2>
  <div class="box">Ken - Fallen Seraph</div>
  <div class="box">Feilong - Dragon Hybrid</div>
  <div class="box">Sun Void - Parasite Variant</div>
</section>

<section id="world">
  <h2>World & Lore</h2>
  <div class="box">Celestial Realm</div>
  <div class="box">Royal Academy</div>
  <div class="box">Void Dimension</div>
</section>

<section id="gallery">
  <h2>Gallery</h2>
  <div class="box">Artwork & Illustrations</div>
</section>

<section id="news">
  <h2>News</h2>
  <div class="box">Season ใหม่กำลังพัฒนา</div>
</section>

<section id="projects">
  <h2>Projects</h2>
  <div class="box">Royal Academy of the Fallen Line</div>
</section>

<section id="community">
  <h2>Community</h2>
  <div class="box">Discord | Instagram | Fandom Wiki</div>
</section>

<section id="contact">
  <h2>Contact</h2>
  <div class="box">Email: rebirthstudio@gmail.com</div>
</section>

<section id="admin">
  <h2>Creator Admin Panel</h2>
  <div class="box">คุณสามารถแก้ไขเว็บไซต์ผ่าน GitHub ได้ทั้งหมด</div>
</section>

</div>

<script>

const CREATOR_SECRET = "Rebirth999";

function creatorLogin(){
  const pass = document.getElementById("creatorPass").value;
  if(pass === CREATOR_SECRET){
    enterSite(true);
  }else{
    alert("รหัสไม่ถูกต้อง");
  }
}

function memberLogin(){
  const user = document.getElementById("user").value;
  const pass = document.getElementById("pass").value;
  if(user && pass){
    enterSite(false);
  }else{
    alert("กรอกข้อมูลให้ครบ");
  }
}

function enterSite(isCreator){
  document.getElementById("loginArea").classList.add("hidden");
  document.getElementById("site").classList.remove("hidden");
  if(isCreator){
    document.getElementById("adminBtn").classList.remove("hidden");
  }
}

function showPage(id){
  document.querySelectorAll("section").forEach(sec=>{
    sec.classList.remove("active");
  });
  document.getElementById(id).classList.add("active");
}

function logout(){
  location.reload();
}

</script>

</body>
</html>
