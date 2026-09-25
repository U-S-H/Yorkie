<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crust Pizza - Ultra Modern UI</title>
    <!-- Tailwind CSS Standard CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #0b0f19;
            color: #f8fafc;
            padding-bottom: 90px;
            margin: 0;
        }
        .app-screen { display: none; opacity: 0; transition: opacity 0.3s ease-in-out; }
        .app-screen.active { display: block; opacity: 1; }
        
        /* Glassmorphism Ultra Premium */
        .glass-card {
            background: linear-gradient(135deg, rgba(30, 41, 59, 0.7), rgba(15, 23, 42, 0.8));
            backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }
        .glass-nav {
            background: rgba(11, 15, 25, 0.85);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        /* Modern Glow & Gradients */
        .glow-red { box-shadow: 0 0 20px rgba(239, 68, 68, 0.35); }
        .glow-yellow { box-shadow: 0 0 20px rgba(245, 158, 11, 0.35); }
        .gradient-hot { background: linear-gradient(135deg, #ff416c, #ff4b2b); }
        .gradient-gold { background: linear-gradient(135deg, #f59e0b, #d97706); }

        /* Smooth Hide Scrollbars */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        /* Modern Spinner */
        #splash-screen {
            position: fixed; inset: 0; background: #0b0f19; z-index: 99999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 0.5s ease;
        }
        .modern-spinner {
            width: 60px; height: 60px;
            border: 4px solid rgba(239, 68, 68, 0.2);
            border-top: 4px solid #ef4444;
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Wheel Styles */
        .wheel-container { position: relative; width: 250px; height: 250px; margin: 0 auto; }
        .wheel-pointer {
            position: absolute; top: -12px; left: 50%; transform: translateX(-50%);
            width: 0; height: 0;
            border-left: 12px solid transparent; border-right: 12px solid transparent;
            border-top: 22px solid #facc15; z-index: 30;
            filter: drop-shadow(0px 4px 6px rgba(0,0,0,0.6));
        }
        .wheel-disc {
            width: 100%; height: 100%; border-radius: 50%;
            border: 5px solid #facc15;
            background: conic-gradient(
                #ef4444 0deg 60deg, #3b82f6 60deg 120deg,
                #10b981 120deg 180deg, #f59e0b 180deg 240deg,
                #8b5cf6 240deg 300deg, #ec4899 300deg 360deg
            );
            transition: transform 4s cubic-bezier(0.15, 0.9, 0.15, 1);
            position: relative; overflow: hidden;
        }
        .wheel-label {
            position: absolute; top: 50%; left: 50%; width: 50%; height: 30px; margin-top: -15px;
            transform-origin: 0% 50%; display: flex; align-items: center; justify-content: flex-end;
            padding-right: 15px; font-size: 11px; font-weight: 900; color: #ffffff;
            text-shadow: 1px 1px 3px rgba(0,0,0,0.9);
        }
        .label-1 { transform: rotate(30deg); } .label-2 { transform: rotate(90deg); }
        .label-3 { transform: rotate(150deg); } .label-4 { transform: rotate(210deg); }
        .label-5 { transform: rotate(270deg); } .label-6 { transform: rotate(330deg); }
    </style>
</head>
<body class="max-w-md mx-auto min-h-screen relative border-x border-slate-800/80 shadow-2xl">

    <!-- Splash Screen Loader -->
    <div id="splash-screen">
        <div class="relative flex items-center justify-center mb-6">
            <div class="modern-spinner"></div>
            <div class="absolute bg-red-600 p-3.5 rounded-2xl text-white glow-red">
                <i class="fa-solid fa-pizza-slice text-2xl"></i>
            </div>
        </div>
        <h1 class="font-black text-2xl tracking-widest text-white">CRUST PIZZA</h1>
        <p class="text-[10px] text-yellow-400 font-bold mt-1 tracking-widest uppercase">Ultra Modern Super App</p>
    </div>

    <!-- Toast Container -->
    <div id="toast-container" class="fixed top-5 left-1/2 -translate-x-1/2 z-50 w-11/12 max-w-sm space-y-2 pointer-events-none"></div>

    <!-- App Header -->
    <header class="sticky top-0 z-40 px-4 py-3.5 flex justify-between items-center glass-nav border-b border-slate-800">
        <div class="flex items-center space-x-3" onclick="switchTab('home')">
            <div class="bg-gradient-to-tr from-red-600 to-orange-500 p-2.5 rounded-2xl text-white glow-red">
                <i class="fa-solid fa-pizza-slice text-lg"></i>
            </div>
            <div>
                <h1 class="font-black text-lg tracking-wide text-white leading-tight">CRUST PIZZA</h1>
                <div class="flex items-center space-x-1 text-yellow-400 text-[11px] font-bold">
                    <i class="fa-solid fa-location-dot text-[10px]"></i>
                    <select id="branch-select" onchange="updateDeliveryCharges()" class="bg-transparent border-none text-yellow-400 font-bold focus:outline-none cursor-pointer">
                        <option value="Astore Main" class="bg-slate-900">Astore Main Branch</option>
                        <option value="Eidgah Branch" class="bg-slate-900">Astore Eidgah Branch (+Rs. 100)</option>
                    </select>
                </div>
            </div>
        </div>
        <button onclick="switchTab('admin')" class="bg-slate-800/80 hover:bg-slate-700 text-slate-300 text-xs px-3 py-2 rounded-xl font-bold border border-slate-700 flex items-center space-x-1.5 transition active:scale-95">
            <i class="fa-solid fa-chart-pie text-yellow-400"></i>
            <span>Admin</span>
        </button>
    </header>

    <!-- Main Body -->
    <main class="p-4 space-y-4">

        <!-- SCREEN 1: HOME -->
        <div id="screen-home" class="app-screen active space-y-4">
            
            <!-- AI Smart Recommendation -->
            <div class="glass-card p-4 rounded-3xl border border-amber-500/30 relative overflow-hidden">
                <div class="flex items-center justify-between mb-2">
                    <div class="flex items-center space-x-2">
                        <div class="bg-amber-400/10 p-2 rounded-xl text-yellow-400">
                            <i class="fa-solid fa-wand-magic-sparkles"></i>
                        </div>
                        <div>
                            <h3 class="text-xs font-black text-white uppercase tracking-wider">AI Pizza Recommendation</h3>
                            <p class="text-[10px] text-slate-400">Find best pizza according to budget</p>
                        </div>
                    </div>
                    <span class="text-[9px] bg-gradient-to-r from-amber-400 to-orange-500 text-slate-950 px-2 py-0.5 rounded-full font-black">SMART AI</span>
                </div>
                <div class="flex space-x-2 mt-3">
                    <select id="ai-budget" class="bg-slate-900/90 text-slate-200 border border-slate-700/80 rounded-2xl text-xs p-3 flex-1 focus:outline-none focus:border-amber-400">
                        <option value="1500">Budget: Under Rs. 1500</option>
                        <option value="2000" selected>Budget: Under Rs. 2000</option>
                        <option value="2500">Budget: Unlimited</option>
                    </select>
                    <button onclick="getAIRecommendation()" class="gradient-gold text-slate-950 font-black px-4 rounded-2xl text-xs flex items-center space-x-1.5 shadow-lg active:scale-95 transition">
                        <i class="fa-solid fa-bolt"></i>
                        <span>Suggest</span>
                    </button>
                </div>
                <div id="ai-result" class="hidden mt-3 pt-3 border-t border-slate-800 text-xs text-yellow-300 font-bold flex justify-between items-center">
                    <span id="ai-text"></span>
                    <button id="ai-add-btn" class="bg-red-600 text-white px-3 py-1.5 rounded-xl text-[10px] font-bold shadow-md">Add Now</button>
                </div>
            </div>

            <!-- Promo Banner -->
            <div class="gradient-hot text-white p-5 rounded-3xl glow-red relative overflow-hidden">
                <div class="relative z-10">
                    <span class="bg-black/30 text-yellow-300 text-[10px] font-black px-3 py-1 rounded-full uppercase tracking-widest backdrop-blur-md border border-white/10">LIMITED OFFER</span>
                    <h2 class="text-2xl font-black mt-2 leading-tight">Spin & Win Deals!</h2>
                    <p class="text-xs text-red-100 mt-1 font-medium">Win instant discounts & free food vouchers!</p>
                    <button onclick="openSpinWheel()" class="mt-4 bg-slate-950 hover:bg-slate-900 text-yellow-400 text-xs font-black px-4 py-2.5 rounded-2xl shadow-2xl transition active:scale-95 flex items-center space-x-2 border border-yellow-400/30">
                        <i class="fa-solid fa-dharmachakra animate-spin text-sm"></i>
                        <span>Spin Magic Wheel</span>
                    </button>
                </div>
                <i class="fa-solid fa-pizza-slice text-white/10 text-9xl absolute -right-6 -bottom-6"></i>
            </div>

            <!-- Quick Service Actions -->
            <div class="grid grid-cols-2 gap-3">
                <button onclick="switchTab('builder')" class="glass-card p-3.5 rounded-2xl border border-slate-800 text-left hover:border-yellow-500/50 transition flex items-center space-x-3 active:scale-95">
                    <div class="bg-amber-500/10 p-2.5 rounded-xl text-yellow-400"><i class="fa-solid fa-sliders text-base"></i></div>
                    <div>
                        <div class="text-xs font-black text-white">Half-&-Half</div>
                        <div class="text-[10px] text-slate-400">Custom 2 Flavors</div>
                    </div>
                </button>
                <button onclick="switchTab('tracking')" class="glass-card p-3.5 rounded-2xl border border-slate-800 text-left hover:border-red-500/50 transition flex items-center space-x-3 active:scale-95">
                    <div class="bg-red-500/10 p-2.5 rounded-xl text-red-400"><i class="fa-solid fa-location-dot text-base"></i></div>
                    <div>
                        <div class="text-xs font-black text-white">Track Order</div>
                        <div class="text-[10px] text-slate-400">Live Kitchen Status</div>
                    </div>
                </button>
            </div>

            <!-- Modern Horizontal Category Filters -->
            <div class="flex space-x-2.5 overflow-x-auto no-scrollbar py-1">
                <button onclick="filterMenu('all')" class="gradient-hot text-white text-xs px-4 py-2.5 rounded-2xl font-black shadow-lg shadow-red-600/30 whitespace-nowrap">🔥 All Menu</button>
                <button onclick="filterMenu('deals')" class="glass-card text-slate-300 text-xs px-4 py-2.5 rounded-2xl font-bold whitespace-nowrap border border-slate-800 hover:text-white">🎉 Special Deals</button>
                <button onclick="filterMenu('pizzas')" class="glass-card text-slate-300 text-xs px-4 py-2.5 rounded-2xl font-bold whitespace-nowrap border border-slate-800 hover:text-white">🍕 Pizzas</button>
                <button onclick="filterMenu('burgers')" class="glass-card text-slate-300 text-xs px-4 py-2.5 rounded-2xl font-bold whitespace-nowrap border border-slate-800 hover:text-white">🍔 Fast Food</button>
            </div>

            <!-- Product Cards List -->
            <div class="space-y-3.5" id="product-list-container">
                
                <!-- Item 1 -->
                <div class="glass-card p-3.5 rounded-3xl border border-slate-800/80 flex space-x-3.5 items-center hover:border-red-500/40 transition relative">
                    <img src="https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=300&q=80" class="w-24 h-24 object-cover rounded-2xl shadow-lg">
                    <div class="flex-1">
                        <div class="flex justify-between items-start">
                            <h4 class="font-black text-white text-sm">Family Feast Combo</h4>
                            <span class="text-[9px] bg-red-500/20 text-red-400 border border-red-500/30 px-2 py-0.5 rounded-full font-black">MEGA DEAL</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-1">2 Large Pizzas + 1.5L Drink + Garlic Bread</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-900 text-slate-200 rounded-xl mt-2 p-1.5 w-full focus:outline-none">
                            <option value="Standard Deal" data-price="3800">Standard Package - Rs. 3800</option>
                        </select>
                        <div class="flex justify-between items-center mt-3">
                            <span class="font-black text-red-500 text-base">Rs. 3800</span>
                            <button onclick="addToCart('Family Feast Combo', this)" class="gradient-hot hover:opacity-90 text-white text-xs px-4 py-2 rounded-xl font-bold shadow-lg glow-red flex items-center space-x-1 active:scale-95 transition">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Item 2 -->
                <div class="glass-card p-3.5 rounded-3xl border border-slate-800/80 flex space-x-3.5 items-center hover:border-red-500/40 transition relative">
                    <img src="https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=300&q=80" class="w-24 h-24 object-cover rounded-2xl shadow-lg">
                    <div class="flex-1">
                        <div class="flex justify-between items-start">
                            <h4 class="font-black text-white text-sm">Chicken Fajita Special</h4>
                            <span class="text-[9px] bg-green-500/20 text-green-400 border border-green-500/30 px-2 py-0.5 rounded-full font-black">POPULAR</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-1">Spicy chicken, onions, capsicum, mozzarella</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-900 text-slate-200 rounded-xl mt-2 p-1.5 w-full focus:outline-none">
                            <option value="Small" data-price="1200">Small (10") - Rs. 1200</option>
                            <option value="Medium" data-price="1700" selected>Medium (13") - Rs. 1700</option>
                            <option value="Large" data-price="2200">Large (16") - Rs. 2200</option>
                        </select>
                        <div class="flex justify-between items-center mt-3">
                            <span class="font-black text-red-500 text-base">Rs. 1700</span>
                            <button onclick="addToCart('Chicken Fajita Special', this)" class="gradient-hot hover:opacity-90 text-white text-xs px-4 py-2 rounded-xl font-bold shadow-lg glow-red flex items-center space-x-1 active:scale-95 transition">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Item 3 -->
                <div class="glass-card p-3.5 rounded-3xl border border-slate-800/80 flex space-x-3.5 items-center hover:border-red-500/40 transition relative">
                    <img src="https://images.unsplash.com/photo-1568901346375-23c9450c58cd?auto=format&fit=crop&w=300&q=80" class="w-24 h-24 object-cover rounded-2xl shadow-lg">
                    <div class="flex-1">
                        <div class="flex justify-between items-start">
                            <h4 class="font-black text-white text-sm">Crispy Zinger Burger</h4>
                            <span class="text-[9px] bg-amber-500/20 text-amber-400 border border-amber-500/30 px-2 py-0.5 rounded-full font-black">HOT</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-1">Crispy fried chicken fillet, mayo, cheese</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-900 text-slate-200 rounded-xl mt-2 p-1.5 w-full focus:outline-none">
                            <option value="Single Burger" data-price="550">Single Burger - Rs. 550</option>
                            <option value="Zinger Meal (+Fries & Drink)" data-price="850">Zinger Meal - Rs. 850</option>
                        </select>
                        <div class="flex justify-between items-center mt-3">
                            <span class="font-black text-red-500 text-base">Rs. 550</span>
                            <button onclick="addToCart('Crispy Zinger Burger', this)" class="gradient-hot hover:opacity-90 text-white text-xs px-4 py-2 rounded-xl font-bold shadow-lg glow-red flex items-center space-x-1 active:scale-95 transition">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>

            </div>
        </div>

        <!-- SCREEN 2: BUILDER -->
        <div id="screen-builder" class="app-screen space-y-4">
            <div class="glass-card p-5 rounded-3xl border border-slate-800 space-y-4">
                <div class="flex items-center space-x-3">
                    <div class="gradient-gold p-3 rounded-2xl text-slate-950 font-black"><i class="fa-solid fa-pizza-slice text-xl"></i></div>
                    <div>
                        <h3 class="font-black text-white text-base">Half-&-Half Pizza Builder</h3>
                        <p class="text-xs text-slate-400">Combine 2 flavors on 1 pizza</p>
                    </div>
                </div>
                
                <div class="space-y-3.5">
                    <div>
                        <label class="text-[10px] font-black text-amber-400 uppercase tracking-widest">Left Side Flavor</label>
                        <select id="half-left" class="w-full border border-slate-700/80 bg-slate-900 text-slate-200 rounded-2xl p-3 text-xs mt-1 focus:outline-none">
                            <option value="Chicken Fajita">Chicken Fajita</option>
                            <option value="Super Supreme">Super Supreme</option>
                            <option value="BBQ Tikka">BBQ Tikka</option>
                        </select>
                    </div>

                    <div>
                        <label class="text-[10px] font-black text-amber-400 uppercase tracking-widest">Right Side Flavor</label>
                        <select id="half-right" class="w-full border border-slate-700/80 bg-slate-900 text-slate-200 rounded-2xl p-3 text-xs mt-1 focus:outline-none">
                            <option value="Veggie Supreme">Veggie Supreme</option>
                            <option value="Cheese Lover">Cheese Lover</option>
                            <option value="Pepperoni Passion">Pepperoni Passion</option>
                        </select>
                    </div>

                    <div>
                        <label class="text-[10px] font-black text-amber-400 uppercase tracking-widest">Crust Style</label>
                        <select id="build-dough" class="w-full border border-slate-700/80 bg-slate-900 text-slate-200 rounded-2xl p-3 text-xs mt-1 focus:outline-none">
                            <option value="Regular Crust (Rs. 1800)">Medium Regular Crust - Rs. 1800</option>
                            <option value="Cheese Stuffed Crust (Rs. 2100)">Medium Stuffed Crust - Rs. 2100</option>
                        </select>
                    </div>

                    <button onclick="addHalfPizza()" class="w-full gradient-hot text-white font-black py-3.5 rounded-2xl text-xs shadow-xl glow-red mt-2 active:scale-95 transition">
                        <i class="fa-solid fa-cart-plus mr-1.5"></i> Add Custom Pizza To Cart
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 3: CART -->
        <div id="screen-cart" class="app-screen space-y-4">
            <div class="glass-card p-5 rounded-3xl border border-slate-800">
                <h3 class="font-black text-white text-base mb-4 flex items-center justify-between">
                    <span>🛒 Checkout Cart</span>
                    <span id="item-count-badge" class="text-[10px] bg-red-600 text-white px-2.5 py-1 rounded-full font-bold">0 Items</span>
                </h3>

                <div id="cart-items" class="divide-y divide-slate-800/80 text-xs mb-4 max-h-48 overflow-y-auto">
                    <p class="text-slate-500 text-center py-6">Your cart is empty.</p>
                </div>

                <div class="flex space-x-2 mb-4">
                    <input type="text" id="coupon-code" placeholder="Promo Code (CRUST10)" class="border border-slate-700/80 bg-slate-900 text-white rounded-2xl px-3.5 py-2.5 text-xs w-full uppercase focus:outline-none">
                    <button onclick="applyCoupon()" class="bg-slate-800 hover:bg-slate-700 border border-slate-700 text-white px-4 py-2.5 rounded-2xl text-xs font-bold">Apply</button>
                </div>

                <div class="border-t border-slate-800 pt-3 text-xs space-y-2 mb-4">
                    <div class="flex justify-between text-slate-400"><span>Subtotal:</span><span id="subtotal" class="text-white font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-slate-400"><span>Delivery Fee:</span><span id="delivery-fee" class="text-yellow-400 font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-slate-400"><span>Discount:</span><span id="discount" class="text-green-400 font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-base font-black text-white border-t border-slate-800 pt-2"><span>Total Amount:</span><span id="grand-total" class="text-red-500">Rs. 0</span></div>
                </div>

                <div class="space-y-3">
                    <h4 class="font-black text-[11px] text-amber-400 uppercase tracking-widest">Delivery Address</h4>
                    <input type="text" id="cust-name" placeholder="Full Name" class="w-full border border-slate-700/80 bg-slate-900 text-white rounded-2xl p-3 text-xs focus:outline-none">
                    <input type="text" id="cust-phone" placeholder="Active Phone Number" class="w-full border border-slate-700/80 bg-slate-900 text-white rounded-2xl p-3 text-xs focus:outline-none">
                    <textarea id="cust-address" placeholder="Full Address" class="w-full border border-slate-700/80 bg-slate-900 text-white rounded-2xl p-3 text-xs focus:outline-none" rows="2"></textarea>
                    
                    <button onclick="checkoutWhatsApp()" class="w-full bg-green-600 hover:bg-green-500 text-white font-black py-4 rounded-2xl shadow-xl flex items-center justify-center space-x-2 text-xs active:scale-95 transition">
                        <i class="fa-brands fa-whatsapp text-lg"></i>
                        <span>Order via WhatsApp Now</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 4: TRACKING -->
        <div id="screen-tracking" class="app-screen space-y-4">
            <div class="glass-card p-5 rounded-3xl border border-slate-800">
                <h3 class="font-black text-white text-base mb-2 flex items-center space-x-2">
                    <i class="fa-solid fa-radar text-red-500"></i>
                    <span>Live Order Tracker</span>
                </h3>
                <div class="flex space-x-2 my-4">
                    <input type="text" id="track-id" placeholder="Enter Order ID" class="border border-slate-700/80 bg-slate-900 text-white rounded-2xl p-3 text-xs w-full focus:outline-none">
                    <button onclick="trackOrder()" class="gradient-hot text-white px-5 py-3 rounded-2xl text-xs font-bold">Track</button>
                </div>
                <div id="tracking-result" class="hidden space-y-3 bg-slate-950 p-4 rounded-2xl border border-slate-800 text-xs">
                    <div class="flex items-center space-x-3 text-green-400 font-bold"><i class="fa-solid fa-circle-check"></i><span>Order Accepted</span></div>
                    <div class="flex items-center space-x-3 text-yellow-400 font-bold"><i class="fa-solid fa-fire-burner animate-spin"></i><span>Kitchen Preparing</span></div>
                </div>
            </div>
        </div>

        <!-- SCREEN 5: ADMIN -->
        <div id="screen-admin" class="app-screen space-y-4">
            <div class="glass-card p-5 rounded-3xl border border-slate-800 space-y-3">
                <h3 class="font-black text-white text-base"> Kitchen Admin Control</h3>
                <div id="admin-orders-list" class="space-y-2 text-xs">No active kitchen orders.</div>
            </div>
        </div>

    </main>

    <!-- SPIN WHEEL MODAL -->
    <div id="spin-modal" class="fixed inset-0 bg-black/85 z-50 flex items-center justify-center hidden p-4 backdrop-blur-md">
        <div class="glass-card p-6 rounded-3xl border border-slate-700 text-center max-w-xs w-full space-y-4 relative">
            <h3 class="font-black text-xl text-white">SPIN & WIN DEALS</h3>
            <p class="text-xs text-slate-300">Tap below to spin for instant discount coupons!</p>
            
            <div class="wheel-container">
                <div class="wheel-pointer"></div>
                <div id="modern-wheel" class="wheel-disc">
                    <div class="wheel-label label-1">10% OFF</div>
                    <div class="wheel-label label-2">FREE DRINK</div>
                    <div class="wheel-label label-3">15% OFF</div>
                    <div class="wheel-label label-4">20% OFF</div>
                    <div class="wheel-label label-5">FREE FRIES</div>
                    <div class="wheel-label label-6">5% OFF</div>

                    <div class="absolute inset-0 flex items-center justify-center">
                        <div class="w-10 h-10 bg-slate-950 border-2 border-yellow-400 rounded-full flex items-center justify-center z-20 shadow-xl">
                            <i class="fa-solid fa-star text-yellow-400 text-xs"></i>
                        </div>
                    </div>
                </div>
            </div>

            <div id="spin-result" class="text-yellow-300 font-bold text-sm min-h-[20px]"></div>

            <button onclick="spinModernWheel()" id="spin-btn" class="w-full gradient-gold text-slate-950 font-black py-3.5 rounded-2xl text-xs shadow-xl active:scale-95 transition">
                SPIN WHEEL NOW
            </button>
            <button onclick="document.getElementById('spin-modal').classList.add('hidden')" class="text-slate-400 text-xs hover:text-white underline block mx-auto">Close</button>
        </div>
    </div>

    <!-- Bottom Navigation Bar -->
    <nav class="glass-nav fixed bottom-0 left-0 right-0 max-w-md mx-auto px-4 py-3 z-50 flex justify-around items-center border-t border-slate-800">
        <button onclick="switchTab('home')" id="nav-home" class="text-red-500 flex flex-col items-center text-[10px] font-bold flex-1">
            <i class="fa-solid fa-utensils text-lg mb-1"></i><span>Menu</span>
        </button>
        <button onclick="switchTab('builder')" id="nav-builder" class="text-slate-400 flex flex-col items-center text-[10px] font-medium flex-1">
            <i class="fa-solid fa-sliders text-lg mb-1"></i><span>Builder</span>
        </button>
        <button onclick="switchTab('cart')" id="nav-cart" class="text-slate-400 flex flex-col items-center text-[10px] font-medium relative flex-1">
            <i class="fa-solid fa-cart-shopping text-lg mb-1"></i><span>Cart</span>
            <span id="nav-badge" class="absolute -top-1 right-5 bg-red-600 text-white text-[9px] px-1.5 py-0.2 rounded-full font-bold">0</span>
        </button>
        <button onclick="switchTab('tracking')" id="nav-tracking" class="text-slate-400 flex flex-col items-center text-[10px] font-medium flex-1">
            <i class="fa-solid fa-location-arrow text-lg mb-1"></i><span>Track</span>
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
        });

        let cart = [];
        let discountRate = 0;
        let deliveryFee = 0;
        let isSpinning = false;

        function showToast(message) {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = `glass-card border border-green-500/50 text-white text-xs font-bold px-4 py-3 rounded-2xl shadow-2xl flex items-center justify-between transition transform translate-y-2 opacity-0`;
            toast.innerHTML = `<span>${message}</span><i class="fa-solid fa-circle-check text-green-400 text-sm ml-2"></i>`;
            container.appendChild(toast);
            setTimeout(() => { toast.style.transform = 'translateY(0)'; toast.style.opacity = '1'; }, 50);
            setTimeout(() => { toast.style.opacity = '0'; setTimeout(() => toast.remove(), 300); }, 2500);
        }

        function switchTab(tabId) {
            document.querySelectorAll('.app-screen').forEach(s => s.classList.remove('active'));
            document.getElementById(`screen-${tabId}`).classList.add('active');
            ['home', 'builder', 'cart', 'tracking'].forEach(t => {
                const btn = document.getElementById(`nav-${t}`);
                if(btn) btn.className = (t === tabId) ? "text-red-500 flex flex-col items-center text-[10px] font-bold flex-1" : "text-slate-400 flex flex-col items-center text-[10px] font-medium flex-1";
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
        }

        function addHalfPizza() {
            const left = document.getElementById('half-left').value;
            const right = document.getElementById('half-right').value;
            const crust = document.getElementById('build-dough').value;
            const price = crust.includes('2100') ? 2100 : 1800;

            cart.push({ name: `Half-&-Half Pizza`, size: `${left} / ${right}`, price });
            updateCartUI();
            showToast('Half-&-Half Pizza added!');
            switchTab('cart');
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
                    <div class="py-2.5 flex justify-between items-center">
                        <div><b class="text-white text-xs">${item.name}</b><br><span class="text-[10px] text-slate-400">${item.size} - Rs. ${item.price}</span></div>
                        <button onclick="cart.splice(${index},1);updateCartUI()" class="text-red-400 font-bold text-xs p-1"><i class="fa-solid fa-trash-can"></i></button>
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

        function openSpinWheel() { document.getElementById('spin-modal').classList.remove('hidden'); }

        function spinModernWheel() {
            if(isSpinning) return;
            isSpinning = true;
            const wheel = document.getElementById('modern-wheel');
            const res = document.getElementById('spin-result');
            const btn = document.getElementById('spin-btn');
            
            btn.disabled = true;
            res.innerText = "Spinning...";

            const targetRotation = 3600 + 210; 
            wheel.style.transform = `rotate(${targetRotation}deg)`;

            setTimeout(() => {
                discountRate = 0.15;
                updateCartUI();
                res.innerHTML = "🎉 YOU WON 15% OFF DISCOUNT!";
                showToast("15% Discount unlocked!");
                isSpinning = false;
            }, 4000);
        }

        function updateDeliveryCharges() {
            const branch = document.getElementById('branch-select').value;
            deliveryFee = branch.includes('Eidgah') ? 100 : 0;
            updateCartUI();
        }

        function checkoutWhatsApp() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value;
            const branch = document.getElementById('branch-select').value;

            if(!name || !phone || !address || cart.length === 0) {
                showToast('Please complete all details!');
                return;
            }

            let sub = 0;
            cart.forEach(i => sub += i.price);
            let final = sub + deliveryFee - (sub * discountRate);

            let msg = `*New Order - Crust Pizza (${branch})*%0A%0A*Name:* ${name}%0A*Phone:* ${phone}%0A*Address:* ${address}%0A%0A*Items:*%0A`;
            cart.forEach(i => { msg += `- ${i.name} (${i.size}): Rs. ${i.price}%0A`; });
            msg += `%0A*Total Bill:* Rs. ${final}`;

            window.open(`https://wa.me/923001234567?text=${msg}`, '_blank');
            cart = [];
            updateCartUI();
            switchTab('tracking');
        }

        function getAIRecommendation() {
            const budget = document.getElementById('ai-budget').value;
            const resBox = document.getElementById('ai-result');
            const resText = document.getElementById('ai-text');
            resBox.classList.remove('hidden');
            resText.innerText = budget === '1500' ? "Suggested: Zinger Meal (Rs. 850)" : "Suggested: Chicken Fajita Medium (Rs. 1700)";
        }

        function trackOrder() {
            if(document.getElementById('track-id').value) {
                document.getElementById('tracking-result').classList.remove('hidden');
            }
        }

        function applyCoupon() {
            if(document.getElementById('coupon-code').value.toUpperCase() === 'CRUST10') {
                discountRate = 0.10;
                updateCartUI();
                showToast('10% Coupon Applied!');
            }
        }
        function filterMenu(cat) { showToast(`Showing: ${cat.toUpperCase()}`); }
    </script>
</body>
</html> 
