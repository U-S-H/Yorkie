<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crust Pizza - Ultimate Super App</title>
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
        #toast-container {
            position: fixed; top: 70px; left: 50%; transform: translateX(-50%);
            z-index: 99999; width: 90%; max-width: 400px; pointer-events: none;
        }
        @media print {
            body * { visibility: hidden; }
            #printable-receipt, #printable-receipt * { visibility: visible; }
            #printable-receipt { position: absolute; left: 0; top: 0; width: 100%; color: black; background: white; padding: 20px; }
        }
    </style>
</head>
<body class="font-sans antialiased max-w-md mx-auto min-h-screen relative shadow-2xl overflow-x-hidden border-x border-slate-800">

    <!-- Splash Screen -->
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

    <!-- Toast Notifications -->
    <div id="toast-container" class="space-y-2"></div>

    <!-- Printable Receipt Container (Hidden by default) -->
    <div id="printable-receipt" class="hidden text-black bg-white p-4 font-mono text-xs"></div>

    <!-- Header -->
    <header class="glass sticky top-0 z-40 px-4 py-3 flex justify-between items-center border-b border-slate-800">
        <div class="flex items-center space-x-2.5 cursor-pointer" onclick="switchTab('home')">
            <div class="bg-red-600 p-2 rounded-xl text-white shadow-lg shadow-red-600/30 animate-pulse-slow">
                <i class="fa-solid fa-pizza-slice text-xl"></i>
            </div>
            <div>
                <h1 class="font-black text-base tracking-wider text-white leading-none">CRUST PIZZA</h1>
                <select id="branch-select" onchange="updateDeliveryCharges()" class="text-[10px] text-yellow-400 bg-transparent border-none font-semibold cursor-pointer focus:outline-none mt-0.5">
                    <option value="Astore Main" class="bg-slate-900">📍 Astore Main Branch</option>
                    <option value="Eidgah Branch" class="bg-slate-900">📍 Astore Eidgah Branch (+Rs. 100)</option>
                </select>
            </div>
        </div>
        <div class="flex items-center space-x-2">
            <button onclick="switchTab('admin')" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-3 py-1.5 rounded-xl font-bold border border-slate-700 flex items-center space-x-1.5 transition active:scale-95">
                <i class="fa-solid fa-gauge text-yellow-400"></i>
                <span>Admin Panel</span>
            </button>
        </div>
    </header>

    <!-- MAIN CONTAINER -->
    <main class="p-4 space-y-4">

        <!-- SCREEN 1: HOME -->
        <div id="screen-home" class="app-screen active space-y-4">
            
            <!-- AI Recommendation Box -->
            <div class="bg-slate-900/90 border border-yellow-500/30 p-3.5 rounded-2xl relative overflow-hidden">
                <div class="flex items-center justify-between mb-2">
                    <div class="flex items-center space-x-2">
                        <i class="fa-solid fa-robot text-yellow-400 text-lg"></i>
                        <h3 class="text-xs font-bold text-white">AI Pizza Recommendation Assistant</h3>
                    </div>
                    <span class="text-[9px] bg-yellow-400/20 text-yellow-300 px-2 py-0.5 rounded-full font-bold">SMART</span>
                </div>
                <div class="flex space-x-2">
                    <select id="ai-budget" class="bg-slate-800 text-slate-200 border border-slate-700 rounded-xl text-[11px] p-2 flex-1 focus:outline-none">
                        <option value="1500">Budget: Under Rs. 1500</option>
                        <option value="2000" selected>Budget: Under Rs. 2000</option>
                        <option value="2500">Budget: Unlimited</option>
                    </select>
                    <button onclick="getAIRecommendation()" class="bg-yellow-400 text-slate-950 font-black px-3 rounded-xl text-xs flex items-center space-x-1 hover:bg-yellow-300 transition">
                        <i class="fa-solid fa-wand-magic-sparkles"></i>
                        <span>Suggest</span>
                    </button>
                </div>
                <div id="ai-result" class="hidden mt-2 pt-2 border-t border-slate-800 text-xs text-yellow-300 font-semibold flex justify-between items-center">
                    <span id="ai-text"></span>
                    <button id="ai-add-btn" class="bg-red-600 text-white px-2 py-1 rounded-lg text-[10px] font-bold">Add Suggested</button>
                </div>
            </div>

            <!-- Promo & Wheel -->
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

            <!-- Quick Action Buttons -->
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

            <!-- Categories -->
            <div class="flex space-x-3 overflow-x-auto pb-1 scrollbar-none">
                <button onclick="filterMenu('all')" class="bg-red-600 text-white text-xs px-4 py-2 rounded-xl font-bold shadow-md shadow-red-600/30 whitespace-nowrap transition active:scale-95">🔥 All Pizzas</button>
                <button onclick="filterMenu('deals')" class="glass text-slate-300 text-xs px-4 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800 hover:text-white transition">🎉 Combo Deals</button>
                <button onclick="filterMenu('burgers')" class="glass text-slate-300 text-xs px-4 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800 hover:text-white transition">🍔 Burgers & FastFood</button>
            </div>

            <!-- Products Container -->
            <div class="space-y-3" id="product-list-container">
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

        <!-- SCREEN 2: HALF-&-HALF BUILDER -->
        <div id="screen-builder" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <div class="flex items-center space-x-2 mb-3">
                    <div class="bg-orange-600/20 p-2 rounded-xl text-orange-400">
                        <i class="fa-solid fa-wand-magic-sparkles text-lg"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-white text-sm">Half-&-Half Custom Pizza</h3>
                        <p class="text-[11px] text-slate-400">Mix 2 different flavors in 1 pizza!</p>
                    </div>
                </div>
                
                <div class="space-y-3">
                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Left Half Flavor:</label>
                        <select id="half-left" class="w-full border border-slate-700 bg-slate-800 text-slate-200 rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Chicken Fajita">Chicken Fajita</option>
                            <option value="Super Supreme">Super Supreme</option>
                            <option value="BBQ Tikka">BBQ Tikka</option>
                        </select>
                    </div>

                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Right Half Flavor:</label>
                        <select id="half-right" class="w-full border border-slate-700 bg-slate-800 text-slate-200 rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Veggie Supreme">Veggie Supreme</option>
                            <option value="Cheese Lover">Cheese Lover</option>
                            <option value="Pepperoni Passion">Pepperoni Passion</option>
                        </select>
                    </div>

                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Crust Type:</label>
                        <select id="build-dough" class="w-full border border-slate-700 bg-slate-800 text-slate-200 rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Regular Crust (Rs. 1800)">Medium Regular Crust - Rs. 1800</option>
                            <option value="Cheese Stuffed Crust (Rs. 2100)">Medium Stuffed Crust - Rs. 2100</option>
                        </select>
                    </div>

                    <button onclick="addHalfPizza()" class="w-full bg-red-600 hover:bg-red-500 text-white font-bold py-3 rounded-xl text-xs shadow-lg shadow-red-600/30 transition active:scale-95 flex items-center justify-center space-x-2 mt-2">
                        <i class="fa-solid fa-cart-plus"></i><span>Add Half-&-Half to Cart</span>
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
                    <div class="flex justify-between text-slate-400"><span>Delivery Fee:</span><span id="delivery-fee" class="text-yellow-400 font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-slate-400"><span>Discount:</span><span id="discount" class="text-green-400 font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-sm font-black text-white border-t border-slate-800 pt-2"><span>Grand Total:</span><span id="grand-total" class="text-red-500">Rs. 0</span></div>
                </div>

                <!-- Delivery Form -->
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

        <!-- SCREEN 4: LIVE TRACKING -->
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

                <!-- Past Orders -->
                <h4 class="font-bold text-xs text-slate-300 mt-4 mb-2 border-t border-slate-800 pt-3">📦 Past Orders History</h4>
                <div id="order-history-list" class="space-y-2 text-xs">
                    <p class="text-slate-500 text-center py-3">No past orders found.</p>
                </div>
            </div>
        </div>

        <!-- SCREEN 5: ADMIN DASHBOARD -->
        <div id="screen-admin" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800 space-y-4">
                <div class="flex justify-between items-center border-b border-slate-800 pb-3">
                    <div>
                        <h3 class="font-bold text-white text-sm">📊 Kitchen & POS Back-End</h3>
                        <span class="text-[10px] text-green-400 font-bold">● Live Orders Active</span>
                    </div>
                </div>

                <!-- Orders Table & Management -->
                <div class="bg-slate-900 p-3 rounded-xl border border-slate-800 space-y-2">
                    <h4 class="font-bold text-xs text-yellow-300">Live Orders Management</h4>
                    <div id="admin-orders-list" class="space-y-2 text-xs max-h-52 overflow-y-auto">
                        <!-- Dynamic Admin Orders -->
                    </div>
                </div>
            </div>
        </div>

        <!-- SCREEN 6 & 7 (Placeholder Table & Refer) -->
        <div id="screen-table" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800 text-center">
                <h3 class="font-bold text-white text-sm mb-2">Table Booking Active</h3>
                <p class="text-xs text-slate-400">Fill details to reserve table.</p>
            </div>
        </div>
        <div id="screen-refer" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800 text-center">
                <h3 class="font-bold text-white text-sm mb-2">Referral System</h3>
                <p class="text-xs text-slate-400">Share with friends on WhatsApp.</p>
            </div>
        </div>

    </main>

    <!-- Upsell Cross-Sell Modal -->
    <div id="upsell-modal" class="fixed inset-0 bg-black/80 z-50 flex items-center justify-center hidden p-4">
        <div class="glass p-5 rounded-3xl border border-slate-700 text-center max-w-xs w-full space-y-3">
            <div class="bg-yellow-500/20 text-yellow-400 w-12 h-12 rounded-full flex items-center justify-center mx-auto text-xl">
                <i class="fa-solid fa-bottle-water"></i>
            </div>
            <h3 class="font-black text-sm text-white">Add Drinks & Sides?</h3>
            <p class="text-xs text-slate-300">Customers who bought pizza also added 1.5L Drink & Garlic Bread!</p>
            <div class="bg-slate-900/80 p-2 rounded-xl text-left text-xs flex justify-between items-center border border-slate-800">
                <div><b>1.5L Cold Drink</b><br><span class="text-slate-400">Rs. 180</span></div>
                <button onclick="addUpsell('1.5L Cold Drink', 180)" class="bg-red-600 text-white font-bold px-3 py-1 rounded-lg text-[10px]">Add</button>
            </div>
            <button onclick="closeUpsell()" class="w-full bg-slate-800 text-slate-300 font-bold py-2 rounded-xl text-xs">No, Thanks</button>
        </div>
    </div>

    <!-- Spin Wheel Modal -->
    <div id="spin-modal" class="fixed inset-0 bg-black/80 z-50 flex items-center justify-center hidden p-4">
        <div class="glass p-6 rounded-3xl border border-slate-700 text-center max-w-xs w-full space-y-4">
            <div class="bg-red-600 w-14 h-14 rounded-2xl flex items-center justify-center mx-auto text-2xl text-white shadow-xl animate-spin"><i class="fa-solid fa-dharmachakra"></i></div>
            <h3 class="font-black text-base text-white">Spin the Wheel!</h3>
            <div id="spin-result" class="text-yellow-400 font-bold text-sm min-h-[24px]"></div>
            <button onclick="spinWheelAction()" id="spin-btn" class="w-full bg-yellow-400 text-slate-950 font-bold py-2.5 rounded-xl text-xs shadow-lg">SPIN NOW</button>
            <button onclick="document.getElementById('spin-modal').classList.add('hidden')" class="text-slate-400 text-xs hover:text-white underline">Close</button>
        </div>
    </div>

    <!-- Fixed Bottom Navigation -->
    <nav class="glass bottom-nav border-t border-slate-800 px-4 py-2.5 shadow-2xl">
        <button onclick="switchTab('home')" id="nav-home" class="text-red-500 flex flex-col items-center text-[10px] font-bold space-y-0.5 flex-1">
            <i class="fa-solid fa-house text-base"></i><span>Menu</span>
        </button>
        <button onclick="switchTab('builder')" id="nav-builder" class="text-slate-400 flex flex-col items-center text-[10px] font-medium space-y-0.5 flex-1">
            <i class="fa-solid fa-wand-magic-sparkles text-base"></i><span>Builder</span>
        </button>
        <button onclick="switchTab('cart')" id="nav-cart" class="text-slate-400 flex flex-col items-center text-[10px] font-medium space-y-0.5 relative flex-1">
            <i class="fa-solid fa-cart-shopping text-base"></i><span>Cart</span>
            <span id="nav-badge" class="absolute -top-1 right-5 bg-red-600 text-white text-[9px] px-1.5 py-0.2 rounded-full font-bold">0</span>
        </button>
        <button onclick="switchTab('tracking')" id="nav-tracking" class="text-slate-400 flex flex-col items-center text-[10px] font-medium space-y-0.5 flex-1">
            <i class="fa-solid fa-location-crosshairs text-base"></i><span>Track</span>
        </button>
    </nav>

    <!-- App Logic -->
    <script>
        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                splash.style.opacity = '0';
                setTimeout(() => splash.remove(), 500);
            }, 1000);
            loadOrderHistory();
            updateAdminDashboard();
        });

        let cart = [];
        let discountRate = 0;
        let deliveryFee = 0;
        let orderHistory = JSON.parse(localStorage.getItem('crust_orders') || '[]');
        let liveAdminOrders = [
            { id: 101, name: 'Ali Khan', item: 'Medium Fajita', price: 1700, status: 'Preparing' }
        ];

        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            const bg = type === 'success' ? 'bg-green-600' : 'bg-red-600';
            toast.className = `${bg} text-white text-xs font-bold px-4 py-3 rounded-2xl shadow-xl flex items-center justify-between transition transform translate-y-2 opacity-0`;
            toast.innerHTML = `<span>${message}</span><i class="fa-solid fa-circle-check text-sm ml-2"></i>`;
            container.appendChild(toast);
            setTimeout(() => { toast.style.transform = 'translateY(0)'; toast.style.opacity = '1'; }, 50);
            setTimeout(() => { toast.style.opacity = '0'; setTimeout(() => toast.remove(), 300); }, 3000);
        }

        function switchTab(tabId) {
            document.querySelectorAll('.app-screen').forEach(s => s.classList.remove('active'));
            document.getElementById(`screen-${tabId}`).classList.add('active');
            ['home', 'builder', 'cart', 'tracking'].forEach(t => {
                const btn = document.getElementById(`nav-${t}`);
                if(btn) btn.className = (t === tabId) ? "text-red-500 flex flex-col items-center text-[10px] font-bold space-y-0.5 flex-1" : "text-slate-400 flex flex-col items-center text-[10px] font-medium space-y-0.5 flex-1";
            });
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function addToCart(name, btn) {
            const container = btn.closest('.flex-1');
            const sizeSelect = container.querySelector('.size-select');
            const size = sizeSelect.value;
            const price = parseInt(sizeSelect.options[sizeSelect.selectedIndex].getAttribute('data-price'));

            cart.push({ name, size, price });
            updateCartUI();
            showToast(`Added ${name} to Cart!`);
            document.getElementById('upsell-modal').classList.remove('hidden');
        }

        function addHalfPizza() {
            const left = document.getElementById('half-left').value;
            const right = document.getElementById('half-right').value;
            const crust = document.getElementById('build-dough').value;
            const price = crust.includes('2100') ? 2100 : 1800;

            cart.push({ name: `Half-&-Half Pizza`, size: `${left} / ${right}`, price });
            updateCartUI();
            showToast('Half-&-Half Pizza added to cart!');
            switchTab('cart');
        }

        function addUpsell(itemName, price) {
            cart.push({ name: itemName, size: 'Side', price });
            updateCartUI();
            closeUpsell();
            showToast(`Added ${itemName}!`);
        }

        function closeUpsell() {
            document.getElementById('upsell-modal').classList.add('hidden');
        }

        function updateDeliveryCharges() {
            const branch = document.getElementById('branch-select').value;
            deliveryFee = branch.includes('Eidgah') ? 100 : 0;
            updateCartUI();
        }

        function updateCartUI() {
            const container = document.getElementById('cart-items');
            document.getElementById('nav-badge').innerText = cart.length;
            document.getElementById('item-count-badge').innerText = `${cart.length} Items`;

            if(cart.length === 0) {
                container.innerHTML = '<p class="text-slate-500 text-center py-6">Your cart is empty.</p>';
                document.getElementById('subtotal').innerText = 'Rs. 0';
                document.getElementById('delivery-fee').innerText = 'Rs. 0';
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
                        <button onclick="cart.splice(${index},1);updateCartUI()" class="text-red-400 font-bold text-xs"><i class="fa-solid fa-trash"></i></button>
                    </div>
                `;
            });

            let discount = subtotal * discountRate;
            let total = subtotal + deliveryFee - discount;
            document.getElementById('subtotal').innerText = `Rs. ${subtotal}`;
            document.getElementById('delivery-fee').innerText = `Rs. ${deliveryFee}`;
            document.getElementById('discount').innerText = `Rs. ${discount}`;
            document.getElementById('grand-total').innerText = `Rs. ${total}`;
        }

        function getAIRecommendation() {
            const budget = document.getElementById('ai-budget').value;
            const resBox = document.getElementById('ai-result');
            const resText = document.getElementById('ai-text');
            resBox.classList.remove('hidden');

            if(budget === '1500') {
                resText.innerText = "Suggested: Small Chicken Fajita (Rs. 1200)";
            } else {
                resText.innerText = "Suggested: Medium Super Supreme Feast (Rs. 1850)";
            }
        }

        function checkoutWhatsApp() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value;
            const branch = document.getElementById('branch-select').value;
            const payment = document.getElementById('payment-method').value;

            if(!name || !phone || !address || cart.length === 0) {
                showToast('Please fill all details & add items!', 'error');
                return;
            }

            let sub = 0;
            cart.forEach(i => sub += i.price);
            let final = sub + deliveryFee - (sub * discountRate);

            const newOrder = { id: Math.floor(100 + Math.random() * 900), name, items: [...cart], total: final, date: new Date().toLocaleDateString() };
            orderHistory.unshift(newOrder);
            localStorage.setItem('crust_orders', JSON.stringify(orderHistory));
            liveAdminOrders.unshift({ id: newOrder.id, name, item: `${cart.length} Items`, price: final, status: 'Pending' });

            loadOrderHistory();
            updateAdminDashboard();

            let msg = `*New Order - Crust Pizza (${branch})*%0A%0A*Name:* ${name}%0A*Phone:* ${phone}%0A*Address:* ${address}%0A*Payment:* ${payment}%0A%0A*Items:*%0A`;
            cart.forEach(i => { msg += `- ${i.name} (${i.size}): Rs. ${i.price}%0A`; });
            msg += `%0A*Total Bill:* Rs. ${final} (ETA: 35 mins)`;

            window.open(`https://wa.me/923001234567?text=${msg}`, '_blank');
            cart = [];
            updateCartUI();
            switchTab('tracking');
            document.getElementById('track-id').value = `#${newOrder.id}`;
            trackOrder();
        }

        function updateAdminDashboard() {
            const list = document.getElementById('admin-orders-list');
            list.innerHTML = '';
            liveAdminOrders.forEach((o, index) => {
                list.innerHTML += `
                    <div class="bg-slate-800 p-2.5 rounded-lg space-y-1 border border-slate-700">
                        <div class="flex justify-between items-center">
                            <strong class="text-white">#${o.id} - ${o.name}</strong>
                            <span class="text-[10px] bg-yellow-500/20 text-yellow-400 px-1.5 py-0.5 rounded font-bold">${o.status}</span>
                        </div>
                        <p class="text-[10px] text-slate-400">${o.item} - Total: Rs. ${o.price}</p>
                        <div class="flex space-x-1 mt-1">
                            <button onclick="changeOrderStatus(${index}, 'Cooking')" class="bg-blue-600 text-white text-[9px] px-2 py-0.5 rounded">Cooking</button>
                            <button onclick="changeOrderStatus(${index}, 'Out for Delivery')" class="bg-purple-600 text-white text-[9px] px-2 py-0.5 rounded">Dispatch</button>
                            <button onclick="printKOT(${o.id})" class="bg-yellow-500 text-slate-950 font-bold text-[9px] px-2 py-0.5 rounded"><i class="fa-solid fa-print"></i> KOT Print</button>
                        </div>
                    </div>
                `;
            });
        }

        function changeOrderStatus(index, status) {
            liveAdminOrders[index].status = status;
            updateAdminDashboard();
            showToast(`Order status updated to: ${status}`);
        }

        function printKOT(orderId) {
            const order = liveAdminOrders.find(o => o.id === orderId);
            const container = document.getElementById('printable-receipt');
            container.innerHTML = `
                <h2>--- CRUST PIZZA KOT ---</h2>
                <p>Order ID: #${order.id}</p>
                <p>Customer: ${order.name}</p>
                <p>Items: ${order.item}</p>
                <p>Amount: Rs. ${order.price}</p>
                <hr>
                <p>Time: ${new Date().toLocaleTimeString()}</p>
            `;
            window.print();
        }

        function trackOrder() {
            const id = document.getElementById('track-id').value.trim();
            if(id) {
                document.getElementById('tracking-result').classList.remove('hidden');
            }
        }

        function loadOrderHistory() {
            const container = document.getElementById('order-history-list');
            if(orderHistory.length === 0) return;
            container.innerHTML = '';
            orderHistory.forEach((ord) => {
                container.innerHTML += `
                    <div class="bg-slate-900 p-2.5 rounded-xl border border-slate-800 flex justify-between items-center">
                        <div><strong class="text-white">#${ord.id} - ${ord.date}</strong><br><span class="text-[10px] text-slate-400">Total: Rs. ${ord.total}</span></div>
                        <span class="text-[10px] text-green-400 font-bold">Delivered</span>
                    </div>
                `;
            });
        }

        function openSpinWheel() { document.getElementById('spin-modal').classList.remove('hidden'); }
        function spinWheelAction() {
            discountRate = 0.15;
            updateCartUI();
            document.getElementById('spin-result').innerText = "🎉 You won 15% OFF!";
            showToast('15% Discount applied!');
        }
        function applyCoupon() {
            if(document.getElementById('coupon-code').value.toUpperCase() === 'CRUST10') {
                discountRate = 0.10;
                updateCartUI();
                showToast('10% Coupon Applied!');
            }
        }
        function toggleWishlist(name, btn) { showToast(`Saved ${name} to Wishlist!`); }
        function filterMenu(cat) { showToast(`Showing: ${cat.toUpperCase()}`); }
    </script>
</body>
</html>
