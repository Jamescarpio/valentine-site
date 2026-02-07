<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>For My Lablab ❤️</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=Poppins:wght@300;400&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box}

body{
  background:linear-gradient(to bottom,#4a0000,#7a0000,#b30000);
  font-family:'Poppins',sans-serif;
  color:#fff;
  text-align:center;
  min-height:100vh;
  overflow-x:hidden;
}

/* 🌟 GLOBAL GLOW */
h1,h2,p,img,.modal-content{
  text-shadow:0 0 12px rgba(255,200,200,.6);
}

/* 🎬 LOADING INTRO */
#intro{
  position:fixed;
  inset:0;
  background:#3a0000;
  display:flex;
  justify-content:center;
  align-items:center;
  flex-direction:column;
  z-index:999;
  animation:fadeOut 1.5s ease forwards;
  animation-delay:2.5s;
}

#intro h1{
  font-size:3rem;
  animation:pulse 2s infinite;
}

@keyframes pulse{
  0%{transform:scale(1)}
  50%{transform:scale(1.05)}
  100%{transform:scale(1)}
}

@keyframes fadeOut{
  to{opacity:0;visibility:hidden}
}

/* Titles */
h1{
  font-family:'Great Vibes',cursive;
  font-size:clamp(2.4rem,6vw,3.5rem);
  margin:25px 0;
}

/* ❤️ Heart Gallery */
.heart-gallery{
  position:relative;
  width:min(360px,90vw);
  height:min(320px,80vw);
  margin:30px auto;
}

.heart-gallery img{
  position:absolute;
  width:30%;
  aspect-ratio:1/1;
  object-fit:cover;
  border-radius:20px;
  cursor:pointer;
  transition:.3s;
  box-shadow:0 0 25px rgba(255,150,150,.7);
}

.heart-gallery img:active{transform:scale(1.08)}

.pos1{top:0;left:20%}
.pos2{top:0;right:20%}
.pos3{top:25%;left:0}
.pos4{top:25%;right:0}
.pos5{top:50%;left:20%}
.pos6{top:50%;right:20%}
.pos7{top:75%;left:35%}

/* 🌹 Falling flowers */
.flower{
  position:fixed;
  top:-10px;
  font-size:22px;
  pointer-events:none;
  animation:fall linear infinite;
  z-index:1;
}

@keyframes fall{
  to{transform:translateY(110vh) rotate(360deg);opacity:0}
}

/* 💗 Floating hearts */
.heart{
  position:fixed;
  bottom:-20px;
  font-size:18px;
  animation:float 6s linear infinite;
  opacity:.7;
  pointer-events:none;
}

@keyframes float{
  to{transform:translateY(-110vh);opacity:0}
}

/* Modal */
.modal{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.85);
  display:none;
  justify-content:center;
  align-items:center;
  z-index:10;
  padding:15px;
}

.modal-content{
  background:#fff;
  color:#8b0000;
  padding:20px;
  border-radius:22px;
  max-width:520px;
  width:100%;
  max-height:90vh;
  overflow-y:auto;
}

.modal-content img{
  width:100%;
  border-radius:18px;
  margin-bottom:15px;
}

/* 🔐 Secret Letter */
.secret{
  margin:60px 0;
}

.secret button{
  background:#ffb3b3;
  color:#5a0000;
  border:none;
  padding:14px 26px;
  border-radius:30px;
  font-size:1rem;
  cursor:pointer;
  box-shadow:0 0 20px rgba(255,200,200,.7);
}

#secretLetter{
  display:none;
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.85);
  z-index:20;
  justify-content:center;
  align-items:center;
  padding:20px;
}

#secretContent{
  background:#fff;
  color:#8b0000;
  padding:30px;
  border-radius:25px;
  max-width:500px;
}

/* Admin */
.admin{display:none;background:#fff;color:#8b0000;max-width:500px;margin:20px auto;padding:20px;border-radius:15px}
.delete-btn{margin-top:15px;background:#b00000;color:#fff;padding:8px 14px;border:none;border-radius:20px}
</style>
</head>

<body>

<!-- 🎬 INTRO -->
<div id="intro">
  <h1>For someone special ❤️</h1>
</div>

<h1>Happy Valentine’s Day ❤️</h1>
<p>Tap the memories 🌹</p>

<div class="heart-gallery" id="gallery"></div>

<!-- 🔐 FINAL LETTER -->
<div class="secret">
  <button onclick="openSecret()">One last thing…</button>
</div>

<div id="secretLetter" onclick="closeSecret()">
  <div id="secretContent">
    <h2>For You ❤️</h2>
    <p>
      No matter where life takes us,
      I hope you always remember this —
      someone believes in you deeply,
      quietly, and completely.
    </p>
  </div>
</div>

<!-- MODAL -->
<div class="modal" id="modal" onclick="closeModal()">
  <div class="modal-content" onclick="event.stopPropagation()">
    <img id="modalImg">
    <h2 id="modalTitle"></h2>
    <p id="modalText"></p>
    <button id="deleteBtn" class="delete-btn" onclick="deleteMemory()">Delete</button>
  </div>
</div>

<audio id="musicPlayer"></audio>

<script>
const gallery=document.getElementById("gallery");
const modal=document.getElementById("modal");
const modalImg=document.getElementById("modalImg");
const modalTitle=document.getElementById("modalTitle");
const modalText=document.getElementById("modalText");
const musicPlayer=document.getElementById("musicPlayer");
const deleteBtn=document.getElementById("deleteBtn");

let memories=JSON.parse(localStorage.getItem("memories"))||[];
let adminMode=false,currentIndex=null;
const pos=["pos1","pos2","pos3","pos4","pos5","pos6","pos7"];

function render(){
  gallery.innerHTML="";
  memories.slice(0,7).forEach((m,i)=>{
    const img=document.createElement("img");
    img.src=m.img; img.className=pos[i];
    img.onclick=()=>openModal(i);
    gallery.appendChild(img);
  });
}
render();

function openModal(i){
  const m=memories[i];
  currentIndex=i;
  modalImg.src=m.img;
  modalTitle.innerText=m.title;
  modalText.innerText=m.text;
  modal.style.display="flex";
  deleteBtn.style.display=adminMode?"block":"none";
  if(m.music){musicPlayer.src=m.music;musicPlayer.play();}
}
function closeModal(){modal.style.display="none";musicPlayer.pause();}

function deleteMemory(){
  if(confirm("Delete this memory?")){
    memories.splice(currentIndex,1);
    localStorage.setItem("memories",JSON.stringify(memories));
    closeModal();render();
  }
}

function openSecret(){document.getElementById("secretLetter").style.display="flex"}
function closeSecret(){document.getElementById("secretLetter").style.display="none"}

document.addEventListener("keydown",e=>{
  if(e.key==="a"){
    const pass=prompt("Admin password:");
    if(pass==="admin123"){adminMode=true;alert("Admin mode enabled")}
  }
});

/* Effects */
setInterval(()=>{
  const f=document.createElement("div");
  f.className="flower";
  f.innerText=Math.random()>.5?"🌹":"🌸";
  f.style.left=Math.random()*100+"vw";
  f.style.animationDuration=4+Math.random()*4+"s";
  document.body.appendChild(f);
  setTimeout(()=>f.remove(),8000);
},500);

setInterval(()=>{
  const h=document.createElement("div");
  h.className="heart";
  h.innerText="💗";
  h.style.left=Math.random()*100+"vw";
  document.body.appendChild(h);
  setTimeout(()=>h.remove(),8000);
},800);
</script>

</body>
</html>
