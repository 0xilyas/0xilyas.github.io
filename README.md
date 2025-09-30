<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HE8ST | Advanced Digital Research Laboratory</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;600;700;800&family=Space+Grotesk:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        :root {
            --deep-purple: #0A0015;
            --dark-purple: #1A0F2E;
            --medium-purple: #2D1B4E;
            --accent-purple: #4A2B7C;
            --bright-purple: #7C3AED;
            --cyber-blue: #00D4FF;
            --neon-green: #00FF9F;
            --text-primary: #E0E6ED;
            --text-secondary: #A0A8B5;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'JetBrains Mono', monospace;
            color: var(--text-primary);
            background: var(--deep-purple);
            overflow-x: hidden;
        }

        #cursor-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            pointer-events: none;
        }

        nav {
            position: fixed;
            top: 0;
            width: 100%;
            background: rgba(10, 0, 21, 0.85);
            backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(124, 58, 237, 0.2);
            z-index: 1000;
            transition: all 0.3s ease;
        }

        .nav-container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 70px;
        }

        .nav-logo {
            font-family: 'Space Grotesk', sans-serif;
            font-size: 1.5rem;
            font-weight: 900;
            background: linear-gradient(135deg, var(--bright-purple), var(--cyber-blue));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: 0.1em;
        }

        .nav-links {
            display: flex;
            gap: 2rem;
            align-items: center;
        }

        .nav-btn {
            color: var(--text-primary);
            text-decoration: none;
            font-weight: 500;
            transition: all 0.3s ease;
            position: relative;
        }

        .nav-btn::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--bright-purple);
            transition: width 0.3s ease;
        }

        .nav-btn:hover::after {
            width: 100%;
        }

        .mobile-menu-btn {
            display: none;
            flex-direction: column;
            cursor: pointer;
            background: none;
            border: none;
        }

        .mobile-menu-btn span {
            width: 25px;
            height: 2px;
            background: var(--text-primary);
            margin: 3px 0;
            transition: 0.3s;
        }

        section {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 6rem 2rem 4rem;
            position: relative;
        }

        .hero-logo {
            font-family: 'Space Grotesk', sans-serif;
            font-size: clamp(4rem, 12vw, 9rem);
            font-weight: 900;
            background: linear-gradient(135deg, var(--bright-purple), var(--cyber-blue), var(--neon-green));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: 0.1em;
            margin-bottom: 1rem;
            position: relative;
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .lab-subtitle {
            font-size: clamp(1rem, 2vw, 1.5rem);
            color: var(--text-secondary);
            letter-spacing: 0.3em;
            text-transform: uppercase;
            margin-bottom: 3rem;
        }

        .offer-card {
            background: linear-gradient(135deg, rgba(74, 43, 124, 0.3), rgba(45, 27, 78, 0.3));
            border: 2px solid var(--bright-purple);
            border-radius: 20px;
            padding: 3rem;
            max-width: 700px;
            width: 100%;
            position: relative;
            overflow: hidden;
            cursor: pointer;
            transition: all 0.4s ease;
        }

        .offer-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: conic-gradient(transparent, rgba(124, 58, 237, 0.3), transparent 30%);
            animation: rotate 6s linear infinite;
        }

        @keyframes rotate {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .offer-content {
            position: relative;
            z-index: 1;
        }

        .service-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            gap: 2rem;
            max-width: 1400px;
            width: 100%;
            margin-top: 3rem;
        }

        .service-card {
            background: rgba(45, 27, 78, 0.4);
            border: 1px solid rgba(124, 58, 237, 0.3);
            border-radius: 15px;
            padding: 2rem;
            transition: all 0.4s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .service-card::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(124, 58, 237, 0.2), transparent);
            transition: left 0.5s ease;
        }

        .service-card:hover::after {
            left: 100%;
        }

        .service-card:hover {
            transform: translateY(-10px);
            border-color: var(--bright-purple);
            box-shadow: 0 20px 40px rgba(124, 58, 237, 0.3);
        }

        .service-animation {
            width: 100%;
            height: 150px;
            margin-bottom: 1.5rem;
            position: relative;
        }

        .cta-button {
            background: linear-gradient(135deg, var(--bright-purple), var(--cyber-blue));
            border: none;
            border-radius: 50px;
            padding: 1.2rem 3rem;
            font-weight: 600;
            font-family: 'Space Grotesk', sans-serif;
            color: white;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            gap: 0.75rem;
            transition: all 0.4s ease;
            box-shadow: 0 10px 30px rgba(124, 58, 237, 0.4);
            cursor: pointer;
        }

        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 40px rgba(124, 58, 237, 0.6);
        }

        .popup {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.9);
            backdrop-filter: blur(10px);
            z-index: 2000;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .popup-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(135deg, var(--dark-purple), var(--medium-purple));
            border: 2px solid var(--bright-purple);
            border-radius: 25px;
            max-width: 600px;
            width: 90%;
            max-height: 85vh;
            overflow-y: auto;
            box-shadow: 0 30px 60px rgba(124, 58, 237, 0.5);
            animation: slideUp 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translate(-50%, -40%);
            }
            to {
                opacity: 1;
                transform: translate(-50%, -50%);
            }
        }

        .close-popup {
            position: absolute;
            top: 1.5rem;
            right: 1.5rem;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(124, 58, 237, 0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1.5rem;
            z-index: 10;
        }

        .close-popup:hover {
            background: rgba(255, 0, 0, 0.5);
            transform: rotate(90deg);
        }

        footer {
            background: var(--dark-purple);
            border-top: 1px solid rgba(124, 58, 237, 0.3);
            padding: 3rem 2rem 2rem;
            text-align: center;
        }

        .social-links {
            display: flex;
            justify-content: center;
            gap: 2rem;
            margin-bottom: 2rem;
        }

        .social-icon {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: rgba(124, 58, 237, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
            text-decoration: none;
            color: var(--text-primary);
        }

        .social-icon:hover {
            background: var(--bright-purple);
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(124, 58, 237, 0.5);
        }

        @media (max-width: 768px) {
            .nav-links { display: none; }
            .mobile-menu-btn { display: flex; }
            section { padding: 5rem 1rem 3rem; }
            .service-grid { grid-template-columns: 1fr; }
            .offer-card { padding: 2rem; }
        }
    </style>
</head>
<body>
    <canvas id="cursor-canvas"></canvas>

    <nav>
        <div class="nav-container">
            <div class="nav-logo">HE8ST</div>
            <div class="nav-links">
                <a href="#home" class="nav-btn">Home</a>
                <a href="#research" class="nav-btn">Research</a>
                <a href="#contact" class="nav-btn">Contact</a>
            </div>
            <button class="mobile-menu-btn">
                <span></span>
                <span></span>
                <span></span>
            </button>
        </div>
    </nav>

    <section id="home">
        <h1 class="hero-logo">HE8ST</h1>
        <p class="lab-subtitle">Advanced Digital Research Laboratory</p>
        
        <div class="offer-card" id="offer-card">
            <div class="offer-content">
                <h3 style="font-size: 2.5rem; font-weight: 900; margin-bottom: 1rem; background: linear-gradient(135deg, var(--cyber-blue), var(--neon-green)); -webkit-background-clip: text; -webkit-text-fill-color: transparent;">$350 Basic Website Build</h3>
                <p style="font-size: 1.25rem; margin-bottom: 1rem; color: var(--text-secondary);">Complete Responsive Web Solutions</p>
                <p style="color: var(--text-secondary);">Full-stack development • Modern animations • 48-hour delivery • SEO optimized</p>
            </div>
        </div>

        <p style="max-width: 800px; text-align: center; margin: 3rem 0; font-size: 1.25rem; line-height: 1.8; color: var(--text-secondary);">
            Where cutting-edge research meets practical innovation. We develop advanced digital solutions through rigorous experimentation and breakthrough technologies.
        </p>

        <a href="#research" class="cta-button">
            Explore Research Areas
            <span>→</span>
        </a>
    </section>

    <section id="research">
        <h2 style="font-size: clamp(3rem, 8vw, 5rem); font-weight: 900; margin-bottom: 1rem; background: linear-gradient(135deg, var(--bright-purple), var(--cyber-blue)); -webkit-background-clip: text; -webkit-text-fill-color: transparent;">Research Areas</h2>
        <p style="font-size: 1.25rem; color: var(--text-secondary); margin-bottom: 3rem;">Experimental projects and practical implementations</p>

        <div class="service-grid">
            <div class="service-card" data-popup="website-popup">
                <div class="service-animation" id="anim-website"></div>
                <h3 style="font-size: 1.5rem; font-weight: 700; margin-bottom: 1rem; font-family: 'Space Grotesk', sans-serif;">Website Development</h3>
                <p style="color: var(--text-secondary); margin-bottom: 1rem;">Complete responsive web solutions with modern frameworks and animations</p>
                <div style="font-size: 1.75rem; font-weight: 900; color: var(--cyber-blue);">$350</div>
            </div>

            <div class="service-card" data-popup="webapp-popup">
                <div class="service-animation" id="anim-webapp"></div>
                <h3 style="font-size: 1.5rem; font-weight: 700; margin-bottom: 1rem; font-family: 'Space Grotesk', sans-serif;">Web Application Research</h3>
                <p style="color: var(--text-secondary); margin-bottom: 1rem;">Advanced full-stack applications with scalable architecture</p>
                <div style="color: var(--text-secondary);">From $3500</div>
            </div>

            <div class="service-card" data-popup="web3-popup">
                <div class="service-animation" id="anim-web3"></div>
                <h3 style="font-size: 1.5rem; font-weight: 700; margin-bottom: 1rem; font-family: 'Space Grotesk', sans-serif;">Web3 & Blockchain Lab</h3>
                <p style="color: var(--text-secondary); margin-bottom: 1rem;">Decentralized applications and smart contract development</p>
                <div style="color: var(--text-secondary);">From $1000</div>
            </div>

            <div class="service-card" data-popup="automation-popup">
                <div class="service-animation" id="anim-automation"></div>
                <h3 style="font-size: 1.5rem; font-weight: 700; margin-bottom: 1rem; font-family: 'Space Grotesk', sans-serif;">Automation Systems</h3>
                <p style="color: var(--text-secondary); margin-bottom: 1rem;">Intelligent workflow automation and optimization</p>
                <div style="color: var(--text-secondary);">From $100</div>
            </div>

            <div class="service-card" data-popup="mobile-popup">
                <div class="service-animation" id="anim-mobile"></div>
                <h3 style="font-size: 1.5rem; font-weight: 700; margin-bottom: 1rem; font-family: 'Space Grotesk', sans-serif;">Mobile Platforms</h3>
                <p style="color: var(--text-secondary); margin-bottom: 1rem;">Cross-platform mobile solutions for iOS and Android</p>
                <div style="color: var(--text-secondary);">From $800</div>
            </div>

            <div class="service-card" data-popup="backend-popup">
                <div class="service-animation" id="anim-backend"></div>
                <h3 style="font-size: 1.5rem; font-weight: 700; margin-bottom: 1rem; font-family: 'Space Grotesk', sans-serif;">Backend Infrastructure</h3>
                <p style="color: var(--text-secondary); margin-bottom: 1rem;">Robust server architecture and API development</p>
                <div style="color: var(--text-secondary);">From $600</div>
            </div>
        </div>
    </section>

    <section id="contact">
        <h2 style="font-size: clamp(3rem, 8vw, 5rem); font-weight: 900; margin-bottom: 2rem; background: linear-gradient(135deg, var(--bright-purple), var(--cyber-blue)); -webkit-background-clip: text; -webkit-text-fill-color: transparent;">Initiate Contact</h2>
        <p style="font-size: 1.5rem; text-align: center; margin-bottom: 3rem; max-width: 700px; color: var(--text-secondary);">
            Ready to collaborate on groundbreaking digital projects?
        </p>
        <div style="display: flex; gap: 1.5rem; flex-wrap: wrap; justify-content: center;">
            <a href="https://t.me/HE8ST" class="cta-button">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm4.64 6.8c-.15 1.58-.8 5.42-1.13 7.19-.14.75-.42 1-.68 1.03-.58.05-1.02-.38-1.58-.75-.88-.58-1.38-.94-2.23-1.5-.99-.65-.35-1.01.22-1.59.15-.15 2.71-2.48 2.76-2.69a.2.2 0 0 0-.05-.18c-.06-.05-.14-.03-.21-.02-.09.02-1.49.95-4.22 2.79-.4.27-.76.41-1.08.4-.36-.01-1.04-.2-1.55-.37-.63-.2-1.12-.31-1.08-.66.02-.18.27-.36.74-.55 2.92-1.27 4.86-2.11 5.83-2.51 2.78-1.16 3.35-1.36 3.73-1.36.08 0 .27.02.39.12.1.08.13.19.14.27-.01.06.01.24 0 .38z"/>
                </svg>
                Telegram
            </a>
            <a href="https://x.com/0xhe8st" class="cta-button">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
                </svg>
                X (Twitter)
            </a>
        </div>
    </section>

    <footer>
        <div class="social-links">
            <a href="https://x.com/0xhe8st" class="social-icon" target="_blank">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
                </svg>
            </a>
            <a href="https://instagram.com" class="social-icon" target="_blank">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
                </svg>
            </a>
            <a href="https://youtube.com" class="social-icon" target="_blank">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
                </svg>
            </a>
        </div>
        <p style="color: var(--text-secondary); font-size: 0.9rem;">© 2025 HE8ST. All rights reserved.</p>
    </footer>

    <!-- Popups -->
    <div id="offer-popup" class="popup">
        <div class="popup-content">
            <span class="close-popup">✕</span>
            <div style="padding: 3rem;">
                <h3 style="font-size: 2.5rem; font-weight: 900; margin-bottom: 1.5rem; background: linear-gradient(135deg, var(--bright-purple), var(--cyber-blue)); -webkit-background-clip: text; -webkit-text-fill-color: transparent;">$350 Basic Website Build</h3>
                <div style="margin-bottom: 2rem; line-height: 1.8; color: var(--text-secondary);">
                    <p style="margin-bottom: 1rem;">✨ Complete responsive website development</p>
                    <p style="margin-bottom: 1rem;">🎨 Modern design with smooth animations</p>
                    <p style="margin-bottom: 1rem;">⚡ Lightning-fast performance optimization</p>
                    <p style="margin-bottom: 1rem;">📱 Mobile-first responsive design</p>
                    <p style="margin-bottom: 1rem;">🔍 SEO-optimized structure</p>
                    <p style="margin-bottom: 1rem;">⏱️ 48-hour delivery guarantee</p>
                </div>
                <div style="display: flex; gap: 1rem; flex-wrap: wrap;">
                    <a href="https://t.me/HE8ST" class="cta-button" style="flex: 1; justify-content: center;">Telegram</a>
                    <a href="https://x.com/0xhe8st" class="cta-button" style="flex: 1; justify-content: center;">X (Twitter)</a>
                </div>
            </div>
        </div>
    </div>

    <div id="web3-popup" class="popup">
        <div class="popup-content">
            <span class="close-popup">✕</span>
            <div style="padding: 3rem;">
                <h3 style="font-size: 2rem; font-weight: 900; margin-bottom: 1.5rem; color: var(--bright-purple);">Web3 & Blockchain Lab</h3>
                <p style="margin-bottom: 2rem; line-height: 1.8; color: var(--text-secondary);">Cutting-edge decentralized applications on Solana and other blockchains. Smart contract development, DeFi protocols, and NFT platforms built with security and efficiency in mind.</p>
                <div style="display: flex; gap: 1rem; flex-wrap: wrap;">
                    <a href="https://t.me/HE8ST" class="cta-button" style="flex: 1; justify-content: center;">Telegram</a>
                    <a href="https://x.com/0xhe8st" class="cta-button" style="flex: 1; justify-content: center;">X (Twitter)</a>
                </div>
            </div>
        </div>
    </div>

    <div id="automation-popup" class="popup">
        <div class="popup-content">
            <span class="close-popup">✕</span>
            <div style="padding: 3rem;">
                <h3 style="font-size: 2rem; font-weight: 900; margin-bottom: 1.5rem; color: var(--bright-purple);">Automation Systems</h3>
                <p style="margin-bottom: 2rem; line-height: 1.8; color: var(--text-secondary);">Intelligent Python and Flask automation scripts to streamline workflows, boost productivity, and eliminate repetitive tasks across your operations.</p>
                <div style="display: flex; gap: 1rem; flex-wrap: wrap;">
                    <a href="https://t.me/HE8ST" class="cta-button" style="flex: 1; justify-content: center;">Telegram</a>
                    <a href="https://x.com/0xhe8st" class="cta-button" style="flex: 1; justify-content: center;">X (Twitter)</a>
                </div>
            </div>
        </div>
    </div>

    <div id="mobile-popup" class="popup">
        <div class="popup-content">
            <span class="close-popup">✕</span>
            <div style="padding: 3rem;">
                <h3 style="font-size: 2rem; font-weight: 900; margin-bottom: 1.5rem; color: var(--bright-purple);">Mobile Platforms</h3>
                <p style="margin-bottom: 2rem; line-height: 1.8; color: var(--text-secondary);">Native and cross-platform mobile applications for iOS and Android. Built with React Native and Flutter for seamless performance and user experience.</p>
                <div style="display: flex; gap: 1rem; flex-wrap: wrap;">
                    <a href="https://t.me/HE8ST" class="cta-button" style="flex: 1; justify-content: center;">Telegram</a>
                    <a href="https://x.com/0xhe8st" class="cta-button" style="flex: 1; justify-content: center;">X (Twitter)</a>
                </div>
            </div>
        </div>
    </div>

    <div id="backend-popup" class="popup">
        <div class="popup-content">
            <span class="close-popup">✕</span>
            <div style="padding: 3rem;">
                <h3 style="font-size: 2rem; font-weight: 900; margin-bottom: 1.5rem; color: var(--bright-purple);">Backend Infrastructure</h3>
                <p style="margin-bottom: 2rem; line-height: 1.8; color: var(--text-secondary);">Robust, scalable backend systems with secure REST and GraphQL APIs. High-performance server architecture built for reliability and growth.</p>
                <div style="display: flex; gap: 1rem; flex-wrap: wrap;">
                    <a href="https://t.me/HE8ST" class="cta-button" style="flex: 1; justify-content: center;">Telegram</a>
                    <a href="https://x.com/0xhe8st" class="cta-button" style="flex: 1; justify-content: center;">X (Twitter)</a>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Interactive cursor background
        const canvas = document.getElementById('cursor-canvas');
        const ctx = canvas.getContext('2d');
        let particles = [];
        let mouseX = window.innerWidth / 2;
        let mouseY = window.innerHeight / 2;
        let isMobile = window.innerWidth <= 768;

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        class Particle {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.size = Math.random() * 3 + 1;
                this.speedX = Math.random() * 2 - 1;
                this.speedY = Math.random() * 2 - 1;
                this.color = Math.random() > 0.5 ? 'rgba(124, 58, 237, 0.5)' : 'rgba(0, 212, 255, 0.5)';
            }

            update() {
                this.x += this.speedX;
                this.y += this.speedY;

                if (!isMobile) {
                    const dx = mouseX - this.x;
                    const dy = mouseY - this.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance < 150) {
                        const force = (150 - distance) / 150;
                        this.x -= (dx / distance) * force * 2;
                        this.y -= (dy / distance) * force * 2;
                    }
                }

                if (this.x < 0 || this.x > canvas.width) this.speedX *= -1;
                if (this.y < 0 || this.y > canvas.height) this.speedY *= -1;
            }

            draw() {
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
            }
        }

        function init() {
            particles = [];
            const particleCount = isMobile ? 50 : 100;
            for (let i = 0; i < particleCount; i++) {
                particles.push(new Particle(Math.random() * canvas.width, Math.random() * canvas.height));
            }
        }

        function animate() {
            ctx.fillStyle = 'rgba(10, 0, 21, 0.1)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            particles.forEach(particle => {
                particle.update();
                particle.draw();
            });

            particles.forEach((p1, i) => {
                particles.slice(i + 1).forEach(p2 => {
                    const dx = p1.x - p2.x;
                    const dy = p1.y - p2.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);

                    if (distance < 100) {
                        ctx.strokeStyle = `rgba(124, 58, 237, ${0.2 - distance / 500})`;
                        ctx.lineWidth = 1;
                        ctx.beginPath();
                        ctx.moveTo(p1.x, p1.y);
                        ctx.lineTo(p2.x, p2.y);
                        ctx.stroke();
                    }
                });
            });

            requestAnimationFrame(animate);
        }

        document.addEventListener('mousemove', (e) => {
            mouseX = e.clientX;
            mouseY = e.clientY;
        });

        window.addEventListener('resize', () => {
            isMobile = window.innerWidth <= 768;
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            init();
        });

        // Service animations
        function createWebsiteAnimation() {
            const container = document.getElementById('anim-website');
            container.innerHTML = '<div style="width: 100%; height: 100%; display: flex; flex-direction: column; gap: 10px; padding: 20px; background: rgba(124, 58, 237, 0.1); border-radius: 10px;"><div style="width: 60%; height: 20px; background: linear-gradient(90deg, var(--bright-purple), var(--cyber-blue)); border-radius: 5px; animation: pulse 2s infinite;"></div><div style="width: 80%; height: 15px; background: rgba(124, 58, 237, 0.3); border-radius: 5px; animation: pulse 2s 0.2s infinite;"></div><div style="width: 70%; height: 15px; background: rgba(0, 212, 255, 0.3); border-radius: 5px; animation: pulse 2s 0.4s infinite;"></div></div>';
        }

        function createWebAppAnimation() {
            const container = document.getElementById('anim-webapp');
            container.innerHTML = '<div style="width: 100%; height: 100%; display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; padding: 20px;"><div style="background: var(--bright-purple); border-radius: 5px; animation: bounce 1.5s infinite;"></div><div style="background: var(--cyber-blue); border-radius: 5px; animation: bounce 1.5s 0.2s infinite;"></div><div style="background: var(--neon-green); border-radius: 5px; animation: bounce 1.5s 0.4s infinite;"></div></div>';
        }

        function createWeb3Animation() {
            const container = document.getElementById('anim-web3');
            container.innerHTML = '<div style="width: 100%; height: 100%; display: flex; align-items: center; justify-content: center; position: relative;"><div style="width: 60px; height: 60px; border: 3px solid var(--bright-purple); border-radius: 50%; position: absolute; animation: rotate 3s linear infinite;"></div><div style="width: 40px; height: 40px; background: linear-gradient(135deg, var(--cyber-blue), var(--neon-green)); border-radius: 50%; animation: pulse 2s infinite;"></div></div>';
        }

        function createAutomationAnimation() {
            const container = document.getElementById('anim-automation');
            container.innerHTML = '<div style="width: 100%; height: 100%; display: flex; align-items: center; justify-content: space-around; padding: 20px;"><div style="width: 30px; height: 30px; background: var(--bright-purple); border-radius: 5px; animation: slide 2s infinite;"></div><div style="width: 30px; height: 30px; background: var(--cyber-blue); border-radius: 5px; animation: slide 2s 0.5s infinite;"></div><div style="width: 30px; height: 30px; background: var(--neon-green); border-radius: 5px; animation: slide 2s 1s infinite;"></div></div>';
        }

        function createMobileAnimation() {
            const container = document.getElementById('anim-mobile');
            container.innerHTML = '<div style="width: 100%; height: 100%; display: flex; align-items: center; justify-content: center;"><div style="width: 80px; height: 130px; border: 3px solid var(--bright-purple); border-radius: 15px; position: relative; animation: shake 3s infinite;"><div style="width: 50%; height: 5px; background: var(--cyber-blue); border-radius: 5px; position: absolute; top: 50%; left: 25%; animation: pulse 1.5s infinite;"></div></div></div>';
        }

        function createBackendAnimation() {
            const container = document.getElementById('anim-backend');
            container.innerHTML = '<div style="width: 100%; height: 100%; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 15px;"><div style="display: flex; gap: 10px;"><div style="width: 15px; height: 15px; background: var(--bright-purple); border-radius: 50%; animation: pulse 1s infinite;"></div><div style="width: 15px; height: 15px; background: var(--cyber-blue); border-radius: 50%; animation: pulse 1s 0.2s infinite;"></div><div style="width: 15px; height: 15px; background: var(--neon-green); border-radius: 50%; animation: pulse 1s 0.4s infinite;"></div></div><div style="width: 100px; height: 3px; background: linear-gradient(90deg, var(--bright-purple), var(--cyber-blue)); border-radius: 5px;"></div></div>';
        }

        // Popup handling
        const serviceCards = document.querySelectorAll('.service-card');
        const popups = document.querySelectorAll('.popup');
        const closeButtons = document.querySelectorAll('.close-popup');

        document.getElementById('offer-card').addEventListener('click', () => {
            document.getElementById('offer-popup').style.display = 'block';
        });

        serviceCards.forEach(card => {
            card.addEventListener('click', () => {
                const popupId = card.dataset.popup;
                document.getElementById(popupId).style.display = 'block';
            });
        });

        closeButtons.forEach(btn => {
            btn.addEventListener('click', () => {
                popups.forEach(popup => popup.style.display = 'none');
            });
        });

        popups.forEach(popup => {
            popup.addEventListener('click', (e) => {
                if (e.target === popup) {
                    popup.style.display = 'none';
                }
            });
        });

        // Mobile menu
        const mobileMenuBtn = document.querySelector('.mobile-menu-btn');
        const navLinks = document.querySelector('.nav-links');
        
        if (mobileMenuBtn) {
            mobileMenuBtn.addEventListener('click', () => {
                navLinks.style.display = navLinks.style.display === 'flex' ? 'none' : 'flex';
                if (navLinks.style.display === 'flex') {
                    navLinks.style.position = 'absolute';
                    navLinks.style.top = '70px';
                    navLinks.style.left = '0';
                    navLinks.style.right = '0';
                    navLinks.style.background = 'rgba(10, 0, 21, 0.95)';
                    navLinks.style.flexDirection = 'column';
                    navLinks.style.padding = '2rem';
                }
            });
        }

        // Smooth scroll
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    target.scrollIntoView({ behavior: 'smooth' });
                    if (window.innerWidth <= 768) {
                        navLinks.style.display = 'none';
                    }
                }
            });
        });

        // CSS animations
        const style = document.createElement('style');
        style.textContent = `
            @keyframes pulse {
                0%, 100% { opacity: 0.5; transform: scale(1); }
                50% { opacity: 1; transform: scale(1.05); }
            }
            @keyframes bounce {
                0%, 100% { transform: translateY(0); }
                50% { transform: translateY(-20px); }
            }
            @keyframes rotate {
                0% { transform: rotate(0deg); }
                100% { transform: rotate(360deg); }
            }
            @keyframes slide {
                0%, 100% { transform: translateX(-50px); opacity: 0; }
                50% { transform: translateX(0); opacity: 1; }
            }
            @keyframes shake {
                0%, 100% { transform: rotate(0deg); }
                25% { transform: rotate(-5deg); }
                75% { transform: rotate(5deg); }
            }
        `;
        document.head.appendChild(style);

        // Initialize
        init();
        animate();
        createWebsiteAnimation();
        createWebAppAnimation();
        createWeb3Animation();
        createAutomationAnimation();
        createMobileAnimation();
        createBackendAnimation();
    </script>
</body>
</html><a href="https://x.com/0xhe8st" class="cta-button" style="flex: 1; justify-content: center;">X (Twitter)</a>
                </div>
            </div>
        </div>
    </div>

