<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crust Pizza App</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        :root {
            --primary: #ff3838;
            --primary-dark: #c51111;
            --accent: #ff9f1a;
            --bg: #f8f9fa;
            --surface: #ffffff;
            --text-dark: #1e272e;
            --text-muted: #808e9b;
            --radius: 16px;
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
            padding-bottom: 80px; /* Space for bottom nav */
        }

        /* Top App Header */
        .app-header {
            background: var(--surface);
            position: sticky;
            top: 0;
            z-index: 100;
            padding: 12px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 10px;
            font-weight: 700;
            font-size: 1.2rem;
            color: var(--primary);
        }

        .brand i {
            font-size: 1.4rem;
        }

        .cart-icon-btn {
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

        .cart-badge {
            position: absolute;
            top: -2px;
            right: -2px;
            background: var(--primary);
            color: white;
            font-size: 0.75rem;
            font-weight: 700;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* Hero / Banner */
        .promo-banner {
            margin: 15px 20px;
            background: linear-gradient(135deg, #ff3838, #ff9f1a);
            border-radius: var(--radius);
            padding: 20px;
            color: white;
            box-shadow: 0 8px 20px rgba(255, 56, 56, 0.2);
        }

        .promo-banner h2 { font-size: 1.4rem; margin-bottom: 4px; }
        .promo-banner p { font-size: 0.85rem; opacity: 0.9; }

        /* Category Filter Tabs (Horizontal Scroll) */
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
            transition: 0.2s;
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
            font-size: 0.95rem;
            font-weight: 600;
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
            width: 32px;
            height: 32px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 0.9rem;
        }

        /* Cart Drawer Slide Up */
        .cart-modal {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.5);
            z-index: 200;
            display: none;
            align-items: flex-end;
        }

        .cart-modal.active { display: flex; }

        .cart-content {
            background: var(--surface);
            width: 100%;
            border-radius: 24px 24px 0 0;
            padding: 20px;
            max-height: 85vh;
            overflow-y: auto;
            animation: slideUp 0.3s ease-out;
        }

        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }

        .cart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            border-bottom: 1px solid #eee;
            padding-bottom: 10px;
        }

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

        /* Form Inputs */
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
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            cursor: pointer;
            margin-top: 15px;
        }

        /* Bottom Navigation Bar */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0; right: 0;
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

        .nav-item.active { color: var(--primary); font-weight: 600; }
        .nav-item i { font-size: 1.2rem; }

    </style>
