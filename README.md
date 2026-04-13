<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Editya — Video Editor & Thumbnail Designer</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
<style>
/* ========== RESET & BASE ========== */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth;font-size:16px}
body{font-family:'Poppins',sans-serif;background:#0d0d0d;color:#f0f0f0;overflow-x:hidden;line-height:1.6}
::-webkit-scrollbar{width:4px}
::-webkit-scrollbar-track{background:#0d0d0d}
::-webkit-scrollbar-thumb{background:#00ff88;border-radius:2px}

/* ========== CSS VARIABLES ========== */
:root{
  --bg:#0d0d0d;
  --bg2:#111111;
  --card:#141414;
  --accent:#00ff88;
  --text:#f0f0f0;
  --muted:#888888;
  --glow:0 0 20px rgba(0,255,136,0.4),0 0 40px rgba(0,255,136,0.15);
  --border:rgba(255,255,255,0.07);
}

/* ========== LOADING SCREEN ========== */
#loader{
  position:fixed;inset:0;background:#0d0d0d;z-index:99999;
  display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2.5rem;
  transition:opacity 0.6s ease, visibility 0.6s ease;
}
#loader.fade-out{opacity:0;visibility:hidden}
.loader-name{
  font-size:clamp(3rem,10vw,6rem);font-weight:900;letter-spacing:-2px;
  animation:namePulse 1.5s ease-in-out infinite alternate;
}
.loader-name span{color:var(--accent)}
@keyframes namePulse{
  0%{text-shadow:0 0 20px rgba(0,255,136,0.3)}
  100%{text-shadow:0 0 50px rgba(0,255,136,0.9),0 0 80px rgba(0,255,136,0.4)}
}
.timeline-wrap{
  width:min(500px,80vw);display:flex;flex-direction:column;gap:12px;
}
.timeline-bar-outer{
  position:relative;height:6px;background:rgba(255,255,255,0.08);border-radius:3px;overflow:visible;
}
.timeline-bar-fill{
  height:100%;width:0;background:linear-gradient(90deg,#00ff88,#00ccff);border-radius:3px;
  animation:fillBar 2.4s cubic-bezier(0.4,0,0.2,1) forwards;
  position:relative;
}
.timeline-playhead{
  position:absolute;top:-14px;left:0;transform:translateX(-50%);
  filter:drop-shadow(0 0 8px #00ff88);
  animation:movePlayhead 2.4s cubic-bezier(0.4,0,0.2,1) forwards;
  pointer-events:none;
}
.playhead-tri{
  width:0;height:0;border-left:7px solid transparent;border-right:7px solid transparent;border-top:10px solid #00ff88;
  margin:0 auto;
}
.playhead-line{width:2px;height:26px;background:#00ff88;margin:0 auto}
@keyframes fillBar{from{width:0}to{width:100%}}
@keyframes movePlayhead{from{left:0}to{left:100%}}
.timeline-clips{display:flex;gap:6px;align-items:center;height:22px}
.clip{
  height:18px;border-radius:4px;opacity:0;
  animation:clipIn 0.3s ease forwards;
}
.clip:nth-child(1){width:52px;background:#ff6b6b;animation-delay:0.15s}
.clip:nth-child(2){width:38px;background:#ffd93d;animation-delay:0.3s}
.clip:nth-child(3){width:65px;background:#00ff88;animation-delay:0.45s}
.clip:nth-child(4){width:28px;background:#6bcbff;animation-delay:0.6s}
.clip:nth-child(5){width:48px;background:#c77dff;animation-delay:0.75s}
.clip:nth-child(6){width:35px;background:#ff9a3c;animation-delay:0.9s}
.clip:nth-child(7){width:55px;background:#00ff88;animation-delay:1.05s}
.clip:nth-child(8){width:30px;background:#ff6b6b;animation-delay:1.2s}
.clip:nth-child(9){width:44px;background:#ffd93d;animation-delay:1.35s}
.clip:nth-child(10){width:40px;background:#6bcbff;animation-delay:1.5s}
@keyframes clipIn{from{opacity:0;transform:scaleY(0)}to{opacity:1;transform:scaleY(1)}}

/* ========== AVAILABILITY BAR ========== */
#avail-bar{
  position:fixed;top:0;left:0;right:0;z-index:9998;
  height:32px;display:flex;align-items:center;justify-content:center;gap:10px;
  background:rgba(0,255,136,0.07);border-bottom:1px solid rgba(0,255,136,0.25);
  backdrop-filter:blur(10px);-webkit-backdrop-filter:blur(10px);
  font-size:0.78rem;font-weight:500;letter-spacing:0.03em;color:#c8ffe8;
}
.avail-dot{
  width:8px;height:8px;border-radius:50%;background:#00ff88;
  animation:dotPulse 2s ease-in-out infinite;flex-shrink:0;
}
@keyframes dotPulse{
  0%,100%{box-shadow:0 0 0 0 rgba(0,255,136,0.7)}
  50%{box-shadow:0 0 0 6px rgba(0,255,136,0)}
}

/* ========== NAVBAR ========== */
#navbar{
  position:fixed;top:32px;left:0;right:0;z-index:9997;
  padding:10px 20px;transition:padding 0.3s;
}
.nav-inner{
  max-width:1100px;margin:0 auto;
  display:flex;align-items:center;justify-content:space-between;
  background:rgba(13,13,13,0.85);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);
  border:1px solid var(--border);border-radius:50px;padding:12px 24px;
  transition:box-shadow 0.3s;
}
.nav-inner.scrolled{box-shadow:0 4px 30px rgba(0,0,0,0.5)}
.nav-logo{font-weight:900;font-size:1.25rem;letter-spacing:-0.5px;text-decoration:none;color:var(--text)}
.nav-logo span{color:var(--accent)}
.nav-links{display:flex;gap:6px;list-style:none}
.nav-links a{
  text-decoration:none;color:var(--muted);font-size:0.88rem;font-weight:500;
  padding:7px 14px;border-radius:50px;transition:color 0.2s,background 0.2s;
}
.nav-links a:hover,.nav-links a.active{color:var(--accent);background:rgba(0,255,136,0.1)}
.btn-hireme{
  background:var(--accent);color:#000;font-weight:700;font-size:0.85rem;
  padding:9px 22px;border-radius:50px;text-decoration:none;border:none;cursor:pointer;
  transition:transform 0.2s,box-shadow 0.2s;display:inline-block;
}
.btn-hireme:hover{transform:translateY(-2px);box-shadow:var(--glow)}
.hamburger{display:none;flex-direction:column;gap:5px;cursor:pointer;padding:4px;background:none;border:none}
.hamburger span{width:22px;height:2px;background:var(--text);border-radius:2px;transition:transform 0.3s,opacity 0.3s}
.hamburger.open span:nth-child(1){transform:translateY(7px) rotate(45deg)}
.hamburger.open span:nth-child(2){opacity:0}
.hamburger.open span:nth-child(3){transform:translateY(-7px) rotate(-45deg)}
#mobile-menu{
  display:none;position:fixed;inset:0;background:#0d0d0d;z-index:9996;
  flex-direction:column;align-items:center;justify-content:center;gap:2rem;
}
#mobile-menu.open{display:flex}
#mobile-menu a{font-size:2rem;font-weight:800;color:var(--text);text-decoration:none;transition:color 0.2s}
#mobile-menu a:hover{color:var(--accent)}
#mobile-menu .btn-hireme{font-size:1.1rem;padding:14px 36px;margin-top:1rem}

/* ========== HERO ========== */
#hero{
  min-height:100vh;display:flex;align-items:center;justify-content:center;
  padding:120px 20px 80px;position:relative;overflow:hidden;
}
.hero-bg{
  position:absolute;inset:0;z-index:0;
  background:
    radial-gradient(ellipse 60% 50% at 50% 50%, rgba(0,255,136,0.06) 0%, transparent 70%),
    linear-gradient(rgba(0,255,136,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,255,136,0.03) 1px, transparent 1px);
  background-size:auto,40px 40px,40px 40px;
}
.hero-content{position:relative;z-index:1;text-align:center;max-width:820px;margin:0 auto}
.hero-badge{
  display:inline-flex;align-items:center;gap:8px;
  background:rgba(0,255,136,0.1);border:1px solid rgba(0,255,136,0.3);
  color:var(--accent);font-size:0.82rem;font-weight:600;letter-spacing:0.05em;
  padding:7px 18px;border-radius:50px;margin-bottom:1.5rem;
  opacity:0;animation:fadeUp 0.6s ease 0.2s forwards;
}
.hero-name{
  font-size:clamp(3.5rem,12vw,8rem);font-weight:900;line-height:1;
  letter-spacing:-3px;margin-bottom:1.2rem;
  opacity:0;animation:fadeUp 0.6s ease 0.35s forwards;
}
.hero-name span{color:var(--accent)}
.hero-tagline{
  font-size:clamp(1rem,2.5vw,1.35rem);font-weight:600;color:var(--text);
  margin-bottom:1rem;
  opacity:0;animation:fadeUp 0.6s ease 0.5s forwards;
}
.hero-intro{
  font-size:0.97rem;color:var(--muted);max-width:520px;margin:0 auto 2.5rem;
  opacity:0;animation:fadeUp 0.6s ease 0.65s forwards;
}
.hero-btns{
  display:flex;gap:14px;justify-content:center;flex-wrap:wrap;
  opacity:0;animation:fadeUp 0.6s ease 0.8s forwards;
}
.btn-primary{
  background:var(--accent);color:#000;font-weight:700;font-size:0.95rem;
  padding:14px 32px;border-radius:50px;text-decoration:none;border:none;cursor:pointer;
  transition:transform 0.2s,box-shadow 0.2s;
}
.btn-primary:hover{transform:translateY(-3px);box-shadow:var(--glow)}
.btn-ghost{
  background:transparent;color:var(--text);font-weight:600;font-size:0.95rem;
  padding:14px 32px;border-radius:50px;text-decoration:none;
  border:1.5px solid rgba(255,255,255,0.25);
  transition:transform 0.2s,border-color 0.2s,box-shadow 0.2s;
}
.btn-ghost:hover{transform:translateY(-3px);border-color:var(--accent);box-shadow:var(--glow);color:var(--accent)}
.scroll-hint{
  position:absolute;bottom:36px;left:50%;transform:translateX(-50%);
  display:flex;flex-direction:column;align-items:center;gap:6px;color:var(--muted);font-size:0.75rem;
  animation:bounce 2s ease-in-out infinite;opacity:0;animation-delay:1.2s;animation-fill-mode:forwards;
}
.scroll-hint svg{opacity:0.5}
@keyframes bounce{0%,100%{transform:translateX(-50%) translateY(0)}50%{transform:translateX(-50%) translateY(-8px)}}
@keyframes fadeUp{from{opacity:0;transform:translateY(25px)}to{opacity:1;transform:translateY(0)}}

/* ========== SECTION COMMON ========== */
section{padding:90px 20px}
.section-inner{max-width:1100px;margin:0 auto}
.section-label{
  font-size:0.75rem;font-weight:700;letter-spacing:0.15em;text-transform:uppercase;
  color:var(--accent);margin-bottom:0.6rem;
}
.section-title{font-size:clamp(1.8rem,4vw,2.6rem);font-weight:800;margin-bottom:0.7rem;letter-spacing:-1px}
.section-subtitle{color:var(--muted);font-size:0.97rem;max-width:500px;margin-bottom:3rem}

/* ========== SERVICES ========== */
#services{background:var(--bg)}
.services-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:1.2rem}
.service-card{
  background:var(--card);border:1px solid var(--border);border-radius:16px;padding:2rem;
  transition:transform 0.3s,box-shadow 0.3s,border-color 0.3s;
  position:relative;overflow:hidden;
}
.service-card::before{
  content:'';position:absolute;inset:0;border-radius:16px;
  background:radial-gradient(ellipse at top left, rgba(0,255,136,0.05), transparent 60%);
  opacity:0;transition:opacity 0.3s;
}
.service-card:hover{transform:translateY(-5px) scale(1.01);border-color:rgba(0,255,136,0.4);box-shadow:var(--glow)}
.service-card:hover::before{opacity:1}
.svc-icon{font-size:2.2rem;margin-bottom:1rem;display:block}
.svc-title{font-size:1.2rem;font-weight:700;margin-bottom:0.6rem}
.svc-desc{color:var(--muted);font-size:0.9rem;line-height:1.7;margin-bottom:1.2rem}
.svc-tag{
  display:inline-block;background:rgba(0,255,136,0.1);color:var(--accent);
  border:1px solid rgba(0,255,136,0.3);font-size:0.73rem;font-weight:700;letter-spacing:0.05em;
  padding:4px 12px;border-radius:50px;
}

/* ========== SKILLS ========== */
#skills{background:var(--bg2)}
.skills-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:1rem}
.skill-card{
  background:var(--card);border:1px solid var(--border);border-radius:16px;padding:1.8rem;
  position:relative;overflow:hidden;
  transition:transform 0.3s,border-color 0.3s;
}
.skill-card::before{
  content:'';position:absolute;left:0;top:0;width:3px;height:0;background:var(--accent);
  border-radius:3px 0 0 3px;transition:height 0.4s ease;
}
.skill-card:hover{transform:translateX(4px);border-color:rgba(0,255,136,0.3)}
.skill-card:hover::before{height:100%}
.skill-icon{font-size:1.8rem;margin-bottom:0.8rem;display:block}
.skill-title{font-size:1rem;font-weight:700;margin-bottom:0.4rem}
.skill-desc{color:var(--muted);font-size:0.85rem;line-height:1.6}

/* ========== PORTFOLIO ========== */
#portfolio{background:var(--bg)}
.port-tabs{display:flex;gap:8px;margin-bottom:2rem;flex-wrap:wrap}
.tab-btn{
  background:transparent;color:var(--muted);font-family:'Poppins',sans-serif;
  font-weight:600;font-size:0.88rem;border:1.5px solid var(--border);
  padding:9px 24px;border-radius:50px;cursor:pointer;transition:all 0.2s;
}
.tab-btn.active{background:var(--accent);color:#000;border-color:var(--accent);font-weight:700}
.tab-btn:hover:not(.active){border-color:rgba(0,255,136,0.4);color:var(--accent)}
.port-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:1rem}
.port-item{
  background:var(--card);border:1px solid var(--border);border-radius:16px;
  overflow:hidden;cursor:pointer;position:relative;
  transition:transform 0.3s,border-color 0.3s,box-shadow 0.3s,opacity 0.4s;
}
.port-item[data-type="video"]{aspect-ratio:16/10}
.port-item[data-type="thumbnail"]{aspect-ratio:16/9}
.port-item:hover{transform:scale(1.02);border-color:rgba(0,255,136,0.5);box-shadow:var(--glow)}
.port-item.hidden{opacity:0;pointer-events:none;display:none}
.port-media{width:100%;height:100%;object-fit:cover;display:block}
.port-video{width:100%;height:100%;object-fit:cover;display:block}
.port-overlay{
  position:absolute;inset:0;background:linear-gradient(to top,rgba(0,0,0,0.9) 0%,rgba(0,0,0,0.3) 60%,transparent 100%);
  opacity:0;transition:opacity 0.3s;display:flex;flex-direction:column;justify-content:flex-end;padding:1.2rem;
}
.port-item:hover .port-overlay{opacity:1}
.port-title{font-weight:700;font-size:0.95rem;margin-bottom:3px}
.port-cat{font-size:0.78rem;color:var(--accent)}
.play-btn{
  position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);
  width:54px;height:54px;border-radius:50%;
  background:rgba(0,255,136,0.9);display:flex;align-items:center;justify-content:center;
  opacity:0;transition:opacity 0.3s ease;pointer-events:none;
  box-shadow:0 0 25px rgba(0,255,136,0.6);
}
.port-item:hover .play-btn{opacity:1}
.play-btn.hidden-play{opacity:0!important}
.play-btn svg{margin-left:4px}

/* Placeholder thumbnails */
.port-thumb-placeholder{
  width:100%;height:100%;display:flex;align-items:center;justify-content:center;
  background:linear-gradient(135deg,#141414,#1c1c1c);font-size:2.5rem;
  position:relative;overflow:hidden;
}
.port-thumb-placeholder::before{
  content:'';position:absolute;inset:0;
  background:linear-gradient(135deg,rgba(0,255,136,0.05),transparent);
}
.port-thumb-placeholder .thumb-text{
  position:absolute;bottom:14px;left:16px;right:16px;font-size:0.8rem;font-weight:600;color:#c0c0c0;
}

/* ========== PRICING ========== */
#pricing{background:var(--bg2)}
.pricing-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:1.2rem;align-items:start}
.price-card{
  background:var(--card);border:1px solid var(--border);border-radius:16px;padding:2rem;
  position:relative;transition:transform 0.3s,box-shadow 0.3s;
}
.price-card:hover{transform:translateY(-4px)}
.price-card.featured{
  background:linear-gradient(145deg,#0d1f14,#111a14);
  border:1.5px solid rgba(0,255,136,0.5);
  box-shadow:0 0 30px rgba(0,255,136,0.12);
  padding-top:2.8rem;
}
.price-card.featured:hover{box-shadow:var(--glow)}
.popular-badge{
  position:absolute;top:-14px;left:50%;transform:translateX(-50%);
  background:var(--accent);color:#000;font-size:0.75rem;font-weight:700;
  padding:5px 16px;border-radius:50px;white-space:nowrap;letter-spacing:0.03em;
}
.price-icon{font-size:2rem;margin-bottom:1rem;display:block}
.price-title{font-size:1.05rem;font-weight:700;margin-bottom:0.3rem}
.price-amount{font-size:2.4rem;font-weight:800;color:var(--accent);margin-bottom:0.3rem;line-height:1.1}
.price-amount span{font-size:0.85rem;font-weight:400;color:var(--muted)}
.price-desc{color:var(--muted);font-size:0.85rem;margin-bottom:1.5rem;line-height:1.6}
.price-features{list-style:none;margin-bottom:1.8rem;display:flex;flex-direction:column;gap:8px}
.price-features li{font-size:0.88rem;color:var(--text);display:flex;align-items:center;gap:8px}
.price-features li::before{content:'✓';color:var(--accent);font-weight:700;flex-shrink:0}
.btn-price-ghost{
  width:100%;padding:12px;border-radius:50px;border:1.5px solid rgba(255,255,255,0.2);
  background:transparent;color:var(--text);font-family:'Poppins',sans-serif;font-weight:600;font-size:0.9rem;
  cursor:pointer;transition:border-color 0.2s,color 0.2s,box-shadow 0.2s;
}
.btn-price-ghost:hover{border-color:var(--accent);color:var(--accent);box-shadow:var(--glow)}
.btn-price-filled{
  width:100%;padding:12px;border-radius:50px;border:none;
  background:var(--accent);color:#000;font-family:'Poppins',sans-serif;font-weight:700;font-size:0.9rem;
  cursor:pointer;transition:transform 0.2s,box-shadow 0.2s;
}
.btn-price-filled:hover{transform:translateY(-2px);box-shadow:var(--glow)}

/* ========== TOOLS ========== */
#tools{background:var(--bg)}
.tools-flex{display:flex;gap:1rem;flex-wrap:wrap}
.tool-card{
  flex:1;min-width:200px;background:var(--card);border:1px solid var(--border);
  border-radius:16px;padding:1.6rem;display:flex;align-items:center;gap:1rem;
  transition:transform 0.3s,box-shadow 0.3s,border-color 0.3s;
}
.tool-card:hover{transform:translateY(-4px);box-shadow:0 8px 30px rgba(0,0,0,0.4);border-color:rgba(255,255,255,0.12)}
.tool-icon{width:48px;height:48px;flex-shrink:0}
.tool-name{font-weight:700;font-size:0.97rem}
.tool-sub{color:var(--muted);font-size:0.8rem;margin-top:2px}

/* ========== CONTACT ========== */
#contact{background:var(--bg2)}
.contact-grid{display:grid;grid-template-columns:1fr;gap:3rem;align-items:start;max-width:600px}
.contact-left h2{font-size:clamp(1.6rem,3.5vw,2.4rem);font-weight:800;margin-bottom:1rem;letter-spacing:-1px}
.contact-left p{color:var(--muted);font-size:0.95rem;line-height:1.8;margin-bottom:2rem}
.social-btns{display:flex;flex-direction:column;gap:10px}
.social-btn{
  display:flex;align-items:center;gap:12px;background:var(--card);
  border:1px solid var(--border);border-radius:50px;padding:12px 20px;
  text-decoration:none;color:var(--text);font-weight:600;font-size:0.9rem;
  transition:transform 0.2s,border-color 0.2s,box-shadow 0.2s;
}
.social-btn:hover{transform:translateX(4px);border-color:rgba(0,255,136,0.4);box-shadow:0 4px 20px rgba(0,0,0,0.3)}
.contact-form{display:flex;flex-direction:column;gap:14px}
.form-field{
  width:100%;padding:14px 16px;background:rgba(255,255,255,0.04);
  border:1px solid var(--border);border-radius:10px;
  color:var(--text);font-family:'Poppins',sans-serif;font-size:0.9rem;
  outline:none;transition:border-color 0.2s,box-shadow 0.2s;resize:vertical;
}
.form-field:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(0,255,136,0.12)}
.form-field::placeholder{color:var(--muted)}
textarea.form-field{min-height:120px}
.btn-submit{
  width:100%;padding:14px;border-radius:50px;border:none;
  background:var(--accent);color:#000;font-family:'Poppins',sans-serif;font-weight:700;font-size:0.95rem;
  cursor:pointer;transition:transform 0.2s,box-shadow 0.2s;
}
.btn-submit:hover{transform:translateY(-2px);box-shadow:var(--glow)}
.form-success{
  display:none;text-align:center;padding:1.5rem;
  background:rgba(0,255,136,0.1);border:1px solid rgba(0,255,136,0.3);
  border-radius:12px;color:var(--accent);font-weight:600;font-size:1rem;
}

/* ========== CHAT BUBBLE ========== */
#chat-bubble{
  position:fixed;bottom:28px;right:28px;z-index:9990;
  display:flex;flex-direction:column;align-items:flex-end;gap:10px;
}
.bubble-msg{
  background:#fff;color:#111;font-size:0.85rem;font-weight:600;
  padding:12px 16px;border-radius:18px 18px 4px 18px;
  box-shadow:0 8px 32px rgba(0,0,0,0.3),0 2px 8px rgba(0,0,0,0.2);
  position:relative;max-width:220px;line-height:1.4;
  animation:floatBubble 3s ease-in-out infinite;
}
.bubble-msg::after{
  content:'';position:absolute;bottom:-8px;right:12px;
  width:0;height:0;
  border-left:8px solid transparent;border-top:8px solid #fff;
}
@keyframes floatBubble{
  0%,100%{transform:translateY(0)}
  50%{transform:translateY(-6px)}
}
.bubble-text-wrap{position:relative;min-height:40px}
.bubble-text{
  transition:opacity 0.35s ease,transform 0.35s ease;
  display:block;
}
.bubble-text.out{opacity:0;transform:transl
