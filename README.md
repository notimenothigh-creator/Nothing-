<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Project Shadow | Ultimate Theme Store Engine</title>
    
    <style>
        /* === GLOBAL CONFIGURATION & DESIGN TOKENS === */
        :root {
            --bg-main: #090d16;
            --bg-surface: rgba(15, 23, 42, 0.6);
            --border-color: rgba(255, 255, 255, 0.08);
            --accent: #38bdf8;
            --accent-glow: rgba(56, 189, 248, 0.3);
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --app-bg-image: none; /* यह डायनेमिकली बदलेगा */
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, -apple-system, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        /* द मास्टर बैकग्राउंड इंजन (लाइव वॉलपेपर सपोर्ट के साथ) */
        body {
            background-color: var(--bg-main);
            background-image: var(--app-bg-image);
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            color: var(--text-main);
            min-height: 100vh;
            transition: background 0.5s ease, color 0.3s ease;
            position: relative;
        }

        /* ग्लास इफ़ेक्ट को बढ़ाने के लिए बैकग्राउंड ओवरले */
        body::before {
            content: '';
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(9, 13, 22, 0.4); /* हल्का डार्क मास्क */
            backdrop-filter: blur(4px); /* वॉलपेपर को हल्का ब्लर करना ताकि टेक्स्ट साफ दिखे */
            z-index: -1;
        }

        /* === PREMIUM UI COMPONENTS === */
        .app-header {
            padding: 1rem 1.5rem;
            background: rgba(15, 23, 42, 0.7);
            backdrop-filter: blur(20px);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .logo-area h2 {
            font-size: 1.3rem;
            letter-spacing: -0.03em;
            background: linear-gradient(to right, #38bdf8, #a855f7);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .nav-buttons {
            display: flex;
            gap: 10px;
        }

        .store-btn {
            background: linear-gradient(135deg, #a855f7, #6366f1);
            color: #fff;
            border: none;
            padding: 0.6rem 1.2rem;
            border-radius: 20px;
            font-weight: 600;
            font-size: 0.85rem;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(168, 85, 247, 0.4);
        }

        .workspace-container {
            padding: 1.5rem;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1.5rem;
            max-width: 1400px;
            margin: 0 auto;
        }

        .glass-card {
            background: var(--bg-surface);
            border: 1px solid var(--border-color);
            border-radius: 20px;
            padding: 1.5rem;
            backdrop-filter: blur(25px);
            -webkit-backdrop-filter: blur(25px);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.21);
            display: flex;
            flex-direction: column;
            gap: 1rem;
            transition: transform 0.2s ease;
        }

        .glass-card:active { transform: scale(0.98); }

        .interactive-btn {
            background: transparent;
            color: var(--accent);
            border: 1px solid var(--accent);
            padding: 0.75rem 1.5rem;
            border-radius: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.25s ease;
            text-align: center;
            width: 100%;
        }

        .interactive-btn:hover, .interactive-btn:active {
            background: var(--accent);
            color: #000;
            box-shadow: 0 0 15px var(--accent-glow);
        }

        /* === ULTRA PREMIUM WALLPAPER STORE PANEL (BOTTOM SHEET STYLING) === */
        .theme-store-overlay {
            position: fixed;
            bottom: -100%; /* छुपा हुआ */
            left: 0; width: 100%; height: 80vh;
            background: rgba(10, 15, 30, 0.95);
            backdrop-filter: blur(30px);
            border-top: 2px solid var(--border-color);
            border-top-left-radius: 30px;
            border-top-right-radius: 30px;
            z-index: 1000;
            transition: bottom 0.4s cubic-bezier(0.1, 0.76, 0.55, 0.94);
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        .theme-store-overlay.active {
            bottom: 0; /* स्क्रीन पर आ जाएगा */
        }

        .store-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* लाखो वॉलपेपर्स का ग्रिड लेआउट */
        .wallpaper-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
            gap: 12px;
            overflow-y: auto;
            flex-grow: 1;
            padding-bottom: 2rem;
        }

        .wallpaper-thumb {
            width: 100%;
            height: 160px;
            border-radius: 12px;
            object-fit: cover;
            border: 2px solid transparent;
            cursor: pointer;
            transition: transform 0.2s, border-color 0.2s;
            background: #1e293b; /* लोडिंग स्टेट कलर */
        }

        .wallpaper-thumb:active {
            transform: scale(0.95);
        }

        .wallpaper-thumb.selected {
            border-color: var(--accent);
            box-shadow: 0 0 10px var(--accent-glow);
        }

        .close-store {
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            border: none;
            width: 35px; height: 35px;
            border-radius: 50%;
            font-weight: bold;
            cursor: pointer;
        }

        p { color: var(--text-muted); font-size: 0.9rem; }
    </style>
</head>
<body>

    <header class="app-header">
        <div class="logo-area">
            <h2>SHADOW ENGINE</h2>
            <p style="font-size: 0.75rem;">Premium Studio</p>
        </div>
        <div class="nav-buttons">
            <button class="store-btn" onclick="StoreEngine.openStore()">✨ Theme Store</button>
        </div>
    </header>

    <div class="workspace-container" id="main-workspace">
        <div class="glass-card">
            <h3>Minimal Portfolio</h3>
            <p>Clean and fast template optimized for developers and designers.</p>
            <button class="interactive-btn">Customize Template</button>
        </div>

        <div class="glass-card">
            <h3>E-Book Landing Page</h3>
            <p>High-conversion layout designed to sell digital products.</p>
            <button class="interactive-btn">Customize Template</button>
        </div>

        <div class="glass-card">
            <h3>Cyber-Link Hub</h3>
            <p>Mobile-first social media bio linker with custom neon widgets.</p>
            <button class="interactive-btn">Customize Template</button>
        </div>
    </div>

    <div class="theme-store-overlay" id="theme-store">
        <div class="store-header">
            <div>
                <h3>🌌 Live Wallpaper & Theme Store</h3>
                <p>Select any premium skin to apply it instantly to your mobile app workspace</p>
            </div>
            <button class="close-store" onclick="StoreEngine.closeStore()">✕</button>
        </div>

        <div class="wallpaper-grid" id="store-wallpapers">
            </div>
    </div>

    <script>
        const StoreEngine = {
            // हाई-रेसोल्यूशन वॉलपेपर डेटाबेस आईडी (Unsplash से ली गई बेस्ट कैटेगरीज)
            // इन्हें बदलकर आप लाखों इमेजेस का पूल जनरेट कर सकते हैं
            wallpaperIds: [
                '1508739174042-c02c97b010e2', // Cyberpunk Purple
                '1618005182384-a83a8bd57fbe', // Abstract Premium Wave
                '1607604276583-eef5d076aa5f', // Neon Tokyo Gaming
                '1579546929518-9e396f3cc809', // Fluid Gradient Blue
                '1518770660439-4636190af475', // Tech Matrix Grid
                '1535223289827-42f1e9919769', // Dark Cyber Tech
                '1507525428034-b723cf961d3e', // Premium Minimal Nature
                '1528459801416-a9e53bbf4e17'  // Aesthetic Pastel
            ],

            init: function() {
                this.renderWallpapers();
                this.loadSavedWallpaper();
            },

            openStore: function() {
                this.triggerHaptic(60);
                document.getElementById('theme-store').classList.add('active');
            },

            closeStore: function() {
                this.triggerHaptic(30);
                document.getElementById('theme-store').classList.remove('active');
            },

            triggerHaptic: function(ms = 40) {
                if ('vibrate' in navigator) navigator.vibrate(ms);
            },

            // लाखों इमेजेस लोड करने वाला रेंडरिंग इंजन
            renderWallpapers: function() {
                const grid = document.getElementById('store-wallpapers');
                grid.innerHTML = ''; // क्लियर करना

                this.wallpaperIds.forEach((id, index) => {
                    const imgUrl = `https://images.unsplash.com/photo-${id}?auto=format&fit=crop&w=400&q=80`;
                    const fullImgUrl = `https://images.unsplash.com/photo-${id}?auto=format&fit=crop&w=1200&q=90`;

                    const imgElement = document.createElement('img');
                    imgElement.src = imgUrl;
                    imgElement.className = 'wallpaper-thumb';
                    imgElement.alt = 'Premium Wallpaper';
                    
                    // वॉलपेपर पर क्लिक करने का मास्टर इवेंट
                    imgElement.addEventListener('click', () => {
                        this.triggerHaptic(50);
                        
                        // पुरानी सिलेक्टेड इमेज से बॉर्डर हटाना
                        const active = document.querySelector('.wallpaper-thumb.selected');
                        if (active) active.classList.remove('selected');
                        
                        // नए वाले पर बॉर्डर लगाना
                        imgElement.classList.add('selected');
                        
                        // पूरे ऐप का वॉलपेपर बदलना
                        this.applyWallpaper(fullImgUrl);
                    });

                    grid.appendChild(imgElement);
                });
            },

            applyWallpaper: function(url) {
                // CSS variable `--app-bg-image` को लाइव बदलना
                document.documentElement.style.setProperty('--app-app-bg-image', `url('${url}')`);
                // सेफ साइड के लिए डायरेक्ट बॉडी स्टाइल भी बदलना
                document.body.style.setProperty('--app-bg-image', `url('${url}')`);
                
                // लोकल स्टोरेज में सेव करना ताकि ऐप बंद करने पर वॉलपेपर न हटे
                localStorage.setItem('shadow_wallpaper', url);
                console.log("🌌 Premium UI Wallpaper Configuration Engine Applied Successfully.");
            },

            loadSavedWallpaper: function() {
                const savedWall = localStorage.getItem('shadow_wallpaper');
                if (savedWall) {
                    this.applyWallpaper(savedWall);
                }
            }
        };

        // DOM लोड होते ही इंजन को एक्टिवेट करना
        document.addEventListener('DOMContentLoaded', () => {
            StoreEngine.init();
        });
    </script>
</body>
</html>
