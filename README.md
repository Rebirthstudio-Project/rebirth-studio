<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rebirth Studio Wiki</title>

<style>
body{margin:0;font-family:Arial;background:#0f0f18;color:white;}
.topbar{
  background:#1a1a2e;
  padding:12px;
  display:flex;
  justify-content:space-between;
  align-items:center;
}
.menu-btn{font-size:22px;cursor:pointer;}
.sidebar{
  position:fixed;
  left:-250px;
  top:0;
  width:250px;
  height:100%;
  background:#141425;
  padding-top:60px;
  transition:0.3s;
}
.sidebar a{
  display:block;
  padding:12px;
  text-decoration:none;
  color:white;
}
.sidebar a:hover{background:#8a2be2;}
.container{padding:20px;}
.card{
  background:#1a1a2e;
  padding:15px;
  border-radius:10px;
  margin-bottom:15px;
}
button{
  padding:6px 10px;
  border:none;
  border-radius:6px;
  background:#8a2be2;
  color:white;
  cursor:pointer;
  margin:3px;
}
input,textarea{
  width:100%;
  padding:6px;
  margin:5px 0;
  border-radius:6px;
  border:none;
}
img{max-width:100%;border-radius:8px;margin:10px 0;}
.popup{
  display:none;
  position:fixed;
  top:50%;
  left:50%;
  transform:translate(-50%,-50%);
  background:#1a1a2e;
  padding:20px;
  border-radius:10px;
  width:300px;
}
</style>
</head>

<body>

<div class="sidebar" id="sidebar">
  <a onclick="showPage('home')">Home</a>
  <a onclick="showPage('characters')">Characters</a>
</div>

<div class="topbar">
  <div>
    <span class="menu-btn" onclick="toggleMenu()">☰</span>
    <b>Rebirth Studio Wiki</b>
    <span id="ownerLabel"></span>
  </div>
  <div>
    <input type="text" id="searchBox" placeholder="Search..." oninput="searchCharacter()">
    <button onclick="ownerLogin()">Owner</button>
    <button id="createBtn" onclick="openForm()" style="display:none;">+ Create</button>
    <button id="logoutBtn" onclick="logout()" style="display:none;">Logout</button>
  </div>
</div>

<div class="container">

  <div id="homePage">
    <h2>Welcome to Rebirth Studio</h2>
    <p>จักรวาลใหม่กำลังเริ่มต้น...</p>
  </div>

  <div id="charactersPage" style="display:none;">
    <div id="characterList"></div>
  </div>

</div>

<div class="popup" id="formPopup">
  <h3 id="formTitle">Create Character</h3>
  <input type="text" id="name" placeholder="Name">
  <input type="text" id="title" placeholder="Title">
  <input type="file" id="imageInput" accept="image/*">
  <textarea id="desc" placeholder="Description"></textarea>
  <button onclick="saveCharacter()">Save</button>
  <button onclick="closeForm()">Cancel</button>
</div>

<script>
const OWNER_PASSWORD="Rebirth999";
let isOwner=false;
let editIndex=null;
let characters=JSON.parse(localStorage.getItem("rebirthChars"))||[];

function toggleMenu(){
  let s=document.getElementById("sidebar");
  s.style.left=s.style.left==="0px"?"-250px":"0px";
}

function showPage(page){
  document.getElementById("homePage").style.display=page==="home"?"block":"none";
  document.getElementById("charactersPage").style.display=page==="characters"?"block":"none";
  toggleMenu();
}

function ownerLogin(){
  let pass=prompt("Enter Owner Password:");
  if(pass===OWNER_PASSWORD){
    isOwner=true;
    document.getElementById("createBtn").style.display="inline-block";
    document.getElementById("logoutBtn").style.display="inline-block";
    document.getElementById("ownerLabel").innerHTML=" 👑";
    displayCharacters();
  }else alert("Wrong Password");
}

function logout(){
  isOwner=false;
  document.getElementById("createBtn").style.display="none";
  document.getElementById("logoutBtn").style.display="none";
  document.getElementById("ownerLabel").innerHTML="";
  displayCharacters();
}

function openForm(index=null){
  editIndex=index;
  document.getElementById("formPopup").style.display="block";
  if(index!==null){
    let c=characters[index];
    name.value=c.name;
    title.value=c.title;
    desc.value=c.desc;
  }else{
    name.value="";title.value="";desc.value="";
  }
}

function closeForm(){formPopup.style.display="none";}

function saveCharacter(){
  let file=imageInput.files[0];
  let reader=new FileReader();
  reader.onload=function(e){
    let imgData=file?e.target.result:(editIndex!==null?characters[editIndex].img:null);
    let newChar={name:name.value,title:title.value,desc:desc.value,img:imgData};
    if(editIndex!==null)characters[editIndex]=newChar;
    else characters.push(newChar);
    localStorage.setItem("rebirthChars",JSON.stringify(characters));
    displayCharacters();
    closeForm();
  };
  if(file)reader.readAsDataURL(file);
  else reader.onload({target:{result:null}});
}

function deleteCharacter(i){
  if(!isOwner)return;
  characters.splice(i,1);
  localStorage.setItem("rebirthChars",JSON.stringify(characters));
  displayCharacters();
}

function displayCharacters(filter=""){
  let list=document.getElementById("characterList");
  list.innerHTML="";
  characters.forEach((c,i)=>{
    if(c.name.toLowerCase().includes(filter.toLowerCase())){
      list.innerHTML+=`
      <div class="card">
        <h2>${c.name}</h2>
        <p><b>${c.title}</b></p>
        ${c.img?`<img src="${c.img}">`:""}
        <p>${c.desc}</p>
        ${isOwner?`
          <button onclick="openForm(${i})">Edit</button>
          <button onclick="deleteCharacter(${i})">Delete</button>`:""}
      </div>`;
    }
  });
}

function searchCharacter(){
  displayCharacters(searchBox.value);
}

displayCharacters();
</script>

</body>
</html>
