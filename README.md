# Pour-nos-8-mois
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Pour Sarah - 8 Mois ❤️</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;1,400;1,600&family=Montserrat:wght@300;400;500;600&family=Sacramento&display=swap" rel="stylesheet">
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <style>
        /* Modern reset and font family assignment */
        body {
            font-family: 'Montserrat', sans-serif;
            background: linear-gradient(135deg, #1e1b2e 0%, #2a1f3d 50%, #171226 100%);
            overflow-x: hidden;
            touch-action: manipulation;
        }

        .font-serif-custom {
            font-family: 'Cormorant Garamond', serif;
        }

        .font-cursive {
            font-family: 'Sacramento', cursive;
        }

        /* Glassmorphism styling */
        .glass-card {
            background: rgba(255, 255, 255, 0.07);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.15);
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.4);
        }

        .glass-card-message {
            background: rgba(255, 245, 247, 0.94);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 215, 225, 0.6);
            box-shadow: 0 25px 60px rgba(0, 0, 0, 0.35);
        }

        /* Floating background elements animation */
        @keyframes floatSlow {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-15px) rotate(3deg); }
        }

        .animate-float {
            animation: floatSlow 4s ease-in-out infinite;
        }

        @keyframes pulseGlow {
            0%, 100% { opacity: 0.4; filter: drop-shadow(0 0 15px rgba(244, 114, 182, 0.4)); }
            50% { opacity: 0.8; filter: drop-shadow(0 0 25px rgba(244, 114, 182, 0.7)); }
        }

        .pulse-glow {
            animation: pulseGlow 3s ease-in-out infinite;
        }

        /* Canvas particles layer */
        #bgCanvas, #burstCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        #bgCanvas { z-index: 1; }
        #burstCanvas { z-index: 50; }

        /* Draggable flower styles */
        #flower-draggable {
            cursor: grab;
            touch-action: none;
            transition: transform 0.1s ease-out;
            user-select: none;
            -webkit-user-select: none;
        }

        #flower-draggable:active {
            cursor: grabbing;
        }

        /* Smooth reveal transitions */
        .fade-out {
            opacity: 0;
            transform: scale(0.9) translateY(-20px);
            transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
            pointer-events: none;
        }

        .fade-in-up {
            opacity: 1 !important;
            transform: translateY(0) scale(1) !important;
            transition: all 1s cubic-bezier(0.16, 1, 0.3, 1) 0.3s;
        }

        .hidden-card {
            opacity: 0;
            transform: translateY(40px) scale(0.95);
            pointer-events: none;
        }

        .active-card {
            pointer-events: auto !important;
        }

        /* Custom scrollbar for message card */
        .custom-scrollbar::-webkit-scrollbar {
            width: 5px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: rgba(255, 200, 210, 0.2);
            border-radius: 10px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: rgba(225, 112, 150, 0.5);
            border-radius: 10px;
        }
    </style>
