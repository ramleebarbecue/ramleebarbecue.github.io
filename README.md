<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ramlee Barbecue - Order Online</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body {
      font-family: 'Inter', sans-serif;
    }
    .cart-count {
      display: none;
    }
    .cart-count.active {
      display: inline-block;
    }
  </style>
</head>
<body class="bg-gray-100">
  <!-- Navbar -->
  <nav class="bg-orange-600 text-white p-4 sticky top-0 z-10 shadow-md">
    <div class="container mx-auto flex justify-between items-center">
      <h1 class="text-2xl font-bold">Ramlee Barbecue</h1>
      <div class="relative">
        <button id="cart-btn" class="flex items-center space-x-2">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
          </svg>
          <span id="cart-count" class="cart-count bg-red-500 text-white rounded-full px-2 py-1 text-xs">0</span>
        </button>
      </div>
    </div>
  </nav>

  <!-- Hero Section -->
  <section class="bg-orange-100 py-12">
    <div class="container mx-auto flex flex-col md:flex-row items-center">
      <div class="md:w-1/2 text-center md:text-left">
        <h2 class="text-4xl font-bold text-gray-800 mb-4">Ramlee Barbecue Signature Grilled Wings</h2>
        <p class="text-gray-600 mb-6">Savor our juicy, smoky grilled chicken wings, crafted with love and the finest spices. Order now and satisfy your cravings!</p>
        <button class="bg-orange-600 text-white px-6 py-3 rounded-full hover:bg-orange-700 transition">Shop Now</button>
      </div>
      <div class="md:w-1/2 mt-6 md:mt-0">
        <img src="https://via.placeholder.com/500x300?text=Grilled+Wings" alt="Grilled Chicken Wings" class="rounded-lg shadow-lg w-full">
      </div>
    </div>
  </section>

  <!-- Product Section -->
  <section class="container mx-auto py-12">
    <h2 class="text-3xl font-bold text-center text-gray-800 mb-8">Our Menu</h2>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
      <!-- Product Card -->
      <div class="bg-white p-6 rounded-lg shadow-md">
        <img src="https://via.placeholder.com/300x200?text=Signature+Wings" alt="Signature Grilled Wings" class="w-full h-48 object-cover rounded-md mb-4">
        <h3 class="text-xl font-semibold text-gray-800">Sayap Ayam Panggang</h3>
        <p class="text-gray-600 mb-4">BND 6.00 (5 pieces)</p>
        <div class="flex items-center space-x-2 mb-4">
          <button class="decrease-qty bg-gray-300 text-gray-800 px-3 py-1 rounded">-</button>
          <input type="number" class="quantity w-16 text-center border rounded" value="1" min="1">
          <button class="increase-qty bg-gray-300 text-gray-800 px-3 py-1 rounded">+</button>
        </div>
        <button class="add-to-cart bg-orange-600 text-white w-full py-2 rounded hover:bg-orange-700 transition">Add to Cart</button>
      </div><section class="container mx-auto py-12">
    <h2 class="text-3xl font-bold text-center text-gray-800 mb-8">Our Menu</h2>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
      <!-- Product Card -->
      <div class="bg-white p-6 rounded-lg shadow-md">
        <img src="https://via.placeholder.com/300x200?text=Signature+Wings" alt="Signature Grilled Wings" class="w-full h-48 object-cover rounded-md mb-4">
        <h3 class="text-xl font-semibold text-gray-800">Tongking Panggang</h3>
        <p class="text-gray-600 mb-4">BND 6.00 (5 pieces)</p>
        <div class="flex items-center space-x-2 mb-4">
          <button class="decrease-qty bg-gray-300 text-gray-800 px-3 py-1 rounded">-</button>
          <input type="number" class="quantity w-16 text-center border rounded" value="1" min="1">
          <button class="increase-qty bg-gray-300 text-gray-800 px-3 py-1 rounded">+</button>
        </div>
        <button class="add-to-cart bg-orange-600 text-white w-full py-2 rounded hover:bg-orange-700 transition">Add to Cart</button>
      </div>
      <!-- Add more products here if needed -->
    </div>
  </section>

  <!-- Cart Modal -->
  <div id="cart-modal" class="fixed inset-0 bg-black bg-opacity-50 hidden flex items-center justify-center z-20">
    <div class="bg-white p-6 rounded-lg w-full max-w-md">
      <h2 class="text-2xl font-bold mb-4">Your Cart</h2>
      <div id="cart-items" class="mb-4"></div>
      <p id="cart-total" class="text-lg font-semibold mb-4">Total: RM 0.00</p>
      <div class="flex justify-between">
        <button id="close-cart" class="bg-gray-300 text-gray-800 px-4 py-2 rounded hover:bg-gray-400">Close</button>
        <button id="checkout-btn" class="bg-orange-600 text-white px-4 py-2 rounded hover:bg-orange-700">Checkout</button>
      </div>
    </div>
  </div>

  <script>
    let cart = [];

    // Update cart count display
    function updateCartCount() {
      const cartCount = document.getElementById('cart-count');
      const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
      cartCount.textContent = totalItems;
      cartCount.classList.toggle('active', totalItems > 0);
    }

    // Update cart display
    function updateCartDisplay() {
      const cartItems = document.getElementById('cart-items');
      const cartTotal = document.getElementById('cart-total');
      cartItems.innerHTML = '';
      let total = 0;

      cart.forEach((item, index) => {
        const itemTotal = item.price * item.quantity;
        total += itemTotal;
        cartItems.innerHTML += `
          <div class="flex justify-between items-center mb-2">
            <div>
              <p class="font-semibold">${item.name}</p>
              <p>RM ${item.price.toFixed(2)} x ${item.quantity}</p>
            </div>
            <button class="remove-item text-red-500" data-index="${index}">Remove</button>
          </div>
        `;
      });

      cartTotal.textContent = `Total: RM ${total.toFixed(2)}`;

      // Add event listeners for remove buttons
      document.querySelectorAll('.remove-item').forEach(button => {
        button.addEventListener('click', () => {
          const index = button.dataset.index;
          cart.splice(index, 1);
          updateCartDisplay();
          updateCartCount();
        });
      });
    }

    // Add to cart
    document.querySelectorAll('.add-to-cart').forEach(button => {
      button.addEventListener('click', () => {
        const card = button.closest('.bg-white');
        const name = card.querySelector('h3').textContent;
        const price = parseFloat(card.querySelector('p').textContent.replace('RM ', ''));
        const quantity = parseInt(card.querySelector('.quantity').value);

        const existingItem = cart.find(item => item.name === name);
        if (existingItem) {
          existingItem.quantity += quantity;
        } else {
          cart.push({ name, price, quantity });
        }

        updateCartCount();
        updateCartDisplay();
      });
    });

    // Quantity controls
    document.querySelectorAll('.increase-qty').forEach(button => {
      button.addEventListener('click', () => {
        const input = button.previousElementSibling;
        input.value = parseInt(input.value) + 1;
      });
    });

    document.querySelectorAll('.decrease-qty').forEach(button => {
      button.addEventListener('click', () => {
        const input = button.nextElementSibling;
        if (parseInt(input.value) > 1) {
          input.value = parseInt(input.value) - 1;
        }
      });
    });

    // Cart modal toggle
    document.getElementById('cart-btn').addEventListener('click', () => {
      document.getElementById('cart-modal').classList.toggle('hidden');
    });

    document.getElementById('close-cart').addEventListener('click', () => {
      document.getElementById('cart-modal').classList.add('hidden');
    });

    // Checkout (placeholder)
    document.getElementById('checkout-btn').addEventListener('click', () => {
      alert('Proceeding to checkout! (This is a placeholder)');
    });
  </script>
</body>
</html>
