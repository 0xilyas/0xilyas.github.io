<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HE8ST | Elite Software Development & Cybersecurity</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Space+Grotesk:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-blue: #00D4FF;
            --primary-gold: #FFB800;
            --dark-purple: #0A0A1E;
            --medium-purple: #1A1A3E;
            --light-purple: #2A2A5E;
            --cyber-green: #00FF9F;
            --text-light: #E0E6ED;
            --text-dim: #8892B0;
        }

        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            font-family: 'Inter', sans-serif;
            color: var(--text-light);
            background: linear-gradient(135deg, var(--dark-purple) 0%, #000011 50%, var(--medium-purple) 100%);
            overflow-x: hidden;
        }

        body.light {
            --primary-blue: #0066CC;
            --primary-gold: #FF8C00;
            --dark-purple: #F8FAFC;
            --medium-purple: #E2E8F0;
            --light-purple: #CBD5E1;
            --cyber-green: #059669;
            --text-light: #0F172A;
            --text-dim: #475569;
            background: linear-gradient(135deg, #F1F5F9 0%, #FFFFFF 50%, #E2E8F0 100%);
        }

        /* Enhanced 3D Background Canvas */
        #background-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
        }

        /* Mobile-First Navigation */
        nav {
            background: rgba(10, 10, 30, 0.9);
            backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(0, 212, 255, 0.1);
            box-shadow: 0 8px 32px rgba(0, 212, 255, 0.1);
            transition: all 0.3s ease;
        }

        body.light nav {
            background: rgba(248, 250, 252, 0.95);
            border-bottom: 1px solid rgba(0, 102, 204, 0.1);
            box-shadow: 0 8px 32px rgba(0, 102, 204, 0.1);
        }

        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 1rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 70px;
        }

        .nav-logo {
            font-family: 'Space Grotesk', monospace;
            font-size: 1.5rem;
            font-weight: 900;
            background: linear-gradient(135deg, var(--primary-blue), var(--cyber-green));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .nav-links {
            display: flex;
            gap: 2rem;
            align-items: center;
        }

        .nav-btn {
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
            text-decoration: none;
            color: var(--text-light);
        }

        .nav-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0, 212, 255, 0.2), transparent);
            transition: left 0.5s;
        }

        .nav-btn:hover::before {
            left: 100%;
        }

        .nav-btn:hover {
            color: var(--primary-blue);
            transform: translateY(-2px) scale(1.05);
            text-shadow: 0 0 20px var(--primary-blue);
        }

        /* Mobile Menu */
        .mobile-menu-btn {
            display: none;
            flex-direction: column;
            cursor: pointer;
            background: none;
            border: none;
            padding: 0.5rem;
        }

        .mobile-menu-btn span {
            width: 25px;
            height: 3px;
            background: var(--text-light);
            margin: 3px 0;
            transition: 0.3s;
            border-radius: 2px;
        }

        .mobile-menu-btn.active span:nth-child(1) {
            transform: rotate(-45deg) translate(-5px, 6px);
        }

        .mobile-menu-btn.active span:nth-child(2) {
            opacity: 0;
        }

        .mobile-menu-btn.active span:nth-child(3) {
            transform: rotate(45deg) translate(-5px, -6px);
        }

        .mobile-menu {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background: rgba(10, 10, 30, 0.95);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(0, 212, 255, 0.1);
            transform: translateY(-100%);
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
            z-index: 50;
        }

        body.light .mobile-menu {
            background: rgba(248, 250, 252, 0.95);
            border-top: 1px solid rgba(0, 102, 204, 0.1);
        }

        .mobile-menu.active {
            transform: translateY(0);
            opacity: 1;
            visibility: visible;
        }

        .mobile-menu-links {
            display: flex;
            flex-direction: column;
            padding: 2rem;
            gap: 1.5rem;
        }

        /* Enhanced HE8ST Logo */
        .logo-container {
            position: relative;
            perspective: 1000px;
            margin-bottom: 2rem;
        }

        .he8st-logo {
            font-family: 'Space Grotesk', monospace;
            font-size: clamp(3rem, 15vw, 8rem);
            font-weight: 900;
            background: linear-gradient(135deg, var(--primary-blue), var(--cyber-green), var(--primary-gold));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-align: center;
            letter-spacing: 0.1em;
            position: relative;
            animation: logoFloat 6s ease-in-out infinite, logoGlow 4s ease-in-out infinite alternate;
            line-height: 1.1;
        }

        .he8st-logo::before {
            content: 'HE8ST';
            position: absolute;
            top: 0;
            left: 0;
            z-index: -1;
            background: linear-gradient(135deg, var(--primary-blue), var(--cyber-green));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            filter: blur(10px);
            opacity: 0.5;
            animation: pulse 3s ease-in-out infinite;
        }

        .he8st-logo::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 120%;
            height: 120%;
            background: conic-gradient(from 0deg, var(--primary-blue), var(--cyber-green), var(--primary-gold), var(--primary-blue));
            border-radius: 20px;
            z-index: -2;
            opacity: 0.1;
            animation: rotate 20s linear infinite;
        }

        @keyframes logoFloat {
            0%, 100% { transform: translateY(0px) rotateX(0deg); }
            50% { transform: translateY(-20px) rotateX(5deg); }
        }

        @keyframes logoGlow {
            0% { filter: drop-shadow(0 0 20px rgba(0, 212, 255, 0.5)); }
            100% { filter: drop-shadow(0 0 40px rgba(0, 255, 159, 0.8)); }
        }

        @keyframes rotate {
            0% { transform: translate(-50%, -50%) rotate(0deg); }
            100% { transform: translate(-50%, -50%) rotate(360deg); }
        }

        @keyframes pulse {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 0.7; }
        }

        /* Enhanced Cards with Morphism */
        .morphic-card {
            background: rgba(26, 26, 62, 0.4);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(0, 212, 255, 0.2);
            border-radius: 20px;
            transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        body.light .morphic-card {
            background: rgba(248, 250, 252, 0.6);
            border: 1px solid rgba(0, 102, 204, 0.2);
        }

        .morphic-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, var(--primary-blue), transparent);
            transition: all 0.5s ease;
        }

        .morphic-card:hover {
            transform: translateY(-10px) scale(1.02);
            box-shadow: 0 25px 50px rgba(0, 212, 255, 0.2), 0 0 0 1px rgba(0, 212, 255, 0.1);
            border-color: var(--primary-blue);
        }

        .morphic-card:hover::before {
            height: 3px;
            box-shadow: 0 0 20px var(--primary-blue);
        }

        /* Offer Section Enhancement */
        .offer-card {
            background: linear-gradient(135deg, rgba(0, 212, 255, 0.1), rgba(0, 255, 159, 0.1));
            border: 2px solid;
            border-image: linear-gradient(135deg, var(--primary-blue), var(--cyber-green)) 1;
            position: relative;
            overflow: hidden;
            cursor: pointer;
        }

        .offer-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: conic-gradient(transparent, rgba(0, 212, 255, 0.1), transparent, rgba(0, 255, 159, 0.1), transparent);
            animation: rotate 10s linear infinite;
            z-index: -1;
        }

        .offer-price {
            font-size: clamp(2rem, 8vw, 3.5rem);
            font-weight: 900;
            background: linear-gradient(135deg, var(--primary-gold), var(--primary-blue));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            animation: textShine 3s ease-in-out infinite;
        }

        @keyframes textShine {
            0%, 100% { background-position: -200% center; }
            50% { background-position: 200% center; }
        }

        /* Service Cards Grid - Mobile Optimized */
        .services-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
            max-width: 1400px;
            width: 100%;
            padding: 0 1rem;
        }

        .service-card {
            position: relative;
            overflow: hidden;
            cursor: pointer;
            transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            min-height: 200px;
        }

        .service-card::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0, 212, 255, 0.1), transparent);
            transition: left 0.8s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .service-card:hover::after {
            left: 100%;
        }

        .service-card:active {
            transform: scale(0.98);
        }

        /* Enhanced Popups - Mobile Optimized */
        .popup {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            backdrop-filter: blur(10px);
            padding: 1rem;
            animation: popupSlide 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .popup-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(26, 26, 62, 0.95);
            backdrop-filter: blur(25px);
            border: 1px solid rgba(0, 212, 255, 0.3);
            border-radius: 20px;
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5), 0 0 0 1px rgba(0, 212, 255, 0.1);
            max-width: 90%;
            width: 500px;
            max-height: 80vh;
            overflow-y: auto;
        }

        body.light .popup-content {
            background: rgba(248, 250, 252, 0.95);
            border: 1px solid rgba(0, 102, 204, 0.3);
        }

        @keyframes popupSlide {
            0% {
                opacity: 0;
                transform: scale(0.8);
            }
            100% {
                opacity: 1;
                transform: scale(1);
            }
        }

        .close-popup {
            position: absolute;
            top: 1rem;
            right: 1rem;
            font-size: 1.5rem;
            cursor: pointer;
            z-index: 10;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            background: rgba(0, 0, 0, 0.5);
            transition: all 0.3s ease;
        }

        .close-popup:hover {
            background: rgba(255, 0, 0, 0.5);
            transform: scale(1.1);
        }

        /* Enhanced Buttons - Mobile Optimized */
        .cta-button {
            background: linear-gradient(135deg, var(--primary-blue), var(--cyber-green));
            border: none;
            border-radius: 50px;
            padding: 1rem 2rem;
            font-weight: 600;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
            box-shadow: 0 10px 25px rgba(0, 212, 255, 0.3);
            min-height: 50px;
            color: white;
            font-size: 1rem;
        }

        .cta-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.6s;
        }

        .cta-button:hover::before {
            left: 100%;
        }

        .cta-button:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 15px 35px rgba(0, 212, 255, 0.4);
        }

        .cta-button:active {
            transform: translateY(-1px) scale(1.02);
        }

        /* Smooth scrolling */
        html {
            scroll-behavior: smooth;
        }

        section {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 2rem 1rem;
            position: relative;
        }

        /* Loading animation for Three.js */
        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--dark-purple);
            z-index: 9999;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: opacity 1s ease-out;
        }

        .loading-overlay.hidden {
            opacity: 0;
            pointer-events: none;
        }

        /* Mobile Responsive Breakpoints */
        @media (max-width: 768px) {
            /* Navigation */
            .nav-links {
                display: none;
            }

            .mobile-menu-btn {
                display: flex;
            }

            /* Hero Section */
            .he8st-logo {
                font-size: clamp(2.5rem, 12vw, 4rem);
                letter-spacing: 0.05em;
            }

            /* Services Grid */
            .services-grid {
                grid-template-columns: 1fr;
                gap: 1rem;
                padding: 0 0.5rem;
            }

            .service-card {
                min-height: 160px;
            }

            /* Offer Card */
            .offer-card {
                margin: 0 0.5rem 2rem;
            }

            .offer-price {
                font-size: clamp(1.8rem, 10vw, 2.5rem);
            }

            /* Popups */
            .popup {
                padding: 0.5rem;
            }

            .popup-content {
                width: 95%;
                max-height: 90vh;
            }

            /* Sections */
            section {
                padding: 1rem 0.5rem;
                min-height: auto;
            }

            /* Text sizing */
            h2 {
                font-size: clamp(2rem, 8vw, 4rem) !important;
            }

            .text-2xl {
                font-size: 1.25rem !important;
            }

            .text-xl {
                font-size: 1.1rem !important;
            }

            .text-lg {
                font-size: 1rem !important;
            }

            /* CTA Button */
            .cta-button {
                width: 100%;
                max-width: 280px;
                padding: 1.2rem 2rem;
            }

            /* Navigation height adjustment */
            .nav-container {
                height: 60px;
            }

            /* Background canvas optimization */
            #background-canvas {
                transform: scale(0.8);
                opacity: 0.6;
            }
        }

        @media (max-width: 480px) {
            /* Extra small screens */
            section {
                padding: 1rem 0.25rem;
            }

            .morphic-card {
                margin: 0 0.25rem;
            }

            .he8st-logo {
                font-size: clamp(2rem, 15vw, 3rem);
            }

            .offer-price {
                font-size: clamp(1.5rem, 12vw, 2rem);
            }

            h2 {
                font-size: clamp(1.5rem, 10vw, 3rem) !important;
            }

            .popup-content {
                border-radius: 15px;
            }

            /* Reduce Three.js complexity on very small screens */
            #background-canvas {
                transform: scale(0.6);
                opacity: 0.4;
            }
        }

        /* Tablet optimizations */
        @media (min-width: 769px) and (max-width: 1024px) {
            .services-grid {
                grid-template-columns: repeat(2, 1fr);
                gap: 1.5rem;
            }

            .he8st-logo {
                font-size: clamp(4rem, 10vw, 6rem);
            }

            section {
                padding: 3rem 2rem;
            }
        }

        /* Large screen optimizations */
        @media (min-width: 1200px) {
            .services-grid {
                grid-template-columns: repeat(3, 1fr);
                gap: 2rem;
            }

            .nav-container {
                padding: 0 2rem;
            }
        }

        /* Touch device optimizations */
        @media (hover: none) and (pointer: coarse) {
            .morphic-card:hover {
                transform: none;
                box-shadow: 0 15px 30px rgba(0, 212, 255, 0.15);
            }

            .service-card:hover::after {
                left: 0;
                opacity: 0.1;
            }

            .cta-button:hover {
                transform: none;
            }

            .nav-btn:hover {
                transform: none;
                text-shadow: none;
            }
        }

        /* High DPI screen optimizations */
        @media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
            .he8st-logo {
                text-rendering: optimizeLegibility;
            }
        }

        /* Landscape phone optimizations */
        @media screen and (max-height: 500px) and (orientation: landscape) {
            section {
                min-height: auto;
                padding: 2rem 1rem;
            }

            .he8st-logo {
                font-size: clamp(2rem, 8vw, 3rem);
            }
        }

        /* Accessibility improvements */
        @media (prefers-reduced-motion: reduce) {
            * {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: 0.01ms !important;
            }

            .he8st-logo::before,
            .he8st-logo::after {
                animation: none;
            }
        }
    </style>