</head>
<body class="min-h-screen w-full flex items-center justify-center p-4 relative text-slate-100 selection:bg-pink-500 selection:text-white">

    <!-- Ambient background canvas (Stars & Floating Hearts) -->
    <canvas id="bgCanvas"></canvas>
    
    <!-- Magic Burst canvas for interaction effects -->
    <canvas id="burstCanvas"></canvas>

    <!-- Main Container -->
    <main class="w-full max-w-md mx-auto z-10 flex flex-col items-center justify-center min-h-[90vh]">

        <!-- HEADER TITLE -->
        <header id="header-section" class="text-center mb-6 transition-all duration-700">
            <span class="inline-block text-xs uppercase tracking-[0.3em] text-pink-300 font-semibold mb-1 px-3 py-1 rounded-full bg-pink-500/10 border border-pink-500/20">
                8 Mois Ensemble
            </span>
            <h1 class="text-3xl sm:text-4xl font-serif-custom font-semibold text-rose-100 tracking-wide">
                Pour toi, <span class="font-cursive text-4xl sm:text-5xl text-pink-300">Sarah</span>
            </h1>
        </header>

        <!-- STEP 1: INTERACTIVE TEDDY BEAR WITH FLOWER CARD -->
        <section id="interactive-card" class="w-full glass-card rounded-3xl p-6 text-center relative flex flex-col items-center justify-between min-h-[420px] transition-all duration-700">
            
            <!-- Instruction badge -->
            <div id="drag-hint" class="animate-bounce bg-pink-500/20 text-pink-200 border border-pink-400/30 text-xs py-1.5 px-4 rounded-full flex items-center gap-2 mb-2 shadow-lg">
                <i data-lucide="hand" class="w-3.5 h-3.5"></i>
                <span>Prends la fleur et tire-la vers le haut !</span>
            </div>

            <!-- Central SVG Illustration Container -->
            <div class="relative w-64 h-64 my-auto flex items-center justify-center">
                
                <!-- Background Soft Glow -->
                <div class="absolute w-48 h-48 bg-pink-500/20 rounded-full blur-2xl pulse-glow"></div>

                <!-- TEDDY BEAR SVG -->
                <svg id="teddy-bear" class="w-56 h-56 relative z-10 filter drop-shadow-xl animate-float" viewBox="0 0 200 200" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <!-- Ears -->
                    <circle cx="55" cy="55" r="22" fill="#B38B6D" />
                    <circle cx="55" cy="55" r="14" fill="#E8C39E" />
                    <circle cx="145" cy="55" r="22" fill="#B38B6D" />
                    <circle cx="145" cy="55" r="14" fill="#E8C39E" />

                    <!-- Bear Head -->
                    <circle cx="100" cy="85" r="48" fill="#C49A78" />

                    <!-- Snout -->
                    <ellipse cx="100" cy="96" rx="20" ry="15" fill="#F4DDBE" />
                    <ellipse cx="100" cy="90" rx="8" ry="6" fill="#4A3525" />
                    <!-- Mouth -->
                    <path d="M 94 97 Q 100 102 106 97" stroke="#4A3525" stroke-width="2.5" stroke-linecap="round" fill="none" />

                    <!-- Eyes & Blush -->
                    <circle cx="82" cy="78" r="4.5" fill="#2C1D11" />
                    <circle cx="118" cy="78" r="4.5" fill="#2C1D11" />
                    <circle cx="83.5" cy="76.5" r="1.5" fill="white" />
                    <circle cx="119.5" cy="76.5" r="1.5" fill="white" />
                    <!-- Cheek Blush -->
                    <ellipse cx="73" cy="88" rx="6" ry="3.5" fill="#F472B6" opacity="0.4" />
                    <ellipse cx="127" cy="88" rx="6" ry="3.5" fill="#F472B6" opacity="0.4" />

                    <!-- Bear Body -->
                    <path d="M 65 125 C 65 110, 135 110, 135 125 C 145 155, 140 180, 100 180 C 60 180, 55 155, 65 125 Z" fill="#B38B6D" />
                    <!-- Belly Pad -->
                    <ellipse cx="100" cy="148" rx="24" ry="20" fill="#E8C39E" />

                    <!-- Feet -->
                    <ellipse cx="60" cy="172" rx="16" ry="12" fill="#9E7659" />
                    <ellipse cx="60" cy="172" rx="10" ry="7" fill="#E8C39E" />
                    <ellipse cx="140" cy="172" rx="16" ry="12" fill="#9E7659" />
                    <ellipse cx="140" cy="172" rx="10" ry="7" fill="#E8C39E" />

                    <!-- Arms Holding Position -->
                    <path d="M 65 125 C 50 135, 60 155, 82 150" stroke="#9E7659" stroke-width="14" stroke-linecap="round" fill="none" />
                    <path d="M 135 125 C 150 135, 140 155, 118 150" stroke="#9E7659" stroke-width="14" stroke-linecap="round" fill="none" />
                </svg>

                <!-- INTERACTIVE DRAGGABLE FLOWER SVG -->
                <div id="flower-draggable" class="absolute z-20 top-20 right-16 w-28 h-36 flex items-center justify-center">
                    <svg class="w-full h-full filter drop-shadow-lg" viewBox="0 0 100 130" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <!-- Stem -->
                        <path d="M 50 55 Q 40 85 48 120" stroke="#4ADE80" stroke-width="4.5" stroke-linecap="round" fill="none" />
                        <!-- Leaf -->
                        <path d="M 46 85 Q 25 80 32 95 Q 42 95 46 88" fill="#22C55E" />
                        
                        <!-- Flower Petals -->
                        <g id="petals">
                            <circle cx="50" cy="32" r="14" fill="#FB7185" />
                            <circle cx="37" cy="45" r="14" fill="#F43F5E" />
                            <circle cx="63" cy="45" r="14" fill="#F43F5E" />
                            <circle cx="42" cy="58" r="14" fill="#E11D48" />
                            <circle cx="58" cy="58" r="14" fill="#E11D48" />
                            <!-- Flower Center -->
                            <circle cx="50" cy="48" r="11" fill="#FACC15" />
                            <circle cx="50" cy="48" r="8" fill="#FEF08A" />
                        </g>
                    </svg>

                    <!-- Touch indicator glow -->
                    <div class="absolute inset-0 rounded-full bg-pink-400/20 blur-md pointer-events-none animate-ping"></div>
                </div>

            </div>

            <!-- Footer note -->
            <p class="text-xs text-rose-200/70 font-light tracking-wide mt-2">
                Offre cette fleur à mon cœur...
            </p>
        </section>

        <!-- STEP 2: REVEALED ROMANTIC LOVE LETTER CARD -->
        <section id="message-card" class="w-full glass-card-message rounded-3xl p-6 text-slate-800 relative hidden-card active-card transition-all duration-1000 hidden">
            
            <!-- Floating Heart Icon Decor -->
            <div class="absolute -top-5 left-1/2 -translate-x-1/2 w-11 h-11 bg-rose-500 rounded-full flex items-center justify-center shadow-lg border-2 border-white text-white">
                <i data-lucide="heart" class="w-6 h-6 fill-white"></i>
            </div>

            <div class="pt-4 pb-2 text-center border-b border-rose-200/60 mb-4">
                <h2 class="font-serif-custom text-2xl font-bold text-rose-900 tracking-wide">Mon Amour,</h2>
                <p class="text-xs text-rose-500 font-medium uppercase tracking-widest mt-0.5">8 Mois de nous</p>
            </div>

            <!-- Letter Body with scrollbar -->
            <div class="max-h-[55vh] overflow-y-auto custom-scrollbar pr-2 space-y-4 text-sm leading-relaxed text-slate-700 font-normal">
                <p>
                    Cela donc fait 8 mois que j’ai rencontré la femme de ma vie, mais je sais que pour toi en ce moment tu ne veux plus de moi, je m’en veux tellement sur beaucoup de points. Et je veux t’aider à aller mieux pck c’est mon rôle. 
                    <br>
                    Je veux que tu restes à mes côtés, pour continuer notre meilleure histoire, à écrire les meilleurs chapitres de notre vie ensemble.
                </p>

                <p class="pl-3 border-l-2 border-rose-400 italic text-slate-800">
                    Tu as bien évidemment tout ce dont j’ai rêvé, en fait la réalité est que j’aurais jamais imaginé qu’une personne comme toi existe. Je sais que je vais me répéter mais tu es vraiment exceptionnelle, de la beauté dans toutes ses formes, à mes yeux tu es parfaite.
                </p>

                <p>
                    Je suis trop admiratif et jaloux de ton intelligence pck je sais que je suis juste un petit con mdrrr, mais je suis fier que tu sois à mes côtés, que j’aie la femme la plus belle à mes yeux.
                </p>

                <p>
                    Tous les jours, même chaque seconde je pense à toi et je t’oublie jamais dans mes prières pour qu’on puisse s’assembler (je sais que tu veux plus), mais surtout pour que tout s’améliore inchAllah. Pck voir et entendre ton sourire c’est le plus beau cadeau que je puisse avoir.
                </p>

                <div class="pt-3 border-t border-rose-200/60 text-right">
                    <p class="text-xs text-rose-500 font-medium">De la part de</p>
                    <p class="font-serif-custom text-lg font-bold text-rose-900 tracking-wide">Ton plus grand admirateur ‼️‼️</p>
                </div>
            </div>

            <!-- Bottom Replay Button -->
            <div class="mt-4 pt-3 flex justify-center">
                <button id="restart-btn" class="text-xs text-rose-500 hover:text-rose-700 flex items-center gap-1.5 py-1 px-3 rounded-full hover:bg-rose-100/50 transition">
                    <i data-lucide="rotate-ccw" class="w-3.5 h-3.5"></i>
                    <span>Revoir la magie</span>
                </button>
            </div>
        </section>

    </main>

    <!-- JavaScript Interaction Logic -->
    <script>
        // Initialize Lucide icons
        lucide.createIcons();

        // Canvas Setup for Background Ambient Animation
        const bgCanvas = document.getElementById('bgCanvas');
        const bgCtx = bgCanvas.getContext('2d');
        const burstCanvas = document.getElementById('burstCanvas');
        const burstCtx = burstCanvas.getContext('2d');

        let width = bgCanvas.width = burstCanvas.width = window.innerWidth;
        let height = bgCanvas.height = burstCanvas.height = window.innerHeight;

        window.addEventListener('resize', () => {
            width = bgCanvas.width = burstCanvas.width = window.innerWidth;
            height = bgCanvas.height = burstCanvas.height = window.innerHeight;
            initStars();
        });

        // Background Particles (Stars & Floating Hearts)
        let particles = [];
        function initStars() {
            particles = [];
            const count = Math.floor((width * height) / 10000);
            for (let i = 0; i < count; i++) {
                particles.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    size: Math.random() * 2 + 0.5,
                    alpha: Math.random(),
                    speed: Math.random() * 0.02 + 0.005,
                    isHeart: Math.random() < 0.25,
                    floatSpeedY: Math.random() * 0.4 + 0.1
                });
            }
        }
        initStars();

        function drawHeart(ctx, x, y, size, color, alpha) {
            ctx.save();
            ctx.globalAlpha = alpha;
            ctx.fillStyle = color;
            ctx.beginPath();
            const topCurveHeight = size * 0.3;
            ctx.moveTo(x, y + topCurveHeight);
            ctx.bezierCurveTo(x, y, x - size / 2, y, x - size / 2, y + topCurveHeight);
            ctx.bezierCurveTo(x - size / 2, y + (size + topCurveHeight) / 2, x, y + size, x, y + size);
            ctx.bezierCurveTo(x, y + size, x + size / 2, y + (size + topCurveHeight) / 2, x + size / 2, y + topCurveHeight);
            ctx.bezierCurveTo(x + size / 2, y, x, y, x, y + topCurveHeight);
            ctx.closePath();
            ctx.fill();
            ctx.restore();
        }

        function renderBackground() {
            bgCtx.clearRect(0, 0, width, height);

            particles.forEach(p => {
                p.alpha += p.speed;
                if (p.alpha > 1 || p.alpha < 0.2) p.speed = -p.speed;

                if (p.isHeart) {
                    p.y -= p.floatSpeedY;
                    if (p.y < -20) p.y = height + 20;
                    drawHeart(bgCtx, p.x, p.y, p.size * 5, '#F472B6', p.alpha * 0.5);
                } else {
                    bgCtx.fillStyle = `rgba(255, 255, 255, ${p.alpha})`;
                    bgCtx.beginPath();
                    bgCtx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                    bgCtx.fill();
                }
            });

            requestAnimationFrame(renderBackground);
        }
        renderBackground();

        // Burst Effects Particles logic on Flower pluck
        let burstParticles = [];
        function createBurst(originX, originY) {
            burstParticles = [];
            const colors = ['#F43F5E', '#FB7185', '#FBBF24', '#F472B6', '#FFFFFF'];
            
            for (let i = 0; i < 90; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = Math.random() * 8 + 2;
                burstParticles.push({
                    x: originX,
                    y: originY,
                    vx: Math.cos(angle) * speed,
                    vy: Math.sin(angle) * speed - 1,
                    size: Math.random() * 8 + 4,
                    color: colors[Math.floor(Math.random() * colors.length)],
                    alpha: 1,
                    decay: Math.random() * 0.02 + 0.01,
                    isHeart: Math.random() > 0.4
                });
            }
        }

        function renderBurst() {
            burstCtx.clearRect(0, 0, width, height);
            
            burstParticles.forEach((p, index) => {
                p.x += p.vx;
                p.y += p.vy;
                p.vy += 0.1; // gravity
                p.alpha -= p.decay;

                if (p.alpha <= 0) {
                    burstParticles.splice(index, 1);
                } else {
                    if (p.isHeart) {
                        drawHeart(burstCtx, p.x, p.y, p.size, p.color, p.alpha);
                    } else {
                        burstCtx.save();
                        burstCtx.globalAlpha = p.alpha;
                        burstCtx.fillStyle = p.color;
                        burstCtx.beginPath();
                        burstCtx.arc(p.x, p.y, p.size / 2, 0, Math.PI * 2);
                        burstCtx.fill();
                        burstCtx.restore();
                    }
                }
            });

            if (burstParticles.length > 0) {
                requestAnimationFrame(renderBurst);
            }
        }

        // DRAG & DROP / CLICK INTERACTION
        const flower = document.getElementById('flower-draggable');
        const interactiveCard = document.getElementById('interactive-card');
        const messageCard = document.getElementById('message-card');
        const headerSection = document.getElementById('header-section');
        const restartBtn = document.getElementById('restart-btn');

        let isDragging = false;
        let startX, startY, currentX = 0, currentY = 0;
        let flowerDetached = false;

        // Touch and Mouse listeners
        function onPointerDown(e) {
            if (flowerDetached) return;
            isDragging = true;
            const clientX = e.touches ? e.touches[0].clientX : e.clientX;
            const clientY = e.touches ? e.touches[0].clientY : e.clientY;
            startX = clientX - currentX;
            startY = clientY - currentY;
            flower.style.transition = 'none';
        }

        function onPointerMove(e) {
            if (!isDragging || flowerDetached) return;
            e.preventDefault();
            const clientX = e.touches ? e.touches[0].clientX : e.clientX;
            const clientY = e.touches ? e.touches[0].clientY : e.clientY;
            
            currentX = clientX - startX;
            currentY = clientY - startY;

            flower.style.transform = `translate3d(${currentX}px, ${currentY}px, 0) rotate(${currentX * 0.1}deg)`;

            // Check distance threshold to trigger release automatically
            const dist = Math.hypot(currentX, currentY);
            if (dist > 70) {
                triggerFlowerPlucked(clientX, clientY);
            }
        }

        function onPointerUp() {
            if (!isDragging || flowerDetached) return;
            isDragging = false;
            
            // If moved slightly or tapped, trigger transition
            const dist = Math.hypot(currentX, currentY);
            if (dist > 30) {
                const rect = flower.getBoundingClientRect();
                triggerFlowerPlucked(rect.left + rect.width / 2, rect.top + rect.height / 2);
            } else {
                // Reset position smoothly
                flower.style.transition = 'transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275)';
                flower.style.transform = 'translate3d(0px, 0px, 0)';
                currentX = 0;
                currentY = 0;
            }
        }

        // Click alternative (Tap to release directly)
        flower.addEventListener('click', (e) => {
            if (flowerDetached) return;
            const rect = flower.getBoundingClientRect();
            triggerFlowerPlucked(rect.left + rect.width / 2, rect.top + rect.height / 2);
        });

        // Event Bindings
        flower.addEventListener('mousedown', onPointerDown);
        window.addEventListener('mousemove', onPointerMove);
        window.addEventListener('mouseup', onPointerUp);

        flower.addEventListener('touchstart', onPointerDown, { passive: false });
        window.addEventListener('touchmove', onPointerMove, { passive: false });
        window.addEventListener('touchend', onPointerUp);

        // Core Trigger Function when Flower is Removed
        function triggerFlowerPlucked(burstX, burstY) {
            if (flowerDetached) return;
            flowerDetached = true;
            isDragging = false;

            // Animate flower flying off
            flower.style.transition = 'all 0.6s ease-out';
            flower.style.transform = `translate3d(${currentX * 1.5}px, ${currentY - 150}px, 0) scale(1.3) rotate(45deg)`;
            flower.style.opacity = '0';

            // Start particle explosion
            createBurst(burstX, burstY);
            renderBurst();

            // Transition cards
            setTimeout(() => {
                interactiveCard.classList.add('fade-out');
                
                setTimeout(() => {
                    interactiveCard.classList.add('hidden');
                    messageCard.classList.remove('hidden');
                    
                    // Force reflow
                    void messageCard.offsetWidth;
                    
                    messageCard.classList.remove('hidden-card');
                    messageCard.classList.add('fade-in-up');
                }, 400);

            }, 300);
        }

        // Reset Interaction
        restartBtn.addEventListener('click', () => {
            flowerDetached = false;
            currentX = 0;
            currentY = 0;

            messageCard.classList.remove('fade-in-up');
            messageCard.classList.add('hidden-card');

            setTimeout(() => {
                messageCard.classList.add('hidden');
                interactiveCard.classList.remove('hidden', 'fade-out');
                
                flower.style.transition = 'none';
                flower.style.transform = 'translate3d(0px, 0px, 0) scale(1) rotate(0deg)';
                flower.style.opacity = '1';
            }, 500);
        });
    </script>
</body>
</html>
