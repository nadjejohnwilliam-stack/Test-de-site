<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>L'Élégance | Boutique en Ligne Premium</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        serif: ['Playfair Display', 'serif'],
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        primary: '#1a1a1a',
                        secondary: '#f4f4f4',
                        accent: '#d4a574',
                    }
                }
            }
        }
    </script>
    <style>
        * {
            box-sizing: border-box;
        }
        
        html {
            scroll-behavior: smooth;
        }
        
        body {
            margin: 0;
            padding: 0;
            font-family: 'Inter', sans-serif;
            background-color: #fafafa;
            overflow-x: hidden;
        }
        
        .hero-gradient {
            background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
        }
        
        .glass-effect {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
        }
        
        .product-card {
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .product-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        
        .product-image {
            transition: transform 0.6s ease;
        }
        
        .product-card:hover .product-image {
            transform: scale(1.05);
        }
        
        .btn-primary {
            background: linear-gradient(135deg, #1a1a1a 0%, #333 100%);
            transition: all 0.3s ease;
        }
        
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(26,26,26,0.3);
        }
        
        .cart-sidebar {
            transform: translateX(100%);
            transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .cart-sidebar.open {
            transform: translateX(0);
        }
        
        .overlay {
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }
        
        .overlay.active {
            opacity: 1;
            visibility: visible;
        }
        
        .notification {
            transform: translateY(-100%);
            transition: transform 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        
        .notification.show {
            transform: translateY(0);
        }
        
        .category-pill {
            transition: all 0.3s ease;
        }
        
        .category-pill.active {
            background-color: #1a1a1a;
            color: white;
        }
        
        .scroll-reveal {
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s ease;
        }
        
        .scroll-reveal.visible {
            opacity: 1;
            transform: translateY(0);
        }
        
        .quantity-btn {
            transition: all 0.2s ease;
        }
        
        .quantity-btn:hover {
            background-color: #e5e5e5;
        }
        
        .remove-btn {
            opacity: 0;
            transition: opacity 0.2s ease;
        }
        
        .cart-item:hover .remove-btn {
            opacity: 1;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }
        
        .floating {
            animation: float 3s ease-in-out infinite;
        }
        
        .loader {
            border-top-color: #1a1a1a;
            -webkit-animation: spinner 1.5s linear infinite;
            animation: spinner 1.5s linear infinite;
        }
        
        @keyframes spinner {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .search-input {
            transition: all 0.3s ease;
        }
        
        .search-input:focus {
            width: 300px;
        }
        
        @media (max-width: 768px) {
            .search-input:focus {
                width: 200px;
            }
        }
    </style>
</head>
<body class="antialiased text-gray-800">

    <!-- Notification Toast -->
    <div id="notification" class="notification fixed top-6 left-1/2 transform -translate-x-1/2 z-50 bg-gray-900 text-white px-6 py-3 rounded-full shadow-2xl flex items-center gap-3">
        <i class="fas fa-check-circle text-green-400"></i>
        <span id="notification-text" class="font-medium">Article ajouté au panier</span>
    </div>

    <!-- Overlay -->
    <div id="overlay" class="overlay fixed inset-0 bg-black bg-opacity-50 z-40 backdrop-blur-sm" onclick="toggleCart()"></div>

    <!-- Cart Sidebar -->
    <div id="cart-sidebar" class="cart-sidebar fixed right-0 top-0 h-full w-full max-w-md bg-white z-50 shadow-2xl flex flex-col">
        <div class="p-6 border-b border-gray-100 flex justify-between items-center bg-gray-50">
            <h2 class="text-2xl font-serif font-bold text-gray-900">Votre Panier</h2>
            <button onclick="toggleCart()" class="w-10 h-10 rounded-full hover:bg-gray-200 flex items-center justify-center transition-colors">
                <i class="fas fa-times text-lg"></i>
            </button>
        </div>
        
        <div id="cart-items" class="flex-1 overflow-y-auto p-6 space-y-4">
            <!-- Cart items will be injected here -->
        </div>
        
        <div class="p-6 border-t border-gray-100 bg-gray-50">
            <div class="flex justify-between mb-4 text-lg">
                <span class="font-medium text-gray-600">Sous-total</span>
                <span id="cart-total" class="font-bold text-2xl font-serif">0,00 €</span>
            </div>
            <button onclick="checkout()" class="w-full btn-primary text-white py-4 rounded-lg font-semibold text-lg mb-3">
                Passer la commande
            </button>
            <button onclick="toggleCart()" class="w-full text-gray-500 hover:text-gray-800 font-medium py-2 transition-colors">
                Continuer les achats
            </button>
        </div>
    </div>

    <!-- Navigation -->
    <nav class="fixed w-full z-30 glass-effect border-b border-gray-100 transition-all duration-300" id="navbar">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-20">
                <!-- Logo -->
                <div class="flex-shrink-0 flex items-center cursor-pointer" onclick="window.scrollTo(0,0)">
                    <span class="font-serif text-3xl font-bold tracking-tight text-gray-900">L'Élégance</span>
                </div>

                <!-- Desktop Menu -->
                <div class="hidden md:flex space-x-8 items-center">
                    <a href="#products" class="text-gray-700 hover:text-gray-900 font-medium transition-colors">Boutique</a>
                    <a href="#collections" class="text-gray-700 hover:text-gray-900 font-medium transition-colors">Collections</a>
                    <a href="#about" class="text-gray-700 hover:text-gray-900 font-medium transition-colors">À propos</a>
                    <a href="#contact" class="text-gray-700 hover:text-gray-900 font-medium transition-colors">Contact</a>
                </div>

                <!-- Right Icons -->
                <div class="flex items-center space-x-6">
                    <div class="relative hidden sm:block">
                        <input type="text" id="search-input" placeholder="Rechercher..." 
                               class="search-input w-48 pl-10 pr-4 py-2 bg-gray-100 rounded-full text-sm focus:outline-none focus:ring-2 focus:ring-gray-200">
                        <i class="fas fa-search absolute left-3.5 top-2.5 text-gray-400"></i>
                    </div>
                    
                    <button onclick="toggleCart()" class="relative p-2 hover:bg-gray-100 rounded-full transition-colors">
                        <i class="fas fa-shopping-bag text-xl text-gray-800"></i>
                        <span id="cart-count" class="absolute -top-1 -right-1 bg-gray-900 text-white text-xs w-5 h-5 rounded-full flex items-center justify-center font-bold opacity-0 transition-opacity">0</span>
                    </button>
                    
                    <button class="md:hidden p-2 hover:bg-gray-100 rounded-full transition-colors" onclick="toggleMobileMenu()">
                        <i class="fas fa-bars text-xl text-gray-800"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Menu -->
        <div id="mobile-menu" class="hidden md:hidden bg-white border-t border-gray-100">
            <div class="px-4 pt-2 pb-6 space-y-2">
                <a href="#products" class="block px-3 py-2 text-base font-medium text-gray-700 hover:bg-gray-50 rounded-md">Boutique</a>
                <a href="#collections" class="block px-3 py-2 text-base font-medium text-gray-700 hover:bg-gray-50 rounded-md">Collections</a>
                <a href="#about" class="block px-3 py-2 text-base font-medium text-gray-700 hover:bg-gray-50 rounded-md">À propos</a>
                <a href="#contact" class="block px-3 py-2 text-base font-medium text-gray-700 hover:bg-gray-50 rounded-md">Contact</a>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <section class="relative pt-20 min-h-screen flex items-center hero-gradient overflow-hidden">
        <div class="absolute inset-0 opacity-20">
            <div class="absolute top-20 left-10 w-72 h-72 bg-white rounded-full mix-blend-overlay filter blur-3xl floating"></div>
            <div class="absolute bottom-20 right-10 w-96 h-96 bg-gray-400 rounded-full mix-blend-overlay filter blur-3xl floating" style="animation-delay: 1s;"></div>
        </div>
        
        <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-20 lg:py-32 grid lg:grid-cols-2 gap-12 items-center">
            <div class="text-white space-y-8 scroll-reveal">
                <span class="inline-block px-4 py-2 border border-white/30 rounded-full text-sm font-medium tracking-wider uppercase">Nouvelle Collection 2024</span>
                <h1 class="font-serif text-5xl lg:text-7xl font-bold leading-tight">
                    L'Art du <span class="text-accent italic">Style</span> Intemporel
                </h1>
                <p class="text-lg text-gray-300 max-w-lg leading-relaxed">
                    Découvrez notre sélection exclusive d'articles premium, conçus pour ceux qui apprécient l'élégance et la qualité exceptionnelle.
                </p>
                <div class="flex flex-wrap gap-4">
                    <a href="#products" class="btn-primary px-8 py-4 rounded-full text-white font-semibold inline-flex items-center gap-2">
                        Découvrir la collection
                        <i class="fas fa-arrow-right"></i>
                    </a>
                    <button onclick="showVideo()" class="px-8 py-4 rounded-full border-2 border-white/30 text-white font-semibold hover:bg-white/10 transition-all flex items-center gap-2">
                        <i class="fas fa-play"></i>
                        Voir la vidéo
                    </button>
                </div>
                
                <div class="flex items-center gap-8 pt-8 border-t border-white/10">
                    <div>
                        <div class="text-3xl font-bold font-serif">2k+</div>
                        <div class="text-sm text-gray-400">Clients satisfaits</div>
                    </div>
                    <div>
                        <div class="text-3xl font-bold font-serif">500+</div>
                        <div class="text-sm text-gray-400">Produits exclusifs</div>
                    </div>
                    <div>
                        <div class="text-3xl font-bold font-serif">4.9</div>
                        <div class="text-sm text-gray-400">Note moyenne</div>
                    </div>
                </div>
            </div>
            
            <div class="relative scroll-reveal hidden lg:block">
                <div class="relative z-10 rounded-2xl overflow-hidden shadow-2xl transform rotate-2 hover:rotate-0 transition-transform duration-500">
                    <img src="https://images.unsplash.com/photo-1441986300917-64674bd600d8?w=800&h=1000&fit=crop" 
                         alt="Collection Premium" 
                         class="w-full h-auto object-cover">
                </div>
                <div class="absolute -bottom-6 -left-6 bg-white p-6 rounded-xl shadow-xl z-20 max-w-xs">
                    <div class="flex items-center gap-4">
                        <div class="w-12 h-12 bg-accent/20 rounded-full flex items-center justify-center">
                            <i class="fas fa-award text-accent text-xl"></i>
                        </div>
                        <div>
                            <div class="font-bold text-gray-900">Qualité Premium</div>
                            <div class="text-sm text-gray-500">Garantie satisfait ou remboursé</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Features -->
    <section class="py-20 bg-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid md:grid-cols-3 gap-8">
                <div class="text-center p-8 rounded-2xl hover:bg-gray-50 transition-colors scroll-reveal">
                    <div class="w-16 h-16 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-6 text-2xl">
                        <i class="fas fa-shipping-fast text-gray-800"></i>
                    </div>
                    <h3 class="font-serif text-xl font-bold mb-3">Livraison Gratuite</h3>
                    <p class="text-gray-600">Livraison gratuite pour toute commande supérieure à 100€. Retours acceptés sous 30 jours.</p>
                </div>
                
                <div class="text-center p-8 rounded-2xl hover:bg-gray-50 transition-colors scroll-reveal" style="transition-delay: 0.1s;">
                    <div class="w-16 h-16 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-6 text-2xl">
                        <i class="fas fa-shield-alt text-gray-800"></i>
                    </div>
                    <h3 class="font-serif text-xl font-bold mb-3">Paiement Sécurisé</h3>
                    <p class="text-gray-600">Vos transactions sont 100% sécurisées. Nous acceptons toutes les cartes bancaires majeures.</p>
                </div>
                
                <div class="text-center p-8 rounded-2xl hover:bg-gray-50 transition-colors scroll-reveal" style="transition-delay: 0.2s;">
                    <div class="w-16 h-16 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-6 text-2xl">
                        <i class="fas fa-headset text-gray-800"></i>
                    </div>
                    <h3 class="font-serif text-xl font-bold mb-3">Support 24/7</h3>
                    <p class="text-gray-600">Notre équipe d'experts est disponible à tout moment pour répondre à vos questions.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Products Section -->
    <section id="products" class="py-20 bg-gray-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center mb-16 scroll-reveal">
                <span class="text-accent font-semibold tracking-wider uppercase text-sm">Notre Boutique</span>
                <h2 class="font-serif text-4xl md:text-5xl font-bold mt-3 mb-6">Articles en Vedette</h2>
                <p class="text-gray-600 max-w-2xl mx-auto text-lg">Explorez notre sélection soigneusement choisie d'articles premium pour sublimer votre quotidien.</p>
            </div>

            <!-- Categories -->
            <div class="flex flex-wrap justify-center gap-3 mb-12 scroll-reveal">
                <button onclick="filterCategory('all')" class="category-pill active px-6 py-2 rounded-full border border-gray-300 font-medium text-sm hover:border-gray-900" data-category="all">
                    Tous les articles
                </button>
                <button onclick="filterCategory('mode')" class="category-pill px-6 py-2 rounded-full border border-gray-300 font-medium text-sm hover:border-gray-900" data-category="mode">
                    Mode
                </button>
                <button onclick="filterCategory('accessoires')" class="category-pill px-6 py-2 rounded-full border border-gray-300 font-medium text-sm hover:border-gray-900" data-category="accessoires">
                    Accessoires
                </button>
                <button onclick="filterCategory('maison')" class="category-pill px-6 py-2 rounded-full border border-gray-300 font-medium text-sm hover:border-gray-900" data-category="maison">
                    Maison
                </button>
                <button onclick="filterCategory('lifestyle')" class="category-pill px-6 py-2 rounded-full border border-gray-300 font-medium text-sm hover:border-gray-900" data-category="lifestyle">
                    Lifestyle
                </button>
            </div>

            <!-- Products Grid -->
            <div id="products-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
                <!-- Products will be injected here by JavaScript -->
            </div>

            <div class="text-center mt-16 scroll-reveal">
                <button onclick="loadMoreProducts()" class="px-8 py-4 border-2 border-gray-900 text-gray-900 rounded-full font-semibold hover:bg-gray-900 hover:text-white transition-all inline-flex items-center gap-2">
                    Voir plus de produits
                    <i class="fas fa-chevron-down"></i>
                </button>
            </div>
        </div>
    </section>

    <!-- Collections Banner -->
    <section id="collections" class="py-20 bg-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid md:grid-cols-2 gap-8">
                <div class="relative overflow-hidden rounded-2xl group cursor-pointer scroll-reveal h-96">
                    <img src="https://images.unsplash.com/photo-1490481651871-ab68de25d43d?w=800&h=600&fit=crop" 
                         alt="Collection Femme" 
                         class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
                    <div class="absolute inset-0 bg-gradient-to-t from-black/70 to-transparent flex flex-col justify-end p-8">
                        <h3 class="font-serif text-3xl font-bold text-white mb-2">Collection Femme</h3>
                        <p class="text-gray-200 mb-4">Élégance et féminité</p>
                        <span class="text-white font-semibold inline-flex items-center gap-2 group-hover:gap-4 transition-all">
                            Découvrir <i class="fas fa-arrow-right"></i>
                        </span>
                    </div>
                </div>
                
                <div class="relative overflow-hidden rounded-2xl group cursor-pointer scroll-reveal h-96" style="transition-delay: 0.1s;">
                    <img src="https://images.unsplash.com/photo-1490367532201-b9bc1dc483f6?w=800&h=600&fit=crop" 
                         alt="Collection Homme" 
                         class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
                    <div class="absolute inset-0 bg-gradient-to-t from-black/70 to-transparent flex flex-col justify-end p-8">
                        <h3 class="font-serif text-3xl font-bold text-white mb-2">Collection Homme</h3>
                        <p class="text-gray-200 mb-4">Style et raffinement</p>
                        <span class="text-white font-semibold inline-flex items-center gap-2 group-hover:gap-4 transition-all">
                            Découvrir <i class="fas fa-arrow-right"></i>
                        </span>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- About Section -->
    <section id="about" class="py-20 bg-gray-900 text-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid lg:grid-cols-2 gap-16 items-center">
                <div class="scroll-reveal">
                    <img src="https://images.unsplash.com/photo-1441984904996-e0b6ba687e04?w=800&h=600&fit=crop" 
                         alt="Notre Histoire" 
                         class="rounded-2xl shadow-2xl">
                </div>
                <div class="scroll-reveal">
                    <span class="text-accent font-semibold tracking-wider uppercase text-sm">Notre Histoire</span>
                    <h2 class="font-serif text-4xl md:text-5xl font-bold mt-3 mb-6">Une Passion pour l'Excellence</h2>
                    <p class="text-gray-300 text-lg mb-6 leading-relaxed">
                        Fondée en 2020, L'Élégance est née d'une vision simple : offrir des produits d'exception qui allient esthétique, fonctionnalité et durabilité. Chaque article de notre collection est sélectionné avec le plus grand soin.
                    </p>
                    <p class="text-gray-300 text-lg mb-8 leading-relaxed">
                        Nous croyons en un commerce responsable, en travaillant directement avec des artisans et des fabricants partageant nos valeurs d'excellence et d'éthique.
                    </p>
                    
                    <div class="grid grid-cols-2 gap-6">
                        <div class="border-l-2 border-accent pl-4">
                            <div class="text-3xl font-bold font-serif mb-1">100%</div>
                            <div class="text-gray-400">Produits vérifiés</div>
                        </div>
                        <div class="border-l-2 border-accent pl-4">
                            <div class="text-3xl font-bold font-serif mb-1">50+</div>
                            <div class="text-gray-400">Marques partenaires</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Newsletter -->
    <section class="py-20 bg-accent/10">
        <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 text-center scroll-reveal">
            <h2 class="font-serif text-4xl font-bold mb-4">Rejoignez l'Aventure</h2>
            <p class="text-gray-600 mb-8 text-lg">Inscrivez-vous à notre newsletter pour recevoir nos offres exclusives et les dernières nouveautés.</p>
            
            <form onsubmit="subscribeNewsletter(event)" class="flex flex-col sm:flex-row gap-4 max-w-lg mx-auto">
                <input type="email" placeholder="Votre adresse email" required
                       class="flex-1 px-6 py-4 rounded-full border border-gray-300 focus:outline-none focus:border-gray-900 transition-colors">
                <button type="submit" class="btn-primary px-8 py-4 rounded-full text-white font-semibold whitespace-nowrap">
                    S'inscrire
                </button>
            </form>
            
            <p class="text-sm text-gray-500 mt-4">En vous inscrivant, vous acceptez notre politique de confidentialité.</p>
        </div>
    </section>

    <!-- Footer -->
    <footer id="contact" class="bg-gray-900 text-white pt-20 pb-10">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12 mb-16">
                <div>
                    <span class="font-serif text-3xl font-bold tracking-tight">L'Élégance</span>
                    <p class="mt-4 text-gray-400 leading-relaxed">
                        Votre destination pour des articles premium sélectionnés avec soin. Qualité, élégance et service exceptionnel.
                    </p>
                    <div class="flex gap-4 mt-6">
                        <a href="#" class="w-10 h-10 rounded-full bg-gray-800 flex items-center justify-center hover:bg-accent transition-colors">
                            <i class="fab fa-instagram"></i>
                        </a>
                        <a href="#" class="w-10 h-10 rounded-full bg-gray-800 flex items-center justify-center hover:bg-accent transition-colors">
                            <i class="fab fa-facebook-f"></i>
                        </a>
                        <a href="#" class="w-10 h-10 rounded-full bg-gray-800 flex items-center justify-center hover:bg-accent transition-colors">
                            <i class="fab fa-twitter"></i>
                        </a>
                    </div>
                </div>
                
                <div>
                    <h4 class="font-serif text-lg font-bold mb-6">Boutique</h4>
                    <ul class="space-y-3 text-gray-400">
                        <li><a href="#" class="hover:text-white transition-colors">Nouveautés</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Meilleures ventes</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Promotions</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Collection limitée</a></li>
                    </ul>
                </div>
                
                <div>
                    <h4 class="font-serif text-lg font-bold mb-6">Aide</h4>
                    <ul class="space-y-3 text-gray-400">
                        <li><a href="#" class="hover:text-white transition-colors">FAQ</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Livraison & Retours</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Guide des tailles</a></li>
                        <li><a href="#" class="hover:text-white transition-colors">Contactez-nous</a></li>
                    </ul>
                </div>
                
                <div>
                    <h4 class="font-serif text-lg font-bold mb-6">Contact</h4>
                    <ul class="space-y-3 text-gray-400">
                        <li class="flex items-start gap-3">
                            <i class="fas fa-map-marker-alt mt-1"></i>
                            <span>123 Avenue des Champs-Élysées<br>75008 Paris, France</span>
                        </li>
                        <li class="flex items-center gap-3">
                            <i class="fas fa-phone"></i>
                            <span>+33 1 23 45 67 89</span>
                        </li>
                        <li class="flex items-center gap-3">
                            <i class="fas fa-envelope"></i>
                            <span>contact@lelegance.fr</span>
                        </li>
                    </ul>
                </div>
            </div>
            
            <div class="border-t border-gray-800 pt-8 flex flex-col md:flex-row justify-between items-center gap-4">
                <p class="text-gray-500 text-sm">© 2024 L'Élégance. Tous droits réservés.</p>
                <div class="flex gap-6 text-sm text-gray-500">
                    <a href="#" class="hover:text-white transition-colors">Conditions générales</a>
                    <a href="#" class="hover:text-white transition-colors">Politique de confidentialité</a>
                    <a href="#" class="hover:text-white transition-colors">Mentions légales</a>
                </div>
            </div>
        </div>
    </footer>

    <script>
        // Product Data
        const products = [
            {
                id: 1,
                name: "Sac en Cuir Premium",
                price: 189.00,
                category: "accessoires",
                image: "https://images.unsplash.com/photo-1548036328-c9fa89d128fa?w=600&h=750&fit=crop",
                badge: "Nouveau"
            },
            {
                id: 2,
                name: "Montre Minimaliste",
                price: 249.00,
                category: "accessoires",
                image: "https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=600&h=750&fit=crop",
                badge: "Best-seller"
            },
            {
                id: 3,
                name: "Baskets Édition Limitée",
                price: 159.00,
                category: "mode",
                image: "https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=600&h=750&fit=crop",
                badge: null
            },
            {
                id: 4,
                name: "Lunettes de Soleil Aviateur",
                price: 129.00,
                category: "accessoires",
                image: "https://images.unsplash.com/photo-1572635196237-14b3f281503f?w=600&h=750&fit=crop",
                badge: null
            },
            {
                id: 5,
                name: "Vase Céramique Artisanal",
                price: 89.00,
                category: "maison",
                image: "https://images.unsplash.com/photo-1578500494198-246f612d3b3d?w=600&h=750&fit=crop",
                badge: "Nouveau"
            },
            {
                id: 6,
                name: "Carnet Cuir Véritable",
                price: 45.00,
                category: "lifestyle",
                image: "https://images.unsplash.com/photo-1544816155-12df9643f363?w=600&h=750&fit=crop",
                badge: null
            },
            {
                id: 7,
                name: "Casque Audio Premium",
                price: 299.00,
                category: "lifestyle",
                image: "https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=600&h=750&fit=crop",
                badge: "Promo"
            },
            {
                id: 8,
                name: "Parfum Signature",
                price: 79.00,
                category: "mode",
                image: "https://images.unsplash.com/photo-1541643600914-78b084683601?w=600&h=750&fit=crop",
                badge: null
            }
        ];

        let cart = [];
        let currentCategory = 'all';

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            renderProducts();
            setupScrollReveal();
            setupNavbar();
        });

        // Render Products
        function renderProducts() {
            const grid = document.getElementById('products-grid');
            const filtered = currentCategory === 'all' 
                ? products 
                : products.filter(p => p.category === currentCategory);
            
            grid.innerHTML = filtered.map(product => `
                <div class="product-card bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-xl scroll-reveal">
                    <div class="relative overflow-hidden aspect-[4/5] bg-gray-100">
                        <img src="${product.image}" alt="${product.name}" class="product-image w-full h-full object-cover">
                        ${product.badge ? `
                            <span class="absolute top-4 left-4 bg-gray-900 text-white text-xs font-bold px-3 py-1 rounded-full">
                                ${product.badge}
                            </span>
                        ` : ''}
                        <button onclick="addToCart(${product.id})" 
                                class="absolute bottom-4 right-4 w-12 h-12 bg-white rounded-full shadow-lg flex items-center justify-center text-gray-900 hover:bg-gray-900 hover:text-white transition-all transform hover:scale-110">
                            <i class="fas fa-plus"></i>
                        </button>
                    </div>
                    <div class="p-6">
                        <div class="text-xs text-gray-500 uppercase tracking-wider mb-2">${product.category}</div>
                        <h3 class="font-serif text-lg font-bold text-gray-900 mb-2">${product.name}</h3>
                        <div class="flex items-center justify-between">
                            <span class="text-xl font-bold text-gray-900">${product.price.toFixed(2)} €</span>
                            <div class="flex text-yellow-400 text-sm">
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star-half-alt"></i>
                            </div>
                        </div>
                    </div>
                </div>
            `).join('');
            
            // Re-trigger scroll reveal for new elements
            setTimeout(setupScrollReveal, 100);
        }

        // Filter Category
        function filterCategory(category) {
            currentCategory = category;
            
            // Update buttons
            document.querySelectorAll('.category-pill').forEach(btn => {
                if(btn.dataset.category === category) {
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });
            
            // Animate grid
            const grid = document.getElementById('products-grid');
            grid.style.opacity = '0';
            grid.style.transform = 'translateY(20px)';
            
            setTimeout(() => {
                renderProducts();
                grid.style.transition = 'all 0.5s ease';
                grid.style.opacity = '1';
                grid.style.transform = 'translateY(0)';
            }, 200);
        }

        // Cart Functions
        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            const existingItem = cart.find(item => item.id === productId);
            
            if(existingItem) {
                existingItem.quantity++;
            } else {
                cart.push({...product, quantity: 1});
            }
            
            updateCart();
            showNotification(`${product.name} ajouté au panier`);
            
            // Animate cart icon
            const cartBtn = document.querySelector('.fa-shopping-bag').parentElement;
            cartBtn.classList.add('scale-125');
            setTimeout(() => cartBtn.classList.remove('scale-125'), 200);
        }

        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCart();
        }

        function updateQuantity(productId, change) {
            const item = cart.find(item => item.id === productId);
            if(item) {
                item.quantity += change;
                if(item.quantity <= 0) {
                    removeFromCart(productId);
                } else {
                    updateCart();
                }
            }
        }

        function updateCart() {
            const cartItems = document.getElementById('cart-items');
            const cartCount = document.getElementById('cart-count');
            const cartTotal = document.getElementById('cart-total');
            
            // Update count
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            cartCount.textContent = totalItems;
            cartCount.style.opacity = totalItems > 0 ? '1' : '0';
            
            // Update items
            if(cart.length === 0) {
                cartItems.innerHTML = `
                    <div class="flex flex-col items-center justify-center h-full text-gray-400">
                        <i class="fas fa-shopping-bag text-6xl mb-4 opacity-20"></i>
                        <p class="text-lg">Votre panier est vide</p>
                        <button onclick="toggleCart()" class="mt-4 text-gray-900 font-medium hover:underline">Continuer les achats</button>
                    </div>
                `;
            } else {
                cartItems.innerHTML = cart.map(item => `
                    <div class="cart-item flex gap-4 p-4 bg-gray-50 rounded-xl group">
                        <img src="${item.image}" alt="${item.name}" class="w-20 h-24 object-cover rounded-lg">
                        <div class="flex-1 flex flex-col justify-between">
                            <div>
                                <h4 class="font-serif font-bold text-gray-900 line-clamp-1">${item.name}</h4>
                                <p class="text-sm text-gray-500">${item.category}</p>
                            </div>
                            <div class="flex items-center justify-between">
                                <div class="flex items-center gap-3 bg-white rounded-lg p-1">
                                    <button onclick="updateQuantity(${item.id}, -1)" class="quantity-btn w-8 h-8 rounded flex items-center justify-center text-gray-600">
                                        <i class="fas fa-minus text-xs"></i>
                                    </button>
                                    <span class="font-medium w-4 text-center">${item.quantity}</span>
                                    <button onclick="updateQuantity(${item.id}, 1)" class="quantity-btn w-8 h-8 rounded flex items-center justify-center text-gray-600">
                                        <i class="fas fa-plus text-xs"></i>
                                    </button>
                                </div>
                                <span class="font-bold">${(item.price * item.quantity).toFixed(2)} €</span>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${item.id})" class="remove-btn text-gray-400 hover:text-red-500 transition-colors self-start">
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    </div>
                `).join('');
            }
            
            // Update total
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            cartTotal.textContent = total.toFixed(2) + ' €';
        }

        function toggleCart() {
            const sidebar = document.getElementById('cart-sidebar');
            const overlay = document.getElementById('overlay');
            
            sidebar.classList.toggle('open');
            overlay.classList.toggle('active');
            document.body.style.overflow = sidebar.classList.contains('open') ? 'hidden' : '';
        }

        function checkout() {
            if(cart.length === 0) {
                showNotification('Votre panier est vide', 'error');
                return;
            }
            
            // Simulate checkout
            const btn = event.target;
            const originalText = btn.innerHTML;
            btn.innerHTML = '<div class="loader w-6 h-6 border-2 border-white rounded-full mx-auto"></div>';
            btn.disabled = true;
            
            setTimeout(() => {
                cart = [];
                updateCart();
                toggleCart();
                showNotification('Commande validée ! Merci pour votre achat.');
                btn.innerHTML = originalText;
                btn.disabled = false;
            }, 2000);
        }

        // UI Functions
        function showNotification(message, type = 'success') {
            const notification = document.getElementById('notification');
            const text = document.getElementById('notification-text');
            const icon = notification.querySelector('i');
            
            text.textContent = message;
            
            if(type === 'error') {
                icon.className = 'fas fa-exclamation-circle text-red-400';
            } else {
                icon.className = 'fas fa-check-circle text-green-400';
            }
            
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        function toggleMobileMenu() {
            const menu = document.getElementById('mobile-menu');
            menu.classList.toggle('hidden');
        }

        function subscribeNewsletter(e) {
            e.preventDefault();
            const input = e.target.querySelector('input');
            if(input.value) {
                showNotification('Inscription réussie ! Bienvenue dans la communauté.');
                input.value = '';
            }
        }

        function showVideo() {
            showNotification('Vidéo de présentation à venir prochainement !');
        }

        function loadMoreProducts() {
            showNotification('Plus de produits seront bientôt disponibles !');
        }

        // Scroll Reveal Animation
        function setupScrollReveal() {
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if(entry.isIntersecting) {
                        entry.target.classList.add('visible');
                    }
                });
            }, {
                threshold: 0.1,
                rootMargin: '0px 0px -50px 0px'
            });

            document.querySelectorAll('.scroll-reveal').forEach(el => {
                observer.observe(el);
            });
        }

        // Navbar Scroll Effect
        function setupNavbar() {
            const navbar = document.getElementById('navbar');
            let lastScroll = 0;

            window.addEventListener('scroll', () => {
                const currentScroll = window.pageYOffset;
                
                if(currentScroll > 100) {
                    navbar.classList.add('shadow-md');
                } else {
                    navbar.classList.remove('shadow-md');
                }
                
                lastScroll = currentScroll;
            });
        }

        // Search functionality
        document.getElementById('search-input')?.addEventListener('input', (e) => {
            const term = e.target.value.toLowerCase();
            if(term.length > 0) {
                const filtered = products.filter(p => 
                    p.name.toLowerCase().includes(term) || 
                    p.category.toLowerCase().includes(term)
                );
                
                const grid = document.getElementById('products-grid');
                if(filtered.length === 0) {
                    grid.innerHTML = `
                        <div class="col-span-full text-center py-12 text-gray-500">
                            <i class="fas fa-search text-4xl mb-4 opacity-30"></i>
                            <p>Aucun produit trouvé pour "${term}"</p>
                        </div>
                    `;
                } else {
                    // Temporary render for search results
                    grid.innerHTML = filtered.map(product => `
                        <div class="product-card bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-xl">
                            <div class="relative overflow-hidden aspect-[4/5] bg-gray-100">
                                <img src="${product.image}" alt="${product.name}" class="product-image w-full h-full object-cover">
                                <button onclick="addToCart(${product.id})" 
                                        class="absolute bottom-4 right-4 w-12 h-12 bg-white rounded-full shadow-lg flex items-center justify-center text-gray-900 hover:bg-gray-900 hover:text-white transition-all">
                                    <i class="fas fa-plus"></i>
                                </button>
                            </div>
                            <div class="p-6">
                                <div class="text-xs text-gray-500 uppercase tracking-wider mb-2">${product.category}</div>
                                <h3 class="font-serif text-lg font-bold text-gray-900 mb-2">${product.name}</h3>
                                <div class="flex items-center justify-between">
                                    <span class="text-xl font-bold text-gray-900">${product.price.toFixed(2)} €</span>
                                </div>
                            </div>
                        </div>
                    `).join('');
                }
            } else {
                renderProducts();
            }
        });
    </script>
</body>
</html>
