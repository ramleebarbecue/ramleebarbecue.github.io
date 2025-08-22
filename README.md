<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ramlee Barbecue</title>
<style>
body { font-family: Arial, sans-serif; margin: 0; background: #fafafa; color: #333; }
header { background: #111; color: #fff; padding: 1rem; display: flex; justify-content: space-between; align-items: center; }
header h1 { margin: 0; }
nav a { color: #fff; margin-left: 1rem; text-decoration: none; }
.container { width: 90%; max-width: 900px; margin: 2rem auto; }
section { margin-bottom: 2rem; }
.menu { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px,1fr)); gap: 1rem; }
.menu-item { background: #fff; padding: 1rem; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); text-align: center; }
.menu-item img { width: 100%; height: 150px; object-fit: cover; border-radius: 6px; }
button { padding: 0.5rem 1rem; background: #ff6600; color: #fff; border: none; border-radius: 6px; cursor: pointer; margin-top: 0.5rem; }
button:hover { background: #e65c00; }
.cart { background: #fff; padding: 1rem; border-radius: 8px; }
.cart-items { margin-bottom: 1rem; }
input { width: 100%; margin-bottom: 10px; padding: 8px; border-radius: 6px; border: 1px solid #ccc; }
.button { display: inline-block; background: #28a745; color: #fff; padding: 10px 20px; border-radius: 6px; text-decoration: none; margin-top: 10px; }
.button:hover { background: #218838; }
</style>
</head>
<body>

<header>
  <h1>Ramlee Barbecue</h1>
  <nav>
    <a href="#home">Home</a>
    <a href="#menu">Menu</a>
    <a href="#cart">Cart</a>
  </nav>
</header>

<main class="container">

  <!-- Home Section -->
  <section id="home">
    <h2>Welcome to Ramlee Barbecue!</h2>
    <p>Delicious BBQ meals ready for pickup.</p>
    <a class="button" href="#menu">Order Now</a>
  </section>

  <!-- Menu Section -->
  <section id="menu">
    <h2>Our Menu</h2>
    <div class="menu" id="menuList">
      <!-- Menu items inserted via JS -->
    </div>
  </section>

  <!-- Cart Section -->
  <section id="cart" class="cart">
    <h2>Your Cart</h2>
    <div id="cartItems"></div>
    <h3>Customer Info</h3>
    <input type="text" id="custName" placeholder="Your Name" required>
    <input type="text" id="custPhone" placeholder="Your Phone" required>
    <input type="time" id="pickupTime" required>
    <button onclick="checkout()">Checkout via WhatsApp</button>
  </section>

</main>

<script>
const WHATSAPP_NUMBER = "6738121098"; // Replace with your number

const menuItems = [
  { id: 1, name: "Chicken BBQ", price: 5, img: "https://via.placeholder.com/200x150?text=Chicken+BBQ" },
  { id: 2, name: "Beef BBQ", price: 7, img: "https://via.placeholder.com/200x150?text=Beef+BBQ" },
  { id: 3, name: "Lamb BBQ", price: 8, img: "https://via.placeholder.com/200x150?text=Lamb+BBQ" },
  { id: 4, name: "Drinks", price: 2, img: "https://via.placeholder.com/200x150?text=Drinks" }
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
      <button onclick="addToCart(${item.id})">Add to Cart</button>
    `;
    menuList.appendChild(div);
  });
}

function addToCart(id){
  const item = menuItems.find(i=>i.id===id);
  const existing = cart.find(i=>i.id===id);
  if(existing) existing.qty++;
  else cart.push({...item, qty:1});
  renderCart();
}

function renderCart(){
  const cartDiv = document.getElementById("cartItems");
  cartDiv.innerHTML="";
  let total=0;
  cart.forEach((item,index)=>{
    total += item.price*item.qty;
    cartDiv.innerHTML += `<div>${item.name} x ${item.qty} = BND ${(item.price*item.qty).toFixed(2)} <button onclick="removeItem(${index})">-</button></div>`;
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
    orderText += `${item.name} x ${item.qty} = BND ${(item.price*item.qty).toFixed(2)}%0A`;
    total += item.price*item.qty;
  });
  orderText += `%0ATotal: BND ${total.toFixed(2)}`;

  window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${orderText}`,"_blank");
}

document.addEventListener("DOMContentLoaded", renderMenu);
</script>

</body>
</html>
