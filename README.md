<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Editya Portfolio</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

<style>

*{
  margin:0;
  padding:0;
  box-sizing:border-box;
  font-family:'Poppins',sans-serif;
}

body{
  background:#0d0d0d;
  color:#fff;
  overflow-x:hidden;
}

/* NAVBAR */
nav{
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:20px 80px;
  position:fixed;
  width:100%;
  backdrop-filter: blur(20px);
  background:rgba(0,0,0,0.3);
  z-index:100;
}

nav h1{
  font-weight:600;
  letter-spacing:1px;
}

nav ul{
  display:flex;
  gap:30px;
  list-style:none;
  opacity:0.8;
}

/* HERO */
.hero{
  height:100vh;
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  text-align:center;
  padding:0 20px;
}

.hero h2{
  font-size:3.5rem;
  font-weight:600;
  line-height:1.2;
}

.hero p{
  margin-top:15px;
  opacity:0.6;
  font-size:1.2rem;
}

.tagline{
  margin-top:10px;
  font-size:1rem;
  opacity:0.5;
  letter-spacing:1px;
}

.btn{
  margin-top:30px;
  padding:14px 28px;
  background:#fff;
  color:#000;
  border:none;
  border-radius:50px;
  cursor:pointer;
  transition:0.3s;
}

.btn:hover{
  transform:scale(1.05);
}

/* PORTFOLIO */
.portfolio{
  padding:100px 80px;
}

.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(300px,1fr));
  gap:30px;
}

.card{
  border-radius:20px;
  overflow:hidden;
  position:relative;
}

.card img{
  width:100%;
  transition:0.5s;
}

.card:hover img{
  transform:scale(1.08);
}

.overlay{
  position:absolute;
  top:0;
  left:0;
  width:100%;
  height:100%;
  display:flex;
  justify-content:center;
  align-items:center;
  background:rgba(0,0,0,0.5);
  opacity:0;
  transition:0.4s;
}

.card:hover .overlay{
  opacity:1;
}

/* SERVICES */
.services{
  padding:100px 80px;
  display:flex;
  gap:30px;
  flex-wrap:wrap;
}

.service{
  flex:1;
  background:#111;
  padding:30px;
  border-radius:20px;
  text-align:center;
  transition:0.3s;
}

.service:hover{
  transform:translateY(-10px);
}

/* CONTACT */
.contact{
  padding:100px 80px;
  text-align:center;
}

input, textarea{
  width:100%;
  padding:14px;
  margin:10px 0;
  border-radius:8px;
  border:none;
  background:#111;
  color:#fff;
}

/* FOOTER */
footer{
  padding:30px;
  text-align:center;
  opacity:0.5;
}

</style>
</head>

<body>

<nav>
  <h1>EDITYA</h1>
  <ul>
    <li>Home</li>
    <li>Work</li>
    <li>Services</li>
    <li>Contact</li>
  </ul>
</nav>

<section class="hero">
  <h2>I Edit Videos That Grab Attention</h2>
  <p>Video Editing • Thumbnail Design • Reels & Shorts</p>

  <!-- 🔥 YOUR LINE -->
  <div class="tagline">I’m obsessed with perfection.</div>

  <button class="btn">View My Work</button>
</section>

<section class="portfolio">
  <h2 style="margin-bottom:40px;">Selected Work</h2>

  <div class="grid">
    <div class="card">
      <img src="https://via.placeholder.com/400x250">
      <div class="overlay">View</div>
    </div>

    <div class="card">
      <img src="https://via.placeholder.com/400x250">
      <div class="overlay">View</div>
    </div>

    <div class="card">
      <img src="https://via.placeholder.com/400x250">
      <div class="overlay">View</div>
    </div>
  </div>
</section>

<section class="services">
  <div class="service">
    <h3>Video Editing</h3>
    <p>Clean, cinematic edits</p>
  </div>

  <div class="service">
    <h3>Thumbnail Design</h3>
    <p>High CTR visuals</p>
  </div>

  <div class="service">
    <h3>Shorts Editing</h3>
    <p>Viral content creation</p>
  </div>
</section>

<section class="contact">
  <h2>Let’s Work Together</h2>

  <input type="text" placeholder="Name">
  <input type="email" placeholder="Email">
  <textarea placeholder="Message"></textarea>

  <button class="btn">Hire Me</button>
</section>

<footer>
  © 2026 Editya
</footer>

</body>
</html>
