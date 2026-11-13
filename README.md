<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PriceOja - Know Price Before You Go Market</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif}
body{background:#f4f6f4}
header{background:#0A7A42;color:#fff;padding:18px 14px;text-align:center;position:sticky;top:0;z-index:10}
header h1{font-size:28px} header h1 span{color:#FF8A00}
header p{font-size:13px;opacity:.9;margin-top:4px}
.search{padding:12px;background:#fff;position:sticky;top:88px;z-index:9}
.search input{width:100%;padding:13px 15px;border:2px solid #0A7A42;border-radius:25px;font-size:15px;outline:none}
.cats{display:flex;gap:8px;overflow-x:auto;padding:12px;background:#fff;white-space:nowrap;scrollbar-width:none}
.cats button{border:1px solid #0A7A42;background:#fff;color:#0A7A42;padding:9px 16px;border-radius:20px;font-size:14px;cursor:pointer;flex-shrink:0}
.cats button.active{background:#0A7A42;color:#fff}
.grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;padding:12px;max-width:1200px;margin:0 auto}
@media(min-width:700px){.grid{grid-template-columns:repeat(3,1fr);gap:16px;padding:16px}}
@media(min-width:1000px){.grid{grid-template-columns:repeat(4,1fr)}}
.card{background:#fff;border-radius:14px;overflow:hidden;box-shadow:0 2px 8px rgba(0,0,0,.08)}
.card .img{height:90px;display:flex;align-items:center;justify-content:center;font-size:42px;background:#e8f5ee}
.card .body{padding:12px}
.card h3{font-size:15px;margin-bottom:4px}
.card .cat{font-size:11px;background:#FFF0D9;color:#B25A00;padding:3px 8px;border-radius:10px;display:inline-block;margin-bottom:6px}
.card .price{font-size:18px;font-weight:900;color:#0A7A42}
.card .market{font-size:12px;color:#666;margin:4px 0}
.card .trend{font-size:12px;font-weight:700}
.down{color:#0A7A42}.up{color:#E53935}
.card button{width:100%;margin-top:10px;background:#FF8A00;border:none;color:#fff;padding:10px;border-radius:10px;font-weight:700;cursor:pointer}
footer{text-align:center;padding:20px;color:#888;font-size:12px}
.hide{display:none}
</style>
</head>
<body>
<header><h1>Price<span>Oja</span> 🛒</h1><p>Know Price Before You Go Market • Lagos</p></header>
<div class="search"><input id="search" placeholder="Search Rice, Sneakers, Fan, Handbag..." oninput="filter()"></div>
<div class="cats" id="cats">
<button class="active" onclick="setCat('All',this)">All</button>
<button onclick="setCat('Food',this)">🍚 Food</button>
<button onclick="setCat('Clothes',this)">👕 Clothes</button>
<button onclick="setCat('Bags',this)">👜 Bags</button>
<button onclick="setCat('Shoes',this)">👟 Shoes</button>
<button onclick="setCat('Home Appliance',this)">🏠 Home Appliance</button>
</div>
<div class="grid" id="grid"></div>
<footer>PriceOja • Prices na sample, update daily • Made in Lagos 🇳🇬</footer>
<script>
let cur='All';
const items=[
 // FOOD
 {n:"Rice (50kg)",c:"Food",e:"🍚",p:"₦85,000",m:"Mile 12 • Cheapest",t:"↓ ₦2,000",cl:"down"},
 {n:"Beans (50kg)",c:"Food",e:"🫘",p:"₦7,500",m:"Oyingbo • Cheapest",t:"↓ ₦500",cl:"down"},
 {n:"Garri (50kg)",c:"Food",e:"🥣",p:"₦48,000",m:"Mile 12 • Cheapest",t:"— Same",cl:"down"},
 {n:"Tomatoes (Basket)",c:"Food",e:"🍅",p:"₦35,000",m:"Mile 12 • Cheapest",t:"↑ ₦2,000",cl:"up"},
 {n:"Yam (10 Tubers)",c:"Food",e:"🍠",p:"₦22,000",m:"Ile Epo • Cheapest",t:"↓ ₦1,000",cl:"down"},
 {n:"Palm Oil (25L)",c:"Food",e:"🫙",p:"₦52,000",m:"Mushin • Cheapest",t:"↑ ₦1,500",cl:"up"},
 // CLOTHES
 {n:"Ankara (6 Yards)",c:"Clothes",e:"👗",p:"₦18,000",m:"Balogun • Cheapest",t:"— Same",cl:"down"},
 {n:"T-Shirt (Quality)",c:"Clothes",e:"👕",p:"₦8,500",m:"Yaba • Cheapest",t:"↓ ₦500",cl:"down"},
 {n:"Jeans Trouser",c:"Clothes",e:"👖",p:"₦15,000",m:"Balogun • Cheapest",t:"↑ ₦1,000",cl:"up"},
 {n:"Gown (Ladies)",c:"Clothes",e:"👗",p:"₦22,000",m:"Yaba • Cheapest",t:"↓ ₦2,000",cl:"down"},
 {n:"Native Wear (Senator)",c:"Clothes",e:"🧥",p:"₦28,000",m:"Balogun • Cheapest",t:"— Same",cl:"down"},
 {n:"Hoodie",c:"Clothes",e:"🧶",p:"₦16,500",m:"Yaba • Cheapest",t:"↓ ₦1,000",cl:"down"},
 // BAGS
 {n:"Handbag (Ladies)",c:"Bags",e:"👜",p:"₦15,000",m:"Yaba • Cheapest",t:"↓ ₦1,500",cl:"down"},
 {n:"School Bag",c:"Bags",e:"🎒",p:"₦12,000",m:"Balogun • Cheapest",t:"— Same",cl:"down"},
 {n:"Traveling Bag (Big)",c:"Bags",e:"🧳",p:"₦32,000",m:"Balogun • Cheapest",t:"↑ ₦2,000",cl:"up"},
 {n:"Backpack (Laptop)",c:"Bags",e:"🎒",p:"₦18,500",m:"Yaba • Cheapest",t:"↓ ₦1,000",cl:"down"},
 // SHOES
 {n:"Sneakers (Unisex)",c:"Shoes",e:"👟",p:"₦25,000",m:"Balogun • Cheapest",t:"↓ ₦2,000",cl:"down"},
 {n:"Slippers (Ladies)",c:"Shoes",e:"🩴",p:"₦7,000",m:"Yaba • Cheapest",t:"— Same",cl:"down"},
 {n:"Heels (Ladies)",c:"Shoes",e:"👠",p:"₦18,000",m:"Balogun • Cheapest",t:"↑ ₦1,000",cl:"up"},
 {n:"Sandals (Men)",c:"Shoes",e:"👞",p:"₦14,000",m:"Yaba • Cheapest",t:"↓ ₦500",cl:"down"},
 {n:"Crocs",c:"Shoes",e:"🩴",p:"₦11,500",m:"Balogun • Cheapest",t:"↓ ₦1,000",cl:"down"},
 // HOME APPLIANCE
 {n:"Standing Fan",c:"Home Appliance",e:"🌀",p:"₦45,000",m:"Alaba • Cheapest",t:"↓ ₦3,000",cl:"down"},
 {n:"Blender",c:"Home Appliance",e:"🥤",p:"₦28,000",m:"Alaba • Cheapest",t:"— Same",cl:"down"},
 {n:"Rice Cooker",c:"Home Appliance",e:"🍚",p:"₦35,000",m:"Alaba • Cheapest",t:"↑ ₦2,000",cl:"up"},
 {n:"Electric Kettle",c:"Home Appliance",e:"🍵",p:"₦18,000",m:"Alaba • Cheapest",t:"↓ ₦1,000",cl:"down"},
 {n:"32\" TV",c:"Home Appliance",e:"📺",p:"₦125,000",m:"Alaba • Cheapest",t:"↓ ₦5,000",cl:"down"},
 {n:"Gas Cooker (2 Burner)",c:"Home Appliance",e:"🔥",p:"₦68,000",m:"Alaba • Cheapest",t:"— Same",cl:"down"},
];
function render(){
 let q=document.getElementById('search').value.toLowerCase();
 let html='';
 items.forEach(it=>{
  if(cur!=='All' && it.c!==cur) return;
  if(q && !it.n.toLowerCase().includes(q)) return;
  html+=`<div class="card"><div class="img">${it.e}</div><div class="body"><span class="cat">${it.c}</span><h3>${it.n}</h3><div class="price">${it.p}</div><div class="market">📍 ${it.m}</div><div class="trend ${it.cl}">${it.t}</div><button onclick="alert('Cheapest: ${it.m} - ${it.p}')">Where Cheapest?</button></div></div>`;
 });
 document.getElementById('grid').innerHTML=html||'<p style="padding:20px">No item found</p>';
}
function setCat(c,el){cur=c;document.querySelectorAll('.cats button').forEach(b=>b.classList.remove('active'));el.classList.add('active');render();}
function filter(){render();}
render();
</script>
</body>
</html>
