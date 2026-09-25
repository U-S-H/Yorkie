<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crust Pizza - Ultimate Online Ordering & Demo</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body class="bg-gray-50 font-sans">

    <!-- Header / Navbar -->
    <header class="bg-red-600 text-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <i class="fa-solid fa-pizza-slice text-3xl"></i>
                <span class="text-2xl font-extrabold tracking-wider" id="brand-title">CRUST PIZZA</span>
            </div>
            <nav class="hidden md:flex space-x-6 font-medium items-center">
                <a href="#menu" class="hover:text-yellow-300 transition">Menu</a>
                <a href="#builder" class="hover:text-yellow-300 transition">Build Pizza</a>
                <a href="#reservation" class="hover:text-yellow-300 transition">Book Table</a>
                <a href="#tracking" class="hover:text-yellow-300 transition">Track Order</a>
                <a href="#admin" class="hover:text-yellow-300 transition bg-red-700 px-3 py-1 rounded">Admin Panel</a>
                <!-- Language Switcher -->
                <button onclick="toggleLanguage()" class="bg-yellow-400 text-gray-900 px-2.5 py-1 rounded text-xs font-bold hover:bg-yellow-300 transition">Urdu / Eng</button>
            </nav>
            <div class="flex items-center space-x-4">
                <button onclick="toggleCart()" class="relative bg-white text-red-600 px-4 py-2 rounded-full font-bold shadow flex items-center space-x-2 hover:bg-yellow-100 transition">
                    <i class="fa-solid fa-cart-shopping"></i>
                    <span>Cart</span>
                    <span id="cart-count" class="bg-red-600 text-white text-xs px-2 py-0.5 rounded-full">0</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="bg-gradient-to-r from-red-700 to-red-500 text-white py-16 px-4 text-center">
        <div class="max-w-3xl mx-auto">
            <h1 class="text-4xl md:text-5xl font-extrabold mb-4" id="hero-heading">Hot & Delicious Pizzas Delivered in Astore!</h1>
            <p class="text-lg mb-8 text-yellow-100" id="hero-sub">Order online instantly with live tracking, custom pizzas, and table reservations.</p>
            <a href="#menu" class="bg-yellow-400 text-gray-900 font-bold px-8 py-3 rounded-full text-lg shadow-lg hover:bg-yellow-300 transition">Explore Menu</a>
        </div>
    </section>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto px-4 py-10 grid grid-cols-1 lg:grid-cols-3 gap-8">
        
        <!-- Menu & Customizer Section -->
        <div class="lg:col-span-2 space-y-12">
            
            <!-- Standard Menu -->
            <div id="menu">
                <h2 class="text-3xl font-bold text-gray-800 mb-6 border-b pb-2">Our Special Menu</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <!-- Product 1 -->
                    <div class="bg-white rounded-xl shadow-md overflow-hidden border border-gray-100 flex flex-col justify-between">
                        <div>
                            <img src="https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=500&q=80" alt="Fajita Pizza" class="w-full h-48 object-cover">
                            <div class="p-4">
                                <h3 class="font-bold text-xl text-gray-800">Chicken Fajita Pizza</h3>
                                <p class="text-gray-500 text-sm mt-1">Spicy chicken, onions, capsicum, and mozzarella cheese.</p>
                                <div class="mt-4 space-y-2">
                                    <label class="text-xs font-bold text-gray-600 uppercase">Select Size:</label>
                                    <select class="size-select w-full border rounded p-1.5 text-sm bg-gray-50">
                                        <option value="Small" data-price="1200">Small (10") - Rs. 1200</option>
                                        <option value="Medium" data-price="1700" selected>Medium (13") - Rs. 1700</option>
                                        <option value="Large" data-price="2200">Large (16") - Rs. 2200</option>
                                    </select>
                                </div>
                            </div>
                        </div>
                        <div class="p-4 bg-gray-50 border-t flex justify-between items-center">
                            <span class="text-xl font-extrabold text-red-600 item-price">Rs. 1700</span>
                            <button onclick="addToCart('Chicken Fajita Pizza', this)" class="bg-red-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-red-700 transition">Add to Cart</button>
                        </div>
                    </div>

                    <!-- Product 2 -->
                    <div class="bg-white rounded-xl shadow-md overflow-hidden border border-gray-100 flex flex-col justify-between">
                        <div>
                            <img src="https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=500&q=80" alt="Super Supreme" class="w-full h-48 object-cover">
                            <div class="p-4">
                                <h3 class="font-bold text-xl text-gray-800">Super Supreme Pizza</h3>
                                <p class="text-gray-500 text-sm mt-1">Smoked beef, pepperoni, mushrooms, olives & cheese.</p>
                                <div class="mt-4 space-y-2">
                                    <label class="text-xs font-bold text-gray-600 uppercase">Select Size:</label>
                                    <select class="size-select w-full border rounded p-1.5 text-sm bg-gray-50">
                                        <option value="Small" data-price="1350">Small (10") - Rs. 1350</option>
                                        <option value="Medium" data-price="1850" selected>Medium (13") - Rs. 1850</option>
                                        <option value="Large" data-price="2400">Large (16") - Rs. 2400</option>
                                    </select>
                                </div>
                            </div>
                        </div>
                        <div class="p-4 bg-gray-50 border-t flex justify-between items-center">
                            <span class="text-xl font-extrabold text-red-600 item-price">Rs. 1850</span>
                            <button onclick="addToCart('Super Supreme Pizza', this)" class="bg-red-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-red-700 transition">Add to Cart</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Pizza Builder / Customizer Section -->
            <div id="builder" class="bg-white p-6 rounded-xl shadow-md border border-gray-100">
                <h2 class="text-2xl font-bold text-gray-800 mb-2 flex items-center space-x-2">
                    <i class="fa-solid fa-wand-magic-sparkles text-red-600"></i>
                    <span>Custom Pizza Builder</span>
                </h2>
                <p class="text-gray-500 text-sm mb-4">Design your own personalized pizza step by step!</p>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4">
                    <div>
                        <label class="text-xs font-bold text-gray-600 uppercase">Base Dough:</label>
                        <select id="build-dough" class="w-full border rounded p-2 text-sm mt-1 bg-gray-50">
                            <option value="Regular Crust (Rs. 1000)">Regular Crust - Rs. 1000</option>
                            <option value="Stuffed Crust (Rs. 1300)">Stuffed Crust - Rs. 1300</option>
                            <option value="Thin Crispy (Rs. 1100)">Thin Crispy - Rs. 1100</option>
                        </select>
                    </div>
                    <div>
                        <label class="text-xs font-bold text-gray-600 uppercase">Extra Toppings:</label>
                        <select id="build-topping" class="w-full border rounded p-2 text-sm mt-1 bg-gray-50">
                            <option value="Extra Cheese (+Rs. 200)">Extra Cheese - Rs. 200</option>
                            <option value="Mushroom & Olives (+Rs. 250)">Mushroom & Olives - Rs. 250</option>
                            <option value="Extra Chicken (+Rs. 300)">Extra Chicken - Rs. 300</option>
                        </select>
                    </div>
                    <div class="flex items-end">
                        <button onclick="addCustomPizza()" class="w-full bg-red-600 text-white font-bold py-2 rounded shadow hover:bg-red-700 transition">Add Custom Pizza</button>
                    </div>
                </div>
            </div>

            <!-- Table Reservation Section -->
            <div id="reservation" class="bg-white p-6 rounded-xl shadow-md border border-gray-100">
                <h2 class="text-2xl font-bold text-gray-800 mb-2 flex items-center space-x-2">
                    <i class="fa-solid fa-chair text-red-600"></i>
                    <span>Dine-In Table Reservation</span>
                </h2>
                <p class="text-gray-500 text-sm mb-4">Reserve a table at our Astore branch in advance.</p>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <input type="text" id="res-name" placeholder="Your Name" class="border rounded p-2 text-sm">
                    <input type="number" id="res-guests" placeholder="Number of Guests" class="border rounded p-2 text-sm">
                    <input type="datetime-local" id="res-time" class="border rounded p-2 text-sm">
                </div>
                <button onclick="bookTable()" class="mt-4 bg-gray-900 text-white font-bold px-6 py-2 rounded text-sm hover:bg-gray-800 transition">Confirm Reservation</button>
            </div>
        </div>

        <!-- Sidebar: Cart, ETA & Checkout -->
        <div class="bg-white rounded-xl shadow-md p-6 border border-gray-100 h-fit sticky top-24">
            <h3 class="text-2xl font-bold text-gray-800 mb-4 flex items-center justify-between">
                <span>Your Cart</span>
                <i class="fa-solid fa-basket-shopping text-red-600"></i>
            </h3>

            <div id="cart-items" class="divide-y max-h-60 overflow-y-auto mb-4">
                <p class="text-gray-500 text-center py-4">Your cart is empty.</p>
            </div>

            <!-- Coupon Box -->
            <div class="mb-4 flex space-x-2">
                <input type="text" id="coupon-code" placeholder="Promo Code (CRUST10)" class="border rounded px-3 py-2 text-sm w-full uppercase">
                <button onclick="applyCoupon()" class="bg-gray-800 text-white px-4 py-2 rounded text-sm font-semibold hover:bg-gray-700">Apply</button>
            </div>

            <!-- Bill Summary & ETA -->
            <div class="border-t pt-4 space-y-2 text-sm">
                <div class="flex justify-between text-gray-600">
                    <span>Subtotal</span>
                    <span id="subtotal">Rs. 0</span>
                </div>
                <div class="flex justify-between text-gray-600">
                    <span>Discount</span>
                    <span id="discount" class="text-green-600">Rs. 0</span>
                </div>
                <div class="flex justify-between text-gray-600 font-semibold bg-yellow-50 p-1.5 rounded">
                    <span>Estimated Delivery Time:</span>
                    <span id="eta-time" class="text-red-600">35-45 mins</span>
                </div>
                <div class="flex justify-between text-lg font-bold text-gray-800 border-t pt-2">
                    <span>Total Amount</span>
                    <span id="grand-total">Rs. 0</span>
                </div>
            </div>

            <!-- Customer Details Form -->
            <div class="mt-6 space-y-3">
                <h4 class="font-bold text-gray-700">Delivery Details</h4>
                <input type="text" id="cust-name" placeholder="Full Name" class="w-full border rounded p-2 text-sm">
                <input type="text" id="cust-phone" placeholder="Active Phone Number" class="w-full border rounded p-2 text-sm">
                <textarea id="cust-address" placeholder="Delivery Address in Astore" class="w-full border rounded p-2 text-sm" rows="2"></textarea>
                
                <!-- Dual Payment System -->
                <div class="space-y-1">
                    <label class="text-xs font-bold text-gray-600 uppercase">Payment Method:</label>
                    <select id="payment-method" class="w-full border rounded p-2 text-sm bg-gray-50">
                        <option value="Cash on Delivery">Cash on Delivery (COD)</option>
                        <option value="Online Payment (Easypaisa/JazzCash)">Online Payment (Easypaisa / JazzCash)</option>
                    </select>
                </div>

                <button onclick="checkoutWhatsApp()" class="w-full bg-green-600 text-white font-bold py-3 rounded-lg shadow hover:bg-green-700 transition flex items-center justify-center space-x-2 mt-4">
                    <i class="fa-brands fa-whatsapp text-xl"></i>
                    <span>Confirm Order via WhatsApp</span>
                </button>
            </div>
        </div>
    </main>

    <!-- Order Tracking Section -->
    <section id="tracking" class="max-w-7xl mx-auto px-4 py-10 bg-white rounded-xl shadow-md my-10">
        <h2 class="text-2xl font-bold text-gray-800 mb-4">Live Order Status Tracking</h2>
        <p class="text-gray-600 text-sm mb-6">Enter your order ID to check real-time cooking & delivery status.</p>
        <div class="flex space-x-4 max-w-md">
            <input type="text" id="track-id" placeholder="Enter Order ID (e.g. #102)" class="border rounded px-4 py-2 w-full">
            <button onclick="trackOrder()" class="bg-red-600 text-white px-6 py-2 rounded font-semibold hover:bg-red-700">Track</button>
        </div>
        <div id="tracking-result" class="mt-4 hidden p-4 bg-yellow-50 border border-yellow-200 rounded-lg text-yellow-800 font-semibold"></div>
    </section>

    <!-- Admin Panel Demo Section (With Audio Alarm Simulation) -->
    <section id="admin" class="max-w-7xl mx-auto px-4 py-10 bg-gray-900 text-white rounded-xl shadow-xl my-10">
        <div class="flex justify-between items-center mb-6 border-b border-gray-800 pb-4">
            <h2 class="text-2xl font-bold flex items-center space-x-2">
                <i class="fa-solid fa-gauge text-yellow-400"></i>
                <span>Admin Dashboard (Back-End Panel)</span>
            </h2>
            <div class="flex items-center space-x-3">
                <button onclick="simulateNewOrderAlert()" class="bg-red-600 text-xs px-3 py-1.5 rounded font-bold animate-pulse hover:bg-red-700">🔔 Simulate New Order Alarm</button>
                <span class="bg-green-500 text-xs px-3 py-1 rounded-full font-bold">Live</span>
            </div>
        </div>

        <!-- Sales Analytics -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
            <div class="bg-gray-800 p-5 rounded-lg border border-gray-700">
                <p class="text-gray-400 text-sm">Today's Orders</p>
                <h3 class="text-3xl font-extrabold text-yellow-400 mt-1">24 Orders</h3>
            </div>
            <div class="bg-gray-800 p-5 rounded-lg border border-gray-700">
                <p class="text-gray-400 text-sm">Weekly Sales</p>
                <h3 class="text-3xl font-extrabold text-green-400 mt-1">Rs. 142,500</h3>
            </div>
            <div class="bg-gray-800 p-5 rounded-lg border border-gray-700">
                <p class="text-gray-400 text-sm">Monthly Revenue</p>
                <h3 class="text-3xl font-extrabold text-blue-400 mt-1">Rs. 580,000</h3>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white text-center py-6">
        <p>&copy; 2026 Crust Pizza Astore. Powered by Prime Solutions.</p>
    </footer>

    <!-- JavaScript Logic -->
    <script>
        let cart = [];
        let discountRate = 0;
        let isUrdu = false;

        // Dynamic price update on size change
        document.querySelectorAll('.size-select').forEach(select => {
            select.addEventListener('change', function() {
                const selectedOption = this.options[this.selectedIndex];
                const price = selectedOption.getAttribute('data-price');
                const card = this.closest('.bg-white');
                card.querySelector('.item-price').innerText = `Rs. ${price}`;
            });
        });

        function addToCart(productName, button) {
            const card = button.closest('.bg-white');
            const sizeSelect = card.querySelector('.size-select');
            const size = sizeSelect.value;
            const price = parseInt(sizeSelect.options[sizeSelect.selectedIndex].getAttribute('data-price'));

            cart.push({ name: productName, size: size, price: price });
            updateCartUI();
        }

        function addCustomPizza() {
            const dough = document.getElementById('build-dough').value;
            const topping = document.getElementById('build-topping').value;
            
            let price = 1000;
            if(dough.includes('1300')) price = 1300;
            if(dough.includes('1100')) price = 1100;
            if(topping.includes('250')) price += 250;
            if(topping.includes('300')) price += 300;
            if(topping.includes('200')) price += 200;

            cart.push({ name: `Custom Pizza (${dough.split(' ')[0]})`, size: topping, price: price });
            alert('Custom Pizza added successfully to cart!');
            updateCartUI();
        }

        function bookTable() {
            const name = document.getElementById('res-name').value;
            const guests = document.getElementById('res-guests').value;
            const time = document.getElementById('res-time').value;

            if(!name || !guests || !time) {
                alert('Please fill in all table reservation details!');
                return;
            }
            alert(`Table reserved successfully for ${name} (${guests} Guests) at ${time}!`);
        }

        function updateCartUI() {
            const cartItemsContainer = document.getElementById('cart-items');
            const cartCount = document.getElementById('cart-count');
            
            cartCount.innerText = cart.length;

            if (cart.length === 0) {
                cartItemsContainer.innerHTML = '<p class="text-gray-500 text-center py-4">Your cart is empty.</p>';
                document.getElementById('subtotal').innerText = 'Rs. 0';
                document.getElementById('grand-total').innerText = 'Rs. 0';
                return;
            }

            cartItemsContainer.innerHTML = '';
            let subtotal = 0;

            cart.forEach((item, index) => {
                subtotal += item.price;
                cartItemsContainer.innerHTML += `
                    <div class="py-2 flex justify-between items-center text-sm border-b">
                        <div>
                            <p class="font-bold">${item.name}</p>
                            <span class="text-xs text-gray-500">${item.size} | Rs. ${item.price}</span>
                        </div>
                        <button onclick="removeFromCart(${index})" class="text-red-500 hover:text-red-700 text-xs font-bold">Remove</button>
                    </div>
                `;
            });

            let discountAmount = (subtotal * discountRate);
            let grandTotal = subtotal - discountAmount;

            document.getElementById('subtotal').innerText = `Rs. ${subtotal}`;
            document.getElementById('discount').innerText = `Rs. ${discountAmount}`;
            document.getElementById('grand-total').innerText = `Rs. ${grandTotal}`;
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            updateCartUI();
        }

        function applyCoupon() {
            const code = document.getElementById('coupon-code').value.trim().toUpperCase();
            if (code === 'CRUST10') {
                discountRate = 0.10;
                alert('Coupon Applied: 10% Discount Added!');
                updateCartUI();
            } else {
                alert('Invalid Coupon Code! Try using CRUST10');
            }
        }

        function checkoutWhatsApp() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value;
            const paymentMethod = document.getElementById('payment-method').value;

            if (!name || !phone || !address || cart.length === 0) {
                alert('Please fill in all delivery details and add items to cart!');
                return;
            }

            let orderText = `*New Order - Crust Pizza*%0A%0A`;
            orderText += `*Customer Name:* ${name}%0A`;
            orderText += `*Phone:* ${phone}%0A`;
            orderText += `*Address:* ${address}%0A`;
            orderText += `*Payment Method:* ${paymentMethod}%0A%0A*Items:*%0A`;

            let subtotal = 0;
            cart.forEach(item => {
                orderText += `- ${item.name} (${item.size}): Rs. ${item.price}%0A`;
                subtotal += item.price;
            });

            let finalTotal = subtotal - (subtotal * discountRate);
            orderText += `%0A*Total Bill:* Rs. ${finalTotal} (ETA: 35-45 mins)`;

            const ownerWhatsApp = "923001234567"; 
            window.open(`https://wa.me/${ownerWhatsApp}?text=${orderText}`, '_blank');
        }

        function trackOrder() {
            const orderId = document.getElementById('track-id').value.trim();
            const resultBox = document.getElementById('tracking-result');
            if(orderId) {
                resultBox.classList.remove('hidden');
                resultBox.innerHTML = `Order ID #${orderId}: Status is <span class="text-red-600 font-extrabold">🔥 Cooking in Kitchen (Out for delivery soon)</span>`;
            } else {
                alert('Please enter a valid order ID');
            }
        }

        function simulateNewOrderAlert() {
            // Audio beep simulation for admin alert
            const audio = new Audio('https://www.soundjay.com/buttons/sounds/beep-07.mp3');
            audio.play().catch(e => console.log('Audio autoplay restricted'));
            alert('🚨 NEW ORDER RECEIVED! WhatsApp alert dispatched to owner & kitchen screen updated.');
        }

        function toggleLanguage() {
            isUrdu = !isUrdu;
            if(isUrdu) {
                document.getElementById('hero-heading').innerText = 'Astore mein garm aur mazedaar pizza ghar par mangwayein!';
                document.getElementById('hero-sub').innerText = 'Live tracking, custom pizzas aur table booking ke sath foran order karein.';
            } else {
                document.getElementById('hero-heading').innerText = 'Hot & Delicious Pizzas Delivered in Astore!';
                document.getElementById('hero-sub').innerText = 'Order online instantly with live tracking, custom pizzas, and table reservations.';
            }
        }
    </script>
</body>
</html>
