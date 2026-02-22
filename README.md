<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rebirth Studio Wiki</title>

<style>
body{
  margin:0;
  font-family:Arial, sans-serif;
  background:#0f0f18;
  color:#eee;
}

/* TOP BAR */
.topbar{
  display:flex;
  align-items:center;
  justify-content:space-between;
  background:#1a1a2e;
  padding:12px 20px;
  position:fixed;
  width:100%;
  top:0;
  z-index:1000;
}

.logo{
  font-weight:bold;
  font-size:18px;
}

.search input{
  padding:6px;
  border-radius:6px;
  border:none;
}

/* LAYOUT */
.container{
  display:flex;
  margin-top:60px;
}

/* SIDEBAR */
.sidebar{
  width:220px;
  background:#141425;
  height:100vh;
  padding-top:20px;
  position:fixed;
}

.sidebar a{
  display:block;
  padding:10px 15px;
  color:#ccc;
  text-decoration:none;
}

.sidebar a:hover{
  background:#8a2be2;
  color:white;
}

/* MAIN CONTENT */
.main{
  margin-left:220px;
  padding:20px;
  max-width:900px;
}

/* INFOBOX */
.infobox{
  float:right;
  width:250px;
  background:#1a1a2e;
  padding:15px;
  border-radius:10px;
  margin:10px;
}

.infobox h3{
  margin-top:0;
  text-align:center;
}

.section{
  margin-bottom:20px;
}

h1,h2{
  border-bottom:1px solid #444;
  padding-bottom:5px;
}
</style>
</head>

<body>

<div class="topbar">
  <div class="logo">Rebirth Studio Wiki</div>
  <div class="search">
    <input type="text" placeholder="Search Wiki...">
  </div>
</div>

<div class="container">

  <div class="sidebar">
    <a href="#">Main Page</a>
    <a href="#">Characters</a>
    <a href="#">World</a>
    <a href="#">Gallery</a>
    <a href="#">News</a>
    <a href="#">Community</a>
  </div>

  <div class="main">

    <h1>Ken</h1>

    <div class="infobox">
      <h3>Ken</h3>
      <p><b>Title:</b> Celestial Prodigy</p>
      <p><b>Affiliation:</b> Royal Academy</p>
      <p><b>Power:</b> Sun Flame Core</p>
      <p><b>Status:</b> Alive</p>
    </div>

    <div class="section">
      <h2>Overview</h2>
      <p>
        Ken เป็นตัวเอกของจักรวาล Rebirth Studio 
        ผู้ครอบครองพลังสุริยะขั้นสูงและมีเส้นทางสู่การเป็นจักรพรรดิแห่ง Celestial Realm
      </p>
    </div>

    <div class="section">
      <h2>Abilities</h2>
      <ul>
        <li>Solar Burst</li>
        <li>Heavenly Ascension</li>
        <li>Void Resistance</li>
      </ul>
    </div>

    <div class="section">
      <h2>Story</h2>
      <p>
        Ken เริ่มต้นจากนักเรียนธรรมดาใน Royal Academy 
        ก่อนค้นพบพลัง Sun Flame Core ที่เปลี่ยนชะตาของเขาไปตลอดกาล
      </p>
    </div>

  </div>

</div>

</body>
</html>
