<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crust Pizza - Ultimate Super App</title>
    <!-- Tailwind CSS Standard CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: { red: '#ef4444', gold: '#f59e0b', dark: '#0b0f19', cardDark: '#1e293b' }
                    }
                }
            }
        }
    </script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    <!-- Leaflet CSS for Live Tracking Map -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; transition: background-color 0.3s, color 0.3s; }
        .app-screen { display: none; opacity: 0; transition: opacity 0.3s ease-in-out; }
        .app-screen.active { display: block; opacity: 1; }
        
        .dark .glass-card {
            background: linear-gradient(135deg, rgba(30, 41, 59, 0.75), rgba(15, 23, 42, 0.85));
            backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }
        .light .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(16px);
            border: 1px solid rgba(0, 0, 0, 0.08);
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.06);
        }

        .dark .glass-nav { background: rgba(11, 15, 25, 0.85); backdrop-filter: blur(20px); border-top: 1px solid rgba(255, 255, 255, 0.1); }
        .light .glass-nav { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(20px); border-top: 1px solid rgba(0, 0, 0, 0.1); }

        .glow-red { box-shadow: 0 0 20px rgba(239, 68, 68, 0.35); }
        .gradient-hot { background: linear-gradient(135deg, #ff416c, #ff4b2b); }
        .gradient-gold { background: linear-gradient(135deg, #f59e0b, #d97706); }

        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        #map { height: 180px; width: 100%; border-radius: 1rem; z-index: 1; }

        /* Scratch Card Canvas */
        #scratch-canvas { cursor: pointer; touch-action: none; }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 dark:bg-slate-950 dark:text-slate-100 light:bg-slate-50 light:text-slate-900 max-w-md mx-auto min-h-screen relative border-x border-slate-800/80 shadow-2xl pb-24">

    <!-- Toast Notifications -->
    <div id="toast-container" class="fixed top-5 left-1/2 -translate-x-1/2 z-50 w-11/12 max-w-sm space-y-2 pointer-events-none"></div>

    <!-- App Header -->
    <header class="sticky top-0 z-40 px-4 py-3.5 flex justify-between items-center glass-nav border-b border-slate-800">
        <div class="flex items-center space-x-3" onclick="switchTab('home')">
            <div class="bg-gradient-to-tr from-red-600 to-orange-500 p-2.5 rounded-2xl text-white glow-red">
                <i class="fa-solid fa-pizza-slice text-lg"></i>
            </div>
            <div>
                <h1 class="font-black text-lg tracking-wide text-white dark:text-white light:text-slate-900 leading-tight">CRUST PIZZA</h1>
                <div class="flex items-center space-x-1 text-yellow-400 text-[11px] font-bold">
                    <i class="fa-solid fa-location-dot text-[10px]"></i>
                    <select id="branch-select" onchange="updateDeliveryCharges()" class="bg-transparent border-none text-yellow-400 font-bold focus:outline-none cursor-pointer">
                        <option value="Astore Main" class="bg-slate-900 text-white">Astore Main Branch</option>
                        <option value="Eidgah Branch" class="bg-slate-900 text-white">Astore Eidgah (+Rs. 100)</option>
                    </select>
                </div>
            </div>
        </div>
        <div class="flex items-center space-x-2">
            <!-- Loyalty Coins Widget -->
            <div class="bg-amber-500/10 border border-amber-500/30 text-yellow-400 px-2.5 py-1.5 rounded-xl text-xs font-black flex items-center space-x-1">
                <i class="fa-solid fa-coins text-amber-400"></i>
                <span id="user-coins">120</span>
            </div>
            <!-- Theme Toggle -->
            <button onclick="toggleTheme()" class="bg-slate-800/80 text-slate-300 p-2 rounded-xl text-xs border border-slate-700">
                <i id="theme-icon" class="fa-solid fa-sun text-yellow-400"></i>
            </button>
        </div>
    </header>

    <!-- Main Content -->
    <main class="p-4 space-y-4">

        <!-- SCREEN 1: HOME -->
        <div id="screen-home" class="app-screen active space-y-4">

            <!-- AI Voice Assistant & Quick Reorder Bar -->
            <div class="glass-card p-3 rounded-2xl border border-slate-800 flex justify-between items-center space-x-2">
                <div class="flex items-center space-x-2.5 flex-1">
                    <button onclick="startVoiceAssistant()" class="bg-red-600 hover:bg-red-500 text-white p-2.5 rounded-xl glow-red active:scale-95 transition">
                        <i class="fa-solid fa-microphone text-sm"></i>
                    </button>
                    <div>
                        <div class="text-[11px] font-black text-white dark:text-white light:text-slate-800">Voice Assistant</div>
                        <div class="text-[9px] text-slate-400" id="voice-status">Tap mic & speak your order...</div>
                    </div>
                </div>
                <button onclick="quickReorder()" class="bg-amber-500/10 border border-amber-500/30 text-amber-400 hover:bg-amber-500/20 text-[10px] font-black px-3 py-2 rounded-xl flex items-center space-x-1 active:scale-95 transition">
                    <i class="fa-solid fa-rotate-right"></i>
                    <span>Re-Order</span>
                </button>
            </div>

            <!-- Daily Scratch Reward Banner -->
            <div class="glass-card p-4 rounded-3xl border border-yellow-500/30 flex items-center justify-between">
                <div>
                    <span class="text-[9px] bg-yellow-500/20 text-yellow-400 border border-yellow-500/30 px-2 py-0.5 rounded-full font-black">DAILY BONUS</span>
                    <h3 class="text-xs font-black text-white mt-1">Scratch & Win Coins!</h3>
                    <p class="text-[10px] text-slate-400">Claim free Crust Coins daily</p>
                </div>
                <button onclick="openScratchModal()" class="gradient-gold text-slate-950 font-black text-xs px-3.5 py-2 rounded-xl shadow-lg active:scale-95">Scratch Now</button>
            </div>

            <!-- AI Smart Recommendation -->
            <div class="glass-card p-4 rounded-3xl border border-amber-500/30 relative overflow-hidden">
                <div class="flex items-center justify-between mb-2">
                    <div class="flex items-center space-x-2">
                        <div class="bg-amber-400/10 p-2 rounded-xl text-yellow-400"><i class="fa-solid fa-wand-magic-sparkles"></i></div>
                        <div>
                            <h3 class="text-xs font-black text-white uppercase tracking-wider">AI Pizza Suggestion</h3>
                            <p class="text-[10px] text-slate-400">Best match for your budget</p>
                        </div>
                    </div>
                </div>
                <div class="flex space-x-2 mt-3">
                    <select id="ai-budget" class="bg-slate-900 text-slate-200 border border-slate-700/80 rounded-2xl text-xs p-3 flex-1 focus:outline-none">
                        <option value="1500">Under Rs. 1500</option>
                        <option value="2000" selected>Under Rs. 2000</option>
                    </select>
                    <button onclick="getAIRecommendation()" class="gradient-gold text-slate-950 font-black px-4 rounded-2xl text-xs flex items-center space-x-1 shadow-lg active:scale-95">
                        <i class="fa-solid fa-bolt"></i><span>Suggest</span>
                    </button>
                </div>
                <div id="ai-result" class="hidden mt-3 pt-3 border-t border-slate-800 text-xs text-yellow-300 font-bold flex justify-between items-center">
                    <span id="ai-text"></span>
                    <button id="ai-add-btn" onclick="addToCart('Chicken Fajita Special', this)" class="bg-red-600 text-white px-3 py-1.5 rounded-xl text-[10px] font-bold">Add Now</button>
                </div>
            </div>

            <!-- Menu List -->
            <div class="space-y-3.5" id="product-list-container">
                <div class="glass-card p-3.5 rounded-3xl border border-slate-800 flex space-x-3.5 items-center">
                    <img src="https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=300&q=80" class="w-24 h-24 object-cover rounded-2xl shadow-lg">
                    <div class="flex-1">
                        <h4 class="font-black text-white text-sm">Family Feast Combo</h4>
                        <p class="text-[11px] text-slate-400 mt-1">2 Large Pizzas + 1.5L Drink</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-900 text-slate-200 rounded-xl mt-2 p-1.5 w-full">
                            <option value="Standard Deal" data-price="3800">Standard - Rs. 3800</option>
                        </select>
                        <div class="flex justify-between items-center mt-3">
                            <span class="font-black text-red-500 text-base">Rs. 3800</span>
                            <button onclick="addToCart('Family Feast Combo', this)" class="gradient-hot text-white text-xs px-4 py-2 rounded-xl font-bold glow-red active:scale-95">Add</button>
                        </div>
                    </div>
                </div>

                <div class="glass-card p-3.5 rounded-3xl border border-slate-800 flex space-x-3.5 items-center">
                    <img src="https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=300&q=80" class="w-24 h-24 object-cover rounded-2xl shadow-lg">
                    <div class="flex-1">
                        <h4 class="font-black text-white text-sm">Chicken Fajita Special</h4>
                        <p class="text-[11px] text-slate-400 mt-1">Spicy chicken, capsicum, mozzarella</p>
                        <select class="size-select text-[11px] border border-slate-700 bg-slate-900 text-slate-200 rounded-xl mt-2 p-1.5 w-full">
                            <option value="Medium" data-price="1700">Medium - Rs. 1700</option>
                            <option value="Large" data-price="2200">Large - Rs. 2200</option>
                        </select>
                        <div class="flex justify-between items-center mt-3">
                            <span class="font-black text-red-500 text-base">Rs. 1700</span>
                            <button onclick="addToCart('Chicken Fajita Special', this)" class="gradient-hot text-white text-xs px-4 py-2 rounded-xl font-bold glow-red active:scale-95">Add</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- SCREEN 2: BUILDER -->
        <div id="screen-builder" class="app-screen space-y-4">
            <div class="glass-card p-5 rounded-3xl border border-slate-800 space-y-4">
                <h3 class="font-black text-white text-base">Half-&-Half Custom Builder</h3>
                <div class="space-y-3">
                    <div>
                        <label class="text-[10px] font-black text-amber-400 uppercase">Left Side Flavor</label>
                        <select id="half-left" class="w-full border border-slate-700 bg-slate-900 text-slate-200 rounded-2xl p-3 text-xs mt-1">
                            <option value="Chicken Fajita">Chicken Fajita</option>
                            <option value="BBQ Tikka">BBQ Tikka</option>
                        </select>
                    </div>
                    <div>
                        <label class="text-[10px] font-black text-amber-400 uppercase">Right Side Flavor</label>
                        <select id="half-right" class="w-full border border-slate-700 bg-slate-900 text-slate-200 rounded-2xl p-3 text-xs mt-1">
                            <option value="Veggie Supreme">Veggie Supreme</option>
                            <option value="Cheese Lover">Cheese Lover</option>
                        </select>
                    </div>
                    <button onclick="addHalfPizza()" class="w-full gradient-hot text-white font-black py-3.5 rounded-2xl text-xs glow-red mt-2 active:scale-95">
                        Add Custom Pizza To Cart
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 3: CART & CHECKOUT -->
        <div id="screen-cart" class="app-screen space-y-4">
            <div class="glass-card p-5 rounded-3xl border border-slate-800">
                <h3 class="font-black text-white text-base mb-4 flex items-center justify-between">
                    <span>🛒 Modern Checkout</span>
                    <span id="item-count-badge" class="text-[10px] bg-red-600 text-white px-2.5 py-1 rounded-full font-bold">0 Items</span>
                </h3>

                <div id="cart-items" class="divide-y divide-slate-800 text-xs mb-4 max-h-48 overflow-y-auto">
                    <p class="text-slate-500 text-center py-6">Your cart is empty.</p>
                </div>

                <!-- Schedule Order Feature -->
                <div class="p-3 bg-slate-900/80 rounded-2xl border border-slate-800 mb-4 space-y-2">
                    <div class="flex items-center space-x-2 text-xs font-bold text-amber-400">
                        <i class="fa-regular fa-clock"></i>
                        <span>Schedule Delivery Time</span>
                    </div>
                    <select id="order-schedule" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-2">
                        <option value="ASAP">Deliver ASAP (25-35 mins)</option>
                        <option value="Today Evening (7:00 PM)">Schedule for Today (7:00 PM)</option>
                        <option value="Today Evening (9:00 PM)">Schedule for Today (9:00 PM)</option>
                    </select>
                </div>

                <!-- Payment Method Selection -->
                <div class="p-3 bg-slate-900/80 rounded-2xl border border-slate-800 mb-4 space-y-2">
                    <div class="text-xs font-bold text-amber-400">Select Payment Method</div>
                    <div class="grid grid-cols-3 gap-2">
                        <button onclick="setPayment('COD')" id="pay-cod" class="pay-btn active bg-red-600 text-white text-[10px] font-bold py-2 rounded-xl border border-red-500">Cash on Delivery</button>
                        <button onclick="setPayment('JazzCash')" id="pay-jazz" class="pay-btn bg-slate-800 text-slate-300 text-[10px] font-bold py-2 rounded-xl border border-slate-700">JazzCash</button>
                        <button onclick="setPayment('EasyPaisa')" id="pay-easy" class="pay-btn bg-slate-800 text-slate-300 text-[10px] font-bold py-2 rounded-xl border border-slate-700">EasyPaisa</button>
                    </div>
                </div>

                <div class="border-t border-slate-800 pt-3 text-xs space-y-2 mb-4">
                    <div class="flex justify-between text-slate-400"><span>Subtotal:</span><span id="subtotal" class="text-white font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-slate-400"><span>Delivery Fee:</span><span id="delivery-fee" class="text-yellow-400 font-semibold">Rs. 0</span></div>
                    <div class="flex justify-between text-base font-black text-white border-t border-slate-800 pt-2"><span>Total Amount:</span><span id="grand-total" class="text-red-500">Rs. 0</span></div>
                </div>

                <div class="space-y-3">
                    <input type="text" id="cust-name" placeholder="Full Name" class="w-full border border-slate-700 bg-slate-900 text-white rounded-2xl p-3 text-xs">
                    <input type="text" id="cust-phone" placeholder="Active Phone Number" class="w-full border border-slate-700 bg-slate-900 text-white rounded-2xl p-3 text-xs">
                    <textarea id="cust-address" placeholder="Full Address" class="w-full border border-slate-700 bg-slate-900 text-white rounded-2xl p-3 text-xs" rows="2"></textarea>
                    
                    <button onclick="checkoutWhatsApp()" class="w-full bg-green-600 hover:bg-green-500 text-white font-black py-4 rounded-2xl shadow-xl flex items-center justify-center space-x-2 text-xs active:scale-95">
                        <i class="fa-brands fa-whatsapp text-lg"></i>
                        <span>Order via WhatsApp Now</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 4: LIVE TRACKING & MAP -->
        <div id="screen-tracking" class="app-screen space-y-4">
            <div class="glass-card p-5 rounded-3xl border border-slate-800 space-y-3">
                <h3 class="font-black text-white text-base flex items-center space-x-2">
                    <i class="fa-solid fa-location-crosshairs text-red-500 animate-pulse"></i>
                    <span>Live GPS Delivery Map</span>
                </h3>
                
                <!-- Map Container -->
                <div id="map"></div>

                <div class="space-y-2 text-xs bg-slate-950 p-3 rounded-2xl border border-slate-800">
                    <div class="flex justify-between items-center text-slate-300">
                        <span>Status:</span>
                        <span id="tracking-status-text" class="text-yellow-400 font-bold">Rider Dispatched</span>
                    </div>
                    <div class="flex justify-between items-center text-slate-300">
                        <span>Estimated Arrival:</span>
                        <span class="text-green-400 font-bold">12 Mins</span>
                    </div>
                </div>
            </div>
        </div>

    </main>

    <!-- SCRATCH CARD MODAL -->
    <div id="scratch-modal" class="fixed inset-0 bg-black/85 z-50 flex items-center justify-center hidden p-4 backdrop-blur-md">
        <div class="glass-card p-6 rounded-3xl border border-slate-700 text-center max-w-xs w-full space-y-4 relative">
            <h3 class="font-black text-lg text-white">Daily Rewards</h3>
            <p class="text-xs text-slate-300">Scratch the card below with your finger!</p>
            
            <div class="relative w-48 h-32 mx-auto rounded-2xl overflow-hidden shadow-2xl flex items-center justify-center bg-slate-900 border border-yellow-500/50">
                <div class="absolute inset-0 flex flex-col items-center justify-center text-yellow-400 font-black">
                    <i class="fa-solid fa-coins text-2xl mb-1"></i>
                    <span>+50 Crust Coins!</span>
                </div>
                <canvas id="scratch-canvas" width="200" height="130" class="absolute inset-0"></canvas>
            </div>

            <button onclick="closeScratchModal()" class="w-full gradient-gold text-slate-950 font-black py-3 rounded-xl text-xs">Collect & Close</button>
        </div>
    </div>

    <!-- Bottom Nav -->
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
            <i class="fa-solid fa-location-dot text-lg mb-1"></i><span>Live Map</span>
        </button>
    </nav>

    <!-- App JavaScript Logic -->
    <script>
        let cart = [];
        let coins = 120;
        let selectedPayment = 'COD';
        let map, riderMarker;

        // Theme Toggle
        function toggleTheme() {
            const html = document.documentElement;
            const icon = document.getElementById('theme-icon');
            if (html.classList.contains('dark')) {
                html.classList.remove('dark');
                html.classList.add('light');
                icon.className = 'fa-solid fa-moon text-slate-700';
            } else {
                html.classList.remove('light');
                html.classList.add('dark');
                icon.className = 'fa-solid fa-sun text-yellow-400';
            }
        }

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
            if(tabId === 'tracking') setTimeout(initMap, 200);
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
            cart.push({ name: `Half-&-Half Custom`, size: `${left} / ${right}`, price: 1800 });
            updateCartUI();
            showToast('Custom Pizza Added!');
            switchTab('cart');
        }

        function updateCartUI() {
            const container = document.getElementById('cart-items');
            document.getElementById('nav-badge').innerText = cart.length;
            document.getElementById('item-count-badge').innerText = `${cart.length} Items`;

            if(cart.length === 0) {
                container.innerHTML = '<p class="text-slate-500 text-center py-6">Your cart is empty.</p>';
                document.getElementById('subtotal').innerText = 'Rs. 0';
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
                        <button onclick="cart.splice(${index},1);updateCartUI()" class="text-red-400 font-bold text-xs"><i class="fa-solid fa-trash-can"></i></button>
                    </div>
                `;
            });

            document.getElementById('subtotal').innerText = `Rs. ${subtotal}`;
            document.getElementById('grand-total').innerText = `Rs. ${subtotal}`;
        }

        function setPayment(method) {
            selectedPayment = method;
            document.querySelectorAll('.pay-btn').forEach(btn => btn.className = 'pay-btn bg-slate-800 text-slate-300 text-[10px] font-bold py-2 rounded-xl border border-slate-700');
            if(method === 'COD') document.getElementById('pay-cod').className = 'pay-btn active bg-red-600 text-white text-[10px] font-bold py-2 rounded-xl border border-red-500';
            if(method === 'JazzCash') document.getElementById('pay-jazz').className = 'pay-btn active bg-red-600 text-white text-[10px] font-bold py-2 rounded-xl border border-red-500';
            if(method === 'EasyPaisa') document.getElementById('pay-easy').className = 'pay-btn active bg-red-600 text-white text-[10px] font-bold py-2 rounded-xl border border-red-500';
        }

        /* Voice Assistant Feature */
        function startVoiceAssistant() {
            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            const status = document.getElementById('voice-status');
            
            if (!SpeechRecognition) {
                showToast('Voice feature not supported in this browser.');
                return;
            }

            const recognition = new SpeechRecognition();
            status.innerText = "Listening... Speak now!";
            recognition.start();

            recognition.onresult = (event) => {
                const speech = event.results[0][0].transcript.toLowerCase();
                status.innerText = `You said: "${speech}"`;

                if (speech.includes('fajita') || speech.includes('pizza')) {
                    cart.push({ name: 'Chicken Fajita Special', size: 'Medium', price: 1700 });
                    updateCartUI();
                    showToast('Voice Order: Added Chicken Fajita Pizza!');
                } else {
                    showToast('Could not recognize item. Try again!');
                }
            };
        }

        /* Quick Re-Order Feature */
        function quickReorder() {
            cart.push({ name: 'Chicken Fajita Special', size: 'Medium', price: 1700 });
            updateCartUI();
            showToast('Re-Ordered Previous Meal!');
            switchTab('cart');
        }

        /* Scratch Card Canvas */
        function openScratchModal() {
            document.getElementById('scratch-modal').classList.remove('hidden');
            const canvas = document.getElementById('scratch-canvas');
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = '#64748b';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = '#334155';
            ctx.font = 'bold 12px sans-serif';
            ctx.fillText('Scratch Here!', 60, 70);

            let isScratching = false;
            canvas.onmousedown = canvas.ontouchstart = () => isScratching = true;
            canvas.onmouseup = canvas.ontouchend = () => isScratching = false;
            canvas.onmousemove = canvas.ontouchmove = (e) => {
                if (!isScratching) return;
                const rect = canvas.getBoundingClientRect();
                const x = (e.clientX || e.touches[0].clientX) - rect.left;
                const y = (e.clientY || e.touches[0].clientY) - rect.top;
                ctx.globalCompositeOperation = 'destination-out';
                ctx.beginPath();
                ctx.arc(x, y, 15, 0, Math.PI * 2);
                ctx.fill();
            };
        }

        function closeScratchModal() {
            coins += 50;
            document.getElementById('user-coins').innerText = coins;
            document.getElementById('scratch-modal').classList.add('hidden');
            showToast('50 Crust Coins Added!');
        }

        /* Live Leaflet Map Simulation */
        function initMap() {
            if (map) return;
            map = L.map('map').setView([35.1678, 74.8561], 14); // Astore Location Coords
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

            const riderIcon = L.divIcon({
                html: '<i class="fa-solid fa-motorcycle text-red-600 text-xl"></i>',
                className: 'custom-map-icon'
            });

            riderMarker = L.marker([35.1678, 74.8561], { icon: riderIcon }).addTo(map);
        }

        function checkoutWhatsApp() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value;
            const schedule = document.getElementById('order-schedule').value;

            if(!name || !phone || !address || cart.length === 0) {
                showToast('Please fill all details!');
                return;
            }

            let msg = `*New Order - Crust Pizza*%0A*Name:* ${name}%0A*Phone:* ${phone}%0A*Payment:* ${selectedPayment}%0A*Schedule:* ${schedule}%0A%0A*Items:*%0A`;
            cart.forEach(i => { msg += `- ${i.name} (${i.size}): Rs. ${i.price}%0A`; });

            window.open(`https://wa.me/923001234567?text=${msg}`, '_blank');
            cart = [];
            updateCartUI();
            switchTab('tracking');
        }

        function getAIRecommendation() {
            document.getElementById('ai-result').classList.remove('hidden');
            document.getElementById('ai-text').innerText = "Suggested: Chicken Fajita Medium (Rs. 1700)";
        }
    </script>
</body>
</html>
