<!DOCTYPE html><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Editya — Portfolio</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
body{margin:0;background:#0d0d0d;color:#fff;font-family:Poppins}
.showcase{max-width:900px;margin:60px auto;padding:20px}
video{width:100%;border-radius:16px}
.info{margin-top:10px}
.thumbs{display:flex;gap:10px;margin-top:15px;overflow:auto}
.thumbs video{width:120px;height:70px;border-radius:10px;opacity:.6;cursor:pointer}
.thumbs video:hover{opacity:1;transform:scale(1.05)}
</style>
</head>
<body><div class="showcase">
  <video id="main" muted autoplay loop playsinline></video>
  <div class="info">
    <h2 id="title"></h2>
  </div>
  <div class="thumbs" id="list"></div>
</div><script>
const videos=[
 {src:"lv_0_20260411181235.mp4",title:"Reel Edit"},
 {src:"lv_0_20260411180058.mp4",title:"YouTube Edit"},
 {src:"lv_0_20260331191240(1).mp4",title:"Cinematic Edit"}
];

let i=0;
const main=document.getElementById('main');
const title=document.getElementById('title');
const list=document.getElementById('list');

function load(x){
 main.src=videos[x].src;
 title.innerText=videos[x].title;
 i=x;
}

setInterval(()=>{
 i=(i+1)%videos.length;
 load(i);
},5000);

videos.forEach((v,index)=>{
 const t=document.createElement('video');
 t.src=v.src;
 t.muted=true;
 t.onmouseenter=()=>t.play();
 t.onmouseleave=()=>{t.pause();t.currentTime=0};
 t.onclick=()=>load(index);
 list.appendChild(t);
});

load(0);
</script></body>
</html>
