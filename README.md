<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crust Pizza - Pro Super App</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .app-screen { display: none; opacity: 0; transition: opacity 0.3s ease-in-out; }
        .app-screen.active { display: block; opacity: 1; }
        body { background-color: #0f172a; color: #f8fafc; padding-bottom: 90px; margin: 0; }
        .glass { background: rgba(30, 41, 59, 0.9); backdrop-filter: blur(12px); }
        @keyframes pulse-slow { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }
        .animate-pulse-slow { animation: pulse-slow 3s infinite; }
        
        #splash-screen {
            position: fixed; inset: 0; background: #0f172a; z-index: 99999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 0.5s ease;
        }
        .bottom-nav {
            position: fixed; bottom: 0; left: 0; right: 0; max-width: 28rem;
            margin: 0 auto; display: flex; flex-direction: row; justify-content: space-around;
            align-items: center; z-index: 9999;
        }
        /* Toast Popup Animation */
        #toast-container {
            position: fixed; top: 70px; left: 50%; transform: translateX(-50%);
            z-index: 99999; width: 90%; max-width: 400px; pointer-events: none;
        }
    </style>
</head>
<body class="font-sans antialiased max-w-md mx-auto min-h-screen relative shadow-2xl overflow-x-hidden border-x border-slate-800">

    <!-- Splash Loading Screen -->
    <div id="splash-screen">
        <div class="bg-red-600 p-4 rounded-3xl text-white shadow-2xl shadow-red-600/50 animate-bounce mb-4">
            <i class="fa-solid fa-pizza-slice text-4xl"></i>
        </div>
        <h1 class="font-black text-2xl tracking-wider text-white">CRUST PIZZA</h1>
        <p class="text-xs text-yellow-400 font-semibold mt-1 tracking-widest uppercase">Astore & Eidgah Super App</p>
        <div class="w-32 h-1 bg-slate-800 rounded-full mt-6 overflow-hidden">
            <div id="loader-bar" class="w-full h-full bg-red-600 origin-left animate-pulse"></div>
        </div>
    </div>

    <!-- Floating Toast Notification Box -->
    <div id="toast-container" class="space-y-2"></div>

    <!-- Admin Broadcast Banner -->
    <div id="broadcast-banner" class="bg-yellow-500 text-slate-950 font-bold text-xs py-1.5 px-4 text-center overflow-hidden whitespace-nowrap hidden">
        <marquee scrollamount="4"><span id="broadcast-text">🔥 Flash Deal: Free Delivery on orders above Rs. 2000! Use code CRUST10</span></marquee>
    </div>

    <!-- Top App Header -->
    <header class="glass sticky top-0 z-40 px-4 py-3 flex justify-between items-center border-b border-slate-800">
        <div class="flex items-center space-x-2.5 cursor-pointer" onclick="switchTab('home')">
            <div class="bg-red-600 p-2 rounded-xl text-white shadow-lg shadow-red-600/30 animate-pulse-slow">
                <i class="fa-solid fa-pizza-slice text-xl"></i>
            </div>
            <div>
                <h1 class="font-black text-base tracking-wider text-white leading-none">CRUST PIZZA</h1>
                <select id="branch-select" class="text-[10px] text-yellow-400 bg-transparent border-none font-semibold cursor-pointer focus:outline-none mt-0.5">
                    <option value="Astore Main" class="bg-slate-900">📍 Astore Main Branch</option>
                    <option value="Eidgah Branch" class="bg-slate-900">📍 Astore Eidgah Branch</option>
                </select>
            </div>
        </div>
        <div class="flex items-center space-x-2">
            <button onclick="switchTab('admin')" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-3 py-1.5 rounded-xl font-bold border border-slate-700 flex items-center space-x-1.5 transition active:scale-95">
                <i class="fa-solid fa-gauge text-yellow-400"></i>
                <span>Admin</span>
            </button>
        </div>
    </header>

    <!-- MAIN APP CONTAINER -->
    <main class="p-4 space-y-4">

        <!-- SCREEN 1: HOME / MENU -->
        <div id="screen-home" class="app-screen active space-y-4">
            <!-- Promo Banner & Spin Game Trigger -->
            <div class="bg-gradient-to-r from-red-600 to-orange-600 text-white p-4 rounded-2xl shadow-xl relative overflow-hidden">
                <div class="relative z-10">
                    <span class="bg-black/30 text-yellow-300 text-[10px] font-bold px-2 py-0.5 rounded-full uppercase tracking-wider">Special Offer</span>
                    <h2 class="text-xl font-black mt-1">Get 10% OFF Today!</h2>
                    <p class="text-xs text-slate-100 mt-0.5">Use promo code <span class="font-bold underline text-yellow-300">CRUST10</span> at checkout.</p>
                    <button onclick="openSpinWheel()" class="mt-3 bg-yellow-400 hover:bg-yellow-300 text-slate-950 text-xs font-black px-3 py-1.5 rounded-xl shadow transition active:scale-95 flex items-center space-x-1.5 w-max">
                        <i class="fa-solid fa-gift"></i><span>Spin & Win Discount</span>
                    </button>
                </div>
                <i class="fa-solid fa-fire text-white/10 text-8xl absolute -right-4 -bottom-6"></i>
            </div>

            <!-- Quick Service Buttons (Table Booking & Refer) -->
            <div class="grid grid-cols-2 gap-2.5">
                <button onclick="switchTab('table')" class="glass p-2.5 rounded-xl border border-slate-800 text-left hover:border-yellow-500 transition flex items-center space-x-2.5">
                    <div class="bg-yellow-500/20 p-2 rounded-lg text-yellow-400"><i class="fa-solid fa-chair text-sm"></i></div>
                    <div>
                        <div class="text-xs font-bold text-white">Book Table</div>
                        <div class="text-[9px] text-slate-400">Reserve dine-in</div>
                    </div>
                </button>
                <button onclick="switchTab('refer')" class="glass p-2.5 rounded-xl border border-slate-800 text-left hover:border-green-500 transition flex items-center space-x-2.5">
                    <div class="bg-green-500/20 p-2 rounded-lg text-green-400"><i class="fa-solid fa-share-nodes text-sm"></i></div>
                    <div>
                        <div class="text-xs font-bold text-white">Refer & Earn</div>
                        <div class="text-[9px] text-slate-400">Get reward points</div>
                    </div>
                </button>
            </div>

            <!-- Categories Filter -->
            <div class="flex space-x-3 overflow-x-auto pb-1 scrollbar-none">
                <button onclick="filterMenu('all')" class="bg-red-600 text-white text-xs px-4 py-2 rounded-xl font-bold shadow-md shadow-red-600/30 whitespace-nowrap transition active:scale-95">🔥 All Pizzas</button>
                <button onclick="filterMenu('deals')" class="glass text-slate-300 text-xs px-4 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800 hover:text-white transition">🎉 Combo Deals</button>
                <button onclick="filterMenu('burgers')" class="glass text-slate-300 text-xs px-4 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800 hover:text-white transition">🍔 Burgers & FastFood</button>
            </div>

            <!-- Dynamic Product List -->
            <div class="space-y-3" id="product-list-container">
                <!-- Item 1 -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition relative">
                    <button onclick="toggleWishlist('Chicken Fajita Special', this)" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 text-sm z-10 bg-slate-900/60 p-1.5 rounded-full"><i class="fa-regular fa-heart"></i></button>
                    <img src="https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow">
                    <div class="flex-1">
                        <div class="flex justify-between items-start pr-6">
                            <h4 class="font-bold text-white text-sm">Chicken Fajita Special</h4>
                            <span class="text-[10px] bg-green-500/20 text-green-400 px-1.5 py-0.5 rounded font-bold">Hot</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Spicy chicken, onions, capsicum, extra mozzarella</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none">
                            <option value="Small" data-price="1200">Small (10") - Rs. 1200</option>
                            <option value="Medium" data-price="1700" selected>Medium (13") - Rs. 1700</option>
                            <option value="Large" data-price="2200">Large (16") - Rs. 2200</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. 1700</span>
                            <button onclick="addToCart('Chicken Fajita Special', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 transition active:scale-95 flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Item 2 -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition relative">
                    <button onclick="toggleWishlist('Super Supreme Feast', this)" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 text-sm z-10 bg-slate-900/60 p-1.5 rounded-full"><i class="fa-regular fa-heart"></i></button>
                    <img src="https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow">
                    <div class="flex-1">
                        <div class="flex justify-between items-start pr-6">
                            <h4 class="font-bold text-white text-sm">Super Supreme Feast</h4>
                            <span class="text-[10px] bg-yellow-500/20 text-yellow-400 px-1.5 py-0.5 rounded font-bold">Best Seller</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Smoked beef, pepperoni, mushrooms, black olives</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none">
                            <option value="Small" data-price="1350">Small (10") - Rs. 1350</option>
                            <option value="Medium" data-price="1850" selected>Medium (13") - Rs. 1850</option>
                            <option value="Large" data-price="2400">Large (16") - Rs. 2400</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. 1850</span>
                            <button onclick="addToCart('Super Supreme Feast', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 transition active:scale-95 flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- SCREEN 2: PIZZA BUILDER -->
        <div id="screen-builder" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <div class="flex items-center space-x-2 mb-3">
                    <div class="bg-orange-600/20 p-2 rounded-xl text-orange-400">
                        <i class="fa-solid fa-wand-magic-sparkles text-lg"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-white text-sm">Custom Pizza Maker</h3>
                        <p class="text-[11px] text-slate-400">Build your dream pizza flavor</p>
                    </div>
                </div>
                
                <div class="space-y-3">
                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Select Crust Base:</label>
                        <select id="build-dough" class="w-full border border-slate-700 bg-slate-800 text-slate-200 rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Regular Crust (Rs. 1000)">Regular Crust - Rs. 1000</option>
                            <option value="Stuffed Crust (Rs. 1300)">Stuffed Crust - Rs. 1300</option>
                            <option value="Thin Crispy (Rs. 1100)">Thin Crispy - Rs. 1100</option>
                        </select>
                    </div>
                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Extra Toppings:</label>
                        <select id="build-topping" class="w-full border border-slate-700 bg-slate-800 text-slate-200 rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Extra Cheese (+Rs. 200)">Extra Cheese - Rs. 200</option>
                            <option value="Mushroom & Olives (+Rs. 250)">Mushroom & Olives - Rs. 250</option>
                            <option value="Extra Chicken (+Rs. 300)">Extra Chicken - Rs. 300</option>
                        </select>
                    </div>
                    <button onclick="addCustomPizza()" class="w-full bg-red-600 hover:bg-red-500 text-white font-bold py-3 rounded-xl text-xs shadow-lg shadow-red-600/30 transition active:scale-95 flex items-center justify-center space-x-2 mt-2">
                        <i class="fa-solid fa-cart-plus"></i><span>Add Custom Pizza to Cart</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 3: CART & CHECKOUT -->
        <div id="screen-cart" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <h3 class="font-bold text-white text-sm mb-3 flex items-center justify-between">
                    <span>🛒 Your Order Cart</span>
                    <span id="item-count-badge" class="text-[10px] bg-red-600 text-white px-2 py-0.5 rounded-full font-bold">0 Items</span>
                </h3>
                <div id="cart-items" class="divide-y divide-slate-800 text-xs mb-4 max-h-48 overflow-y-auto">
                    <p class="text-slate-500 text-center py-6">Your cart is empty.</p>
                </div>

                <!-- Coupon Box -->
                <div class="flex space-x-2 mb-3">
                    <input type="text" id="coupon-code" placeholder="Promo (CRUST10)" class="border border-slate-700 bg-slate-800 text-white rounded-xl px-3 py-2 text-xs w-full uppercase focus:outline-none">
                    <button onclick="applyCoupon()" class="bg-slate-700 hover:bg-slate-600 text-white px-4 py-2 rounded-xl text-xs font-bold transition active:scale-95">Apply</button>
                </div>

                <!-- Totals -->
                <div class="border-t border-slate-800 pt-3 text-xs space-y-1.5 mb-4">
                    <div class="flex justify-between text-slate-400"><span>Subtotal:</span><span id="subtotal" class="text-white font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-slate-400"><span>Discount:</span><span id="discount" class="text-green-400 font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-sm font-black text-white border-t border-slate-800 pt-2"><span>Grand Total:</span><span id="grand-total" class="text-red-500">Rs. 0</span></div>
                </div>

                <!-- Delivery Details Form -->
                <div class="space-y-2.5">
                    <h4 class="font-bold text-[11px] text-slate-400 uppercase tracking-wider">Delivery Details</h4>
                    <input type="text" id="cust-name" placeholder="Full Name" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs focus:outline-none focus:border-red-600">
                    <input type="text" id="cust-phone" placeholder="Active Phone Number" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs focus:outline-none focus:border-red-600">
                    <textarea id="cust-address" placeholder="Delivery Address (Astore / Eidgah)" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs focus:outline-none focus:border-red-600" rows="2"></textarea>
                    
                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Payment Mode:</label>
                        <select id="payment-method" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Cash on Delivery">Cash on Delivery (COD)</option>
                            <option value="Online Payment (Easypaisa/JazzCash)">Online Payment (Easypaisa / JazzCash)</option>
                        </select>
                    </div>

                    <button onclick="checkoutWhatsApp()" class="w-full bg-green-600 hover:bg-green-500 text-white font-bold py-3.5 rounded-xl shadow-xl shadow-green-600/30 flex items-center justify-center space-x-2 mt-2 text-xs transition active:scale-95">
                        <i class="fa-brands fa-whatsapp text-lg"></i>
                        <span>Confirm Order via WhatsApp</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 4: LIVE TRACKING & HISTORY -->
        <div id="screen-tracking" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <h3 class="font-bold text-white text-sm mb-1 flex items-center space-x-2">
                    <i class="fa-solid fa-location-crosshairs text-red-500"></i>
                    <span>Live Order Tracking</span>
                </h3>
                <p class="text-[11px] text-slate-400 mb-3">Track cooking & delivery progress.</p>
                
                <div class="flex space-x-2 mb-4">
                    <input type="text" id="track-id" placeholder="Enter Order ID (e.g. #102)" class="border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs w-full focus:outline-none">
                    <button onclick="trackOrder()" class="bg-red-600 hover:bg-red-500 text-white px-4 py-2.5 rounded-xl text-xs font-bold transition active:scale-95">Track</button>
                </div>

                <div id="tracking-result" class="hidden space-y-3 bg-slate-900/80 p-3 rounded-xl border border-slate-800 text-xs mb-4">
                    <div class="flex items-center space-x-3 text-green-400"><i class="fa-solid fa-circle-check text-base"></i><span class="font-bold">Order Received & Confirmed</span></div>
                    <div class="flex items-center space-x-3 text-yellow-400"><i class="fa-solid fa-fire-burner text-base animate-spin"></i><span class="font-bold">Preparing in Kitchen (ETA: <span id="countdown-timer">35:00</span>)</span></div>
                    <div class="flex items-center space-x-3 text-slate-500"><i class="fa-solid fa-motorcycle text-base"></i><span>Out for Delivery</span></div>
                </div>

                <!-- Past Order History -->
                <h4 class="font-bold text-xs text-slate-300 mt-4 mb-2 border-t border-slate-800 pt-3">📦 Past Orders History</h4>
                <div id="order-history-list" class="space-y-2 text-xs">
                    <p class="text-slate-500 text-center py-3">No past orders found.</p>
                </div>
            </div>
        </div>

        <!-- SCREEN 5: TABLE RESERVATION -->
        <div id="screen-table" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <h3 class="font-bold text-white text-sm mb-1 flex items-center space-x-2">
                    <i class="fa-solid fa-chair text-yellow-400"></i>
                    <span>Table Reservation</span>
                </h3>
                <p class="text-[11px] text-slate-400 mb-3">Book your dining table at Crust Pizza.</p>
                <div class="space-y-2.5 text-xs">
                    <input type="text" id="res-name" placeholder="Your Name" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 focus:outline-none">
                    <input type="date" id="res-date" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 focus:outline-none">
                    <input type="time" id="res-time" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 focus:outline-none">
                    <select id="res-guests" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 focus:outline-none">
                        <option value="2 Persons">2 Persons Table</option>
                        <option value="4 Persons Family Table">4 Persons Family Table</option>
                        <option value="6+ VIP Gathering">6+ VIP Gathering</option>
                    </select>
                    <button onclick="bookTable()" class="w-full bg-yellow-500 hover:bg-yellow-400 text-slate-950 font-bold py-3 rounded-xl shadow-lg transition active:scale-95 mt-2">Confirm Table Booking</button>
                </div>
            </div>
        </div>

        <!-- SCREEN 6: REFER & EARN -->
        <div id="screen-refer" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800 text-center space-y-3">
                <div class="bg-green-500/20 text-green-400 w-12 h-12 rounded-2xl flex items-center justify-center mx-auto text-xl"><i class="fa-solid fa-gift"></i></div>
                <h3 class="font-bold text-white text-sm">Refer Friends & Earn Points!</h3>
                <p class="text-xs text-slate-400">Share your referral link with friends on WhatsApp. When they order, you both get Rs. 200 discount!</p>
                <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-800 text-xs font-mono text-yellow-400 font-bold">CRUST-REF-9821</div>
                <button onclick="shareReferral()" class="w-full bg-green-600 hover:bg-green-500 text-white font-bold py-3 rounded-xl text-xs shadow-lg transition active:scale-95 flex items-center justify-center space-x-2">
                    <i class="fa-brands fa-whatsapp text-base"></i><span>Share via WhatsApp</span>
                </button>
            </div>
        </div>

        <!-- SCREEN 7: ADMIN DASHBOARD -->
        <div id="screen-admin" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800 space-y-4">
                <div class="flex justify-between items-center border-b border-slate-800 pb-3">
                    <div>
                        <h3 class="font-bold text-white text-sm">📊 Admin Back-End Panel</h3>
                        <span class="text-[10px] text-green-400 font-bold">● System Active</span>
                    </div>
                    <button onclick="simulateAlarm()" class="bg-red-600 text-white text-[10px] px-3 py-1.5 rounded-xl font-bold animate-pulse">🔔 Test Alarm</button>
                </div>

                <!-- Sales Analytics Cards -->
                <div class="grid grid-cols-2 gap-3">
                    <div class="bg-slate-900 p-3 rounded-xl border border-slate-800 text-center">
                        <span class="text-[10px] text-slate-400">Today Orders</span>
                        <h4 id="admin-total-orders" class="text-xl font-black text-yellow-400 mt-0.5">32</h4>
                    </div>
                    <div class="bg-slate-900 p-3 rounded-xl border border-slate-800 text-center">
                        <span class="text-[10px] text-slate-400">Weekly Revenue</span>
                        <h4 class="text-xl font-black text-green-400 mt-0.5">Rs. 184K</h4>
                    </div>
                </div>

                <!-- Live Orders Management -->
                <div class="bg-slate-900 p-3 rounded-xl border border-slate-800 space-y-2">
                    <h4 class="font-bold text-xs text-yellow-300">Live Incoming Orders</h4>
                    <div id="admin-orders-list" class="space-y-2 text-xs max-h-40 overflow-y-auto">
                        <div class="bg-slate-800 p-2 rounded-lg flex justify-between items-center">
                            <div><strong class="text-white">#101 - Ali Khan</strong><br><span class="text-[10px] text-slate-400">Medium Fajita (Rs. 1700)</span></div>
                            <span class="bg-yellow-500/20 text-yellow-400 text-[10px] px-2 py-0.5 rounded font-bold">Preparing</span>
                        </div>
                    </div>
                </div>

                <!-- Add New Product Form -->
                <div class="bg-slate-900 p-3.5 rounded-xl border border-slate-800 space-y-2.5">
                    <h4 class="font-bold text-xs text-yellow-300">Add New Pizza / Deal</h4>
                    <input type="text" id="admin-p-name" placeholder="Item Name (e.g. Stuffed Crust)" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2 text-xs focus:outline-none">
                    <input type="number" id="admin-p-price" placeholder="Base Price (Rs)" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2 text-xs focus:outline-none">
                    <button onclick="adminAddNewProduct()" class="w-full bg-yellow-400 hover:bg-yellow-300 text-slate-950 font-bold py-2 rounded-xl text-xs transition active:scale-95">Save & Publish Product</button>
                </div>

                <!-- Broadcast Announcement Setup -->
                <div class="bg-slate-900 p-3.5 rounded-xl border border-slate-800 space-y-2.5">
                    <h4 class="font-bold text-xs text-yellow-300">Send App Broadcast Announcement</h4>
                    <input type="text" id="admin-broadcast-msg" placeholder="Announcement text..." class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2 text-xs focus:outline-none">
                    <button onclick="updateBroadcast()" class="w-full bg-red-600 hover:bg-red-500 text-white font-bold py-2 rounded-xl text-xs transition active:scale-95">Broadcast Banner</button>
                </div>
            </div>
        </div>

    </main>

    <!-- Spin Wheel Modal Popup -->
    <div id="spin-modal" class="fixed inset-0 bg-black/80 z-50 flex items-center justify-center hidden p-4">
        <div class="glass p-6 rounded-3xl border border-slate-700 text-center max-w-xs w-full space-y-4">
            <div class="bg-red-600 w-14 h-14 rounded-2xl flex items-center justify-center mx-auto text-2xl text-white shadow-xl animate-spin"><i class="fa-solid fa-dharmachakra"></i></div>
            <h3 class="font-black text-base text-white">Spin the Wheel!</h3>
            <p class="text-xs text-slate-300">Tap below to spin and win an instant discount coupon for your order.</p>
            <div id="spin-result" class="text-yellow-400 font-bold text-sm min-h-[24px]"></div>
            <button onclick="spinWheelAction()" id="spin-btn" class="w-full bg-yellow-400 hover:bg-yellow-300 text-slate-950 font-bold py-2.5 rounded-xl text-xs shadow-lg transition active:scale-95">SPIN NOW</button>
            <button onclick="document.getElementById('spin-modal').classList.add('hidden')" class="text-slate-400 text-xs hover:text-white underline">Close</button>
        </div>
    </div>

    <!-- Perfectly Fixed Horizontal Bottom Navigation Bar -->
    <nav class="glass bottom-nav border-t border-slate-800 px-4 py-2.5 shadow-2xl">
        <button onclick="switchTab('home')" id="nav-home" class="text-red-500 flex flex-col items-center text-[10px] font-bold space-y-0.5 transition active:scale-90 flex-1">
            <i class="fa-solid fa-house text-base"></i><span>Menu</span>
        </button>
        <button onclick="switchTab('builder')" id="nav-builder" class="text-slate-400 hover:text-slate-200 flex flex-col items-center text-[10px] font-medium space-y-0.5 transition active:scale-90 flex-1">
            <i class="fa-solid fa-wand-magic-sparkles text-base"></i><span>Builder</span>
        </button>
        <button onclick="switchTab('cart')" id="nav-cart" class="text-slate-400 hover:text-slate-200 flex flex-col items-center text-[10px] font-medium space-y-0.5 relative transition active:scale-90 flex-1">
            <i class="fa-solid fa-cart-shopping text-base"></i><span>Cart</span>
            <span id="nav-badge" class="absolute -top-1 right-5 bg-red-600 text-white text-[9px] px-1.5 py-0.2 rounded-full font-bold">0</span>
        </button>
        <button onclick="switchTab('tracking')" id="nav-tracking" class="text-slate-400 hover:text-slate-200 flex flex-col items-center text-[10px] font-medium space-y-0.5 transition active:scale-90 flex-1">
            <i class="fa-solid fa-location-crosshairs text-base"></i><span>Track</span>
        </button>
    </nav>

    <!-- JavaScript App Logic -->
    <script>
        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                splash.style.opacity = '0';
                setTimeout(() => splash.remove(), 500);
            }, 1200);
            loadOrderHistory();
        });

        let cart = [];
        let discountRate = 0;
        let orderHistory = JSON.parse(localStorage.getItem('crust_orders') || '[]');
        let liveAdminOrders = [
            { id: 101, name: 'Ali Khan', item: 'Medium Fajita', price: 1700, status: 'Preparing' }
        ];

        // Toast Notification System
        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            const bg = type === 'success' ? 'bg-green-600' : 'bg-red-600';
            toast.className = `${bg} text-white text-xs font-bold px-4 py-3 rounded-2xl shadow-xl flex items-center justify-between pointer-events-auto transition transform translate-y-2 opacity-0`;
            toast.innerHTML = `<span>${message}</span><i class="fa-solid fa-circle-check text-sm ml-2"></i>`;
            container.appendChild(toast);
            setTimeout(() => { toast.style.transform = 'translateY(0)'; toast.style.opacity = '1'; }, 50);
            setTimeout(() => {
                toast.style.opacity = '0';
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        function switchTab(tabId) {
            document.querySelectorAll('.app-screen').forEach(s => s.classList.remove('active'));
            document.getElementById(`screen-${tabId}`).classList.add('active');

            ['home', 'builder', 'cart', 'tracking'].forEach(t => {
                const btn = document.getElementById(`nav-${t}`);
                if(btn) {
                    btn.className = (t === tabId) ? "text-red-500 flex flex-col items-center text-[10px] font-bold space-y-0.5 transition active:scale-90 flex-1" : "text-slate-400 hover:text-slate-200 flex flex-col items-center text-[10px] font-medium space-y-0.5 transition active:scale-90 flex-1";
                }
            });
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

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
            showToast(`Added ${name} (${size}) to Cart!`);
            
            btn.innerHTML = `<i class="fa-solid fa-check text-[10px]"></i><span>Added</span>`;
            setTimeout(() => {
                btn.innerHTML = `<i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>`;
            }, 1000);
        }

        function addCustomPizza() {
            const dough = document.getElementById('build-dough').value;
            const topping = document.getElementById('build-topping').value;
            let price = dough.includes('1300') ? 1300 : (dough.includes('1100') ? 1100 : 1000);
            if(topping.includes('250')) price += 250;
            if(topping.includes('300')) price += 300;
            if(topping.includes('200')) price += 200;

            cart.push({ name: `Custom Pizza`, size: `${dough.split(' ')[0]} + ${topping.split(' ')[0]}`, price });
            updateCartUI();
            showToast('Custom Pizza added to cart!');
            switchTab('cart');
        }

        function updateCartUI() {
            const container = document.getElementById('cart-items');
            document.getElementById('nav-badge').innerText = cart.length;
            document.getElementById('item-count-badge').innerText = `${cart.length} Items`;

            if(cart.length === 0) {
                container.innerHTML = '<p class="text-slate-500 text-center py-6">Your cart is empty.</p>';
                document.getElementById('subtotal').innerText = 'Rs. 0';
                document.getElementById('discount').innerText = 'Rs. 0';
                document.getElementById('grand-total').innerText = 'Rs. 0';
                return;
            }

            container.innerHTML = '';
            let subtotal = 0;
            cart.forEach((item, index) => {
                subtotal += item.price;
                container.innerHTML += `
                    <div class="py-2 flex justify-between items-center">
                        <div><b class="text-white">${item.name}</b><br><span class="text-[10px] text-slate-400">${item.size} - Rs. ${item.price}</span></div>
                        <button onclick="cart.splice(${index},1);updateCartUI()" class="text-red-400 hover:text-red-300 font-bold text-xs p-1"><i class="fa-solid fa-trash"></i></button>
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
            const code = document.getElementById('coupon-code').value.trim().toUpperCase();
            if(code === 'CRUST10') {
                discountRate = 0.10;
                updateCartUI();
                showToast('10% Discount Applied Successfully!');
            } else {
                showToast('Invalid Coupon Code!', 'error');
            }
        }

        function checkoutWhatsApp() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value;
            const branch = document.getElementById('branch-select').value;
            const payment = document.getElementById('payment-method').value;

            if(!name || !phone || !address || cart.length === 0) {
                showToast('Please fill delivery details & add items!', 'error');
                return;
            }

            let sub = 0;
            cart.forEach(i => sub += i.price);
            let final = sub - (sub * discountRate);

            // Save to order history
            const newOrder = { id: Math.floor(100 + Math.random() * 900), name, items: [...cart], total: final, date: new Date().toLocaleDateString() };
            orderHistory.unshift(newOrder);
            localStorage.setItem('crust_orders', JSON.stringify(orderHistory));
            liveAdminOrders.unshift({ id: newOrder.id, name, item: `${cart.length} Items`, price: final, status: 'Pending' });

            loadOrderHistory();
            updateAdminDashboard();

            let msg = `*New Order - Crust Pizza (${branch})*%0A%0A*Name:* ${name}%0A*Phone:* ${phone}%0A*Address:* ${address}%0A*Payment:* ${payment}%0A%0A*Items:*%0A`;
            cart.forEach(i => { msg += `- ${i.name} (${i.size}): Rs. ${i.price}%0A`; });
            msg += `%0A*Total Bill:* Rs. ${final} (ETA: 35 mins)%0A%0A_Powered by Prime Solutions_`;

            window.open(`https://wa.me/923001234567?text=${msg}`, '_blank');
            cart = [];
            updateCartUI();
            switchTab('tracking');
            document.getElementById('track-id').value = `#${newOrder.id}`;
            trackOrder();
        }

        function trackOrder() {
            const id = document.getElementById('track-id').value.trim();
            const res = document.getElementById('tracking-result');
            if(id) {
                res.classList.remove('hidden');
                startTimer(35 * 60);
            } else {
                showToast('Please enter a valid order ID', 'error');
            }
        }

        function startTimer(duration) {
            let timer = duration, minutes, seconds;
            const display = document.getElementById('countdown-timer');
            setInterval(function () {
                minutes = parseInt(timer / 60, 10);
                seconds = parseInt(timer % 60, 10);
                minutes = minutes < 10 ? "0" + minutes : minutes;
                seconds = seconds < 10 ? "0" + seconds : seconds;
                display.textContent = minutes + ":" + seconds;
                if (--timer < 0) { timer = 0; }
            }, 1000);
        }

        function loadOrderHistory() {
            const container = document.getElementById('order-history-list');
            if(orderHistory.length === 0) return;
            container.innerHTML = '';
            orderHistory.forEach((ord, idx) => {
                container.innerHTML += `
                    <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-800 flex justify-between items-center">
                        <div><strong class="text-white">#${ord.id} - ${ord.date}</strong><br><span class="text-[10px] text-slate-400">Total: Rs. ${ord.total}</span></div>
                        <button onclick="repeatOrder(${idx})" class="bg-red-600 text-white text-[10px] px-2.5 py-1 rounded-lg font-bold">Repeat</button>
                    </div>
                `;
            });
        }

        function repeatOrder(idx) {
            const ord = orderHistory[idx];
            cart = [...ord.items];
            updateCartUI();
            switchTab('cart');
            showToast('Order loaded into cart!');
        }

        function bookTable() {
            const name = document.getElementById('res-name').value;
            const date = document.getElementById('res-date').value;
            const time = document.getElementById('res-time').value;
            const guests = document.getElementById('res-guests').value;
            if(!name || !date || !time) {
                showToast('Please fill all table details!', 'error');
                return;
            }
            showToast(`Table booked successfully for ${guests} on ${date}!`);
            switchTab('home');
        }

        function shareReferral() {
            window.open(`https://wa.me/?text=Hey! Join Crust Pizza using my reference code CRUST-REF-9821 and get Rs. 200 off your first pizza!`, '_blank');
        }

        function openSpinWheel() {
            document.getElementById('spin-modal').classList.remove('hidden');
        }

        function spinWheelAction() {
            const res = document.getElementById('spin-result');
            const btn = document.getElementById('spin-btn');
            btn.disabled = true;
            res.innerHTML = "Spinning...";
            setTimeout(() => {
                res.innerHTML = "🎉 Congratulations! You won 15% OFF!";
                discountRate = 0.15;
                updateCartUI();
                showToast('15% Discount unlocked!');
            }, 1500);
        }

        function toggleWishlist(itemName, btn) {
            const icon = btn.querySelector('i');
            if(icon.classList.contains('fa-regular')) {
                icon.classList.remove('fa-regular');
                icon.classList.add('fa-solid', 'text-red-500');
                showToast(`Added ${itemName} to Wishlist!`);
            } else {
                icon.classList.remove('fa-solid', 'text-red-500');
                icon.classList.add('fa-regular');
                showToast(`Removed from Wishlist`);
            }
        }

        function updateAdminDashboard() {
            document.getElementById('admin-total-orders').innerText = liveAdminOrders.length + 31;
            const list = document.getElementById('admin-orders-list');
            list.innerHTML = '';
            liveAdminOrders.forEach(o => {
                list.innerHTML += `
                    <div class="bg-slate-800 p-2 rounded-lg flex justify-between items-center">
                        <div><strong class="text-white">#${o.id} - ${o.name}</strong><br><span class="text-[10px] text-slate-400">${o.item} (Rs. ${o.price})</span></div>
                        <span class="bg-yellow-500/20 text-yellow-400 text-[10px] px-2 py-0.5 rounded font-bold">${o.status}</span>
                    </div>
                `;
            });
        }

        function adminAddNewProduct() {
            const name = document.getElementById('admin-p-name').value;
            const price = document.getElementById('admin-p-price').value;
            if(!name || !price) {
                showToast('Enter item name and price', 'error');
                return;
            }
            const container = document.getElementById('product-list-container');
            container.innerHTML += `
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition">
                    <img src="https://images.unsplash.com/photo-1541745537411-b8046dc6d66c?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow">
                    <div class="flex-1">
                        <div class="flex justify-between items-start"><h4 class="font-bold text-white text-sm">${name}</h4><span class="text-[10px] bg-red-500/20 text-red-400 px-1.5 py-0.5 rounded font-bold">New</span></div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Freshly added special menu item</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none">
                            <option value="Regular" data-price="${price}">Regular - Rs. ${price}</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. ${price}</span>
                            <button onclick="addToCart('${name}', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 transition active:scale-95 flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>
            `;
            showToast('New product added to live menu!');
        }

        function updateBroadcast() {
            const msg = document.getElementById('admin-broadcast-msg').value;
            if(!msg) return;
            document.getElementById('broadcast-text').innerText = msg;
            document.getElementById('broadcast-banner').classList.remove('hidden');
            showToast('Broadcast banner updated live!');
        }

        function simulateAlarm() {
            const audio = new Audio('https://www.soundjay.com/buttons/sounds/beep-07.mp3');
            audio.play().catch(e => console.log('Audio restricted'));
            showToast('🚨 Kitchen Alert: New Order Received!');
        }

        function filterMenu(category) {
            showToast(`Filtered category: ${category.toUpperCase()}`);
        }
    </script>
</body>
</html>
