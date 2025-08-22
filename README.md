<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ramlee Barbecue Menu</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; background: #fafafa; }
    h1, h2 { text-align: center; }
    .menu, .cart { max-width: 600px; margin: 20px auto; }
    .item { display: flex; justify-content: space-between; padding: 10px; background: #fff; margin-bottom: 10px; border-radius: 8px; box-shadow: 0 1px 4px rgba(0,0,0,0.1); }
    button { background: #ff6600; color: #fff; border: none; padding: 6px 12px; border-radius: 6px; cursor: pointer; }
    button:hover { background: #e65c00; }
    .cart-items { background: #fff; padding: 10px; border-radius: 8px; margin-top: 20px; }
    .checkout { margin-top: 20px; text-align: center; }
    input, textarea { width: 100%; margin-bottom: 10px; padding: 8px; border-radius: 6px; border: 1px solid #ccc; }
  </style>
</head>
<body>
  <h1>Ramlee Barbecue</h1>
  <h2>Menu</h2>
  <div class="menu">
    <div class="item">
      <span>Chicken BBQ - $5</span>
      <button onclick="addToCart('Chicken BBQ', 5)">Add</button>
    </div>
    <div class="item">
      <span>Lamb BBQ - $8</span>
      <button onclick="addToCart('Lamb BBQ', 8)">Add</button>
    </div>
    <div class="item">
      <span>Beef BBQ - $7</span>
      <button onclick="addToCart('Beef BBQ', 7)">Add</button>
    </div>
    <div class="item">
      <span>Drinks - $2</span>
      <button onclick="addToCart('Drinks', 2)">Add</button>
    </div>
  </div>

  <h2>Your Cart</h2>
  <div class="cart">
    <div id="cartItems" class="cart-items"></div>
    <div class="checkout">
      <h3>Customer Info</h3>
      <input type="text" id="custName" placeholder="Your Name" required>
      <input type="text" id="custPhone" placeholder="Your Phone" required>
      <input type="time" id="pickupTime" required>
      <button onclick="checkout()">Checkout via WhatsApp</button>
    </div>
  </div>

  <script>
    const WHATSAPP_NUMBER = "6738121098"; // <--- put your number here (e.g. 6738123456)

    let cart = [];

    function addToCart(item, price) {
      const existing = cart.find(i => i.name === item);
      if (existing) {
        existing.qty++;
      } else {
        cart.push({ name: item, price: price, qty: 1 });
      }
      renderCart();
    }

    function renderCart() {
      const cartDiv = document.getElementById("cartItems");
      cartDiv.innerHTML = "";
      let total = 0;

      cart.forEach((item, index) => {
        total += item.price * item.qty;
        cartDiv.innerHTML += `
          <div>
            ${item.name} x ${item.qty} = $${item.price * item.qty}
            <button onclick="removeItem(${index})">-</button>
          </div>
        `;
      });

      if (cart.length > 0) {
        cartDiv.innerHTML += `<hr><strong>Total: $${total}</strong>`;
      } else {
        cartDiv.innerHTML = "<em>Cart is empty</em>";
      }
    }

    function removeItem(index) {
      if (cart[index].qty > 1) {
        cart[index].qty--;
      } else {
        cart.splice(index, 1);
      }
      renderCart();
    }

    function checkout() {
      if (cart.length === 0) {
        alert("Your cart is empty!");
        return;
      }
      let name = document.getElementById("custName").value;
      let phone = document.getElementById("custPhone").value;
      let time = document.getElementById("pickupTime").value;

      if (!name || !phone || !time) {
        alert("Please fill in your details");
        return;
      }

      let orderText = `New Order from ${name}%0APhone: ${phone}%0APickup Time: ${time}%0A%0AOrder:%0A`;
      cart.forEach(item => {
        orderText += `${item.name} x ${item.qty} = $${item.price * item.qty}%0A`;
      });

      let total = cart.reduce((sum, i) => sum + i.price * i.qty, 0);
      orderText += `%0ATotal: $${total}`;

      let url = `https://wa.me/${WHATSAPP_NUMBER}?text=${orderText}`;
      window.open(url, "_blank");
    }
  </script>
</body>
</html>
