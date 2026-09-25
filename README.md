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
        
        /* Modern Splash Screen Loader */
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

        .bottom-nav {
            position: fixed; bottom: 0; left: 0; right: 0; max-width: 28rem;
            margin: 0 auto; display: flex; flex-direction: row; justify-content: space-around;
            align-items: center; z-index: 9999;
        }
        #toast-container {
            position: fixed; top: 70px; left: 50%; transform: translateX(-50%);
            z-index: 99999; width: 90%; max-width: 400px; pointer-events: none;
        }

        /* Modern Spin Wheel Styles */
        .wheel-container {
            position: relative; width: 260px; height: 260px; margin: 0 auto;
        }
        .wheel-pointer {
            position: absolute; top: -14px; left: 50%; transform: translateX(-50%);
            width: 0; height: 0;
            border-left: 14px solid transparent;
            border-right: 14px solid transparent;
            border-top: 24px solid #facc15;
            z-index: 30;
            filter: drop-shadow(0px 4px 6px rgba(0,0,0,0.6));
        }
        .wheel-disc {
            width: 100%; height: 100%; border-radius: 50%;
            border: 6px solid #facc15;
            box-shadow: 0 0 20px rgba(250, 204, 21, 0.4), inset 0 0 15px rgba(0,0,0,0.6);
            background: conic-gradient(
                #ef4444 0deg 60deg,
                #3b82f6 60deg 120deg,
                #10b981 120deg 180deg,
                #f59e0b 180deg 240deg,
                #8b5cf6 240deg 300deg,
                #ec4899 300deg 360deg
            );
            transition: transform 4s cubic-bezier(0.15, 0.9, 0.15, 1);
            position: relative;
            overflow: hidden;
        }
        
        /* Wheel Slice Text Overlay */
        .wheel-label {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 50%;
            height: 30px;
            margin-top: -15px;
            transform-origin: 0% 50%;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 18px;
            font-size: 12px;
            font-weight: 900;
            color: #ffffff;
            text-shadow: 1px 1px 3px rgba(0,0,0,0.8);
        }
        
        .label-1 { transform: rotate(30deg); }
        .label-2 { transform: rotate(90deg); }
        .label-3 { transform: rotate(150deg); }
        .label-4 { transform: rotate(210deg); }
        .label-5 { transform: rotate(270deg); }
        .label-6 { transform: rotate(330deg); }

        @media print {
            body * { visibility: hidden; }
            #printable-receipt, #printable-receipt * { visibility: visible; }
            #printable-receipt { position: absolute; left: 0; top: 0; width: 100%; color: black; background: white; padding: 20px; }
        }
    </style>
