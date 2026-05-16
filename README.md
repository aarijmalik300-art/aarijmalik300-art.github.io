<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEXORA AI — Create Viral AI Influencers That Look 100% Human</title>
    <meta name="description" content="Generate ultra realistic AI influencers with perfect face consistency, cinematic quality, and full-body realism. 4K & 8K quality.">
    
    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&family=Playfair+Display:ital,wght@0,400;0,500;0,600;0,700;0,800;0,900;1,400;1,700&family=Space+Grotesk:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- GSAP & Plugins -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/TextPlugin.min.js"></script>
    
    <!-- Three.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>

    <style>
        /* ============================================
           CSS CUSTOM PROPERTIES & RESET
        ============================================ */
        :root {
            --gold: #C9A84C;
            --gold-light: #F0D080;
            --gold-bright: #FFD700;
            --gold-dark: #8B6914;
            --black: #000000;
            --black-deep: #050505;
            --black-card: #0A0A0A;
            --black-glass: rgba(10, 10, 10, 0.6);
            --white: #FFFFFF;
            --white-dim: rgba(255,255,255,0.7);
            --white-subtle: rgba(255,255,255,0.08);
            --border-gold: rgba(201, 168, 76, 0.3);
            --border-gold-bright: rgba(201, 168, 76, 0.7);
            --glow-gold: 0 0 40px rgba(201, 168, 76, 0.4);
            --glow-gold-strong: 0 0 80px rgba(201, 168, 76, 0.6);
            --font-display: 'Playfair Display', serif;
            --font-body: 'Inter', sans-serif;
            --font-tech: 'Space Grotesk', sans-serif;
        }

        *, *::before, *::after {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
            overflow-x: hidden;
        }

        body {
            background-color: var(--black-deep);
            color: var(--white);
            font-family: var(--font-body);
            overflow-x: hidden;
            cursor: none;
        }

        ::selection {
            background: rgba(201, 168, 76, 0.3);
            color: var(--gold-light);
        }

        /* ============================================
           CUSTOM CURSOR
        ============================================ */
        .cursor {
            position: fixed;
            width: 12px;
            height: 12px;
            background: var(--gold);
            border-radius: 50%;
            pointer-events: none;
            z-index: 99999;
            transition: transform 0.1s ease;
            mix-blend-mode: difference;
        }

        .cursor-follower {
            position: fixed;
            width: 40px;
            height: 40px;
            border: 1px solid rgba(201, 168, 76, 0.5);
            border-radius: 50%;
            pointer-events: none;
            z-index: 99998;
            transition: transform 0.15s ease, width 0.3s ease, height 0.3s ease, opacity 0.3s ease;
        }

        .cursor-follower.hover {
            width: 60px;
            height: 60px;
            border-color: var(--gold);
            background: rgba(201, 168, 76, 0.05);
        }

        /* ============================================
           SCROLLBAR
        ============================================ */
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: #000; }
        ::-webkit-scrollbar-thumb { background: linear-gradient(180deg, var(--gold), var(--gold-dark)); border-radius: 2px; }

        /* ============================================
           CANVAS / PARTICLE BG
        ============================================ */
        #particle-canvas {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            z-index: 0;
            pointer-events: none;
        }

        /* ============================================
           AMBIENT MOUSE LIGHT
        ============================================ */
        #mouse-light {
            position: fixed;
            width: 600px;
            height: 600px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(201,168,76,0.06) 0%, transparent 70%);
            pointer-events: none;
            z-index: 1;
            transform: translate(-50%, -50%);
            transition: opacity 0.3s ease;
        }

        /* ============================================
           NAVIGATION
        ============================================ */
        nav {
            position: fixed;
            top: 0; left: 0; right: 0;
            z-index: 1000;
            padding: 20px 60px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            transition: all 0.4s ease;
        }

        nav.scrolled {
            background: rgba(0,0,0,0.85);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-bottom: 1px solid var(--border-gold);
            padding: 14px 60px;
        }

        .nav-logo {
            display: flex;
            align-items: center;
            gap: 12px;
            text-decoration: none;
        }

        .nav-logo-icon {
            width: 38px;
            height: 38px;
            background: linear-gradient(135deg, var(--gold), var(--gold-dark));
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            font-weight: 900;
            color: #000;
            font-family: var(--font-tech);
            box-shadow: 0 0 20px rgba(201,168,76,0.4);
        }

        .nav-logo-text {
            font-family: var(--font-tech);
            font-size: 20px;
            font-weight: 700;
            background: linear-gradient(135deg, var(--gold-light), var(--gold));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: 2px;
        }

        .nav-links {
            display: flex;
            align-items: center;
            gap: 36px;
            list-style: none;
        }

        .nav-links a {
            color: rgba(255,255,255,0.7);
            text-decoration: none;
            font-size: 13px;
            font-weight: 500;
            letter-spacing: 1px;
            text-transform: uppercase;
            transition: color 0.3s ease;
            font-family: var(--font-tech);
        }

        .nav-links a:hover { color: var(--gold-light); }

        .nav-cta {
            background: linear-gradient(135deg, var(--gold), var(--gold-dark));
            color: #000 !important;
            padding: 10px 24px;
            border-radius: 8px;
            font-weight: 700 !important;
            letter-spacing: 1px;
            transition: all 0.3s ease !important;
            box-shadow: 0 0 20px rgba(201,168,76,0.3);
        }

        .nav-cta:hover {
            transform: translateY(-2px);
            box-shadow: 0 0 40px rgba(201,168,76,0.6) !important;
            color: #000 !important;
        }

        /* ============================================
           HERO SECTION
        ============================================ */
        #hero {
            position: relative;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 120px 60px 80px;
            overflow: hidden;
            z-index: 2;
        }

        .hero-bg-gradient {
            position: absolute;
            inset: 0;
            background: 
                radial-gradient(ellipse 80% 60% at 50% 0%, rgba(201,168,76,0.12) 0%, transparent 60%),
                radial-gradient(ellipse 50% 40% at 20% 80%, rgba(201,168,76,0.06) 0%, transparent 50%),
                radial-gradient(ellipse 50% 40% at 80% 70%, rgba(201,168,76,0.06) 0%, transparent 50%);
            z-index: 0;
        }

        .hero-grid-lines {
            position: absolute;
            inset: 0;
            background-image: 
                linear-gradient(rgba(201,168,76,0.04) 1px, transparent 1px),
                linear-gradient(90deg, rgba(201,168,76,0.04) 1px, transparent 1px);
            background-size: 60px 60px;
            z-index: 0;
        }

        .hero-badge {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            background: rgba(201,168,76,0.08);
            border: 1px solid var(--border-gold);
            border-radius: 100px;
            padding: 8px 20px;
            font-size: 12px;
            font-weight: 600;
            letter-spacing: 2px;
            text-transform: uppercase;
            color: var(--gold-light);
            font-family: var(--font-tech);
            margin-bottom: 32px;
            position: relative;
            z-index: 2;
            backdrop-filter: blur(10px);
        }

        .hero-badge-dot {
            width: 6px;
            height: 6px;
            background: var(--gold);
            border-radius: 50%;
            animation: pulse-dot 2s infinite;
        }

        @keyframes pulse-dot {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.5; transform: scale(1.5); }
        }

        .hero-headline {
            font-family: var(--font-display);
            font-size: clamp(48px, 7vw, 96px);
            font-weight: 800;
            line-height: 1.05;
            text-align: center;
            max-width: 1100px;
            position: relative;
            z-index: 2;
            margin-bottom: 28px;
        }

        .hero-headline .line-1 { color: var(--white); }
        .hero-headline .line-2 {
            background: linear-gradient(135deg, var(--gold-bright) 0%, var(--gold) 40%, var(--gold-light) 70%, var(--gold-bright) 100%);
            background-size: 200% 200%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            animation: gold-shimmer 4s ease infinite;
            display: block;
        }

        @keyframes gold-shimmer {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .hero-subheadline {
            font-size: clamp(16px, 2vw, 20px);
            color: rgba(255,255,255,0.6);
            text-align: center;
            max-width: 700px;
            line-height: 1.7;
            position: relative;
            z-index: 2;
            margin-bottom: 48px;
            font-weight: 300;
        }

        .hero-buttons {
            display: flex;
            gap: 20px;
            align-items: center;
            position: relative;
            z-index: 2;
            margin-bottom: 80px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .btn-primary {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            background: linear-gradient(135deg, var(--gold-light), var(--gold), var(--gold-dark));
            color: #000;
            padding: 18px 40px;
            border-radius: 12px;
            font-weight: 700;
            font-size: 15px;
            letter-spacing: 1px;
            text-decoration: none;
            transition: all 0.3s ease;
            box-shadow: 0 0 40px rgba(201,168,76,0.4), 0 4px 24px rgba(0,0,0,0.4);
            border: none;
            cursor: pointer;
            font-family: var(--font-tech);
            position: relative;
            overflow: hidden;
        }

        .btn-primary::before {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(135deg, rgba(255,255,255,0.2), transparent);
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .btn-primary:hover::before { opacity: 1; }
        .btn-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 0 60px rgba(201,168,76,0.7), 0 8px 40px rgba(0,0,0,0.5);
        }

        .btn-secondary {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            background: rgba(255,255,255,0.04);
            color: var(--white);
            padding: 18px 40px;
            border-radius: 12px;
            font-weight: 600;
            font-size: 15px;
            letter-spacing: 1px;
            text-decoration: none;
            transition: all 0.3s ease;
            border: 1px solid rgba(255,255,255,0.12);
            cursor: pointer;
            font-family: var(--font-tech);
            backdrop-filter: blur(10px);
        }

        .btn-secondary:hover {
            background: rgba(255,255,255,0.08);
            border-color: var(--border-gold);
            color: var(--gold-light);
            transform: translateY(-3px);
        }

        .play-icon {
            width: 32px;
            height: 32px;
            background: rgba(201,168,76,0.2);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 1px solid var(--border-gold);
        }

        /* ============================================
           HERO STATS
        ============================================ */
        .hero-stats {
            display: flex;
            gap: 60px;
            position: relative;
            z-index: 2;
            flex-wrap: wrap;
            justify-content: center;
            margin-bottom: 80px;
        }

        .hero-stat {
            text-align: center;
        }

        .hero-stat-number {
            font-family: var(--font-tech);
            font-size: 36px;
            font-weight: 700;
            background: linear-gradient(135deg, var(--gold-light), var(--gold));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .hero-stat-label {
            font-size: 12px;
            color: rgba(255,255,255,0.4);
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-top: 4px;
        }

        /* ============================================
           FLOATING CARDS
        ============================================ */
        .floating-cards {
            position: absolute;
            inset: 0;
            z-index: 3;
            pointer-events: none;
        }

        .float-card {
            position: absolute;
            background: rgba(10,10,10,0.8);
            border: 1px solid var(--border-gold);
            border-radius: 16px;
            padding: 16px 20px;
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            box-shadow: 0 8px 40px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.05);
            animation: float-anim 6s ease-in-out infinite;
        }

        .float-card:nth-child(2) { animation-delay: -2s; }
        .float-card:nth-child(3) { animation-delay: -4s; }

        @keyframes float-anim {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-15px) rotate(1deg); }
        }

        .float-card-1 {
            top: 25%;
            left: 4%;
            min-width: 200px;
        }

        .float-card-2 {
            top: 30%;
            right: 4%;
            min-width: 180px;
        }

        .float-card-3 {
            bottom: 25%;
            left: 7%;
            min-width: 160px;
        }

        .float-card-label {
            font-size: 10px;
            color: var(--gold);
            text-transform: uppercase;
            letter-spacing: 2px;
            font-family: var(--font-tech);
            margin-bottom: 8px;
        }

        .float-card-value {
            font-size: 20px;
            font-weight: 700;
            color: var(--white);
            font-family: var(--font-tech);
        }

        .float-card-sub {
            font-size: 11px;
            color: rgba(255,255,255,0.4);
            margin-top: 4px;
        }

        .live-dot {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            font-size: 11px;
            color: #4ade80;
        }

        .live-dot::before {
            content: '';
            width: 6px;
            height: 6px;
            background: #4ade80;
            border-radius: 50%;
            display: inline-block;
            animation: pulse-dot 1.5s infinite;
        }

        /* ============================================
           GENERATION PREVIEW / BEFORE-AFTER
        ============================================ */
        .hero-preview {
            position: relative;
            z-index: 2;
            width: 100%;
            max-width: 1200px;
            margin: 0 auto;
        }

        .preview-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 12px;
            margin-top: 20px;
        }

        .preview-card {
            aspect-ratio: 3/4;
            border-radius: 16px;
            overflow: hidden;
            position: relative;
            border: 1px solid var(--border-gold);
            transition: transform 0.4s ease, box-shadow 0.4s ease;
        }

        .preview-card:hover {
            transform: translateY(-8px) scale(1.02);
            box-shadow: 0 20px 60px rgba(201,168,76,0.2);
        }

        .preview-card-inner {
            width: 100%;
            height: 100%;
            position: relative;
        }

        /* AI generated placeholder images with CSS art */
        .preview-card-bg {
            position: absolute;
            inset: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }

        .ai-person {
            width: 100%;
            height: 100%;
            position: relative;
        }

        /* Person 1 - Female Fashion */
        .person-1 { background: linear-gradient(160deg, #1a1008 0%, #0d0d0d 40%, #0a0805 100%); }
        .person-1::after {
            content: '';
            position: absolute;
            inset: 0;
            background: 
                radial-gradient(ellipse 60% 50% at 50% 30%, rgba(255,200,150,0.15) 0%, transparent 60%),
                radial-gradient(ellipse 40% 60% at 50% 60%, rgba(180,120,80,0.1) 0%, transparent 60%);
        }

        .person-2 { background: linear-gradient(160deg, #08101a 0%, #0d0d0d 40%, #050a0d 100%); }
        .person-3 { background: linear-gradient(160deg, #100808 0%, #0d0d0d 40%, #0d0505 100%); }
        .person-4 { background: linear-gradient(160deg, #0a100a 0%, #0d0d0d 40%, #050d05 100%); }
        .person-5 { background: linear-gradient(160deg, #100a14 0%, #0d0d0d 40%, #0d0510 100%); }

        .preview-card-overlay {
            position: absolute;
            inset: 0;
            background: linear-gradient(0deg, rgba(0,0,0,0.8) 0%, transparent 50%);
            z-index: 2;
        }

        .preview-card-info {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            padding: 16px;
            z-index: 3;
        }

        .preview-card-name {
            font-size: 13px;
            font-weight: 700;
            color: var(--white);
            font-family: var(--font-tech);
        }

        .preview-card-tag {
            font-size: 10px;
            color: var(--gold);
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-top: 4px;
        }

        .preview-badge {
            position: absolute;
            top: 12px;
            right: 12px;
            background: rgba(0,0,0,0.7);
            border: 1px solid var(--border-gold);
            border-radius: 6px;
            padding: 4px 10px;
            font-size: 10px;
            font-weight: 700;
            color: var(--gold);
            font-family: var(--font-tech);
            letter-spacing: 1px;
            z-index: 3;
            backdrop-filter: blur(10px);
        }

        /* Human silhouette SVG */
        .silhouette-container {
            position: absolute;
            inset: 0;
            display: flex;
            align-items: flex-end;
            justify-content: center;
            overflow: hidden;
        }

        /* ============================================
           SECTION UTILITIES
        ============================================ */
        section {
            position: relative;
            z-index: 2;
        }

        .section-padding {
            padding: 120px 60px;
        }

        .section-tag {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            background: rgba(201,168,76,0.08);
            border: 1px solid var(--border-gold);
            border-radius: 100px;
            padding: 6px 16px;
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 2px;
            text-transform: uppercase;
            color: var(--gold);
            font-family: var(--font-tech);
            margin-bottom: 24px;
        }

        .section-title {
            font-family: var(--font-display);
            font-size: clamp(36px, 5vw, 64px);
            font-weight: 800;
            line-height: 1.1;
            margin-bottom: 20px;
        }

        .section-title .gold {
            background: linear-gradient(135deg, var(--gold-bright), var(--gold), var(--gold-light));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .section-subtitle {
            font-size: 18px;
            color: rgba(255,255,255,0.5);
            line-height: 1.7;
            max-width: 600px;
            font-weight: 300;
        }

        .text-center { text-align: center; }
        .mx-auto { margin-left: auto; margin-right: auto; }

        /* ============================================
           TRUSTED BY
        ============================================ */
        #trusted {
            padding: 40px 60px;
            background: rgba(201,168,76,0.02);
            border-top: 1px solid var(--border-gold);
            border-bottom: 1px solid var(--border-gold);
        }

        .trusted-inner {
            display: flex;
            align-items: center;
            gap: 60px;
            overflow: hidden;
            position: relative;
        }

        .trusted-label {
            font-size: 11px;
            color: rgba(255,255,255,0.3);
            text-transform: uppercase;
            letter-spacing: 3px;
            white-space: nowrap;
            font-family: var(--font-tech);
            flex-shrink: 0;
        }

        .trusted-scroll {
            display: flex;
            gap: 60px;
            align-items: center;
            animation: scroll-left 20s linear infinite;
            flex-shrink: 0;
        }

        @keyframes scroll-left {
            0% { transform: translateX(0); }
            100% { transform: translateX(-50%); }
        }

        .trusted-brand {
            font-size: 16px;
            font-weight: 700;
            color: rgba(255,255,255,0.25);
            white-space: nowrap;
            font-family: var(--font-tech);
            letter-spacing: 2px;
            text-transform: uppercase;
            transition: color 0.3s ease;
        }

        /* ============================================
           FEATURES SECTION
        ============================================ */
        #features {
            background: linear-gradient(180deg, transparent, rgba(201,168,76,0.02) 50%, transparent);
        }

        .features-header {
            text-align: center;
            max-width: 700px;
            margin: 0 auto 80px;
        }

        .features-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 24px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .feature-card {
            background: rgba(10,10,10,0.8);
            border: 1px solid var(--border-gold);
            border-radius: 24px;
            padding: 40px 36px;
            position: relative;
            overflow: hidden;
            transition: all 0.4s ease;
            backdrop-filter: blur(10px);
        }

        .feature-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, var(--gold), transparent);
            opacity: 0;
            transition: opacity 0.4s ease;
        }

        .feature-card:hover::before { opacity: 1; }

        .feature-card:hover {
            border-color: var(--border-gold-bright);
            transform: translateY(-8px);
            box-shadow: 0 20px 60px rgba(201,168,76,0.1), 0 0 0 1px rgba(201,168,76,0.1);
        }

        .feature-card.featured {
            grid-column: span 1;
            background: linear-gradient(135deg, rgba(201,168,76,0.08), rgba(10,10,10,0.9));
            border-color: var(--border-gold-bright);
        }

        .feature-icon {
            width: 56px;
            height: 56px;
            background: linear-gradient(135deg, rgba(201,168,76,0.2), rgba(201,168,76,0.05));
            border: 1px solid var(--border-gold);
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 24px;
            font-size: 24px;
        }

        .feature-number {
            position: absolute;
            top: 24px;
            right: 24px;
            font-size: 64px;
            font-weight: 900;
            color: rgba(201,168,76,0.06);
            font-family: var(--font-tech);
            line-height: 1;
        }

        .feature-title {
            font-family: var(--font-tech);
            font-size: 20px;
            font-weight: 700;
            color: var(--white);
            margin-bottom: 12px;
        }

        .feature-desc {
            font-size: 14px;
            color: rgba(255,255,255,0.5);
            line-height: 1.7;
        }

        .feature-tags {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 24px;
        }

        .feature-tag {
            background: rgba(201,168,76,0.08);
            border: 1px solid var(--border-gold);
            border-radius: 6px;
            padding: 4px 10px;
            font-size: 11px;
            color: var(--gold);
            font-family: var(--font-tech);
            letter-spacing: 1px;
        }

        /* ============================================
           GENERATOR PANEL
        ============================================ */
        #generator {
            background: linear-gradient(180deg, transparent, rgba(0,0,0,0.5));
        }

        .generator-wrapper {
            max-width: 1400px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: 400px 1fr;
            gap: 24px;
            align-items: start;
        }

        .generator-panel {
            background: rgba(8,8,8,0.9);
            border: 1px solid var(--border-gold);
            border-radius: 24px;
            padding: 32px;
            backdrop-filter: blur(20px);
            position: sticky;
            top: 100px;
        }

        .generator-panel-title {
            font-family: var(--font-tech);
            font-size: 14px;
            font-weight: 700;
            color: var(--gold);
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 24px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .panel-section {
            margin-bottom: 28px;
        }

        .panel-label {
            font-size: 11px;
            color: rgba(255,255,255,0.4);
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 12px;
            font-family: var(--font-tech);
        }

        .prompt-box {
            width: 100%;
            background: rgba(255,255,255,0.04);
            border: 1px solid rgba(255,255,255,0.08);
            border-radius: 12px;
            padding: 16px;
            color: var(--white);
            font-size: 14px;
            resize: none;
            outline: none;
            font-family: var(--font-body);
            transition: border-color 0.3s ease;
            line-height: 1.6;
        }

        .prompt-box:focus {
            border-color: var(--border-gold);
            background: rgba(201,168,76,0.03);
        }

        .prompt-box::placeholder { color: rgba(255,255,255,0.25); }

        .toggle-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
        }

        .toggle-btn {
            background: rgba(255,255,255,0.04);
            border: 1px solid rgba(255,255,255,0.08);
            border-radius: 10px;
            padding: 10px 14px;
            color: rgba(255,255,255,0.5);
            font-size: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-family: var(--font-tech);
            text-align: center;
        }

        .toggle-btn:hover,
        .toggle-btn.active {
            background: rgba(201,168,76,0.1);
            border-color: var(--border-gold);
            color: var(--gold);
        }

        .slider-row {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .slider-label-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }

        .slider-value {
            font-family: var(--font-tech);
            font-size: 13px;
            color: var(--gold);
            font-weight: 600;
        }

        input[type="range"] {
            -webkit-appearance: none;
            width: 100%;
            height: 3px;
            background: rgba(255,255,255,0.1);
            border-radius: 3px;
            outline: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            background: linear-gradient(135deg, var(--gold-light), var(--gold));
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 0 10px rgba(201,168,76,0.5);
        }

        .quality-switch {
            display: flex;
            background: rgba(255,255,255,0.04);
            border: 1px solid rgba(255,255,255,0.08);
            border-radius: 10px;
            padding: 4px;
            gap: 4px;
        }

        .quality-option {
            flex: 1;
            text-align: center;
            padding: 8px;
            border-radius: 8px;
            font-size: 12px;
            font-weight: 700;
            font-family: var(--font-tech);
            cursor: pointer;
            transition: all 0.3s ease;
            color: rgba(255,255,255,0.4);
        }

        .quality-option.active {
            background: linear-gradient(135deg, var(--gold), var(--gold-dark));
            color: #000;
        }

        .generate-btn {
            width: 100%;
            background: linear-gradient(135deg, var(--gold-light), var(--gold), var(--gold-dark));
            color: #000;
            border: none;
            border-radius: 14px;
            padding: 18px;
            font-size: 15px;
            font-weight: 800;
            font-family: var(--font-tech);
            cursor: pointer;
            letter-spacing: 1px;
            transition: all 0.3s ease;
            box-shadow: 0 0 40px rgba(201,168,76,0.4);
            position: relative;
            overflow: hidden;
            text-transform: uppercase;
        }

        .generate-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 0 70px rgba(201,168,76,0.7);
        }

        .generate-btn.generating {
            animation: generating-pulse 1.5s ease infinite;
        }

        @keyframes generating-pulse {
            0%, 100% { box-shadow: 0 0 40px rgba(201,168,76,0.4); }
            50% { box-shadow: 0 0 80px rgba(201,168,76,0.8); }
        }

        /* Generator Output */
        .generator-output {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 16px;
        }

        .output-card {
            aspect-ratio: 3/4;
            border-radius: 20px;
            border: 1px solid var(--border-gold);
            overflow: hidden;
            position: relative;
            background: rgba(10,10,10,0.8);
        }

        .output-placeholder {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 16px;
            color: rgba(255,255,255,0.2);
        }

        .output-generating {
            position: absolute;
            inset: 0;
            background: rgba(0,0,0,0.9);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 20px;
            z-index: 5;
        }

        .gen-ring {
            width: 60px;
            height: 60px;
            border: 2px solid rgba(201,168,76,0.2);
            border-top-color: var(--gold);
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin { to { transform: rotate(360deg); } }

        .gen-text {
            font-size: 13px;
            color: var(--gold);
            font-family: var(--font-tech);
            letter-spacing: 2px;
            text-transform: uppercase;
            animation: gen-text-pulse 1.5s ease infinite;
        }

        @keyframes gen-text-pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.4; }
        }

        .progress-bar {
            width: 120px;
            height: 2px;
            background: rgba(255,255,255,0.1);
            border-radius: 2px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--gold-dark), var(--gold-light));
            border-radius: 2px;
            width: 0%;
            transition: width 0.3s ease;
        }

        /* ============================================
           SHOWCASE GALLERY
        ============================================ */
        #showcase {
            overflow: hidden;
        }

        .showcase-header {
            text-align: center;
            max-width: 700px;
            margin: 0 auto 60px;
        }

        .showcase-row {
            display: flex;
            gap: 16px;
            margin-bottom: 16px;
            overflow: hidden;
        }

        .showcase-row-inner {
            display: flex;
            gap: 16px;
            animation: scroll-showcase 30s linear infinite;
        }

        .showcase-row:nth-child(2) .showcase-row-inner {
            animation-direction: reverse;
            animation-duration: 25s;
        }

        @keyframes scroll-showcase {
            0% { transform: translateX(0); }
            100% { transform: translateX(-50%); }
        }

        .showcase-item {
            flex-shrink: 0;
            width: 200px;
            aspect-ratio: 3/4;
            border-radius: 16px;
            overflow: hidden;
            border: 1px solid var(--border-gold);
            position: relative;
            transition: transform 0.3s ease;
        }

        .showcase-item:hover { transform: scale(1.05); }

        .showcase-item-bg {
            width: 100%;
            height: 100%;
            position: relative;
            overflow: hidden;
        }

        /* Showcase person styles */
        .sc-person {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        /* ============================================
           TESTIMONIALS
        ============================================ */
        #testimonials {
            background: linear-gradient(180deg, transparent, rgba(201,168,76,0.02));
        }

        .testimonials-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 24px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .testimonial-card {
            background: rgba(10,10,10,0.8);
            border: 1px solid var(--border-gold);
            border-radius: 24px;
            padding: 36px;
            position: relative;
            overflow: hidden;
            backdrop-filter: blur(10px);
            transition: all 0.4s ease;
        }

        .testimonial-card:hover {
            transform: translateY(-6px);
            border-color: var(--border-gold-bright);
            box-shadow: 0 20px 60px rgba(201,168,76,0.1);
        }

        .testimonial-card::before {
            content: '"';
            position: absolute;
            top: -10px;
            left: 24px;
            font-size: 120px;
            color: rgba(201,168,76,0.06);
            font-family: var(--font-display);
            line-height: 1;
        }

        .testimonial-stars {
            display: flex;
            gap: 4px;
            margin-bottom: 20px;
        }

        .star {
            color: var(--gold);
            font-size: 16px;
        }

        .testimonial-text {
            font-size: 15px;
            color: rgba(255,255,255,0.7);
            line-height: 1.7;
            margin-bottom: 24px;
            font-style: italic;
        }

        .testimonial-author {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .author-avatar {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--gold), var(--gold-dark));
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 16px;
            color: #000;
            font-family: var(--font-tech);
            border: 2px solid rgba(201,168,76,0.3);
        }

        .author-name {
            font-weight: 700;
            font-size: 14px;
            font-family: var(--font-tech);
        }

        .author-role {
            font-size: 12px;
            color: rgba(255,255,255,0.4);
        }

        /* ============================================
           TECH SECTION
        ============================================ */
        #tech {
            background: rgba(0,0,0,0.5);
        }

        .tech-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 16px;
            max-width: 1000px;
            margin: 0 auto 60px;
        }

        .tech-badge {
            background: rgba(10,10,10,0.9);
            border: 1px solid var(--border-gold);
            border-radius: 16px;
            padding: 24px;
            text-align: center;
            transition: all 0.3s ease;
        }

        .tech-badge:hover {
            border-color: var(--border-gold-bright);
            transform: translateY(-4px);
            box-shadow: 0 10px 40px rgba(201,168,76,0.1);
        }

        .tech-badge-name {
            font-family: var(--font-tech);
            font-size: 14px;
            font-weight: 700;
            color: var(--gold-light);
            margin-bottom: 6px;
        }

        .tech-badge-desc {
            font-size: 11px;
            color: rgba(255,255,255,0.3);
        }

        /* ============================================
           PRICING
        ============================================ */
        #pricing {
            background: linear-gradient(180deg, transparent, rgba(201,168,76,0.02) 50%, transparent);
        }

        .pricing-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 24px;
            max-width: 1100px;
            margin: 0 auto;
        }

        .pricing-card {
            background: rgba(10,10,10,0.9);
            border: 1px solid var(--border-gold);
            border-radius: 28px;
            padding: 48px 40px;
            position: relative;
            overflow: hidden;
            transition: all 0.4s ease;
        }

        .pricing-card.featured {
            background: linear-gradient(135deg, rgba(201,168,76,0.1), rgba(10,10,10,0.95));
            border-color: var(--gold);
            box-shadow: 0 0 60px rgba(201,168,76,0.15), inset 0 1px 0 rgba(201,168,76,0.2);
            transform: scale(1.05);
        }

        .pricing-card:hover {
            transform: translateY(-8px);
            border-color: var(--border-gold-bright);
        }

        .pricing-card.featured:hover {
            transform: scale(1.05) translateY(-8px);
        }

        .pricing-popular {
            position: absolute;
            top: -1px;
            left: 50%;
            transform: translateX(-50%);
            background: linear-gradient(135deg, var(--gold-light), var(--gold));
            color: #000;
            padding: 6px 24px;
            border-radius: 0 0 14px 14px;
            font-size: 11px;
            font-weight: 800;
            letter-spacing: 2px;
            text-transform: uppercase;
            font-family: var(--font-tech);
        }

        .pricing-plan {
            font-size: 12px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 3px;
            color: var(--gold);
            font-family: var(--font-tech);
            margin-bottom: 16px;
        }

        .pricing-price {
            font-size: 56px;
            font-weight: 900;
            color: var(--white);
            font-family: var(--font-tech);
            line-height: 1;
            margin-bottom: 8px;
        }

        .pricing-price span {
            font-size: 20px;
            color: rgba(255,255,255,0.5);
        }

        .pricing-period {
            font-size: 13px;
            color: rgba(255,255,255,0.4);
            margin-bottom: 36px;
        }

        .pricing-features {
            list-style: none;
            margin-bottom: 40px;
        }

        .pricing-feature {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 10px 0;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            font-size: 14px;
            color: rgba(255,255,255,0.7);
        }

        .pricing-feature:last-child { border-bottom: none; }

        .check-icon {
            width: 20px;
            height: 20px;
            background: rgba(201,168,76,0.15);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
            color: var(--gold);
            font-size: 11px;
        }

        /* ============================================
           CTA SECTION
        ============================================ */
        #cta {
            padding: 120px 60px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        #cta::before {
            content: '';
            position: absolute;
            inset: 0;
            background: radial-gradient(ellipse 80% 60% at 50% 50%, rgba(201,168,76,0.1) 0%, transparent 70%);
        }

        .cta-title {
            font-family: var(--font-display);
            font-size: clamp(48px, 6vw, 80px);
            font-weight: 800;
            line-height: 1.1;
            max-width: 900px;
            margin: 0 auto 28px;
            position: relative;
            z-index: 2;
        }

        .cta-sub {
            font-size: 18px;
            color: rgba(255,255,255,0.5);
            max-width: 500px;
            margin: 0 auto 48px;
            position: relative;
            z-index: 2;
        }

        .cta-buttons {
            display: flex;
            gap: 20px;
            justify-content: center;
            position: relative;
            z-index: 2;
            flex-wrap: wrap;
        }

        /* ============================================
           FOOTER
        ============================================ */
        footer {
            background: rgba(0,0,0,0.95);
            border-top: 1px solid var(--border-gold);
            padding: 80px 60px 40px;
            position: relative;
            z-index: 2;
        }

        .footer-grid {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1fr;
            gap: 60px;
            margin-bottom: 60px;
        }

        .footer-brand p {
            font-size: 14px;
            color: rgba(255,255,255,0.4);
            line-height: 1.7;
            margin-top: 16px;
            max-width: 280px;
        }

        .footer-heading {
            font-family: var(--font-tech);
            font-size: 12px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: var(--gold);
            margin-bottom: 20px;
        }

        .footer-links {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .footer-links a {
            color: rgba(255,255,255,0.4);
            text-decoration: none;
            font-size: 14px;
            transition: color 0.3s ease;
        }

        .footer-links a:hover { color: var(--gold-light); }

        .footer-bottom {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-top: 32px;
            border-top: 1px solid rgba(255,255,255,0.06);
        }

        .footer-copy {
            font-size: 13px;
            color: rgba(255,255,255,0.25);
        }

        .footer-socials {
            display: flex;
            gap: 16px;
        }

        .social-link {
            width: 36px;
            height: 36px;
            background: rgba(255,255,255,0.04);
            border: 1px solid rgba(255,255,255,0.08);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: rgba(255,255,255,0.4);
            text-decoration: none;
            font-size: 14px;
            transition: all 0.3s ease;
        }

        .social-link:hover {
            background: rgba(201,168,76,0.1);
            border-color: var(--border-gold);
            color: var(--gold);
        }

        /* ============================================
           ANIMATED LINES / DECORATIONS
        ============================================ */
        .animated-line {
            height: 1px;
            background: linear-gradient(90deg, transparent, var(--gold), transparent);
            animation: line-glow 3s ease infinite;
        }

        @keyframes line-glow {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        .gold-orb {
            position: absolute;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(201,168,76,0.15), transparent 70%);
            pointer-events: none;
        }

        /* ============================================
           REVEAL ANIMATIONS
        ============================================ */
        .reveal {
            opacity: 0;
            transform: translateY(40px);
        }

        .reveal-left {
            opacity: 0;
            transform: translateX(-40px);
        }

        .reveal-right {
            opacity: 0;
            transform: translateX(40px);
        }

        /* ============================================
           COUNTER SECTION
        ============================================ */
        .live-counter-bar {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            background: rgba(201,168,76,0.06);
            border: 1px solid var(--border-gold);
            border-radius: 100px;
            padding: 10px 24px;
            max-width: fit-content;
            margin: 0 auto;
            font-family: var(--font-tech);
            font-size: 13px;
            color: rgba(255,255,255,0.7);
        }

        .live-counter-number {
            color: var(--gold-light);
            font-weight: 700;
        }

        /* ============================================
           RESPONSIVE
        ============================================ */
        @media (max-width: 1024px) {
            nav { padding: 20px 30px; }
            .nav-links { display: none; }
            .section-padding { padding: 80px 30px; }
            .features-grid { grid-template-columns: repeat(2, 1fr); }
            .generator-wrapper { grid-template-columns: 1fr; }
            .generator-panel { position: static; }
            .testimonials-grid { grid-template-columns: 1fr; }
            .tech-grid { grid-template-columns: repeat(2, 1fr); }
            .pricing-grid { grid-template-columns: 1fr; }
            .pricing-card.featured { transform: none; }
            .footer-grid { grid-template-columns: 1fr 1fr; }
            .preview-grid { grid-template-columns: repeat(3, 1fr); }
        }

        @media (max-width: 768px) {
            #hero { padding: 100px 24px 60px; }
            .section-padding { padding: 60px 24px; }
            .hero-stats { gap: 32px; }
            .features-grid { grid-template-columns: 1fr; }
            .generator-output { grid-template-columns: 1fr; }
            .footer-grid { grid-template-columns: 1fr; gap: 40px; }
            .footer-bottom { flex-direction: column; gap: 20px; text-align: center; }
            .preview-grid { grid-template-columns: repeat(2, 1fr); }
            .float-card { display: none; }
            .trusted { padding: 30px 24px; }
            #cta { padding: 80px 24px; }
            footer { padding: 60px 24px 30px; }
        }

        /* ============================================
           LOADING SCREEN
        ============================================ */
        #loading-screen {
            position: fixed;
            inset: 0;
            background: #000;
            z-index: 99999;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 24px;
            transition: opacity 0.8s ease, visibility 0.8s ease;
        }

        #loading-screen.hidden {
            opacity: 0;
            visibility: hidden;
        }

        .loading-logo {
            font-family: var(--font-tech);
            font-size: 32px;
            font-weight: 900;
            background: linear-gradient(135deg, var(--gold-bright), var(--gold), var(--gold-light));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: 6px;
        }

        .loading-bar-container {
            width: 200px;
            height: 2px;
            background: rgba(255,255,255,0.1);
            border-radius: 2px;
            overflow: hidden;
        }

        .loading-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--gold-dark), var(--gold-bright));
            width: 0%;
            transition: width 0.1s ease;
        }

        .loading-text {
            font-size: 11px;
            color: rgba(255,255,255,0.3);
            font-family: var(--font-tech);
            letter-spacing: 3px;
            text-transform: uppercase;
        }

        /* ============================================
           SPECIAL AI PREVIEW CARDS
        ============================================ */
        .ai-preview-face {
            width: 100%;
            height: 100%;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* Stylized gradient portraits */
        .portrait-1 {
            background: linear-gradient(135deg, #1a0f0f 0%, #2d1a0a 30%, #1a0f0f 60%, #0d0808 100%);
        }
        .portrait-1::before {
            content: '';
            position: absolute;
            inset: 0;
            background: 
                radial-gradient(ellipse 50% 60% at 50% 25%, rgba(255,200,160,0.3) 0%, transparent 50%),
                radial-gradient(ellipse 30% 20% at 50% 45%, rgba(200,160,120,0.2) 0%, transparent 40%),
                linear-gradient(180deg, transparent 50%, rgba(0,0,0,0.8) 100%);
        }

        .portrait-2 {
            background: linear-gradient(135deg, #0a0f1a 0%, #0a1520 30%, #081018 60%, #050a0d 100%);
        }
        .portrait-2::before {
            content: '';
            position: absolute;
            inset: 0;
            background: 
                radial-gradient(ellipse 50% 60% at 50% 25%, rgba(180,200,255,0.2) 0%, transparent 50%),
                radial-gradient(ellipse 30% 20% at 50% 45%, rgba(160,180,220,0.15) 0%, transparent 40%),
                linear-gradient(180deg, transparent 50%, rgba(0,0,0,0.8) 100%);
        }

        .portrait-3 {
            background: linear-gradient(135deg, #0f1a0a 0%, #1a2010 30%, #0f1a0a 60%, #080d05 100%);
        }
        .portrait-3::before {
            content: '';
            position: absolute;
            inset: 0;
            background: 
                radial-gradient(ellipse 50% 60% at 50% 25%, rgba(200,230,180,0.2) 0%, transparent 50%),
                linear-gradient(180deg, transparent 50%, rgba(0,0,0,0.8) 100%);
        }

        .portrait-4 {
            background: linear-gradient(135deg, #1a100a 0%, #201408 30%, #180f08 60%, #0d0805 100%);
        }
        .portrait-4::before {
            content: '';
            position: absolute;
            inset: 0;
            background: 
                radial-gradient(ellipse 50% 60% at 50% 25%, rgba(230,180,120,0.25) 0%, transparent 50%),
                linear-gradient(180deg, transparent 50%, rgba(0,0,0,0.8) 100%);
        }

        .portrait-5 {
            background: linear-gradient(135deg, #14081a 0%, #1c0d24 30%, #140818 60%, #0a0510 100%);
        }
        .portrait-5::before {
            content: '';
            position: absolute;
            inset: 0;
            background: 
                radial-gradient(ellipse 50% 60% at 50% 25%, rgba(200,160,255,0.2) 0%, transparent 50%),
                linear-gradient(180deg, transparent 50%, rgba(0,0,0,0.8) 100%);
        }

        /* Decorative human shape SVG overlay */
        .human-shape {
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 70%;
            height: 85%;
            opacity: 0.6;
        }

        /* Camera lens overlay effect */
        .lens-flare {
            position: absolute;
            top: 20%;
            right: 20%;
            width: 30px;
            height: 30px;
            background: radial-gradient(circle, rgba(255,255,255,0.4) 0%, rgba(201,168,76,0.2) 30%, transparent 70%);
            border-radius: 50%;
            mix-blend-mode: screen;
            animation: flare-pulse 4s ease infinite;
        }

        @keyframes flare-pulse {
            0%, 100% { opacity: 0.6; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.3); }
        }

        /* ============================================
           WORKFLOW SECTION
        ============================================ */
        #workflow {
            background: linear-gradient(180deg, rgba(201,168,76,0.02), transparent);
        }

        .workflow-steps {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 0;
            max-width: 1100px;
            margin: 0 auto;
            position: relative;
        }

        .workflow-steps::before {
            content: '';
            position: absolute;
            top: 32px;
            left: 12.5%;
            right: 12.5%;
            height: 1px;
            background: linear-gradient(90deg, transparent, var(--gold), transparent);
        }

        .workflow-step {
            text-align: center;
            padding: 0 24px;
        }

        .step-number {
            width: 64px;
            height: 64px;
            background: rgba(10,10,10,0.9);
            border: 1px solid var(--border-gold);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: var(--font-tech);
            font-size: 22px;
            font-weight: 900;
            color: var(--gold);
            margin: 0 auto 24px;
            position: relative;
            z-index: 2;
            transition: all 0.3s ease;
        }

        .workflow-step:hover .step-number {
            background: linear-gradient(135deg, var(--gold), var(--gold-dark));
            color: #000;
            box-shadow: 0 0 30px rgba(201,168,76,0.5);
        }

        .step-title {
            font-family: var(--font-tech);
            font-size: 16px;
            font-weight: 700;
            color: var(--white);
            margin-bottom: 10px;
        }

        .step-desc {
            font-size: 13px;
            color: rgba(255,255,255,0.4);
            line-height: 1.6;
        }

        @media (max-width: 768px) {
            .workflow-steps { grid-template-columns: repeat(2, 1fr); gap: 40px; }
            .workflow-steps::before { display: none; }
            .tech-grid { grid-template-columns: repeat(2, 1fr); }
        }

        /* ============================================
           NOTIFICATION TOASTS
        ============================================ */
        .toast-container {
            position: fixed;
            bottom: 24px;
            right: 24px;
            z-index: 9999;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .toast {
            background: rgba(10,10,10,0.95);
            border: 1px solid var(--border-gold);
            border-radius: 16px;
            padding: 16px 20px;
            display: flex;
            align-items: center;
            gap: 12px;
            backdrop-filter: blur(20px);
            min-width: 280px;
            animation: toast-in 0.4s ease;
            box-shadow: 0 8px 40px rgba(0,0,0,0.5);
        }

        @keyframes toast-in {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .toast-icon {
            font-size: 20px;
        }

        .toast-text {
            font-size: 13px;
            color: rgba(255,255,255,0.8);
            font-family: var(--font-tech);
        }

        .toast-name {
            font-weight: 700;
            color: var(--gold-light);
        }

        /* Separator */
        .section-separator {
            width: 60px;
            height: 2px;
            background: linear-gradient(90deg, var(--gold), transparent);
            margin: 16px auto 0;
        }
    </style>
</head>
<body>

    <!-- LOADING SCREEN -->
    <div id="loading-screen">
        <div class="loading-logo">NEXORA AI</div>
        <div class="loading-bar-container">
            <div class="loading-bar" id="loading-bar"></div>
        </div>
        <div class="loading-text">Initializing AI Systems...</div>
    </div>

    <!-- CUSTOM CURSOR -->
    <div class="cursor" id="cursor"></div>
    <div class="cursor-follower" id="cursor-follower"></div>

    <!-- PARTICLE CANVAS -->
    <canvas id="particle-canvas"></canvas>

    <!-- AMBIENT MOUSE LIGHT -->
    <div id="mouse-light"></div>

    <!-- TOAST CONTAINER -->
    <div class="toast-container" id="toast-container"></div>

    <!-- ============================================
         NAVIGATION
    ============================================ -->
    <nav id="navbar">
        <a href="#" class="nav-logo">
            <div class="nav-logo-icon">N</div>
            <span class="nav-logo-text">NEXORA AI</span>
        </a>
        <ul class="nav-links">
            <li><a href="#features">Features</a></li>
            <li><a href="#generator">Generator</a></li>
            <li><a href="#showcase">Showcase</a></li>
            <li><a href="#pricing">Pricing</a></li>
            <li><a href="#cta" class="nav-cta">Start Free</a></li>
        </ul>
    </nav>

    <!-- ============================================
         HERO SECTION
    ============================================ -->
    <section id="hero">
        <div class="hero-bg-gradient"></div>
        <div class="hero-grid-lines"></div>

        <!-- Floating Cards -->
        <div class="floating-cards">
            <div class="float-card float-card-1">
                <div class="float-card-label">Live Generation</div>
                <div class="float-card-value" id="live-counter">12,847</div>
                <div class="float-card-sub">Influencers created today</div>
                <br>
                <div class="live-dot">Active Now</div>
            </div>
            <div class="float-card float-card-2">
                <div class="float-card-label">Quality</div>
                <div class="float-card-value" style="color: var(--gold)">8K Ultra</div>
                <div class="float-card-sub">Face Consistent • Full Body</div>
            </div>
            <div class="float-card float-card-3">
                <div class="float-card-label">AI Engine</div>
                <div class="float-card-value" style="font-size: 15px; color: var(--gold-light)">FLUX + FaceID</div>
                <div class="float-card-sub">Next-Gen Rendering</div>
            </div>
        </div>

        <!-- Hero Content -->
        <div class="hero-badge">
            <div class="hero-badge-dot"></div>
            Next-Generation AI Influencer Platform
        </div>

        <h1 class="hero-headline">
            <span class="line-1">Create Viral AI Influencers</span>
            <span class="line-2">That Look 100% Human</span>
        </h1>

        <p class="hero-subheadline">
            Generate ultra realistic influencers with perfect face consistency, cinematic quality, and full-body realism. Never look AI-generated again.
        </p>

        <div class="hero-buttons">
            <a href="#generator" class="btn-primary">
                ✦ Start Creating
            </a>
            <a href="#showcase" class="btn-secondary">
                <div class="play-icon">▶</div>
                Watch Demo
            </a>
        </div>

        <div class="live-counter-bar">
            <div class="hero-badge-dot"></div>
            <span><span class="live-counter-number" id="gen-counter">2,341</span> influencers generating right now</span>
        </div>

        <br><br>

        <!-- Hero Stats -->
        <div class="hero-stats">
            <div class="hero-stat">
                <div class="hero-stat-number" id="stat-1">250K+</div>
                <div class="hero-stat-label">Creators Trust Us</div>
            </div>
            <div class="hero-stat">
                <div class="hero-stat-number" id="stat-2">48M+</div>
                <div class="hero-stat-label">Images Generated</div>
            </div>
            <div class="hero-stat">
                <div class="hero-stat-number" id="stat-3">99.9%</div>
                <div class="hero-stat-label">Human-Like Rating</div>
            </div>
            <div class="hero-stat">
                <div class="hero-stat-number" id="stat-4">8K</div>
                <div class="hero-stat-label">Max Resolution</div>
            </div>
        </div>

        <!-- Preview Grid -->
        <div class="hero-preview">
            <div class="animated-line" style="margin-bottom: 20px;"></div>
            <div class="preview-grid" id="preview-grid">
                <!-- Cards generated by JS -->
            </div>
        </div>
    </section>

    <!-- ============================================
         TRUSTED BY
    ============================================ -->
    <section id="trusted">
        <div class="trusted-inner">
            <div class="trusted-label">Trusted by</div>
            <div style="display: flex; gap: 60px; overflow: hidden; flex: 1;">
                <div class="trusted-scroll" id="trusted-scroll">
                    <span class="trusted-brand">Adobe</span>
                    <span class="trusted-brand">Vogue Media</span>
                    <span class="trusted-brand">TikTok Studio</span>
                    <span class="trusted-brand">Nike Digital</span>
                    <span class="trusted-brand">Spotify</span>
                    <span class="trusted-brand">Forbes</span>
                    <span class="trusted-brand">Meta Creative</span>
                    <span class="trusted-brand">Gucci Labs</span>
                    <span class="trusted-brand">Google AI</span>
                    <span class="trusted-brand">Netflix</span>
                    <!-- Duplicated for seamless loop -->
                    <span class="trusted-brand">Adobe</span>
                    <span class="trusted-brand">Vogue Media</span>
                    <span class="trusted-brand">TikTok Studio</span>
                    <span class="trusted-brand">Nike Digital</span>
                    <span class="trusted-brand">Spotify</span>
                    <span class="trusted-brand">Forbes</span>
                    <span class="trusted-brand">Meta Creative</span>
                    <span class="trusted-brand">Gucci Labs</span>
                    <span class="trusted-brand">Google AI</span>
                    <span class="trusted-brand">Netflix</span>
                </div>
            </div>
        </div>
    </section>

    <!-- ============================================
         FEATURES SECTION
    ============================================ -->
    <section id="features" class="section-padding">
        <div class="features-header reveal">
            <div class="section-tag">⬡ Core Features</div>
            <h2 class="section-title">Everything You Need to <span class="gold">Dominate Social Media</span></h2>
            <p class="section-subtitle">Industry-leading AI technology that makes your influencers indistinguishable from real humans.</p>
        </div>

        <div class="features-grid">
            <div class="feature-card reveal">
                <div class="feature-number">01</div>
                <div class="feature-icon">🧬</div>
                <h3 class="feature-title">Face Consistency Engine</h3>
                <p class="feature-desc">Our proprietary Character DNA system locks your influencer's identity across every single image, angle, and pose. Perfect consistency, every time.</p>
                <div class="feature-tags">
                    <span class="feature-tag">FaceID</span>
                    <span class="feature-tag">Multi-Angle</span>
                    <span class="feature-tag">DNA Lock</span>
                </div>
            </div>

            <div class="feature-card reveal">
                <div class="feature-number">02</div>
                <div class="feature-icon">🏛️</div>
                <h3 class="feature-title">Full Body Generation</h3>
                <p class="feature-desc">Generate complete human anatomy with perfect proportions. From fashion poses to gym shots — every pixel is anatomically accurate.</p>
                <div class="feature-tags">
                    <span class="feature-tag">Full Body</span>
                    <span class="feature-tag">Anatomy AI</span>
                    <span class="feature-tag">Pose Control</span>
                </div>
            </div>

            <div class="feature-card featured reveal">
                <div class="feature-number">03</div>
                <div class="feature-icon">✨</div>
                <h3 class="feature-title">Ultra Realistic Skin Engine</h3>
                <p class="feature-desc">Microskin texture rendering with HDR realism. Skin pores, natural imperfections, realistic makeup — DSLR-quality results at 8K resolution.</p>
                <div class="feature-tags">
                    <span class="feature-tag">HDR Skin</span>
                    <span class="feature-tag">8K Output</span>
                    <span class="feature-tag">DSLR Quality</span>
                </div>
            </div>

            <div class="feature-card reveal">
                <div class="feature-number">04</div>
                <div class="feature-icon">🎬</div>
                <h3 class="feature-title">AI Video Generation</h3>
                <p class="feature-desc">Create talking AI influencers with lip sync, walking animations, and cinematic reels. Same face, same body — now in motion for TikTok & Reels.</p>
                <div class="feature-tags">
                    <span class="feature-tag">Lip Sync</span>
                    <span class="feature-tag">TikTok Ready</span>
                    <span class="feature-tag">Cinematic</span>
                </div>
            </div>

            <div class="feature-card reveal">
                <div class="feature-number">05</div>
                <div class="feature-icon">🧠</div>
                <h3 class="feature-title">AI Model Training</h3>
                <p class="feature-desc">Upload reference photos, train your custom influencer profile, and reuse the same character forever across unlimited campaigns.</p>
                <div class="feature-tags">
                    <span class="feature-tag">LoRA Training</span>
                    <span class="feature-tag">Custom AI</span>
                    <span class="feature-tag">Forever Profile</span>
                </div>
            </div>

            <div class="feature-card reveal">
                <div class="feature-number">06</div>
                <div class="feature-icon">⚡</div>
                <h3 class="feature-title">Smart Prompt Enhancer</h3>
                <p class="feature-desc">AI-powered prompt builder automatically enhances your input to generate maximum realism. Professional results with zero prompt engineering skills.</p>
                <div class="feature-tags">
                    <span class="feature-tag">Auto-Enhance</span>
                    <span class="feature-tag">FLUX AI</span>
                    <span class="feature-tag">SDXL</span>
                </div>
            </div>
        </div>
    </section>

    <!-- ============================================
         WORKFLOW
    ============================================ -->
    <section id="workflow" class="section-padding">
        <div class="features-header reveal text-center">
            <div class="section-tag mx-auto">⬡ How It Works</div>
            <h2 class="section-title">Generate in <span class="gold">4 Simple Steps</span></h2>
            <p class="section-subtitle mx-auto" style="margin: 0 auto;">From idea to viral influencer in minutes. No design skills required.</p>
        </div>

        <div style="height: 60px;"></div>

        <div class="workflow-steps">
            <div class="workflow-step reveal">
                <div class="step-number">1</div>
                <h4 class="step-title">Describe Your Influencer</h4>
                <p class="step-desc">Type a prompt or use our smart builder to define your perfect AI influencer.</p>
            </div>
            <div class="workflow-step reveal">
                <div class="step-number">2</div>
                <h4 class="step-title">Set Style Presets</h4>
                <p class="step-desc">Choose from fashion, fitness, lifestyle, or luxury — hundreds of professional presets.</p>
            </div>
            <div class="workflow-step reveal">
                <div class="step-number">3</div>
                <h4 class="step-title">Lock the DNA</h4>
                <p class="step-desc">Character lock ensures the same face and body in every single generation forever.</p>
            </div>
            <div class="workflow-step reveal">
                <div class="step-number">4</div>
                <h4 class="step-title">Export & Publish</h4>
                <p class="step-desc">Download in 4K or 8K and publish directly to your platforms. Instant viral content.</p>
            </div>
        </div>
    </section>

    <!-- ============================================
         GENERATOR PANEL
    ============================================ -->
    <section id="generator" class="section-padding">
        <div class="features-header reveal text-center" style="margin-bottom: 60px;">
            <div class="section-tag mx-auto">⬡ AI Generator</div>
            <h2 class="section-title">The Most <span class="gold">Powerful Generator</span> Ever Built</h2>
            <p class="section-subtitle mx-auto" style="margin: 0 auto;">Professional-grade controls designed for creators who demand perfection.</p>
        </div>

        <div class="generator-wrapper">
            <!-- Panel -->
            <div class="generator-panel reveal-left">
                <div class="generator-panel-title">
                    <span style="color: var(--gold);">◆</span> Generator Controls
                </div>

                <div class="panel-section">
                    <div class="panel-label">Prompt</div>
                    <textarea class="prompt-box" rows="4" placeholder="Beautiful female fashion influencer, 25 years old, European, wearing luxury Chanel outfit, studio lighting, 8K DSLR quality, ultra realistic..."></textarea>
                </div>

                <div class="panel-section">
                    <div class="panel-label">Negative Prompt</div>
                    <textarea class="prompt-box" rows="2" placeholder="Deformed, AI look, cartoon, blurry, low quality..."></textarea>
                </div>

                <div class="panel-section">
                    <div class="panel-label">Gender</div>
                    <div class="toggle-grid">
                        <div class="toggle-btn active" onclick="setActive(this, '.toggle-btn')">♀ Female</div>
                        <div class="toggle-btn" onclick="setActive(this, '.toggle-btn')">♂ Male</div>
                    </div>
                </div>

                <div class="panel-section">
                    <div class="panel-label">Style Preset</div>
                    <div class="toggle-grid">
                        <div class="toggle-btn active" onclick="setActive(this, '.p-style')">👗 Fashion</div>
                        <div class="toggle-btn p-style" onclick="setActive(this, '.p-style')">💪 Fitness</div>
                        <div class="toggle-btn p-style" onclick="setActive(this, '.p-style')">✈️ Lifestyle</div>
                        <div class="toggle-btn p-style" onclick="setActive(this, '.p-style')">💎 Luxury</div>
                    </div>
                </div>

                <div class="panel-section">
                    <div class="panel-label">Ethnicity</div>
                    <select class="prompt-box" style="height: 44px; cursor: pointer;">
                        <option>European / Caucasian</option>
                        <option>East Asian</option>
                        <option>South Asian / Indian</option>
                        <option>African American</option>
                        <option>Latin / Hispanic</option>
                        <option>Middle Eastern</option>
                        <option>Mixed / Multiracial</option>
                    </select>
                </div>

                <div class="panel-section">
                    <div class="slider-label-row">
                        <div class="panel-label" style="margin: 0;">Age</div>
                        <span class="slider-value" id="age-val">25</span>
                    </div>
                    <input type="range" min="18" max="60" value="25" id="age-slider">
                </div>

                <div class="panel-section">
                    <div class="slider-label-row">
                        <div class="panel-label" style="margin: 0;">Realism</div>
                        <span class="slider-value" id="real-val">98%</span>
                    </div>
                    <input type="range" min="60" max="100" value="98" id="real-slider">
                </div>

                <div class="panel-section">
                    <div class="panel-label">Output Quality</div>
                    <div class="quality-switch">
                        <div class="quality-option">HD</div>
                        <div class="quality-option active">4K</div>
                        <div class="quality-option">8K</div>
                        <div class="quality-option">Ultra</div>
                    </div>
                </div>

                <div class="panel-section">
                    <div class="panel-label">Face Lock System</div>
                    <div class="toggle-grid">
                        <div class="toggle-btn active">🔒 Face Lock ON</div>
                        <div class="toggle-btn">🔓 Free Mode</div>
                    </div>
                </div>

                <div class="panel-section">
                    <div class="panel-label">Batch Size</div>
                    <div class="toggle-grid">
                        <div class="toggle-btn active">1 Image</div>
                        <div class="toggle-btn">4 Images</div>
                        <div class="toggle-btn">8 Images</div>
                        <div class="toggle-btn">16 Images</div>
                    </div>
                </div>

                <button class="generate-btn" id="gen-btn" onclick="startGeneration()">
                    ✦ Generate AI Influencer
                </button>
            </div>

            <!-- Output Area -->
            <div class="reveal-right">
                <div class="generator-output" id="generator-output">
                    <div class="output-card">
                        <div class="output-generating" id="gen-overlay-1" style="display: none;">
                            <div class="gen-ring"></div>
                            <div class="gen-text">Generating...</div>
                            <div class="progress-bar"><div class="progress-fill" id="prog-1"></div></div>
                        </div>
                        <div class="output-placeholder">
                            <span style="font-size: 40px; opacity: 0.2;">◆</span>
                            <span style="font-size: 12px; color: rgba(255,255,255,0.2); font-family: var(--font-tech); letter-spacing: 2px;">READY TO GENERATE</span>
                        </div>
                    </div>
                    <div class="output-card">
                        <div class="output-generating" id="gen-overlay-2" style="display: none;">
                            <div class="gen-ring"></div>
                            <div class="gen-text">Processing...</div>
                            <div class="progress-bar"><div class="progress-fill" id="prog-2"></div></div>
                        </div>
                        <div class="output-placeholder">
                            <span style="font-size: 40px; opacity: 0.2;">◆</span>
                            <span style="font-size: 12px; color: rgba(255,255,255,0.2); font-family: var(--font-tech); letter-spacing: 2px;">AWAITING INPUT</span>
                        </div>
                    </div>
                    <div class="output-card">
                        <div class="output-generating" id="gen-overlay-3" style="display: none;">
                            <div class="gen-ring"></div>
                            <div class="gen-text">Enhancing...</div>
                            <div class="progress-bar"><div class="progress-fill" id="prog-3"></div></div>
                        </div>
                        <div class="output-placeholder">
                            <span style="font-size: 40px; opacity: 0.2;">◆</span>
                            <span style="font-size: 12px; color: rgba(255,255,255,0.2); font-family: var(--font-tech); letter-spacing: 2px;">8K READY</span>
                        </div>
                    </div>
                    <div class="output-card">
                        <div class="output-generating" id="gen-overlay-4" style="display: none;">
                            <div class="gen-ring"></div>
                            <div class="gen-text">Face Locking...</div>
                            <div class="progress-bar"><div class="progress-fill" id="prog-4"></div></div>
                        </div>
                        <div class="output-placeholder">
                            <span style="font-size: 40px; opacity: 0.2;">◆</span>
                            <span style="font-size: 12px; color: rgba(255,255,255,0.2); font-family: var(--font-tech); letter-spacing: 2px;">FACE LOCK READY</span>
                        </div>
                    </div>
                </div>

                <!-- Generator Info -->
                <div style="margin-top: 24px; display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px;">
                    <div class="feature-card" style="padding: 20px; text-align: center;">
                        <div style="font-size: 24px; margin-bottom: 8px;">🧬</div>
                        <div style="font-size: 11px; color: var(--gold); font-family: var(--font-tech); text-transform: uppercase; letter-spacing: 1px;">Face DNA</div>
                        <div style="font-size: 10px; color: rgba(255,255,255,0.3); margin-top: 4px;">Active</div>
                    </div>
                    <div class="feature-card" style="padding: 20px; text-align: center;">
                        <div style="font-size: 24px; margin-bottom: 8px;">🔒</div>
                        <div style="font-size: 11px; color: var(--gold); font-family: var(--font-tech); text-transform: uppercase; letter-spacing: 1px;">Identity Lock</div>
                        <div style="font-size: 10px; color: rgba(255,255,255,0.3); margin-top: 4px;">Enabled</div>
                    </div>
                    <div class="feature-card" style="padding: 20px; text-align: center;">
                        <div style="font-size: 24px; margin-bottom: 8px;">⚡</div>
                        <div style="font-size: 11px; color: var(--gold); font-family: var(--font-tech); text-transform: uppercase; letter-spacing: 1px;">FLUX Engine</div>
                        <div style="font-size: 10px; color: rgba(255,255,255,0.3); margin-top: 4px;">Online</div>
                    </div>
                    <div class="feature-card" style="padding: 20px; text-align: center;">
                        <div style="font-size: 24px; margin-bottom: 8px;">🛡️</div>
                        <div style="font-size: 11px; color: var(--gold); font-family: var(--font-tech); text-transform: uppercase; letter-spacing: 1px;">NSFW Guard</div>
                        <div style="font-size: 10px; color: rgba(255,255,255,0.3); margin-top: 4px;">Protected</div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- ============================================
         SHOWCASE GALLERY
    ============================================ -->
    <section id="showcase" style="padding: 120px 0; overflow: hidden;">
        <div class="showcase-header reveal" style="padding: 0 60px;">
            <div class="section-tag mx-auto">⬡ AI Showcase</div>
            <h2 class="section-title">Meet Your Future <span class="gold">AI Influencer Army</span></h2>
            <p class="section-subtitle mx-auto" style="margin: 0 auto;">Every single one of these was created by NEXORA AI. Can you tell the difference?</p>
        </div>

        <div style="height: 60px;"></div>

        <!-- Row 1 -->
        <div class="showcase-row" id="showcase-row-1">
            <div class="showcase-row-inner" id="showcase-inner-1">
                <!-- Generated by JS -->
            </div>
        </div>

        <!-- Row 2 -->
        <div class="showcase-row" id="showcase-row-2" style="margin-top: 16px;">
            <div class="showcase-row-inner" id="showcase-inner-2">
                <!-- Generated by JS -->
            </div>
        </div>
    </section>

    <!-- ============================================
         TESTIMONIALS
    ============================================ -->
    <section id="testimonials" class="section-padding">
        <div class="features-header reveal text-center" style="margin-bottom: 60px;">
            <div class="section-tag mx-auto">⬡ Social Proof</div>
            <h2 class="section-title">Loved by <span class="gold">250K+ Creators</span></h2>
            <p class="section-subtitle mx-auto" style="margin: 0 auto;">Creators, brands, and agencies worldwide trust NEXORA AI to power their content.</p>
        </div>

        <div class="testimonials-grid">
            <div class="testimonial-card reveal">
                <div class="testimonial-stars">★★★★★</div>
                <p class="testimonial-text">"I've been using NEXORA for 3 months and the face consistency is absolutely insane. My AI influencer has 200K followers and nobody suspects it's AI. The quality is Hollywood-level."</p>
                <div class="testimonial-author">
                    <div class="author-avatar">J</div>
                    <div>
                        <div class="author-name">Jessica M.</div>
                        <div class="author-role">Digital Creator • 2.4M Followers</div>
                    </div>
                </div>
            </div>

            <div class="testimonial-card reveal">
                <div class="testimonial-stars">★★★★★</div>
                <p class="testimonial-text">"We replaced our entire model casting budget with NEXORA. The ROI is 10x. Every campaign now runs faster, cheaper, and the AI models look better than any human we've ever worked with."</p>
                <div class="testimonial-author">
                    <div class="author-avatar" style="background: linear-gradient(135deg, #4a90d9, #2c5aa0);">R</div>
                    <div>
                        <div class="author-name">Ryan Okafor</div>
                        <div class="author-role">Creative Director • LUXE Agency</div>
                    </div>
                </div>
            </div>

            <div class="testimonial-card reveal">
                <div class="testimonial-stars">★★★★★</div>
                <p class="testimonial-text">"The skin texture is unbelievable. I zoomed into the 8K output and it had real skin pores. This isn't AI art anymore — this is digital human technology. Mind-blowing product."</p>
                <div class="testimonial-author">
                    <div class="author-avatar" style="background: linear-gradient(135deg, #d94a90, #a02c5a);">A</div>
                    <div>
                        <div class="author-name">Aria Tanaka</div>
                        <div class="author-role">AI Artist & Entrepreneur</div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- ============================================
         TECH SECTION
    ============================================ -->
    <section id="tech" class="section-padding">
        <div class="features-header reveal text-center" style="margin-bottom: 60px;">
            <div class="section-tag mx-auto">⬡ Powered By</div>
            <h2 class="section-title">Built on <span class="gold">Cutting-Edge AI</span></h2>
        </div>

        <div class="tech-grid">
            <div class="tech-badge reveal">
                <div class="tech-badge-name">FLUX.1</div>
                <div class="tech-badge-desc">Ultra Realistic Generation</div>
            </div>
            <div class="tech-badge reveal">
                <div class="tech-badge-name">SDXL</div>
                <div class="tech-badge-desc">High Resolution Engine</div>
            </div>
            <div class="tech-badge reveal">
                <div class="tech-badge-name">FaceID</div>
                <div class="tech-badge-desc">Face Consistency Lock</div>
            </div>
            <div class="tech-badge reveal">
                <div class="tech-badge-name">ControlNet</div>
                <div class="tech-badge-desc">Pose & Body Control</div>
            </div>
            <div class="tech-badge reveal">
                <div class="tech-badge-name">LoRA</div>
                <div class="tech-badge-desc">Custom Model Training</div>
            </div>
            <div class="tech-badge reveal">
                <div class="tech-badge-name">IPAdapter</div>
                <div class="tech-badge-desc">Identity Preservation</div>
            </div>
            <div class="tech-badge reveal">
                <div class="tech-badge-name">ESRGAN</div>
                <div class="tech-badge-desc">8K Upscaling Engine</div>
            </div>
            <div class="tech-badge reveal">
                <div class="tech-badge-name">AnimateDiff</div>
                <div class="tech-badge-desc">AI Video Generation</div>
            </div>
        </div>
    </section>

    <!-- ============================================
         PRICING
    ============================================ -->
    <section id="pricing" class="section-padding">
        <div class="features-header reveal text-center" style="margin-bottom: 60px;">
            <div class="section-tag mx-auto">⬡ Pricing</div>
            <h2 class="section-title">Transparent <span class="gold">Creator Pricing</span></h2>
            <p class="section-subtitle mx-auto" style="margin: 0 auto;">Start free. Scale as you grow. Cancel anytime.</p>
        </div>

        <div class="pricing-grid">
            <div class="pricing-card reveal">
                <div class="pricing-plan">Starter</div>
                <div class="pricing-price"><span>$</span>29</div>
                <div class="pricing-period">per month, billed annually</div>
                <ul class="pricing-features">
                    <li class="pricing-feature"><div class="check-icon">✓</div> 200 AI Image Generations</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> 4K Resolution Output</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Face Consistency Engine</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> 3 Character Profiles</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Basic Style Presets</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> NSFW Protection</li>
                </ul>
                <button class="btn-secondary" style="width: 100%; justify-content: center; border-radius: 12px;">Get Started</button>
            </div>

            <div class="pricing-card featured reveal">
                <div class="pricing-popular">Most Popular</div>
                <div class="pricing-plan">Pro Creator</div>
                <div class="pricing-price"><span>$</span>79</div>
                <div class="pricing-period">per month, billed annually</div>
                <ul class="pricing-features">
                    <li class="pricing-feature"><div class="check-icon">✓</div> 2,000 AI Generations/mo</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> 8K Ultra Resolution</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Advanced DNA Face Lock</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Unlimited Characters</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> AI Video Generation</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> All Style Presets</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> LoRA Training (5 models)</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Batch Generation (16x)</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Priority GPU Queue</li>
                </ul>
                <button class="btn-primary" style="width: 100%; justify-content: center; border-radius: 12px; text-align: center;">Start Pro →</button>
            </div>

            <div class="pricing-card reveal">
                <div class="pricing-plan">Enterprise</div>
                <div class="pricing-price"><span>$</span>299</div>
                <div class="pricing-period">per month, billed annually</div>
                <ul class="pricing-features">
                    <li class="pricing-feature"><div class="check-icon">✓</div> Unlimited Generations</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> 8K Maximum Quality</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Custom AI Model Training</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> White-Label Solution</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> API Access</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Dedicated GPU Server</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Custom LoRA Unlimited</li>
                    <li class="pricing-feature"><div class="check-icon">✓</div> Priority Support 24/7</div></li>
                </ul>
                <button class="btn-secondary" style="width: 100%; justify-content: center; border-radius: 12px;">Contact Sales</button>
            </div>
        </div>
    </section>

    <!-- ============================================
         CTA SECTION
    ============================================ -->
    <section id="cta">
        <div class="gold-orb" style="width: 800px; height: 800px; top: -200px; left: 50%; transform: translateX(-50%);"></div>

        <div class="section-tag mx-auto reveal" style="max-width: fit-content;">⬡ Start Today</div>
        <h2 class="cta-title reveal">
            The Future of <span class="gold">Influencer Marketing</span> Is Here
        </h2>
        <p class="cta-sub reveal">Join 250,000+ creators already building their AI influencer empires. Start free, no credit card required.</p>
        <div class="cta-buttons reveal">
            <a href="#generator" class="btn-primary" style="font-size: 16px; padding: 20px 48px;">✦ Create Your First Influencer</a>
            <a href="#features" class="btn-secondary" style="font-size: 16px; padding: 20px 48px;">Explore Features</a>
        </div>

        <div style="margin-top: 40px; display: flex; gap: 32px; justify-content: center; flex-wrap: wrap; position: relative; z-index: 2;">
            <span style="font-size: 13px; color: rgba(255,255,255,0.4);">✓ No Credit Card Required</span>
            <span style="font-size: 13px; color: rgba(255,255,255,0.4);">✓ Cancel Anytime</span>
            <span style="font-size: 13px; color: rgba(255,255,255,0.4);">✓ 10 Free Generations</span>
            <span style="font-size: 13px; color: rgba(255,255,255,0.4);">✓ Instant Access</span>
        </div>
    </section>

    <!-- ============================================
         FOOTER
    ============================================ -->
    <footer>
        <div class="footer-grid">
            <div class="footer-brand">
                <a href="#" class="nav-logo" style="margin-bottom: 0;">
                    <div class="nav-logo-icon">N</div>
                    <span class="nav-logo-text">NEXORA AI</span>
                </a>
                <p>The world's most advanced AI Influencer Generator. Create hyper-realistic AI humans that are indistinguishable from real people.</p>
            </div>
            <div>
                <div class="footer-heading">Product</div>
                <ul class="footer-links">
                    <li><a href="#">Features</a></li>
                    <li><a href="#">Generator</a></li>
                    <li><a href="#">Showcase</a></li>
                    <li><a href="#">Pricing</a></li>
                    <li><a href="#">API</a></li>
                </ul>
            </div>
            <div>
                <div class="footer-heading">Company</div>
                <ul class="footer-links">
                    <li><a href="#">About Us</a></li>
                    <li><a href="#">Blog</a></li>
                    <li><a href="#">Careers</a></li>
                    <li><a href="#">Press Kit</a></li>
                    <li><a href="#">Contact</a></li>
                </ul>
            </div>
            <div>
                <div class="footer-heading">Legal</div>
                <ul class="footer-links">
                    <li><a href="#">Privacy Policy</a></li>
                    <li><a href="#">Terms of Service</a></li>
                    <li><a href="#">Content Policy</a></li>
                    <li><a href="#">DMCA</a></li>
                    <li><a href="#">Ethics</a></li>
                </ul>
            </div>
        </div>

        <div class="footer-bottom">
            <div class="footer-copy">© 2025 NEXORA AI. All rights reserved. Powered by Next-Gen AI Technology.</div>
            <div class="footer-socials">
                <a href="#" class="social-link">𝕏</a>
                <a href="#" class="social-link">in</a>
                <a href="#" class="social-link">ig</a>
                <a href="#" class="social-link">yt</a>
            </div>
        </div>
    </footer>

    <script>
    // ============================================
    // LOADING SCREEN
    // ============================================
    let loadProgress = 0;
    const loadBar = document.getElementById('loading-bar');
    const loadScreen = document.getElementById('loading-screen');

    const loadInterval = setInterval(() => {
        loadProgress += Math.random() * 18;
        if (loadProgress >= 100) {
            loadProgress = 100;
            clearInterval(loadInterval);
            setTimeout(() => {
                loadScreen.classList.add('hidden');
                initAnimations();
            }, 400);
        }
        loadBar.style.width = loadProgress + '%';
    }, 80);

    // ============================================
    // CUSTOM CURSOR
    // ============================================
    const cursor = document.getElementById('cursor');
    const cursorFollower = document.getElementById('cursor-follower');
    const mouseLight = document.getElementById('mouse-light');

    let mouseX = 0, mouseY = 0;
    let followerX = 0, followerY = 0;

    document.addEventListener('mousemove', (e) => {
        mouseX = e.clientX;
        mouseY = e.clientY;
        cursor.style.left = mouseX - 6 + 'px';
        cursor.style.top = mouseY - 6 + 'px';
        mouseLight.style.left = mouseX + 'px';
        mouseLight.style.top = mouseY + 'px';
    });

    function animateCursor() {
        followerX += (mouseX - followerX - 20) * 0.12;
        followerY += (mouseY - followerY - 20) * 0.12;
        cursorFollower.style.left = followerX + 'px';
        cursorFollower.style.top = followerY + 'px';
        requestAnimationFrame(animateCursor);
    }
    animateCursor();

    document.querySelectorAll('a, button, .feature-card, .pricing-card, .toggle-btn').forEach(el => {
        el.addEventListener('mouseenter', () => cursorFollower.classList.add('hover'));
        el.addEventListener('mouseleave', () => cursorFollower.classList.remove('hover'));
    });

    // ============================================
    // PARTICLE SYSTEM
    // ============================================
    const canvas = document.getElementById('particle-canvas');
    const ctx = canvas.getContext('2d');

    let particles = [];
    let W = window.innerWidth;
    let H = window.innerHeight;

    canvas.width = W;
    canvas.height = H;

    window.addEventListener('resize', () => {
        W = canvas.width = window.innerWidth;
        H = canvas.height = window.innerHeight;
    });

    class Particle {
        constructor() { this.reset(); }
        reset() {
            this.x = Math.random() * W;
            this.y = Math.random() * H;
            this.size = Math.random() * 1.5 + 0.3;
            this.speedX = (Math.random() - 0.5) * 0.3;
            this.speedY = -Math.random() * 0.4 - 0.1;
            this.opacity = Math.random() * 0.5 + 0.1;
            this.life = 0;
            this.maxLife = Math.random() * 200 + 100;
            this.color = Math.random() > 0.7 ? 
                `rgba(201, 168, 76, ${this.opacity})` : 
                `rgba(255, 255, 255, ${this.opacity * 0.3})`;
        }
        update() {
            this.x += this.speedX;
            this.y += this.speedY;
            this.life++;
            if (this.life > this.maxLife || this.y < 0) this.reset();
        }
        draw() {
            ctx.save();
            ctx.globalAlpha = this.opacity * (1 - this.life / this.maxLife);
            ctx.fillStyle = this.color;
            ctx.beginPath();
            ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
            ctx.fill();
            ctx.restore();
        }
    }

    for (let i = 0; i < 120; i++) {
        const p = new Particle();
        p.life = Math.random() * p.maxLife;
        particles.push(p);
    }

    function animateParticles() {
        ctx.clearRect(0, 0, W, H);
        particles.forEach(p => { p.update(); p.draw(); });
        requestAnimationFrame(animateParticles);
    }
    animateParticles();

    // ============================================
    // NAVBAR SCROLL
    // ============================================
    const navbar = document.getElementById('navbar');
    window.addEventListener('scroll', () => {
        if (window.scrollY > 60) navbar.classList.add('scrolled');
        else navbar.classList.remove('scrolled');
    });

    // ============================================
    // GSAP ANIMATIONS
    // ============================================
    function initAnimations() {
        gsap.registerPlugin(ScrollTrigger);

        // Hero animations
        gsap.from('.hero-badge', { duration: 1, y: 30, opacity: 0, ease: 'power3.out', delay: 0.2 });
        gsap.from('.hero-headline', { duration: 1.2, y: 50, opacity: 0, ease: 'power3.out', delay: 0.4 });
        gsap.from('.hero-subheadline', { duration: 1, y: 30, opacity: 0, ease: 'power3.out', delay: 0.6 });
        gsap.from('.hero-buttons', { duration: 1, y: 30, opacity: 0, ease: 'power3.out', delay: 0.8 });
        gsap.from('.live-counter-bar', { duration: 1, y: 20, opacity: 0, ease: 'power3.out', delay: 1 });
        gsap.from('.hero-stats', { duration: 1, y: 30, opacity: 0, ease: 'power3.out', delay: 1.1 });
        gsap.from('.float-card', { duration: 1.2, scale: 0.8, opacity: 0, stagger: 0.2, ease: 'back.out(1.7)', delay: 1.2 });
        gsap.from('.preview-card', { duration: 0.8, y: 40, opacity: 0, stagger: 0.1, ease: 'power3.out', delay: 1.4 });

        // Scroll reveals
        gsap.utils.toArray('.reveal').forEach(el => {
            gsap.fromTo(el, 
                { y: 50, opacity: 0 },
                { y: 0, opacity: 1, duration: 0.9, ease: 'power3.out',
                  scrollTrigger: { trigger: el, start: 'top 85%', toggleActions: 'play none none none' }
                }
            );
        });

        gsap.utils.toArray('.reveal-left').forEach(el => {
            gsap.fromTo(el,
                { x: -60, opacity: 0 },
                { x: 0, opacity: 1, duration: 1, ease: 'power3.out',
                  scrollTrigger: { trigger: el, start: 'top 80%', toggleActions: 'play none none none' }
                }
            );
        });

        gsap.utils.toArray('.reveal-right').forEach(el => {
            gsap.fromTo(el,
                { x: 60, opacity: 0 },
                { x: 0, opacity: 1, duration: 1, ease: 'power3.out',
                  scrollTrigger: { trigger: el, start: 'top 80%', toggleActions: 'play none none none' }
                }
            );
        });

        // Feature cards stagger
        gsap.utils.toArray('.feature-card').forEach((card, i) => {
            gsap.fromTo(card,
                { y: 60, opacity: 0 },
                { y: 0, opacity: 1, duration: 0.8, ease: 'power3.out', delay: i * 0.1,
                  scrollTrigger: { trigger: card, start: 'top 88%' }
                }
            );

            // 3D tilt effect
            card.addEventListener('mousemove', (e) => {
                const rect = card.getBoundingClientRect();
                const x = e.clientX - rect.left - rect.width / 2;
                const y = e.clientY - rect.top - rect.height / 2;
                const rotX = (-y / rect.height) * 12;
                const rotY = (x / rect.width) * 12;
                gsap.to(card, { rotationX: rotX, rotationY: rotY, duration: 0.3, transformPerspective: 1000, ease: 'power2.out' });
            });
            card.addEventListener('mouseleave', () => {
                gsap.to(card, { rotationX: 0, rotationY: 0, duration: 0.5, ease: 'power2.out' });
            });
        });

        // Pricing cards
        gsap.utils.toArray('.pricing-card').forEach((card, i) => {
            gsap.fromTo(card,
                { y: 60, opacity: 0 },
                { y: 0, opacity: 1, duration: 0.9, ease: 'power3.out', delay: i * 0.15,
                  scrollTrigger: { trigger: card, start: 'top 88%' }
                }
            );
        });

        // Testimonial cards
        gsap.utils.toArray('.testimonial-card').forEach((card, i) => {
            gsap.fromTo(card,
                { y: 50, opacity: 0 },
                { y: 0, opacity: 1, duration: 0.9, ease: 'power3.out', delay: i * 0.15,
                  scrollTrigger: { trigger: card, start: 'top 88%' }
                }
            );
        });
    }

    // ============================================
    // BUILD HERO PREVIEW CARDS
    // ============================================
    const previewData = [
        { name: 'Sofia L.', tag: 'Fashion Model', quality: '8K', bg: 'portrait-1', skin: '#e8c5a0' },
        { name: 'Marcus D.', tag: 'Fitness Influencer', quality: '4K', bg: 'portrait-2', skin: '#c4a882' },
        { name: 'Zara K.', tag: 'Luxury Lifestyle', quality: '8K', bg: 'portrait-3', skin: '#d4a574' },
        { name: 'Alex T.', tag: 'Urban Style', quality: '4K', bg: 'portrait-4', skin: '#b8956a' },
        { name: 'Mei W.', tag: 'Beauty Creator', quality: '8K', bg: 'portrait-5', skin: '#f0d5b0' },
    ];

    const previewGrid = document.getElementById('preview-grid');
    previewData.forEach((p, i) => {
        previewGrid.innerHTML += `
            <div class="preview-card" style="animation-delay: ${i * 0.1}s">
                <div class="preview-card-inner">
                    <div class="preview-card-bg">
                        <div class="ai-preview-face ${p.bg}">
                            <div class="lens-flare" style="animation-delay: ${i * 0.8}s"></div>
                            <svg class="human-shape" viewBox="0 0 200 400" fill="none" xmlns="http://www.w3.org/2000/svg" style="opacity: 0.4;">
                                <ellipse cx="100" cy="60" rx="40" ry="50" fill="${p.skin}" opacity="0.5"/>
                                <path d="M60 110 Q100 130 140 110 L150 280 Q100 300 50 280 Z" fill="${p.skin}" opacity="0.35"/>
                                <path d="M50 280 L30 380" stroke="${p.skin}" stroke-width="20" stroke-linecap="round" opacity="0.3"/>
                                <path d="M150 280 L170 380" stroke="${p.skin}" stroke-width="20" stroke-linecap="round" opacity="0.3"/>
                                <path d="M60 115 L20 200" stroke="${p.skin}" stroke-width="18" stroke-linecap="round" opacity="0.3"/>
                                <path d="M140 115 L180 200" stroke="${p.skin}" stroke-width="18" stroke-linecap="round" opacity="0.3"/>
                            </svg>
                        </div>
                    </div>
                    <div class="preview-card-overlay"></div>
                    <div class="preview-badge">${p.quality}</div>
                    <div class="preview-card-info">
                        <div class="preview-card-name">${p.name}</div>
                        <div class="preview-card-tag">${p.tag}</div>
                    </div>
                </div>
            </div>
        `;
    });

    // ============================================
    // BUILD SHOWCASE ROWS
    // ============================================
    const showcaseItems = [
        { name: 'Aria', type: 'Asian Fashion', bg: '#0a0f1a', color: '#b8d4f0' },
        { name: 'Lena', type: 'European Model', bg: '#1a0f0f', color: '#f0c8a0' },
        { name: 'Nadia', type: 'Fitness Queen', bg: '#0f1a0a', color: '#c0e0a0' },
        { name: 'Blake', type: 'Urban Style', bg: '#100a14', color: '#d0b0f0' },
        { name: 'Priya', type: 'Indian Glam', bg: '#1a100a', color: '#f0c870' },
        { name: 'Zoe', type: 'Cyberpunk AI', bg: '#0a1020', color: '#70c0f0' },
        { name: 'Marcus', type: 'Gym Influencer', bg: '#100808', color: '#e0906070' },
        { name: 'Sakura', type: 'K-Style Beauty', bg: '#14081a', color: '#f0b0e0' },
        { name: 'Leo', type: 'Luxury Travel', bg: '#0a0a14', color: '#a0b0f0' },
        { name: 'Amara', type: 'Vogue Editorial', bg: '#0a0f0a', color: '#90d090' },
    ];

    function buildShowcaseItem(item, i) {
        return `
            <div class="showcase-item">
                <div class="showcase-item-bg" style="background: ${item.bg};">
                    <div style="position: absolute; inset: 0; background: radial-gradient(ellipse 60% 60% at 50% 30%, ${item.color}33 0%, transparent 60%), linear-gradient(180deg, transparent 40%, rgba(0,0,0,0.8) 100%);"></div>
                    <svg style="position:absolute; bottom:0; left:50%; transform:translateX(-50%); width:70%; height:85%; opacity:0.35;" viewBox="0 0 200 400" fill="none">
                        <ellipse cx="100" cy="60" rx="38" ry="48" fill="${item.color}" opacity="0.6"/>
                        <path d="M62 108 Q100 128 138 108 L148 278 Q100 298 52 278 Z" fill="${item.color}" opacity="0.4"/>
                        <path d="M52 278 L32 378" stroke="${item.color}" stroke-width="20" stroke-linecap="round" opacity="0.35"/>
                        <path d="M148 278 L168 378" stroke="${item.color}" stroke-width="20" stroke-linecap="round" opacity="0.35"/>
                    </svg>
                    <div style="position: absolute; bottom: 0; left: 0; right: 0; padding: 14px 12px; z-index: 2;">
                        <div style="font-size: 13px; font-weight: 700; color: white; font-family: var(--font-tech);">${item.name}</div>
                        <div style="font-size: 10px; color: var(--gold); text-transform: uppercase; letter-spacing: 1px; margin-top: 2px;">${item.type}</div>
                    </div>
                    <div style="position: absolute; top: 10px; right: 10px; background: rgba(0,0,0,0.6); border: 1px solid var(--border-gold); border-radius: 6px; padding: 3px 8px; font-size: 9px; color: var(--gold); font-family: var(--font-tech); letter-spacing: 1px;">AI</div>
                </div>
            </div>
        `;
    }

    const row1 = document.getElementById('showcase-inner-1');
    const row2 = document.getElementById('showcase-inner-2');

    let row1HTML = '', row2HTML = '';
    for (let i = 0; i < showcaseItems.length; i++) {
        if (i < 5) row1HTML += buildShowcaseItem(showcaseItems[i], i);
        else row2HTML += buildShowcaseItem(showcaseItems[i], i);
    }
    // Double for seamless loop
    row1.innerHTML = row1HTML + row1HTML;
    row2.innerHTML = row2HTML + row2HTML;

    // ============================================
    // LIVE COUNTER ANIMATION
    // ============================================
    let liveCount = 12847;
    let genCount = 2341;

    setInterval(() => {
        liveCount += Math.floor(Math.random() * 5) - 1;
        genCount += Math.floor(Math.random() * 3) - 1;
        const liveEl = document.getElementById('live-counter');
        const genEl = document.getElementById('gen-counter');
        if (liveEl) liveEl.textContent = liveCount.toLocaleString();
        if (genEl) genEl.textContent = genCount.toLocaleString();
    }, 1800);

    // ============================================
    // SLIDER CONTROLS
    // ============================================
    const ageSlider = document.getElementById('age-slider');
    const ageVal = document.getElementById('age-val');
    const realSlider = document.getElementById('real-slider');
    const realVal = document.getElementById('real-val');

    if (ageSlider) {
        ageSlider.addEventListener('input', () => ageVal.textContent = ageSlider.value);
    }
    if (realSlider) {
        realSlider.addEventListener('input', () => realVal.textContent = realSlider.value + '%');
    }

    // Quality switch
    document.querySelectorAll('.quality-option').forEach(opt => {
        opt.addEventListener('click', function() {
            document.querySelectorAll('.quality-option').forEach(o => o.classList.remove('active'));
            this.classList.add('active');
        });
    });

    // ============================================
    // TOGGLE BUTTON
    // ============================================
    function setActive(el, selector) {
        const parent = el.closest('.panel-section') || el.parentElement;
        parent.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
        el.classList.add('active');
    }
    window.setActive = setActive;

    // ============================================
    // GENERATION ANIMATION
    // ============================================
    function startGeneration() {
        const btn = document.getElementById('gen-btn');
        btn.classList.add('generating');
        btn.textContent = '⟳ Generating...';

        const overlays = [
            document.getElementById('gen-overlay-1'),
            document.getElementById('gen-overlay-2'),
            document.getElementById('gen-overlay-3'),
            document.getElementById('gen-overlay-4'),
        ];

        const progs = [
            document.getElementById('prog-1'),
            document.getElementById('prog-2'),
            document.getElementById('prog-3'),
            document.getElementById('prog-4'),
        ];

        overlays.forEach(o => { if(o) o.style.display = 'flex'; });
        let progress = 0;
        const interval = setInterval(() => {
            progress += Math.random() * 4;
            if (progress >= 100) {
                progress = 100;
                clearInterval(interval);
                setTimeout(() => {
                    overlays.forEach(o => { if(o) o.style.display = 'none'; });
                    btn.classList.remove('generating');
                    btn.textContent = '✦ Generate AI Influencer';
                    showToast('✨', 'Generation Complete! Your AI influencer is ready.');
                }, 300);
            }
            progs.forEach(p => { if(p) p.style.width = progress + '%'; });
        }, 60);
    }
    window.startGeneration = startGeneration;

    // ============================================
    // TOAST NOTIFICATIONS
    // ============================================
    const toastContainer = document.getElementById('toast-container');
    const toastMessages = [
        ['🎉', '<span class="toast-name">@sophia_ai</span> just created a new influencer'],
        ['⚡', '<span class="toast-name">12,400</span> images generated in the last hour'],
        ['🔥', '<span class="toast-name">@marcus_creates</span> is trending with 2M views'],
        ['💎', '<span class="toast-name">@luxury_ai_brand</span> just upgraded to Enterprise'],
        ['🌟', 'New influencer <span class="toast-name">Aria Kim</span> just went viral'],
        ['🚀', '<span class="toast-name">@creator_pro</span> generated 500 images today'],
    ];

    function showToast(icon, message) {
        const toast = document.createElement('div');
        toast.className = 'toast';
        toast.innerHTML = `<div class="toast-icon">${icon}</div><div class="toast-text">${message}</div>`;
        toastContainer.appendChild(toast);
        setTimeout(() => {
            toast.style.animation = 'toast-in 0.4s ease reverse';
            setTimeout(() => toast.remove(), 400);
        }, 4000);
    }

    // Auto show toasts
    let toastIndex = 0;
    function showRandomToast() {
        const [icon, msg] = toastMessages[toastIndex % toastMessages.length];
        showToast(icon, msg);
        toastIndex++;
        setTimeout(showRandomToast, Math.random() * 6000 + 4000);
    }

    setTimeout(showRandomToast, 3000);

    // ============================================
    // PARALLAX MOUSE
    // ============================================
    document.addEventListener('mousemove', (e) => {
        const x = (e.clientX / window.innerWidth - 0.5) * 20;
        const y = (e.clientY / window.innerHeight - 0.5) * 20;
        const floatCards = document.querySelectorAll('.float-card');
        floatCards.forEach((card, i) => {
            const factor = (i + 1) * 0.3;
            card.style.transform += ` translate(${x * factor}px, ${y * factor}px)`;
        });
    });

    // ============================================
    // SCROLL PROGRESS INDICATOR
    // ============================================
    const scrollIndicator = document.createElement('div');
    scrollIndicator.style.cssText = `
        position: fixed; top: 0; left: 0; height: 2px; z-index: 10000;
        background: linear-gradient(90deg, var(--gold-dark), var(--gold-bright));
        transition: width 0.1s ease; width: 0%;
    `;
    document.body.appendChild(scrollIndicator);

    window.addEventListener('scroll', () => {
        const scrollTop = window.scrollY;
        const docHeight = document.documentElement.scrollHeight - window.innerHeight;
        const progress = (scrollTop / docHeight) * 100;
        scrollIndicator.style.width = progress + '%';
    });

    // ============================================
    // ANIMATED GRID ON HERO MOUSE MOVE
    // ============================================
    const heroSection = document.getElementById('hero');
    if (heroSection) {
        heroSection.addEventListener('mousemove', (e) => {
            const rect = heroSection.getBoundingClientRect();
            const x = ((e.clientX - rect.left) / rect.width) * 100;
            const y = ((e.clientY - rect.top) / rect.height) * 100;
            heroSection.querySelector('.hero-bg-gradient').style.background = `
                radial-gradient(ellipse 80% 60% at ${x}% ${y}%, rgba(201,168,76,0.14) 0%, transparent 60%),
                radial-gradient(ellipse 50% 40% at 20% 80%, rgba(201,168,76,0.05) 0%, transparent 50%)
            `;
        });
    }

    console.log('%c✦ NEXORA AI — Premium AI Influencer Platform', 
        'color: #C9A84C; font-size: 16px; font-weight: bold; font-family: monospace;');
    console.log('%cBuilt with ❤️ using pure HTML, CSS & JS', 
        'color: #888; font-size: 12px;');
    </script>
</body>
</html>
