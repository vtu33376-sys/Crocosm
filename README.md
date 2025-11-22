<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crocosm | Premium Shopping App</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <!-- Payment Gateway SDK (Razorpay) -->
    <script src="https://checkout.razorpay.com/v1/checkout.js"></script>
    
    <style>
        :root {
            --glass-bg: rgba(20, 20, 25, 0.7);
            --glass-border: rgba(255, 255, 255, 0.08);
            --primary: #00C9A7; /* Crocosm Teal */
            --secondary: #845EC2; /* Purple */
            --bg-dark: #0f1014;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-dark);
            color: #ffffff;
            padding-bottom: 90px; /* Space for bottom nav */
            -webkit-tap-highlight-color: transparent;
            overflow-x: hidden;
        }

        /* Loading Animation */
        .loader {
            border: 3px solid #f3f3f3;
            border-radius: 50%;
            border-top: 3px solid var(--primary);
            width: 24px;
            height: 24px;
            -webkit-animation: spin 1s linear infinite; /* Safari */
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Glassmorphism Utilities */
        .glass {
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid var(--glass-border);
        }

        .glass-panel {
            background: rgba(30, 30, 35, 0.6);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.05);
            border-radius: 16px;
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        }

        /* Navigation Transitions */
        .page-section {
            display: none;
            opacity: 0;
            transform: translateY(10px);
            transition: opacity 0.3s ease, transform 0.3s ease;
        }
        
        .page-section.active {
            display: block;
            opacity: 1;
            transform: translateY(0);
        }

        /* Utilities */
        .hide-scrollbar::-webkit-scrollbar { display: none; }
        .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        .text-gradient {
            background: linear-gradient(to right, #00C9A7, #845EC2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* Custom Radio for Payment */
        .payment-radio:checked + div {
            border-color: var(--primary);
            background: rgba(0, 201, 167, 0.1);
        }
    </style>
</head>
<body class="text-gray-100 selection:bg-teal-500 selection:text-black">

    <!-- Background Ambient Light -->
    <div class="fixed top-0 left-0 w-full h-full overflow-hidden -z-10 pointer-events-none">
        <div class="absolute top-[-10%] left-[-10%] w-96 h-96 bg-purple-600/20 rounded-full blur-[100px]"></div>
        <div class="absolute bottom-[-10%] right-[-10%] w-80 h-80 bg-teal-600/20 rounded-full blur-[100px]"></div>
    </div>

    <!-- Top Header (Sticky) -->
    <header class="fixed top-0 w-full z-50 glass border-b-0 px-4 py-3 flex items-center justify-between transition-all duration-300" id="main-header">
        <div class="flex items-center gap-2" onclick="router('home')">
            <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-teal-400 to-teal-600 flex items-center justify-center shadow-lg shadow-teal-500/20">
                <i class="fa-solid fa-layer-group text-black text-sm"></i>
            </div>
            <h1 class="text-lg font-extrabold tracking-tight">CROCOSM</h1>
        </div>
        <div class="flex items-center gap-5">
            <div class="relative" onclick="router('cart')">
                <i class="fa-solid fa-bag-shopping text-xl text-gray-200 hover:text-white transition"></i>
                <span id="header-cart-count" class="absolute -top-1.5 -right-1.5 bg-red-500 text-white text-[10px] font-bold px-1.5 py-0.5 rounded-full hidden border border-[#0f1014]">0</span>
            </div>
        </div>
    </header>

    <!-- MAIN CONTENT AREA -->
    <main class="pt-20 px-4 max-w-md mx-auto min-h-screen pb-10">

        <!-- HOME PAGE -->
        <section id="home" class="page-section active">
            <!-- Search Bar -->
            <div class="glass-panel p-3 flex items-center gap-3 mb-6 sticky top-16 z-40 shadow-lg">
                <i class="fa-solid fa-search text-gray-400 ml-1"></i>
                <input type="text" placeholder="Search Crocosm..." class="bg-transparent w-full focus:outline-none text-sm py-1 text-white placeholder-gray-500">
            </div>

            <!-- Categories -->
            <div class="flex gap-4 overflow-x-auto hide-scrollbar mb-8 pb-2">
                <div class="flex-shrink-0 flex flex-col items-center gap-2 opacity-80 hover:opacity-100 transition">
                    <div class="w-14 h-14 rounded-full bg-[#1a1b21] border border-white/10 flex items-center justify-center">
                        <i class="fa-solid fa-mobile-screen text-teal-400"></i>
                    </div>
                    <span class="text-[10px] uppercase font-bold tracking-wide text-gray-400">Mobiles</span>
                </div>
                <div class="flex-shrink-0 flex flex-col items-center gap-2 opacity-80 hover:opacity-100 transition">
                    <div class="w-14 h-14 rounded-full bg-[#1a1b21] border border-white/10 flex items-center justify-center">
                        <i class="fa-solid fa-shoe-prints text-purple-400"></i>
                    </div>
                    <span class="text-[10px] uppercase font-bold tracking-wide text-gray-400">Fashion</span>
                </div>
                <div class="flex-shrink-0 flex flex-col items-center gap-2 opacity-80 hover:opacity-100 transition">
                    <div class="w-14 h-14 rounded-full bg-[#1a1b21] border border-white/10 flex items-center justify-center">
                        <i class="fa-solid fa-headphones text-pink-400"></i>
                    </div>
                    <span class="text-[10px] uppercase font-bold tracking-wide text-gray-400">Audio</span>
                </div>
                <div class="flex-shrink-0 flex flex-col items-center gap-2 opacity-80 hover:opacity-100 transition">
                    <div class="w-14 h-14 rounded-full bg-[#1a1b21] border border-white/10 flex items-center justify-center">
                        <i class="fa-solid fa-laptop text-blue-400"></i>
                    </div>
                    <span class="text-[10px] uppercase font-bold tracking-wide text-gray-400">Tech</span>
                </div>
            </div>

            <!-- Banner -->
            <div class="glass-panel p-6 mb-8 relative overflow-hidden min-h-[180px] flex flex-col justify-center rounded-2xl border-teal-500/20">
                <div class="relative z-10 max-w-[60%]">
                    <span class="text-xs font-bold text-teal-400 bg-teal-500/10 px-2 py-1 rounded mb-2 inline-block">NEW ARRIVAL</span>
                    <h2 class="text-2xl font-bold mb-2 leading-tight">Future Tech <br> <span class="text-white">Is Here</span></h2>
                    <button class="mt-2 text-xs font-bold border-b border-teal-400 pb-0.5">Shop Collection</button>
                </div>
                <img src="https://images.unsplash.com/photo-1550009158-9ebf69173e03?auto=format&fit=crop&w=400&q=80" class="absolute -right-10 top-0 h-full w-3/4 object-cover opacity-60" style="mask-image: linear-gradient(to left, black, transparent);">
            </div>

            <div class="flex justify-between items-end mb-4">
                <h3 class="text-lg font-bold">Trending Now</h3>
                <span class="text-xs text-teal-400">View All</span>
            </div>
            
            <div id="product-grid" class="grid grid-cols-2 gap-3 pb-20">
                <!-- Products injected via JS -->
            </div>
        </section>

        <!-- PRODUCT DETAILS PAGE -->
        <section id="product-details" class="page-section">
            <div class="relative">
                <button onclick="router('home')" class="absolute top-4 left-4 z-20 p-2 glass rounded-full hover:bg-white/10"><i class="fa-solid fa-arrow-left"></i></button>
                <div class="h-96 w-full rounded-b-3xl overflow-hidden mb-6 bg-[#1a1b21] relative">
                    <img id="detail-img" src="" class="w-full h-full object-cover opacity-90">
                    <div class="absolute bottom-0 left-0 w-full h-32 bg-gradient-to-t from-[#0f1014] to-transparent"></div>
                </div>
            </div>
            
            <div class="px-2">
                <div class="flex justify-between items-start mb-2">
                    <h1 id="detail-name" class="text-2xl font-bold w-3/4"></h1>
                    <div id="detail-price" class="text-2xl font-bold text-teal-400"></div>
                </div>
                
                <div class="flex items-center gap-4 mb-6 text-sm text-gray-400">
                    <span class="bg-white/5 px-2 py-1 rounded flex items-center gap-1"><i class="fa-solid fa-star text-yellow-400 text-xs"></i> <span id="detail-rating"></span></span>
                    <span>Free Delivery</span>
                    <span>1 Year Warranty</span>
                </div>

                <h3 class="font-bold mb-2 text-gray-200">Description</h3>
                <p id="detail-desc" class="text-gray-400 text-sm leading-relaxed mb-8"></p>
            </div>

            <div class="fixed bottom-0 left-0 w-full glass border-t border-white/10 p-4 z-50 backdrop-blur-xl">
                <div class="max-w-md mx-auto flex gap-3">
                    <button onclick="addToCart(currentProduct.id)" class="flex-1 py-3.5 rounded-xl font-bold border border-white/10 bg-white/5 hover:bg-white/10 transition">Add to Cart</button>
                    <button onclick="addToCart(currentProduct.id); router('cart')" class="flex-1 py-3.5 rounded-xl font-bold bg-white text-black shadow-lg shadow-white/10">Buy Now</button>
                </div>
            </div>
            <div class="h-20"></div> 
        </section>

        <!-- CART PAGE -->
        <section id="cart" class="page-section">
            <h2 class="text-2xl font-bold mb-6">My Cart</h2>
            <div id="cart-items-container" class="flex flex-col gap-4 mb-32">
                <!-- Cart Items injected -->
            </div>
            
            <!-- Cart Total Footer -->
            <div id="cart-footer" class="fixed bottom-[70px] left-0 w-full glass border-t border-white/10 p-4 hidden z-40">
                <div class="max-w-md mx-auto flex justify-between items-center">
                    <div>
                        <p class="text-xs text-gray-400 mb-0.5">Total Amount</p>
                        <p class="text-xl font-bold text-white" id="cart-total-price">₹0</p>
                    </div>
                    <button onclick="initCheckout()" class="bg-teal-500 text-black px-8 py-3 rounded-xl font-bold shadow-lg shadow-teal-500/20 hover:scale-105 transition-transform">Checkout</button>
                </div>
            </div>
        </section>

        <!-- CHECKOUT: ADDRESS -->
        <section id="address" class="page-section">
            <div class="flex items-center gap-3 mb-6">
                <button onclick="router('cart')" class="p-2 glass rounded-full hover:bg-white/10"><i class="fa-solid fa-arrow-left"></i></button>
                <h2 class="text-xl font-bold">Shipping Details</h2>
            </div>

            <form id="address-form" onsubmit="saveAddress(event)" class="flex flex-col gap-4">
                <div class="glass-panel p-0 overflow-hidden">
                    <div class="p-4 border-b border-white/5">
                        <label class="text-[10px] uppercase font-bold text-gray-500 tracking-wider">Contact Info</label>
                        <input type="text" id="addr-name" placeholder="Full Name" required class="w-full bg-transparent border-b border-white/10 py-2 mt-1 focus:border-teal-500 outline-none transition text-sm">
                        <input type="tel" id="addr-phone" placeholder="Mobile Number" required pattern="[0-9]{10}" class="w-full bg-transparent border-none py-2 mt-2 focus:ring-0 outline-none text-sm">
                    </div>
                    <div class="p-4 bg-white/5">
                         <label class="text-[10px] uppercase font-bold text-gray-500 tracking-wider">Address</label>
                         <input type="text" id="addr-pin" placeholder="Pincode" required class="w-full bg-transparent border-b border-white/10 py-2 mt-1 focus:border-teal-500 outline-none transition text-sm">
                         <input type="text" id="addr-city" placeholder="City, State" required class="w-full bg-transparent border-b border-white/10 py-2 mt-2 focus:border-teal-500 outline-none transition text-sm">
                         <textarea id="addr-text" placeholder="House No, Building, Area" required rows="2" class="w-full bg-transparent border-none py-2 mt-2 focus:ring-0 outline-none resize-none text-sm"></textarea>
                    </div>
                </div>
                
                <button type="submit" class="w-full bg-white text-black py-4 rounded-xl font-bold mt-4 shadow-lg">Proceed to Payment</button>
            </form>
        </section>

        <!-- CHECKOUT: PAYMENT -->
        <section id="payment" class="page-section">
            <div class="flex items-center gap-3 mb-6">
                <button onclick="router('address')" class="p-2 glass rounded-full hover:bg-white/10"><i class="fa-solid fa-arrow-left"></i></button>
                <h2 class="text-xl font-bold">Secure Payment</h2>
            </div>

            <!-- Order Summary Card -->
            <div class="glass-panel p-6 mb-6 relative overflow-hidden">
                <div class="absolute top-0 right-0 p-4 opacity-10">
                    <i class="fa-solid fa-receipt text-6xl"></i>
                </div>
                <div class="text-gray-400 text-xs uppercase tracking-wider mb-1">Total Payable</div>
                <div class="text-3xl font-bold text-white mb-4" id="payment-amount">₹0</div>
                <div class="h-px w-full bg-white/10 mb-3"></div>
                <div class="flex items-start gap-2 text-sm text-gray-400">
                    <i class="fa-solid fa-location-dot mt-1 text-teal-400"></i>
                    <span id="payment-address-preview" class="line-clamp-2"></span>
                </div>
            </div>

            <div class="space-y-4">
                <h3 class="text-sm font-bold text-gray-400 ml-1">Select Method</h3>
                
                <!-- Online Payment (Razorpay/Gateway) -->
                <label class="cursor-pointer relative">
                    <input type="radio" name="payment" value="online" checked class="peer sr-only payment-radio" onclick="selectPayment('online')">
                    <div class="glass-panel p-4 border border-transparent peer-checked:border-teal-500 transition-all flex items-center justify-between group">
                        <div class="flex items-center gap-4">
                            <div class="w-10 h-10 rounded-full bg-white flex items-center justify-center">
                                <i class="fa-solid fa-bolt text-yellow-500 text-lg"></i>
                            </div>
                            <div>
                                <div class="font-bold text-white">Pay Online</div>
                                <div class="text-xs text-gray-400">UPI, Cards, Netbanking</div>
                            </div>
                        </div>
                        <div class="w-5 h-5 rounded-full border border-gray-500 peer-checked:bg-teal-500 peer-checked:border-teal-500 flex items-center justify-center">
                            <div class="w-2 h-2 bg-black rounded-full hidden peer-checked:block"></div>
                        </div>
                    </div>
                    <!-- Trust Badges -->
                    <div class="flex gap-2 mt-2 ml-16 opacity-50 grayscale transition-all peer-checked:grayscale-0 peer-checked:opacity-80">
                        <i class="fa-brands fa-google-pay text-xl"></i>
                        <i class="fa-brands fa-cc-visa text-xl"></i>
                        <i class="fa-brands fa-cc-mastercard text-xl"></i>
                    </div>
                </label>

                <!-- COD Option -->
                <label class="cursor-pointer relative block mt-4">
                    <input type="radio" name="payment" value="cod" class="peer sr-only payment-radio" onclick="selectPayment('cod')">
                    <div class="glass-panel p-4 border border-transparent peer-checked:border-teal-500 transition-all flex items-center justify-between">
                        <div class="flex items-center gap-4">
                            <div class="w-10 h-10 rounded-full bg-[#2a2a30] flex items-center justify-center">
                                <i class="fa-solid fa-money-bill-wave text-green-400"></i>
                            </div>
                            <div>
                                <div class="font-bold text-white">Cash on Delivery</div>
                                <div class="text-xs text-gray-400">Pay cash at your doorstep</div>
                            </div>
                        </div>
                        <div class="w-5 h-5 rounded-full border border-gray-500 peer-checked:bg-teal-500 peer-checked:border-teal-500 flex items-center justify-center">
                            <div class="w-2 h-2 bg-black rounded-full hidden peer-checked:block"></div>
                        </div>
                    </div>
                </label>
            </div>

            <!-- Action Button -->
            <div class="fixed bottom-0 left-0 w-full p-4 bg-[#0f1014] border-t border-white/10 z-50">
                <div class="max-w-md mx-auto">
                    <button id="pay-btn" onclick="processPayment()" class="w-full bg-teal-500 text-black py-4 rounded-xl font-bold shadow-lg shadow-teal-500/20 flex items-center justify-center gap-2">
                        <span>Pay Securely</span> <i class="fa-solid fa-lock text-xs"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- SUCCESS PAGE -->
        <section id="success" class="page-section text-center pt-20">
            <div class="relative inline-block mb-8">
                <div class="absolute inset-0 bg-green-500 blur-xl opacity-40 animate-pulse"></div>
                <div class="w-24 h-24 bg-green-500 rounded-full flex items-center justify-center relative shadow-2xl shadow-green-500/30">
                    <i class="fa-solid fa-check text-4xl text-white"></i>
                </div>
            </div>
            
            <h2 class="text-3xl font-bold mb-2 text-white">Payment Successful!</h2>
            <p class="text-gray-400 mb-8">Your order has been confirmed.</p>
            
            <div class="glass-panel p-6 max-w-xs mx-auto text-left mb-8 border-t-4 border-green-500">
                <div class="flex justify-between mb-3 border-b border-white/5 pb-2">
                    <span class="text-gray-400 text-xs">Order ID</span>
                    <span class="font-mono font-bold text-sm" id="success-order-id">#0000</span>
                </div>
                <div class="flex justify-between mb-3">
                    <span class="text-gray-400 text-xs">Amount Paid</span>
                    <span class="font-bold text-teal-400" id="success-amount">₹0</span>
                </div>
                <div class="flex justify-between items-center">
                    <span class="text-gray-400 text-xs">Status</span>
                    <span class="text-xs bg-green-500/20 text-green-400 px-2 py-1 rounded font-bold">VERIFIED</span>
                </div>
            </div>

            <button onclick="router('home')" class="bg-white text-black px-8 py-3 rounded-full font-bold hover:bg-gray-200 transition">Continue Shopping</button>
        </section>

        <!-- ACCOUNT PAGE -->
        <section id="account" class="page-section">
            <div class="flex items-center gap-4 mb-8 p-4">
                <div class="w-16 h-16 rounded-full bg-gradient-to-tr from-teal-400 to-blue-500 flex items-center justify-center text-2xl font-bold text-white shadow-lg">
                    U
                </div>
                <div>
                    <h2 class="text-xl font-bold">User Profile</h2>
                    <p class="text-sm text-gray-400">+91 98XXX XXXXX</p>
                </div>
            </div>

            <div class="space-y-3">
                <div class="glass-panel p-4 flex justify-between items-center active:scale-[0.98] transition-transform">
                    <div class="flex items-center gap-4">
                        <div class="w-8 h-8 rounded bg-blue-500/20 flex items-center justify-center text-blue-400"><i class="fa-solid fa-box"></i></div>
                        <span>My Orders</span>
                    </div>
                    <i class="fa-solid fa-chevron-right text-gray-600 text-sm"></i>
                </div>
                <div class="glass-panel p-4 flex justify-between items-center active:scale-[0.98] transition-transform">
                    <div class="flex items-center gap-4">
                        <div class="w-8 h-8 rounded bg-pink-500/20 flex items-center justify-center text-pink-400"><i class="fa-solid fa-location-dot"></i></div>
                        <span>Addresses</span>
                    </div>
                    <i class="fa-solid fa-chevron-right text-gray-600 text-sm"></i>
                </div>
                <div class="glass-panel p-4 flex justify-between items-center active:scale-[0.98] transition-transform">
                    <div class="flex items-center gap-4">
                        <div class="w-8 h-8 rounded bg-yellow-500/20 flex items-center justify-center text-yellow-400"><i class="fa-solid fa-shield-halved"></i></div>
                        <span>Privacy & Payments</span>
                    </div>
                    <i class="fa-solid fa-chevron-right text-gray-600 text-sm"></i>
                </div>
            </div>
        </section>

    </main>

    <!-- Bottom Navigation -->
    <nav class="fixed bottom-0 w-full glass border-t border-white/10 z-[60] pb-safe">
        <div class="flex justify-around items-center py-3 text-[10px] font-bold max-w-md mx-auto">
            <button onclick="router('home')" class="flex flex-col items-center gap-1 text-teal-400 nav-btn transition-colors" id="nav-home">
                <i class="fa-solid fa-house text-lg mb-0.5"></i>
                <span>Home</span>
            </button>
            <button onclick="router('cart')" class="flex flex-col items-center gap-1 text-gray-500 hover:text-gray-300 nav-btn transition-colors" id="nav-cart">
                <i class="fa-solid fa-bag-shopping text-lg mb-0.5"></i>
                <span>Cart</span>
            </button>
            <button onclick="router('account')" class="flex flex-col items-center gap-1 text-gray-500 hover:text-gray-300 nav-btn transition-colors" id="nav-account">
                <i class="fa-regular fa-user text-lg mb-0.5"></i>
                <span>Profile</span>
            </button>
        </div>
    </nav>

    <script>
        // --- CONFIGURATION ---
        // REPLACE THIS WITH YOUR ACTUAL RAZORPAY KEY ID from https://dashboard.razorpay.com/app/keys
        // If you leave it empty, the simulation mode will run.
        const RAZORPAY_KEY_ID = ""; 
        
        // --- DATABASE & STATE ---
        const DB = {
            save: (key, data) => localStorage.setItem('crocosm_' + key, JSON.stringify(data)),
            get: (key) => JSON.parse(localStorage.getItem('crocosm_' + key)) || null,
            products: [
                { id: 1, name: "iPhone 15 Pro", price: 129900, rating: 4.9, img: "https://images.unsplash.com/photo-1696446701796-da61225697cc?auto=format&fit=crop&w=500&q=80", desc: "Titanium design. A17 Pro chip. The most powerful iPhone ever." },
                { id: 2, name: "Sony WH-1000XM5", price: 26990, rating: 4.8, img: "https://images.unsplash.com/photo-1618366712010-f4ae9c647dcb?auto=format&fit=crop&w=500&q=80", desc: "Industry-leading noise canceling headphones." },
                { id: 3, name: "Samsung S24 Ultra", price: 115999, rating: 4.7, img: "https://images.unsplash.com/photo-1706606991536-e320c95e1d86?auto=format&fit=crop&w=500&q=80", desc: "Galaxy AI is here. Note-worthy performance." },
                { id: 4, name: "Nike Air Jordan", price: 16995, rating: 4.6, img: "https://images.unsplash.com/photo-1575537302964-96cd47c06b1b?auto=format&fit=crop&w=500&q=80", desc: "Premium high-top sneakers for the streets." },
                { id: 5, name: "PlayStation 5 Slim", price: 54990, rating: 4.9, img: "https://images.unsplash.com/photo-1606144042614-b2417e99c4e3?auto=format&fit=crop&w=500&q=80", desc: "Play Has No Limits. Stunning graphics." },
                { id: 6, name: "MacBook Air M3", price: 114900, rating: 4.8, img: "https://images.unsplash.com/photo-1517336714731-489689fd1ca4?auto=format&fit=crop&w=500&q=80", desc: "Lean. Mean. M3 Machine." }
            ]
        };

        let cart = DB.get('cart') || [];
        let userAddress = DB.get('address') || {};
        let currentProduct = null;
        let selectedPaymentMethod = 'online';

        // --- INITIALIZATION ---
        window.onload = () => {
            renderProducts();
            updateCartBadge();
            if(userAddress.name) populateAddressForm();
        };

        // --- NAVIGATION SYSTEM ---
        function router(pageId) {
            document.querySelectorAll('.page-section').forEach(el => el.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            
            document.querySelectorAll('.nav-btn').forEach(el => {
                el.classList.remove('text-teal-400');
                el.classList.add('text-gray-500');
            });
            const navBtn = document.getElementById('nav-' + (pageId === 'product-details' || pageId === 'address' || pageId === 'payment' || pageId === 'success' ? 'home' : pageId));
            if(navBtn) {
                navBtn.classList.remove('text-gray-500');
                navBtn.classList.add('text-teal-400');
            }

            window.scrollTo(0,0);

            // Specific page logic
            if(pageId === 'cart') renderCart();
            if(pageId === 'address') populateAddressForm();
        }

        // --- HOME & PRODUCT LOGIC ---
        function renderProducts() {
            const grid = document.getElementById('product-grid');
            grid.innerHTML = DB.products.map(p => `
                <div class="glass-panel p-3 flex flex-col h-full cursor-pointer active:scale-95 transition-transform" onclick="showProduct(${p.id})">
                    <div class="h-36 mb-3 rounded-xl overflow-hidden bg-[#202126] relative">
                        <img src="${p.img}" class="w-full h-full object-cover">
                        <button onclick="event.stopPropagation(); addToCart(${p.id})" class="absolute bottom-2 right-2 w-8 h-8 rounded-full bg-white text-black flex items-center justify-center shadow-lg hover:bg-teal-400 transition">
                            <i class="fa-solid fa-plus text-xs"></i>
                        </button>
                    </div>
                    <h4 class="font-semibold text-xs text-gray-200 mb-1 line-clamp-1">${p.name}</h4>
                    <div class="flex items-center gap-1 text-[10px] text-gray-400 mb-2">
                        <i class="fa-solid fa-star text-yellow-500"></i> <span>${p.rating}</span>
                    </div>
                    <div class="mt-auto font-bold text-sm text-teal-400">₹${p.price.toLocaleString()}</div>
                </div>
            `).join('');
        }

        function showProduct(id) {
            currentProduct = DB.products.find(x => x.id === id);
            document.getElementById('detail-img').src = currentProduct.img;
            document.getElementById('detail-name').innerText = currentProduct.name;
            document.getElementById('detail-price').innerText = '₹' + currentProduct.price.toLocaleString();
            document.getElementById('detail-desc').innerText = currentProduct.desc;
            document.getElementById('detail-rating').innerText = currentProduct.rating;
            router('product-details');
        }

        // --- CART LOGIC ---
        function addToCart(id) {
            const existing = cart.find(x => x.id === id);
            if(existing) existing.qty++;
            else {
                const p = DB.products.find(x => x.id === id);
                cart.push({...p, qty: 1});
            }
            saveCart();
            
            // Animation feedback
            const badge = document.getElementById('header-cart-count');
            badge.classList.add('scale-150');
            setTimeout(() => badge.classList.remove('scale-150'), 200);
        }

        function saveCart() {
            DB.save('cart', cart);
            updateCartBadge();
        }

        function updateCartBadge() {
            const count = cart.reduce((a,b) => a + b.qty, 0);
            const el = document.getElementById('header-cart-count');
            el.innerText = count;
            el.style.display = count > 0 ? 'block' : 'none';
        }

        function renderCart() {
            const container = document.getElementById('cart-items-container');
            const footer = document.getElementById('cart-footer');
            
            if(cart.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-32 flex flex-col items-center opacity-50">
                        <i class="fa-solid fa-cart-arrow-down text-5xl mb-4 text-gray-600"></i>
                        <p class="text-gray-400">Your cart is empty</p>
                        <button onclick="router('home')" class="mt-6 text-teal-400 font-bold text-sm border border-teal-400/30 px-6 py-2 rounded-full">Start Shopping</button>
                    </div>
                `;
                footer.classList.add('hidden');
                return;
            }

            footer.classList.remove('hidden');
            let total = 0;

            container.innerHTML = cart.map(item => {
                total += item.price * item.qty;
                return `
                <div class="glass-panel p-3 flex gap-3 items-center">
                    <img src="${item.img}" class="w-16 h-16 object-cover rounded-lg bg-[#202126]">
                    <div class="flex-1">
                        <h4 class="font-bold text-sm mb-1 text-gray-200">${item.name}</h4>
                        <div class="text-teal-400 font-bold text-sm mb-2">₹${item.price.toLocaleString()}</div>
                        <div class="flex items-center gap-3 bg-[#0f1014] w-max rounded px-2 py-1 border border-white/5">
                            <button onclick="changeQty(${item.id}, -1)" class="text-gray-400 w-4">-</button>
                            <span class="font-bold text-xs text-white w-4 text-center">${item.qty}</span>
                            <button onclick="changeQty(${item.id}, 1)" class="text-gray-400 w-4">+</button>
                        </div>
                    </div>
                    <button onclick="removeItem(${item.id})" class="p-2 text-gray-600 hover:text-red-500"><i class="fa-solid fa-trash text-sm"></i></button>
                </div>
            `}).join('');

            document.getElementById('cart-total-price').innerText = '₹' + total.toLocaleString();
        }

        function changeQty(id, delta) {
            const item = cart.find(x => x.id === id);
            if(item) {
                item.qty += delta;
                if(item.qty <= 0) removeItem(id);
                else saveCart();
                renderCart(); // Re-render immediately
            }
        }

        function removeItem(id) {
            cart = cart.filter(x => x.id !== id);
            saveCart();
            renderCart();
        }

        // --- CHECKOUT FLOW ---
        function initCheckout() {
            router('address');
        }

        function populateAddressForm() {
            if(userAddress.name) {
                document.getElementById('addr-name').value = userAddress.name;
                document.getElementById('addr-phone').value = userAddress.phone;
                document.getElementById('addr-pin').value = userAddress.pin;
                document.getElementById('addr-text').value = userAddress.text;
                document.getElementById('addr-city').value = userAddress.city;
            }
        }

        function saveAddress(e) {
            e.preventDefault();
            userAddress = {
                name: document.getElementById('addr-name').value,
                phone: document.getElementById('addr-phone').value,
                pin: document.getElementById('addr-pin').value,
                text: document.getElementById('addr-text').value,
                city: document.getElementById('addr-city').value
            };
            DB.save('address', userAddress);
            
            // Prep Payment Page
            const total = cart.reduce((a,b) => a + (b.price * b.qty), 0);
            document.getElementById('payment-amount').innerText = '₹' + total.toLocaleString();
            document.getElementById('payment-address-preview').innerText = `${userAddress.name}, ${userAddress.city}`;
            
            router('payment');
        }

        // --- PAYMENT GATEWAY INTEGRATION ---
        function selectPayment(mode) {
            selectedPaymentMethod = mode;
            const btn = document.getElementById('pay-btn');
            if(mode === 'cod') {
                btn.innerHTML = '<span>Place Order (COD)</span> <i class="fa-solid fa-arrow-right text-xs"></i>';
                btn.className = "w-full bg-white text-black py-4 rounded-xl font-bold shadow-lg flex items-center justify-center gap-2";
            } else {
                btn.innerHTML = '<span>Pay Securely</span> <i class="fa-solid fa-lock text-xs"></i>';
                btn.className = "w-full bg-teal-500 text-black py-4 rounded-xl font-bold shadow-lg shadow-teal-500/20 flex items-center justify-center gap-2";
            }
        }

        function processPayment() {
            const totalAmount = cart.reduce((a,b) => a + (b.price * b.qty), 0);
            
            if (selectedPaymentMethod === 'cod') {
                confirmOrder("Cash on Delivery", totalAmount);
            } else {
                // START RAZORPAY FLOW
                launchRazorpay(totalAmount);
            }
        }

        function launchRazorpay(amount) {
            const options = {
                "key": RAZORPAY_KEY_ID, // Key is hidden in variable or server
                "amount": amount * 100, // Amount is in currency subunits. Default currency is INR. Hence, 50000 refers to 50000 paise
                "currency": "INR",
                "name": "Crocosm Store",
                "description": "Order Payment",
                "image": "https://cdn-icons-png.flaticon.com/512/4403/4403548.png",
                "handler": function (response){
                    // This function is called ONLY on success
                    // In a real app, you send response.razorpay_payment_id to your backend for verification
                    confirmOrder("Online (Razorpay)", amount, response.razorpay_payment_id);
                },
                "prefill": {
                    "name": userAddress.name,
                    "email": "customer@example.com",
                    "contact": userAddress.phone
                },
                "theme": {
                    "color": "#00C9A7"
                },
                // Handle failure/modal close
                "modal": {
                    "ondismiss": function(){
                        // console.log('Checkout form closed');
                    }
                }
            };

            // SIMULATION MODE (If no key is provided)
            if(!RAZORPAY_KEY_ID) {
                // Create a fake modal for demo
                const simulation = confirm("SIMULATION MODE:\n\nSince no API Key was provided, we will simulate a successful payment.\n\nClick OK to Simulate Success.\nClick Cancel to Simulate Failure.");
                if(simulation) {
                    confirmOrder("Online (Simulated)", amount, "pay_test_123456789");
                }
            } else {
                // REAL MODE
                const rzp1 = new Razorpay(options);
                rzp1.on('payment.failed', function (response){
                    alert("Payment Failed: " + response.error.description);
                });
                rzp1.open();
            }
        }

        function confirmOrder(mode, amount, txId = "") {
            const orderId = 'ORD-' + Math.floor(Math.random() * 1000000);
            
            // Setup Success Page
            document.getElementById('success-order-id').innerText = orderId;
            document.getElementById('success-amount').innerText = '₹' + amount.toLocaleString();
            
            // Clear Cart
            cart = [];
            DB.save('cart', cart);
            updateCartBadge();
            
            router('success');
        }

    </script>
</body>
</html>