</head>
<body>

    <!-- App Bar -->
    <div class="app-header">
        <div class="brand">
            <i class="fa-solid fa-pizza-slice"></i>
            <span>Crust Pizza</span>
        </div>
        <button class="cart-icon-btn" onclick="toggleCart()">
            <i class="fa-solid fa-basket-shopping"></i>
            <span class="cart-badge" id="cartCount">0</span>
        </button>
    </div>

    <!-- Promo Banner -->
    <div class="promo-banner">
        <h2>20% OFF Your First Order!</h2>
        <p>Use Code: <b>CRUST20</b> at WhatsApp checkout.</p>
    </div>

    <!-- Categories Tab Switcher -->
    <div class="categories-container">
        <div class="cat-chip active" onclick="filterCategory('all', this)">🔥 Deals</div>
        <div class="cat-chip" onclick="filterCategory('pizza', this)">🍕 Pizzas</div>
        <div class="cat-chip" onclick="filterCategory('burger', this)">🍔 Burgers</div>
        <div class="cat-chip" onclick="filterCategory('sides', this)">🍟 Fries & Sides</div>
        <div class="cat-chip" onclick="filterCategory('drinks', this)">🥤 Drinks</div>
    </div>

    <!-- Products List -->
    <h3 class="section-title">Popular Items</h3>
    <div class="products-grid" id="productList">
        <!-- Products generated dynamically via JS -->
    </div>

    <!-- Cart Drawer Modal -->
    <div class="cart-modal" id="cartModal">
        <div class="cart-content">
            <div class="cart-header">
                <h3>Your Cart</h3>
                <i class="fa-solid fa-xmark" style="font-size:1.4rem; cursor:pointer;" onclick="toggleCart()"></i>
            </div>
            
            <div id="cartItems">
                <p style="text-align: center; color: var(--text-muted); margin: 20px 0;">Cart is empty!</p>
            </div>

            <hr style="margin: 15px 0; border: 0; border-top: 1px solid #eee;">
            
            <div style="display: flex; justify-content: space-between; font-weight: 700; margin-bottom: 15px;">
                <span>Total Amount:</span>
                <span id="cartTotal">Rs. 0</span>
            </div>

            <!-- Customer Details -->
            <input type="text" id="custName" class="form-control" placeholder="Your Name">
            <input type="tel" id="custPhone" class="form-control" placeholder="Mobile Number">
            <textarea id="custAddress" class="form-control" rows="2" placeholder="Full Delivery Address"></textarea>

            <button class="checkout-btn" onclick="sendOrder()">
                <i class="fa-brands fa-whatsapp" style="font-size:1.3rem;"></i> Confirm Order via WhatsApp
            </button>
        </div>
    </div>

    <!-- Bottom Navigation Bar -->
    <div class="bottom-nav">
        <a href="#" class="nav-item active">
            <i class="fa-solid fa-house"></i>
            <span>Home</span>
        </a>
        <a href="javascript:void(0)" class="nav-item" onclick="toggleCart()">
            <i class="fa-solid fa-utensils"></i>
            <span>Cart</span>
        </a>
        <a href="tel:03000000000" class="nav-item">
            <i class="fa-solid fa-phone"></i>
            <span>Call Us</span>
        </a>
    </div>

    <!-- App Logic JS -->
    <script>
        const products = [
            { id: 1, category: 'pizza', name: 'Crust Special Pizza', price: 1299, desc: 'Loaded chicken, olives, bell peppers', img: 'https://images.unsplash.com/photo-1534308983496-4fabb1a015ee?q=80&w=300' },
            { id: 2, category: 'pizza', name: 'Chicken Tikka Pizza', price: 1099, desc: 'Smokey chicken tikka & spicy toppings', img: 'https://images.unsplash.com/photo-1513104890138-7c749659a591?q=80&w=300' },
            { id: 3, category: 'burger', name: 'Crispy Zinger', price: 450, desc: 'Crunchy chicken thigh patty & mayo', img: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?q=80&w=300' },
            { id: 4, category: 'sides', name: 'Cheese Loaded Fries', price: 350, desc: 'Fries topped with melted cheddar', img: 'https://images.unsplash.com/photo-1573080496219-bb080dd4f877?q=80&w=300' },
            { id: 5, category: 'drinks', name: 'Cold Drink 1.5L', price: 180, desc: 'Chilled Pepsi / Seven Up', img: 'https://images.unsplash.com/photo-1622483767028-3f66f32aef97?q=80&w=300' }
        ];

        let cart = {};

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
            
            if (cat === 'all') {
                renderProducts(products);
            } else {
                const filtered = products.filter(p => p.category === cat);
                renderProducts(filtered);
            }
        }

        function addToCart(id) {
            if (cart[id]) {
                cart[id].qty += 1;
            } else {
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

        function updateCartUI() {
            let totalItems = 0;
            let totalPrice = 0;
            const cartItemsDiv = document.getElementById('cartItems');
            cartItemsDiv.innerHTML = '';

            const keys = Object.keys(cart);
            if(keys.length === 0) {
                cartItemsDiv.innerHTML = '<p style="text-align: center; color: var(--text-muted); margin: 20px 0;">Cart is empty!</p>';
            }

            keys.forEach(id => {
                const item = cart[id];
                totalItems += item.qty;
                totalPrice += item.price * item.qty;

                cartItemsDiv.innerHTML += `
                    <div class="cart-item">
                        <div>
                            <div style="font-weight:600; font-size:0.9rem;">${item.name}</div>
                            <div style="font-size:0.8rem; color:var(--text-muted);">Rs. ${item.price} x ${item.qty}</div>
                        </div>
                        <div class="qty-controls">
                            <button class="qty-btn" onclick="updateQty(${id}, -1)">-</button>
                            <span>${item.qty}</span>
                            <button class="qty-btn" onclick="updateQty(${id}, 1)">+</button>
                        </div>
                    </div>
                `;
            });

            document.getElementById('cartCount').innerText = totalItems;
            document.getElementById('cartTotal').innerText = `Rs. ${totalPrice}`;
        }

        function toggleCart() {
            document.getElementById('cartModal').classList.toggle('active');
        }

        function sendOrder() {
            const name = document.getElementById('custName').value.trim();
            const phone = document.getElementById('custPhone').value.trim();
            const address = document.getElementById('custAddress').value.trim();

            if (Object.keys(cart).length === 0) {
                alert('Please add items to cart first!');
                return;
            }
            if (!name || !phone || !address) {
                alert('Please fill out name, phone, and address!');
                return;
            }

            let itemDetails = '';
            let total = 0;

            Object.values(cart).forEach(item => {
                itemDetails += `• ${item.name} x ${item.qty} = Rs. ${item.price * item.qty}%0A`;
                total += item.price * item.qty;
            });

            const shopNumber = "923000000000"; // Replace with client's WhatsApp Number
            const msg = `*NEW ONLINE ORDER - CRUST PIZZA*%0A%0A` +
                        `*Customer:* ${name}%0A` +
                        `*Phone:* ${phone}%0A` +
                        `*Address:* ${address}%0A%0A` +
                        `*Items Ordered:*%0A${itemDetails}%0A` +
                        `*Total Bill:* Rs. ${total}`;

            window.open(`https://wa.me/${shopNumber}?text=${msg}`, '_blank');
        }

        // Initialize Products
        renderProducts(products);
    </script>
</body>
</html>
