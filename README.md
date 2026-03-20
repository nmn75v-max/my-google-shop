<!DOCTYPE html> 
<html lang="vi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Robux Shop Demo</title>
<style>
  :root{
    --bg:#000;
    --panel:#111827;
    --panel2:#0b1220;
    --text:#e5e7eb;
    --muted:#9ca3af;
    --accent:#6366f1;
    --accent2:#3b82f6;
    --line:rgba(255,255,255,.08);
    --good:#22c55e;
    --warn:#f59e0b;
  }
  *{box-sizing:border-box}
  body{
    margin:0;
    font-family:Arial, Helvetica, sans-serif;
    background:var(--bg);
    color:var(--text);
    overflow-x:hidden;
  }
  canvas{
    position:fixed;
    inset:0;
    pointer-events:none;
    z-index:0;
  }
  .topbar{
    position:sticky;
    top:0;
    z-index:5;
    display:flex;
    justify-content:space-between;
    align-items:center;
    gap:12px;
    padding:12px 18px;
    background:rgba(0,0,0,.88);
    border-bottom:1px solid var(--line);
    backdrop-filter: blur(10px);
  }
  .nav-left,.user-box{
    display:flex;
    gap:10px;
    align-items:center;
    flex-wrap:wrap;
  }
  .nav-btn,.chip,.btn,.mini-btn{
    border:none;
    cursor:pointer;
    color:var(--text);
  }
  .nav-btn{
    padding:10px 14px;
    border-radius:999px;
    background:#111;
  }
  .nav-btn.primary{background:linear-gradient(90deg,var(--accent2),var(--accent))}
  .chip{
    padding:8px 12px;
    border-radius:999px;
    background:#0f172a;
    border:1px solid var(--line);
    color:var(--muted);
  }
  .wrap{
    position:relative;
    z-index:1;
    max-width:1100px;
    margin:0 auto;
    padding:18px 16px 30px;
  }
  .banner,.panel,.card{
    background:linear-gradient(180deg,rgba(17,24,39,.96),rgba(11,18,32,.96));
    border:1px solid var(--line);
    box-shadow:0 20px 70px rgba(0,0,0,.35);
  }
  .banner{
    border-radius:24px;
    padding:18px;
    min-height:180px;
    display:flex;
    flex-direction:column;
    justify-content:center;
    gap:10px;
  }
  .headline{
    margin:0;
    font-size:28px;
    font-weight:800;
    line-height:1.2;
  }
  .subline{
    margin:0;
    color:var(--muted);
  }
  .notice{
    display:inline-block;
    width:fit-content;
    margin-top:8px;
    padding:10px 14px;
    border-radius:14px;
    background:rgba(245,158,11,.12);
    color:#fbbf24;
    border:1px solid rgba(245,158,11,.22);
    font-weight:700;
  }
  .section{
    display:none;
    margin-top:18px;
  }
  .section.active{display:block}
  .panel{
    border-radius:24px;
    padding:22px;
  }
  .title{
    text-align:center;
    margin:0 0 8px;
    font-size:24px;
    font-weight:800;
  }
  .subtitle{
    text-align:center;
    margin:0 0 18px;
    color:var(--muted);
  }
  .grid{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:16px;
  }
  .grid.one{grid-template-columns:1fr}
  label{
    display:block;
    margin-bottom:6px;
    font-size:14px;
    color:#cbd5e1;
  }
  select,input{
    width:100%;
    padding:12px 13px;
    border:none;
    border-radius:12px;
    background:#263244;
    color:#fff;
    outline:none;
  }
  .btn{
    width:100%;
    padding:13px 16px;
    border-radius:14px;
    font-weight:700;
    background:linear-gradient(90deg,var(--accent2),var(--accent));
  }
  .btn.gray{background:#334155}
  .btn-row{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:10px;
    margin-top:14px;
  }
  .muted{
    color:var(--muted);
    font-size:13px;
    line-height:1.5;
  }
  .result{
    margin-top:12px;
    color:var(--good);
    font-weight:700;
  }
  .warn{
    margin-top:10px;
    color:var(--warn);
    font-size:14px;
  }
  .card-grid{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:14px;
    margin-top:14px;
  }
  .card{
    border-radius:18px;
    padding:16px;
  }
  .card h3{
    margin:0 0 8px;
    font-size:18px;
  }
  .price{
    font-size:28px;
    font-weight:800;
    margin:10px 0;
  }
  .card ul{
    margin:0;
    padding-left:18px;
    color:#d1d5db;
  }
  .card li{margin:6px 0}
  .small{
    font-size:12px;
    color:var(--muted);
  }
  .admin-fab{
    position:fixed;
    right:15px;
    bottom:15px;
    width:62px;
    height:62px;
    border:none;
    border-radius:50%;
    background:linear-gradient(135deg,var(--accent),var(--accent2));
    color:white;
    font-weight:800;
    cursor:pointer;
    z-index:10;
    box-shadow:0 14px 30px rgba(0,0,0,.4);
  }
  .modal{
    display:none;
    position:fixed;
    inset:0;
    background:rgba(0,0,0,.72);
    z-index:20;
    align-items:center;
    justify-content:center;
    padding:16px;
  }
  .modal-card{
    width:min(760px,100%);
    border-radius:22px;
    overflow:hidden;
    border:1px solid var(--line);
    background:linear-gradient(180deg,#111827,#0b1220);
    box-shadow:0 30px 100px rgba(0,0,0,.5);
  }
  .modal-head{
    display:flex;
    justify-content:space-between;
    align-items:center;
    padding:14px 16px;
    border-bottom:1px solid var(--line);
  }
  .close{
    width:36px;
    height:36px;
    border:none;
    border-radius:10px;
    background:#0f172a;
    color:#fff;
    cursor:pointer;
    font-size:20px;
  }
  .modal-body{padding:16px}
  .status{
    display:inline-block;
    margin-top:8px;
    padding:6px 10px;
    border-radius:999px;
    font-size:12px;
    font-weight:700;
  }
  .status.wait{background:rgba(245,158,11,.12);color:#fbbf24}
  .status.ok{background:rgba(34,197,94,.12);color:#4ade80}
  .status.fail{background:rgba(239,68,68,.12);color:#f87171}
  .admin-list{
    display:grid;
    gap:12px;
    margin-top:14px;
  }
  .admin-item{
    background:#263244;
    border:1px solid var(--line);
    border-radius:16px;
    padding:14px;
  }
  .admin-actions{
    display:flex;
    gap:8px;
    flex-wrap:wrap;
    margin-top:12px;
  }
  .mini-btn{
    padding:9px 12px;
    border-radius:10px;
    background:#0f172a;
  }
  .mini-btn.ok{background:#166534}
  .mini-btn.fail{background:#991b1b}
  .hidden{display:none !important;}
  @media (max-width:820px){
    .grid,.card-grid,.btn-row{grid-template-columns:1fr}
    .headline{font-size:22px}
  }
</style>
</head>
<body>

<canvas id="snow"></canvas>

<div class="topbar">
  <div class="nav-left">
    <button class="nav-btn primary" onclick="show('home')">Trang chủ</button>
    <button class="nav-btn" onclick="show('shop')">Nạp thẻ</button>
    <button class="nav-btn" onclick="show('account')">Đăng ký / Đăng nhập</button>
  </div>
  <div class="user-box" id="userBox">
    <span class="chip">Chưa đăng nhập</span>
  </div>
</div>

<div class="wrap">
  <section class="banner" id="home">
    <h1 class="headline">🔥 10k = 90 robux mua nhanh kẻo hết !!!</h1>
    <p class="subline">admin chỉ duyệt thẻ lúc 12:00-7:00 tối hoặc lúc rảnh 😅</p>
    <div class="notice">Robux siêu hạt dẻ</div>
  </section>

  <section class="section active" id="homeSection">
    <div class="panel">
      <div class="title">Robux siêu hạt dẻ</div>
      <div class="subtitle">Bấm vào gói bên dưới để xem giá</div>

      <div class="card-grid">
        <div class="card">
          <h3>Gói 90 Robux</h3>
          <div class="price">10k</div>
          <ul>
            <li>Giao diện mua nhanh</li>
            <li>Hiển thị giá đẹp</li>
            <li>Demo an toàn</li>
          </ul>
          <div class="small">Bấm nút để mở trang mua</div>
        </div>

        <div class="card">
          <h3>Gói 180 Robux</h3>
          <div class="price">20k</div>
          <ul>
            <li>Giao diện đơn giản</li>
            <li>Tối màu, dễ nhìn</li>
            <li>Hiệu ứng tuyết rơi</li>
          </ul>
          <div class="small">Bấm nút để mở trang mua</div>
        </div>

        <div class="card">
          <h3>Gói 450 Robux</h3>
          <div class="price">50k</div>
          <ul>
            <li>Bản demo shop</li>
            <li>Không xử lý thanh toán thật</li>
            <li>Chỉ mô phỏng giao diện</li>
          </ul>
          <div class="small">Bấm nút để mở trang mua</div>
        </div>
      </div>

      <div class="btn-row">
        <button class="btn" onclick="show('shop')">Mở trang mua Robux</button>
        <button class="btn gray" onclick="show('account')">Đăng ký / Đăng nhập</button>
      </div>
    </div>
  </section>

  <se  <!-- Tiếp nối phần <section class="section active" id="homeSection"> -->
  
  <!-- SECTION NẠP THẺ -->
  <section class="section" id="shopSection">
    <div class="panel">
      <h2 class="title">Nạp Thẻ Cào</h2>
      <p class="subtitle">Tỉ lệ: 10k VNĐ = 90 Robux</p>
      
      <div class="grid">
        <div>
          <label>Loại thẻ</label>
          <select id="cardType">
            <option value="Viettel">Viettel</option>
            <option value="Vinaphone">Vinaphone</option>
            <option value="Mobifone">Mobifone</option>
            <option value="Zing">Zing</option>
          </select>
        </div>
        <div>
          <label>Mệnh giá</label>
          <select id="cardValue" onchange="updateRobux(this.value)">
            <option value="10000">10,000đ (90 R$)</option>
            <option value="20000">20,000đ (180 R$)</option>
            <option value="50000">50,000đ (450 R$)</option>
            <option value="100000">100,000đ (900 R$)</option>
          </select>
        </div>
      </div>

      <div class="grid" style="margin-top:15px">
        <div>
          <label>Mã thẻ</label>
          <input type="text" id="cardCode" placeholder="1000...">
        </div>
        <div>
          <label>Số seri</label>
          <input type="text" id="cardSeri" placeholder="1000...">
        </div>
      </div>

      <button class="btn" style="margin-top:20px" onclick="submitCard()">Gửi thẻ ngay</button>
      <div id="shopMsg" class="result" style="display:none"></div>
    </div>
  </section>

  <!-- SECTION ĐĂNG NHẬP -->
  <section class="section" id="accountSection">
    <div class="panel" style="max-width:450px; margin: 0 auto;">
      <h2 class="title">Tài Khoản</h2>
      <div id="loginForm">
        <label>Tên đăng nhập</label>
        <input type="text" id="username" placeholder="admin123">
        <br><br>
        <label>Mật khẩu</label>
        <input type="password" id="password" placeholder="••••••••">
        <button class="btn" style="margin-top:20px" onclick="login()">Đăng Nhập</button>
        <p class="muted" style="text-align:center; margin-top:15px">Chưa có tài khoản? <a href="#" style="color:var(--accent)">Đăng ký ngay</a></p>
      </div>
      <div id="userInfo" class="hidden">
          <p>Xin chào, <strong id="displayUser"></strong>!</p>
          <p>Số dư: <span style="color:var(--good)">0đ</span> | Robux: <span style="color:var(--accent2)">0</span></p>
          <button class="btn gray" onclick="logout()">Đăng xuất</button>
      </div>
    </div>
  </section>
</div>

<!-- Nút Admin ảo -->
<button class="admin-fab" onclick="toggleAdmin()">⚙️</button>

<script>
  // Chuyển đổi các trang
  function show(page) {
    document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
    if(page === 'home') document.getElementById('homeSection').classList.add('active');
    if(page === 'shop') document.getElementById('shopSection').classList.add('active');
    if(page === 'account') document.getElementById('accountSection').classList.add('active');
  }

  // Giả lập Đăng nhập
  function login() {
    const user = document.getElementById('username').value;
    if(user) {
      document.getElementById('loginForm').classList.add('hidden');
      document.getElementById('userInfo').classList.remove('hidden');
      document.getElementById('displayUser').innerText = user;
      document.getElementById('userBox').innerHTML = `<span class="chip">${user} - 0đ</span>`;
    }
  }

  function logout() {
    location.reload();
  }

  // Gi giả lập Nạp thẻ
  function submitCard() {
    const msg = document.getElementById('shopMsg');
    msg.style.display = 'block';
    msg.innerText = "Đang gửi thẻ... vui lòng đợi admin duyệt!";
    msg.style.color = "var(--warn)";
    
    setTimeout(() => {
      msg.innerText = "Gửi thẻ thành công! Lịch sử nạp sẽ cập nhật sau 5-10p.";
      msg.style.color = "var(--good)";
    }, 2000);
  }

  // Hiệu ứng tuyết rơi đơn giản
  const canvas = document.getElementById('snow');
  const ctx = canvas.getContext('2d');
  let particles = [];

  function resize() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }
  window.addEventListener('resize', resize);
  resize();

  for(let i=0; i<50; i++) {
    particles.push({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      r: Math.random() * 3 + 1,
      v: Math.random() * 1 + 0.5
    });
  }

  function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = "rgba(255,255,255,0.3)";
    ctx.beginPath();
    particles.forEach(p => {
      ctx.moveTo(p.x, p.y);
      ctx.arc(p.x, p.y, p.r, 0, Math.PI*2);
      p.y += p.v;
      if(p.y > canvas.height) p.y = -10;
    });
    ctx.fill();
    requestAnimationFrame(draw);
  }
  draw();
</script>
</body>
</html>
