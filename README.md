
<!--
File: wep-kelas-animated.html
Deskripsi: Full single-file animated website untuk "Kelas IPS 12.4 C".
Cara pakai:
1. Simpan sebagai index.html
2. Buka di browser (double-click) atau deploy ke GitHub Pages

Fitur utama:
- Responsive (desktop/mobile)
- Animasi hero (gradient + typing)
- Navigation dengan smooth scroll
- Cards animasi untuk pengumuman dan jadwal
- Gallery grid dengan hover effects
- Floating particles canvas ringan
- Dark mode toggle
- Scroll reveal (IntersectionObserver)
- CSS-only fallback jika JS mati

Silakan edit teks, gambar (src), dan warna sesuai keperluan.
-->

<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kelas IPS 12.4 C — Web Kelas Animated</title>
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;800&display=swap" rel="stylesheet">
  <script src="https://kit.fontawesome.com/yourkitid.js" crossorigin="anonymous"></script>
  <style>
    :root{
      --bg:#0f1724; --card:#0b1220; --muted:#9aa4b2; --accent:#6ee7b7; --accent2:#60a5fa; --glass: rgba(255,255,255,0.04);
      --glass-2: rgba(255,255,255,0.03);
    }
    *{box-sizing:border-box}
    html,body{height:100%; margin:0; font-family:Inter,system-ui,Segoe UI,Roboto,Arial; background:linear-gradient(180deg,#071028 0%, #081224 100%); color:#e6eef6}
    a{color:inherit; text-decoration:none}
    header{position:sticky; top:0; z-index:80; backdrop-filter: blur(6px); background:linear-gradient(180deg, rgba(6,10,20,0.55), rgba(6,10,20,0.35)); border-bottom:1px solid rgba(255,255,255,0.03)}
    .container{max-width:1100px; margin:0 auto; padding:28px}

    /* NAV */
    .nav{display:flex;align-items:center;justify-content:space-between;gap:16px}
    .brand{display:flex;gap:12px;align-items:center}
    .logo{width:46px;height:46px;border-radius:10px;background:linear-gradient(135deg,var(--accent),var(--accent2));display:flex;align-items:center;justify-content:center;font-weight:800;color:#042;box-shadow:0 6px 18px rgba(9,12,20,0.6)}
    nav a{margin-left:18px;font-weight:600;opacity:0.95}
    .nav-right{display:flex;align-items:center;gap:8px}
    .btn{padding:8px 12px;border-radius:10px;background:var(--glass);border:1px solid rgba(255,255,255,0.03);cursor:pointer}

    /* HERO */
    .hero{min-height:70vh;display:grid;align-items:center;grid-template-columns:1fr 380px;gap:32px}
    .hero-left h1{font-size:38px; margin:0 0 10px 0;line-height:1.05}
    .typing{color:var(--accent); font-weight:700}
    .lead{color:var(--muted); margin-top:8px}
    .cta-row{margin-top:20px;display:flex;gap:12px;align-items:center}

    /* cards */
    .cards{display:grid;grid-template-columns:repeat(3,1fr);gap:16px;margin-top:22px}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:18px;border-radius:12px;border:1px solid rgba(255,255,255,0.03);box-shadow:0 6px 24px rgba(2,6,15,0.6);transform:translateY(16px);opacity:0;transition:all .6s cubic-bezier(.2,.9,.2,1)}
    .card.show{transform:none;opacity:1}
    .card h3{margin:0 0 8px 0}
    .card p{margin:0;color:var(--muted);font-size:14px}

    /* RIGHT SIDE */
    .hero-right{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:14px;padding:18px;border:1px solid rgba(255,255,255,0.03)}
    .profile{display:flex;gap:12px;align-items:center}
    .photo{width:62px;height:62px;border-radius:10px; background:linear-gradient(135deg,var(--accent2),var(--accent));display:flex;align-items:center;justify-content:center;font-weight:700;color:#012}

    /* SCHEDULE & GALLERY */
    section{padding:44px 0}
    .grid-2{display:grid;grid-template-columns:1fr 1fr;gap:22px}
    .schedule-list{list-style:none;padding:0;margin:0}
    .schedule-list li{padding:12px;border-radius:10px;background:var(--glass);margin-bottom:10px}

    .gallery{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}
    .gimg{height:120px;border-radius:10px;overflow:hidden;position:relative;background-size:cover;background-position:center;transition:transform .4s ease, box-shadow .4s}
    .gimg:hover{transform:translateY(-8px);box-shadow:0 20px 40px rgba(2,8,20,0.6)}

    /* Footer */
    footer{padding:28px 0;color:var(--muted);border-top:1px solid rgba(255,255,255,0.02)}

    /* particles canvas */
    #particles{position:fixed;inset:0;z-index:0;pointer-events:none}

    /* Responsive */
    @media (max-width:900px){.hero{grid-template-columns:1fr;min-height:75vh}.cards{grid-template-columns:repeat(2,1fr)}.gallery{grid-template-columns:repeat(2,1fr)}.grid-2{grid-template-columns:1fr}}
    @media (max-width:520px){.cards{grid-template-columns:1fr}.hero-left h1{font-size:28px}}

    /* typing caret */
    .caret{display:inline-block;width:2px;height:22px;background:var(--accent);animation:blink 1s steps(2) infinite;vertical-align:middle;margin-left:6px}
    @keyframes blink{50%{opacity:0}}

    /* dark mode class for future */
    .light{--bg:#f7fafc;--card:#ffffff;--muted:#475569;color:#041022}
  </style>
</head>
<body>
  <canvas id="particles"></canvas>
  <header>
    <div class="container nav">
      <div class="brand">
        <div class="logo">IPS</div>
        <div>
          <div style="font-weight:700">Kelas IPS 12.4 C</div>
          <div style="font-size:12px;color:var(--muted)">Web kelas interaktif</div>
        </div>
      </div>

      <div style="display:flex;align-items:center;gap:18px">
        <nav style="display:flex;align-items:center">
          <a href="#home">Home</a>
          <a href="#pengumuman">Pengumuman</a>
          <a href="#jadwal">Jadwal</a>
          <a href="#gallery">Gallery</a>
        </nav>
        <div class="nav-right">
          <button class="btn" id="themeToggle" title="Toggle theme">🌙</button>
          <a class="btn" href="#daftar">Daftar</a>
        </div>
      </div>
    </div>
  </header>

  <main class="container" id="home">
    <section class="hero">
      <div class="hero-left">
        <h1>Selamat Datang di <span style="color:var(--accent)">Kelas IPS 12.4 C</span></h1>
        <div class="typing" aria-hidden="true">Web kelas interaktif — <span id="typed"></span><span class="caret"></span></div>
        <p class="lead">Tempat berbagi pengumuman, materi, jadwal, dan galeri kegiatan kelas dengan tampilan animasi menarik.</p>

        <div class="cta-row">
          <a class="btn" href="#pengumuman">Lihat Pengumuman</a>
          <a class="btn" href="#jadwal">Lihat Jadwal</a>
        </div>

        <div class="cards" style="margin-top:26px">
          <div class="card" data-idx="0">
            <h3>📢 Pengumuman Penting</h3>
            <p>Ujian tengah semester dilaksanakan 20 Desember 2025. Persiapkan diri dan materi.</p>
          </div>
          <div class="card" data-idx="1">
            <h3>📝 Tugas Kelompok</h3>
            <p>Kelompok A: Ekonomi. Kelompok B: Peta Geografi. Kumpul: 15 Desember.</p>
          </div>
          <div class="card" data-idx="2">
            <h3>🎉 Kegiatan Kelas</h3>
            <p>Field trip ke museum sejarah—info dan pendaftaran di bagian Daftar.</p>
          </div>
        </div>

      </div>

      <aside class="hero-right">
        <div class="profile">
          <div class="photo">12</div>
          <div>
            <div style="font-weight:700">Nama Wali Kelas</div>
            <div style="font-size:13px;color:var(--muted)">Guru IPS — Mr./Ms. Contoh</div>
          </div>
        </div>

        <div style="margin-top:14px">
          <h4 style="margin:0 0 8px 0">Statistik Kelas</h4>
          <div style="display:flex;gap:10px;align-items:center;margin-top:8px">
            <div style="flex:1">
              <div style="font-size:12px;color:var(--muted)">Siswa</div>
              <div style="font-weight:700">36</div>
            </div>
            <div style="flex:1">
              <div style="font-size:12px;color:var(--muted)">Tugas Aktif</div>
              <div style="font-weight:700">4</div>
            </div>
          </div>
        </div>

        <div style="margin-top:14px">
          <h4 style="margin:0 0 8px 0">Kontak</h4>
          <div style="font-size:13px;color:var(--muted)">WA: 0822-5689-4889</div>
        </div>
      </aside>
    </section>

    <!-- PENGUMUMAN -->
    <section id="pengumuman">
      <h2>Pengumuman</h2>
      <div class="grid-2" style="margin-top:12px">
        <div>
          <div class="card show">
            <h3>Pengumuman Terbaru</h3>
            <p>Perbaruan materi: bab 5 - Sistem Ekonomi. Silabus dan file bisa diunduh di sini (link nanti).</p>
          </div>

          <div class="card" style="margin-top:12px">
            <h3>Pengumuman 2</h3>
            <p>Seragam lengkap pada saat presentasi poster.</p>
          </div>
        </div>
        <div>
          <div class="card">
            <h3>Catatan Wali</h3>
            <p>Jangan lupa kumpulkan formulir kunjungan orang tua.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- JADWAL -->
    <section id="jadwal">
      <h2>Jadwal Pelajaran</h2>
      <ul class="schedule-list" style="margin-top:12px">
        <li><strong>Senin</strong> — IPS (08:00 - 09:30)</li>
        <li><strong>Selasa</strong> — Bahasa Indonesia (10:00 - 11:30)</li>
        <li><strong>Rabu</strong> — Matematika (08:00 - 09:30)</li>
        <li><strong>Kamis</strong> — PJOK (10:00 - 11:00)</li>
      </ul>
    </section>

    <!-- GALLERY -->
    <section id="gallery">
      <h2>Gallery Kegiatan</h2>
      <div class="gallery" style="margin-top:12px">
        <div class="gimg" style="background-image:url('https://images.unsplash.com/photo-1523240795612-9a054b0db644?q=80&w=800&auto=format&fit=crop&s=1')"></div>
        <div class="gimg" style="background-image:url('https://images.unsplash.com/photo-1496307042754-b4aa456c4a2d?q=80&w=800&auto=format&fit=crop&s=2')"></div>
        <div class="gimg" style="background-image:url('https://images.unsplash.com/photo-1503676260728-1c00da094a0b?q=80&w=800&auto=format&fit=crop&s=3')"></div>
        <div class="gimg" style="background-image:url('https://images.unsplash.com/photo-1529070538774-1843cb3265df?q=80&w=800&auto=format&fit=crop&s=4')"></div>
      </div>
    </section>

    <!-- DAFTAR / FORM SEDERHANA -->
    <section id="daftar">
      <h2>Form Pendaftaran Kegiatan</h2>
      <div class="card" style="margin-top:12px;padding:18px">
        <form id="formDaftar">
          <div style="display:flex;gap:10px;flex-wrap:wrap">
            <input type="text" id="nama" placeholder="Nama lengkap" style="flex:1;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:inherit" required>
            <input type="text" id="kelas" placeholder="Kelas" style="width:160px;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:inherit" required>
            <input type="tel" id="wa" placeholder="No. WA" style="width:180px;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:inherit" required>
          </div>
          <div style="margin-top:12px;display:flex;gap:10px">
            <button class="btn" type="submit">Daftar Sekarang</button>
            <button class="btn" type="button" id="resetForm">Reset</button>
          </div>
          <div id="formMsg" style="margin-top:10px;color:var(--accent)"></div>
        </form>
      </div>
    </section>

    <footer>
      <div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap">
        <div>© 2025 Kelas IPS 12.4 C — dibuat dengan ❤️</div>
        <div style="color:var(--muted)">Kontak: 0822-5689-4889</div>
      </div>
    </footer>
  </main>

  <script>
    // Smooth scroll for anchor links
    document.querySelectorAll('a[href^="#"]').forEach(a=>{
      a.addEventListener('click', e=>{
        const href = a.getAttribute('href');
        if(href.length>1){
          e.preventDefault();
          document.querySelector(href).scrollIntoView({behavior:'smooth', block:'start'});
        }
      })
    })

    // Typing effect (simple)
    const words = ['Belajar', 'Berbagi', 'Berkreasi', 'Berkolaborasi'];
    let idx=0, charIdx=0, forward=true;
    const out = document.getElementById('typed');
    function tick(){
      const w = words[idx];
      if(forward){
        charIdx++;
        out.textContent = w.slice(0,charIdx);
        if(charIdx===w.length){ forward=false; setTimeout(tick,900); return }
      } else {
        charIdx--;
        out.textContent = w.slice(0,charIdx);
        if(charIdx===0){ forward=true; idx=(idx+1)%words.length }
      }
      setTimeout(tick, 90);
    }
    tick();

    // Reveal animation for cards
    const cards = document.querySelectorAll('.card');
    const io = new IntersectionObserver((entries)=>{
      entries.forEach(ent=>{
        if(ent.isIntersecting) ent.target.classList.add('show');
      })
    },{threshold:0.12});
    cards.forEach(c=>io.observe(c));

    // Simple particles canvas (lightweight)
    const canvas = document.getElementById('particles');
    const ctx = canvas.getContext('2d');
    let W, H, particles=[];
    function resize(){W=canvas.width=innerWidth; H=canvas.height=innerHeight}
    window.addEventListener('resize', resize);
    function rand(min,max){return Math.random()*(max-min)+min}
    function initParticles(){particles=[]; for(let i=0;i<70;i++){particles.push({x:rand(0,W), y:rand(0,H), r:rand(0.6,2.6), vx:rand(-0.2,0.2), vy:rand(-0.2,0.2), alpha:rand(0.05,0.25)})}}
    function draw(){ctx.clearRect(0,0,W,H); for(const p of particles){ctx.beginPath(); ctx.fillStyle = 'rgba(110,231,183,'+p.alpha+')'; ctx.arc(p.x,p.y,p.r,0,Math.PI*2); ctx.fill(); p.x+=p.vx; p.y+=p.vy; if(p.x<0) p.x=W; if(p.x>W) p.x=0; if(p.y<0) p.y=H; if(p.y>H) p.y=0} requestAnimationFrame(draw)}
    resize(); initParticles(); draw();

    // Theme toggle simple
    const themeBtn = document.getElementById('themeToggle');
    themeBtn.addEventListener('click', ()=>{
      document.body.classList.toggle('light');
      themeBtn.textContent = document.body.classList.contains('light')? '🌞' : '🌙';
    });

    // Form handling (simple, stores to localStorage)
    const form = document.getElementById('formDaftar');
    const formMsg = document.getElementById('formMsg');
    form.addEventListener('submit', (e)=>{
      e.preventDefault();
      const data = {nama:form.nama.value, kelas:form.kelas.value, wa:form.wa.value, t:new Date().toISOString()};
      const arr = JSON.parse(localStorage.getItem('daftar')||'[]');
      arr.push(data); localStorage.setItem('daftar', JSON.stringify(arr));
      formMsg.textContent = 'Terima kasih, pendaftaran berhasil!';
      form.reset(); setTimeout(()=>formMsg.textContent='',3500);
    });
    document.getElementById('resetForm').addEventListener('click', ()=>form.reset());

    // Small accessibility: reduce motion
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    if(mq.matches){document.querySelectorAll('.card').forEach(c=>c.style.transition='none');}
  </script>
</body>
</html>
