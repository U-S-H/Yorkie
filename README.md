<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crust Pizza - Online Delivery</title>
    <!-- Google Fonts & FontAwesome Icons -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        :root {
            --primary: #ff3838;
            --primary-dark: #d63031;
            --accent: #ff9f1a;
            --bg: #f8f9fa;
            --surface: #ffffff;
            --text-dark: #2d3436;
            --text-muted: #636e72;
            --radius: 18px;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg);
            color: var(--text-dark);
            padding-bottom: 85px;
            overflow-x: hidden;
        }

        /* 1. App Splash Screen Loader */
        #splash-screen {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100vh;
            background: linear-gradient(135deg, #ff3838, #ff9f1a);
            z-index: 9999;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            transition: opacity 0.5s ease, visibility 0.5s;
        }

        .splash-icon {
            font-size: 4rem;
            animation: bounce 1.2s infinite ease-in-out;
        }

        .splash-title {
            font-size: 2rem;
            font-weight: 800;
            margin-top: 15px;
            letter-spacing: 1px;
        }

        .splash-sub {
            font-size: 0.9rem;
            opacity: 0.9;
            margin-top: 5px;
        }

        .loader-bar {
            width: 150px;
            height: 4px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 10px;
            margin-top: 25px;
            overflow: hidden;
            position: relative;
        }

        .loader-progress {
            width: 0%;
            height: 100%;
            background: white;
            animation: loadProgress 2s forwards;
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-15px) scale(1.1); }
        }

        @keyframes loadProgress {
            0% { width: 0%; }
            100% { width: 100%; }
        }

        /* Header App Bar */
        .app-header {
            background: var(--surface);
            position: sticky;
            top: 0;
            z-index: 100;
            padding: 12px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 4px 15px rgba(0,0,0,0.03);
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 10px;
            font-weight: 800;
            font-size: 1.3rem;
            color: var(--primary);
        }

        .header-actions {
            display: flex;
            gap: 10px;
        }

        .icon-btn {
            position: relative;
            background: var(--bg);
            border: none;
            width: 42px;
            height: 42px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 1.1rem;
            color: var(--text-dark);
        }

        .badge {
            position: absolute;
            top: -2px; right: -2px;
            background: var(--primary);
            color: white;
            font-size: 0.7rem;
            font-weight: 700;
            width: 18px; height: 18px;
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
        }

        /* Promo Banner */
        .promo-banner {
            margin: 15px 20px;
            background: linear-gradient(135deg, #1e272e, #2f3542);
            border-radius: var(--radius);
            padding: 18px 20px;
            color: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .spin-trigger-btn {
            background: var(--accent);
            color: white;
            border: none;
            padding: 8px 14px;
            border-radius: 20px;
            font-weight: 700;
            font-size: 0.8rem;
            cursor: pointer;
            box-shadow: 0 4px 10px rgba(255, 159, 26, 0.4);
        }

        /* Category Filter Tabs */
        .categories-container {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding: 5px 20px 15px;
            scrollbar-width: none;
        }
        .categories-container::-webkit-scrollbar { display: none; }

        .cat-chip {
            background: var(--surface);
            padding: 8px 18px;
            border-radius: 25px;
            font-size: 0.85rem;
            font-weight: 600;
            color: var(--text-muted);
            white-space: nowrap;
            border: 1px solid #e1e8ef;
            cursor: pointer;
        }

        .cat-chip.active {
            background: var(--primary);
            color: white;
            border-color: var(--primary);
            box-shadow: 0 4px 10px rgba(255, 56, 56, 0.3);
        }

        /* Products Grid */
        .section-title {
            padding: 0 20px;
            font-size: 1.1rem;
            font-weight: 700;
            margin-bottom: 12px;
        }

        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
            gap: 15px;
            padding: 0 20px;
        }

        .product-card {
            background: var(--surface);
            border-radius: var(--radius);
            padding: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.03);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .product-card img {
            width: 100%;
            height: 110px;
            object-fit: cover;
            border-radius: 12px;
            margin-bottom: 10px;
        }

        .product-title {
            font-size: 0.9rem;
            font-weight: 700;
            margin-bottom: 4px;
        }

        .product-desc {
            font-size: 0.75rem;
            color: var(--text-muted);
            margin-bottom: 10px;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
        }

        .product-footer {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .price {
            font-weight: 700;
            font-size: 0.95rem;
            color: var(--primary);
        }

        .add-btn {
            background: var(--primary);
            color: white;
            border: none;
            width: 32px; height: 32px;
            border-radius: 10px;
            display: flex; align-items: center; justify-content: center;
            cursor: pointer;
        }

        /* Modals & Drawers */
        .modal {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.6);
            z-index: 200;
            display: none;
            align-items: flex-end;
            justify-content: center;
        }

        .modal.active { display: flex; }

        .modal-content {
            background: var(--surface);
            width: 100%;
            max-width: 500px;
            border-radius: 24px 24px 0 0;
            padding: 20px;
            max-height: 88vh;
            overflow-y: auto;
            animation: slideUp 0.3s ease-out;
        }

        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #eee;
        }

        /* Spin Wheel Style */
        .wheel-container {
            text-align: center;
            padding: 10px 0;
        }

        .wheel-box {
            width: 220px; height: 220px;
            border-radius: 50%;
            border: 8px solid var(--accent);
            margin: 15px auto;
            position: relative;
            overflow: hidden;
            transition: transform 3s cubic-bezier(0.1, 0.7, 0.1, 1);
            background: conic-gradient(
                #ff3838 0deg 60deg,
                #ff9f1a 60deg 120deg,
                #2ed573 120deg 180deg,
                #1e90ff 180deg 240deg,
                #9b59b6 240deg 300deg,
                #e1b12c 300deg 360deg
            );
        }

        .wheel-pointer {
            width: 0; height: 0;
            border-left: 12px solid transparent;
            border-right: 12px solid transparent;
            border-top: 20px solid var(--text-dark);
            margin: 0 auto -10px;
            position: relative;
            z-index: 10;
        }

        /* Order Form Inputs */
        .delivery-toggle {
            display: flex;
            background: var(--bg);
            border-radius: 12px;
            padding: 4px;
            margin-bottom: 15px;
        }

        .toggle-btn {
            flex: 1;
            padding: 8px;
            border: none;
            border-radius: 10px;
            background: transparent;
            font-weight: 600;
            font-size: 0.85rem;
            cursor: pointer;
        }

        .toggle-btn.active {
            background: var(--surface);
            color: var(--primary);
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .form-control {
            width: 100%;
            padding: 12px;
            border-radius: 10px;
            border: 1px solid #e1e8ef;
            margin-bottom: 10px;
            font-size: 0.9rem;
            outline: none;
        }

        .checkout-btn {
            background: #25d366;
            color: white;
            border: none;
            width: 100%;
            padding: 14px;
            border-radius: 12px;
            font-weight: 700;
            font-size: 1rem;
            display: flex; align-items: center; justify-content: center;
            gap: 10px;
            cursor: pointer;
            margin-top: 10px;
        }

        /* Bottom Nav Bar */
        .bottom-nav {
            position: fixed;
            bottom: 0; left: 0; right: 0;
            background: var(--surface);
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            box-shadow: 0 -4px 15px rgba(0,0,0,0.05);
            z-index: 99;
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            font-size: 0.75rem;
            color: var(--text-muted);
            text-decoration: none;
            gap: 4px;
        }

        .nav-item.active { color: var(--primary); font-weight: 700; }
        .nav-item i { font-size: 1.2rem; }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .qty-controls {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .qty-btn {
            width: 26px; height: 26px;
            border-radius: 50%;
            border: 1px solid #ccc;
            background: var(--surface);
            font-weight: 600;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <!-- 1. App Splash Screen -->
    <div id="splash-screen">
        <i class="fa-solid fa-pizza-slice splash-icon"></i>
        <div class="splash-title">CRUST PIZZA</div>
        <div class="splash-sub">Hot, Fresh & Cheesy Delivery</div>
        <div class="loader-bar">
            <div class="loader-progress"></div>
        </div>
    </div>

    <!-- 2. Header App Bar -->
    <div class="app-header">
        <div class="brand">
            <i class="fa-solid fa-pizza-slice"></i>
            <span>Crust Pizza</span>
        </div>
        <div class="header-actions">
            <button class="icon-btn" onclick="openModal('spinModal')">
                <i class="fa-solid fa-gift" style="color:var(--accent);"></i>
            </button>
            <button class="icon-btn" onclick="openModal('cartModal')">
                <i class="fa-solid fa-basket-shopping"></i>
                <span class="badge" id="cartCount">0</span>
            </button>
        </div>
    </div>

    <!-- Promo Banner -->
    <div class="promo-banner">
        <div>
            <h4 style="font-size:1rem; margin-bottom: 2px;">Spin & Win Discount! 🎁</h4>
            <p style="font-size:0.75rem; opacity:0.8;">Try your luck to get free deals.</p>
        </div>
        <button class="spin-trigger-btn" onclick="openModal('spinModal')">Spin Now</button>
    </div>

    <!-- Category Filters -->
    <div class="categories-container">
        <div class="cat-chip active" onclick="filterCategory('all', this)">🔥 Deals</div>
        <div class="cat-chip" onclick="filterCategory('pizza', this)">🍕 Pizzas</div>
        <div class="cat-chip" onclick="filterCategory('burger', this)">🍔 Burgers</div>
        <div class="cat-chip" onclick="filterCategory('sides', this)">🍟 Sides</div>
        <div class="cat-chip" onclick="filterCategory('drinks', this)">🥤 Drinks</div>
    </div>

    <!-- Product Grid -->
    <h3 class="section-title">Menu & Online Ordering</h3>
    <div class="products-grid" id="productList">
        <!-- JS Dynamic Products -->
    </div>

    <!-- 3. Spin Wheel Modal -->
    <div class="modal" id="spinModal">
        <div class="modal-content wheel-container">
            <div class="modal-header">
                <h3>Spin & Win Lucky Wheel</h3>
                <i class="fa-solid fa-xmark" style="font-size:1.4rem; cursor:pointer;" onclick="closeModal('spinModal')"></i>
            </div>
            <p style="font-size:0.85rem; color:var(--text-muted);">Spin to win exclusive discounts on your order!</p>
            
            <div class="wheel-pointer"></div>
            <div class="wheel-box" id="wheel"></div>

            <button class="spin-trigger-btn" style="width:100%; padding:12px; font-size:1rem; margin-top:15px;" onclick="spinWheel()">SPIN THE WHEEL</button>
            <div id="wheelResult" style="margin-top:10px; font-weight:700; color:var(--primary);"></div>
        </div>
    </div>

    <!-- 4. Online Cart & Checkout Modal -->
    <div class="modal" id="cartModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Your Delivery Order</h3>
                <i class="fa-solid fa-xmark" style="font-size:1.4rem; cursor:pointer;" onclick="closeModal('cartModal')"></i>
            </div>

            <!-- Delivery vs Takeaway Switch -->
            <div class="delivery-toggle">
                <button class="toggle-btn active" id="btnDelivery" onclick="setOrderType('delivery')">🚴 Delivery (Rs. 100)</button>
                <button class="toggle-btn" id="btnTakeaway" onclick="setOrderType('takeaway')">🛍️ Takeaway</button>
            </div>

            <div id="cartItems">
                <p style="text-align: center; color: var(--text-muted); margin: 20px 0;">Your cart is empty!</p>
            </div>

            <hr style="margin: 15px 0; border: 0; border-top: 1px solid #eee;">
            
            <div style="font-size:0.9rem; margin-bottom:5px; display:flex; justify-content:space-between;">
                <span>Subtotal:</span>
                <span id="subTotal">Rs. 0</span>
            </div>
            <div style="font-size:0.9rem; margin-bottom:5px; display:flex; justify-content:space-between;">
                <span>Delivery Charge:</span>
                <span id="delCharge">Rs. 100</span>
            </div>
            <div style="font-size:1.1rem; font-weight:800; margin-bottom:15px; display:flex; justify-content:space-between; color:var(--primary);">
                <span>Total Amount:</span>
                <span id="cartTotal">Rs. 100</span>
            </div>

            <!-- Customer Details Form -->
            <input type="text" id="custName" class="form-control" placeholder="Full Name">
            <input type="tel" id="custPhone" class="form-control" placeholder="WhatsApp / Phone Number">
            <textarea id="custAddress" class="form-control" rows="2" placeholder="Complete Street / Home Address"></textarea>

            <button class="checkout-btn" onclick="sendOrder()">
                <i class="fa-brands fa-whatsapp" style="font-size:1.3rem;"></i> Place Order via WhatsApp
            </button>
        </div>
    </div>

    <!-- 5. Bottom Navigation Bar -->
    <div class="bottom-nav">
        <a href="#" class="nav-item active">
            <i class="fa-solid fa-house"></i>
            <span>Home</span>
        </a>
        <a href="javascript:void(0)" class="nav-item" onclick="openModal('cartModal')">
            <i class="fa-solid fa-utensils"></i>
            <span>Cart</span>
        </a>
        <a href="javascript:void(0)" class="nav-item" onclick="openModal('spinModal')">
            <i class="fa-solid fa-gift"></i>
            <span>Offers</span>
        </a>
        <a href="tel:03000000000" class="nav-item">
            <i class="fa-solid fa-phone"></i>
            <span>Call Shop</span>
        </a>
    </div>

    <script>
        // Splash Loader Hide Script
        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                splash.style.opacity = '0';
                splash.style.visibility = 'hidden';
            }, 2000); // 2 Seconds splash display
        });

        const products = [
            { id: 1, category: 'pizza', name: 'Crust Special Pizza', price: 1299, desc: 'Loaded chicken, olives & cheese crust', img: 'https://images.unsplash.com/photo-1534308983496-4fabb1a015ee?q=80&w=400' },
            { id: 2, category: 'pizza', name: 'Chicken Tikka Pizza', price: 1099, desc: 'Smokey tikka chunks & spicy peppers', img: 'https://images.unsplash.com/photo-1513104890138-7c749659a591?q=80&w=400' },
            { id: 3, category: 'burger', name: 'Crispy Zinger Burger', price: 450, desc: 'Crispy fried chicken thigh patty', img: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?q=80&w=400' },
            { id: 4, category: 'sides', name: 'Cheese Loaded Fries', price: 350, desc: 'Fries with melted cheddar sauce', img: 'https://images.unsplash.com/photo-1573080496219-bb080dd4f877?q=80&w=400' },
            { id: 5, category: 'drinks', name: 'Chilled Pepsi 1.5L', price: 180, desc: '1.5 Liter cold drink', img: 'https://images.unsplash.com/photo-1622483767028-3f66f32aef97?q=80&w=400' }
        ];

        let cart = {};
        let orderType = 'delivery';
        let deliveryFee = 100;
        let discountText = "";

        function renderProducts(items) {
            const container = document.getElementById('productList');
            container.innerHTML = '';
            items.forEach(p => {
                container.innerHTML += `
                    <div class="product-card">
                        <div>
                            <img src="${p.img}" alt="${p.name}">
                            <div class="product-title">${p.name}</div>
                            <div class="product-desc">${p.desc}</div>
                        </div>
                        <div class="product-footer">
                            <span class="price">Rs. ${p.price}</span>
                            <button class="add-btn" onclick="addToCart(${p.id})"><i class="fa-solid fa-plus"></i></button>
                        </div>
                    </div>
                `;
            });
        }

        function filterCategory(cat, element) {
            document.querySelectorAll('.cat-chip').forEach(el => el.classList.remove('active'));
            element.classList.add('active');
            if (cat === 'all') renderProducts(products);
            else renderProducts(products.filter(p => p.category === cat));
        }

        function addToCart(id) {
            if (cart[id]) cart[id].qty += 1;
            else {
                const prod = products.find(p => p.id === id);
                cart[id] = { ...prod, qty: 1 };
            }
            updateCartUI();
        }

        function updateQty(id, delta) {
            if (cart[id]) {
                cart[id].qty += delta;
                if (cart[id].qty <= 0) delete cart[id];
            }
            updateCartUI();
        }

        function setOrderType(type) {
            orderType = type;
            document.getElementById('btnDelivery').classList.toggle('active', type === 'delivery');
            document.getElementById('btnTakeaway').classList.toggle('active', type === 'takeaway');
            deliveryFee = (type === 'delivery') ? 100 : 0;
            document.getElementById('delCharge').innerText = `Rs. ${deliveryFee}`;
            updateCartUI();
        }

        function updateCartUI() {
            let totalItems = 0;
            let subtotal = 0;
            const cartItemsDiv = document.getElementById('cartItems');
            cartItemsDiv.innerHTML = '';

            const keys = Object.keys(cart);
            if(keys.length === 0) {
                cartItemsDiv.innerHTML = '<p style="text-align: center; color: var(--text-muted); margin: 20px 0;">Your cart is empty!</p>';
            }

            keys.forEach(id => {
                const item = cart[id];
                totalItems += item.qty;
                subtotal += item.price * item.qty;

                cartItemsDiv.innerHTML += `
                    <div class="cart-item">
                        <div>
                            <div style="font-weight:700; font-size:0.85rem;">${item.name}</div>
                            <div style="font-size:0.75rem; color:var(--text-muted);">Rs. ${item.price} x ${item.qty}</div>
                        </div>
                        <div class="qty-controls">
                            <button class="qty-btn" onclick="updateQty(${id}, -1)">-</button>
                            <span>${item.qty}</span>
                            <button class="qty-btn" onclick="updateQty(${id}, 1)">+</button>
                        </div>
                    </div>
                `;
            });

            const grandTotal = subtotal > 0 ? (subtotal + deliveryFee) : 0;
            document.getElementById('cartCount').innerText = totalItems;
            document.getElementById('subTotal').innerText = `Rs. ${subtotal}`;
            document.getElementById('cartTotal').innerText = `Rs. ${grandTotal}`;
        }

        function openModal(id) { document.getElementById(id).classList.add('active'); }
        function closeModal(id) { document.getElementById(id).classList.remove('active'); }

        // Spin Wheel Logic
        let spun = false;
        function spinWheel() {
            if(spun) {
                alert("You have already spun the wheel!");
                return;
            }
            const wheel = document.getElementById('wheel');
            const randomDegree = 1800 + Math.floor(Math.random() * 360);
            wheel.style.transform = `rotate(${randomDegree}deg)`;
            spun = true;

            setTimeout(() => {
                discountText = "10% OFF Discount Claimed!";
                document.getElementById('wheelResult').innerText = "🎉 Congratulations! You Won 10% OFF!";
            }, 3000);
        }

        // Send Order to WhatsApp
        function sendOrder() {
            const name = document.getElementById('custName').value.trim();
            const phone = document.getElementById('custPhone').value.trim();
            const address = document.getElementById('custAddress').value.trim();

            if (Object.keys(cart).length === 0) {
                alert('Please add items to cart first!');
                return;
            }
            if (!name || !phone || (orderType === 'delivery' && !address)) {
                alert('Please fill out all contact and address details!');
                return;
            }

            let itemDetails = '';
            let subtotal = 0;

            Object.values(cart).forEach(item => {
                itemDetails += `• ${item.name} x ${item.qty} = Rs. ${item.price * item.qty}%0A`;
                subtotal += item.price * item.qty;
            });

            const grandTotal = subtotal + deliveryFee;
            const shopNumber = "923000000000"; // Shop's WhatsApp Number

            const msg = `*NEW ONLINE ORDER - CRUST PIZZA*%0A%0A` +
                        `*Order Type:* ${orderType.toUpperCase()}%0A` +
                        `*Customer Name:* ${name}%0A` +
                        `*Phone:* ${phone}%0A` +
                        (orderType === 'delivery' ? `*Address:* ${address}%0A` : '') +
                        (discountText ? `*Offer Applied:* ${discountText}%0A` : '') +
                        `%0A*Items:*%0A${itemDetails}%0A` +
                        `*Subtotal:* Rs. ${subtotal}%0A` +
                        `*Delivery Charges:* Rs. ${deliveryFee}%0A` +
                        `*Total Bill:* Rs. ${grandTotal}`;

            window.open(`https://wa.me/${shopNumber}?text=${msg}`, '_blank');
        }

        // Initialize Products
        renderProducts(products);
    </script>
</body>
</html>