</head>
<body class="font-sans antialiased max-w-md mx-auto min-h-screen relative shadow-2xl overflow-x-hidden border-x border-slate-800">

    <!-- Modern Splash Loading Screen -->
    <div id="splash-screen">
        <div class="relative flex items-center justify-center mb-6">
            <div class="modern-spinner"></div>
            <div class="absolute bg-red-600 p-3 rounded-2xl text-white shadow-xl shadow-red-600/50">
                <i class="fa-solid fa-pizza-slice text-2xl"></i>
            </div>
        </div>
        <h1 class="font-black text-2xl tracking-wider text-white">CRUST PIZZA</h1>
        <p class="text-xs text-yellow-400 font-semibold mt-1 tracking-widest uppercase">Astore & Eidgah Super App</p>
    </div>

    <!-- Toast Container -->
    <div id="toast-container" class="space-y-2"></div>

    <!-- Printable Receipt -->
    <div id="printable-receipt" class="hidden text-black bg-white p-4 font-mono text-xs"></div>

    <!-- App Header -->
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
                <span>Admin</span>
            </button>
        </div>
    </header>

    <!-- Main Screens Container -->
    <main class="p-4 space-y-4">

        <!-- SCREEN 1: HOME -->
        <div id="screen-home" class="app-screen active space-y-4">
            
            <!-- AI Recommendation Assistant -->
            <div class="bg-slate-900/90 border border-yellow-500/30 p-3.5 rounded-2xl relative overflow-hidden">
                <div class="flex items-center justify-between mb-2">
                    <div class="flex items-center space-x-2">
                        <i class="fa-solid fa-robot text-yellow-400 text-lg"></i>
                        <h3 class="text-xs font-bold text-white">AI Pizza Recommendation</h3>
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

            <!-- Banner & Modern Spin Trigger -->
            <div class="bg-gradient-to-r from-red-600 via-orange-600 to-amber-500 text-white p-4 rounded-2xl shadow-xl relative overflow-hidden">
                <div class="relative z-10">
                    <span class="bg-black/40 text-yellow-300 text-[10px] font-bold px-2.5 py-0.5 rounded-full uppercase tracking-wider backdrop-blur-md">Hot Promo</span>
                    <h2 class="text-xl font-black mt-1">Spin & Win Free Deals!</h2>
                    <p class="text-xs text-slate-100 mt-0.5">Instant discounts & promo vouchers waiting.</p>
                    <button onclick="openSpinWheel()" class="mt-3 bg-yellow-400 hover:bg-yellow-300 text-slate-950 text-xs font-black px-3.5 py-2 rounded-xl shadow-lg transition active:scale-95 flex items-center space-x-2">
                        <i class="fa-solid fa-dharmachakra animate-spin text-sm"></i>
                        <span>Spin Magic Wheel Now</span>
                    </button>
                </div>
                <i class="fa-solid fa-pizza-slice text-white/10 text-9xl absolute -right-6 -bottom-8"></i>
            </div>

            <!-- Quick Service Buttons -->
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

            <!-- Category Filters -->
            <div class="flex space-x-2 overflow-x-auto pb-1 scrollbar-none">
                <button onclick="filterMenu('all')" class="bg-red-600 text-white text-xs px-3.5 py-2 rounded-xl font-bold shadow-md shadow-red-600/30 whitespace-nowrap">🔥 All Menu</button>
                <button onclick="filterMenu('deals')" class="glass text-slate-300 text-xs px-3.5 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800">🎉 Deals & Combos</button>
                <button onclick="filterMenu('pizzas')" class="glass text-slate-300 text-xs px-3.5 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800">🍕 Pizzas</button>
                <button onclick="filterMenu('burgers')" class="glass text-slate-300 text-xs px-3.5 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800">🍔 Burgers & Fast Food</button>
            </div>

            <!-- Product Items List with Images -->
            <div class="space-y-3" id="product-list-container">
                
                <!-- Combo Deal 1 -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition relative">
                    <button onclick="toggleWishlist('Family Feast Combo', this)" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 text-sm z-10 bg-slate-900/60 p-1.5 rounded-full"><i class="fa-regular fa-heart"></i></button>
                    <img src="https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow">
                    <div class="flex-1">
                        <div class="flex justify-between items-start pr-6">
                            <h4 class="font-bold text-white text-sm">Family Feast Combo</h4>
                            <span class="text-[10px] bg-red-500/20 text-red-400 px-1.5 py-0.5 rounded font-bold">Mega Deal</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">2 Large Pizzas + 1.5L Drink + Garlic Bread</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none">
                            <option value="Standard Deal" data-price="3800">Standard Package - Rs. 3800</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. 3800</span>
                            <button onclick="addToCart('Family Feast Combo', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Pizza 1 -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition relative">
                    <button onclick="toggleWishlist('Chicken Fajita Special', this)" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 text-sm z-10 bg-slate-900/60 p-1.5 rounded-full"><i class="fa-regular fa-heart"></i></button>
                    <img src="https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow">
                    <div class="flex-1">
                        <div class="flex justify-between items-start pr-6">
                            <h4 class="font-bold text-white text-sm">Chicken Fajita Special</h4>
                            <span class="text-[10px] bg-green-500/20 text-green-400 px-1.5 py-0.5 rounded font-bold">Hot</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Spicy chicken, onions, capsicum, mozzarella</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none">
                            <option value="Small" data-price="1200">Small (10") - Rs. 1200</option>
                            <option value="Medium" data-price="1700" selected>Medium (13") - Rs. 1700</option>
                            <option value="Large" data-price="2200">Large (16") - Rs. 2200</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. 1700</span>
                            <button onclick="addToCart('Chicken Fajita Special', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Fast Food 1: Zinger Burger -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition relative">
                    <button onclick="toggleWishlist('Crispy Zinger Burger', this)" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 text-sm z-10 bg-slate-900/60 p-1.5 rounded-full"><i class="fa-regular fa-heart"></i></button>
                    <img src="https://images.unsplash.com/photo-1568901346375-23c9450c58cd?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow">
                    <div class="flex-1">
                        <div class="flex justify-between items-start pr-6">
                            <h4 class="font-bold text-white text-sm">Crispy Zinger Burger</h4>
                            <span class="text-[10px] bg-yellow-500/20 text-yellow-400 px-1.5 py-0.5 rounded font-bold">Popular</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Crispy fried chicken fillet, mayo, cheese</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none">
                            <option value="Single Burger" data-price="550">Single Burger - Rs. 550</option>
                            <option value="Zinger Meal (+Fries & Drink)" data-price="850">Zinger Meal - Rs. 850</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. 550</span>
                            <button onclick="addToCart('Crispy Zinger Burger', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Fast Food 2: Chicken Wrap -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition relative">
                    <button onclick="toggleWishlist('Grilled Chicken Wrap', this)" class="absolute top-2 right-2 text-slate-400 hover:text-red-500 text-sm z-10 bg-slate-900/60 p-1.5 rounded-full"><i class="fa-regular fa-heart"></i></button>
                    <img src="https://images.unsplash.com/photo-1626700051175-6818013e1d4f?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow">
                    <div class="flex-1">
                        <div class="flex justify-between items-start pr-6">
                            <h4 class="font-bold text-white text-sm">Grilled Chicken Wrap</h4>
                            <span class="text-[10px] bg-blue-500/20 text-blue-400 px-1.5 py-0.5 rounded font-bold">New</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Tortilla wrap, grilled chicken strips, spicy sauce</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none">
                            <option value="Standard Wrap" data-price="480">Standard Wrap - Rs. 480</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. 480</span>
                            <button onclick="addToCart('Grilled Chicken Wrap', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 flex items-center space-x-1">
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
                    <div class="bg-orange-600/20 p-2 rounded-xl text-orange-400"><i class="fa-solid fa-wand-magic-sparkles text-lg"></i></div>
                    <div>
                        <h3 class="font-bold text-white text-sm">Half-&-Half Custom Pizza</h3>
                        <p class="text-[11px] text-slate-400">Mix 2 flavors in 1 single pizza!</p>
                    </div>
                </div>
                
                <div class="space-y-3">
                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase">Left Half Flavor:</label>
                        <select id="half-left" class="w-full border border-slate-700 bg-slate-800 text-slate-200 rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Chicken Fajita">Chicken Fajita</option>
                            <option value="Super Supreme">Super Supreme</option>
                            <option value="BBQ Tikka">BBQ Tikka</option>
                        </select>
                    </div>

                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase">Right Half Flavor:</label>
                        <select id="half-right" class="w-full border border-slate-700 bg-slate-800 text-slate-200 rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Veggie Supreme">Veggie Supreme</option>
                            <option value="Cheese Lover">Cheese Lover</option>
                            <option value="Pepperoni Passion">Pepperoni Passion</option>
                        </select>
                    </div>

                    <div>
                        <label class="text-[10px] font-bold text-slate-400 uppercase">Crust Type:</label>
                        <select id="build-dough" class="w-full border border-slate-700 bg-slate-800 text-slate-200 rounded-xl p-2.5 text-xs mt-1 focus:outline-none">
                            <option value="Regular Crust (Rs. 1800)">Medium Regular Crust - Rs. 1800</option>
                            <option value="Cheese Stuffed Crust (Rs. 2100)">Medium Stuffed Crust - Rs. 2100</option>
                        </select>
                    </div>

                    <button onclick="addHalfPizza()" class="w-full bg-red-600 hover:bg-red-500 text-white font-bold py-3 rounded-xl text-xs shadow-lg shadow-red-600/30 mt-2">
                        <i class="fa-solid fa-cart-plus mr-1"></i> Add Half-&-Half to Cart
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 3: CART & CHECKOUT -->
        <div id="screen-cart" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <h3 class="font-bold text-white text-sm mb-3 flex items-center justify-between">
                    <span>🛒 Your Cart</span>
                    <span id="item-count-badge" class="text-[10px] bg-red-600 text-white px-2 py-0.5 rounded-full font-bold">0 Items</span>
                </h3>
                <div id="cart-items" class="divide-y divide-slate-800 text-xs mb-4 max-h-48 overflow-y-auto">
                    <p class="text-slate-500 text-center py-6">Your cart is empty.</p>
                </div>

                <div class="flex space-x-2 mb-3">
                    <input type="text" id="coupon-code" placeholder="Promo (CRUST10)" class="border border-slate-700 bg-slate-800 text-white rounded-xl px-3 py-2 text-xs w-full uppercase focus:outline-none">
                    <button onclick="applyCoupon()" class="bg-slate-700 hover:bg-slate-600 text-white px-4 py-2 rounded-xl text-xs font-bold">Apply</button>
                </div>

                <div class="border-t border-slate-800 pt-3 text-xs space-y-1.5 mb-4">
                    <div class="flex justify-between text-slate-400"><span>Subtotal:</span><span id="subtotal" class="text-white font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-slate-400"><span>Delivery Fee:</span><span id="delivery-fee" class="text-yellow-400 font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-slate-400"><span>Discount:</span><span id="discount" class="text-green-400 font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-sm font-black text-white border-t border-slate-800 pt-2"><span>Grand Total:</span><span id="grand-total" class="text-red-500">Rs. 0</span></div>
                </div>

                <div class="space-y-2.5">
                    <h4 class="font-bold text-[11px] text-slate-400 uppercase">Delivery Details</h4>
                    <input type="text" id="cust-name" placeholder="Full Name" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs focus:outline-none">
                    <input type="text" id="cust-phone" placeholder="Active Phone Number" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs focus:outline-none">
                    <textarea id="cust-address" placeholder="Delivery Address" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs focus:outline-none" rows="2"></textarea>
                    
                    <button onclick="checkoutWhatsApp()" class="w-full bg-green-600 hover:bg-green-500 text-white font-bold py-3.5 rounded-xl shadow-xl shadow-green-600/30 flex items-center justify-center space-x-2 text-xs">
                        <i class="fa-brands fa-whatsapp text-lg"></i>
                        <span>Confirm Order via WhatsApp</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 4: TRACKING -->
        <div id="screen-tracking" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <h3 class="font-bold text-white text-sm mb-1 flex items-center space-x-2">
                    <i class="fa-solid fa-location-crosshairs text-red-500"></i>
                    <span>Live Order Tracking</span>
                </h3>
                <div class="flex space-x-2 my-3">
                    <input type="text" id="track-id" placeholder="Enter Order ID" class="border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs w-full focus:outline-none">
                    <button onclick="trackOrder()" class="bg-red-600 hover:bg-red-500 text-white px-4 py-2.5 rounded-xl text-xs font-bold">Track</button>
                </div>
                <div id="tracking-result" class="hidden space-y-3 bg-slate-900/80 p-3 rounded-xl border border-slate-800 text-xs">
                    <div class="flex items-center space-x-3 text-green-400"><i class="fa-solid fa-circle-check"></i><span class="font-bold">Order Received</span></div>
                    <div class="flex items-center space-x-3 text-yellow-400"><i class="fa-solid fa-fire-burner animate-spin"></i><span class="font-bold">Preparing in Kitchen</span></div>
                </div>
            </div>
        </div>

        <!-- SCREEN 5: ADMIN -->
        <div id="screen-admin" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800 space-y-3">
                <h3 class="font-bold text-white text-sm">📊 Kitchen Dashboard</h3>
                <div id="admin-orders-list" class="space-y-2 text-xs"></div>
            </div>
        </div>

        <div id="screen-table" class="app-screen space-y-4"><div class="glass p-4 rounded-2xl text-center text-xs">Table Reservation Active</div></div>
        <div id="screen-refer" class="app-screen space-y-4"><div class="glass p-4 rounded-2xl text-center text-xs">Referral Program Active</div></div>

    </main>

    <!-- MODERN SPIN WHEEL MODAL WITH LABELS -->
    <div id="spin-modal" class="fixed inset-0 bg-black/85 z-50 flex items-center justify-center hidden p-4 backdrop-blur-sm">
        <div class="glass p-6 rounded-3xl border border-slate-700 text-center max-w-xs w-full space-y-4 relative">
            <h3 class="font-black text-lg text-white">SPIN & WIN DEALS</h3>
            <p class="text-[11px] text-slate-300">Tap below to spin for instant discount coupons!</p>
            
            <!-- Dynamic Wheel Graphic with Slice Labels -->
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
                        <div class="w-10 h-10 bg-slate-900 border-2 border-yellow-400 rounded-full flex items-center justify-center z-20 shadow-lg">
                            <i class="fa-solid fa-star text-yellow-400 text-xs"></i>
                        </div>
                    </div>
                </div>
            </div>

            <div id="spin-result" class="text-yellow-300 font-bold text-sm min-h-[20px]"></div>

            <button onclick="spinModernWheel()" id="spin-btn" class="w-full bg-gradient-to-r from-yellow-400 to-amber-500 text-slate-950 font-black py-3 rounded-2xl text-xs shadow-xl active:scale-95 transition">
                SPIN WHEEL NOW
            </button>
            <button onclick="document.getElementById('spin-modal').classList.add('hidden')" class="text-slate-400 text-xs hover:text-white underline">Close</button>
        </div>
    </div>

    <!-- Bottom Navigation -->
    <nav class="glass bottom-nav border-t border-slate-800 px-4 py-2.5 shadow-2xl">
        <button onclick="switchTab('home')" id="nav-home" class="text-red-500 flex flex-col items-center text-[10px] font-bold flex-1">
            <i class="fa-solid fa-house text-base"></i><span>Menu</span>
        </button>
        <button onclick="switchTab('builder')" id="nav-builder" class="text-slate-400 flex flex-col items-center text-[10px] font-medium flex-1">
            <i class="fa-solid fa-wand-magic-sparkles text-base"></i><span>Builder</span>
        </button>
        <button onclick="switchTab('cart')" id="nav-cart" class="text-slate-400 flex flex-col items-center text-[10px] font-medium relative flex-1">
            <i class="fa-solid fa-cart-shopping text-base"></i><span>Cart</span>
            <span id="nav-badge" class="absolute -top-1 right-5 bg-red-600 text-white text-[9px] px-1.5 py-0.2 rounded-full font-bold">0</span>
        </button>
        <button onclick="switchTab('tracking')" id="nav-tracking" class="text-slate-400 flex flex-col items-center text-[10px] font-medium flex-1">
            <i class="fa-solid fa-location-crosshairs text-base"></i><span>Track</span>
        </button>
    </nav>

    <!-- JavaScript Logic -->
    <script>
        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                splash.style.opacity = '0';
                setTimeout(() => splash.remove(), 500);
            }, 1200);
        });

        let cart = [];
        let discountRate = 0;
        let deliveryFee = 0;
        let isSpinning = false;

        function showToast(message) {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = `bg-green-600 text-white text-xs font-bold px-4 py-3 rounded-2xl shadow-xl flex items-center justify-between transition transform translate-y-2 opacity-0`;
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

        function openSpinWheel() {
            document.getElementById('spin-modal').classList.remove('hidden');
        }

        function spinModernWheel() {
            if(isSpinning) return;
            isSpinning = true;
            const wheel = document.getElementById('modern-wheel');
            const res = document.getElementById('spin-result');
            const btn = document.getElementById('spin-btn');
            
            btn.disabled = true;
            res.innerText = "Spinning...";

            // Rotation angle targeted specifically for 15% OFF slice
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
                showToast('Fill all details & add items!');
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
        function toggleWishlist(name, btn) { showToast(`Saved ${name} to Wishlist!`); }
        function filterMenu(cat) { showToast(`Showing: ${cat.toUpperCase()}`); }
    </script>
</body>
</html>
