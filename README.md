
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PandaBF - Application Mobile</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        /* Styles de base pour le mode sombre */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1a1a2e; /* Fond principal plus sombre */
            color: #ffffff; /* Texte par défaut clair */
        }
        /* Style général des cartes */
        .card {
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 0 10px -5px rgba(0, 0, 0, 0.04);
            background-color: #1f213a; /* Fond des cartes plus sombre */
        }
        .dark-screen {
            background-color: #1a1a2e;
            color: #ffffff;
            min-height: 100vh;
            padding-bottom: 80px; /* Pour la barre de navigation */
        }
        /* Style des cartes de portefeuilles et de statistiques */
        .wallet-card {
            background-color: #1a1a2e; 
            color: #ffffff;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
        }
        #section-wallet .wallet-stat-card {
            background-color: #1f213a; 
            color: #ffffff;
            border: 1px solid #2f334d; 
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
        }
        /* Couleurs pour le portefeuille */
        .text-cyan {
            color: #00bcd4; 
        }
        /* Visibilité lors de l'authentification */
        .hidden-until-auth {
             display: none;
        }
        /* Bouton de montant sélectionné (Recharge) */
        .selected-amount {
            background-color: #ff9800; 
            color: white;
            border-color: #ff9800;
        }
        /* Carte d'information d'accueil (fixée en haut) */
        .home-info-card {
            background-color: #1f213a; 
            border-radius: 10px;
            padding: 1rem; 
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }
        /* Fond du conteneur fixe */
        .bg-1f213a { 
             background-color: #1f213a; 
        }
        /* Entrée de retrait stylisée */
        .withdrawal-input-field {
            background-color: transparent;
            border-bottom: 1px solid #4a5568; 
            color: white;
            font-size: 2.5rem; 
            font-weight: 700;
        }
        /* Contenu d'en-tête fixe (Accueil, Simulateur) */
        .fixed-header-content {
            position: fixed;
            top: 0;
            width: 100%;
            max-width: 512px; /* max-w-lg */
            z-index: 10;
            background-color: #1f213a; 
            padding-top: 1rem;
            padding-left: 1rem;
            padding-right: 1rem;
            padding-bottom: 0.5rem;
        }
        /* Boutons d'icônes circulaires (Accueil) */
        .icon-button-circle {
            background: radial-gradient(circle at center, #3f3e58 0%, #20202e 100%); 
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
        }
        /* Cartes de produits à défilement horizontal (Accueil) */
        .product-scroll-card {
            flex: 0 0 60%; 
            max-width: 60%;
            background-color: #1f213a;
            border: 1px solid #2f334d;
        }
         /* Cartes de logos de banques */
        .bank-logo-card {
            flex: 0 0 20%; 
            max-width: 20%;
            height: 50px;
            background-color: #3f3e58;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        /* Masquage de la barre de défilement pour une apparence mobile */
        .scrollbar-hide::-webkit-scrollbar {
            display: none;
        }
        .scrollbar-hide {
            -ms-overflow-style: none; 
            scrollbar-width: none;  
        }

    </style>
</head>
<body class="flex flex-col min-h-screen">

    <div id="app" class="w-full max-w-lg mx-auto pb-24">

        <div id="authScreen" class="w-full pt-8 px-4">
            <div class="card bg-gray-800 p-6 sm:p-8 rounded-xl border-t-4 border-amber-500 w-full mb-8">
                <h2 id="authTitle" class="text-2xl font-bold text-white mb-4 text-center">🔑 Connexion</h2>
                <p id="authSubtitle" class="text-center text-sm text-gray-400 mb-6">Veuillez entrer vos identifiants pour vous connecter.</p>

                <form id="authForm" class="space-y-4">
                    <input type="hidden" id="authMode" value="login">
                    <div class="space-y-2">
                        <label for="phone" class="block text-sm font-medium text-gray-300">Numéro de Téléphone</label>
                        <div class="flex space-x-2">
                            <select id="countryCode" name="countryCode" 
                                    class="px-2 py-2 border border-gray-700 rounded-lg focus:ring-amber-500 focus:border-amber-500 bg-gray-700 text-sm font-medium text-white">
                                <option value="+226">+226 BF</option>
                                <option value="+223">+223 Mali</option>
                                <option value="+225">+225 Côte d'Ivoire</option>
                            </select>
                            <input type="tel" id="phone" name="phone" placeholder="00 00 00 00" required
                                   class="flex-grow px-3 py-2 border border-gray-700 rounded-lg focus:ring-amber-500 focus:border-amber-500 bg-gray-700 text-white">
                        </div>
                    </div>
                    
                    <div class="space-y-2">
                        <label for="password" class="block text-sm font-medium text-gray-300">Mot de Passe</label>
                        <div class="relative">
                            <input type="password" id="password" name="password" required
                                   class="w-full px-3 py-2 pr-10 border border-gray-700 rounded-lg focus:ring-amber-500 focus:border-amber-500 bg-gray-700 text-white">
                            <button type="button" id="togglePassword"
                                    class="absolute inset-y-0 right-0 flex items-center pr-3 text-gray-400 hover:text-white">
                                <svg id="eye-closed" class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.875 18.825A10.05 10.05 0 0112 19c-4.97 0-9-3.357-9-7s4.03-7 9-7 9 3.357 9 7c0 .167-.02.33-.06.49M16 12a4 4 0 11-8 0 4 4 0 018 0zM12 17.5V19M12 5v1.5"></path></svg>
                                <svg id="eye-open" class="w-5 h-5 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0zM2.458 12C3.732 7.943 7.523 5 12 5c4.477 0 8.268 2.943 9.542 7-1.274 4.057-5.065 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"></path></svg>
                            </button>
                        </div>
                    </div>
                    
                    <div id="invitationCodeField" class="space-y-2 hidden">
                        <label for="invitationCodeInput" class="block text-sm font-medium text-gray-300">Code d'Invitation (Optionnel)</label>
                        <input type="text" id="invitationCodeInput" name="invitationCode" placeholder="Code de parrainage"
                               class="w-full px-3 py-2 border border-gray-700 rounded-lg focus:ring-amber-500 focus:border-amber-500 bg-gray-700 text-white">
                    </div>
                    
                    <button type="submit" id="authButton"
                            class="w-full bg-amber-500 text-white py-3 rounded-lg font-semibold hover:bg-amber-600 transition duration-150 ease-in-out focus:outline-none focus:ring-2 focus:ring-amber-500 focus:ring-offset-2">
                        Se Connecter
                    </button>
                </form>
                
                <div class="mt-6 text-center">
                    <button type="button" id="toggleAuthMode"
                            class="text-sm text-cyan-400 hover:text-cyan-300 transition duration-150 ease-in-out">
                        Pas de compte ? S'inscrire
                    </button>
                </div>
            </div>
        </div>
        
        <div id="mainContent" class="hidden-until-auth w-full">
            <div id="section-home" class="section-content w-full">
                
                <div class="fixed-header-content bg-1f213a">
                    <div class="home-info-card border-t-4 border-amber-500 p-4"> 
                        
                        <div class="flex justify-between items-start mb-2">
                            <div class="w-1/2">
                                <span class="block text-xs font-medium text-gray-400 uppercase tracking-wider mb-0">BALANCE</span>
                                <span class="block text-2xl font-extrabold text-white" id="currentBalance">0.00</span>
                                <span class="text-xs text-gray-300">XOF</span>
                            </div>
                            <div class="w-1/2 text-right cursor-pointer" onclick="showSection('exchange')">
                                <span class="block text-xs font-medium text-amber-600 uppercase tracking-wider mb-0">MESSAGE</span>
                                <span class="block text-2xl font-bold text-amber-500" id="messageStatus">0</span>
                                <span class="text-xs text-amber-600" id="messageLabel">NOUVEAU</span>
                            </div>
                        </div>
                        
                        <hr class="border-gray-700 my-2">

                        <div class="flex justify-between items-end">
                            <div class="w-1/2">
                                <span class="block text-xs font-medium text-gray-400 uppercase tracking-wider mb-0">REVENUS DU JOUR</span>
                                <span class="block text-xl font-semibold text-white" id="dailyRevenueDisplay">0.00</span>
                                <span class="text-xs text-gray-300">XOF</span>
                            </div>
                            <div class="w-1/2 text-right">
                                <span class="block text-xs font-medium text-amber-600 uppercase tracking-wider mb-0">PANDA BANK</span>
                                <div class="flex items-center justify-end space-x-1">
                                    <span class="block text-xl font-semibold text-amber-500" id="pandaBankAmountDisplay">0</span>
                                    <span class="text-xs text-amber-600">XOF</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="h-60 w-full"></div> 

                <div class="px-4 py-4">
                    
                    <div class="grid grid-cols-4 gap-4 mb-4 text-center">
                        
                        <button onclick="showSection('my-product')" class="flex flex-col items-center space-y-1">
                            <div class="w-12 h-12 icon-button-circle rounded-full flex items-center justify-center">
                                <svg class="w-6 h-6 text-white/90" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M3 7v10a2 2 0 002 2h14a2 2 0 002-2V7M3 7h18M3 7l9 6 9-6"></path></svg>
                            </div>
                            <span class="text-xs text-gray-300">Mon Produit</span>
                        </button>
                        
                        <button onclick="showSection('recharge')" class="flex flex-col items-center space-y-1">
                             <div class="w-12 h-12 icon-button-circle rounded-full flex items-center justify-center">
                                <svg class="w-6 h-6 text-white/90" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 6v6m0 0v6m0-6h6m-6 0H6"></path></svg>
                            </div>
                            <span class="text-xs text-gray-300">Recharger</span>
                        </button>
                        
                        <button onclick="showSection('withdrawal')" class="flex flex-col items-center space-y-1">
                             <div class="w-12 h-12 icon-button-circle rounded-full flex items-center justify-center">
                                <svg class="w-6 h-6 text-white/90 transform rotate-45" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"></path></svg>
                            </div>
                            <span class="text-xs text-gray-300">Retrait</span>
                        </button>
                        
                        <button onclick="showSection('team')" class="flex flex-col items-center space-y-1">
                             <div class="w-12 h-12 icon-button-circle rounded-full flex items-center justify-center">
                                <svg class="w-6 h-6 text-white/90" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M17 20h-1a2 2 0 01-2-2V5a2 2 0 012-2h1a2 2 0 012 2v13a2 2 0 01-2 2zM7 20h1a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v13a2 2 0 002 2zM12 3v18"></path></svg>
                            </div>
                            <span class="text-xs text-gray-300">Mon équipe</span>
                        </button>
                        
                    </div>
                    
                    <div class="flex space-x-3 mb-8">
                        
                        <button onclick="showSection('service')" class="flex-1 card p-2 rounded-lg flex items-center justify-center space-x-2 bg-gray-800 border border-gray-700">
                            <svg class="w-5 h-5 text-cyan-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10H7m7 0a2 2 0 012 2v4a2 2 0 01-2 2H7a2 2 0 01-2-2v-4a2 2 0 012-2h7zm10 0h-4M19 12h2M19 16h2"></path></svg>
                            <span class="text-xs font-semibold text-white">Service</span>
                        </button>
                        
                        <button onclick="showSection('invite')" class="flex-1 card p-2 rounded-lg flex items-center justify-center space-x-2 bg-gray-800 border border-gray-700">
                            <svg class="w-5 h-5 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 11c0 3.517-1.025 6.68-3.234 9.387M12 11a4 4 0 100-8 4 4 0 000 8zm0 0v1m-4 10h8a2 2 0 002-2v-2a2 2 0 00-2-2H8a2 2 0 00-2 2v2a2 2 0 002 2z"></path></svg>
                            <span class="text-xs font-semibold text-white">Inviter Des Amis</span>
                        </button>
                        
                        <button onclick="showSection('exchange')" class="flex-1 card p-2 rounded-lg flex items-center justify-center space-x-2 bg-gray-800 border border-gray-700">
                            <svg class="w-5 h-5 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7h12m0 0l-4-4m4 4l-4 4m-4 6H4m0 0l4 4m-4-4l4-4"></path></svg>
                            <span class="text-xs font-semibold text-white">Échanger</span>
                        </button>
                        
                    </div>
                    
                    <h2 class="text-lg font-bold text-white mb-3">🔥 Produits à Chaud</h2>
                    <div class="flex space-x-4 overflow-x-scroll pb-4 scrollbar-hide">
                        
                        <div class="product-scroll-card card p-3 rounded-xl flex flex-col justify-between cursor-pointer" onclick="showProductDetails('C-5D')">
                            <div>
                                <h3 class="text-base font-bold text-white mb-1">C-5D</h3>
                                <div class="flex items-center space-x-1 mb-2">
                                    <svg class="w-4 h-4 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                                    <span class="text-xs text-gray-400">48 Jours</span>
                                </div>
                                <p class="text-xs text-gray-400">Revenu Total: <span class="font-semibold text-green-400">9,600.00 F</span></p>
                            </div>
                            <div class="mt-3 flex justify-between items-center pt-2 border-t border-gray-700">
                                <span class="text-sm font-bold text-white">Prix:</span>
                                <span class="text-lg font-extrabold text-amber-500">5,000 F</span>
                            </div>
                        </div>
                        
                        <div class="product-scroll-card card p-3 rounded-xl flex flex-col justify-between cursor-pointer" onclick="showProductDetails('C-20D')">
                            <div>
                                <h3 class="text-base font-bold text-white mb-1">C-20D</h3>
                                <div class="flex items-center space-x-1 mb-2">
                                    <svg class="w-4 h-4 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                                    <span class="text-xs text-gray-400">48 Jours</span>
                                </div>
                                <p class="text-xs text-gray-400">Revenu Total: <span class="font-semibold text-green-400">40,320.00 F</span></p>
                            </div>
                            <div class="mt-3 flex justify-between items-center pt-2 border-t border-gray-700">
                                <span class="text-sm font-bold text-white">Prix:</span>
                                <span class="text-lg font-extrabold text-amber-500">20,000 F</span>
                            </div>
                        </div>
                        
                        <div class="product-scroll-card card p-3 rounded-xl flex flex-col justify-between cursor-pointer" onclick="showProductDetails('C-60D')">
                            <div>
                                <h3 class="text-base font-bold text-white mb-1">C-60D</h3>
                                <div class="flex items-center space-x-1 mb-2">
                                    <svg class="w-4 h-4 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                                    <span class="text-xs text-gray-400">48 Jours</span>
                                </div>
                                <p class="text-xs text-gray-400">Revenu Total: <span class="font-semibold text-green-400">126,720.00 F</span></p>
                            </div>
                            <div class="mt-3 flex justify-between items-center pt-2 border-t border-gray-700">
                                <span class="text-sm font-bold text-white">Prix:</span>
                                <span class="text-lg font-extrabold text-amber-500">60,000 F</span>
                            </div>
                        </div>
                        
                        <div class="product-scroll-card card p-3 rounded-xl flex flex-col justify-between cursor-pointer" onclick="showProductDetails('C-150D')">
                            <div>
                                <h3 class="text-base font-bold text-white mb-1">C-150D</h3>
                                <div class="flex items-center space-x-1 mb-2">
                                    <svg class="w-4 h-4 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                                    <span class="text-xs text-gray-400">48 Jours</span>
                                </div>
                                <p class="text-xs text-gray-400">Revenu Total: <span class="font-semibold text-green-400">331,200.00 F</span></p>
                            </div>
                            <div class="mt-3 flex justify-between items-center pt-2 border-t border-gray-700">
                                <span class="text-sm font-bold text-white">Prix:</span>
                                <span class="text-lg font-extrabold text-amber-500">150,000 F</span>
                            </div>
                        </div>

                    </div>
                    
                    <h2 class="text-lg font-bold text-white mt-6 mb-3">🤝 Nos Partenaires Bancaires</h2>
                    <div class="flex space-x-4 overflow-x-scroll pb-4 scrollbar-hide">
                        
                        <div class="bank-logo-card">
                            <span class="text-xs font-bold text-white/70">Banque 1</span>
                        </div>
                        <div class="bank-logo-card">
                            <span class="text-xs font-bold text-white/70">Banque 2</span>
                        </div>
                        <div class="bank-logo-card">
                            <span class="text-xs font-bold text-white/70">Banque 3</span>
                        </div>
                        <div class="bank-logo-card">
                            <span class="text-xs font-bold text-white/70">Banque 4</span>
                        </div>
                        <div class="bank-logo-card">
                            <span class="text-xs font-bold text-white/70">Banque 5</span>
                        </div>
                        <div class="bank-logo-card">
                            <span class="text-xs font-bold text-white/70">Banque 6</span>
                        </div>
                    </div>
                    
                </div>
                
            </div>
            
            <div id="section-my-product" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
    
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('home')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Mon produit</h2>
                </div>

                <div class="w-full bg-gray-900 border-b border-gray-800 p-4 sticky top-16 z-10">
                    <div class="flex space-x-3 overflow-x-auto">
                        <button id="tab-bonus" onclick="switchProductTab('bonus')" class="flex-shrink-0 bg-cyan-600 text-white font-semibold py-2 px-4 rounded-lg text-sm transition duration-150">
                            Bonus
                        </button>
                        <button id="tab-purchased" onclick="switchProductTab('purchased')" class="flex-shrink-0 bg-gray-800 text-gray-300 font-semibold py-2 px-4 rounded-lg text-sm transition duration-150 border border-gray-700">
                            Acheté
                        </button>
                        <button id="tab-expired" onclick="switchProductTab('expired')" class="flex-shrink-0 bg-gray-800 text-gray-300 font-semibold py-2 px-4 rounded-lg text-sm transition duration-150 border border-gray-700">
                            Expiré
                        </button>
                    </div>
                </div>
                
                <div class="p-4 flex-grow overflow-y-auto space-y-4">

                    <div id="bonus-list">
                        <div id="free-trial-card" class="card p-4 rounded-xl border border-gray-700">
                            <div class="flex justify-between items-start mb-2">
                                <span class="bg-purple-600 text-white text-xs font-bold px-2 py-0.5 rounded-full shadow-lg">Free Trial (Bonus)</span>
                                <span id="free-trial-status-badge" class="bg-green-700 text-green-300 text-xs font-bold px-2 py-0.5 rounded-full">Actif</span>
                            </div>
                            
                            <div class="flex space-x-3">
                                <div class="flex-shrink-0 w-16 h-16 relative overflow-hidden rounded-lg bg-purple-900/30 flex items-center justify-center border border-gray-600">
                                     <svg class="w-8 h-8 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path></svg>
                                </div>
                                
                                <div class="flex-grow text-sm space-y-0.5">
                                    <div class="flex justify-between">
                                        <span class="text-gray-400">Cycles:</span>
                                        <span class="font-bold text-white"><span id="free-trial-received">0</span> / 3</span>
                                    </div>
                                    <div class="flex justify-between">
                                        <span class="text-gray-400">Revenu Obtenu:</span>
                                        <span class="font-bold text-amber-400" id="free-trial-revenue">0.00 FCFA</span>
                                    </div>
                                     <div class="flex justify-between">
                                        <span class="text-gray-400">Revenu Journalier:</span>
                                        <span class="font-bold text-white">200.00 FCFA</span>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="flex justify-between items-center mt-2 pt-2 border-t border-gray-700">
                                 <span class="text-xs text-gray-500">Heure Réception: <span class="text-white font-semibold last-collected-time">Non Reçu</span></span>
                                 <button id="free-trial-receive-btn" 
                                         onclick="handleReceiveDailyRevenue('free-trial', 200)"
                                         class="bg-amber-500 text-white font-semibold py-1 px-3 rounded-lg text-xs hover:bg-amber-600 transition duration-150">
                                    Collecter
                                 </button>
                            </div>
                        </div>
                    </div>

                    <div id="purchased-list" class="hidden">
                        <div class="p-4 text-center text-gray-400 border border-dashed border-gray-700 rounded-xl" id="purchased-empty-state">
                             <p class="text-sm font-medium">Vous n'avez pas encore acheté de produit.</p>
                             <button onclick="showSection('simulator')" class="text-cyan-500 text-sm mt-2 hover:text-cyan-400">
                                 Acheter un produit maintenant
                             </button>
                        </div>
                    </div>
                    
                    <div id="expired-list" class="hidden">
                        <div id="expired1-card" class="card p-4 rounded-xl border border-gray-700 opacity-70">
                            <div class="flex justify-between items-start mb-2">
                                <span class="bg-cyan-600 text-white text-xs font-bold px-2 py-0.5 rounded-full shadow-lg">C-5D</span>
                                <span id="expired1-status-badge" class="bg-red-700 text-red-300 text-xs font-bold px-2 py-0.5 rounded-full">Expiré</span>
                            </div>
                            
                            <div class="flex space-x-3">
                                <div class="flex-shrink-0 w-16 h-16 relative overflow-hidden rounded-lg bg-cyan-900/30 flex items-center justify-center border border-gray-600">
                                     <svg class="w-8 h-8 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M19 21V19C19 18.4477 18.5523 18 18 18H6C5.44772 18 5 18.4477 5 19V21M3 10L12 3L21 10M3 10V21C3 21.5523 3.44772 22 4 22H20C20.5523 22 21 21.5523 21 21V10M10 15H14M10 15V22"></path></svg>
                                </div>
                                
                                <div class="flex-grow text-sm space-y-0.5">
                                    <div class="flex justify-between">
                                        <span class="text-gray-400">Cycles:</span>
                                        <span class="font-bold text-white"><span id="expired1-received">48</span> / 48</span>
                                    </div>
                                    <div class="flex justify-between">
                                        <span class="text-gray-400">Revenu Obtenu:</span>
                                        <span class="font-bold text-amber-400" id="expired1-revenue">9,600.00 FCFA</span>
                                    </div>
                                     <div class="flex justify-between">
                                        <span class="text-gray-400">Revenu Journalier:</span>
                                        <span class="font-bold text-white">200.00 FCFA</span>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="flex justify-between items-center mt-2 pt-2 border-t border-gray-700">
                                 <span class="text-xs text-gray-500">Heure Réception: <span class="text-white font-semibold last-collected-time">Terminé</span></span>
                                 <button id="expired1-receive-btn" 
                                         disabled
                                         class="bg-gray-600 text-gray-400 font-semibold py-1 px-3 rounded-lg text-xs cursor-not-allowed">
                                    Terminé
                                 </button>
                            </div>
                        </div>
                    </div>
                    
                </div>
            </div>

            
            <div id="section-simulator" class="section-content w-full hidden dark-screen">
                
                <div class="fixed-header-content bg-1f213a py-3 border-b border-gray-700">
                    <div class="flex space-x-2 overflow-x-auto px-4 pb-2">
                        <button class="flex-shrink-0 bg-cyan-600 text-white font-semibold py-2 px-6 rounded-full text-sm">
                            Tous
                        </button>
                        <button class="flex-shrink-0 bg-gray-800 text-gray-300 font-semibold py-2 px-6 rounded-full text-sm border border-cyan-600">
                            Série C
                        </button>
                        <button class="flex-shrink-0 bg-gray-800 text-gray-300 font-semibold py-2 px-6 rounded-full text-sm border border-gray-700">
                            Série F
                        </button>
                         <button class="flex-shrink-0 bg-gray-800 text-gray-300 font-semibold py-2 px-6 rounded-full text-sm border border-gray-700">
                            Série S
                        </button>
                    </div>
                </div>
                
                <div class="h-20 w-full"></div> 

                <div class="px-4 py-2 space-y-4">
                    
                    <div class="card p-3 rounded-xl flex items-center space-x-3 border border-gray-700 cursor-pointer" onclick="showProductDetails('C-5D')">
                        <div class="flex-shrink-0 w-16 h-16 relative overflow-hidden rounded-lg bg-cyan-900/30 flex items-center justify-center">
                             <svg class="w-10 h-10 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M19 21V19C19 18.4477 18.5523 18 18 18H6C5.44772 18 5 18.4477 5 19V21M3 10L12 3L21 10M3 10V21C3 21.5523 3.44772 22 4 22H20C20.5523 22 21 21.5523 21 21V10M10 15H14M10 15V22"></path></svg>
                        </div>
                        <div class="flex-grow grid grid-cols-2 gap-y-0.5">
                            <h3 class="col-span-1 text-lg font-bold text-white">C-5D</h3>
                            <div class="col-span-1 text-right">
                                <span class="inline-block bg-green-700 text-green-300 text-xs font-bold px-2 py-0.5 rounded-full">192.0%</span>
                            </div>
                            <div class="col-span-1">
                                <span class="block text-xs text-gray-400">Revenu Total</span>
                                <span class="block text-sm font-bold text-white">9,600.00 <span class="text-xs text-gray-400">F</span></span>
                            </div>
                            <div class="col-span-1 text-right">
                                <span class="block text-xs text-gray-400">Prix</span>
                                <span class="block text-sm font-bold text-amber-500">5,000.00 <span class="text-xs">F</span></span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="card p-3 rounded-xl flex items-center space-x-3 border border-gray-700 cursor-pointer" onclick="showProductDetails('C-20D')">
                        <div class="flex-shrink-0 w-16 h-16 relative overflow-hidden rounded-lg bg-cyan-900/30 flex items-center justify-center">
                            <svg class="w-10 h-10 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M19 21V19C19 18.4477 18.5523 18 18 18H6C5.44772 18 5 18.4477 5 19V21M3 10L12 3L21 10M3 10V21C3 21.5523 3.44772 22 4 22H20C20.5523 22 21 21.5523 21 21V10M10 15H14M10 15V22"></path></svg>
                        </div>
                        <div class="flex-grow grid grid-cols-2 gap-y-0.5">
                            <h3 class="col-span-1 text-lg font-bold text-white">C-20D</h3>
                            <div class="col-span-1 text-right">
                                <span class="inline-block bg-green-700 text-green-300 text-xs font-bold px-2 py-0.5 rounded-full">201.6%</span>
                            </div>
                            <div class="col-span-1">
                                <span class="block text-xs text-gray-400">Revenu Total</span>
                                <span class="block text-sm font-bold text-white">40,320.00 <span class="text-xs text-gray-400">F</span></span>
                            </div>
                            <div class="col-span-1 text-right">
                                <span class="block text-xs text-gray-400">Prix</span>
                                <span class="block text-sm font-bold text-amber-500">20,000.00 <span class="text-xs">F</span></span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="card p-3 rounded-xl flex items-center space-x-3 border border-gray-700 cursor-pointer" onclick="showProductDetails('C-60D')">
                        <div class="flex-shrink-0 w-16 h-16 relative overflow-hidden rounded-lg bg-cyan-900/30 flex items-center justify-center">
                            <svg class="w-10 h-10 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M19 21V19C19 18.4477 18.5523 18 18 18H6C5.44772 18 5 18.4477 5 19V21M3 10L12 3L21 10M3 10V21C3 21.5523 3.44772 22 4 22H20C20.5523 22 21 21.5523 21 21V10M10 15H14M10 15V22"></path></svg>
                        </div>
                        <div class="flex-grow grid grid-cols-2 gap-y-0.5">
                            <h3 class="col-span-1 text-lg font-bold text-white">C-60D</h3>
                            <div class="col-span-1 text-right">
                                <span class="inline-block bg-green-700 text-green-300 text-xs font-bold px-2 py-0.5 rounded-full">211.2%</span>
                            </div>
                            <div class="col-span-1">
                                <span class="block text-xs text-gray-400">Revenu Total</span>
                                <span class="block text-sm font-bold text-white">126,720.00 <span class="text-xs text-gray-400">F</span></span>
                            </div>
                            <div class="col-span-1 text-right">
                                <span class="block text-xs text-gray-400">Prix</span>
                                <span class="block text-sm font-bold text-amber-500">60,000.00 <span class="text-xs">F</span></span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="card p-3 rounded-xl flex items-center space-x-3 border border-cyan-600 relative overflow-hidden cursor-pointer" onclick="showProductDetails('C-150D')">
                        <div class="absolute top-0 left-0 bg-cyan-600 text-white text-xs font-bold px-2 py-1 transform -rotate-45 -translate-x-4 translate-y-4">
                            new
                        </div>
                        <div class="flex-shrink-0 w-16 h-16 relative overflow-hidden rounded-lg bg-cyan-900/30 flex items-center justify-center">
                            <svg class="w-10 h-10 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M19 21V19C19 18.4477 18.5523 18 18 18H6C5.44772 18 5 18.4477 5 19V21M3 10L12 3L21 10M3 10V21C3 21.5523 3.44772 22 4 22H20C20.5523 22 21 21.5523 21 21V10M10 15H14M10 15V22"></path></svg>
                        </div>
                        <div class="flex-grow grid grid-cols-2 gap-y-0.5">
                            <h3 class="col-span-1 text-lg font-bold text-white">C-150D</h3>
                            <div class="col-span-1 text-right">
                                <span class="inline-block bg-green-700 text-green-300 text-xs font-bold px-2 py-0.5 rounded-full">220.8%</span>
                            </div>
                            <div class="col-span-1">
                                <span class="block text-xs text-gray-400">Revenu Total</span>
                                <span class="block text-sm font-bold text-white">331,200.00 <span class="text-xs text-gray-400">F</span></span>
                            </div>
                            <div class="col-span-1 text-right">
                                <span class="block text-xs text-gray-400">Prix</span>
                                <span class="block text-sm font-bold text-amber-500">150,000.00 <span class="text-xs">F</span></span>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
            
            <div id="section-product-details" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
    
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('simulator')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Détails Produit</h2>
                </div>

                <div class="p-4 flex-grow overflow-y-auto">
                    
                    <div class="relative mb-4 rounded-xl overflow-hidden bg-gray-900 shadow-xl border border-gray-700">
                        <div id="detail-product-image" class="h-48 flex items-center justify-center bg-cyan-900/20">
                            <svg class="w-24 h-24 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M19 21V19C19 18.4477 18.5523 18 18 18H6C5.44772 18 5 18.4477 5 19V21M3 10L12 3L21 10M3 10V21C3 21.5523 3.44772 22 4 22H20C20.5523 22 21 21.5523 21 21V10M10 15H14M10 15V22"></path></svg>
                        </div>
                        <div class="absolute bottom-3 left-3">
                            <span id="detail-product-name" class="text-2xl font-bold text-white bg-black/40 px-3 py-1 rounded">C-5D</span>
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-3 mb-4">
                        
                        <div class="p-4 bg-gray-900 rounded-xl shadow-lg border border-gray-700">
                            <span class="block text-sm text-gray-400">Prix</span>
                            <span class="block text-xl font-extrabold text-amber-500 mt-1" id="detail-price">5,000.00</span>
                            <span class="text-xs text-gray-400">FCFA</span>
                        </div>
                        
                        <div class="p-4 bg-gray-900 rounded-xl shadow-lg border border-gray-700">
                            <span class="block text-sm text-gray-400">Durée Détention</span>
                            <span class="block text-xl font-extrabold text-white mt-1" id="detail-cycles">48 Jours Ouvrables</span>
                        </div>
                        
                        <div class="p-4 bg-gray-900 rounded-xl shadow-lg border border-gray-700">
                            <span class="block text-sm text-gray-400">Revenu Total</span>
                            <span class="block text-xl font-extrabold text-green-400 mt-1" id="detail-total-revenue">9,600.00</span>
                            <span class="text-xs text-gray-400">FCFA</span>
                        </div>
                        
                        <div class="p-4 bg-gray-900 rounded-xl shadow-lg border border-gray-700">
                            <span class="block text-sm text-gray-400">Revenu Quotidien</span>
                            <span class="block text-xl font-extrabold text-white mt-1" id="detail-daily-revenue">200.00</span>
                            <span class="text-xs text-gray-400">FCFA</span>
                        </div>
                        
                        <div class="p-4 bg-gray-900 rounded-xl shadow-lg border border-gray-700">
                            <span class="block text-sm text-gray-400">Taux Réponse</span>
                            <span class="block text-xl font-extrabold text-cyan-400 mt-1" id="detail-rate">192.0%</span>
                        </div>
                        
                        <div class="p-4 bg-gray-900 rounded-xl shadow-lg border border-gray-700">
                            <span class="block text-sm text-gray-400">Achat limité</span>
                            <span class="block text-xl font-extrabold text-white mt-1" id="detail-limit">2</span>
                        </div>
                    </div>

                    <div class="mb-4">
                        <h3 class="text-lg font-bold text-white mb-2">Description Produit</h3>
                        <p id="detail-description" class="text-sm text-gray-400 leading-relaxed">
                            La série C est un système de production d'énergie solaire domestique de Clearway. Ce produit peut vous aider à améliorer votre quotidien et à réduire vos factures d'électricité. Il produit principalement de l'électricité de...
                        </p>
                    </div>
                    
                </div>
                
                <div class="p-4 w-full bg-gray-900 border-t border-gray-700 flex space-x-4 flex-shrink-0">
                    <button onclick="handlePurchaseWithPandaBank()" 
                            id="purchase-panda-bank-btn"
                            class="flex-1 py-3 rounded-lg font-bold text-lg text-white bg-purple-600/90 hover:bg-purple-600 shadow-lg shadow-purple-600/30">
                        Déposé sur le Bank
                    </button>
                    <button class="flex-1 py-3 rounded-lg font-bold text-lg text-white bg-cyan-600/90 hover:bg-cyan-600 shadow-lg shadow-cyan-600/30">
                        Solde Recharge
                    </button>
                </div>
                
            </div>

            <div id="section-service" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
                
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('home')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Centre de Service</h2>
                </div>

                <div class="p-4 flex-grow overflow-y-auto space-y-6">
                    
                    <div class="card p-6 rounded-xl border-t-4 border-cyan-500 text-center">
                        <svg class="w-16 h-16 mx-auto mb-4 text-cyan-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 8h2a2 2 0 012 2v6a2 2 0 01-2 2h-2v4l-4-4H9a1.994 1.994 0 01-1.414-.586M17 8V6a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2h2M7 12h4m-4 4h4m6 0h.01"></path></svg>
                        <h3 class="text-2xl font-bold text-white mb-2">Besoin d'aide ?</h3>
                        <p class="text-gray-400 mb-6">Contactez notre service client pour toute question, problème de transaction, ou assistance.</p>
                        
                        <a href="https://wa.me/22666867654" target="_blank"
                           class="inline-flex items-center justify-center w-full py-3 rounded-lg font-bold text-lg text-white 
                                  bg-green-600 hover:bg-green-700 transition duration-150 shadow-lg shadow-green-600/30">
                            <svg class="w-6 h-6 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.669A9.76 9.76 0 013 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"></path></svg>
                            WhatsApp : +226 66867654
                        </a>
                    </div>
                    
                    <div class="card p-4 rounded-xl border border-gray-700">
                        <h3 class="text-lg font-bold text-white mb-3">Informations de Contact</h3>
                        <div class="space-y-3 text-sm">
                            <div class="flex justify-between p-2 bg-gray-700 rounded-lg">
                                <span class="text-gray-400">Canal :</span>
                                <span class="text-white font-medium">Télégramme (Exemple)</span>
                            </div>
                            <div class="flex justify-between p-2 bg-gray-700 rounded-lg">
                                <span class="text-gray-400">E-mail :</span>
                                <span class="text-white font-medium">support@pandabf.com</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="p-4 text-center text-gray-500 border border-dashed border-gray-700 rounded-xl">
                        <p class="text-sm font-medium">Horaires de service : Lundi - Samedi, 8h00 - 18h00 (GMT)</p>
                    </div>

                </div>
            </div>
            
            <div id="section-wallet" class="section-content w-full hidden px-4 dark-screen">
                
                <div class="card wallet-card p-6 sm:p-8 rounded-xl w-full mb-4 mt-4"> 
                    
                    <h1 class="text-xl font-semibold text-gray-300 mb-2 text-center">Portefeuille Revenus</h1>
                    
                    <div class="text-center mb-6">
                        <span class="text-3xl font-extrabold text-cyan" id="walletBalance">0.00</span> 
                        <span class="text-lg font-semibold text-gray-400"> FCFA</span>
                    </div>

                    <div class="flex justify-center items-center mb-10 text-sm font-medium">
                        <span class="text-gray-400 mr-2">Portefeuille Recharge</span>
                        <span class="text-gray-100 font-bold" id="rechargeBalance">0.00</span>
                        <span class="text-gray-400 ml-1">FCFA</span>
                    </div>

                    <div class="flex space-x-4 mt-4 mb-4">
                        <button onclick="showSection('recharge')" class="flex-1 flex justify-center items-center space-x-2 py-3 rounded-lg font-semibold transition duration-150 ease-in-out 
                                       bg-gradient-to-r from-teal-400 to-purple-500 text-white shadow-lg shadow-purple-500/30 hover:shadow-purple-500/50">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z"></path></svg>
                            <span>Recharger</span>
                        </button>
                        <button onclick="showSection('withdrawal')" class="flex-1 flex justify-center items-center space-x-2 py-3 rounded-lg font-semibold transition duration-150 ease-in-out 
                                       bg-gray-700 text-white shadow-md shadow-gray-700/30 hover:bg-gray-600">
                            <svg class="w-5 h-5 transform -rotate-45" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"></path></svg>
                            <span>Retrait</span>
                        </button>
                    </div>
                </div>
                
                <div class="w-full space-y-4">
                    
                    <div class="space-y-4 text-sm font-medium bg-gray-800 p-4 rounded-xl shadow-lg mb-8 border border-gray-700">
                        
                        <h2 class="text-sm font-semibold text-gray-300 uppercase tracking-wider mb-4">HISTORIQUE & STATS</h2>
                        
                        <div onclick="showSection('withdrawal-history')" class="flex justify-between items-center border-b border-gray-700 pb-3 cursor-pointer hover:bg-gray-700/50 p-2 -mx-2 rounded">
                            <span class="text-gray-200">Historique des retraits</span>
                            <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                        </div>
                        <div onclick="showSection('fund-history')" class="flex justify-between items-center pb-3 cursor-pointer hover:bg-gray-700/50 p-2 -mx-2 rounded">
                            <span class="text-gray-200">Historique des fonds</span>
                            <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                        </div>
                        <div class="flex justify-between items-center pt-2">
                            <span class="text-gray-200">PandaBank</span>
                            <span class="text-green-500 font-bold" id="pandaBankAmount">0</span>
                        </div>
                        
                        <hr class="border-gray-700 my-4">
                        
                        <div class="grid grid-cols-2 gap-3 text-center">
                            <div class="p-3 bg-gray-900 rounded-lg border border-gray-700">
                                <span class="block text-xl font-extrabold text-cyan" id="dailyRevenue">0.00</span>
                                <span class="block text-xs text-gray-400">Revenu Aujourd'hui</span>
                            </div>
                            <div class="p-3 bg-gray-900 rounded-lg border border-gray-700">
                                <span class="block text-xl font-extrabold text-cyan" id="totalRevenue">0.00</span>
                                <span class="block text-xs text-gray-400">Revenu Total</span>
                            </div>
                            <div class="p-3 bg-gray-900 rounded-lg border border-gray-700">
                                <span class="block text-xl font-extrabold text-yellow-600" id="totalRecharge">0.00</span>
                                <span class="block text-xs text-gray-400">Recharge Totale</span>
                            </div>
                            <div class="p-3 bg-gray-900 rounded-lg border border-gray-700">
                                <span class="block text-xl font-extrabold text-red-600" id="totalWithdrawals">0.00</span>
                                <span class="block text-xs text-gray-400">Total Retraits</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div id="section-recharge" class="section-content w-full hidden dark-screen">
                
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('wallet')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Recharger</h2>
                </div>

                <div class="p-4 sm:p-8">
                    <div class="mb-8">
                        <label for="rechargeAmountInput" class="block text-lg font-medium text-gray-300 mb-4">Veuillez saisir le montant</label>
                        <div class="relative rounded-lg border border-gray-600 focus-within:border-amber-500">
                            <input type="number" id="rechargeAmountInput"
                                oninput="selectAmount(this.value, true)"
                                class="w-full pl-4 pr-12 py-3 bg-transparent text-2xl font-bold text-white focus:outline-none"
                                placeholder="0" min="1000">
                            <div class="absolute inset-y-0 right-0 flex items-center pr-3 pointer-events-none">
                                <span class="text-gray-400 sm:text-xl">FCFA</span>
                            </div>
                        </div>
                    </div>

                    <div class="mb-10">
                        <p class="block text-sm font-medium text-gray-300 mb-3">Sélectionner Le Montant</p>
                        <div class="grid grid-cols-3 gap-3">
                            <button class="amount-btn border border-gray-600 text-gray-300 py-2 rounded-lg transition duration-150" 
                                    data-amount="5000" onclick="selectAmount(5000)">5000</button>
                            <button class="amount-btn border border-gray-600 text-gray-300 py-2 rounded-lg transition duration-150" 
                                    data-amount="20000" onclick="selectAmount(20000)">20000</button>
                            <button class="amount-btn border border-gray-600 text-gray-300 py-2 rounded-lg transition duration-150" 
                                    data-amount="60000" onclick="selectAmount(60000)">60000</button>
                            <button class="amount-btn border border-gray-600 text-gray-300 py-2 rounded-lg transition duration-150" 
                                    data-amount="150000" onclick="selectAmount(150000)">150000</button>
                            <button class="amount-btn border border-gray-600 text-gray-300 py-2 rounded-lg transition duration-150" 
                                    data-amount="390000" onclick="selectAmount(390000)">390000</button>
                        </div>
                    </div>

                    <div class="mb-10">
                        <p class="block text-sm font-medium text-gray-300 mb-3">Méthodes Paiement</p>
                        <div class="space-y-3">
                            <label class="flex items-center p-3 bg-gray-700 rounded-lg cursor-pointer">
                                <input type="radio" name="paymentMethod" value="om" checked 
                                       class="form-radio h-5 w-5 text-amber-500 border-gray-500 focus:ring-amber-500 transition duration-150">
                                <span class="ml-3 text-white font-semibold">Orange Money</span>
                            </label>
                            <label class="flex items-center p-3 bg-gray-700 rounded-lg cursor-pointer">
                                <input type="radio" name="paymentMethod" value="moov" 
                                       class="form-radio h-5 w-5 text-amber-500 border-gray-500 focus:ring-amber-500 transition duration-150">
                                <span class="ml-3 text-white font-semibold">Moov Money</span>
                            </label>
                             <label class="flex items-center p-3 bg-gray-700 rounded-lg cursor-pointer">
                                <input type="radio" name="paymentMethod" value="cwen" 
                                       class="form-radio h-5 w-5 text-amber-500 border-gray-500 focus:ring-amber-500 transition duration-150">
                                <span class="ml-3 text-white font-semibold">CWEN</span>
                            </label>
                        </div>
                    </div>

                    <div class="p-4 bg-gray-700 rounded-lg mb-8">
                        <h3 class="text-sm font-semibold text-gray-300 mb-2">Instructions De Recharge:</h3>
                        <ul class="list-disc list-inside text-xs text-gray-400 space-y-2">
                            <li>Le montant minimum du dépôt par transaction est de **1000 FCFA**.</li>
                            <li>Le montant maximum du dépôt par transaction est de **1900000 FCFA**.</li>
                            <li>Après avoir cliqué sur Confirmer, le code USSD s'ouvrira dans l'application d'appel de votre téléphone.</li>
                            <li>Suivez les étapes de confirmation pour valider le paiement.</li>
                            <li>Si vous rencontrez des problèmes, veuillez contacter le service client.</li>
                        </ul>
                    </div>

                    <button onclick="handleRecharge()"
                            class="w-full py-4 rounded-xl font-bold text-lg 
                                   bg-gradient-to-r from-teal-400 to-purple-500 text-white 
                                   shadow-lg shadow-purple-500/30 hover:shadow-purple-500/50 transition duration-150">
                        Confirmer
                    </button>
                </div>
            </div>
            
            <div id="section-withdrawal" class="section-content w-full hidden flex flex-col dark-screen">
                
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('wallet')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Retrait</h2>
                </div>

                <div class="withdrawal-content flex flex-col flex-grow">
                    
                    <div class="flex justify-between items-center p-4 rounded-lg bg-gray-900 mb-8 shadow-xl">
                        <div>
                            <span class="block text-sm text-gray-400">Solde Revenu</span>
                            <span class="block text-3xl font-extrabold text-cyan" id="withdrawalBalanceDisplay">0.00</span>
                        </div>
                        <span class="text-base text-gray-400">FCFA</span>
                         <div class="w-8 h-8 rounded-full bg-green-500 flex items-center justify-center text-white">
                             <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M12 4.354a4 4 0 000 5.292m7 5.292A7 7 0 015 10.5M19 15.054A7 7 0 015 10.5"></path></svg>
                        </div>
                    </div>

                    <div class="mb-4">
                        <label for="withdrawalAmountInput" class="block text-lg font-medium text-gray-300 mb-4">Veuillez saisir le montant</label>
                        <div class="relative">
                            <input type="number" id="withdrawalAmountInput"
                                oninput="updateWithdrawalDetails(this.value)"
                                class="w-full pr-24 py-3 withdrawal-input-field focus:outline-none"
                                placeholder="|" min="1000">
                            <div class="absolute inset-y-0 right-0 flex items-center pr-3 pointer-events-none">
                                <span class="text-gray-400 sm:text-xl">FCFA</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="mb-8 text-sm text-gray-400 space-y-2">
                        <p>Réception sous **24-72 heures**, Frais <span class="text-red-400 font-bold">7%</span></p>
                        
                        <div class="flex justify-between items-center p-3 bg-gray-800 rounded-lg cursor-pointer mt-4">
                            <div class="flex items-center space-x-3">
                                <svg class="w-6 h-6 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z"></path></svg>
                                <div>
                                    <span class="block text-sm font-semibold text-white">Argent Mobile</span>
                                    <span class="block text-xs text-gray-400" id="withdrawalMethodDisplay">ORANGE 76343106</span>
                                </div>
                            </div>
                            <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                        </div>
                    </div>

                    <div class="p-4 bg-gray-700 rounded-lg mb-8 mt-auto">
                        <h3 class="text-sm font-semibold text-gray-300 mb-2">Instructions De Retrait:</h3>
                        <ul class="list-decimal list-inside text-xs text-gray-400 space-y-2">
                            <li>Le montant minimum de retrait est de **1000 FCFA**.</li>
                            <li>Le montant maximum de retrait est de **1500000 FCFA**.</li>
                            <li>Les deux derniers chiffres du montant du retrait doivent être **0** (par exemple: 1000 FCFA, 9900 FCFA, 99900 FCFA).</li>
                            <li>Des frais bancaires de **7%** seront facturés pour chaque retrait. (Le montant réel reçu est calculé automatiquement ci-dessous).</li>
                        </ul>
                        <div id="withdrawal-summary" class="mt-4 p-3 bg-gray-600 rounded-lg">
                            <p class="text-sm text-white font-medium">Montant demandez: <span id="requestedAmount">0</span> FCFA</p>
                            <p class="text-sm text-red-300 font-medium">Frais (7%): <span id="feeAmount">0</span> FCFA</p>
                            <p class="text-base text-green-300 font-extrabold mt-1">Montant Réel Reçu: <span id="receivedAmount">0</span> FCFA</p>
                        </div>
                    </div>
                    
                    <button onclick="handleWithdrawal()"
                            class="w-full py-4 rounded-xl font-bold text-lg 
                                   bg-gradient-to-r from-teal-400 to-purple-500 text-white 
                                   shadow-lg shadow-purple-500/30 hover:shadow-purple-500/50 transition duration-150">
                        Retrait
                    </button>
                </div>
            </div>
            
            <div id="section-withdrawal-history" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
                
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('wallet')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Historique des retraits</h2>
                </div>

                <div class="p-4 flex-grow overflow-y-auto space-y-6">
                    
                    <div class="p-8 text-center text-gray-500 border border-dashed border-gray-700 rounded-xl">
                        <svg class="w-16 h-16 mx-auto mb-3 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 13h6m-3-3v6m-9 1a8 8 0 1116 0A8 8 0 013 14z"></path></svg>
                        <p class="text-lg font-semibold">Aucun enregistrement de retrait trouvé.</p>
                        <p class="text-sm mt-1">Les retraits réussis et en attente apparaîtront ici.</p>
                    </div>

                </div>
            </div>
            
            <div id="section-fund-history" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
                
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('wallet')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Historique des fonds</h2>
                </div>

                <div id="section-fund-history-content" class="p-4 flex-grow overflow-y-auto space-y-2">
                    
                    <div class="p-3 bg-gray-900 rounded-lg flex justify-between items-center border border-gray-700">
                        <div>
                             <h3 class="font-bold text-white text-lg">Revenus du jour</h3>
                             <span class="text-sm text-gray-400">10/11/2025</span>
                        </div>
                        <span class="text-green-400 text-2xl font-extrabold">+0.00</span>
                    </div>
                    
                    <div class="pt-4 pb-2">
                        <h3 class="text-lg font-bold text-gray-300">Détails des transactions</h3>
                    </div>

                    <div class="flex items-center p-3 card rounded-lg">
                        <div class="flex-shrink-0 w-8 h-8 rounded-full bg-cyan-600/30 flex items-center justify-center mr-3">
                            <svg class="w-5 h-5 text-cyan-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c1.657 0 3 .895 3 2s-1.343 2-3 2v2m0 0h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                        </div>
                        <div class="flex-grow">
                             <p class="text-sm font-medium text-white">Revenus de distribution</p>
                             <span class="text-xs text-gray-400">10/11/2025</span>
                        </div>
                        <span class="text-sm font-semibold text-green-500">+0.00</span>
                    </div>

                    <div class="flex items-center p-3 card rounded-lg">
                        <div class="flex-shrink-0 w-8 h-8 rounded-full bg-amber-600/30 flex items-center justify-center mr-3">
                            <svg class="w-5 h-5 text-amber-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 9V7a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2m2 4h10a2 2 0 002-2v-6a2 2 0 00-2-2H9a2 2 0 00-2 2v6a2 2 0 002 2z"></path></svg>
                        </div>
                        <div class="flex-grow">
                             <p class="text-sm font-medium text-white">Revenus des commandes</p>
                             <span class="text-xs text-gray-400">10/11/2025</span>
                        </div>
                        <span class="text-sm font-semibold text-green-500">+0.00</span>
                    </div>
                    
                    <div class="flex items-center p-3 card rounded-lg">
                        <div class="flex-shrink-0 w-8 h-8 rounded-full bg-red-600/30 flex items-center justify-center mr-3">
                            <svg class="w-5 h-5 text-red-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 17h6m-6-4h6m-6-4h6m-7 4l-4 4m0 0l-4-4"></path></svg>
                        </div>
                        <div class="flex-grow">
                             <p class="text-sm font-medium text-white">Retrait (Déduit)</p>
                             <span class="text-xs text-gray-400">08/11/2025</span>
                        </div>
                        <span class="text-sm font-semibold text-red-500">-0.00</span>
                    </div>
                    
                </div>
                
                <button onclick="scrollToTop('section-fund-history-content')"
                        class="fixed bottom-24 right-4 w-12 h-12 bg-amber-500 rounded-full flex items-center justify-center text-white shadow-xl hover:bg-amber-600 transition duration-300 z-20">
                    <svg class="w-6 h-6 transform rotate-180" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                </button>
            </div>
            
            <div id="section-invite" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
    
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('home')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Inviter Des Amis</h2>
                </div>

                <div class="p-4 flex-grow overflow-y-auto space-y-6">
                    
                    <div class="card p-6 rounded-xl border-t-4 border-amber-500 text-center">
                        <h3 class="text-xl font-bold text-white mb-2">Votre Code d'Invitation</h3>
                        <p class="text-gray-400 mb-4">Partagez ce code avec vos amis pour gagner des récompenses !</p>
                        
                        <div class="relative bg-gray-700 p-4 rounded-lg flex items-center justify-between shadow-inner">
                            <span id="invitationCode" class="text-3xl font-extrabold text-amber-400 select-all">PANDA66867654</span>
                            <button onclick="copyInvitationCode()" class="bg-amber-500 hover:bg-amber-600 text-white font-semibold py-1 px-3 rounded-full text-sm transition duration-150">
                                Copier
                            </button>
                        </div>
                    </div>
                    
                    <div class="card p-6 rounded-xl border border-gray-700">
                        <h3 class="text-lg font-bold text-white mb-4">Historique des Codes Renvoyés</h3>
                        
                        <div id="returned-codes-list" class="space-y-3">
                            <div class="p-3 bg-gray-700 rounded-lg flex justify-between items-center text-sm">
                                <span class="text-white font-medium">Code Renvoyé (Exemple):</span>
                                <span class="text-green-400 font-semibold">SUCCESS</span>
                            </div>
                        </div>
                        
                        <div class="mt-4 pt-4 border-t border-gray-700">
                             <button onclick="simulateReturnAllCodes()" class="w-full bg-cyan-600 hover:bg-cyan-700 text-white font-semibold py-2 rounded-lg transition duration-150">
                                Simuler: Renvoyer Tous les Codes
                            </button>
                        </div>
                    </div>
                    
                    <div class="card p-4 rounded-xl border border-gray-700">
                        <h3 class="text-lg font-bold text-white mb-3">Règles d'Invitation</h3>
                        <ul class="list-disc list-inside text-sm text-gray-400 space-y-2">
                            <li>Chaque ami qui s'inscrit avec votre code vous rapporte un bonus de **X FCFA**.</li>
                            <li>Vous recevez une commission sur les investissements de vos filleuls (3 niveaux).</li>
                            <li>Le nombre maximum d'invitations est illimité.</li>
                        </ul>
                    </div>

                </div>
            </div>
            
             <div id="section-exchange" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
                
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('home')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Échanger Un Code</h2>
                </div>

                <div class="p-4 flex-grow overflow-y-auto space-y-6">
                    
                    <div class="card p-6 rounded-xl border-t-4 border-green-500 text-center">
                        <svg class="w-16 h-16 mx-auto mb-4 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-14-11l-3 3m0 0h5m-5 0v5M16 5l3-3m0 0h-5m5 0v5"></path></svg>
                        <h3 class="text-xl font-bold text-white mb-2">Entrez Votre Code Cadeau</h3>
                        <p class="text-gray-400 mb-6">Recevez instantanément des fonds ou des produits avec un code d'échange.</p>
                        
                        <div class="relative mb-4">
                            <input type="text" id="exchangeCodeInput" placeholder="Saisir le code ici (Ex: GIFT-XXXX)"
                                   class="w-full px-4 py-3 border border-gray-700 rounded-lg focus:ring-green-500 focus:border-green-500 bg-gray-700 text-white text-center uppercase tracking-widest font-bold">
                        </div>
                        
                        <button onclick="handleCodeExchange()"
                                class="inline-flex items-center justify-center w-full py-3 rounded-lg font-bold text-lg text-white 
                                       bg-green-600 hover:bg-green-700 transition duration-150 shadow-lg shadow-green-600/30">
                            Valider le Code
                        </button>
                    </div>
                    
                    <div class="card p-4 rounded-xl border border-gray-700">
                        <h3 class="text-lg font-bold text-white mb-3">Codes Non Échangés (<span id="unredeemedCount">0</span>)</h3>
                        <div id="unredeemed-codes-list" class="space-y-3 text-sm">
                             <p class="text-gray-500 text-center" id="no-unredeemed-msg">Vous n'avez aucun code d'échange en attente.</p>
                        </div>
                    </div>
                    
                    <div class="card p-4 rounded-xl border border-gray-700">
                        <h3 class="text-lg font-bold text-white mb-3">Codes Échangés (Historique)</h3>
                        <div id="redeemed-codes-list" class="space-y-3 text-sm">
                            <div class="p-3 bg-gray-700 rounded-lg flex justify-between items-center">
                                <span class="text-white font-medium">GIFT-001</span>
                                <span class="text-xs text-green-400">Réglé (500 F)</span>
                            </div>
                        </div>
                    </div>

                </div>
            </div>

            
            <div id="section-team" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
    
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('home')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Mon Équipe</h2>
                </div>

                <div class="p-4 flex-grow overflow-y-auto space-y-4">
                    
                    <div class="card p-4 rounded-xl border-t-4 border-cyan-500">
                        <div class="flex justify-between items-center mb-4">
                            <h3 class="text-lg font-bold text-white">Statistiques de l'Équipe</h3>
                             <span class="bg-cyan-600 text-white text-xs font-bold px-2 py-1 rounded-full">Niveau 1</span>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-3 text-center">
                            <div class="p-3 bg-gray-900 rounded-lg border border-gray-700">
                                <span class="block text-xl font-extrabold text-white" id="teamTotalMembers">0</span>
                                <span class="block text-xs text-gray-400">Membres Totaux</span>
                            </div>
                            <div class="p-3 bg-gray-900 rounded-lg border border-gray-700">
                                <span class="block text-xl font-extrabold text-green-400" id="teamTotalRecharge">0.00</span>
                                <span class="block text-xs text-gray-400">Recharge Équipe (F)</span>
                            </div>
                        </div>
                    </div>
                    
                    <h3 class="text-lg font-bold text-white mt-6 mb-3">Membres (Niveau 1)</h3>
                    
                    <div id="team-list" class="space-y-3">
                        <div class="p-4 text-center text-gray-500 border border-dashed border-gray-700 rounded-xl">
                            <p class="text-sm font-medium">Chargement des membres de l'équipe...</p>
                        </div>
                    </div>

                </div>
            </div>
            
            <div id="section-account" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
                
                <div class="flex items-center p-4 bg-gray-900 sticky top-0 z-10">
                    <button onclick="showSection('home')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-semibold text-white">Mon Compte</h2>
                </div>
                
                <div class="p-4 flex-grow overflow-y-auto">

                    <div class="card bg-gray-800 p-6 sm:p-8 rounded-xl border-t-4 border-amber-500 w-full mb-8">
                        <h1 class="text-3xl font-bold text-white mb-6 text-center">👤 Profil Utilisateur</h1>
                        
                        <div class="space-y-4 mb-8">
                            <div class="p-3 bg-gray-700 rounded-lg border border-gray-600">
                                <span class="block text-sm font-medium text-gray-400">Numéro de Téléphone :</span>
                                <span class="block text-lg font-semibold text-white" id="userPhoneNumber">+226 00 00 00 00</span>
                            </div>
                            <div class="p-3 bg-gray-700 rounded-lg border border-gray-600">
                                <span class="block text-sm font-medium text-gray-400">Statut :</span>
                                <span class="block text-lg font-semibold text-green-500">✅ Actif</span>
                            </div>
                        </div>
                        
                        <div class="space-y-3 pt-4 border-t border-gray-700">
                            <button onclick="showSection('modify-password')" class="w-full text-left p-3 rounded-lg flex justify-between items-center bg-gray-700 hover:bg-gray-600 transition duration-150">
                                <span class="text-white font-medium">Modifier le Mot de Passe</span>
                                <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                            </button>
                            <button onclick="showSection('bind-mobile-money')" class="w-full text-left p-3 rounded-lg flex justify-between items-center bg-gray-700 hover:bg-gray-600 transition duration-150">
                                <span class="text-white font-medium">Lier Argent Mobile (Retrait)</span>
                                <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                            </button>
                            <button onclick="showSection('service-center')" class="w-full text-left p-3 rounded-lg flex justify-between items-center bg-gray-700 hover:bg-gray-600 transition duration-150">
                                <span class="text-white font-medium">Centre de Service Client</span>
                                <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                            </button>
                        </div>
                    </div>
                    
                    <button onclick="logout()"
                            class="w-full bg-red-500 text-white py-3 rounded-lg font-semibold hover:bg-red-600 transition duration-150 ease-in-out focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 mt-4">
                        Déconnexion
                    </button>
                    
                    <div class="h-16"></div> 
                </div>
            </div>
            
            <div id="section-admin" class="section-content w-full hidden dark-screen flex flex-col min-h-screen">
                
                 <div class="flex items-center p-4 bg-red-800 sticky top-0 z-10">
                    <button onclick="showSection('home')" class="text-white mr-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                    </button>
                    <h2 class="text-xl font-bold text-white">🔒 Panneau Admin (66867654)</h2>
                </div>

                <div class="p-4 flex-grow overflow-y-auto space-y-6">
                    
                    <div class="bg-gray-800 p-4 rounded-xl border border-cyan-700">
                        <h3 class="text-lg font-bold text-cyan-400 mb-4">Créer un Code d'Échange</h3>
                        <div class="space-y-4">
                            <div>
                                <label for="newCodeAmount" class="block text-sm font-medium text-gray-300">Montant (FCFA)</label>
                                <input type="number" id="newCodeAmount" placeholder="Ex: 500" min="100" 
                                       class="w-full px-3 py-2 border border-gray-700 rounded-lg bg-gray-700 text-white" value="500">
                            </div>
                            <button onclick="adminCreateExchangeCode()" class="w-full bg-cyan-600 hover:bg-cyan-700 text-white font-semibold py-2 rounded-lg transition duration-150">
                                Générer le Code
                            </button>
                            <div id="generatedCodeDisplay" class="p-3 bg-gray-900 rounded-lg text-center hidden">
                                <span class="block text-xs text-gray-400">Code Généré:</span>
                                <span id="latestCode" class="text-xl font-extrabold text-amber-400 select-all"></span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-gray-800 p-4 rounded-xl border border-red-700">
                        <h3 class="text-lg font-bold text-red-400 mb-4">Validation des Transactions (Retraits)</h3>
                        <div id="pending-transactions" class="space-y-3">
                            <div id="trx-1" class="p-3 bg-gray-700 rounded-lg flex justify-between items-center">
                                <div>
                                    <span class="block text-sm font-bold text-white">Retrait 5,000 F</span>
                                    <span class="block text-xs text-gray-400">User: +226 77001122</span>
                                    <span class="block text-xs text-amber-400">Méthode: Orange Money</span>
                                </div>
                                <button onclick="validateTransaction('trx-1')" class="bg-green-600 hover:bg-green-700 text-white font-semibold py-1 px-3 rounded-lg text-sm transition duration-150">
                                    Valider
                                </button>
                            </div>
                            <div id="trx-2" class="p-3 bg-gray-700 rounded-lg flex justify-between items-center">
                                <div>
                                    <span class="block text-sm font-bold text-white">Retrait 15,000 F</span>
                                    <span class="block text-xs text-gray-400">User: +226 66998877</span>
                                    <span class="block text-xs text-amber-400">Méthode: Moov Money</span>
                                </div>
                                <button onclick="validateTransaction('trx-2')" class="bg-green-600 hover:bg-green-700 text-white font-semibold py-1 px-3 rounded-lg text-sm transition duration-150">
                                    Valider
                                </button>
                            </div>
                            
                            <div id="no-pending-trx" class="p-4 text-center text-gray-500 bg-gray-900 rounded-lg hidden">
                                <p>✅ Aucune transaction en attente de validation.</p>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-gray-800 p-4 rounded-xl border border-red-700">
                        <h3 class="text-lg font-bold text-red-400 mb-4">Liste des Utilisateurs</h3>
                        <div class="space-y-3" id="user-list">
                            <div id="user-1" class="p-3 bg-gray-700 rounded-lg flex justify-between items-center">
                                <div>
                                    <span class="block text-sm font-bold text-white">ID: #001 - Bamba</span>
                                    <span class="block text-xs text-gray-400">Num: +226 77001122</span>
                                    <span class="block text-xs text-green-400">Solde: 5500 F</span>
                                </div>
                                <button onclick="editUser('user-1')" class="bg-amber-600 hover:bg-amber-700 text-white font-semibold py-1 px-3 rounded-lg text-sm transition duration-150">
                                    Modifier
                                </button>
                            </div>
                            <div id="user-2" class="p-3 bg-gray-700 rounded-lg flex justify-between items-center">
                                <div>
                                    <span class="block text-sm font-bold text-white">ID: #002 - Diarra</span>
                                    <span class="block text-xs text-gray-400">Num: +226 66998877</span>
                                    <span class="block text-xs text-green-400">Solde: 12000 F</span>
                                </div>
                                <button onclick="editUser('user-2')" class="bg-amber-600 hover:bg-amber-700 text-white font-semibold py-1 px-3 rounded-lg text-sm transition duration-150">
                                    Modifier
                                </button>
                            </div>
                            <div id="user-admin" class="p-3 bg-red-700 rounded-lg flex justify-between items-center">
                                <div>
                                    <span class="block text-sm font-bold text-white">ID: #000 - Admin</span>
                                    <span class="block text-xs text-gray-200">Num: +226 66867654</span>
                                    <span class="block text-xs text-green-400">Solde: N/A</span>
                                </div>
                                <button disabled class="bg-gray-500 text-white font-semibold py-1 px-3 rounded-lg text-sm cursor-not-allowed">
                                    Admin
                                </button>
                            </div>
                        </div>
                    </div>
                    
                </div>
            </div>
            
        </div>
        </div>

    <nav id="navbar" class="fixed bottom-0 left-0 w-full bg-gray-800 shadow-lg border-t border-gray-700 z-20 hidden-until-auth">
        <div class="flex justify-around items-center h-16 max-w-lg mx-auto">
            
            <button id="nav-home" onclick="showSection('home')" 
                    class="nav-item flex flex-col items-center justify-center text-xs p-2 
                           text-amber-600 font-semibold transition duration-150">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"></path></svg>
                Accueil
            </button>
            
            <button id="nav-simulator" onclick="showSection('simulator')" class="nav-item flex flex-col items-center justify-center text-xs text-gray-400 transition duration-150 p-2">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"></path></svg>
                Produits
            </button>
            
            <button id="nav-wallet" onclick="showSection('wallet')" class="nav-item flex flex-col items-center justify-center text-xs text-gray-400 transition duration-150 p-2">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z"></path></svg>
                Portefeuille
            </button>
            
            <button id="nav-account" onclick="showSection('account')" class="nav-item flex flex-col items-center justify-center text-xs text-gray-400 transition duration-150 p-2">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                Compte
            </button>
            
            <button id="nav-admin" onclick="showSection('admin')" class="nav-item hidden flex-col items-center justify-center text-xs text-red-500 transition duration-150 p-2">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35-.426.24-1.05.65-.91 1.722a1.724 1.724 0 001.066 2.573c1.543.94-.223 2.707-1.766 2.015a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35.426-.24 1.05-.65 0-1.722a1.724 1.724 0 00-1.066-2.573c-1.543-.94.223-2.707 1.766-2.015a1.724 1.724 0 002.572-1.065z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
                Admin
            </button>
        </div>
    </nav>
    
    <div id="toast" class="fixed bottom-24 left-1/2 transform -translate-x-1/2 p-3 rounded-xl shadow-2xl transition-opacity duration-300 opacity-0 z-50 text-white font-semibold">
        </div>

    <script>
        // --- DONNÉES SIMULÉES ---
        let isAuthenticated = false;
        let isAdmin = false;
        const ADMIN_PHONE = "+22666867654";
        const ADMIN_PASS = "admin123";
        let userPhone = "";
        let userBalance = 0; // Solde Revenu (Wallet)
        let rechargeBalance = 0; // Solde Recharge

        // Codes d'échange (simulés en local)
        let exchangeCodes = [
            { code: "GIFT-001", amount: 500, redeemedBy: [] }, // Code de base
            { code: "PANDA-1000", amount: 1000, redeemedBy: [] },
        ];
        
        let userRedeemedCodes = localStorage.getItem('userRedeemedCodes') ? JSON.parse(localStorage.getItem('userRedeemedCodes')) : [];
        let userUnredeemedCodes = localStorage.getItem('userUnredeemedCodes') ? JSON.parse(localStorage.getItem('userUnredeemedCodes')) : [];

        // --- FONCTIONS UTILITAIRES ---

        function formatCurrency(amount) {
            return parseFloat(amount).toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        }

        function showToast(message, isError = false) {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.className = 'fixed bottom-24 left-1/2 transform -translate-x-1/2 p-3 rounded-xl shadow-2xl transition-opacity duration-300 z-50 text-white font-semibold';
            toast.classList.add(isError ? 'bg-red-500' : 'bg-green-500');
            toast.classList.remove('opacity-0');
            
            setTimeout(() => {
                toast.classList.add('opacity-0');
            }, 3000);
        }

        function updateUI() {
            document.getElementById('currentBalance').textContent = formatCurrency(userBalance);
            document.getElementById('walletBalance').textContent = formatCurrency(userBalance);
            document.getElementById('rechargeBalance').textContent = formatCurrency(rechargeBalance);
            document.getElementById('withdrawalBalanceDisplay').textContent = formatCurrency(userBalance);
            
            // Mise à jour des messages d'accueil pour les codes non échangés
            const unredeemedCount = userUnredeemedCodes.length;
            document.getElementById('messageStatus').textContent = unredeemedCount;
            document.getElementById('messageLabel').textContent = unredeemedCount > 0 ? 'NOUVEAU' : 'PAS DE MESSAGE';
            if (unredeemedCount > 0) {
                 document.getElementById('messageStatus').classList.add('text-red-500');
                 document.getElementById('messageLabel').classList.add('text-red-600');
                 document.getElementById('messageStatus').classList.remove('text-amber-500');
                 document.getElementById('messageLabel').classList.remove('text-amber-600');
            } else {
                 document.getElementById('messageStatus').classList.remove('text-red-500');
                 document.getElementById('messageLabel').classList.remove('text-red-600');
                 document.getElementById('messageStatus').classList.add('text-amber-500');
                 document.getElementById('messageLabel').classList.add('text-amber-600');
            }

            // Mise à jour de l'affichage des codes d'échange
            updateExchangeCodeLists();
        }

        function updateExchangeCodeLists() {
            const unredeemedList = document.getElementById('unredeemed-codes-list');
            const redeemedList = document.getElementById('redeemed-codes-list');
            const unredeemedCountEl = document.getElementById('unredeemedCount');
            const noUnredeemedMsg = document.getElementById('no-unredeemed-msg');

            unredeemedList.innerHTML = '';
            redeemedList.innerHTML = '';
            
            // Masquer le message d'absence par défaut
            noUnredeemedMsg.classList.add('hidden');

            userUnredeemedCodes.forEach(item => {
                const div = document.createElement('div');
                div.className = 'p-3 bg-gray-700 rounded-lg flex justify-between items-center';
                div.innerHTML = `
                    <span class="text-white font-medium">${item.code}</span>
                    <span class="text-xs text-green-400">Valeur: ${formatCurrency(item.amount)} F</span>
                `;
                unredeemedList.appendChild(div);
            });
            
            if (userUnredeemedCodes.length === 0) {
                 noUnredeemedMsg.classList.remove('hidden');
            }

            userRedeemedCodes.forEach(item => {
                const div = document.createElement('div');
                div.className = 'p-3 bg-gray-700 rounded-lg flex justify-between items-center opacity-70';
                div.innerHTML = `
                    <span class="text-white font-medium">${item.code}</span>
                    <span class="text-xs text-red-400">Échangé (+${formatCurrency(item.amount)} F)</span>
                `;
                redeemedList.appendChild(div);
            });
            
            unredeemedCountEl.textContent = userUnredeemedCodes.length;

            // Mettre à jour la notification sur la page d'accueil
            const count = userUnredeemedCodes.length;
            document.getElementById('messageStatus').textContent = count;
            document.getElementById('messageLabel').textContent = count > 0 ? 'NOUVEAU' : 'CHECK';
        }

        // --- GESTION DE L'AUTHENTIFICATION ET DE L'AFFICHAGE ---
        
        document.getElementById('authForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const mode = document.getElementById('authMode').value;
            const phone = document.getElementById('countryCode').value + document.getElementById('phone').value.replace(/\s/g, '');
            const password = document.getElementById('password').value;

            if (mode === 'login' || mode === 'register') {
                if (phone === ADMIN_PHONE && password === ADMIN_PASS) {
                    isAuthenticated = true;
                    isAdmin = true;
                    userPhone = phone;
                    document.getElementById('userPhoneNumber').textContent = userPhone;
                    showSection('home');
                    document.getElementById('nav-admin').classList.remove('hidden');
                    showToast("Connexion Admin réussie!");
                } else if (password === "user123") { // Simulation de connexion utilisateur
                    isAuthenticated = true;
                    isAdmin = false;
                    userPhone = phone;
                    document.getElementById('userPhoneNumber').textContent = userPhone;
                    document.getElementById('nav-admin').classList.add('hidden');
                    showSection('home');
                    // Simuler des données utilisateur
                    userBalance = 1500;
                    rechargeBalance = 5000;
                    updateUI();
                    showToast("Connexion réussie!");
                } else {
                    showToast(mode === 'login' ? "Identifiants incorrects ou non trouvés." : "Erreur d'inscription simulée. Essayez une autre combinaison.", true);
                }
            }
        });

        function showSection(sectionId) {
            // Cacher toutes les sections de contenu
            document.querySelectorAll('.section-content').forEach(section => {
                section.classList.add('hidden');
            });
            
            // Afficher la section demandée
            const targetSection = document.getElementById('section-' + sectionId);
            if (targetSection) {
                targetSection.classList.remove('hidden');
                targetSection.scrollTop = 0; // Remettre le défilement en haut

                // Mettre à jour la navigation
                document.querySelectorAll('.nav-item').forEach(item => {
                    item.classList.remove('text-amber-600', 'font-semibold');
                    item.classList.add('text-gray-400');
                });
                
                const navItem = document.getElementById('nav-' + sectionId) || 
                                document.getElementById('nav-home'); // Fallback pour les sous-pages

                if (navItem) {
                    navItem.classList.add('text-amber-600', 'font-semibold');
                    navItem.classList.remove('text-gray-400');
                }
                
                 if (sectionId === 'exchange') {
                     updateExchangeCodeLists(); 
                 }
                 if (sectionId === 'wallet') {
                     updateUI();
                 }
            }
        }
        
        function logout() {
            isAuthenticated = false;
            isAdmin = false;
            userPhone = "";
            document.getElementById('authScreen').classList.remove('hidden');
            document.getElementById('mainContent').classList.add('hidden-until-auth');
            document.getElementById('navbar').classList.add('hidden-until-auth');
            showSection('auth');
            showToast("Déconnexion réussie.");
        }
        
        // Simuler le changement de statut d'authentification après le chargement initial
        if (!isAuthenticated) {
            document.getElementById('authScreen').classList.remove('hidden');
            document.getElementById('mainContent').classList.add('hidden-until-auth');
            document.getElementById('navbar').classList.add('hidden-until-auth');
        } else {
             document.getElementById('authScreen').classList.add('hidden');
             document.getElementById('mainContent').classList.remove('hidden-until-auth');
             document.getElementById('navbar').classList.remove('hidden-until-auth');
             showSection('home');
             updateUI();
        }

        // --- GESTION DE L'ÉCHANGE DE CODES (NOUVEAU) ---
        
        function handleCodeExchange() {
            const codeInput = document.getElementById('exchangeCodeInput');
            const code = codeInput.value.toUpperCase().trim();
            codeInput.value = '';

            if (!code) {
                showToast("Veuillez entrer un code.", true);
                return;
            }

            // 1. Vérifier si l'utilisateur a déjà échangé ce code
            const isRedeemed = userRedeemedCodes.some(c => c.code === code);
            if (isRedeemed) {
                showToast("Ce code a déjà été utilisé sur ce compte.", true);
                return;
            }

            // 2. Vérifier si le code est dans la liste des codes non échangés de l'utilisateur
            let codeIndex = -1;
            let codeObject = userUnredeemedCodes.find((item, index) => {
                if (item.code === code) {
                    codeIndex = index;
                    return true;
                }
                return false;
            });

            if (codeObject) {
                // Le code existe et est attribué à l'utilisateur
                userBalance += codeObject.amount;
                userRedeemedCodes.push(codeObject);
                userUnredeemedCodes.splice(codeIndex, 1); // Retirer de la liste non échangée
                
                // Sauvegarder les données
                localStorage.setItem('userRedeemedCodes', JSON.stringify(userRedeemedCodes));
                localStorage.setItem('userUnredeemedCodes', JSON.stringify(userUnredeemedCodes));

                showToast(`Code **${code}** échangé avec succès ! ${formatCurrency(codeObject.amount)} FCFA ajoutés au solde.`, false);
                updateUI();
                return;
            }
            
            // 3. Simuler la vérification d'un code universel (moins prioritaire, peut être activé si le code n'est pas trouvé)
            const universalCode = exchangeCodes.find(c => c.code === code);
            if (universalCode) {
                 // Un code universel (ou non attribué) est trouvé
                userBalance += universalCode.amount;
                userRedeemedCodes.push(universalCode); // Ajouter comme échangé
                
                // Sauvegarder les données
                localStorage.setItem('userRedeemedCodes', JSON.stringify(userRedeemedCodes));

                showToast(`Code **${code}** échangé avec succès ! ${formatCurrency(universalCode.amount)} FCFA ajoutés au solde.`, false);
                updateUI();
                return;
            }
            
            // 4. Code non trouvé ou invalide
            showToast("Code d'échange invalide ou expiré.", true);
        }

        // --- FONCTIONS ADMIN (NOUVEAU) ---
        
        function generateRandomCode() {
            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
            let result = 'GIFT-';
            for (let i = 0; i < 4; i++) {
                result += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return result;
        }

        function adminCreateExchangeCode() {
            if (!isAdmin) return;

            const amountInput = document.getElementById('newCodeAmount');
            const amount = parseInt(amountInput.value);

            if (isNaN(amount) || amount < 100) {
                showToast("Veuillez entrer un montant valide (min 100 F).", true);
                return;
            }
            
            const newCode = {
                code: generateRandomCode(),
                amount: amount,
                // Dans un vrai système, il y aurait ici l'ID de l'utilisateur ciblé (si non universel)
                // Pour cette simulation, on l'ajoute à la liste des codes non échangés de l'utilisateur simulé.
            };

            // Simuler l'attribution du code à l'utilisateur actuel
            userUnredeemedCodes.push(newCode);
             localStorage.setItem('userUnredeemedCodes', JSON.stringify(userUnredeemedCodes));


            // Afficher le code généré
            document.getElementById('latestCode').textContent = newCode.code;
            document.getElementById('generatedCodeDisplay').classList.remove('hidden');
            showToast(`Code **${newCode.code}** généré et attribué à l'utilisateur simulé.`, false);
            updateUI();
        }
        
        // Fonctions existantes qui doivent toujours exister (simulations)

        function showProductDetails(productName) {
            // Fonction de simulation
            const productData = {
                'C-5D': { price: 5000, daily: 200, cycles: 48, total: 9600, rate: 192.0, limit: 2, desc: "La série C est un système de production d'énergie solaire domestique de Clearway. Ce produit peut vous aider à améliorer votre quotidien et à réduire vos factures d'électricité. Il produit principalement de l'électricité de..." },
                'C-20D': { price: 20000, daily: 840, cycles: 48, total: 40320, rate: 201.6, limit: 2, desc: "Produit de niveau intermédiaire offrant un rendement énergétique élevé. Idéal pour les ménages à consommation moyenne." },
                'C-60D': { price: 60000, daily: 2640, cycles: 48, total: 126720, rate: 211.2, limit: 1, desc: "Produit haut de gamme pour une production électrique substantielle, adapté aux grandes maisons ou aux petites entreprises." },
                'C-150D': { price: 150000, daily: 6900, cycles: 48, total: 331200, rate: 220.8, limit: 1, desc: "Notre produit phare offrant le meilleur rendement total sur la durée de vie du contrat. Capacité et rentabilité maximales." },
            };
            
            const product = productData[productName];
            if (product) {
                document.getElementById('detail-product-name').textContent = productName;
                document.getElementById('detail-price').textContent = formatCurrency(product.price);
                document.getElementById('detail-cycles').textContent = product.cycles + " Jours Ouvrables";
                document.getElementById('detail-total-revenue').textContent = formatCurrency(product.total);
                document.getElementById('detail-daily-revenue').textContent = formatCurrency(product.daily);
                document.getElementById('detail-rate').textContent = product.rate + "%";
                document.getElementById('detail-limit').textContent = product.limit;
                document.getElementById('detail-description').textContent = product.desc;
                showSection('product-details');
            }
        }
        
        function selectAmount(amount, fromInput = false) {
             // Implémentation de la sélection du montant (Recharge)
        }
        
        function handleRecharge() {
            // Implémentation de la recharge
        }
        
        function updateWithdrawalDetails(amount) {
            // Implémentation de la mise à jour des détails de retrait
            const requested = parseInt(amount);
            if (isNaN(requested) || requested < 1000) {
                document.getElementById('requestedAmount').textContent = '0';
                document.getElementById('feeAmount').textContent = '0';
                document.getElementById('receivedAmount').textContent = '0';
                return;
            }

            const feeRate = 0.07;
            const fee = Math.floor(requested * feeRate);
            const received = requested - fee;

            document.getElementById('requestedAmount').textContent = formatCurrency(requested);
            document.getElementById('feeAmount').textContent = formatCurrency(fee);
            document.getElementById('receivedAmount').textContent = formatCurrency(received);
        }
        
        function handleWithdrawal() {
             // Implémentation du retrait
        }
        
        function switchProductTab(tab) {
             // Implémentation du changement d'onglet
        }
        
        function handleReceiveDailyRevenue(productId, amount) {
             // Implémentation de la collecte de revenus
        }
        
        function handlePurchaseWithPandaBank() {
             // Implémentation de l'achat avec PandaBank
        }
        
        function scrollToTop(elementId) {
             // Implémentation du défilement vers le haut
        }
        
        function validateTransaction(trxId) {
             // Implémentation de la validation admin
        }
        
        function editUser(userId) {
             // Implémentation de l'édition utilisateur admin
        }
        
        function copyInvitationCode() {
             // Implémentation de la copie de code
        }
        
        function simulateReturnAllCodes() {
             // Implémentation de la simulation de codes renvoyés
        }
        
        document.addEventListener('DOMContentLoaded', () => {
             // Masquer la section d'authentification et afficher la section principale si l'utilisateur est déjà authentifié (simulé)
             if (isAuthenticated) {
                 document.getElementById('authScreen').classList.add('hidden');
                 document.getElementById('mainContent').classList.remove('hidden-until-auth');
                 document.getElementById('navbar').classList.remove('hidden-until-auth');
                 showSection('home');
             }
             
             updateUI();
        });

    </script>
</body>
</html>
