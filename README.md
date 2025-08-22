<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ramlee Barbecue</title>
<style>
/* Reset */
* { margin:0; padding:0; box-sizing:border-box; }
body { font-family: 'Segoe UI', Arial, sans-serif; background:#fff; color:#111; line-height:1.6; }

/* Header */
header { background:#111; color:#fff; padding:1rem 2rem; display:flex; justify-content:space-between; align-items:center; }
header h1 { color:orange; font-family:'Arial Black', sans-serif; font-size:1.8rem; }
nav a { margin-left:1rem; color:#fff; font-weight:bold; transition:.3s; }
nav a:hover { color:orange; }

/* Container */
.container { width:90%; max-width:1000px; margin:2rem auto; }

/* Home */
#home { text-align:center; padding:2rem 0; }
#home h2 { font-size:2rem; color:#111; margin-bottom:1rem; }
#home p { font-size:1.1rem; margin-bottom:1.5rem; }
.button { display:inline-block; background:orange; color:#fff; padding:0.8rem 1.5rem; border-radius:6px; font-weight:bold; transition:0.3s; }
.button:hover { background:#e65c00; }

/* Menu */
#menu h2 { text-align:center; margin-bottom:1rem; color:#111; }
.menu { display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:1rem; }
.menu-item { background:#fff; padding:1rem; border-radius:10px; box-shadow:0 3px 10px rgba(0,0,0,0.1); text-align:center; position:relative; }
.menu-item img { width:100%; height:150px; object-fit:cover; border-radius:8px; margin-bottom:0.5rem; }
.menu-item h3 { margin:0.5rem 0; font-size:1.1rem; }
.menu-item p { margin-bottom:0.5rem; font-weight:bold; color:#111; }
.quantity { display:flex; justify-content:center; align-items:center; margin-bottom:0.5rem; }
.quantity button { width:30px; height:30px; border:none; border-radius:4px; background:orange; color:#fff; font-weight:bold; cursor:pointer; }
.quantity span { margin:0 10px; min-width:20px; text-align:center; }
.remark { width:90%; margin:0.5rem auto; padding:5px; border-radius:6px; border:1px solid #ccc; }

/* Cart */
.cart { background:#fff; padding:1rem; border-radius:10px; box-shadow:0 3px 10px rgba(0,0,0,0.05); margin-top:2rem; }
.cart-items div { margin-bottom:0.5rem; }
.cart-items strong { display:block; margin-top:0.5rem; }

/* Location */
#location { text-align:center; padding:2rem 0; }
#location a { display:inline-block; background:black; color:orange; padding:0.8rem 1.5rem; border-radius:6px; font-weight:bold; margin-top:1rem; transition:0.3s; }
#location a:hover { background:orange; color:#fff; }

/* Email subscription */
#subscribe { text-align:center; padding:2rem 0; background:#f5f5f5; border-radius:10px; margin-top:2rem; }
#subscribe input[type="email"] { padding:0.8rem; width:250px; border-radius:6px; border:1px solid #ccc; margin-right:5px; }
#subscribe button { padding:0.8rem 1.2rem; background:orange; color:#fff; border:none; border-radius:6px; cursor:pointer; transition:0.3s; }
#subscribe button:hover { background:#e65c00; }

/* Responsive */
@media(max-width:600px){ .quantity { flex-direction:row; } }
</style>
</head>
<body>

<header>
  <h1>Ramlee Barbecue</h1>
  <nav>
    <a href="#home">Home</a>
    <a href="#menu">Menu</a>
    <a href="#location">Location</a>
    <a href="#subscribe">Subscribe</a>
  </nav>
</header>

<main class="container">

<!-- Home -->
<section id="home">
  <h2>Welcome to Ramlee Barbecue!</h2>
  <p>Delicious BBQ meals ready for pickup. Taste the flavor, feel the tradition!</p>
  <a class="button" href="#menu">Order Now</a>
</section>

<!-- Menu -->
<section id="menu">
  <h2>Our Menu</h2>
  <div class="menu" id="menuList"></div>

  <div class="cart" id="cart">
    <h2>Your Cart</h2>
    <div class="cart-items" id="cartItems"></div>
    <h3>Customer Info</h3>
    <input type="text" id="custName" placeholder="Your Name" required>
    <input type="text" id="custPhone" placeholder="Your Phone" required>
    <input type="time" id="pickupTime" required>
    <button onclick="checkout()">Checkout via WhatsApp</button>
  </div>
</section>

<!-- Location -->
<section id="location">
  <h2>Find Us</h2>
  <p>Click below to get directions to our shop.</p>
  <a href="https://www.google.com/maps/dir/?api=1&destination=Brunei+Ramlee+Barbecue" target="_blank">Get Directions</a>
</section>

<!-- Email Subscription -->
<section id="subscribe">
  <h2>Subscribe to Our Newsletter</h2>
  <p>Get latest updates and promotions</p>
  <input type="email" id="email" placeholder="Enter your email">
  <button onclick="subscribe()">Subscribe</button>
  <p id="subscribeMsg"></p>
</section>

</main>

<script>
const WHATSAPP_NUMBER = "6738121098"; // replace with your number

// Menu items
const menuItems = [
  { id:1, name:"Chicken BBQ", price:5, img:"images/chicken_bbq.png" },
  { id:2, name:"Beef BBQ", price:7, img:"images/beef_bbq.png" },
  { id:3, name:"Lamb BBQ", price:8, img:"images/lamb_bbq.png" },
  { id:4, name:"Drinks", price:2, img:"images/drinks.png" }
];

let cart = [];

function renderMenu(){
  const menuList = document.getElementById("menuList");
  menuList.innerHTML = "";
  menuItems.forEach(item=>{
    const div = document.createElement("div");
    div.classList.add("menu-item");
    div.innerHTML = `
      <img src="${item.img}" alt="${item.name}">
      <h3>${item.name}</h3>
      <p>BND ${item.price}</p>
      <div class="quantity">
        <button onclick="changeQty(${item.id}, -1)">-</button>
        <span id="qty-${item.id}">0</span>
        <button onclick="changeQty(${item.id}, 1)">+</button>
      </div>
      <input class="remark" id="remark-${item.id}" placeholder="Add remark">
      <button onclick="addToCartWithQty(${item.id})">Add to Cart</button>
    `;
    menuList.appendChild(div);
  });
}

function changeQty(id, delta){
  const qtySpan = document.getElementById(`qty-${id}`);
  let qty = parseInt(qtySpan.innerText);
  qty += delta;
  if(qty<0) qty=0;
  qtySpan.innerText = qty;
}

function addToCartWithQty(id){
  const qty = parseInt(document.getElementById(`qty-${id}`).innerText);
  if(qty<=0){ alert("Quantity must be at least 1"); return; }
  const remark = document.getElementById(`remark-${id}`).value;
  const item = menuItems.find(i=>i.id===id);
  const cartItem = {...item, qty:qty, remark: remark};
  const existing = cart.find(i=>i.id===id && i.remark===remark);
  if(existing) existing.qty += qty;
  else cart.push(cartItem);
  renderCart();
  document.getElementById(`qty-${id}`).innerText = 0;
  document.getElementById(`remark-${id}`).value = "";
}

function renderCart(){
  const cartDiv = document.getElementById("cartItems");
  cartDiv.innerHTML="";
  let total=0;
  cart.forEach((item,index)=>{
    total += item.price*item.qty;
    cartDiv.innerHTML += `<div>${item.name} x ${item.qty} ${item.remark ? "- "+item.remark : ""} = BND ${(item.price*item.qty).toFixed(2)} <button onclick="removeItem(${index})">-</button></div>`;
  });
  if(cart.length) cartDiv.innerHTML += `<hr><strong>Total: BND ${total.toFixed(2)}</strong>`;
  else cartDiv.innerHTML = "<em>Cart is empty</em>";
}

function removeItem(index){
  if(cart[index].qty>1) cart[index].qty--;
  else cart.splice(index,1);
  renderCart();
}

function checkout(){
  if(cart.length===0){ alert("Cart is empty!"); return; }
  const name = document.getElementById("custName").value;
  const phone = document.getElementById("custPhone").value;
  const time = document.getElementById("pickupTime").value;
  if(!name || !phone || !time){ alert("Fill all details!"); return; }

  let orderText = `New Order from ${name}%0APhone: ${phone}%0APickup: ${time}%0A%0AOrder:%0A`;
  let total=0;
  cart.forEach(item=>{
    orderText += `${item.name} x ${item.qty} ${item.remark ? "- "+item.remark : ""} = BND ${(item.price*item.qty).toFixed(2)}%0A`;
    total += item.price*item.qty;
  });
  orderText += `%0ATotal: BND ${total.toFixed(2)}`;
  window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${orderText}`,"_blank");
}

// Simple email subscription
function subscribe(){
  const email = document.getElementById("email").value;
  if(!email){ alert("Enter a valid email"); return; }
  document.getElementById("subscribeMsg").innerText = `Thank you! ${email} subscribed.`;
  document.getElementById("email").value = "";
}

document.addEventListener("DOMContentLoaded", renderMenu);
</script>

</body>
</html>
