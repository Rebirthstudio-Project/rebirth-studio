<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rebirth Studio</title>

<style>
:root{
 --bg:#0b0f1a;
 --card:rgba(255,255,255,0.05);
 --text:#ffffff;
 --accent:#9d4edd;
 --blur:blur(12px);
 --border:rgba(255,255,255,0.08);
}

.light{
 --bg:#f4f6fb;
 --card:rgba(255,255,255,0.7);
 --text:#111;
 --accent:#ff4081;
 --border:rgba(0,0,0,0.08);
}

*{box-sizing:border-box;}
body{
 margin:0;
 font-family:Segoe UI, sans-serif;
 background:var(--bg);
 color:var(--text);
 transition:0.3s;
}

/* TOP NAV */
.topbar{
 display:flex;
 justify-content:space-between;
 align-items:center;
 padding:10px 15px;
 backdrop-filter:var(--blur);
 background:var(--card);
 border-bottom:1px solid var(--border);
 position:sticky;
 top:0;
 z-index:1000;
}

.left{
 display:flex;
 align-items:center;
 gap:10px;
}

.logo{
 font-weight:bold;
 font-size:18px;
}

.menu-btn{
 font-size:22px;
 cursor:pointer;
}

/* RIGHT SIDE */
.right{
 display:flex;
 align-items:center;
 gap:12px;
}

.icon{
 position:relative;
 cursor:pointer;
 font-size:20px;
}

.badge{
 position:absolute;
 top:-5px;
 right:-6px;
 background:red;
 color:white;
 font-size:10px;
 padding:2px 5px;
 border-radius:10px;
}

/* SUB NAV */
.subnav{
 display:flex;
 justify-content:center;
 gap:20px;
 padding:8px;
 backdrop-filter:var(--blur);
 background:var(--card);
 border-bottom:1px solid var(--border);
 font-size:14px;
}

.subnav a{
 text-decoration:none;
 color:var(--text);
 opacity:0.8;
}
.subnav a:hover{
 opacity:1;
 color:var(--accent);
}

/* SIDEBAR */
.sidebar{
 position:fixed;
 top:0;
 left:0;
 height:100%;
 width:260px;
 background:var(--card);
 backdrop-filter:var(--blur);
 border-right:1px solid var(--border);
 transform:translateX(-100%);
 transition:transform 0.4s cubic-bezier(.77,0,.18,1);
 padding-top:70px;
 z-index:999;
}

.sidebar.open{
 transform:translateX(0);
}

.sidebar a{
 display:block;
 padding:12px 20px;
 text-decoration:none;
 color:var(--text);
}
.sidebar a:hover{
 background:var(--accent);
 color:white;
}

/* CONTENT */
.container{
 padding:20px;
}

.card{
 background:var(--card);
 backdrop-filter:var(--blur);
 padding:15px;
 border-radius:15px;
 margin-bottom:15px;
 border:1px solid var(--border);
 animation:fadeUp 0.4s ease;
}

@keyframes fadeUp{
 from{opacity:0;transform:translateY(20px);}
 to{opacity:1;transform:translateY(0);}
}

button{
 padding:6px 12px;
 border:none;
 border-radius:20px;
 background:var(--accent);
 color:white;
 cursor:pointer;
}

/* SEARCH */
.search{
 padding:6px 10px;
 border-radius:20px;
 border:none;
 width:150px;
}

/* POPUP */
.popup{
 display:none;
 position:fixed;
 top:50%;
 left:50%;
 transform:translate(-50%,-50%);
 background:var(--card);
 backdrop-filter:var(--blur);
 padding:20px;
 border-radius:15px;
 width:90%;
 max-width:350px;
 border:1px solid var(--border);
 z-index:2000;
}

input{
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
 <a href="#">Dashboard</a>
 <a href="#">Characters</a>
 <a href="#">World</a>
 <a href="#">Settings</a>
</div>

<!-- NAVBAR 1 -->
<div class="topbar">
 <div class="left">
   <span class="menu-btn" onclick="toggleMenu()">☰</span>
   <span class="logo">Rebirth Studio</span>
 </div>

 <div class="right">
   <input class="search" placeholder="Search...">
   <span class="icon" onclick="showNotif()">🔔
     <span class="badge" id="notifCount">3</span>
   </span>
   <span class="icon" onclick="toggleTheme()">🌙</span>
   <button onclick="openLogin()">Login</button>
 </div>
</div>

<!-- NAVBAR 2 -->
<div class="subnav">
 <a href="#">Home</a>
 <a href="#">Explore</a>
 <a href="#">Trending</a>
 <a href="#">Community</a>
</div>

<div class="container">
 <div class="card">
   <h2>Welcome to Rebirth Studio</h2>
   <p>Premium fandom system UI with smooth animation.</p>
 </div>
</div>

<!-- LOGIN POPUP -->
<div class="popup" id="loginPopup">
 <h3>Login</h3>
 <input placeholder="Username">
 <input type="password" placeholder="Password">
 <button onclick="closeLogin()">Login</button>
</div>

<script>
function toggleMenu(){
 sidebar.classList.toggle("open");
}

function toggleTheme(){
 document.body.classList.toggle("light");
}

function openLogin(){
 loginPopup.style.display="block";
}
function closeLogin(){
 loginPopup.style.display="none";
}

function showNotif(){
 alert("You have 3 notifications.");
}
</script>

</body>
</html>