</head>
<body class="dark">
    <div class="loading-overlay" id="loading">
        <div class="he8st-logo" style="font-size: 2rem;">HE8ST</div>
    </div>
    
    <canvas id="background-canvas"></canvas>
    
    <nav class="fixed top-0 w-full z-50">
        <div class="nav-container">
            <div class="nav-logo">HE8ST</div>
            
            <div class="nav-links">
                <a href="#home" class="nav-btn text-lg font-semibold px-4 py-2">Home</a>
                <a href="#services" class="nav-btn text-lg font-semibold px-4 py-2">Services</a>
                <a href="#contact" class="nav-btn text-lg font-semibold px-4 py-2">Contact</a>
                <button id="theme-toggle" class="nav-btn text-lg font-semibold px-4 py-2">🌙</button>
            </div>

            <button class="mobile-menu-btn" id="mobile-menu-btn">
                <span></span>
                <span></span>
                <span></span>
            </button>
        </div>

        <div class="mobile-menu" id="mobile-menu">
            <div class="mobile-menu-links">
                <a href="#home" class="nav-btn text-lg font-semibold">Home</a>
                <a href="#services" class="nav-btn text-lg font-semibold">Services</a>
                <a href="#contact" class="nav-btn text-lg font-semibold">Contact</a>
                <button id="mobile-theme-toggle" class="nav-btn text-lg font-semibold text-left">🌙 Toggle Theme</button>
            </div>
        </div>
    </nav>

    <section id="home">
        <div class="logo-container">
            <h1 class="he8st-logo">HE8ST</h1>
        </div>
        
        <div class="morphic-card offer-card p-6 sm:p-8 mb-8 sm:mb-12 cursor-pointer max-w-2xl w-full mx-4" id="offer-section">
            <div class="offer-price mb-4 text-center">$50 Landing Page</div>
            <p class="text-lg sm:text-xl font-medium text-center mb-4">Premium Animated Designs That Convert</p>
            <p class="text-base sm:text-lg opacity-80 text-center">Delivered in 48 hours • 3D Animations • SEO Optimized</p>
        </div>
        
        <p class="text-lg sm:text-2xl max-w-3xl mb-8 sm:mb-12 font-medium text-center leading-relaxed opacity-90 px-4">
            Elite Software Development & Cybersecurity Solutions for Tomorrow's Digital Landscape
        </p>
        
        <a href="#services" class="cta-button text-white font-semibold text-lg">
            Explore Services
            <span>→</span>
        </a>
        
        <div id="offer-popup" class="popup">
            <div class="popup-content">
                <span class="close-popup">✕</span>
                <div class="p-6 sm:p-8">
                    <h3 class="text-2xl sm:text-3xl font-bold mb-6" style="color: var(--primary-blue)">$50 Premium Landing Page</h3>
                    <div class="space-y-4 mb-8 text-base sm:text-lg">
                        <p>✨ Modern responsive design with 3D animations</p>
                        <p>🚀 SEO-optimized for maximum visibility</p>
                        <p>⚡ Perfect for startups and crypto projects</p>
                        <p>🎯 Conversion-focused user experience</p>
                        <p>📱 Mobile-first approach</p>
                    </div>
                    <a href="https://t.me/HE8ST" class="cta-button text-white w-full">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
                            <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm4.64 6.8c-.15 1.58-.8 5.42-1.13 7.19-.14.75-.42 1-.68 1.03-.58.05-1.02-.38-1.58-.75-.88-.58-1.38-.94-2.23-1.5-.99-.65-.35-1.01.22-1.59.15-.15 2.71-2.48 2.76-2.69a.2.2 0 0 0-.05-.18c-.06-.05-.14-.03-.21-.02-.09.02-1.49.95-4.22 2.79-.4.27-.76.41-1.08.4-.36-.01-1.04-.2-1.55-.37-.63-.2-1.12-.31-1.08-.66.02-.18.27-.36.74-.55 2.92-1.27 4.86-2.11 5.83-2.51 2.78-1.16 3.35-1.36 3.73-1.36.08 0 .27.02.39.12.1.08.13.19.14.27-.01.06.01.24 0 .38z"/>
                        </svg>
                        Contact on Telegram
                    </a>
                </div>
            </div>
        </div>
    </section>

    <section id="services">
        <h2 class="text-4xl sm:text-6xl font-bold mb-4 text-center" style="color: var(--primary-blue)">Services</h2>
        <p class="text-lg sm:text-xl opacity-75 text-center mb-12 sm:mb-16 max-w-2xl px-4">Cutting-edge solutions powered by the latest technologies</p>
        
        <div class="services-grid">
            <div class="service-card morphic-card p-6 sm:p-8" data-popup="landing-popup">
                <h3 class="text-xl sm:text-2xl font-bold mb-4" style="color: var(--primary-gold)">Landing Page Design</h3>
                <p class="text-base sm:text-lg opacity-90 mb-4">Premium responsive pages with 3D animations</p>
                <div class="text-2xl sm:text-3xl font-bold" style="color: var(--primary-blue)">$50</div>
            </div>
            
            <div class="service-card morphic-card p-6 sm:p-8" data-popup="webapp-popup">
                <h3 class="text-xl sm:text-2xl font-bold mb-4" style="color: var(--primary-gold)">Web App Development</h3>
                <p class="text-base sm:text-lg opacity-90 mb-4">Custom applications with modern frameworks</p>
                <div class="text-sm opacity-75">From $500</div>
            </div>
            
            <div class="service-card morphic-card p-6 sm:p-8" data-popup="crypto-popup">
                <h3 class="text-xl sm:text-2xl font-bold mb-4" style="color: var(--primary-gold)">Web3 DApp Development</h3>
                <p class="text-base sm:text-lg opacity-90 mb-4">Solana DApps and smart contracts</p>
                <div class="text-sm opacity-75">From $1000</div>
            </div>
            
            <div class="service-card morphic-card p-6 sm:p-8" data-popup="scripting-popup">
                <h3 class="text-xl sm:text-2xl font-bold mb-4" style="color: var(--primary-gold)">Automation Scripts</h3>
                <p class="text-base sm:text-lg opacity-90 mb-4">Python/Flask efficiency solutions</p>
                <div class="text-sm opacity-75">From $100</div>
            </div>
            
            <div class="service-card morphic-card p-6 sm:p-8" data-popup="mobile-popup">
                <h3 class="text-xl sm:text-2xl font-bold mb-4" style="color: var(--primary-gold)">Mobile Apps</h3>
                <p class="text-base sm:text-lg opacity-90 mb-4">Cross-platform iOS and Android</p>
                <div class="text-sm opacity-75">From $800</div>
            </div>
            
            <div class="service-card morphic-card p-6 sm:p-8" data-popup="backend-popup">
                <h3 class="text-xl sm:text-2xl font-bold mb-4" style="color: var(--primary-gold)">Backend Systems</h3>
                <p class="text-base sm:text-lg opacity-90 mb-4">Scalable APIs and server solutions</p>
                <div class="text-sm opacity-75">From $600</div>
            </div>
        </div>

        <!-- Service Popups -->
        <div id="landing-popup" class="popup">
            <div class="popup-content">
                <span class="close-popup">✕</span>
                <div class="p-6 sm:p-8">
                    <h3 class="text-2xl sm:text-3xl font-bold mb-6" style="color: var(--primary-blue)">Landing Page Design</h3>
                    <p class="text-base sm:text-lg mb-6">Professional responsive landing pages with modern 3D animations, optimized for conversions and SEO.</p>
                    <a href="https://t.me/HE8ST" class="cta-button text-white w-full">Contact on Telegram</a>
                </div>
            </div>
        </div>
        
        <div id="webapp-popup" class="popup">
            <div class="popup-content">
                <span class="close-popup">✕</span>
                <div class="p-6 sm:p-8">
                    <h3 class="text-2xl sm:text-3xl font-bold mb-6" style="color: var(--primary-blue)">Web App Development</h3>
                    <p class="text-base sm:text-lg mb-6">Custom web applications built with React, Vue, or vanilla JS for optimal performance and user experience.</p>
                    <a href="https://t.me/HE8ST" class="cta-button text-white w-full">Contact on Telegram</a>
                </div>
            </div>
        </div>
        
        <div id="crypto-popup" class="popup">
            <div class="popup-content">
                <span class="close-popup">✕</span>
                <div class="p-6 sm:p-8">
                    <h3 class="text-2xl sm:text-3xl font-bold mb-6" style="color: var(--primary-blue)">Web3 DApp Development</h3>
                    <p class="text-base sm:text-lg mb-6">Cutting-edge Solana-based DApps, smart contracts, and trading systems for blockchain innovators.</p>
                    <a href="https://t.me/HE8ST" class="cta-button text-white w-full">Contact on Telegram</a>
                </div>
            </div>
        </div>
        
        <div id="scripting-popup" class="popup">
            <div class="popup-content">
                <span class="close-popup">✕</span>
                <div class="p-6 sm:p-8">
                    <h3 class="text-2xl sm:text-3xl font-bold mb-6" style="color: var(--primary-blue)">Automation Scripts</h3>
                    <p class="text-base sm:text-lg mb-6">Efficient Python and Flask scripts to automate workflows and boost productivity across your business.</p>
                    <a href="https://t.me/HE8ST" class="cta-button text-white w-full">Contact on Telegram</a>
                </div>
            </div>
        </div>
        
        <div id="mobile-popup" class="popup">
            <div class="popup-content">
                <span class="close-popup">✕</span>
                <div class="p-6 sm:p-8">
                    <h3 class="text-2xl sm:text-3xl font-bold mb-6" style="color: var(--primary-blue)">Mobile App Development</h3>
                    <p class="text-base sm:text-lg mb-6">Native and cross-platform mobile applications for iOS and Android with seamless user experiences.</p>
                    <a href="https://t.me/HE8ST" class="cta-button text-white w-full">Contact on Telegram</a>
                </div>
            </div>
        </div>
        
        <div id="backend-popup" class="popup">
            <div class="popup-content">
                <span class="close-popup">✕</span>
                <div class="p-6 sm:p-8">
                    <h3 class="text-2xl sm:text-3xl font-bold mb-6" style="color: var(--primary-blue)">Backend Development</h3>
                    <p class="text-base sm:text-lg mb-6">Robust, scalable backend systems with secure APIs and high-performance server architecture.</p>
                    <a href="https://t.me/HE8ST" class="cta-button text-white w-full">Contact on Telegram</a>
                </div>
            </div>
        </div>
    </section>

    <section id="contact">
        <h2 class="text-4xl sm:text-6xl font-bold mb-8 text-center" style="color: var(--primary-blue)">Get Started</h2>
        <p class="text-lg sm:text-2xl text-center mb-8 sm:mb-12 max-w-3xl font-medium opacity-90 px-4">
            Ready to transform your digital presence with cutting-edge technology?
        </p>
        <a href="https://t.me/HE8ST" class="cta-button text-white text-lg sm:text-xl px-8 sm:px-12 py-4">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm4.64 6.8c-.15 1.58-.8 5.42-1.13 7.19-.14.75-.42 1-.68 1.03-.58.05-1.02-.38-1.58-.75-.88-.58-1.38-.94-2.23-1.5-.99-.65-.35-1.01.22-1.59.15-.15 2.71-2.48 2.76-2.69a.2.2 0 0 0-.05-.18c-.06-.05-.14-.03-.21-.02-.09.02-1.49.95-4.22 2.79-.4.27-.76.41-1.08.4-.36-.01-1.04-.2-1.55-.37-.63-.2-1.12-.31-1.08-.66.02-.18.27-.36.74-.55 2.92-1.27 4.86-2.11 5.83-2.51 2.78-1.16 3.35-1.36 3.73-1.36.08 0 .27.02.39.12.1.08.13.19.14.27-.01.06.01.24 0 .38z"/>
            </svg>
            Start Your Project
        </a>
    </section>

    <script>
        // Enhanced Three.js Background with Mobile Optimizations
        let bgScene, bgCamera, bgRenderer, shapes = [], particles, helix, grid;
        let isMobile = window.innerWidth <= 768;
        let isReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

        function initBackground() {
            bgScene = new THREE.Scene();
            bgCamera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            bgRenderer = new THREE.WebGLRenderer({ 
                canvas: document.getElementById('background-canvas'), 
                alpha: true,
                antialias: !isMobile,
                powerPreference: isMobile ? 'low-power' : 'high-performance'
            });
            bgRenderer.setSize(window.innerWidth, window.innerHeight);
            bgRenderer.setPixelRatio(isMobile ? 1 : Math.min(window.devicePixelRatio, 2));

            // Animated Grid (simplified for mobile)
            const gridGeometry = new THREE.PlaneGeometry(50, 50, isMobile ? 20 : 100, isMobile ? 20 : 100);
            const gridMaterial = new THREE.ShaderMaterial({
                uniforms: {
                    time: { value: 0 },
                    color1: { value: new THREE.Color(0x00D4FF) },
                    color2: { value: new THREE.Color(0x00FF9F) }
                },
                vertexShader: `
                    uniform float time;
                    varying vec2 vUv;
                    varying vec3 vPosition;
                    void main() {
                        vUv = uv;
                        vec3 pos = position;
                        pos.z += sin(pos.x * 0.5 + time) * cos(pos.y * 0.5 + time) * ${isMobile ? '1.0' : '2.0'};
                        vPosition = pos;
                        gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
                    }
                `,
                fragmentShader: `
                    uniform float time;
                    uniform vec3 color1;
                    uniform vec3 color2;
                    varying vec2 vUv;
                    varying vec3 vPosition;
                    void main() {
                        float wave = sin(vUv.x * ${isMobile ? '10.0' : '20.0'} + time) * cos(vUv.y * ${isMobile ? '10.0' : '20.0'} + time * 0.5);
                        vec3 color = mix(color1, color2, wave * 0.5 + 0.5);
                        float alpha = ${isMobile ? '0.2' : '0.3'} + wave * 0.2;
                        gl_FragColor = vec4(color, alpha);
                    }
                `,
                wireframe: true,
                transparent: true
            });
            grid = new THREE.Mesh(gridGeometry, gridMaterial);
            grid.rotation.x = -Math.PI / 2;
            bgScene.add(grid);

            // Floating Geometric Shapes (reduced for mobile)
            const shapeCount = isMobile ? 20 : 50;
            const shapeGeometries = [
                new THREE.BoxGeometry(1, 1, 1),
                new THREE.SphereGeometry(0.5, isMobile ? 8 : 16, isMobile ? 8 : 16),
                new THREE.ConeGeometry(0.5, 1, isMobile ? 4 : 8),
                new THREE.OctahedronGeometry(0.6),
            ];

            for (let i = 0; i < shapeCount; i++) {
                const geometry = shapeGeometries[Math.floor(Math.random() * shapeGeometries.length)];
                const material = new THREE.MeshBasicMaterial({
                    color: Math.random() > 0.5 ? 0x00D4FF : 0x00FF9F,
                    wireframe: true,
                    transparent: true,
                    opacity: isMobile ? 0.2 : 0.3
                });
                const shape = new THREE.Mesh(geometry, material);
                
                shape.position.set(
                    (Math.random() - 0.5) * 100,
                    Math.random() * 50 + 10,
                    (Math.random() - 0.5) * 100
                );
                
                shape.rotation.set(
                    Math.random() * Math.PI,
                    Math.random() * Math.PI,
                    Math.random() * Math.PI
                );
                
                shape.userData = {
                    rotationSpeed: {
                        x: (Math.random() - 0.5) * (isMobile ? 0.01 : 0.02),
                        y: (Math.random() - 0.5) * (isMobile ? 0.01 : 0.02),
                        z: (Math.random() - 0.5) * (isMobile ? 0.01 : 0.02)
                    },
                    floatSpeed: Math.random() * (isMobile ? 0.01 : 0.02) + 0.01
                };
                
                shapes.push(shape);
                bgScene.add(shape);
            }

            // Particle System (reduced for mobile)
            const particleCount = isMobile ? 200 : 1000;
            const particleGeometry = new THREE.BufferGeometry();
            const particlePositions = new Float32Array(particleCount * 3);
            const particleColors = new Float32Array(particleCount * 3);
            const particleSizes = new Float32Array(particleCount);

            for (let i = 0; i < particleCount; i++) {
                particlePositions[i * 3] = (Math.random() - 0.5) * 200;
                particlePositions[i * 3 + 1] = Math.random() * 100;
                particlePositions[i * 3 + 2] = (Math.random() - 0.5) * 200;
                
                const color = new THREE.Color();
                color.setHSL(Math.random() > 0.5 ? 0.55 : 0.15, 1, 0.7);
                particleColors[i * 3] = color.r;
                particleColors[i * 3 + 1] = color.g;
                particleColors[i * 3 + 2] = color.b;
                
                particleSizes[i] = Math.random() * (isMobile ? 1 : 2) + 1;
            }

            particleGeometry.setAttribute('position', new THREE.BufferAttribute(particlePositions, 3));
            particleGeometry.setAttribute('color', new THREE.BufferAttribute(particleColors, 3));
            particleGeometry.setAttribute('size', new THREE.BufferAttribute(particleSizes, 1));

            const particleMaterial = new THREE.ShaderMaterial({
                uniforms: {
                    time: { value: 0 }
                },
                vertexShader: `
                    attribute float size;
                    attribute vec3 color;
                    varying vec3 vColor;
                    uniform float time;
                    void main() {
                        vColor = color;
                        vec4 mvPosition = modelViewMatrix * vec4(position, 1.0);
                        gl_PointSize = size * (300.0 / -mvPosition.z) * (1.0 + sin(time + position.x * 0.01) * 0.3);
                        gl_Position = projectionMatrix * mvPosition;
                    }
                `,
                fragmentShader: `
                    varying vec3 vColor;
                    void main() {
                        vec2 center = gl_PointCoord - 0.5;
                        float dist = length(center);
                        if (dist > 0.5) discard;
                        float alpha = 1.0 - smoothstep(0.0, 0.5, dist);
                        gl_FragColor = vec4(vColor, alpha * ${isMobile ? '0.4' : '0.6'});
                    }
                `,
                transparent: true,
                vertexColors: true,
                blending: THREE.AdditiveBlending
            });

            particles = new THREE.Points(particleGeometry, particleMaterial);
            bgScene.add(particles);

            // DNA Helix Structure (simplified for mobile)
            if (!isMobile) {
                const helixGeometry = new THREE.BufferGeometry();
                const helixPositions = [];
                const helixColors = [];

                for (let i = 0; i < 100; i++) {
                    const t = i / 100 * Math.PI * 4;
                    const radius = 15;
                    
                    // First strand
                    helixPositions.push(
                        Math.cos(t) * radius,
                        i * 1 - 50,
                        Math.sin(t) * radius
                    );
                    helixColors.push(0, 0.83, 1);
                    
                    // Second strand
                    helixPositions.push(
                        Math.cos(t + Math.PI) * radius,
                        i * 1 - 50,
                        Math.sin(t + Math.PI) * radius
                    );
                    helixColors.push(0, 1, 0.62);
                }

                helixGeometry.setAttribute('position', new THREE.Float32BufferAttribute(helixPositions, 3));
                helixGeometry.setAttribute('color', new THREE.Float32BufferAttribute(helixColors, 3));

                const helixMaterial = new THREE.PointsMaterial({
                    size: 0.8,
                    vertexColors: true,
                    transparent: true,
                    opacity: 0.6
                });

                helix = new THREE.Points(helixGeometry, helixMaterial);
                helix.position.x = 30;
                bgScene.add(helix);
            }

            // Camera position
            bgCamera.position.set(0, 20, 40);
            bgCamera.lookAt(0, 0, 0);
        }

        // Animation loop with mobile optimizations
        let time = 0;
        function animateBackground() {
            requestAnimationFrame(animateBackground);
            
            if (isReducedMotion) return;
            
            time += isMobile ? 0.008 : 0.016;

            // Update grid shader
            if (grid && grid.material.uniforms) {
                grid.material.uniforms.time.value = time;
            }
            if (particles && particles.material.uniforms) {
                particles.material.uniforms.time.value = time;
            }

            // Animate floating shapes (reduced frequency for mobile)
            if (time % (isMobile ? 4 : 2) < (isMobile ? 0.008 : 0.016)) {
                shapes.forEach((shape, i) => {
                    if (shape.userData) {
                        shape.rotation.x += shape.userData.rotationSpeed.x;
                        shape.rotation.y += shape.userData.rotationSpeed.y;
                        shape.rotation.z += shape.userData.rotationSpeed.z;
                        
                        shape.position.y += Math.sin(time * shape.userData.floatSpeed + i) * (isMobile ? 0.05 : 0.1);
                    }
                });
            }

            // Animate particles (reduced for mobile)
            if (particles && particles.geometry.attributes.position) {
                const positions = particles.geometry.attributes.position.array;
                const particleCount = isMobile ? 200 : 1000;
                for (let i = 0; i < particleCount; i++) {
                    positions[i * 3 + 1] += isMobile ? 0.05 : 0.1;
                    if (positions[i * 3 + 1] > 100) {
                        positions[i * 3 + 1] = 0;
                    }
                }
                particles.geometry.attributes.position.needsUpdate = true;
            }

            // Rotate helix (desktop only)
            if (helix && !isMobile) {
                helix.rotation.y += 0.005;
            }

            // Camera movement (reduced for mobile)
            if (!isMobile) {
                bgCamera.position.x = Math.sin(time * 0.1) * 5;
                bgCamera.position.z = 40 + Math.cos(time * 0.05) * 10;
                bgCamera.lookAt(0, 0, 0);
            }

            bgRenderer.render(bgScene, bgCamera);
        }

        // Mobile menu functionality
        const mobileMenuBtn = document.getElementById('mobile-menu-btn');
        const mobileMenu = document.getElementById('mobile-menu');
        const mobileMenuLinks = document.querySelectorAll('.mobile-menu-links a');

        mobileMenuBtn.addEventListener('click', () => {
            mobileMenuBtn.classList.toggle('active');
            mobileMenu.classList.toggle('active');
        });

        // Close mobile menu when clicking on links
        mobileMenuLinks.forEach(link => {
            link.addEventListener('click', () => {
                mobileMenuBtn.classList.remove('active');
                mobileMenu.classList.remove('active');
            });
        });

        // Close mobile menu when clicking outside
        document.addEventListener('click', (e) => {
            if (!mobileMenu.contains(e.target) && !mobileMenuBtn.contains(e.target)) {
                mobileMenuBtn.classList.remove('active');
                mobileMenu.classList.remove('active');
            }
        });

        // Theme toggle for both desktop and mobile
        const themeToggle = document.getElementById('theme-toggle');
        const mobileThemeToggle = document.getElementById('mobile-theme-toggle');

        function toggleTheme() {
            document.body.classList.toggle('dark');
            document.body.classList.toggle('light');
            const isDark = document.body.classList.contains('dark');
            const themeIcon = isDark ? '🌙' : '☀️';
            
            themeToggle.textContent = themeIcon;
            mobileThemeToggle.textContent = `${themeIcon} Toggle Theme`;
            
            // Update shader colors based on theme
            if (grid && grid.material.uniforms) {
                if (document.body.classList.contains('light')) {
                    grid.material.uniforms.color1.value.setHex(0x0066CC);
                    grid.material.uniforms.color2.value.setHex(0x059669);
                } else {
                    grid.material.uniforms.color1.value.setHex(0x00D4FF);
                    grid.material.uniforms.color2.value.setHex(0x00FF9F);
                }
            }
        }

        themeToggle.addEventListener('click', toggleTheme);
        mobileThemeToggle.addEventListener('click', toggleTheme);

        // Popup handling with mobile optimizations
        const offerSection = document.getElementById('offer-section');
        const offerPopup = document.getElementById('offer-popup');
        const serviceCards = document.querySelectorAll('.service-card');
        const popups = document.querySelectorAll('.popup');
        const closePopups = document.querySelectorAll('.close-popup');

        offerSection.addEventListener('click', (e) => {
            e.preventDefault();
            offerPopup.style.display = 'block';
            document.body.style.overflow = 'hidden'; // Prevent background scrolling
        });

        serviceCards.forEach(card => {
            card.addEventListener('click', (e) => {
                e.preventDefault();
                const popupId = card.dataset.popup;
                const popup = document.getElementById(popupId);
                if (popup) {
                    popup.style.display = 'block';
                    document.body.style.overflow = 'hidden';
                }
            });
        });

        closePopups.forEach(close => {
            close.addEventListener('click', () => {
                popups.forEach(popup => {
                    popup.style.display = 'none';
                    document.body.style.overflow = 'auto';
                });
            });
        });

        // Close popups on background click
        popups.forEach(popup => {
            popup.addEventListener('click', (e) => {
                if (e.target === popup) {
                    popup.style.display = 'none';
                    document.body.style.overflow = 'auto';
                }
            });
        });

        // Smooth scroll for navigation with mobile optimization
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    // Close mobile menu if open
                    mobileMenuBtn.classList.remove('active');
                    mobileMenu.classList.remove('active');
                    
                    // Account for fixed nav height
                    const navHeight = document.querySelector('nav').offsetHeight;
                    const targetPosition = target.offsetTop - navHeight;
                    
                    window.scrollTo({
                        top: targetPosition,
                        behavior: 'smooth'
                    });
                }
            });
        });

        // Resize handler with mobile detection
        function handleResize() {
            const wasMobile = isMobile;
            isMobile = window.innerWidth <= 768;
            
            if (bgCamera && bgRenderer) {
                bgCamera.aspect = window.innerWidth / window.innerHeight;
                bgCamera.updateProjectionMatrix();
                bgRenderer.setSize(window.innerWidth, window.innerHeight);
                bgRenderer.setPixelRatio(isMobile ? 1 : Math.min(window.devicePixelRatio, 2));
            }
            
            // Reinitialize if mobile state changed
            if (wasModal !== isMobile && bgScene) {
                // Clear existing scene
                while(bgScene.children.length > 0) {
                    bgScene.remove(bgScene.children[0]);
                }
                shapes = [];
                initBackground();
            }
        }

        window.addEventListener('resize', handleResize);

        // Touch event optimization for mobile
        if (isMobile) {
            document.addEventListener('touchstart', function() {}, {passive: true});
            document.addEventListener('touchmove', function() {}, {passive: true});
        }

        // Loading screen with mobile optimization
        window.addEventListener('load', () => {
            setTimeout(() => {
                document.getElementById('loading').classList.add('hidden');
                initBackground();
                animateBackground();
            }, isMobile ? 500 : 1000);
        });

        // Intersection Observer for animations (mobile optimized)
        const observerOptions = {
            threshold: isMobile ? 0.05 : 0.1,
            rootMargin: '0px 0px -50px 0px'
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0)';
                }
            });
        }, observerOptions);

        // Observe service cards for animation
        document.querySelectorAll('.service-card').forEach(card => {
            card.style.opacity = '0';
            card.style.transform = `translateY(${isMobile ? '30px' : '50px'})`;
            card.style.transition = `opacity ${isMobile ? '0.6s' : '0.8s'} ease, transform ${isMobile ? '0.6s' : '0.8s'} ease`;
            observer.observe(card);
        });

        // Parallax effect (disabled on mobile for performance)
        if (!isMobile) {
            window.addEventListener('scroll', () => {
                const scrolled = window.pageYOffset;
                const parallaxElements = document.querySelectorAll('.parallax');
                
                parallaxElements.forEach(element => {
                    const speed = element.dataset.speed || 0.5;
                    element.style.transform = `translateY(${scrolled * speed}px)`;
                });
            });
        }

        // Prevent zoom on double tap (iOS)
        let lastTouchEnd = 0;
        document.addEventListener('touchend', function (event) {
            const now = (new Date()).getTime();
            if (now - lastTouchEnd <= 300) {
                event.preventDefault();
            }
            lastTouchEnd = now;
        }, false);

        // Orientation change handling
        window.addEventListener('orientationchange', () => {
            setTimeout(() => {
                handleResize();
            }, 100);
        });
    </script>
</body>
</html>
