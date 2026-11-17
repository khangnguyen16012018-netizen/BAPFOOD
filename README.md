# BAPFOOD
App Giao Hàng
<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bapfood - Đặt món thông minh</title>
<style>
  * { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }
  body {
    font-family: -apple-system, 'Segoe UI', Roboto, sans-serif;
    background: linear-gradient(135deg, #fff5eb 0%, #ffe8d1 100%);
    min-height: 100vh;
    display: flex; justify-content: center; align-items: flex-start;
    padding: 20px 0;
    color: #2d2d2d;
  }
  .app {
    width: 100%; max-width: 430px;
    background: #fff;
    border-radius: 28px;
    box-shadow: 0 25px 60px rgba(255,107,53,0.25);
    overflow: hidden;
    position: relative;
    min-height: 90vh;
    display: flex; flex-direction: column;
  }
  /* HEADER */
  .header {
    background: linear-gradient(135deg, #ff6b35 0%, #f7931e 100%);
    color: #fff; padding: 20px 20px 16px;
    border-radius: 0 0 24px 24px;
    position: sticky; top: 0; z-index: 50;
  }
  .header-top { display:flex; justify-content:space-between; align-items:center; }
  .logo { font-size: 24px; font-weight: 800; letter-spacing:-0.5px; display:flex; align-items:center; gap:6px; }
  .logo .dot { width:10px; height:10px; background:#fff; border-radius:50%; }
  .header-actions { display:flex; gap:14px; }
  .header-actions span { font-size: 20px; cursor:pointer; }
  .search-bar {
    margin-top: 14px; background: rgba(255,255,255,0.95);
    border-radius: 14px; display: flex; align-items: center;
    padding: 12px 16px; gap: 10px;
  }
  .search-bar input { border:none; outline:none; flex:1; font-size:14px; background:transparent; color:#333; }
  /* VIEWS */
  .view { display: none; padding: 20px; flex:1; overflow-y:auto; }
  .view.active { display: block; animation: fadeIn 0.3s ease; }
  @keyframes fadeIn { from{opacity:0; transform:translateY(8px);} to{opacity:1; transform:translateY(0);} }
  h2 { font-size: 20px; margin-bottom: 14px; color:#2d2d2d; }
  h3 { font-size: 16px; margin: 18px 0 10px; color:#2d2d2d; }
  /* BANNER */
  .banner {
    background: linear-gradient(135deg, #ff6b35 0%, #ffa94d 100%);
    color: #fff; border-radius: 18px; padding: 20px;
    margin-bottom: 20px; position: relative; overflow: hidden;
  }
  .banner::after {
    content:'🍜'; position:absolute; right:-10px; bottom:-20px;
    font-size:100px; opacity:0.25; transform: rotate(-15deg);
  }
  .banner h3 { color:#fff; font-size:18px; margin:0 0 6px; }
  .banner p { font-size:13px; opacity:0.95; }
  /* CATEGORY PILLS */
  .cats { display:flex; gap:10px; overflow-x:auto; padding-bottom: 6px; margin-bottom:6px; scrollbar-width:none; }
  .cats::-webkit-scrollbar { display:none; }
  .cat {
    flex-shrink:0; padding:10px 18px; border-radius: 999px;
    background:#fff5eb; color:#ff6b35; font-size:13px; font-weight:600;
    border: 1.5px solid transparent; cursor:pointer; transition:all 0.2s; white-space:nowrap;
  }
  .cat.active, .cat:hover { background:#ff6b35; color:#fff; border-color:#ff6b35; }
  /* FOOD GRID */
  .foods { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
  .food-card {
    background:#fff; border-radius: 18px; overflow: hidden;
    box-shadow: 0 4px 14px rgba(0,0,0,0.06);
    cursor: pointer; transition: transform 0.15s;
  }
  .food-card:active { transform: scale(0.97); }
  .food-card img { width:100%; height: 110px; object-fit: cover; display:block; }
  .food-info { padding: 10px 12px 12px; }
  .food-info h4 { font-size:14px; font-weight:700; margin-bottom:4px; color:#2d2d2d; }
  .food-info .price { color:#ff6b35; font-weight:800; font-size:14px; }
  .food-info .meta { font-size:11px; color:#999; margin-top:4px; }
  /* FOOD DETAIL */
  .detail-img { width:100%; height:220px; object-fit:cover; border-radius:18px; }
  .detail-title { font-size:22px; margin:16px 0 6px; font-weight:800; }
  .detail-price { color:#ff6b35; font-size:22px; font-weight:800; margin-bottom:10px; }
  .detail-desc { color:#666; font-size:14px; line-height:1.6; margin-bottom:16px; }
  .qty-row { display:flex; align-items:center; gap:16px; margin: 16px 0; }
  .qty-btn {
    width:40px; height:40px; border-radius:50%; border:none;
    background:#fff5eb; color:#ff6b35; font-size:20px; font-weight:700; cursor:pointer;
  }
  .qty-btn:active { transform:scale(0.9); }
  .qty-num { font-size:18px; font-weight:700; min-width:30px; text-align:center; }
  .btn-primary {
    width:100%; padding:16px; border:none; border-radius:16px;
    background: linear-gradient(135deg, #ff6b35 0%, #f7931e 100%);
    color:#fff; font-size:16px; font-weight:700; cursor:pointer;
    box-shadow: 0 6px 20px rgba(255,107,53,0.35);
  }
  .btn-primary:active { transform: scale(0.98); }
  .btn-secondary {
    width:100%; padding:14px; border:2px solid #ff6b35; border-radius:16px;
    background:transparent; color:#ff6b35; font-size:15px; font-weight:700; cursor:pointer;
    margin-top:10px;
  }
  /* SMART LUNCH */
  .combo {
    background: linear-gradient(135deg, #fff5eb, #ffe0c2);
    border-radius: 18px; padding: 16px; margin-bottom:14px;
    display:flex; gap:14px; align-items:center;
    border: 1.5px dashed #ffb37a;
  }
  .combo-img { width:90px; height:90px; border-radius:14px; object-fit:cover; flex-shrink:0; }
  .combo-info h4 { font-size:15px; margin-bottom:4px; }
  .combo-info p { font-size:12px; color:#666; margin-bottom:6px; }
  .combo-price { color:#ff6b35; font-weight:800; font-size:15px; }
  .combo-price .old { color:#aaa; text-decoration:line-through; font-size:12px; font-weight:600; margin-left:6px; }
  .combo button {
    background:#ff6b35; color:#fff; border:none; padding:8px 14px;
    border-radius:10px; font-size:12px; font-weight:700; cursor:pointer; margin-top:6px;
  }
  /* CART */
  .cart-item {
    display:flex; gap:12px; padding:12px; background:#fff5eb;
    border-radius:14px; margin-bottom:10px; align-items:center;
  }
  .cart-item img { width:60px; height:60px; border-radius:10px; object-fit:cover; }
  .cart-item-info { flex:1; }
  .cart-item-info h4 { font-size:14px; margin-bottom:4px; }
  .cart-item-info .price { color:#ff6b35; font-weight:700; font-size:13px; }
  .cart-qty { display:flex; align-items:center; gap:8px; margin-top:6px; }
  .cart-qty button { width:26px; height:26px; border-radius:50%; border:none; background:#fff; color:#ff6b35; font-weight:700; cursor:pointer; }
  .cart-qty span { font-size:13px; font-weight:700; min-width:20px; text-align:center; }
  .cart-summary {
    background: #2d2d2d; color:#fff; border-radius:16px; padding:16px; margin-top:16px;
  }
  .summary-row { display:flex; justify-content:space-between; margin-bottom:8px; font-size:14px; }
  .summary-row.total { font-size:18px; font-weight:800; color:#ffb37a; margin-top:12px; padding-top:12px; border-top:1px solid rgba(255,255,255,0.15); }
  .empty-cart { text-align:center; padding:60px 20px; color:#999; }
  .empty-cart .icon { font-size:60px; margin-bottom:14px; }
  /* CHECKOUT FORM */
  .form-group { margin-bottom:14px; }
  .form-group label { display:block; font-size:13px; font-weight:600; color:#555; margin-bottom:6px; }
  .form-group input, .form-group textarea, .form-group select {
    width:100%; padding:14px; border:1.5px solid #eee; border-radius:12px;
    font-size:14px; outline:none; font-family:inherit; background:#fafafa;
  }
  .form-group input:focus, .form-group textarea:focus, .form-group select:focus {
    border-color:#ff6b35; background:#fff;
  }
  .pay-options { display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:16px; }
  .pay-opt {
    padding:14px 10px; border:2px solid #eee; border-radius:12px;
    text-align:center; cursor:pointer; font-size:13px; font-weight:600; color:#666;
    background:#fafafa;
  }
  .pay-opt.selected { border-color:#ff6b35; color:#ff6b35; background:#fff5eb; }
  .pay-opt .icon { display:block; font-size:22px; margin-bottom:4px; }
  /* TRACKING */
  .tracking-steps { position:relative; padding-left:36px; margin-top:20px; }
  .tracking-steps::before {
    content:''; position:absolute; left:14px; top:8px; bottom:8px;
    width:3px; background:#eee; border-radius:2px;
  }
  .step { position:relative; padding-bottom:24px; }
  .step:last-child { padding-bottom:0; }
  .step::before {
    content:''; position:absolute; left:-28px; top:2px;
    width:22px; height:22px; border-radius:50%;
    background:#eee; border:3px solid #fff; box-shadow:0 0 0 2px #eee;
  }
  .step.done::before { background:#ff6b35; box-shadow:0 0 0 2px #ff6b35; }
  .step.current::before { background:#fff; box-shadow:0 0 0 2px #ff6b35; animation:pulse 1.5s infinite; }
  @keyframes pulse { 0%,100%{box-shadow:0 0 0 2px #ff6b35;} 50%{box-shadow:0 0 0 6px rgba(255,107,53,0.3);} }
  .step h4 { font-size:14px; margin-bottom:2px; }
  .step.done h4 { color:#ff6b35; }
  .step p { font-size:12px; color:#888; }
  /* BOTTOM NAV */
  .nav {
    display:flex; background:#fff; border-top:1px solid #f0f0f0;
    padding:10px 0 14px; position:sticky; bottom:0;
  }
  .nav-item {
    flex:1; text-align:center; cursor:pointer; color:#aaa;
    font-size:11px; font-weight:600; transition:color 0.2s;
  }
  .nav-item .icon { font-size:20px; display:block; margin-bottom:2px; }
  .nav-item.active { color:#ff6b35; }
  /* TOAST */
  .toast {
    position:fixed; top:30px; left:50%; transform:translateX(-50%) translateY(-100px);
    background:#2d2d2d; color:#fff; padding:12px 22px; border-radius:999px;
    font-size:13px; font-weight:600; transition:transform 0.3s ease; z-index:999;
    box-shadow:0 8px 24px rgba(0,0,0,0.2);
  }
  .toast.show { transform:translateX(-50%) translateY(0); }
  /* QR SECTION */
  .qr-section {
    padding: 24px 20px; background:#fafafa; text-align:center;
    border-top: 1px dashed #eee;
  }
  .qr-section h3 { margin:0 0 8px; font-size:16px; color:#2d2d2d; }
  .qr-section p { font-size:12px; color:#888; margin-bottom:14px; }
  .qr-section img { border-radius:12px; padding:8px; background:#fff; box-shadow:0 4px 14px rgba(0,0,0,0.08); }
  /* PROGRESS */
  .progress-bar {
    height:6px; background:#eee; border-radius:3px; overflow:hidden; margin:10px 0;
  }
  .progress-fill {
    height:100%; background:linear-gradient(90deg, #ff6b35, #ffb37a);
    transition:width 1s ease; border-radius:3px;
  }
  .order-id {
    background:#fff5eb; border-radius:12px; padding:14px;
    font-size:13px; color:#666; margin-bottom:16px;
    display:flex; justify-content:space-between;
  }
  .order-id strong { color:#ff6b35; }
</style>
</head>
<body>
<div class="app" id="app">

  <!-- HEADER -->
  <div class="header" id="header">
    <div class="header-top">
      <div class="logo">Bapfood <span class="dot"></span></div>
      <div class="header-actions">
        <span onclick="goTo('cart')">🛒</span>
        <span>👤</span>
      </div>
    </div>
    <div class="search-bar">
      <span>🔍</span>
      <input id="globalSearch" type="text" placeholder="Tìm món ăn bạn muốn..." oninput="handleSearch(this.value)">
    </div>
  </div>

  <!-- VIEW: HOME -->
  <div class="view active" id="view-home">
    <div class="banner">
      <h3>Chào buổi trưa! 👋</h3>
      <p>Hôm nay ăn gì? Bapfood gợi ý cho bạn những món ngon nhất.</p>
    </div>
    <div class="cats" id="homeCats"></div>
    <h3 style="margin-top:16px;">🔥 Món phổ biến</h3>
    <div class="foods" id="homeFoods"></div>
  </div>

  <!-- VIEW: MENU -->
  <div class="view" id="view-menu">
    <h2>Thực đơn</h2>
    <div class="cats" id="menuCats"></div>
    <div class="foods" id="menuFoods" style="margin-top:14px;"></div>
  </div>

  <!-- VIEW: SMART LUNCH -->
  <div class="view" id="view-smart">
    <h2>⚡ Smart Lunch</h2>
    <p style="color:#888; font-size:13px; margin-bottom:16px;">Combo bữa trưa thông minh — tiết kiệm hơn, đủ chất hơn.</p>
    <div id="combos"></div>
  </div>

  <!-- VIEW: DETAIL -->
  <div class="view" id="view-detail">
    <div id="detailContent"></div>
  </div>

  <!-- VIEW: CART -->
  <div class="view" id="view-cart">
    <h2>🛒 Giỏ hàng</h2>
    <div id="cartContent"></div>
  </div>

  <!-- VIEW: CHECKOUT -->
  <div class="view" id="view-checkout">
    <h2>💳 Thanh toán</h2>
    <div class="form-group">
      <label>Họ và tên</label>
      <input type="text" id="ckName" placeholder="Nguyễn Văn A">
    </div>
    <div class="form-group">
      <label>Số điện thoại</label>
      <input type="tel" id="ckPhone" placeholder="0901234567">
    </div>
    <div class="form-group">
      <label>Địa chỉ giao hàng</label>
      <textarea id="ckAddress" rows="2" placeholder="Số nhà, đường, quận..."></textarea>
    </div>
    <label style="display:block; font-size:13px; font-weight:600; color:#555; margin-bottom:8px;">Phương thức thanh toán</label>
    <div class="pay-options">
      <div class="pay-opt selected" data-pay="cash" onclick="selectPay(this)">
        <span class="icon">💵</span>Tiền mặt
      </div>
      <div class="pay-opt" data-pay="bank" onclick="selectPay(this)">
        <span class="icon">🏦</span>Chuyển khoản
      </div>
      <div class="pay-opt" data-pay="momo" onclick="selectPay(this)">
        <span class="icon">📱</span>MoMo
      </div>
      <div class="pay-opt" data-pay="card" onclick="selectPay(this)">
        <span class="icon">💳</span>Thẻ
      </div>
    </div>
    <div class="cart-summary" id="ckSummary"></div>
    <button class="btn-primary" style="margin-top:16px;" onclick="placeOrder()">Đặt hàng</button>
  </div>

  <!-- VIEW: TRACKING -->
  <div class="view" id="view-tracking">
    <h2>📍 Theo dõi đơn hàng</h2>
    <div class="order-id">
      <span>Mã đơn: <strong id="orderCode">#BF00000</strong></span>
      <span id="orderTime">--:--</span>
    </div>
    <div class="progress-bar"><div class="progress-fill" id="progressFill" style="width:25%"></div></div>
    <div class="tracking-steps" id="trackingSteps"></div>
    <button class="btn-secondary" style="margin-top:24px;" onclick="goTo('home')">Về trang chủ</button>
  </div>

  <!-- BOTTOM NAV -->
  <div class="nav">
    <div class="nav-item active" data-view="home" onclick="goTo('home')"><span class="icon">🏠</span>Trang chủ</div>
    <div class="nav-item" data-view="menu" onclick="goTo('menu')"><span class="icon">🍽️</span>Thực đơn</div>
    <div class="nav-item" data-view="smart" onclick="goTo('smart')"><span class="icon">⚡</span>Smart Lunch</div>
    <div class="nav-item" data-view="cart" onclick="goTo('cart')"><span class="icon">🛒</span>Giỏ hàng</div>
    <div class="nav-item" data-view="tracking" onclick="goTo('tracking')"><span class="icon">📦</span>Đơn hàng</div>
  </div>

  <!-- QR SECTION -->
  <div class="qr-section">
    <h3>📲 Quét mã để trải nghiệm</h3>
    <p>Đưa điện thoại quét mã QR bên dưới để mở app</p>
    <img id="qrImg" alt="QR" width="160" height="160">
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ===== DATA =====
const categories = [
  { id: 'all', name: 'Tất cả' },
  { id: 'mon-nuoc', name: 'Món nước' },
  { id: 'com', name: 'Cơm' },
  { id: 'banh-mi', name: 'Bánh mì' },
  { id: 'healthy', name: 'Healthy' },
  { id: 'uong', name: 'Đồ uống' }
];

const foods = [
  { id:1, name:'Phở bò Hà Nội', price:45000, cat:'mon-nuoc', rating:4.8, sold:1200, img:'https://images.unsplash.com/photo-1582878826629-29b7ad1cdc43?w=400', desc:'Phở bò truyền thống với nước dùng hầm xương 12 tiếng, thịt bò tái mềm và bánh phở dai ngon.' },
  { id:2, name:'Cơm tấm sườn bì', price:50000, cat:'com', rating:4.9, sold:980, img:'https://images.unsplash.com/photo-1603133872878-684f208fb84b?w=400', desc:'Cơm tấm Sài Gòn với sườn nướng mật ong, bì heo và nước mắm chua ngọt đặc trưng.' },
  { id:3, name:'Bánh mì thịt nướng', price:30000, cat:'banh-mi', rating:4.7, sold:2100, img:'https://images.unsplash.com/photo-1600454309261-3dc9b7599982?w=400', desc:'Bánh mì giòn với thịt nướng thơm phức, rau sống và nước sốt đặc biệt.' },
  { id:4, name:'Bún chả Hà Nội', price:45000, cat:'mon-nuoc', rating:4.8, sold:760, img:'https://images.unsplash.com/photo-1555126634-323283e090fa?w=400', desc:'Bún chả nướng than hoa, ăn kèm nước mắm chua ngọt và rau sống tươi.' },
  { id:5, name:'Salad ức gà', price:55000, cat:'healthy', rating:4.6, sold:540, img:'https://images.unsplash.com/photo-1512621776951-a57141f2eefd?w=400', desc:'Salad tươi mát với ức gà áp chảo, rau xanh và sốt mè rang thơm ngon.' },
  { id:6, name:'Gà rán giòn', price:65000, cat:'com', rating:4.7, sold:1500, img:'https://images.unsplash.com/photo-1626645738196-c2a7c87a8f58?w=400', desc:'Gà rán giòn rụm với lớp vỏ vàng ươm, ăn kèm khoai tây chiên.' },
  { id:7, name:'Trà sữa trân châu', price:35000, cat:'uong', rating:4.9, sold:2800, img:'https://images.unsplash.com/photo-1558857563-b371033873b8?w=400', desc:'Trà sữa trân châu đường đen thơm béo, đá xay mát lạnh.' },
  { id:8, name:'Cơm gà Hải Nam', price:52000, cat:'com', rating:4.8, sold:890, img:'https://images.unsplash.com/photo-1598515214211-89d3c73ae83b?w=400', desc:'Cơm gà luộc mềm, nước dùng gà thanh ngọt, ăn kèm gừng và tương ớt.' },
  { id:9, name:'Bún bò Huế', price:50000, cat:'mon-nuoc', rating:4.7, sold:620, img:'https://images.unsplash.com/photo-1582878826629-29b7ad1cdc43?w=400', desc:'Bún bò Huế cay nồng với giò heo, thịt bò và sả thơm đặc trưng.' },
  { id:10, name:'Sinh tố bơ', price:30000, cat:'uong', rating:4.8, sold:1100, img:'https://images.unsplash.com/photo-1553530666-ba11a7da3888?w=400', desc:'Sinh tố bơ sánh mịn, béo ngậy, giàu dinh dưỡng.' }
];

const combos = [
  { id:'c1', name:'Combo Tiết kiệm', desc:'Cơm tấm sườn + Trà đá', price:45000, old:55000, img:'https://images.unsplash.com/photo-1603133872878-684f208fb84b?w=400', items:[2] },
  { id:'c2', name:'Combo Healthy', desc:'Salad ức gà + Sinh tố bơ', price:70000, old:85000, img:'https://images.unsplash.com/photo-1512621776951-a57141f2eefd?w=400', items:[5,10] },
  { id:'c3', name:'Combo No bụng', desc:'Phở bò + Trà sữa', price:70000, old:80000, img:'https://images.unsplash.com/photo-1582878826629-29b7ad1cdc43?w=400', items:[1,7] }
];

// ===== STATE =====
let cart = JSON.parse(localStorage.getItem('bapfood_cart') || '[]');
let currentFood = null;
let currentQty = 1;
let currentCat = 'all';
let selectedPay = 'cash';

// ===== HELPERS =====
const $ = id => document.getElementById(id);
const formatPrice = n => n.toLocaleString('vi-VN') + 'đ';
const saveCart = () => localStorage.setItem('bapfood_cart', JSON.stringify(cart));

function toast(msg) {
  const t = $('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(t._timer);
  t._timer = setTimeout(() => t.classList.remove('show'), 2000);
}

function goTo(view) {
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  const el = $('view-' + view);
  if (el) el.classList.add('active');
  document.querySelectorAll('.nav-item').forEach(n => n.classList.toggle('active', n.dataset.view === view));
  // Hide header on detail / checkout for cleaner look
  $('header').style.display = (view === 'detail' || view === 'checkout') ? 'none' : 'block';
  window.scrollTo({ top: 0, behavior: 'smooth' });
  if (view === 'cart') renderCart();
  if (view === 'checkout') renderCheckout();
  if (view === 'tracking') renderTracking();
}

// ===== RENDER =====
function renderHome() {
  const catHTML = categories.map(c =>
    `<div class="cat ${c.id==='all'?'active':''}" onclick="filterHome('${c.id}', this)">${c.name}</div>`
  ).join('');
  $('homeCats').innerHTML = catHTML;
  renderFoods('homeFoods', foods.slice(0, 6));
}

function renderMenu() {
  const catHTML = categories.map(c =>
    `<div class="cat ${c.id===currentCat?'active':''}" onclick="filterMenu('${c.id}', this)">${c.name}</div>`
  ).join('');
  $('menuCats').innerHTML = catHTML;
  const list = currentCat === 'all' ? foods : foods.filter(f => f.cat === currentCat);
  renderFoods('menuFoods', list);
}

function renderFoods(containerId, list) {
  const html = list.map(f => `
    <div class="food-card" onclick="showDetail(${f.id})">
      <img src="${f.img}" alt="${f.name}" loading="lazy">
      <div class="food-info">
        <h4>${f.name}</h4>
        <div class="price">${formatPrice(f.price)}</div>
        <div class="meta">⭐ ${f.rating} • ${f.sold} đã bán</div>
      </div>
    </div>
  `).join('');
  $(containerId).innerHTML = html;
}

function renderCombos() {
  $('combos').innerHTML = combos.map(c => `
    <div class="combo">
      <img src="${c.img}" class="combo-img" alt="${c.name}">
      <div class="combo-info" style="flex:1;">
        <h4>${c.name}</h4>
        <p>${c.desc}</p>
        <div class="combo-price">${formatPrice(c.price)} <span class="old">${formatPrice(c.old)}</span></div>
        <button onclick="addCombo('${c.id}')">+ Thêm vào giỏ</button>
      </div>
    </div>
  `).join('');
}

function filterHome(cat, el) {
  document.querySelectorAll('#homeCats .cat').forEach(c => c.classList.remove('active'));
  el.classList.add('active');
  const list = cat === 'all' ? foods.slice(0, 6) : foods.filter(f => f.cat === cat);
  renderFoods('homeFoods', list);
}

function filterMenu(cat, el) {
  currentCat = cat;
  document.querySelectorAll('#menuCats .cat').forEach(c => c.classList.remove('active'));
  el.classList.add('active');
  const list = cat === 'all' ? foods : foods.filter(f => f.cat === cat);
  renderFoods('menuFoods', list);
}

function handleSearch(q) {
  const query = q.trim().toLowerCase();
  if (!query) { renderMenu(); return; }
  goTo('menu');
  const list = foods.filter(f => f.name.toLowerCase().includes(query));
  renderFoods('menuFoods', list);
}

// ===== DETAIL =====
function showDetail(id) {
  currentFood = foods.find(f => f.id === id);
  currentQty = 1;
  const f = currentFood;
  $('detailContent').innerHTML = `
    <button onclick="goTo('home')" style="background:none;border:none;font-size:22px;cursor:pointer;margin-bottom:10px;">←</button>
    <img src="${f.img}" class="detail-img" alt="${f.name}">
    <h2 class="detail-title">${f.name}</h2>
    <div style="display:flex; gap:10px; align-items:center; margin-bottom:8px;">
      <span style="color:#f7931e;">⭐ ${f.rating}</span>
      <span style="color:#999; font-size:13px;">• ${f.sold} đã bán</span>
    </div>
    <div class="detail-price">${formatPrice(f.price)}</div>
    <p class="detail-desc">${f.desc}</p>
    <h3>Số lượng</h3>
    <div class="qty-row">
      <button class="qty-btn" onclick="changeQty(-1)">−</button>
      <span class="qty-num" id="detailQty">1</span>
      <button class="qty-btn" onclick="changeQty(1)">+</button>
    </div>
    <button class="btn-primary" onclick="addCurrentToCart()">🛒 Thêm vào giỏ • ${formatPrice(f.price)}</button>
  `;
  goTo('detail');
}

function changeQty(delta) {
  currentQty = Math.max(1, currentQty + delta);
  $('detailQty').textContent = currentQty;
}

function addCurrentToCart() {
  addToCart(currentFood.id, currentQty);
  toast(`Đã thêm ${currentQty} × ${currentFood.name}`);
}

// ===== CART =====
function addToCart(id, qty = 1) {
  const item = cart.find(c => c.id === id);
  if (item) item.qty += qty;
  else cart.push({ id, qty });
  saveCart();
  updateCartBadge();
}

function addCombo(comboId) {
  const combo = combos.find(c => c.id === comboId);
  combo.items.forEach(fid => addToCart(fid, 1));
  toast(`Đã thêm ${combo.name}`);
}

function changeCartQty(id, delta) {
  const item = cart.find(c => c.id === id);
  if (!item) return;
  item.qty += delta;
  if (item.qty <= 0) cart = cart.filter(c => c.id !== id);
  saveCart();
  renderCart();
  updateCartBadge();
}

function renderCart() {
  if (cart.length === 0) {
    $('cartContent').innerHTML = `
      <div class="empty-cart">
        <div class="icon">🛒</div>
        <h3>Giỏ hàng trống</h3>
        <p>Hãy thêm món ăn yêu thích của bạn nhé!</p>
        <button class="btn-primary" style="margin-top:16px;" onclick="goTo('menu')">Xem thực đơn</button>
      </div>`;
    return;
  }
  const itemsHTML = cart.map(c => {
    const f = foods.find(x => x.id === c.id);
    return `
      <div class="cart-item">
        <img src="${f.img}" alt="${f.name}">
        <div class="cart-item-info">
          <h4>${f.name}</h4>
          <div class="price">${formatPrice(f.price)}</div>
          <div class="cart-qty">
            <button onclick="changeCartQty(${f.id}, -1)">−</button>
            <span>${c.qty}</span>
            <button onclick="changeCartQty(${f.id}, 1)">+</button>
          </div>
        </div>
      </div>`;
  }).join('');
  const subtotal = cart.reduce((s, c) => s + foods.find(f => f.id === c.id).price * c.qty, 0);
  const shipping = 15000;
  $('cartContent').innerHTML = `
    ${itemsHTML}
    <div class="cart-summary">
      <div class="summary-row"><span>Tạm tính</span><span>${formatPrice(subtotal)}</span></div>
      <div class="summary-row"><span>Phí giao hàng</span><span>${formatPrice(shipping)}</span></div>
      <div class="summary-row total"><span>Tổng cộng</span><span>${formatPrice(subtotal + shipping)}</span></div>
    </div>
    <button class="btn-primary" style="margin-top:16px;" onclick="goTo('checkout')">Tiến hành thanh toán</button>
  `;
}

function updateCartBadge() {
  const count = cart.reduce((s, c) => s + c.qty, 0);
  const nav = document.querySelector('.nav-item[data-view="cart"]');
  if (nav) {
    nav.querySelector('.icon').textContent = count > 0 ? `🛒${count}` : '🛒';
  }
}

// ===== CHECKOUT =====
function selectPay(el) {
  document.querySelectorAll('.pay-opt').forEach(p => p.classList.remove('selected'));
  el.classList.add('selected');
  selectedPay = el.dataset.pay;
}

function renderCheckout() {
  const subtotal = cart.reduce((s, c) => s + foods.find(f => f.id === c.id).price * c.qty, 0);
  const shipping = 15000;
  const itemsHTML = cart.map(c => {
    const f = foods.find(x => x.id === c.id);
    return `<div class="summary-row"><span>${c.qty} × ${f.name}</span><span>${formatPrice(f.price * c.qty)}</span></div>`;
  }).join('');
  $('ckSummary').innerHTML = `
    ${itemsHTML}
    <div class="summary-row" style="margin-top:8px; border-top:1px solid rgba(255,255,255,0.15); padding-top:8px;"><span>Phí giao hàng</span><span>${formatPrice(shipping)}</span></div>
    <div class="summary-row total"><span>Tổng cộng</span><span>${formatPrice(subtotal + shipping)}</span></div>
  `;
}

function placeOrder() {
  const name = $('ckName').value.trim();
  const phone = $('ckPhone').value.trim();
  const addr = $('ckAddress').value.trim();
  if (!name || !phone || !addr) {
    toast('Vui lòng điền đầy đủ thông tin!');
    return;
  }
  if (cart.length === 0) {
    toast('Giỏ hàng trống!');
    return;
  }
  // Generate order ID
  const orderId = 'BF' + String(Math.floor(Math.random() * 90000) + 10000);
  const time = new Date().toLocaleTimeString('vi-VN', { hour: '2-digit', minute: '2-digit' });
  $('orderCode').textContent = '#' + orderId;
  $('orderTime').textContent = time;
  // Clear cart
  cart = [];
  saveCart();
  updateCartBadge();
  goTo('tracking');
  // Simulate progress
  simulateOrderProgress();
  toast('Đặt hàng thành công! 🎉');
}

// ===== TRACKING =====
const trackingSteps = [
  { name: 'Đã xác nhận đơn', desc: 'Bapfood đã nhận đơn của bạn' },
  { name: 'Đang chuẩn bị', desc: 'Bếp đang chuẩn bị món ăn' },
  { name: 'Đang giao hàng', desc: 'Shipper đang trên đường giao' },
  { name: 'Hoàn thành', desc: 'Chúc bạn ngon miệng!' }
];

let trackingStepIndex = 0;
let trackingTimer = null;

function renderTracking() {
  const html = trackingSteps.map((s, i) => {
    let cls = '';
    if (i < trackingStepIndex) cls = 'done';
    else if (i === trackingStepIndex) cls = 'current';
    return `<div class="step ${cls}"><h4>${s.name}</h4><p>${s.desc}</p></div>`;
  }).join('');
  $('trackingSteps').innerHTML = html;
  $('progressFill').style.width = ((trackingStepIndex + 1) / trackingSteps.length * 100) + '%';
}

function simulateOrderProgress() {
  if (trackingTimer) clearInterval(trackingTimer);
  trackingStepIndex = 0;
  renderTracking();
  trackingTimer = setInterval(() => {
    if (trackingStepIndex < trackingSteps.length - 1) {
      trackingStepIndex++;
      renderTracking();
    } else {
      clearInterval(trackingTimer);
    }
  }, 5000);
}

// ===== QR CODE =====
function renderQR() {
  const url = window.location.href.split('#')[0];
  $('qrImg').src = `https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=${encodeURIComponent(url)}&color=ff6b35&bgcolor=ffffff`;
}

// ===== INIT =====
function init() {
  renderHome();
  renderMenu();
  renderCombos();
  updateCartBadge();
  renderQR();
  renderTracking();
}

init();
</script>
</body>
</html>
