<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Titan Strand Global - Steel Wire Rope Expert</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .rtl { direction: rtl; }
        .ltr { direction: ltr; }
        /* 隐藏滚动条但保留功能 */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="bg-white text-slate-900 transition-all duration-300">
    <div id="app"></div>

    <script>
        // --- 1. 全球语言配置 ---
        const LANGUAGES = [
            { code: 'en', name: 'English', flag: '🇺🇸', dir: 'ltr' },
            { code: 'zh', name: '简体中文', flag: '🇨🇳', dir: 'ltr' },
            { code: 'es', name: 'Español', flag: '🇪🇸', dir: 'ltr' },
            { code: 'ar', name: 'العربية', flag: '🇸🇦', dir: 'rtl' },
            { code: 'ru', name: 'Русский', flag: '🇷🇺', dir: 'ltr' },
            { code: 'de', name: 'Deutsch', flag: '🇩🇪', dir: 'ltr' },
            { code: 'fr', name: 'Français', flag: '🇫🇷', dir: 'ltr' },
            { code: 'jp', name: '日本語', flag: '🇯🇵', dir: 'ltr' },
            { code: 'kr', name: '한국어', flag: '🇰🇷', dir: 'ltr' },
            { code: 'tr', name: 'Türkçe', flag: '🇹🇷', dir: 'ltr' },
            { code: 'pt', name: 'Português', flag: '🇧🇷', dir: 'ltr' },
            { code: 'it', name: 'Italiano', flag: '🇮🇹', dir: 'ltr' },
            { code: 'vn', name: 'Tiếng Việt', flag: '🇻🇳', dir: 'ltr' },
            { code: 'th', name: 'ไทย', flag: '🇹🇭', dir: 'ltr' },
            { code: 'id', name: 'Bahasa Indonesia', flag: '🇮🇩', dir: 'ltr' }
        ];

        // --- 2. 核心翻译字典 ---
        const TRANSLATIONS = {
            en: { home: "Home", products: "Products", quote: "Inquiry", heroTitle: "Premium Steel Ropes", heroSub: "Global Industrial Solutions", add: "Add to Quote", price: "Price" },
            zh: { home: "首页", products: "产品中心", quote: "询价清单", heroTitle: "优质钢丝绳索具", heroSub: "全球工业解决方案", add: "加入询价", price: "参考价" },
            es: { home: "Inicio", products: "Productos", quote: "Cotización", heroTitle: "Cables de Acero", heroSub: "Soluciones Industriales", add: "Añadir", price: "Precio" },
            ar: { home: "الرئيسية", products: "المنتجات", quote: "تسعير", heroTitle: "حبال الصلب الممتازة", heroSub: "حلول صناعية عالمية", add: "أضف للتسعير", price: "السعر" },
            ru: { home: "Главная", products: "Продукция", quote: "Запрос", heroTitle: "Стальные канаты", heroSub: "Промышленные решения", add: "В корзину", price: "Цена" },
            jp: { home: "ホーム", products: "製品", quote: "見積", heroTitle: "高品質ワイヤーロープ", heroSub: "グローバル産業ソリューション", add: "見積に追加", price: "価格" },
            kr: { home: "홈", products: "제품", quote: "견적", heroTitle: "프리미엄 와이어 로프", heroSub: "글로벌 산업 솔루션", add: "견적 추가", price: "가격" }
            // ... 其他语言会自动回退到英文或在此扩展
        };

        // 简化的状态管理
        let state = {
            currentLang: 'en',
            cartCount: 0,
            isLangOpen: false
        };

        function t(key) {
            return (TRANSLATIONS[state.currentLang] && TRANSLATIONS[state.currentLang][key]) || TRANSLATIONS['en'][key];
        }

        // --- 3. 渲染函数 ---
        function render() {
            const app = document.getElementById('app');
            const langData = LANGUAGES.find(l => l.code === state.currentLang);
            const isRTL = langData.dir === 'rtl';

            app.className = isRTL ? 'rtl' : 'ltr';

            app.innerHTML = `
                <!-- 导航栏 -->
                <nav class="bg-slate-900 text-white sticky top-0 z-50 shadow-lg">
                    <div class="container mx-auto px-4 h-16 flex justify-between items-center">
                        <div class="flex items-center gap-2 font-black text-xl tracking-tighter">
                            <i data-lucide="anchor" class="text-blue-500"></i>
                            <span>TITAN STRAND</span>
                        </div>
                        
                        <div class="flex items-center gap-4">
                            <!-- 语言切换器 -->
                            <div class="relative">
                                <button onclick="toggleLang()" class="flex items-center gap-2 bg-slate-800 px-3 py-1 rounded-md border border-slate-700 hover:bg-slate-700 transition">
                                    <span>${langData.flag}</span>
                                    <span class="text-xs uppercase font-bold">${state.currentLang}</span>
                                    <i data-lucide="chevron-down" size="14"></i>
                                </button>
                                
                                <div id="langMenu" class="${state.isLangOpen ? 'block' : 'hidden'} absolute right-0 top-full mt-2 w-48 bg-white text-slate-900 rounded-xl shadow-2xl border border-slate-100 overflow-hidden z-[100]">
                                    <div class="max-h-64 overflow-y-auto no-scrollbar">
                                        ${LANGUAGES.map(l => `
                                            <button onclick="setLanguage('${l.code}')" class="w-full text-left px-4 py-3 text-sm flex items-center gap-3 hover:bg-blue-50 transition ${state.currentLang === l.code ? 'bg-blue-50 font-bold text-blue-600' : ''}">
                                                <span class="text-xl">${l.flag}</span>
                                                <span>${l.name}</span>
                                            </button>
                                        `).join('')}
                                    </div>
                                </div>
                            </div>

                            <button class="relative p-2 hover:bg-slate-800 rounded-full transition">
                                <i data-lucide="shopping-cart" size="20"></i>
                                <span class="absolute -top-1 -right-1 bg-blue-600 text-[10px] w-4 h-4 rounded-full flex items-center justify-center font-bold">${state.cartCount}</span>
                            </button>
                        </div>
                    </div>
                </nav>

                <!-- Hero 主视觉 -->
                <header class="bg-slate-900 text-white py-20 relative">
                    <div class="container mx-auto px-4 text-center relative z-10">
                        <h1 class="text-4xl md:text-6xl font-black mb-6 leading-tight">${t('heroTitle')}</h1>
                        <p class="text-xl text-slate-400 mb-8 italic">${t('heroSub')}</p>
                        <div class="flex justify-center gap-4">
                            <button class="bg-blue-600 hover:bg-blue-700 px-8 py-3 rounded-lg font-bold transition">${t('products')}</button>
                            <button class="border border-slate-700 hover:bg-white hover:text-slate-900 px-8 py-3 rounded-lg font-bold transition">Contact</button>
                        </div>
                    </div>
                </header>

                <!-- 产品网格 -->
                <main class="container mx-auto px-4 py-16">
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
                        ${[1, 2, 3, 4].map(i => `
                            <div class="bg-white border border-slate-100 rounded-2xl overflow-hidden hover:shadow-xl transition group">
                                <div class="h-48 bg-slate-100 flex items-center justify-center relative">
                                    <i data-lucide="box" size="40" class="text-slate-300"></i>
                                    <span class="absolute top-3 left-3 bg-white px-2 py-1 rounded text-[10px] font-bold shadow-sm">ISO 9001</span>
                                </div>
                                <div class="p-6">
                                    <h3 class="font-bold text-lg mb-2">Wire Rope Type ${i}</h3>
                                    <div class="flex justify-between items-center mb-4">
                                        <span class="text-xs text-slate-400 uppercase tracking-tighter">${t('price')}</span>
                                        <span class="font-bold text-blue-600">$850 - $1200</span>
                                    </div>
                                    <button onclick="addToCart()" class="w-full bg-slate-900 text-white py-2.5 rounded-lg font-bold hover:bg-blue-600 transition flex items-center justify-center gap-2">
                                        <i data-lucide="plus" size="16"></i> ${t('add')}
                                    </button>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                </main>

                <!-- 页脚 -->
                <footer class="bg-slate-50 border-t border-slate-200 py-12">
                    <div class="container mx-auto px-4 text-center">
                        <p class="text-slate-400 text-sm">© 2024 Titan Strand Global Export. All Rights Reserved.</p>
                    </div>
                </footer>
            `;
            
            lucide.createIcons();
        }

        // --- 4. 交互逻辑 ---
        window.toggleLang = () => {
            state.isLangOpen = !state.isLangOpen;
            render();
        };

        window.setLanguage = (code) => {
            state.currentLang = code;
            state.isLangOpen = false;
            render();
        };

        window.addToCart = () => {
            state.cartCount++;
            render();
        };

        // 点击外部关闭菜单
        window.onclick = (e) => {
            if (!e.target.closest('.relative')) {
                state.isLangOpen = false;
                render();
            }
        };

        // 首次加载
        render();
    </script>
</body>
</html>

