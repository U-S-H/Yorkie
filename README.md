<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crust Pizza - App Experience</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* Smooth hiding/showing for app sections */
        .app-screen { display: none; }
        .app-screen.active { display: block; }
        body { background-color: #f3f4f6; padding-bottom: 70px; }
    </style>
</head>
<body class="font-sans">

    <!-- Top App Header -->
    <header class="bg-red-600 text-white shadow-md sticky top-0 z-50 px-4 py-3 flex justify-between items-center">
        <div class="flex items-center space-x-2">
            <i class="fa-solid fa-pizza-slice text-2xl text-yellow-300"></i>
            <div>
                <h1 class="font-extrabold text-lg tracking-wider leading-none">CRUST PIZZA</h1>
                <span class="text-[10px] text-yellow-200">Astore Branch 📍</span>
            </div>
        </div>
        <div class="flex items-center space-x-2">
            <button onclick="switchTab('admin')" class="bg-red-700 text-xs px-2.5 py-1.5 rounded-lg font-bold border border-red-500">
                <i class="fa-solid fa-gauge"></i> Admin
            </button>
        </div>
    </header>

    <!-- MAIN APP CONTAINER -->
    <main class="max-w-md mx-auto p-4">

        <!-- SCREEN 1: HOME / MENU -->
        <div id="screen-home" class="app-screen active space-y-4">
            <!-- Banner -->
            <div class="bg-gradient-to-r from-red-700 to-red-500 text-white p-4 rounded-2xl shadow-md">
                <h2 class="text-xl font-black">Hot & Spicy Pizzas 🍕</h2>
                <p class="text-xs text-yellow-100 mt-1">Order now & get it delivered fresh in 35 mins!</p>
            </div>

            <!-- Categories / Menu List -->
            <h3 class="font-bold text-gray-800 text-sm uppercase tracking-wider">Popular Items</h3>
            
            <div class="space-y-4">
                <!-- Item 1 -->
                <div class="bg-white p-3 rounded-xl shadow-sm border border-gray-100 flex space-x-3 items-center">
                    <img src="https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=200&q=80" class="w-20 h-20 object-cover rounded-lg">
                    <div class="flex-1">
                        <h4 class="font-bold text-gray-800 text-sm">Chicken Fajita</h4>
                        <p class="text-[11px] text-gray-500">Spicy chicken, onions, capsicum</p>
                        <select class="size-select text-xs border rounded mt-1 p-1 bg-gray-50 w-full">
                            <option value="Small" data-price="1200">Small (10") - Rs. 1200</option>
                            <option value="Medium" data-price="1700" selected>Medium (13") - Rs. 1700</option>
                            <option value="Large" data-price="2200">Large (16") - Rs. 2200</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-extrabold text-red-600 text-sm item-price">Rs. 1700</span>
                            <button onclick="addToCart('Chicken Fajita', this)" class="bg-red-600 text-white text-xs px-3 py-1.5 rounded-lg font-bold shadow hover:bg-red-700">Add</button>
                        </div>
                    </div>
                </div>

                <!-- Item 2 -->
                <div class="bg-white p-3 rounded-xl shadow-sm border border-gray-100 flex space-x-3 items-center">
                    <img src="https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=200&q=80" class="w-20 h-20 object-cover rounded-lg">
                    <div class="flex-1">
                        <h4 class="font-bold text-gray-800 text-sm">Super Supreme</h4>
                        <p class="text-[11px] text-gray-500">Smoked beef, pepperoni, olives</p>
                        <select class="size-select text-xs border rounded mt-1 p-1 bg-gray-50 w-full">
                            <option value="Small" data-price="1350">Small (10") - Rs. 1350</option>
                            <option value="Medium" data-price="1850" selected>Medium (13") - Rs. 1850</option>
                            <option value="Large" data-price="2400">Large (16") - Rs. 2400</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-extrabold text-red-600 text-sm item-price">Rs. 1850</span>
                            <button onclick="addToCart('Super Supreme', this)" class="bg-red-600 text-white text-xs px-3 py-1.5 rounded-lg font-bold shadow hover:bg-red-700">Add</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- SCREEN 2: PIZZA BUILDER -->
        <div id="screen-builder" class="app-screen space-y-4">
            <div class="bg-white p-4 rounded-xl shadow-sm border">
                <h3 class="font-bold text-gray-800 text-base mb-1">🛠️ Custom Pizza Maker</h3>
                <p class="text-xs text-gray-500 mb-4">Design your own recipe step-by-step.</p>
                
                <div class="space-y-3">
                    <div>
                        <label class="text-xs font-bold text-gray-600 uppercase">Select Crust:</label>
                        <select id="build-dough" class="w-full border rounded p-2 text-xs bg-gray-50 mt-1">
                            <option value="Regular Crust (Rs. 1000)">Regular Crust - Rs. 1000</option>
                            <option value="Stuffed Crust (Rs. 1300)">Stuffed Crust - Rs. 1300</option>
                        </select>
                    </div>
                    <div>
                        <label class="text-xs font-bold text-gray-600 uppercase">Extra Toppings:</label>
                        <select id="build-topping" class="w-full border rounded p-2 text-xs bg-gray-50 mt-1">
                            <option value="Extra Cheese (+Rs. 200)">Extra Cheese - Rs. 200</option>
                            <option value="Mushroom & Olives (+Rs. 250)">Mushroom & Olives - Rs. 250</option>
                            <option value="Extra Chicken (+Rs. 300)">Extra Chicken - Rs. 300</option>
                        </select>
                    </div>
                    <button onclick="addCustomPizza()" class="w-full bg-red-600 text-white font-bold py-2.5 rounded-lg text-xs shadow hover:bg-red-700 mt-2">Add Custom Pizza to Cart</button>
                </div>
            </div>
        </div>

        <!-- SCREEN 3: CART & CHECKOUT -->
        <div id="screen-cart" class="app-screen space-y-4">
            <div class="bg-white p-4 rounded-xl shadow-sm border">
                <h3 class="font-bold text-gray-800 text-base mb-3">🛒 Your Order Cart</h3>
                <div id="cart-items" class="divide-y text-xs mb-4 max-h-48 overflow-y-auto">
                    <p class="text-gray-400 text-center py-4">Cart is empty.</p>
                </div>

                <!-- Coupon -->
                <div class="flex space-x-2 mb-3">
                    <input type="text" id="coupon-code" placeholder="Promo (CRUST10)" class="border rounded px-2 py-1.5 text-xs w-full uppercase">
                    <button onclick="applyCoupon()" class="bg-gray-800 text-white px-3 py-1.5 rounded text-xs font-bold">Apply</button>
                </div>

                <!-- Totals -->
                <div class="border-t pt-2 text-xs space-y-1 mb-4">
                    <div class="flex justify-between text-gray-600"><span>Subtotal:</span><span id="subtotal">Rs. 0</span></div>
                    <div class="flex justify-between text-gray-600"><span>Discount:</span><span id="discount" class="text-green-600">Rs. 0</span></div>
                    <div class="flex justify-between text-sm font-extrabold text-gray-800 border-t pt-1"><span>Total:</span><span id="grand-total">Rs. 0</span></div>
                </div>

                <!-- Delivery Form -->
                <div class="space-y-2">
                    <h4 class="font-bold text-xs text-gray-700 uppercase">Delivery Details</h4>
                    <input type="text" id="cust-name" placeholder="Full Name" class="w-full border rounded p-2 text-xs">
                    <input type="text" id="cust-phone" placeholder="Phone Number" class="w-full border rounded p-2 text-xs">
                    <textarea id="cust-address" placeholder="Delivery Address (Astore)" class="w-full border rounded p-2 text-xs" rows="2"></textarea>
                    
                    <div class="space-y-1">
                        <label class="text-[10px] font-bold text-gray-500 uppercase">Payment Option:</label>
                        <select id="payment-method" class="w-full border rounded p-2 text-xs bg-gray-50">
                            <option value="Cash on Delivery">Cash on Delivery (COD)</option>
                            <option value="Online Payment (Easypaisa/JazzCash)">Online Payment (Easypaisa / JazzCash)</option>
                        </select>
                    </div>

                    <button onclick="checkoutWhatsApp()" class="w-full bg-green-600 text-white font-bold py-3 rounded-xl shadow-lg hover:bg-green-700 flex items-center justify-center space-x-2 mt-3 text-xs">
                        <i class="fa-brands fa-whatsapp text-lg"></i>
                        <span>Confirm Order on WhatsApp</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 4: TRACKING -->
        <div id="screen-tracking" class="app-screen space-y-4">
            <div class="bg-white p-4 rounded-xl shadow-sm border">
                <h3 class="font-bold text-gray-800 text-base mb-2">📍 Track Order Live</h3>
                <p class="text-xs text-gray-500 mb-3">Check real-time preparation status.</p>
                <div class="flex space-x-2">
                    <input type="text" id="track-id" placeholder="Order ID (e.g. #101)" class="border rounded p-2 text-xs w-full">
                    <button onclick="trackOrder()" class="bg-red-600 text-white px-4 py-2 rounded text-xs font-bold">Track</button>
                </div>
                <div id="tracking-result" class="mt-3 hidden p-3 bg-yellow-50 border border-yellow-200 rounded-lg text-xs text-yellow-800 font-semibold"></div>
            </div>
        </div>

        <!-- SCREEN 5: ADMIN PANEL -->
        <div id="screen-admin" class="app-screen space-y-4">
            <div class="bg-gray-900 text-white p-4 rounded-xl shadow-lg">
                <div class="flex justify-between items-center mb-4 border-b border-gray-800 pb-2">
                    <h3 class="font-bold text-sm text-yellow-400">📊 Admin Dashboard</h3>
                    <button onclick="simulateAlarm()" class="bg-red-600 text-[10px] px-2 py-1 rounded font-bold animate-pulse">🔔 Test Alert</button>
                </div>
                <div class="grid grid-cols-2 gap-3 text-center">
                    <div class="bg-gray-800 p-3 rounded-lg"><span class="text-[10px] text-gray-400">Today's Orders</span><h4 class="text-lg font-black text-yellow-400">24</h4></div>
                    <div class="bg-gray-800 p-3 rounded-lg"><span class="text-[10px] text-gray-400">Revenue</span><h4 class="text-lg font-black text-green-400">Rs. 42K</h4></div>
                </div>
            </div>
        </div>

    </main>

    <!-- Bottom App Navigation Bar -->
    <nav class="fixed bottom-0 left-0 right-0 bg-white border-t border-gray-200 py-2 px-6 flex justify-between items-center z-50 max-w-md mx-auto shadow-xl">
        <button onclick="switchTab('home')" id="nav-home" class="text-red-600 flex flex-col items-center text-xs font-bold space-y-1">
            <i class="fa-solid fa-house text-lg"></i><span>Menu</span>
        </button>
        <button onclick="switchTab('builder')" id="nav-builder" class="text-gray-400 flex flex-col items-center text-xs font-medium space-y-1">
            <i class="fa-solid fa-wand-magic-sparkles text-lg"></i><span>Builder</span>
        </button>
        <button onclick="switchTab('cart')" id="nav-cart" class="text-gray-400 flex flex-col items-center text-xs font-medium space-y-1 relative">
            <i class="fa-solid fa-cart-shopping text-lg"></i><span>Cart</span>
            <span id="badge-count" class="absolute -top-1 right-2 bg-red-600 text-white text-[9px] px-1.5 py-0.2 rounded-full">0</span>
        </button>
        <button onclick="switchTab('tracking')" id="nav-tracking" class="text-gray-400 flex flex-col items-center text-xs font-medium space-y-1">
            <i class="fa-solid fa-location-crosshairs text-lg"></i><span>Track</span>
        </button>
    </nav>

    <!-- JavaScript App Logic -->
    <script>
        let cart = [];
        let discountRate = 0;

        function switchTab(tabId) {
            document.querySelectorAll('.app-screen').forEach(s => s.classList.remove('active'));
            document.getElementById(`screen-${tabId}`).classList.add('active');

            // Bottom Nav Active styling
            ['home', 'builder', 'cart', 'tracking'].forEach(t => {
                const btn = document.getElementById(`nav-${t}`);
                if(btn) {
                    btn.className = (t === tabId) ? "text-red-600 flex flex-col items-center text-xs font-bold space-y-1" : "text-gray-400 flex flex-col items-center text-xs font-medium space-y-1";
                }
            });
            window.scrollTo(0,0);
        }

        // Dynamic price changer
        document.querySelectorAll('.size-select').forEach(select => {
            select.addEventListener('change', function() {
                const price = this.options[this.selectedIndex].getAttribute('data-price');
                this.closest('.flex-1').querySelector('.item-price').innerText = `Rs. ${price}`;
            });
        });

        function addToCart(name, btn) {
            const container = btn.closest('.flex-1');
            const sizeSelect = container.querySelector('.size-select');
            const size = sizeSelect.value;
            const price = parseInt(sizeSelect.options[sizeSelect.selectedIndex].getAttribute('data-price'));

            cart.push({ name, size, price });
            updateCartUI();
            alert(`${name} (${size}) added to app cart!`);
        }

        function addCustomPizza() {
            const dough = document.getElementById('build-dough').value;
            const topping = document.getElementById('build-topping').value;
            let price = dough.includes('1300') ? 1300 : 1000;
            if(topping.includes('250')) price += 250;
            if(topping.includes('300')) price += 300;
            if(topping.includes('200')) price += 200;

            cart.push({ name: `Custom Pizza`, size: `${dough.split(' ')[0]} + ${topping.split(' ')[0]}`, price });
            updateCartUI();
            switchTab('cart');
        }

        function updateCartUI() {
            const container = document.getElementById('cart-items');
            document.getElementById('badge-count').innerText = cart.length;

            if(cart.length === 0) {
                container.innerHTML = '<p class="text-gray-400 text-center py-4">Cart is empty.</p>';
                document.getElementById('subtotal').innerText = 'Rs. 0';
                document.getElementById('grand-total').innerText = 'Rs. 0';
                return;
            }

            container.innerHTML = '';
            let subtotal = 0;
            cart.forEach((item, index) => {
                subtotal += item.price;
                container.innerHTML += `
                    <div class="py-2 flex justify-between items-center">
                        <div><b>${item.name}</b><br><span class="text-[10px] text-gray-500">${item.size} - Rs. ${item.price}</span></div>
                        <button onclick="cart.splice(${index},1);updateCartUI()" class="text-red-500 font-bold text-xs">Remove</button>
                    </div>
                `;
            });

            let discount = subtotal * discountRate;
            let total = subtotal - discount;
            document.getElementById('subtotal').innerText = `Rs. ${subtotal}`;
            document.getElementById('discount').innerText = `Rs. ${discount}`;
            document.getElementById('grand-total').innerText = `Rs. ${total}`;
        }

        function applyCoupon() {
            if(document.getElementById('coupon-code').value.trim().toUpperCase() === 'CRUST10') {
                discountRate = 0.10;
                updateCartUI();
                alert('10% Coupon Applied!');
            } else {
                alert('Invalid Code. Use CRUST10');
            }
        }

        function checkoutWhatsApp() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value;
            const payment = document.getElementById('payment-method').value;

            if(!name || !phone || !address || cart.length === 0) {
                alert('Please enter delivery info and add items!');
                return;
            }

            let msg = `*App Order - Crust Pizza*%0A*Name:* ${name}%0A*Phone:* ${phone}%0A*Address:* ${address}%0A*Payment:* ${payment}%0A%0A*Items:*%0A`;
            let sub = 0;
            cart.forEach(i => { msg += `- ${i.name} (${i.size}): ${i.price}%0A`; sub += i.price; });
            let final = sub - (sub * discountRate);
            msg += `%0A*Total:* Rs. ${final} (ETA: 35 mins)`;

            window.open(`https://wa.me/923001234567?text=${msg}`, '_blank');
        }

        function trackOrder() {
            const id = document.getElementById('track-id').value;
            const res = document.getElementById('tracking-result');
            if(id) {
                res.classList.remove('hidden');
                res.innerHTML = `Order #${id}: <span class="text-red-600 font-bold">🔥 Preparing fresh in kitchen!</span>`;
            } else {
                alert('Enter order ID');
            }
        }

        function simulateAlarm() {
            alert('🚨 [APP PUSH NOTIFICATION SIMULATION] New Order Received on Kitchen Screen!');
        }
    </script>
</body>
</html>
