<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Nava-Ratna</title>
<style>
body {
    margin: 0;
    font-family: 'Amazon Ember', Arial, sans-serif;
    background-color: #f3f3f3;
}
/* Sticky header */
header {
    background: #131921;
    color: white;
    padding: 10px 20px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    position: sticky;
    top: 0;
    z-index: 1000;
}
.logo { font-size: 1.8rem; font-weight: bold; display:flex; align-items:center; gap:10px; }
.carousel { width: 100%; overflow: hidden; height: 300px; position: relative; }
.carousel img { width: 100%; height: 300px; object-fit: cover; }
.carousel-track { display: flex; width: 500%; animation: slide 15s infinite; }
@keyframes slide { 0% { transform: translateX(0); } 20% { transform: translateX(-20%); } 40% { transform: translateX(-40%); } 60% { transform: translateX(-60%); } 80% { transform: translateX(-80%); } 100% { transform: translateX(0); } }
.products { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; padding: 20px; }
.product-card { background: white; border: 1px solid #ddd; border-radius: 8px; padding: 15px; cursor: pointer; transition: 0.2s; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
.product-card:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.2); }
.product-card img { width: 100%; aspect-ratio: 1 / 1; object-fit: cover; border-radius: 6px; }
.product-card h3 { margin-top: 12px; color: #111; text-align:center; }
#productPage { display: none; padding: 20px; }
#detailImg { width: 100%; aspect-ratio: 1 / 1; object-fit: cover; border-radius: 10px; }
#detailGallery { display: grid; grid-template-columns: 1fr; gap: 10px; margin-top: 20px; max-height: 60vh; overflow-y: auto; }
.orderButton { display: none; position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); background: #ffa41c; padding: 20px 0; width: 90%; max-width: 700px; border-radius: 14px; text-decoration: none; color: black; font-weight: bold; font-size: 1.3rem; text-align: center; z-index: 999; }
footer { background: #131921; color: white; padding: 20px; text-align: center; margin-top: 30px; }
@media(min-width:900px){ .products { grid-template-columns: repeat(4, 1fr); } #detailGallery { max-height: 70vh; } }
</style>
</head>
<body>
<header>
    <div class="logo"><img src="logo.png" alt="logo" style="height:40px">Nava-Ratna</div>
</header>

<div class="carousel">
    <div class="carousel-track">
        <img src="ring1.jpg" alt="slide1" />
        <img src="ring2.jpg" alt="slide2" />
        <img src="ring3.jpg" alt="slide3" />
        <img src="ring4.jpg" alt="slide4" />
        <img src="ring5.jpg" alt="slide5" />
    </div>
</div>

<h2 style="text-align:center; margin-top:30px;">Our Premium Gold Ring Collection</h2>

<!-- Products grid (generated) -->
<div class="products" id="productsContainer"></div>

<!-- Product detail page (opens when clicking a product) -->
<div id="productPage">
    <button id="backBtn" style="padding:10px 15px; margin-bottom:15px;">← Back</button>
    <img id="detailImg" alt="detail" />
    <h2 id="detailTitle" style="margin-top:15px;">Nava-Ratna</h2>
    <div id="detailGallery"></div>
</div>

<a href="tel:+918317359650" class="orderButton" id="orderBtn">Call to Order</a>

<footer>© 2025 Nava-Ratna — Premium Handcrafted Gold Rings</footer>

<script>
// Helper: create product card element
function createProductCard(index){
  const card = document.createElement('div');
  card.className = 'product-card';
  const img = document.createElement('img');
  img.src = `ring${index}.jpg`;
  img.alt = `Nava-Ratna ${index}`;
  const h = document.createElement('h3');
  h.innerText = 'Nava-Ratna';
  card.appendChild(img);
  card.appendChild(h);
  card.addEventListener('click', ()=> openProduct('Nava-Ratna', img.src, index));
  return card;
}

// Generate 100 Nava-Ratna rings
document.addEventListener('DOMContentLoaded', ()=>{
  const container = document.getElementById('productsContainer');
  for(let i=1;i<=100;i++){
    container.appendChild(createProductCard(i));
  }

  // Search functionality: only attach if the search input exists (some versions removed it)
  const searchInput = document.getElementById('searchInput');
  if(searchInput){
    searchInput.addEventListener('input', ()=>{
      const q = searchInput.value.trim().toLowerCase();
      const cards = container.querySelectorAll('.product-card');
      cards.forEach((c)=>{
        const name = c.querySelector('h3').innerText.toLowerCase();
        c.style.display = name.includes(q) ? 'block' : 'none';
      });
    });
  }

  // Back button
  const backBtn = document.getElementById('backBtn');
  if(backBtn) backBtn.addEventListener('click', closeProductPage);
});

// Open product detail page (new page style within site)
function openProduct(name, imgSrc, index){
  // hide homepage
  const carousel = document.querySelector('.carousel');
  const productsGrid = document.querySelector('.products');
  if(carousel) carousel.style.display = 'none';
  if(productsGrid) productsGrid.style.display = 'none';
  // show product page
  document.getElementById('productPage').style.display = 'block';
  // show order button
  document.getElementById('orderBtn').style.display = 'block';

  document.getElementById('detailImg').src = imgSrc;
  document.getElementById('detailTitle').innerText = name;

  // populate gallery below with 10 images (index to index+9, capped at 100)
  const gallery = document.getElementById('detailGallery');
  gallery.innerHTML = '';
  const start = Math.max(1, index - 4);
  const end = Math.min(100, start + 9);
  for(let i = start; i <= end; i++){
    const g = document.createElement('img');
    g.src = `ring${i}.jpg`;
    g.alt = `ring ${i}`;
    g.style.width = '100%';
    g.style.aspectRatio = '1 / 1';
    g.style.objectFit = 'cover';
    g.style.borderRadius = '8px';
    g.style.cursor = 'pointer';
    g.addEventListener('click', ()=>{ document.getElementById('detailImg').src = g.src; });
    gallery.appendChild(g);
  }
}

function closeProductPage(){
  const carousel = document.querySelector('.carousel');
  const productsGrid = document.querySelector('.products');
  if(carousel) carousel.style.display = 'block';
  if(productsGrid) productsGrid.style.display = 'grid';
  document.getElementById('productPage').style.display = 'none';
  document.getElementById('orderBtn').style.display = 'none';
}
</script>
</body>
</html>
