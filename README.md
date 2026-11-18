<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday, My Love — Open When...</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Caveat:wght@500;600;700&family=Cormorant+Garamond:ital,wght@0,500;0,600;0,700;1,500;1,600&family=Plus+Jakarta+Sans:wght@400;500;600&display=swap" rel="stylesheet">
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

    <style>
        :root {
            --bg-ivory: #FFFDF9;
            --pure-white: #FFFFFF;
            --blush-soft: #FFF1F3;
            --blush-border: #FFE4E8;
            --rose-primary: #E11D48;
            --rose-soft: #FB7185;
            --gold-foil: #D4AF37;
            --gold-light: #F3E5AB;
            --text-dark: #271C1E;
            --text-muted: #6E5A5E;
        }

        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            background-color: var(--bg-ivory);
            color: var(--text-dark);
            font-family: 'Plus Jakarta Sans', sans-serif;
            overflow-x: hidden;
            background-image: 
                radial-gradient(circle at 12% 10%, rgba(255, 228, 232, 0.55) 0%, transparent 42%),
                radial-gradient(circle at 88% 85%, rgba(243, 229, 171, 0.3) 0%, transparent 48%);
            min-height: 100vh;
        }

        .font-serif-editorial {
            font-family: 'Cormorant Garamond', serif;
        }

        .font-handwritten {
            font-family: 'Caveat', cursive;
        }

        #ambient-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        /* 3D Envelope Styling */
        .envelope-wrapper {
            perspective: 1200px;
            cursor: pointer;
            transition: transform 0.35s cubic-bezier(0.22, 1, 0.36, 1);
        }

        .envelope-wrapper:hover {
            transform: translateY(-8px);
        }

        .envelope-wrapper:active {
            transform: translateY(-2px) scale(0.98);
        }

        .envelope-card {
            position: relative;
            width: 100%;
            aspect-ratio: 16 / 10.5;
            background: linear-gradient(145deg, #FFFFFF 0%, #FFF8F9 100%);
            border-radius: 16px;
            box-shadow: 
                0 12px 30px -8px rgba(225, 29, 72, 0.08),
                0 4px 12px -2px rgba(39, 28, 30, 0.04),
                inset 0 0 0 1px rgba(255, 228, 232, 0.9);
            overflow: hidden;
            transition: box-shadow 0.35s ease;
        }

        .envelope-wrapper:hover .envelope-card {
            box-shadow: 
                0 20px 38px -10px rgba(225, 29, 72, 0.14),
                0 8px 18px -4px rgba(212, 175, 55, 0.16),
                inset 0 0 0 1px rgba(212, 175, 55, 0.45);
        }

        .letter-peek {
            position: absolute;
            bottom: 8%;
            left: 7%;
            right: 7%;
            height: 75%;
            background: #FFFFFF;
            border-radius: 10px;
            border: 1px solid rgba(212, 175, 55, 0.2);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
            transition: transform 0.55s cubic-bezier(0.34, 1.56, 0.64, 1);
            z-index: 1;
            display: flex;
            align-items: flex-start;
            justify-content: center;
            padding-top: 14px;
        }

        .envelope-wrapper.opening .letter-peek {
            transform: translateY(-38%);
        }

        .envelope-pocket {
            position: absolute;
            inset: 0;
            z-index: 2;
            pointer-events: none;
        }

        .envelope-pocket::before {
            content: '';
            position: absolute;
            inset: 0;
            background: linear-gradient(135deg, #FFF5F7 50%, transparent 50%),
                        linear-gradient(225deg, #FFEDF0 50%, transparent 50%);
            opacity: 0.95;
        }

        .envelope-bottom {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 62%;
            background: linear-gradient(180deg, #FFFFFF 0%, #FFF1F3 100%);
            clip-path: polygon(0 100%, 50% 18%, 100% 100%);
            z-index: 3;
            box-shadow: 0 -4px 12px rgba(225, 29, 72, 0.04);
        }

        .envelope-flap {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 58%;
            background: linear-gradient(180deg, #FFE4E8 0%, #FFF5F7 100%);
            clip-path: polygon(0 0, 100% 0, 50% 95%);
            transform-origin: top center;
            transition: transform 0.55s cubic-bezier(0.4, 0, 0.2, 1), z-index 0.2s;
            z-index: 4;
            filter: drop-shadow(0 4px 6px rgba(225, 29, 72, 0.08));
        }

        .envelope-wrapper.opening .envelope-flap {
            transform: rotateX(180deg);
            z-index: 0;
        }

        .wax-seal {
            position: absolute;
            top: 47%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 48px;
            height: 48px;
            border-radius: 50%;
            background: radial-gradient(circle at 35% 35%, #F9D5DF, #F1B3C4 70%, #E092A8 100%);
            box-shadow: 
                0 4px 10px rgba(200,
