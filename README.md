<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crust Pizza - Official Mobile App</title>
    <!-- Tailwind CSS v4 -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .app-screen { display: none; opacity: 0; transition: opacity 0.3s ease-in-out; }
        .app-screen.active { display: block; opacity: 1; }
        body { background-color: #0f172a; color: #f8fafc; padding-bottom: 95px; margin: 0; }
        .glass { background: rgba(30, 41, 59, 0.85); backdrop-filter: blur(14px); }
        @keyframes pulse-slow { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.04); } }
        .animate-pulse-slow { animation: pulse-slow 3s infinite; }
        
        /* Splash Screen Animation */
        #splash-screen {
            position: fixed; inset: 0; background: #0f172a; z-index: 99999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 0.5s ease;
        }

        /* Fixed Horizontal Bottom Navbar */
        .bottom-nav {
            position: fixed; bottom: 0; left: 0; right: 0;
            max-width: 28rem; margin: 0 auto;
            display: flex; flex-direction: row; justify-content: space-around; align-items: center;
            z-index: 9999;
        }

        /* Spin Wheel Animation */
        @keyframes spinWheel {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(1800deg); }
        }
        .spinning { animation: spinWheel 4s cubic-bezier(0.15, 0.9, 0.2, 1) forwards; }
    </style>
