<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ramlee Barbecue - Order Online</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body { font-family: 'Inter', sans-serif; }
    .cart-count { display: none; }
    .cart-count.active { display: inline-block; }
  </style>
</head>
<body class="bg-gray-100">

  <!-- Navbar -->
  <nav class="bg-orange-600 text-white p-4 sticky top-0 z-10 shadow-md">
    <div class="container mx-auto flex justify-between items-center">
      <h1 class="text-2xl font-bold">Ramlee Barbecue</h1>
      <button id="cart-btn" class="relative flex items-center space-x-2">
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
            d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 
            2.293c-.63.63-.184 1.707.707 1.707H17m0 
            0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 
            11-4 0 2 2 0 014 0z"/>
        </svg>
        <span id="cart-count"
          class="cart-count absolute -top-2 -right-2 bg-red-500 text-white 
                 rounded-full px-2 py-0.5 text-xs font-bold">0</span>
      </button>
    </div>
  </nav>

  <!-- Hero Section -->
  <section class="bg-orange-100 py-12">
    <div class="container mx-auto flex flex-col md:flex-row items-center">
      <div class="md:w-1/2 text-center md:text-left">
        <h2 class="text-4xl font-bold text-gray-800 mb-4">
          Ramlee Barbecue Signature Grilled Wings
        </h2>
        <p class="text-gray-600 mb-6">
          Savor our juicy, smoky grilled chicken wings, crafted with love and the finest spices.
        </p>
        <button class="bg-orange-600 text-white px-6 py-3 rounded-full 
                       hover:bg-orange-700 transition">Shop Now</button>
      </div>
      <div class="md:w-1/2 mt-6 md:mt-0">
        <img src="https://via.placeholder.com/500x300?text=Grilled+Wings"
             alt="Delicious grilled chicken wings"
             class="rounded-lg shadow-lg w-full">
      </div>
    </div>
  </section>

  <!-- Product Section -->
  <section class="container mx-auto py-12">
    <h2 class="text-3xl font-bold text-center text-gray-800 mb-8">Our Menu</h2>
    <div id="product-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6"></div>
  </section>

  <!-- Cart Modal -->
  <div id="cart-modal" class="fixed inset-0 bg-black bg-opacity-50 hidden flex items-center justify-center z-20">
    <div class="bg-white p-6 rounded-lg w-full max-w-md">
      <h2 class="text-2xl font-bold mb-4">Your Cart</h2>
      <div id="cart-items" class="mb-4"></div>
      <p id="cart-total" class="text-lg font-semibold mb-4">Total: BND 0.00</p>
      <div class="flex justify-between">
        <button id="close-cart"
          class="bg-gray-300 text-gray-800 px-4 py-2 rounded hover:bg-gray-400">Close</button>
        <button id="checkout-btn"
          class="bg-orange-600 text-white px-4 py-2 rounded hover:bg-orange-700">Checkout</button>
      </div>
    </div>
  </div>

  <script>
    // Example product data
    const products = [
      { name: "Sayap Ayam Panggang", price: 6.00, img: "https://via.placeholder.com/300x200?text=Signature+Wings" },
      { name: "Tongking Panggang", price: 6.00, img: "https://via.placeholder.com/300x200?text=Tongking+Panggang" },
    ];

    const cart = [];

    // Render products dynamically
    const productList = document.getElementById("product-list");
    products.forEach((product, idx) => {
      const card = document.createElement("div");
      card.className = "bg-white p-6 rounded-lg shadow-md";
      card.innerHTML = `
        <img src="${product.img}" alt="${product.name}" 
             class="w-full h-48 object-cover rounded-md mb-4">
        <h3 class="text-xl font-semibold text-gray-800">${product.name}</h3>
        <p class="text-gray-600 mb-4">BND ${product.price.toFixed(2)}</p>
        <div class="flex items-center space-x-2 mb-4">
          <button class="decrease-qty bg-gray-300 text-gray-800 px-3 py-1 rounded">-</button>
          <input type="number" class="quantity w-16 text-center border rounded" value="1" min="1">
          <button class="increase-qty bg-gray-300 text-gray-800 px-3 py-1 rounded">+</button>
        </div>
        <button class="add-to-cart bg-orange-600 text-white w-full py-2 rounded hover:bg-orange-700 transition">Add to Cart</button>
      `;
      productList.appendChild(card);
    });

    // Cart updates
    function updateCartCount() {
      const cartCount = document.getElementById("cart-count");
      const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
      cartCount.textContent = totalItems;
      cartCount.classList.toggle("active", totalItems > 0);
    }

    function updateCartDisplay() {
      const cartItems = document.getElementById("cart-items");
      const cartTotal = document.getElementById("cart-total");
      cartItems.innerHTML = "";
      let total = 0;

      cart.forEach((item, index) => {
        const itemTotal = item.price * item.quantity;
        total += itemTotal;

        const div = document.createElement("div");
        div.className = "flex justify-between items-center mb-2";
        div.innerHTML = `
          <div>
            <p class="font-semibold">${item.name}</p>
            <p>BND ${item.price.toFixed(2)} x ${item.quantity}</p>
          </div>
          <button class="r
