<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tiny Paws Yorkies | Elegant Yorkshire Terriers</title>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #c59b27;
            --primary-dark: #a37c1d;
            --secondary: #2c221e;
            --light: #fdfbf7;
            --accent: #f4ece1;
            --text-dark: #333333;
            --text-light: #777777;
            --white: #ffffff;
            --transition: all 0.3s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--light);
            color: var(--text-dark);
            line-height: 1.6;
        }

        h1, h2, h3, h4 {
            font-family: 'Playfair Display', serif;
        }

        a {
            text-decoration: none;
            color: inherit;
        }

        ul {
            list-style: none;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }

        /* Header & Navigation */
        header {
            background-color: var(--white);
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            z-index: 1000;
        }

        .nav-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 80px;
        }

        .logo {
            font-family: 'Playfair Display', serif;
            font-size: 24px;
            font-weight: 700;
            color: var(--secondary);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo span {
            color: var(--primary);
        }

        .nav-links {
            display: flex;
            gap: 30px;
            align-items: center;
        }

        .nav-links a {
            font-weight: 500;
            color: var(--text-dark);
            transition: var(--transition);
        }

        .nav-links a:hover {
            color: var(--primary);
        }

        .btn {
            background-color: var(--primary);
            color: var(--white);
            padding: 10px 25px;
            border-radius: 30px;
            font-weight: 500;
            transition: var(--transition);
            border: none;
            cursor: pointer;
        }

        .btn:hover {
            background-color: var(--primary-dark);
            transform: translateY(-2px);
        }

        .hamburger {
            display: none;
            cursor: pointer;
            font-size: 24px;
        }

        /* Hero Section */
        .hero {
            padding: 160px 0 100px;
            background: linear-gradient(135deg, var(--accent) 0%, var(--light) 100%);
            min-height: 90vh;
            display: flex;
            align-items: center;
        }

        .hero-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 50px;
            align-items: center;
        }

        .hero-content h1 {
            font-size: 48px;
            color: var(--secondary);
            margin-bottom: 20px;
            line-height: 1.2;
        }

        .hero-content h1 span {
            color: var(--primary);
        }

        .hero-content p {
            color: var(--text-light);
            font-size: 16px;
            margin-bottom: 30px;
        }

        .hero-btns {
            display: flex;
            gap: 15px;
        }

        .btn-outline {
            background-color: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
            padding: 10px 25px;
            border-radius: 30px;
            font-weight: 500;
            transition: var(--transition);
        }

        .btn-outline:hover {
            background-color: var(--primary);
            color: var(--white);
        }

        .hero-image img {
            width: 100%;
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0,0,0,0.1);
            object-fit: cover;
            height: 450px;
        }

        /* About Section */
        .section-padding {
            padding: 100px 0;
        }

        .section-title {
            text-align: center;
            margin-bottom: 60px;
        }

        .section-title h2 {
            font-size: 36px;
            color: var(--secondary);
            margin-bottom: 15px;
        }

        .section-title p {
            color: var(--text-light);
            max-width: 600px;
            margin: 0 auto;
        }

        .about-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 30px;
        }

        .about-card {
            background: var(--white);
            padding: 40px 30px;
            border-radius: 15px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.03);
            text-align: center;
            transition: var(--transition);
        }

        .about-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.08);
        }

        .about-card i {
            font-size: 40px;
            color: var(--primary);
            margin-bottom: 20px;
        }

        .about-card h3 {
            font-size: 20px;
            margin-bottom: 15px;
            color: var(--secondary);
        }

        .about-card p {
            color: var(--text-light);
            font-size: 14px;
        }

        /* Gallery / Puppies Showcase */
        .gallery-section {
            background-color: var(--accent);
        }

        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 30px;
        }

        .gallery-item {
            background: var(--white);
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            transition: var(--transition);
        }

        .gallery-item:hover {
            transform: translateY(-5px);
        }

        .gallery-img {
            height: 250px;
            width: 100%;
            object-fit: cover;
        }

        .gallery-info {
            padding: 20px;
            text-align: center;
        }

        .gallery-info h3 {
            font-size: 18px;
            color: var(--secondary);
            margin-bottom: 5px;
        }

        .gallery-info p {
            color: var(--primary);
            font-weight: 600;
            font-size: 14px;
        }

        /* Care & Grooming Tips */
        .tips-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 40px;
            align-items: center;
        }

        .tips-list {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .tip-item {
            display: flex;
            gap: 20px;
            align-items: flex-start;
        }

        .tip-icon {
            background-color: var(--accent);
            color: var(--primary);
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
            font-size: 20px;
        }

        .tip-text h3 {
            font-size: 18px;
            color: var(--secondary);
            margin-bottom: 5px;
        }

        .tip-text p {
            color: var(--text-light);
            font-size: 14px;
        }

        .tips-image img {
            width: 100%;
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0,0,0,0.1);
        }

        /* Contact Section */
        .contact-section {
            background: linear-gradient(135deg, var(--secondary) 0%, #1a1412 100%);
            color: var(--white);
        }

        .contact-section .section-title h2 {
            color: var(--white);
        }

        .contact-section .section-title p {
            color: #b0b0b0;
        }

        .contact-form {
            max-width: 700px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.05);
            padding: 40px;
            border-radius: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .form-control {
            width: 100%;
            padding: 15px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: var(--white);
            font-family: 'Poppins', sans-serif;
            font-size: 14px;
            outline: none;
            transition: var(--transition);
        }

        .form-control:focus {
            border-color: var(--primary);
            background: rgba(255, 255, 255, 0.15);
        }

        .form-control::placeholder {
            color: #b0b0b0;
        }

        textarea.form-control {
            resize: vertical;
            height: 130px;
        }

        .contact-form .btn {
            width: 100%;
            padding: 15px;
            font-size: 16px;
            margin-top: 10px;
        }

        /* Footer */
        footer {
            background-color: #1a1412;
            color: #b0b0b0;
            padding: 40px 0;
            text-align: center;
            font-size: 14px;
            border-top: 1px solid rgba(255,255,255,0.05);
        }

        footer p span {
            color: var(--primary);
        }

        /* Responsive Design */
        @media (max-width: 992px) {
            .hero-grid, .tips-grid {
                grid-template-columns: 1fr;
                text-align: center;
            }

            .hero-btns {
                justify-content: center;
            }

            .about-grid, .gallery-grid {
                grid-template-columns: repeat(2, 1fr);
            }

            .tip-item {
                flex-direction: column;
                align-items: center;
                text-align: center;
            }
        }

        @media (max-width: 768px) {
            .nav-links {
                display: none;
                flex-direction: column;
                position: absolute;
                top: 80px;
                left: 0;
                width: 100%;
                background: var(--white);
                padding: 20px 0;
                box-shadow: 0 10px 15px rgba(0,0,0,0.05);
                text-align: center;
            }

            .nav-links.active {
                display: flex;
            }

            .hamburger {
                display: block;
            }

            .about-grid, .gallery-grid, .form-row {
                grid-template-columns: 1fr;
            }

            .hero-content h1 {
                font-size: 36px;
            }
        }
    </style>
</head>
<body>

    <!-- Header -->
    <header>
        <div class="container nav-container">
            <a href="#" class="logo"><i class="fa-solid fa-dog"></i>Tiny Paws <span>Yorkies</span></a>
            <nav>
                <ul class="nav-links" id="navLinks">
                    <li><a href="#home">Home</a></li>
                    <li><a href="#about">About</a></li>
                    <li><a href="#puppies">Available Puppies</a></li>
                    <li><a href="#care">Care Guide</a></li>
                    <li><a href="#contact" class="btn">Inquire Now</a></li>
                </ul>
            </nav>
            <div class="hamburger" id="hamburger">
                <i class="fa-solid fa-bars"></i>
            </div>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="hero" id="home">
        <div class="container hero-grid">
            <div class="hero-content">
                <h1>Big Personality in a <span>Tiny Package</span></h1>
                <p>Welcome to Tiny Paws Yorkies! We raise healthy, affectionate, and champion-line Yorkshire Terriers with love, care, and proper socialization.</p>
                <div class="hero-btns">
                    <a href="#puppies" class="btn">View Puppies</a>
                    <a href="#contact" class="btn-outline">Contact Us</a>
                </div>
            </div>
            <div class="hero-image">
                <img src="https://images.unsplash.com/photo-1583511655857-d19b40a7a54e?auto=format&fit=crop&w=800&q=80" alt="Cute Yorkshire Terrier">
            </div>
        </div>
    </section>

    <!-- About Section -->
    <section class="section-padding" id="about">
        <div class="container">
            <div class="section-title">
                <h2>Why Choose Our Yorkies?</h2>
                <p>Yorkshire Terriers are renowned for their glamorous silky coats, fearless loyalty, and vibrant, loving spirits.</p>
            </div>
            <div class="about-grid">
                <div class="about-card">
                    <i class="fa-solid fa-heart-pulse"></i>
                    <h3>Health Guaranteed</h3>
                    <p>All our puppies undergo strict veterinary health checks, vaccinations, and deworming before joining their new families.</p>
                </div>
                <div class="about-card">
                    <i class="fa-solid fa-house-chimney-user"></i>
                    <h3>Home Raised</h3>
                    <p>Raised in a loving home environment alongside humans to ensure they are exceptionally friendly and well-socialized.</p>
                </div>
                <div class="about-card">
                    <i class="fa-solid fa-certificate"></i>
                    <h3>Purebred Bloodlines</h3>
                    <p>Carefully selected champion bloodlines ensuring ideal temperament, gorgeous coats, and classic breed conformation.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Gallery / Puppies Showcase -->
    <section class="section-padding gallery-section" id="puppies">
        <div class="container">
            <div class="section-title">
                <h2>Meet Our Available Puppies</h2>
                <p>Explore our adorable litters looking for their forever loving homes.</p>
            </div>
            <div class="gallery-grid">
                <div class="gallery-item">
                    <img src="https://images.unsplash.com/photo-1535930891776-0c2dfb7fda1a?auto=format&fit=crop&w=600&q=80" alt="Yorkie Puppy" class="gallery-img">
                    <div class="gallery-info">
                        <h3>Bella (Female)</h3>
                        <p>10 Weeks Old • Ready</p>
                    </div>
                </div>
                <div class="gallery-item">
                    <img src="https://images.unsplash.com/photo-1576201836106-db1758fd1c97?auto=format&fit=crop&w=600&q=80" alt="Yorkie Puppy" class="gallery-img">
                    <div class="gallery-info">
                        <h3>Milo (Male)</h3>
                        <p>12 Weeks Old • Champion Line</p>
                    </div>
                </div>
                <div class="gallery-item">
                    <img src="https://images.unsplash.com/photo-1543466835-00a7907e9de1?auto=format&fit=crop&w=600&q=80" alt="Yorkie Puppy" class="gallery-img">
                    <div class="gallery-info">
                        <h3>Sophie (Female)</h3>
                        <p>11 Weeks Old • Playful</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Care Guide Section -->
    <section class="section-padding" id="care">
        <div class="container">
            <div class="tips-grid">
                <div class="tips-list">
                    <div class="section-title" style="text-align: left; margin-bottom: 30px;">
                        <h2>Yorkie Care & Grooming Tips</h2>
                        <p style="margin: 0;">Keeping your Yorkshire Terrier healthy, clean, and happy requires proper daily attention.</p>
                    </div>
                    <div class="tip-item">
                        <div class="tip-icon"><i class="fa-solid fa-scissors"></i></div>
                        <div class="tip-text">
                            <h3>Coat Maintenance</h3>
                            <p>Their silky hair needs regular brushing daily to prevent tangles and keep their signature look shining.</p>
                        </div>
                    </div>
                    <div class="tip-item">
                        <div class="tip-icon"><i class="fa-solid fa-bone"></i></div>
                        <div class="tip-text">
                            <h3>Balanced Nutrition</h3>
                            <p>Feed high-quality small-breed formula food formulated for high energy and sensitive digestive systems.</p>
                        </div>
                    </div>
                    <div class="tip-item">
                        <div class="tip-icon"><i class="fa-solid fa-graduation-cap"></i></div>
                        <div class="tip-text">
                            <h3>Early Socialization</h3>
                            <p>Gentle obedience training and positive reinforcement help build a confident and well-behaved companion.</p>
                        </div>
                    </div>
                </div>
                <div class="tips-image">
                    <img src="https://images.unsplash.com/photo-1516734212186-a967f81ad0d7?auto=format&fit=crop&w=800&q=80" alt="Groomed Yorkie">
                </div>
            </div>
        </div>
    </section>

    <!-- Contact Section -->
    <section class="section-padding contact-section" id="contact">
        <div class="container">
            <div class="section-title">
                <h2>Inquire About a Puppy</h2>
                <p>Have questions or want to reserve a puppy? Send us a message today!</p>
            </div>
            <form class="contact-form" onsubmit="event.preventDefault(); alert('Thank you for your message! We will get back to you soon.');">
                <div class="form-row">
                    <div class="form-group">
                        <input type="text" class="form-control" placeholder="Your Name" required>
                    </div>
                    <div class="form-group">
                        <input type="email" class="form-control" placeholder="Your Email" required>
                    </div>
                </div>
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Subject / Interested Puppy">
                </div>
                <div class="form-group">
                    <textarea class="form-control" placeholder="Your Message or Inquiry..." required></textarea>
                </div>
                <button type="submit" class="btn">Send Inquiry</button>
            </form>
        </div>
    </section>

    <!-- Footer -->
    <footer>
        <div class="container">
            <p>&copy; 2026 Tiny Paws Yorkies. All Rights Reserved. Designed with love for <span>Prime Solutions</span>.</p>
        </div>
    </footer>

    <!-- JavaScript for Mobile Menu -->
    <script>
        const hamburger = document.getElementById('hamburger');
        const navLinks = document.getElementById('navLinks');

        hamburger.addEventListener('click', () => {
            navLinks.classList.toggle('active');
        });

        // Close menu on click link
        document.querySelectorAll('.nav-links a').forEach(link => {
            link.addEventListener('click', () => {
                navLinks.classList.remove('active');
            });
        });
    </script>
</body>
</html>
