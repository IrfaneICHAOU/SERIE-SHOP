<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>SérieShop – Boutique de séries en ligne</title>
<style>
  :root{
    --bg:#0f1117; --card:#1a1d27; --text:#f2f3f7; --muted:#9aa0b4;
    --accent:#e50914; --accent2:#ff6b6b; --ok:#22c55e; --radius:14px;
  }
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;background:var(--bg);color:var(--text);line-height:1.5}
  a{color:inherit;text-decoration:none}
  button{font:inherit;cursor:pointer;border:0}

  /* ---------- En-tête ---------- */
  header{position:sticky;top:0;z-index:50;background:rgba(15,17,23,.92);backdrop-filter:blur(8px);
    border-bottom:1px solid #262a38;display:flex;align-items:center;gap:16px;padding:14px 5%;flex-wrap:wrap}
  .logo{font-size:1.5rem;font-weight:800;letter-spacing:-.5px}
  .logo span{color:var(--accent)}
  nav{display:flex;gap:18px;margin-left:auto;align-items:center}
  nav a{color:var(--muted);font-weight:500}
  nav a:hover{color:var(--text)}
  .cart-btn{background:var(--accent);color:#fff;padding:9px 16px;border-radius:999px;font-weight:600;display:flex;gap:8px;align-items:center}
  .cart-btn:hover{background:#c40812}
  #cart-count{background:#fff;color:var(--accent);border-radius:999px;min-width:22px;height:22px;display:inline-grid;place-items:center;font-size:.8rem}

  /* ---------- Hero ---------- */
  .hero{padding:90px 5%;text-align:center;
    background:radial-gradient(ellipse at top,#3a0d12 0%,var(--bg) 70%)}
  .hero h1{font-size:clamp(2rem,5vw,3.6rem);line-height:1.1;margin-bottom:14px}
  .hero p{color:var(--muted);max-width:560px;margin:0 auto 26px;font-size:1.1rem}
  .btn{display:inline-block;background:var(--accent);color:#fff;padding:12px 26px;border-radius:999px;font-weight:600}
  .btn:hover{background:#c40812}

  /* ---------- Filtres ---------- */
  .toolbar{padding:30px 5% 10px;display:flex;gap:14px;flex-wrap:wrap;align-items:center}
  .toolbar input,.toolbar select{background:var(--card);color:var(--text);border:1px solid #2c3142;
    padding:11px 14px;border-radius:10px;font:inherit}
  .toolbar input{flex:1;min-width:200px}
  .chips{display:flex;gap:8px;flex-wrap:wrap;width:100%}
  .chip{background:var(--card);color:var(--muted);padding:7px 15px;border-radius:999px;border:1px solid #2c3142}
  .chip.active,.chip:hover{background:var(--accent);color:#fff;border-color:var(--accent)}

  /* ---------- Catalogue ---------- */
  section{padding:20px 5% 50px}
  h2{font-size:1.6rem;margin-bottom:20px}
  .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(230px,1fr));gap:22px}
  .card{background:var(--card);border-radius:var(--radius);overflow:hidden;display:flex;flex-direction:column;
    transition:transform .2s,box-shadow .2s}
  .card:hover{transform:translateY(-6px);box-shadow:0 14px 30px rgba(0,0,0,.45)}
  .poster{height:280px;display:flex;align-items:flex-end;padding:16px;position:relative;cursor:pointer}
  .poster h3{font-size:1.4rem;line-height:1.1;text-shadow:0 2px 8px rgba(0,0,0,.6)}
  .badge{position:absolute;top:12px;left:12px;background:rgba(0,0,0,.55);padding:3px 10px;border-radius:999px;font-size:.75rem}
  .rating{position:absolute;top:12px;right:12px;background:#f5c518;color:#111;padding:3px 9px;border-radius:8px;font-weight:700;font-size:.8rem}
  .info{padding:14px 16px 18px;display:flex;flex-direction:column;gap:8px;flex:1}
  .info small{color:var(--muted)}
  .price-row{display:flex;justify-content:space-between;align-items:center;margin-top:auto}
  .price{font-weight:800;font-size:1.15rem}
  .add{background:var(--accent);color:#fff;padding:8px 14px;border-radius:10px;font-weight:600}
  .add:hover{background:#c40812}
  .empty{color:var(--muted);padding:40px 0;text-align:center}

  /* ---------- Panier (tiroir) ---------- */
  .overlay{position:fixed;inset:0;background:rgba(0,0,0,.6);opacity:0;pointer-events:none;transition:.25s;z-index:90}
  .overlay.open{opacity:1;pointer-events:auto}
  .drawer{position:fixed;top:0;right:0;height:100%;width:min(420px,100%);background:var(--card);z-index:100;
    transform:translateX(100%);transition:transform .3s;display:flex;flex-direction:column}
  .drawer.open{transform:none}
  .drawer-head{display:flex;justify-content:space-between;align-items:center;padding:18px 20px;border-bottom:1px solid #2c3142}
  .close{background:none;color:var(--text);font-size:1.6rem}
  #cart-items{flex:1;overflow:auto;padding:12px 20px}
  .line{display:flex;gap:12px;align-items:center;padding:12px 0;border-bottom:1px solid #2c3142}
  .thumb{width:48px;height:64px;border-radius:8px;flex-shrink:0}
  .line-info{flex:1}
  .qty{display:flex;align-items:center;gap:8px;margin-top:4px}
  .qty button{background:#2c3142;color:#fff;width:26px;height:26px;border-radius:6px}
  .remove{background:none;color:var(--accent2)}
  .drawer-foot{padding:18px 20px;border-top:1px solid #2c3142}
  .total{display:flex;justify-content:space-between;font-size:1.2rem;font-weight:800;margin-bottom:14px}
  .drawer-foot .btn{width:100%;text-align:center}

  /* ---------- Modales ---------- */
  .modal{position:fixed;inset:0;display:none;place-items:center;z-index:120;background:rgba(0,0,0,.7);padding:20px}
  .modal.open{display:grid}
  .modal-box{background:var(--card);border-radius:var(--radius);padding:26px;width:min(480px,100%);max-height:90vh;overflow:auto}
  .modal-box h3{margin-bottom:14px}
  .modal-box label{display:block;margin:12px 0 4px;color:var(--muted);font-size:.9rem}
  .modal-box input,.modal-box select{width:100%;background:var(--bg);color:var(--text);border:1px solid #2c3142;padding:11px;border-radius:10px;font:inherit}
  .modal-box .btn{margin-top:18px;width:100%;text-align:center}
  .success{text-align:center}
  .success .check{font-size:3rem;color:var(--ok)}

  /* ---------- Toast ---------- */
  #toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%) translateY(80px);background:var(--ok);color:#fff;
    padding:12px 22px;border-radius:999px;font-weight:600;transition:.3s;z-index:200}
  #toast.show{transform:translateX(-50%) translateY(0)}

  footer{border-top:1px solid #262a38;padding:30px 5%;text-align:center;color:var(--muted);font-size:.9rem}
</style>
</head>
<body>

<!-- ======================= EN-TÊTE ======================= -->
<header>
  <a href="#" class="logo">Série<span>Shop</span></a>
  <nav>
    <a href="#catalogue">Catalogue</a>
    <a href="#apropos">À propos</a>
    <button class="cart-btn" id="open-cart" aria-label="Ouvrir le panier">
      🛒 Panier <span id="cart-count">0</span>
    </button>
  </nav>
</header>

<!-- ======================= HERO ======================= -->
<div class="hero">
  <h1>Vos séries préférées,<br>en coffrets &amp; en streaming</h1>
  <p>Achetez des saisons complètes en version numérique ou DVD. Livraison rapide, paiement sécurisé.</p>
  <a href="#catalogue" class="btn">Découvrir le catalogue</a>
</div>

<!-- ======================= CATALOGUE ======================= -->
<section id="catalogue">
  <h2>Notre catalogue</h2>
  <div class="toolbar">
    <input type="search" id="search" placeholder="🔍 Rechercher une série…">
    <select id="sort">
      <option value="default">Trier par…</option>
      <option value="price-asc">Prix croissant</option>
      <option value="price-desc">Prix décroissant</option>
      <option value="rating">Mieux notées</option>
    </select>
    <div class="chips" id="chips"></div>
  </div>
  <div class="grid" id="grid"></div>
</section>

<!-- ======================= À PROPOS ======================= -->
<section id="apropos">
  <h2>À propos</h2>
  <p style="color:var(--muted);max-width:700px">
    SérieShop est une boutique en ligne dédiée aux passionnés de séries. Les titres présentés ici sont fictifs
    et servent d'exemple : remplacez-les par votre propre catalogue.
  </p>
</section>

<footer>© 2026 SérieShop – Tous droits réservés.</footer>

<!-- ======================= PANIER ======================= -->
<div class="overlay" id="overlay"></div>
<aside class="drawer" id="drawer" aria-label="Panier">
  <div class="drawer-head">
    <h3>Mon panier</h3>
    <button class="close" id="close-cart" aria-label="Fermer">×</button>
  </div>
  <div id="cart-items"></div>
  <div class="drawer-foot">
    <div class="total"><span>Total</span><span id="cart-total">0 FCFA</span></div>
    <button class="btn" id="checkout-btn">Passer la commande</button>
  </div>
</aside>

<!-- ======================= MODALE DÉTAILS ======================= -->
<div class="modal" id="detail-modal">
  <div class="modal-box" id="detail-box"></div>
</div>

<!-- ======================= MODALE COMMANDE ======================= -->
<div class="modal" id="checkout-modal">
  <div class="modal-box">
    <div id="checkout-form-wrap">
      <h3>Finaliser la commande</h3>
      <form id="checkout-form">
        <label for="name">Nom complet</label>
        <input id="name" required>
        <label for="email">E-mail</label>
        <input id="email" type="email" required>
        <label for="phone">Téléphone</label>
        <input id="phone" type="tel" required>
        <label for="address">Adresse de livraison</label>
        <input id="address" required>
        <label for="pay">Mode de paiement</label>
        <select id="pay">
          <option>Mobile Money (MTN / Moov)</option>
          <option>Carte bancaire</option>
          <option>Paiement à la livraison</option>
        </select>
        <button class="btn" type="submit">Confirmer sur WhatsApp – <span id="checkout-total"></span></button>
      </form>
    </div>
    <div class="success" id="success" hidden>
      <div class="check">✔</div>
      <h3>Merci ! WhatsApp s'est ouvert pour discuter du prix.</h3>
      <p style="color:var(--muted)">Numéro de commande : <strong id="order-id"></strong></p>
      <button class="btn" id="close-success">Retour à la boutique</button>
    </div>
  </div>
</div>

<div id="toast"></div>

<!-- ======================= SCRIPT ======================= -->
<script>
/* ---------- Données (séries fictives) ---------- */
const SERIES = [
  {id:1, title:"Ombres de Cotonou", genre:"Thriller",  year:2024, seasons:3, rating:8.7, price:15000, c1:"#7f1d1d", c2:"#1e1b4b", desc:"Une enquêtrice traque un réseau criminel dans les ruelles d'une ville côtière."},
  {id:2, title:"Le Dernier Royaume", genre:"Fantastique", year:2023, seasons:4, rating:9.1, price:20000, c1:"#065f46", c2:"#0f172a", desc:"Quatre clans s'affrontent pour le trône d'un royaume oublié."},
  {id:3, title:"Code Orion",         genre:"Science-fiction", year:2025, seasons:2, rating:8.3, price:12000, c1:"#1d4ed8", c2:"#020617", desc:"Une équipe d'ingénieurs découvre un signal venu d'au-delà du système solaire."},
  {id:4, title:"Café Soleil",        genre:"Comédie", year:2022, seasons:5, rating:7.9, price:10000, c1:"#d97706", c2:"#7c2d12", desc:"Les mésaventures hilarantes des employés d'un petit café de quartier."},
  {id:5, title:"Rivales",            genre:"Drame",   year:2024, seasons:2, rating:8.5, price:13500, c1:"#9d174d", c2:"#1f2937", desc:"Deux sœurs que tout oppose héritent de la même entreprise familiale."},
  {id:6, title:"Zone Rouge",         genre:"Action",  year:2025, seasons:1, rating:8.0, price:8000,  c1:"#b91c1c", c2:"#111827", desc:"Une unité d'élite doit déjouer une série d'attaques en 48 heures."},
  {id:7, title:"Mémoires Vives",     genre:"Drame",   year:2021, seasons:3, rating:8.8, price:14000, c1:"#4338ca", c2:"#0b1020", desc:"Un neurologue découvre qu'il peut revivre les souvenirs de ses patients."},
  {id:8, title:"Les Chroniques d'Abomey", genre:"Fantastique", year:2023, seasons:2, rating:8.6, price:16000, c1:"#a16207", c2:"#14532d", desc:"Légendes et magie se mêlent dans l'épopée d'un jeune guerrier."}
];

/* ---------- Numéro WhatsApp (format international, sans + ni espaces) ---------- */
const WHATSAPP_NUMBER = "2290154262570";

/* ---------- Utilitaires ---------- */
const $  = s => document.querySelector(s);
const fmt = n => n.toLocaleString("fr-FR") + " FCFA";
const poster = s => `linear-gradient(160deg, ${s.c1}, ${s.c2})`;

/* ---------- État ---------- */
let cart = [];
try { cart = JSON.parse(localStorage.getItem("serieshop-cart")) || []; } catch(e){}
let filter = { genre:"Tous", q:"", sort:"default" };

const saveCart = () => { try { localStorage.setItem("serieshop-cart", JSON.stringify(cart)); } catch(e){} };

/* ---------- Catalogue ---------- */
function buildChips(){
  const genres = ["Tous", ...new Set(SERIES.map(s => s.genre))];
  $("#chips").innerHTML = genres.map(g =>
    `<button class="chip ${g===filter.genre?"active":""}" data-genre="${g}">${g}</button>`).join("");
}

function renderGrid(){
  let list = SERIES.filter(s =>
    (filter.genre==="Tous" || s.genre===filter.genre) &&
    s.title.toLowerCase().includes(filter.q.toLowerCase()));

  if(filter.sort==="price-asc")  list.sort((a,b)=>a.price-b.price);
  if(filter.sort==="price-desc") list.sort((a,b)=>b.price-a.price);
  if(filter.sort==="rating")     list.sort((a,b)=>b.rating-a.rating);

  $("#grid").innerHTML = list.length ? list.map(s => `
    <article class="card">
      <div class="poster" style="background:${poster(s)}" data-detail="${s.id}">
        <span class="badge">${s.genre}</span>
        <span class="rating">★ ${s.rating}</span>
        <h3>${s.title}</h3>
      </div>
      <div class="info">
        <small>${s.year} • ${s.seasons} saison${s.seasons>1?"s":""}</small>
        <div class="price-row">
          <span class="price">${fmt(s.price)}</span>
          <button class="add" data-add="${s.id}">Ajouter</button>
        </div>
      </div>
    </article>`).join("") : `<p class="empty">Aucune série trouvée.</p>`;
}

/* ---------- Panier ---------- */
function addToCart(id){
  const item = cart.find(i => i.id===id);
  item ? item.qty++ : cart.push({id, qty:1});
  saveCart(); renderCart();
  toast("Ajouté au panier ✔");
}
function changeQty(id, delta){
  const item = cart.find(i => i.id===id);
  if(!item) return;
  item.qty += delta;
  if(item.qty<=0) cart = cart.filter(i => i.id!==id);
  saveCart(); renderCart();
}
function removeItem(id){ cart = cart.filter(i => i.id!==id); saveCart(); renderCart(); }

const cartTotal = () => cart.reduce((t,i) => t + SERIES.find(s=>s.id===i.id).price * i.qty, 0);

function renderCart(){
  $("#cart-count").textContent = cart.reduce((t,i)=>t+i.qty,0);
  $("#cart-total").textContent = fmt(cartTotal());
  $("#cart-items").innerHTML = cart.length ? cart.map(i => {
    const s = SERIES.find(x => x.id===i.id);
    return `
    <div class="line">
      <div class="thumb" style="background:${poster(s)}"></div>
      <div class="line-info">
        <strong>${s.title}</strong><br><small>${fmt(s.price)}</small>
        <div class="qty">
          <button data-dec="${s.id}">−</button><span>${i.qty}</span><button data-inc="${s.id}">+</button>
        </div>
      </div>
      <button class="remove" data-del="${s.id}" aria-label="Supprimer">🗑</button>
    </div>`;
  }).join("") : `<p class="empty">Votre panier est vide.</p>`;
}

const openCart  = () => { $("#drawer").classList.add("open"); $("#overlay").classList.add("open"); };
const closeCart = () => { $("#drawer").classList.remove("open"); $("#overlay").classList.remove("open"); };

/* ---------- Détails ---------- */
function showDetail(id){
  const s = SERIES.find(x => x.id===id);
  $("#detail-box").innerHTML = `
    <div style="height:180px;border-radius:12px;margin-bottom:14px;background:${poster(s)};display:flex;align-items:flex-end;padding:14px">
      <h3 style="font-size:1.6rem">${s.title}</h3>
    </div>
    <p><span class="badge" style="position:static;background:#2c3142">${s.genre}</span>
       &nbsp;★ ${s.rating} • ${s.year} • ${s.seasons} saison${s.seasons>1?"s":""}</p>
    <p style="margin:14px 0;color:var(--muted)">${s.desc}</p>
    <p class="price" style="margin-bottom:6px">${fmt(s.price)}</p>
    <button class="btn" data-add="${s.id}" data-closemodal="1">Ajouter au panier</button>`;
  $("#detail-modal").classList.add("open");
}

/* ---------- Toast ---------- */
let toastTimer;
function toast(msg){
  const t = $("#toast"); t.textContent = msg; t.classList.add("show");
  clearTimeout(toastTimer); toastTimer = setTimeout(()=>t.classList.remove("show"), 1800);
}

/* ---------- Événements ---------- */
document.addEventListener("click", e => {
  const t = e.target;
  if(t.dataset.add)    { addToCart(+t.dataset.add); if(t.dataset.closemodal) $("#detail-modal").classList.remove("open"); }
  if(t.dataset.inc)    changeQty(+t.dataset.inc, 1);
  if(t.dataset.dec)    changeQty(+t.dataset.dec, -1);
  if(t.dataset.del)    removeItem(+t.dataset.del);
  if(t.closest("[data-detail]")) showDetail(+t.closest("[data-detail]").dataset.detail);
  if(t.dataset.genre)  { filter.genre = t.dataset.genre; buildChips(); renderGrid(); }
  if(t === $("#detail-modal")) $("#detail-modal").classList.remove("open");
});

$("#open-cart").onclick  = openCart;
$("#close-cart").onclick = closeCart;
$("#overlay").onclick    = closeCart;
$("#search").oninput = e => { filter.q = e.target.value; renderGrid(); };
$("#sort").onchange  = e => { filter.sort = e.target.value; renderGrid(); };

$("#checkout-btn").onclick = () => {
  if(!cart.length) return toast("Votre panier est vide");
  closeCart();
  $("#checkout-total").textContent = fmt(cartTotal());
  $("#checkout-form-wrap").hidden = false; $("#success").hidden = true;
  $("#checkout-modal").classList.add("open");
};

$("#checkout-form").onsubmit = e => {
  e.preventDefault();

  const orderId = "CMD-" + Date.now().toString().slice(-7);

  /* Construction du message WhatsApp */
  const lignes = cart.map(i => {
    const s = SERIES.find(x => x.id === i.id);
    return `• ${s.title} x${i.qty} : ${fmt(s.price * i.qty)}`;
  }).join("\n");

  const message =
`Bonjour, je souhaite passer commande sur SérieShop.

*Commande ${orderId}*
${lignes}

*Total : ${fmt(cartTotal())}*

*Mes informations :*
Nom : ${$("#name").value}
E-mail : ${$("#email").value}
Téléphone : ${$("#phone").value}
Adresse : ${$("#address").value}
Paiement souhaité : ${$("#pay").value}

Pouvons-nous discuter du prix ?`;

  const lien = `https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`;

  $("#order-id").textContent = orderId;
  $("#checkout-form-wrap").hidden = true; $("#success").hidden = false;
  cart = []; saveCart(); renderCart(); e.target.reset();

  /* Ouvre WhatsApp (nouvel onglet, sinon dans la même page) */
  const w = window.open(lien, "_blank");
  if(!w) window.location.href = lien;
};
$("#close-success").onclick = () => $("#checkout-modal").classList.remove("open");

/* ---------- Initialisation ---------- */
buildChips(); renderGrid(); renderCart();
</script>
</body>
</html>
