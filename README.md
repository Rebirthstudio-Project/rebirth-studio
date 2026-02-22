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
  left:-260px;
  top:0;
  width:260px;
  height:100%;
  background:#141425;
  padding-top:60px;
  transition:0.3s;
  overflow:auto;
}
.sidebar h3{padding-left:12px;}
.sidebar a{
  display:block;
  padding:10px;
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
  z-index:1000;
}
</style>
</head>
<body>

<div class="sidebar" id="sidebar">
  <h3>Categories</h3>
  <div id="categoryList"></div>
  <div id="ownerCategoryBtn" style="display:none;padding:10px;">
    <button onclick="openCategoryForm()">+ Add Category</button>
  </div>
</div>

<div class="topbar">
  <div>
    <span class="menu-btn" onclick="toggleMenu()">☰</span>
    <b>Rebirth Studio Wiki</b>
    <span id="ownerLabel"></span>
  </div>
  <div>
    <input type="text" id="searchBox" placeholder="Search..." oninput="searchContent()">
    <button onclick="ownerLogin()">Owner</button>
    <button id="addContentBtn" onclick="openContentForm()" style="display:none;">+ Add</button>
    <button id="logoutBtn" onclick="logout()" style="display:none;">Logout</button>
  </div>
</div>

<div class="container">
  <div id="contentArea"></div>
</div>

<!-- CATEGORY FORM -->
<div class="popup" id="categoryPopup">
  <h3>New Category</h3>
  <input type="text" id="categoryName" placeholder="Category Name">
  <button onclick="saveCategory()">Save</button>
  <button onclick="closeCategoryForm()">Cancel</button>
</div>

<!-- CONTENT FORM -->
<div class="popup" id="contentPopup">
  <h3 id="contentTitle">Add Content</h3>
  <input type="text" id="contentName" placeholder="Title">
  <input type="file" id="imageInput" accept="image/*">
  <textarea id="contentDesc" placeholder="Description"></textarea>
  <button onclick="saveContent()">Save</button>
  <button onclick="closeContentForm()">Cancel</button>
</div>

<script>
const OWNER_PASSWORD="Rebirth999";
let isOwner=false;
let currentCategory=null;
let editIndex=null;

let data=JSON.parse(localStorage.getItem("rebirthData"))||{};

function toggleMenu(){
  let s=document.getElementById("sidebar");
  s.style.left=s.style.left==="0px"?"-260px":"0px";
}

function ownerLogin(){
  let pass=prompt("Enter Owner Password:");
  if(pass===OWNER_PASSWORD){
    isOwner=true;
    ownerLabel.innerHTML=" 👑";
    addContentBtn.style.display="inline-block";
    logoutBtn.style.display="inline-block";
    ownerCategoryBtn.style.display="block";
    renderCategories();
  }else alert("Wrong Password");
}

function logout(){
  isOwner=false;
  ownerLabel.innerHTML="";
  addContentBtn.style.display="none";
  logoutBtn.style.display="none";
  ownerCategoryBtn.style.display="none";
  renderCategories();
}

function openCategoryForm(){
  categoryPopup.style.display="block";
}
function closeCategoryForm(){
  categoryPopup.style.display="none";
}

function saveCategory(){
  let name=categoryName.value;
  if(!name)return;
  if(!data[name]) data[name]=[];
  localStorage.setItem("rebirthData",JSON.stringify(data));
  renderCategories();
  closeCategoryForm();
}

function renderCategories(){
  categoryList.innerHTML="";
  for(let cat in data){
    categoryList.innerHTML+=`
      <a onclick="openCategory('${cat}')">${cat}</a>
    `;
  }
}

function openCategory(cat){
  currentCategory=cat;
  displayContent();
  toggleMenu();
}

function openContentForm(index=null){
  if(!currentCategory){alert("Select category first");return;}
  editIndex=index;
  contentPopup.style.display="block";
  if(index!==null){
    let item=data[currentCategory][index];
    contentName.value=item.name;
    contentDesc.value=item.desc;
  }else{
    contentName.value="";
    contentDesc.value="";
  }
}

function closeContentForm(){
  contentPopup.style.display="none";
}

function saveContent(){
  let file=imageInput.files[0];
  let reader=new FileReader();
  reader.onload=function(e){
    let imgData=file?e.target.result:(editIndex!==null?data[currentCategory][editIndex].img:null);
    let newItem={name:contentName.value,desc:contentDesc.value,img:imgData};
    if(editIndex!==null)
      data[currentCategory][editIndex]=newItem;
    else
      data[currentCategory].push(newItem);
    localStorage.setItem("rebirthData",JSON.stringify(data));
    displayContent();
    closeContentForm();
  };
  if(file)reader.readAsDataURL(file);
  else reader.onload({target:{result:null}});
}

function deleteContent(i){
  if(!isOwner)return;
  data[currentCategory].splice(i,1);
  localStorage.setItem("rebirthData",JSON.stringify(data));
  displayContent();
}

function displayContent(filter=""){
  contentArea.innerHTML="";
  if(!currentCategory)return;
  data[currentCategory].forEach((item,i)=>{
    if(item.name.toLowerCase().includes(filter.toLowerCase())){
      contentArea.innerHTML+=`
        <div class="card">
          <h2>${item.name}</h2>
          ${item.img?`<img src="${item.img}">`:""}
          <p>${item.desc}</p>
          ${isOwner?`
            <button onclick="openContentForm(${i})">Edit</button>
            <button onclick="deleteContent(${i})">Delete</button>
          `:""}
        </div>
      `;
    }
  });
}

function searchContent(){
  displayContent(searchBox.value);
}

renderCategories();
</script>

</body>
</html>
