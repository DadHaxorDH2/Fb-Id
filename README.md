<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><Mail Generator - Da4k0xmT</title>
    
    <script src="https://cdn.tailwindcss.com"></script>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0" rel="stylesheet" />

    <style>
        /* --- Bloody Theme Colors --- */
        :root {
            --color-primary: 185 28 28; /* Crimson 600 */
            --color-primary-dark: 153 27 27; /* Crimson 700 */
            --color-accent: 245 158 11; /* Amber 500 */
        }
        
        body {
            font-family: 'Space Grotesk', sans-serif;
        }

        /* Custom class to hide scrollbars */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        /* Material Icons sizing adjustment */
        .material-symbols-outlined {
            font-size: inherit;
            vertical-align: middle;
        }

        /* Splash screen loading bar animation */
        @keyframes fill-bar {
            from { width: 0%; }
            to { width: 100%; }
        }
        .animate-fill-bar {
            animation: fill-bar 2s ease-out forwards;
        }
        
        /* Toast notification animations */
        @keyframes toastIn {
            from { transform: translateY(100%) scale(0.9); opacity: 0; }
            to { transform: translateY(0) scale(1); opacity: 1; }
        }
        @keyframes toastOut {
            from { transform: translateY(0) scale(1); opacity: 0; }
            to { transform: translateY(100%) scale(0.9); opacity: 0; }
        }
        .toast-in { animation: toastIn 0.3s ease-out forwards; }
        .toast-out { animation: toastOut 0.3s ease-in forwards; }

        /* A simple spinner for buttons */
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        .spinner {
            display: inline-block;
            width: 1.25rem;
            height: 1.25rem;
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s linear infinite; 
        }
        .dark .spinner {
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-top-color: white;
        }
        .spinner-container {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 100%;
            height: 100%; 
        }

        /* Design: Focus ring for accessibility and polish */
        .action-btn:focus {
            outline: none;
            box-shadow: 0 0 0 3px rgba(var(--color-primary), 0.5); 
        }
        
        /* --- Day/Light Mode Adjustment for Bloody Theme --- */
        /* Body for Light Mode (Less dark background) */
        html.light body {
            background-color: #f3f4f6; /* Gray 100 */
            color: #1f2937; /* Gray 800 */
        }

        /* Header in Light Mode */
        html.light header {
            background-color: #f3f4f6b3; 
            border-bottom-color: #e5e7eb; /* Gray 200 */
        }
        html.light header .text-red-500,
        html.light header .material-symbols-outlined {
            color: #b91c1c; /* Crimson 600 */
        }
        html.light header button:hover {
            background-color: #e5e7eb; /* Gray 200 */
        }
        
        /* Cards/Sections in Light Mode */
        html.light .bg-gray-900,
        html.light .bg-gray-800,
        html.light .bg-gray-900\/70 {
            background-color: #ffffff; /* White card background */
            border-color: #fecaca; /* Red 200 border */
            color: #1f2937;
        }
        html.light .text-red-500 {
            color: #dc2626; /* Red 600 */
        }
        html.light .text-gray-100 {
             color: #1f2937;
        }
        html.light .text-gray-50 {
             color: #000000;
        }
        html.light .text-gray-300, 
        html.light .text-gray-400 {
            color: #6b7280; /* Gray 500 */
        }
        
        /* Buttons in Light Mode (to keep them visible) */
        html.light #refresh-btn {
            background-color: #f3f4f6;
            border-color: #e5e7eb;
        }
        html.light #refresh-btn:hover {
            background-color: #e5e7eb;
        }
    </style>