</head>
<body class="font-sans antialiased max-w-md mx-auto min-h-screen relative shadow-2xl overflow-x-hidden border-x border-slate-800">

    <div id="splash-screen">
        <div class="bg-red-600 p-4 rounded-3xl text-white shadow-2xl shadow-red-600/50 animate-bounce mb-4">
            <i class="fa-solid fa-pizza-slice text-4xl"></i>
        </div>
        <h1 class="font-black text-2xl tracking-wider text-white">CRUST PIZZA</h1>
        <p class="text-xs text-yellow-400 font-semibold mt-1 tracking-widest uppercase">Astore & Eidgah Official App</p>
        <div class="w-32 h-1 bg-slate-800 rounded-full mt-6 overflow-hidden">
            <div id="loader-bar" class="w-full h-full bg-red-600 origin-left animate-pulse"></div>
        </div>
    </div>

    <div id="wheel-modal" class="fixed inset-0 bg-black/80 z-[10000] hidden items-center justify-center p-4 backdrop-blur-md">
        <div class="glass p-6 rounded-3xl border border-slate-700 w-full max-w-xs text-center relative shadow-2xl">
            <button onclick="toggleWheelModal(false)" class="absolute top-3 right-3 text-slate-400 hover:text-white text-sm bg-slate-800 p-1.5 rounded-full"><i class="fa-solid fa-xmark"></i></button>
            <div class="bg-yellow-500/20 text-yellow-400 w-12 h-12 rounded-2xl flex items-center justify-center mx-auto mb-3">
                <i class="fa-solid fa-gift text-xl animate-bounce"></i>
            </div>
            <h3 class="font-black text-lg text-white">Spin & Win Discount!</h3>
            <p class="text-[11px] text-slate-400 mt-1 mb-4">Test your luck to win instant coupon rewards for your pizza order!</p>
            
            <div class="relative w-48 h-48 mx-auto mb-4 bg-slate-900 rounded-full border-4 border-yellow-500/50 flex items-center justify-center overflow-hidden shadow-inner">
                <div id="wheel-inner" class="absolute inset-0 flex items-center justify-center font-black text-xs text-white transition-all duration-4000">
                    <div class="absolute top-2 text-yellow-400 font-bold">15% OFF</div>
                    <div class="absolute right-2 text-red-400 font-bold">Free Delivery</div>
                    <div class="absolute bottom-2 text-green-400 font-bold">Rs. 200 OFF</div>
                    <div class="absolute left-2 text-purple-400 font-bold">Free Drink</div>
                </div>
                <div class="absolute top-0 w-2 h-6 bg-red-600 rounded-b shadow-md z-10"></div>
                <div class="w-10 h-10 bg-slate-800 rounded-full border-2 border-yellow-400 z-10 flex items-center justify-center text-[10px] font-black text-yellow-400">SPIN</div>
            </div>

            <button id="spin-btn" onclick="spinTheWheel()" class="w-full bg-gradient-to-r from-yellow-500 to-amber-600 hover:from-yellow-400 hover:to-amber-500 text-slate-950 font-black py-3 rounded-xl text-xs shadow-lg shadow-yellow-500/30 transition active:scale-95">Spin Wheel Now 🎡</button>
            <p id="wheel-result-msg" class="text-xs font-bold text-green-400 mt-3 h-4"></p>
        </div>
    </div>

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
            <button onclick="toggleWheelModal(true)" class="bg-yellow-500/20 hover:bg-yellow-500/30 text-yellow-400 text-xs px-2.5 py-1.5 rounded-xl font-bold border border-yellow-500/30 flex items-center space-x-1 transition active:scale-95">
                <i class="fa-solid fa-dice text-xs"></i>
                <span>Win Gift</span>
            </button>
            <button onclick="switchTab('admin')" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-2.5 py-1.5 rounded-xl font-bold border border-slate-700 flex items-center space-x-1 transition active:scale-95">
                <i class="fa-solid fa-gauge text-yellow-400"></i>
                <span>Admin</span>
            </button>
        </div>
    </header>

    <main class="p-4 space-y-4">

        <!-- SCREEN 1: HOME / MENU -->
        <div id="screen-home" class="app-screen active space-y-4">
            <!-- Promo Banner -->
            <div class="bg-gradient-to-r from-red-600 to-orange-600 text-white p-4 rounded-2xl shadow-xl relative overflow-hidden">
                <div class="relative z-10">
                    <span class="bg-black/30 text-yellow-300 text-[10px] font-bold px-2 py-0.5 rounded-full uppercase tracking-wider">Special Offer</span>
                    <h2 class="text-xl font-black mt-1">Get 10% OFF Today!</h2>
                    <p class="text-xs text-slate-100 mt-0.5">Use promo code <span class="font-bold underline text-yellow-300">CRUST10</span> at checkout or spin the wheel!</p>
                </div>
                <i class="fa-solid fa-fire text-white/10 text-8xl absolute -right-4 -bottom-6"></i>
            </div>

            <!-- Categories Filter -->
            <div class="flex space-x-3 overflow-x-auto pb-1 scrollbar-none">
                <button onclick="filterMenu('all', this)" class="category-btn bg-red-600 text-white text-xs px-4 py-2 rounded-xl font-bold shadow-md shadow-red-600/30 whitespace-nowrap transition active:scale-95">🔥 All Pizzas</button>
                <button onclick="filterMenu('deals', this)" class="category-btn glass text-slate-300 text-xs px-4 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800 hover:text-white transition">🎉 Combo Deals</button>
                <button onclick="filterMenu('burgers', this)" class="category-btn glass text-slate-300 text-xs px-4 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800 hover:text-white transition">🍔 Burgers & FastFood</button>
                <button onclick="filterMenu('favorites', this)" class="category-btn glass text-slate-300 text-xs px-4 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800 hover:text-white transition">❤️ My Wishlist</button>
            </div>

            <!-- Product List with Live Price Calculator & Wishlist -->
            <div class="space-y-3" id="product-list-container">
                <!-- Item 1 -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition product-card" data-category="pizzas" data-name="Chicken Fajita Special">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow" onerror="this.src='https://placehold.co/200x200/1e293b/ef4444?text=Pizza'">
                        <button onclick="toggleWishlist('Chicken Fajita Special', this)" class="absolute top-1 right-1 bg-black/60 text-slate-300 hover:text-red-500 w-6 h-6 rounded-full flex items-center justify-center text-xs backdrop-blur-sm transition wishlist-btn">
                            <i class="fa-regular fa-heart"></i>
                        </button>
                    </div>
                    <div class="flex-1">
                        <div class="flex justify-between items-start">
                            <h4 class="font-bold text-white text-sm">Chicken Fajita Special</h4>
                            <span class="text-[10px] bg-green-500/20 text-green-400 px-1.5 py-0.5 rounded font-bold">Hot</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Spicy chicken, onions, capsicum, extra mozzarella</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none" onchange="updateItemPrice(this)">
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
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition product-card" data-category="pizzas" data-name="Super Supreme Feast">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow" onerror="this.src='https://placehold.co/200x200/1e293b/ef4444?text=Pizza'">
                        <button onclick="toggleWishlist('Super Supreme Feast', this)" class="absolute top-1 right-1 bg-black/60 text-slate-300 hover:text-red-500 w-6 h-6 rounded-full flex items-center justify-center text-xs backdrop-blur-sm transition wishlist-btn">
                            <i class="fa-regular fa-heart"></i>
                        </button>
                    </div>
                    <div class="flex-1">
                        <div class="flex justify-between items-start">
                            <h4 class="font-bold text-white text-sm">Super Supreme Feast</h4>
                            <span class="text-[10px] bg-yellow-500/20 text-yellow-400 px-1.5 py-0.5 rounded font-bold">Best Seller</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Smoked beef, pepperoni, mushrooms, black olives</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none" onchange="updateItemPrice(this)">
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

                <!-- Item 3 -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition product-card" data-category="pizzas" data-name="Crown Crust Pizza">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1574071318508-1cdbab80d002?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow" onerror="this.src='https://placehold.co/200x200/1e293b/ef4444?text=Pizza'">
                        <button onclick="toggleWishlist('Crown Crust Pizza', this)" class="absolute top-1 right-1 bg-black/60 text-slate-300 hover:text-red-500 w-6 h-6 rounded-full flex items-center justify-center text-xs backdrop-blur-sm transition wishlist-btn">
                            <i class="fa-regular fa-heart"></i>
                        </button>
                    </div>
                    <div class="flex-1">
                        <div class="flex justify-between items-start">
                            <h4 class="font-bold text-white text-sm">Crown Crust Pizza</h4>
                            <span class="text-[10px] bg-purple-500/20 text-purple-400 px-1.5 py-0.5 rounded font-bold">Chef Choice</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">Kabab-filled crust edge, rich herb sauce</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none" onchange="updateItemPrice(this)">
                            <option value="Small" data-price="1400">Small (10") - Rs. 1400</option>
                            <option value="Medium" data-price="1950" selected>Medium (13") - Rs. 1950</option>
                            <option value="Large" data-price="2550">Large (16") - Rs. 2550</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. 1950</span>
                            <button onclick="addToCart('Crown Crust Pizza', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 transition active:scale-95 flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Item 4 (Combo Deal) -->
                <div class="glass p-3 rounded-2xl border border-slate-800 flex space-x-3 items-center hover:border-red-600/50 transition product-card" data-category="deals" data-name="Family Deal 1 (2 Medium Pizzas)">
                    <div class="relative">
                        <img src="https://images.unsplash.com/photo-1541592106381-b31e9677c0e5?auto=format&fit=crop&w=300&q=80" class="w-20 h-20 object-cover rounded-xl shadow" onerror="this.src='https://placehold.co/200x200/1e293b/ef4444?text=Deal'">
                        <button onclick="toggleWishlist('Family Deal 1 (2 Medium Pizzas)', this)" class="absolute top-1 right-1 bg-black/60 text-slate-300 hover:text-red-500 w-6 h-6 rounded-full flex items-center justify-center text-xs backdrop-blur-sm transition wishlist-btn">
                            <i class="fa-regular fa-heart"></i>
                        </button>
                    </div>
                    <div class="flex-1">
                        <div class="flex justify-between items-start">
                            <h4 class="font-bold text-white text-sm">Family Deal 1 (2 Medium Pizzas)</h4>
                            <span class="text-[10px] bg-red-500/20 text-red-400 px-1.5 py-0.5 rounded font-bold">Save 20%</span>
                        </div>
                        <p class="text-[11px] text-slate-400 mt-0.5">2 Medium Pizzas + 1.5L Drink + Garlic Bread</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-800 text-slate-200 rounded-lg mt-1 p-1 w-full focus:outline-none" onchange="updateItemPrice(this)">
                            <option value="Standard Deal" data-price="2999" selected>Standard Deal - Rs. 2999</option>
                        </select>
                        <div class="flex justify-between items-center mt-2">
                            <span class="font-black text-red-500 text-sm item-price">Rs. 2999</span>
                            <button onclick="addToCart('Family Deal 1 (2 Medium Pizzas)', this)" class="bg-red-600 hover:bg-red-500 text-white text-xs px-3.5 py-1.5 rounded-xl font-bold shadow-lg shadow-red-600/30 transition active:scale-95 flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-[10px]"></i><span>Add</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Customer Reviews Carousel Section -->
            <div class="pt-2">
                <div class="flex justify-between items-center mb-2">
                    <h3 class="font-bold text-white text-xs uppercase tracking-wider flex items-center space-x-1.5">
                        <i class="fa-solid fa-star text-yellow-400"></i><span>Customer Reviews & Ratings</span>
                    </h3>
                    <div class="flex space-x-1">
                        <button onclick="scrollReviews(-1)" class="bg-slate-800 hover:bg-slate-700 text-slate-300 w-6 h-6 rounded-lg text-xs flex items-center justify-center"><i class="fa-solid fa-chevron-left"></i></button>
                        <button onclick="scrollReviews(1)" class="bg-slate-800 hover:bg-slate-700 text-slate-300 w-6 h-6 rounded-lg text-xs flex items-center justify-center"><i class="fa-solid fa-chevron-right"></i></button>
                    </div>
                </div>
                <div id="reviews-container" class="flex space-x-3 overflow-x-auto pb-2 scrollbar-none snap-x">
                    <!-- Review 1 -->
                    <div class="glass p-3 rounded-2xl border border-slate-800 min-w-[240px] snap-start">
                        <div class="flex items-center space-x-2 mb-1.5">
                            <div class="w-7 h-7 bg-red-600/30 text-red-400 rounded-full flex items-center justify-center font-bold text-xs">AH</div>
                            <div>
                                <h5 class="font-bold text-white text-xs">Ali Hassan (Astore)</h5>
                                <div class="text-[9px] text-yellow-400"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i></div>
                            </div>
                        </div>
                        <p class="text-[11px] text-slate-300 italic">"Crust Pizza has the most delicious taste in Astore! Hot and fresh delivery right on time."</p>
                    </div>
                    <!-- Review 2 -->
                    <div class="glass p-3 rounded-2xl border border-slate-800 min-w-[240px] snap-start">
                        <div class="flex items-center space-x-2 mb-1.5">
                            <div class="w-7 h-7 bg-yellow-600/30 text-yellow-400 rounded-full flex items-center justify-center font-bold text-xs">FK</div>
                            <div>
                                <h5 class="font-bold text-white text-xs">Fatima Khan (Eidgah)</h5>
                                <div class="text-[9px] text-yellow-400"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i></div>
                            </div>
                        </div>
                        <p class="text-[11px] text-slate-300 italic">"Crown Crust pizza was amazing. Love the kabab stuffed edges! Highly recommended app."</p>
                    </div>
                    <!-- Review 3 -->
                    <div class="glass p-3 rounded-2xl border border-slate-800 min-w-[240px] snap-start">
                        <div class="flex items-center space-x-2 mb-1.5">
                            <div class="w-7 h-7 bg-green-600/30 text-green-400 rounded-full flex items-center justify-center font-bold text-xs">MA</div>
                            <div>
                                <h5 class="font-bold text-white text-xs">Muhammad Abbas</h5>
                                <div class="text-[9px] text-yellow-400"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star-half-stroke"></i></div>
                            </div>
                        </div>
                        <p class="text-[11px] text-slate-300 italic">"Fast delivery and great customer support. The spin wheel coupon gave me a nice discount too!"</p>
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
                        <p class="text-[11px] text-slate-400">Build your dream pizza flavor & toppings</p>
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
                    <input type="text" id="coupon-code" placeholder="Promo code (CRUST10)" class="border border-slate-700 bg-slate-800 text-white rounded-xl px-3 py-2 text-xs w-full uppercase focus:outline-none">
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

        <!-- SCREEN 4: LIVE TRACKING WITH COUNTDOWN TIMER -->
        <div id="screen-tracking" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <h3 class="font-bold text-white text-sm mb-1 flex items-center space-x-2">
                    <i class="fa-solid fa-location-crosshairs text-red-500"></i>
                    <span>Live Order Tracking</span>
                </h3>
                <p class="text-[11px] text-slate-400 mb-3">Monitor live kitchen baking & delivery progress.</p>
                
                <div class="flex space-x-2 mb-4">
                    <input type="text" id="track-id" placeholder="Enter Order ID (e.g. #102)" class="border border-slate-700 bg-slate-800 text-white rounded-xl p-2.5 text-xs w-full focus:outline-none">
                    <button onclick="trackOrder()" class="bg-red-600 hover:bg-red-500 text-white px-4 py-2.5 rounded-xl text-xs font-bold transition active:scale-95">Track</button>
                </div>

                <!-- Live Order Countdown Timer Box -->
                <div id="tracking-result" class="hidden space-y-3 bg-slate-900/90 p-4 rounded-xl border border-slate-800 text-xs">
                    <div class="bg-red-600/20 border border-red-600/40 p-3 rounded-xl text-center">
                        <span class="text-[10px] text-red-400 font-bold uppercase tracking-widest">Estimated Delivery Countdown</span>
                        <div id="countdown-timer" class="text-2xl font-black text-white mt-1">35:00</div>
                    </div>
                    <div class="space-y-2 pt-1">
                        <div class="flex items-center space-x-3 text-green-400">
                            <i class="fa-solid fa-circle-check text-base"></i>
                            <span class="font-bold">Order Received & Confirmed by Branch</span>
                        </div>
                        <div class="flex items-center space-x-3 text-yellow-400">
                            <i class="fa-solid fa-fire-burner text-base animate-spin"></i>
                            <span class="font-bold">Preparing & Baking in Stone Oven</span>
                        </div>
                        <div class="flex items-center space-x-3 text-slate-500">
                            <i class="fa-solid fa-motorcycle text-base"></i>
                            <span>Out for Fast Delivery (Astore)</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- SCREEN 5: ADMIN PANEL -->
        <div id="screen-admin" class="app-screen space-y-4">
            <div class="glass p-4 rounded-2xl border border-slate-800">
                <div class="flex justify-between items-center mb-4 border-b border-slate-800 pb-3">
                    <div>
                        <h3 class="font-bold text-white text-sm">📊 Admin Back-End Panel</h3>
                        <span class="text-[10px] text-green-400 font-bold">● System Active & Syncing</span>
                    </div>
                    <button onclick="simulateAlarm()" class="bg-red-600 hover:bg-red-500 text-white text-[10px] px-3 py-1.5 rounded-xl font-bold animate-pulse shadow-lg shadow-red-600/30 transition active:scale-95">🔔 Test Alarm</button>
                </div>

                <!-- Analytics Cards -->
                <div class="grid grid-cols-2 gap-3 mb-4">
                    <div class="bg-slate-900 p-3 rounded-xl border border-slate-800 text-center">
                        <span class="text-[10px] text-slate-400">Today Orders</span>
                        <h4 class="text-xl font-black text-yellow-400 mt-0.5">34</h4>
                    </div>
                    <div class="bg-slate-900 p-3 rounded-xl border border-slate-800 text-center">
                        <span class="text-[10px] text-slate-400">Weekly Sales</span>
                        <h4 class="text-xl font-black text-green-400 mt-0.5">Rs. 192K</h4>
                    </div>
                </div>

                <!-- Add New Product Form -->
                <div class="bg-slate-900 p-3.5 rounded-xl border border-slate-800 space-y-2.5">
                    <h4 class="font-bold text-xs text-yellow-300">Add New Pizza / Deal</h4>
                    <input type="text" id="admin-p-name" placeholder="Item Name (e.g. Cheese Crust)" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2 text-xs focus:outline-none">
                    <input type="number" id="admin-p-price" placeholder="Base Price (Rs)" class="w-full border border-slate-700 bg-slate-800 text-white rounded-xl p-2 text-xs focus:outline-none">
                    <button onclick="alert('Success! New product added to live menu database.')" class="w-full bg-yellow-400 hover:bg-yellow-300 text-slate-950 font-bold py-2 rounded-xl text-xs transition active:scale-95">Save Product</button>
                </div>
            </div>
        </div>

    </main>

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

    <!-- Footer Branding -->
    <footer class="text-center text-[10px] text-slate-500 pb-24 pt-4">
        <p>&copy; 2026 Crust Pizza Astore & Eidgah.</p>
        <p class="text-yellow-400/80 font-semibold mt-0.5">Designed & Developed by Prime Solutions 🚀</p>
    </footer>

    <script>
        // Splash screen hide after 1.2 seconds
        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                splash.style.opacity = '0';
                setTimeout(() => splash.remove(), 500);
            }, 1200);
        });

        let cart = [];
        let wishlist = [];
        let discountRate = 0;
        let countdownInterval = null;

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

        // Feature 3: Interactive Live Price Calculator
        function updateItemPrice(selectEl) {
            const price = selectEl.options[selectEl.selectedIndex].getAttribute('data-price');
            selectEl.closest('.flex-1').querySelector('.item-price').innerText = `Rs. ${price}`;
        }

        function addToCart(name, btn) {
            const container = btn.closest('.flex-1');
            const sizeSelect = container.querySelector('.size-select');
            const size = sizeSelect ? sizeSelect.value : 'Standard';
            const price = sizeSelect ? parseInt(sizeSelect.options[sizeSelect.selectedIndex].getAttribute('data-price')) : 1500;

            cart.push({ name, size, price });
            updateCartUI();
            
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

        // Feature 4: Spin the Wheel Modal & Rewards
        function toggleWheelModal(show) {
            const modal = document.getElementById('wheel-modal');
            if(show) {
                modal.classList.remove('hidden');
                modal.classList.add('flex');
            } else {
                modal.classList.add('hidden');
                modal.classList.remove('flex');
            }
        }

        function spinTheWheel() {
            const wheel = document.getElementById('wheel-inner');
            const btn = document.getElementById('spin-btn');
            const msg = document.getElementById('wheel-result-msg');
            
            btn.disabled = true;
            wheel.classList.add('spinning');

            setTimeout(() => {
                wheel.classList.remove('spinning');
                discountRate = 0.15; // 15% OFF reward
                updateCartUI();
                msg.innerText = "🎉 Congratulations! You won 15% OFF coupon (CRUST15)! Applied to Cart.";
                btn.disabled = false;
                setTimeout(() => toggleWheelModal(false), 2500);
            }, 4000);
        }

        function applyCoupon() {
            const code = document.getElementById('coupon-code').value.trim().toUpperCase();
            if(code === 'CRUST10') {
                discountRate = 0.10;
                updateCartUI();
                alert('Success! 10% Discount Coupon Applied.');
            } else if(code === 'CRUST15') {
                discountRate = 0.15;
                updateCartUI();
                alert('Success! 15% Wheel Reward Coupon Applied.');
            } else {
                alert('Invalid Code! Try using CRUST10 or Spin the Wheel.');
            }
        }

        // Feature 6: Favorite / Wishlist Heart Icon Toggle
        function toggleWishlist(itemName, btn) {
            const icon = btn.querySelector('i');
            const index = wishlist.indexOf(itemName);
            
            if(index > -1) {
                wishlist.splice(index, 1);
                icon.className = "fa-regular fa-heart";
                btn.classList.remove('text-red-500');
            } else {
                wishlist.push(itemName);
                icon.className = "fa-solid fa-heart text-red-500";
                btn.classList.add('text-red-500');
            }
        }

        function filterMenu(category, btn) {
            document.querySelectorAll('.category-btn').forEach(b => {
                b.className = "category-btn glass text-slate-300 text-xs px-4 py-2 rounded-xl font-medium whitespace-nowrap border border-slate-800 hover:text-white transition";
            });
            if(btn) {
                btn.className = "category-btn bg-red-600 text-white text-xs px-4 py-2 rounded-xl font-bold shadow-md shadow-red-600/30 whitespace-nowrap transition active:scale-95";
            }

            const cards = document.querySelectorAll('.product-card');
            cards.forEach(card => {
                if(category === 'all') {
                    card.style.display = 'flex';
                } else if(category === 'favorites') {
                    if(wishlist.includes(card.getAttribute('data-name'))) {
                        card.style.display = 'flex';
                    } else {
                        card.style.display = 'none';
                    }
                } else {
                    if(card.getAttribute('data-category') === category) {
                        card.style.display = 'flex';
                    } else {
                        card.style.display = 'none';
                    }
                }
            });
        }

        // Reviews Carousel Scroll
        function scrollReviews(direction) {
            const container = document.getElementById('reviews-container');
            container.scrollBy({ left: direction * 260, behavior: 'smooth' });
        }

        // Feature 2: Live Order Timer & Audio Alert
        function checkoutWhatsApp() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value;
            const branch = document.getElementById('branch-select').value;
            const payment = document.getElementById('payment-method').value;

            if(!name || !phone || !address || cart.length === 0) {
                alert('Please fill in all delivery details and add items to cart!');
                return;
            }

            simulateAlarm(); // Play kitchen audio alert upon checkout

            let msg = `*New Order - Crust Pizza (${branch})*%0A%0A*Name:* ${name}%0A*Phone:* ${phone}%0A*Address:* ${address}%0A*Payment:* ${payment}%0A%0A*Items:*%0A`;
            let sub = 0;
            cart.forEach(i => { msg += `- ${i.name} (${i.size}): Rs. ${i.price}%0A`; sub += i.price; });
            let final = sub - (sub * discountRate);
            msg += `%0A*Total Bill:* Rs. ${final} (ETA: 35 mins)%0A%0A_Powered by Prime Solutions_`;

            // Switch to tracking and start countdown
            document.getElementById('track-id').value = '#' + Math.floor(100 + Math.random() * 900);
            switchTab('tracking');
            trackOrder();

            window.open(`https://wa.me/923001234567?text=${msg}`, '_blank');
        }

        function trackOrder() {
            const id = document.getElementById('track-id').value.trim();
            const res = document.getElementById('tracking-result');
            if(id) {
                res.classList.remove('hidden');
                startCountdown(35 * 60); // 35 minutes countdown
            } else {
                alert('Please enter a valid order ID');
            }
        }

        function startCountdown(durationSeconds) {
            if(countdownInterval) clearInterval(countdownInterval);
            let timer = durationSeconds;
            const display = document.getElementById('countdown-timer');
            
            countdownInterval = setInterval(() => {
                let minutes = parseInt(timer / 60, 10);
                let seconds = parseInt(timer % 60, 10);

                minutes = minutes < 10 ? "0" + minutes : minutes;
                seconds = seconds < 10 ? "0" + seconds : seconds;

                display.innerText = `${minutes}:${seconds}`;

                if (--timer < 0) {
                    clearInterval(countdownInterval);
                    display.innerText = "Delivered Enjoy! 🍕";
                }
            }, 1000);
        }

        function simulateAlarm() {
            try {
                const audio = new Audio('https://www.soundjay.com/buttons/sounds/beep-07.mp3');
                audio.play().catch(e => console.log('Audio restricted by browser policy'));
            } catch(e) {}
        }
    </script>
</body>
</html>
