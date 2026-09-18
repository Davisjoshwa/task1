<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Lumina Photo Gallery - Team Showcase</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: #0f172a;
      color: #f8fafc;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* Top Navbar */
    .navbar {
      background: rgba(15, 23, 42, 0.9);
      backdrop-filter: blur(10px);
      border-bottom: 1px solid #334155;
      padding: 16px 28px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      position: sticky;
      top: 0;
      z-index: 100;
    }
    .logo {
      display: flex;
      align-items: center;
      gap: 10px;
      font-size: 20px;
      font-weight: 700;
      background: linear-gradient(135deg, #38bdf8, #818cf8);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    .total-badge {
      display: flex;
      align-items: center;
      gap: 8px;
      background: #1e293b;
      border: 1px solid #475569;
      padding: 8px 16px;
      border-radius: 9999px;
      font-size: 14px;
      font-weight: 600;
    }
    .total-badge .heart-icon { color: #ef4444; }

    /* Header & Filter Pills */
    .gallery-header { text-align: center; padding: 36px 20px 16px; }
    .gallery-header h1 { font-size: 32px; font-weight: 800; margin-bottom: 6px; }
    .gallery-header p { color: #94a3b8; font-size: 15px; }
    .filters {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin: 20px 0 32px;
      flex-wrap: wrap;
    }
    .filter-btn {
      background: #1e293b;
      color: #cbd5e1;
      border: 1px solid #334155;
      padding: 8px 18px;
      border-radius: 20px;
      cursor: pointer;
      font-size: 14px;
      transition: all 0.2s;
    }
    .filter-btn:hover, .filter-btn.active {
      background: #3b82f6;
      color: #ffffff;
      border-color: #3b82f6;
    }

    /* Grid Layout */
    .gallery-wrapper { max-width: 1200px; margin: 0 auto; padding: 0 20px 60px; }
    .gallery-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
      gap: 24px;
      perspective: 1000px;
    }

    /* Photo Cards */
    .photo-card {
      background: #1e293b;
      border-radius: 14px;
      overflow: hidden;
      border: 1px solid #334155;
      display: flex;
      flex-direction: column;
      box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.4);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      position: relative;
    }
    .photo-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 16px 30px -5px rgba(0, 0, 0, 0.6);
      border-color: #475569;
    }
    .img-box { width: 100%; height: 220px; position: relative; overflow: hidden; background: #0b0f19; }
    .photo-img { width: 100%; height: 100%; object-fit: cover; transition: transform 0.4s ease; }
    .photo-card:hover .photo-img { transform: scale(1.05); }
    .category-tag {
      position: absolute;
      top: 10px;
      left: 10px;
      background: rgba(15, 23, 42, 0.8);
      backdrop-filter: blur(4px);
      color: #38bdf8;
      font-size: 11px;
      font-weight: 700;
      padding: 3px 8px;
      border-radius: 12px;
      text-transform: uppercase;
    }
    .card-body { padding: 16px; display: flex; flex-direction: column; gap: 12px; flex-grow: 1; justify-content: space-between; }
    .photo-title { font-size: 15px; font-weight: 700; color: #f1f5f9; }
    .photo-author { font-size: 13px; color: #94a3b8; }
    .card-actions {
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-top: 1px solid #334155;
      padding-top: 12px;
    }

    /* COVER FEATURE: Like Button */
    .like-btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      background: #0f172a;
      border: 1px solid #334155;
      color: #cbd5e1;
      padding: 6px 14px;
      border-radius: 9999px;
      cursor: pointer;
      font-size: 14px;
      font-weight: 600;
      position: relative;
      transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }
    .like-btn:hover { background: #1e293b; border-color: #475569; transform: scale(1.05); }
    .like-btn:active { transform: scale(0.92); }
    .like-btn.liked {
      background: rgba(239, 68, 68, 0.15);
      border-color: rgba(239, 68, 68, 0.5);
      color: #ef4444;
    }
    .like-btn.liked .heart-icon { transform: scale(1.25); color: #ef4444; }
    .heart-icon { display: inline-block; transition: transform 0.2s; }
    .burst-pulse { animation: heartPulse 0.3s ease-out; }
    @keyframes heartPulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.4); }
      100% { transform: scale(1); }
    }
    .floating-plus {
      position: absolute;
      top: -10px;
      right: 12px;
      font-size: 11px;
      font-weight: 800;
      color: #ef4444;
      pointer-events: none;
      animation: floatUp 0.6s forwards ease-out;
    }
    @keyframes floatUp {
      0% { opacity: 1; transform: translateY(0); }
      100% { opacity: 0; transform: translateY(-20px) scale(1.2); }
    }

    /* ⚠ SECRET CHAOS SABOTAGE STYLES */
    .chaos-banner {
      display: none;
      background: linear-gradient(90deg, #ff0055, #7928ca, #ff0055);
      color: #ffffff;
      padding: 12px 20px;
      text-align: center;
      font-weight: 800;
      font-size: 14px;
      letter-spacing: 0.5px;
      box-shadow: 0 4px 20px rgba(255, 0, 85, 0.6);
      position: relative;
      z-index: 999;
      justify-content: center;
      align-items: center;
      gap: 16px;
    }
    .chaos-banner.active { display: flex; }
    .reset-btn {
      background: white;
      color: #0f172a;
      border: none;
      padding: 5px 12px;
      border-radius: 4px;
      font-weight: 700;
      cursor: pointer;
      font-size: 12px;
    }
    .chaos-spin-mode .photo-card {
      animation: chaosSpin 2.5s infinite linear;
      border-color: #ff0055 !important;
      box-shadow: 0 0 25px rgba(255, 0, 85, 0.8) !important;
    }
    .chaos-wobble-page { animation: wobbleScreen 0.25s infinite ease-in-out; }
    @keyframes chaosSpin {
      0% { transform: rotate(0deg) scale(1); filter: hue-rotate(0deg); }
      50% { transform: rotate(180deg) scale(1.08); filter: hue-rotate(180deg); }
      100% { transform: rotate(360deg) scale(1); filter: hue-rotate(360deg); }
    }
    @keyframes wobbleScreen {
      0% { transform: translate(1px, 1px) rotate(0deg); }
      50% { transform: translate(-2px, -1px) rotate(-0.5deg); }
      100% { transform: translate(1px, -1px) rotate(0.5deg); }
    }
    footer { text-align: center; padding: 20px; color: #64748b; font-size: 13px; border-top: 1px solid #1e293b; }
  </style>
</head>
<body>

  <!-- Hidden Chaos Banner -->
  <div id="chaosBanner" class="chaos-banner">
    <span>🚨 <strong>SYSTEM OVERHEAT: BUFFER OVERDRIVE ENGAGED 💥!</strong> 🚨</span>
    <button class="reset-btn" onclick="resetChaos()">Emergency Reset</button>
  </div>

  <!-- Navbar -->
  <header class="navbar">
    <div class="logo">
      <span>📸</span>
      <span>Lumina Gallery</span>
    </div>
    <div class="total-badge">
      <span class="heart-icon">❤️</span>
      <span id="globalLikes">326</span> Total Likes
    </div>
  </header>

  <!-- Header & Category Filters -->
  <section class="gallery-header">
    <h1>Team Photo Showcase</h1>
    <p>High-performance photo stream with live micro-reactions</p>
    <div class="filters">
      <button class="filter-btn active" onclick="filterCategory('all', this)">All</button>
      <button class="filter-btn" onclick="filterCategory('nature', this)">Nature</button>
      <button class="filter-btn" onclick="filterCategory('urban', this)">Urban</button>
      <button class="filter-btn" onclick="filterCategory('architecture', this)">Architecture</button>
    </div>
  </section>

  <!-- Main Grid -->
  <main class="gallery-wrapper">
    <div class="gallery-grid" id="galleryGrid"></div>
  </main>

  <footer>
    Sprint Project • 45-Minute Team Showcase
  </footer>

  <script>
    const photos = [
      { id: "photo-1", title: "Alpine Sunrise Glow", author: "Julian Vance", category: "nature", likes: 42, url: "https://images.unsplash.com/photo-1506744038136-46273834b3fb?w=700&auto=format&fit=crop&q=80" },
      { id: "photo-2", title: "Cyberpunk Highway", author: "Elena Rostova", category: "urban", likes: 87, url: "https://images.unsplash.com/photo-1519501025264-65ba15a82390?w=700&auto=format&fit=crop&q=80" },
      { id: "photo-3", title: "Neon Tokyo Alley", author: "Kenji Sato", category: "urban", likes: 129, url: "https://images.unsplash.com/photo-1509198397868-475647b2a1e5?w=700&auto=format&fit=crop&q=80" }, // ⚠ TARGET CARD!
      { id: "photo-4", title: "Spiral Marble Atrium", author: "Marcus Stone", category: "architecture", likes: 34, url: "https://images.unsplash.com/photo-1513694203232-719a280e022f?w=700&auto=format&fit=crop&q=80" },
      { id: "photo-5", title: "Misty Redwood Trail", author: "Sarah Jenkins", category: "nature", likes: 56, url: "https://images.unsplash.com/photo-1448375240586-882707db888b?w=700&auto=format&fit=crop&q=80" },
      { id: "photo-6", title: "Glass Monolith Tower", author: "Devon Reed", category: "architecture", likes: 61, url: "https://images.unsplash.com/photo-1486406146926-c627a92ad1ab?w=700&auto=format&fit=crop&q=80" }
    ];

    // Secret Meme Pool (Disguised as offline asset cache)
    const OFFLINE_FALLBACK_POOL = [
      "https://i.imgflip.com/4/4t0m5.jpg",      // Doge
      "https://i.imgflip.com/4/1ur9b0.jpg",     // Distracted Boyfriend
      "https://i.imgflip.com/4/30b1gx.jpg",     // Drake Hotline
      "https://i.imgflip.com/4/26am.jpg",       // Grumpy Cat
      "https://i.imgflip.com/1ooaki.jpg",       // Roll Safe
      "https://i.imgflip.com/2kbn1e.jpg"        // Surprised Pikachu
    ];

    let totalLikes = photos.reduce((sum, p) => sum + p.likes, 0);
    const SECRET_TRIGGER_ID = "photo-3"; // Card #3 is your trap!
    let clickState = { count: 0, lastTime: 0 };

    function render(cat = 'all') {
      const grid = document.getElementById('galleryGrid');
      grid.innerHTML = '';
      photos.forEach(p => {
        if (cat !== 'all' && p.category !== cat) return;
        const card = document.createElement('div');
        card.className = 'photo-card';
        card.dataset.id = p.id;
        card.innerHTML = `
          <div class="img-box">
            <img src="${p.url}" class="photo-img" alt="${p.title}">
            <span class="category-tag">${p.category}</span>
          </div>
          <div class="card-body">
            <div>
              <div class="photo-title">${p.title}</div>
              <div class="photo-author">by ${p.author}</div>
            </div>
            <div class="card-actions">
              <button class="like-btn" onclick="onLike('${p.id}', this)">
                <span class="heart-icon">❤️</span>
                <span class="count">${p.likes}</span>
              </button>
              <span style="color:#64748b; font-size:12px;">Curated</span>
            </div>
          </div>
        `;
        grid.appendChild(card);
      });
      document.getElementById('globalLikes').innerText = totalLikes;
    }

    /* COVER JOB: Like button handler */
    function onLike(id, btn) {
      const liked = btn.classList.toggle('liked');
      const countEl = btn.querySelector('.count');
      const heart = btn.querySelector('.heart-icon');
      let c = parseInt(countEl.innerText, 10);

      if (liked) {
        c += 1;
        totalLikes += 1;
        heart.classList.add('burst-pulse');
        setTimeout(() => heart.classList.remove('burst-pulse'), 300);

        // Floating +1
        const plus = document.createElement('span');
        plus.className = 'floating-plus';
        plus.innerText = '+1';
        btn.appendChild(plus);
        setTimeout(() => plus.remove(), 600);
      } else {
        c -= 1;
        totalLikes -= 1;
      }

      countEl.innerText = c;
      document.getElementById('globalLikes').innerText = totalLikes;

      // Disguised combo buffer check
      checkHapticBuffer(id);
    }

    /* ⚠ SECRET TRAP (Disguised as Rapid Click Buffer) */
    function checkHapticBuffer(photoId) {
      const now = Date.now();
      if (now - clickState.lastTime > 800) {
        clickState.count = 0;
      }
      clickState.lastTime = now;
      clickState.count += 1;

      // Trigger if card #photo-3 is clicked 3 times fast!
      if (photoId === SECRET_TRIGGER_ID && clickState.count === 3) {
        activateChaos();
        clickState.count = 0;
      }
    }

    /* 💥 CHAOS PAYLOAD */
    function activateChaos() {
      // 1. Swap all photos to memes
      document.querySelectorAll('.photo-img').forEach((img, i) => {
        img.src = OFFLINE_FALLBACK_POOL[i % OFFLINE_FALLBACK_POOL.length];
      });

      // 2. Spin cards and shake page
      document.getElementById('galleryGrid').classList.add('chaos-spin-mode');
      document.body.classList.add('chaos-wobble-page');

      // 3. Show banner
      document.getElementById('chaosBanner').classList.add('active');
    }

    function resetChaos() {
      document.getElementById('galleryGrid').classList.remove('chaos-spin-mode');
      document.body.classList.remove('chaos-wobble-page');
      document.getElementById('chaosBanner').classList.remove('active');
      render();
    }

    function filterCategory(c, btn) {
      document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      render(c);
    }

    document.addEventListener('DOMContentLoaded', () => render());
  </script>
</body>
</html>