</head>
<body class="bg-gray-950 text-gray-100 antialiased transition-colors duration-300">

    <div id="splash-screen" class="fixed inset-0 z-50 flex flex-col items-center justify-center bg-gray-950 transition-opacity duration-500">
        <div class="text-center">
            <h1 class="text-4xl font-bold bg-gradient-to-r from-red-600 to-red-800 text-transparent bg-clip-text">Mail Generator</h1>
            <p class="text-gray-500 mt-2">Powered by Da4k0xmT</p>
        </div>
        <div class="w-48 h-1.5 bg-gray-700 rounded-full overflow-hidden mt-8">
            <div id="loading-bar" class="h-full bg-gradient-to-r from-red-600 to-red-800 animate-fill-bar"></div>
        </div>
    </div>

    <div id="app-container" class="max-w-lg mx-auto h-screen flex flex-col opacity-0 transition-opacity duration-500">
        
        <div id="home-screen" class="flex flex-col h-full">
            <header class="flex items-center justify-between p-4 border-b border-gray-800 sticky top-0 bg-gray-950/80 backdrop-blur-sm z-10">
                <button id="send-mail-nav-btn" class="p-2 rounded-full hover:bg-gray-800 transition-colors" aria-label="Compose new mail">
                    <span class="material-symbols-outlined text-red-500">edit_square</span>
                </button>
                <h1 class="text-xl font-bold text-center text-red-500">Temp Mail Inbox</h1>
                <button id="theme-toggle-btn" class="p-2 rounded-full hover:bg-gray-800 transition-colors" aria-label="Toggle theme">
                    <span id="theme-icon" class="material-symbols-outlined text-red-500">dark_mode</span>
                </button>
            </header>

            <main class="p-4 space-y-5 flex-grow overflow-y-auto no-scrollbar">
                
                <div class="bg-gray-900 p-5 rounded-2xl shadow-xl border border-red-800/80">
                    <p class="text-sm text-red-500 mb-2 font-medium flex items-center gap-1">
                        <span class="material-symbols-outlined text-base">outgoing_mail</span> Your temporary email address
                    </p>
                    <div class="flex items-center justify-between gap-3">
                        <span id="email-address" class="font-mono text-xl md:text-2xl truncate font-extrabold text-gray-50">loading...</span>
                        <button id="copy-btn" class="flex-shrink-0 p-3 rounded-xl bg-amber-500 text-gray-900 hover:bg-amber-600 transition-colors shadow-lg" aria-label="Copy email address">
                            <span class="material-symbols-outlined text-xl">content_copy</span>
                        </button>
                    </div>
                </div>
                
                <div class="grid grid-cols-2 gap-4">
                    <button id="change-mail-btn" class="action-btn w-full bg-amber-500 hover:bg-amber-600 active:bg-amber-700 text-gray-900 font-semibold py-3 px-4 rounded-xl shadow-lg transition-all duration-200 transform hover:-translate-y-0.5" data-original-content='<span class="button-content flex items-center justify-center gap-2"><span class="material-symbols-outlined">change_circle</span><span>Change Mail</span></span>'>
                         <span class="button-content flex items-center justify-center gap-2">
                            <span class="material-symbols-outlined">change_circle</span>
                            <span>Change Mail</span>
                        </span>
                    </button>
                    <button id="delete-mail-btn" class="action-btn w-full bg-red-800 hover:bg-700 active:bg-red-900 text-white font-semibold py-3 px-4 rounded-xl shadow-lg transition-all duration-200 transform hover:-translate-y-0.5" data-original-content='<span class="button-content flex items-center justify-center gap-2"><span class="material-symbols-outlined">delete</span><span>Delete Mail</span></span>'>
                         <span class="button-content flex items-center justify-center gap-2">
                            <span class="material-symbols-outlined">delete</span>
                            <span>Delete Mail</span>
                        </span>
                    </button>
                </div>

                <div class="pt-2">
                    <div class="flex items-center justify-between mb-4">
                        <h2 class="text-xl font-bold text-red-500">📬 Inbox</h2>
                        <div class="flex items-center gap-3">
                            <div class="flex items-center gap-1 bg-gray-900 px-3 py-1.5 rounded-xl shadow border border-gray-800">
                                <span class="material-symbols-outlined text-lg text-red-500">timer</span>
                                <span id="timer-display" class="text-sm font-bold text-gray-300">10:00</span>
                            </div>
                            <button id="extend-timer-btn" class="p-2 rounded-xl bg-red-600 hover:bg-red-700 text-white shadow-md transition-transform transform hover:scale-105" aria-label="Extend timer">
                                <span class="material-symbols-outlined text-xl">add_alarm</span>
                            </button>
                            <button id="refresh-btn" class="p-3 rounded-full bg-gray-800 hover:bg-gray-700 transition-colors" aria-label="Refresh inbox">
                                <span class="material-symbols-outlined text-xl text-red-400">refresh</span>
                            </button>
                        </div>
                    </div>

                    <div id="empty-inbox" class="text-center py-10 px-4 bg-gray-900 rounded-2xl shadow-inner border border-red-900">
                        <span class="material-symbols-outlined text-6xl text-red-600">all_inbox</span>
                        <p class="mt-3 text-lg font-medium text-gray-400">Your inbox is empty</p>
                        <p class="text-sm text-gray-500">Waiting for incoming temp emails...</p>
                    </div>
                    
                    <div id="inbox-list" class="space-y-3">
                    </div>
                </div>
                
                <footer class="text-center text-xs text-gray-500 py-4 mt-auto">
                    Developed by <b>B.C.S</b>. Powered by <a href="https://t.me/Da4k0xmT" class="font-semibold text-red-500 hover:text-red-400 hover:underline transition-colors">Da4k0xmT</a>.
                </footer>
            </main>
        </div>

        <div id="email-screen" class="hidden flex-col h-full">
            <header class="flex items-center p-4 border-b border-gray-800 sticky top-0 bg-gray-950/80 backdrop-blur-sm z-10">
                <button id="back-to-inbox-btn" class="p-2 rounded-full hover:bg-gray-800 transition-colors" aria-label="Back to inbox">
                    <span class="material-symbols-outlined text-red-500">arrow_back</span>
                </button>
                <h2 class="text-lg font-bold text-center flex-grow truncate px-4 text-red-500" id="email-subject-header">Email Details</h2>
                <div class="w-10"></div> 
            </header>

            <main class="p-4 flex-grow overflow-y-auto no-scrollbar space-y-4 flex flex-col">
                <div class="bg-gray-900 p-4 rounded-xl shadow-lg border border-red-900 flex flex-col space-y-3">
                    <div class="flex items-start gap-3 border-b pb-3 border-gray-700">
                        <div id="email-avatar" class="w-10 h-10 rounded-full bg-red-600 text-white flex items-center justify-center text-xl font-bold flex-shrink-0"></div>
                        <div class="flex-grow min-w-0"> 
                            <div class="flex justify-between items-baseline">
                               <p id="email-from" class="font-bold truncate text-lg text-gray-50"></p>
                               <p id="email-timestamp" class="text-xs text-gray-400 flex-shrink-0 ml-2"></p>
                            </div>
                            <p class="text-sm text-gray-400 truncate">to: <span id="email-to" class="font-mono"></span></p>
                        </div>
                    </div>
                    
                    <h3 class="text-xl font-bold break-words text-red-400" id="email-subject-content">Loading Subject...</h3>
                </div>
                
                <div class="bg-gray-900 rounded-xl shadow-lg flex-grow overflow-hidden" style="min-height: 200px;">
                     <iframe id="email-body" class="w-full h-full border-0 rounded-xl bg-gray-900" sandbox="allow-same-origin"></iframe>
                </div>

                <button id="delete-message-btn" class="action-btn w-full bg-red-800 hover:bg-red-700 active:bg-red-900 text-white font-semibold py-3 px-4 rounded-xl shadow-lg transition-all flex-shrink-0 transform hover:-translate-y-0.5" data-original-content='<span class="button-content flex items-center justify-center gap-2"><span class="material-symbols-outlined">delete_forever</span><span>Delete Message (Server & Local)</span></span>'>
                    <span class="button-content flex items-center justify-center gap-2">
                        <span class="material-symbols-outlined">delete_forever</span>
                        <span>Delete Message (Server & Local)</span>
                    </span>
                </button>
            </main>
        </div>

        <div id="send-screen" class="hidden flex-col h-full">
            <header class="flex items-center p-4 border-b border-gray-800 sticky top-0 bg-gray-950/80 backdrop-blur-sm z-10">
                <button id="back-from-send-btn" class="p-2 rounded-full hover:bg-gray-800 transition-colors" aria-label="Back to inbox">
                    <span class="material-symbols-outlined text-red-500">close</span>
                </button>
                <h2 class="text-lg font-bold text-center flex-grow px-4 text-red-500">New Message</h2>
                <div class="w-10"></div> 
            </header>
            <main class="p-4 flex-grow overflow-y-auto no-scrollbar space-y-5">
                <div class="space-y-3 bg-gray-900 p-4 rounded-xl shadow-lg border border-red-900">
                    <input type="text" id="send-to" placeholder="Recipient Email Address" class="w-full p-3 border border-gray-700 rounded-lg bg-gray-800 text-gray-100 focus:ring-red-500 focus:border-red-500">
                    <input type="text" id="send-subject" placeholder="Subject" class="w-full p-3 border border-gray-700 rounded-lg bg-gray-800 text-gray-100 focus:ring-red-500 focus:border-red-500">
                    <textarea id="send-body" placeholder="Message Body" rows="8" class="w-full p-3 border border-gray-700 rounded-lg bg-gray-800 text-gray-100 focus:ring-red-500 focus:border-red-500 resize-none"></textarea>
                </div>
                
                <button id="send-mail-btn" class="action-btn w-full bg-amber-500 hover:bg-amber-600 active:bg-amber-700 text-gray-900 font-semibold py-3 px-4 rounded-xl shadow-lg transition-all duration-200 transform hover:-translate-y-0.5" data-original-content='<span class="button-content flex items-center justify-center gap-2"><span class="material-symbols-outlined">send</span><span>Send Message</span></span>'>
                    <span class="button-content flex items-center justify-center gap-2">
                        <span class="material-symbols-outlined">send</span>
                        <span>Send Message</span>
                    </span>
                </button>
                <p class="text-sm text-center text-gray-500">Note: Sending mail is currently simulated as the temporary mail API may restrict outgoing messages.</p>
            </main>
        </div>

    </div>

    <div id="toast-container" class="fixed bottom-4 left-1/2 -translate-x-1/2 z-50 space-y-2 max-w-xs w-full"></div>
    
    <script type="module">
        // --- API & Core FUNCTIONS ---
        const API_BASE = 'https://api.mail.tm';
        let currentAccount = {
            id: null,
            address: null,
            password: null,
            token: null,
        };
        let inboxInterval = null;
        let timerInterval = null;
        let timerSeconds = 600; 
        let currentOpenEmailId = null; 

        // --- DOM ELEMENTS ---
        const DOMElements = {
            splashScreen: document.getElementById('splash-screen'),
            appContainer: document.getElementById('app-container'),
            homeScreen: document.getElementById('home-screen'),
            emailScreen: document.getElementById('email-screen'),
            sendScreen: document.getElementById('send-screen'),
            toastContainer: document.getElementById('toast-container'),
            emailAddress: document.getElementById('email-address'),
            copyBtn: document.getElementById('copy-btn'),
            timerDisplay: document.getElementById('timer-display'),
            inboxList: document.getElementById('inbox-list'),
            emptyInbox: document.getElementById('empty-inbox'),
            refreshBtn: document.getElementById('refresh-btn'),
            changeMailBtn: document.getElementById('change-mail-btn'),
            deleteMailBtn: document.getElementById('delete-mail-btn'),
            extendTimerBtn: document.getElementById('extend-timer-btn'),
            sendMailNavBtn: document.getElementById('send-mail-nav-btn'),
            themeToggleBtn: document.getElementById('theme-toggle-btn'),
            themeIcon: document.getElementById('theme-icon'),
            backToInboxBtn: document.getElementById('back-to-inbox-btn'),
            emailSubjectHeader: document.getElementById('email-subject-header'),
            emailSubjectContent: document.getElementById('email-subject-content'), 
            emailAvatar: document.getElementById('email-avatar'),
            emailFrom: document.getElementById('email-from'),
            emailTo: document.getElementById('email-to'),
            emailTimestamp: document.getElementById('email-timestamp'),
            emailBody: document.getElementById('email-body'),
            deleteMessageBtn: document.getElementById('delete-message-btn'),
            backFromSendBtn: document.getElementById('back-from-send-btn'),
            sendTo: document.getElementById('send-to'),
            sendSubject: document.getElementById('send-subject'),
            sendBody: document.getElementById('send-body'),
            sendMailBtn: document.getElementById('send-mail-btn'),
        };

        const fetchWithTimeout = async (url, options = {}, timeout = 8000) => {
            const controller = new AbortController();
            const id = setTimeout(() => controller.abort(), timeout);
            try {
                const response = await fetch(url, { ...options, signal: controller.signal });
                clearTimeout(id);
                return response;
            } catch (error) {
                clearTimeout(id);
                throw error;
            }
        };

        const generatePassword = () => Math.random().toString(36).substring(2) + Math.random().toString(36).substring(2);

        const getNewEmail = async (isDelete = false) => {
            showSpinner(DOMElements.changeMailBtn
