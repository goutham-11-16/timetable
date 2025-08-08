
<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Timetable Alarm Pro</title>
    <!-- Poppins font for headings and Inter for body text -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Poppins:wght@700&display=swap" rel="stylesheet">
    <!-- Tailwind CSS CDN for a modern, responsive design -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        heading: ['Poppins', 'sans-serif']
                    },
                    colors: {
                        primary: 'rgb(var(--color-primary))',
                        secondary: 'rgb(var(--color-secondary))',
                        accent: 'rgb(var(--color-accent))',
                        background: 'rgb(var(--color-background))',
                        card: 'rgb(var(--color-card))',
                        text: 'rgb(var(--color-text))',
                        border: 'rgb(var(--color-border))',
                        highlight: 'rgb(var(--color-highlight))',
                        green: {
                          500: '#22c55e',
                          600: '#16a34a',
                        },
                    },
                    keyframes: {
                        'fade-in-up': {
                            '0%': { opacity: '0', transform: 'translateY(20px)' },
                            '100%': { opacity: '1', transform: 'translateY(0)' },
                        },
                        'fade-in': {
                            '0%': { opacity: '0' },
                            '100%': { opacity: '1' },
                        }
                    },
                    animation: {
                        'fade-in-up': 'fade-in-up 0.6s ease-out',
                        'fade-in': 'fade-in 0.8s ease-out'
                    }
                }
            }
        }
    </script>
    <style>
        :root, [data-theme="light"] {
            --color-primary: 30, 58, 138; /* a deep blue */
            --color-secondary: 239, 246, 255; /* a light blue-gray */
            --color-accent: 20, 184, 166; /* a vibrant teal */
            --color-background: 249, 250, 251; /* very light gray */
            --color-card: 255, 255, 255;
            --color-text: 17, 24, 39; /* dark charcoal text */
            --color-border: 229, 231, 235;
            --color-highlight: 59, 130, 246;
        }

        [data-theme="dark"] {
            --color-primary: 29, 78, 216; /* blue-700 */
            --color-secondary: 30, 41, 59; /* slate-800 */
            --color-accent: 5, 150, 105; /* green-700 */
            --color-background: 17, 24, 39; /* gray-900 */
            --color-card: 31, 41, 55; /* gray-800 */
            --color-text: 255, 255, 255; /* white */
            --color-border: 75, 85, 99; /* gray-600 */
            --color-highlight: 59, 130, 246;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
            background-color: rgb(var(--color-background));
            color: rgb(var(--color-text));
        }

        h1, h2, h3 {
            font-family: 'Poppins', sans-serif;
            letter-spacing: -0.025em;
        }
        
        .download-button {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .download-button:hover {
            transform: translateY(-2px) scale(1.02);
            box-shadow: 0 10px 20px -5px rgba(var(--color-primary), 0.3), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .card-hover {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .card-hover:hover {
            transform: translateY(-4px) scale(1.01);
            box-shadow: 0 20px 25px -5px rgba(var(--color-text), 0.1), 0 8px 10px -6px rgba(var(--color-text), 0.1);
        }
        .screenshot-container {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
            -ms-overflow-style: none;
        }
        .screenshot-container::-webkit-scrollbar {
            display: none;
        }
        
        /* Animations for sections */
        .animate-on-scroll {
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.8s ease-out, transform 0.8s ease-out;
        }
        .animate-on-scroll.is-visible {
            opacity: 1;
            transform: translateY(0);
        }
        .star-filled {
            color: #fbbf24; /* yellow-400 */
            transform: scale(1.1);
        }

        /* Modal specific styles */
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.9);
            display: none; /* Initially hidden */
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .modal.open {
            display: flex;
        }
        .modal-content {
            max-width: 90%;
            max-height: 90%;
            object-fit: contain;
            transition: transform 0.3s ease-in-out;
            margin: auto;
        }
        .modal-nav {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            color: white;
            font-size: 2rem;
            cursor: pointer;
            padding: 1rem;
            user-select: none;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 50%;
        }
        .modal-nav:hover {
            background: rgba(0, 0, 0, 0.6);
        }
        .modal-nav.prev {
            left: 1rem;
        }
        .modal-nav.next {
            right: 1rem;
        }
        .modal-close {
            position: absolute;
            top: 1rem;
            right: 1rem;
            font-size: 2rem;
            color: white;
            cursor: pointer;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .modal-close:hover {
            background: rgba(0, 0, 0, 0.6);
        }
    </style>
</head>
<body class="bg-background text-text">

    <!-- Dark Mode Toggle -->
    <div class="fixed top-4 right-4 z-50">
        <button id="themeToggle" class="p-2 rounded-full bg-card shadow-md text-text hover:text-highlight transition-colors duration-300 focus:outline-none">
            <svg id="sun-icon" class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24" style="display:none;"><path d="M12 2.25a.75.75 0 01.75.75v2.25a.75.75 0 01-1.5 0V3a.75.75 0 01.75-.75zM7.5 12a4.5 4.5 0 119 0 4.5 4.5 0 01-9 0zM18.894 6.106a.75.75 0 00-1.06-1.06l-1.591 1.59a.75.75 0 101.06 1.06l1.59-1.591zM21.75 12a.75.75 0 01-.75.75h-2.25a.75.75 0 010-1.5h2.25a.75.75 0 01.75.75zM18.894 17.894a.75.75 0 001.06-1.06l-1.59-1.59a.75.75 0 00-1.06 1.06l1.59 1.59zM12 21.75a.75.75 0 01-.75-.75v-2.25a.75.75 0 011.5 0v2.25a.75.75 0 01-.75.75zM4.939 17.894a.75.75 0 001.06-1.06l-1.59-1.59a.75.75 0 00-1.06 1.06l1.59 1.59zM3 12a.75.75 0 01.75-.75h2.25a.75.75 0 010 1.5H3.75A.75.75 0 013 12zM4.939 6.106a.75.75 0 00-1.06 1.06l1.59 1.59a.75.75 0 001.06-1.06l-1.59-1.59z"/></svg>
            <svg id="moon-icon" class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24" style="display:none;"><path d="M9.593 2.128a.75.75 0 01-.433 1.059A7.262 7.262 0 006.183 5.48a7.252 7.252 0 00-2.316 6.787A5.963 5.963 0 005.772 18h12.516a5.96 5.96 0 002.316-5.733 7.251 7.251 0 00-3.08-5.352 7.26 7.26 0 00-4.004-1.396A6.299 6.299 0 019.593 2.128zm.536 17.848a.75.75 0 01-.403-.66l.526-2.502a.75.75 0 011.488.312l-.526 2.502a.75.75 0 01-.845.349zm5.556 0a.75.75 0 01-.845-.349l-.526-2.502a.75.75 0 011.488-.312l.526 2.502a.75.75 0 01-.403.66zm5.836-7.854a.75.75 0 01-.635.807c-2.427.34-4.819.866-7.07 1.637a11.364 11.364 0 01-4.032 1.482 1.5 1.5 0 00-1.284 1.472v.228a.75.75 0 01-1.5 0v-.228a3 3 0 012.56-2.943c2.479-.909 5.215-1.464 7.747-1.745l.135-.016a.75.75 0 01.807.635z"/></svg>
        </button>
    </div>

    <!-- Main Content Container -->
    <main class="container mx-auto px-4 py-12 max-w-4xl">

        <!-- App Header Section (Play Store-like) -->
        <section id="app-header" class="mb-8 flex flex-col md:flex-row items-center md:items-start gap-6 animate-on-scroll">
            <img src="https://ik.imagekit.io/a0jenesev/Adobe%20Express%20-%20file.png?updatedAt=1754675441789" alt="Timetable Alarm Pro Logo" class="w-24 h-24 md:w-32 md:h-32 rounded-2xl shadow-lg border-2 border-white">
            <div class="text-center md:text-left flex-1">
                <h1 class="font-heading text-2xl md:text-3xl font-bold text-text mb-1">Timetable Alarm Pro</h1>
                <p class="text-sm md:text-base text-gray-500 mb-4">ByGoutham</p>
                
                <!-- Single Download Button (Visible Initially) -->
                <a href="#" id="initialDownloadButton" class="download-button w-full sm:w-auto px-6 py-2 bg-green-500 text-white font-bold rounded-full hover:bg-green-600 mb-4 sm:mb-0">
                    Download App
                </a>

                <!-- Other Download Buttons (Hidden Initially) -->
                <div id="otherDownloadButtons" class="hidden">
                    <div class="flex flex-col sm:flex-row items-center justify-center md:justify-start gap-2">
                        <a href="https://drive.google.com/file/d/1uk_Hn3poxTL-0XrEHya1f_Z85T8DaeGD/view?usp=sharing" id="downloadButton1" class="download-button w-full sm:w-auto px-6 py-2 bg-green-500 text-white font-bold rounded-full hover:bg-green-600">
                            Download for GOUTHAM
                        </a>
                        <a href="https://drive.google.com/file/d/1uk_Hn3poxTL-0XrEHya1f_Z85T8DaeGD/view?usp=sharing" id="downloadButton2" class="download-button w-full sm:w-auto px-6 py-2 bg-green-500 text-white font-bold rounded-full hover:bg-green-600">
                            Download for KARIM
                        </a>
                    </div>
                    <div class="mt-2 flex flex-col sm:flex-row items-center justify-center md:justify-start gap-2">
                        <a href="https://drive.google.com/file/d/1uk_Hn3poxTL-0XrEHya1f_Z85T8DaeGD/view?usp=sharing" id="downloadButton3" class="download-button w-full sm:w-auto px-6 py-2 bg-green-500 text-white font-bold rounded-full hover:bg-green-600">
                            Download for SHIRI
                        </a>
                        <a href="https://drive.google.com/file/d/1uk_Hn3poxTL-0XrEHya1f_Z85T8DaeGD/view?usp=sharing" id="downloadButton4" class="download-button w-full sm:w-auto px-6 py-2 bg-green-500 text-white font-bold rounded-full hover:bg-green-600">
                            Download for BHAVYA
                        </a>
                    </div>
                </div>
            </div>
        </section>

        <p class="text-center text-sm text-gray-500 mt-2 mb-8">Version 1.5</p>
        
        <!-- App Details: Size and Rating -->
        <section id="app-details" class="mb-12 border-b border-border pb-8 animate-on-scroll">
            <div class="grid grid-cols-2 text-center max-w-xs mx-auto">
                <div class="border-r border-border">
                    <p class="text-xl font-bold text-text">32 MB</p>
                    <p class="text-sm text-gray-500 mt-1">App Size</p>
                </div>
                <div>
                    <p class="text-xl font-bold text-text">4.5/5 ⭐</p>
                    <p class="text-sm text-gray-500 mt-1">Rating</p>
                </div>
            </div>
        </section>

        <!-- App Screenshots (Horizontally Scrollable) -->
        <section id="screenshots" class="mb-12 animate-on-scroll">
            <h2 class="font-heading text-2xl font-bold mb-4 text-text">Preview</h2>
            <div class="screenshot-container flex gap-4 pb-4">
                <!-- App screenshot links -->
                <img src="https://ik.imagekit.io/a0jenesev/1.png?updatedAt=1754675688633" alt="App Screenshot 1" class="screenshot w-auto h-48 md:h-64 rounded-xl shadow-lg flex-shrink-0 card-hover cursor-pointer">
                <img src="https://ik.imagekit.io/a0jenesev/2.png?updatedAt=1754675688487" alt="App Screenshot 2" class="screenshot w-auto h-48 md:h-64 rounded-xl shadow-lg flex-shrink-0 card-hover cursor-pointer">
                <img src="https://ik.imagekit.io/a0jenesev/3.png?updatedAt=1754675688540" alt="App Screenshot 3" class="screenshot w-auto h-48 md:h-64 rounded-xl shadow-lg flex-shrink-0 card-hover cursor-pointer">
                <img src="https://ik.imagekit.io/a0jenesev/4.png?updatedAt=1754675688387" alt="App Screenshot 4" class="screenshot w-auto h-48 md:h-64 rounded-xl shadow-lg flex-shrink-0 card-hover cursor-pointer">
                <img src="https://ik.imagekit.io/a0jenesev/5.png?updatedAt=1754675688513" alt="App Screenshot 5" class="screenshot w-auto h-48 md:h-64 rounded-xl shadow-lg flex-shrink-0 card-hover cursor-pointer">
                <img src="https://ik.imagekit.io/a0jenesev/6.png?updatedAt=1754675688362" alt="App Screenshot 6" class="screenshot w-auto h-48 md:h-64 rounded-xl shadow-lg flex-shrink-0 card-hover cursor-pointer">
                <img src="https://ik.imagekit.io/a0jenesev/7.png?updatedAt=1754675688397" alt="App Screenshot 7" class="screenshot w-auto h-48 md:h-64 rounded-xl shadow-lg flex-shrink-0 card-hover cursor-pointer">
                <img src="https://ik.imagekit.io/a0jenesev/8.png?updatedAt=1754675688411" alt="App Screenshot 8" class="screenshot w-auto h-48 md:h-64 rounded-xl shadow-lg flex-shrink-0 card-hover cursor-pointer">
            </div>
        </section>
        
        <!-- About this app section -->
        <section id="about" class="mb-8 animate-on-scroll">
            <h2 class="font-heading text-2xl font-bold mb-4 text-text">About this app</h2>
            <p class="text-gray-700 leading-relaxed mb-6">
                Timetable Alarm Pro is your ultimate companion for managing busy schedules and ensuring you're always on time for your classes, lectures, or appointments. Designed for students, professionals, and anyone with a recurring schedule, this app transforms your daily routine into a seamless, stress-free experience.
                Gone are the days of missed lessons or overlooked meetings! Timetable Alarm Pro features highly precise alarms that cut through silent modes and Android's battery optimizations (like Doze mode), ensuring your reminders are always heard. Whether your screen is on or off, you'll receive clear visual alerts with our distinctive full-screen incoming call-style alarms and convenient banner notifications.
                Easily set up your entire weekly schedule with our intuitive interface. Customize each class with subject names, room numbers, and even personal notes. Personalize your experience further by choosing a custom alarm sound from your device, or let the app follow your system's default dark or light theme for a consistent look.
                Our intelligent "Up Next" dashboard and home screen widget keep your very next class glanceable right from your phone's home screen. On Saturday? Effortlessly set a unique schedule or declare a holiday with our smart Saturday order feature.
            </p>
        </section>

        <!-- Features section -->
        <section id="features" class="mb-12 animate-on-scroll">
            <h2 class="font-heading text-2xl font-bold mb-4 text-text">Features</h2>
            <ul class="list-disc list-inside text-gray-700 space-y-2 text-sm md:text-base leading-relaxed">
                <li><span class="font-semibold text-text">Reliable & Precise Class Alarms:</span> Get accurate, on-time reminders that break through silent modes and phone optimizations.</li>
                <li><span class="font-semibold text-text">Dynamic Visual Notifications:</span> Includes full-screen alarms that pop up over the lock screen and standard banner notifications.</li>
                <li><span class="font-semibold text-text">Smart Timetable Management:</span> Easily add, modify, and delete classes, with automatic overlap checks and color-coding.</li>
                <li><span class="font-semibold text-text">Home Screen Widget & "Up Next" Dashboard:</span> Stay informed with a glanceable widget and a dedicated screen showing your next class.</li>
                <li><span class="font-semibold text-text">Custom Alarm Sounds:</span> Personalize your alarms by choosing your own audio files.</li>
                <li><span class="font-semibold text-text">Automatic System Theme Sync:</span> The app's appearance (light/dark mode) automatically adjusts to your phone's system settings.</li>
                <li><span class="font-semibold text-text">In-App Guided Setup:</span> Includes a visual guide with videos to help users configure crucial phone settings for optimal alarm reliability.</li>
                <li><span class="font-semibold text-text">Ad-Supported:</span> All features are available for free.</li>
            </ul>
        </section>

        
        
        <!-- Ratings Section -->
        <section id="ratings" class="mb-12 border-b border-border pb-8 animate-on-scroll">
            <h2 class="font-heading text-2xl font-bold mb-4 text-text text-center">Rate this app</h2>
            <div class="flex justify-center gap-4">
                <button class="rating-button text-gray-400 hover:text-yellow-500 transition-all duration-200 focus:outline-none" data-rating="1">
                    <svg class="w-12 h-12 transition-transform duration-200" fill="currentColor" viewBox="0 0 24 24"><path d="M12 .587l3.668 7.568L24 9.42l-6.28 6.111L19.4 24 12 20.21l-7.4 3.79 1.68-8.462L0 9.42l8.332-1.265L12 .587z"/></svg>
                </button>
                <button class="rating-button text-gray-400 hover:text-yellow-500 transition-all duration-200 focus:outline-none" data-rating="2">
                    <svg class="w-12 h-12 transition-transform duration-200" fill="currentColor" viewBox="0 0 24 24"><path d="M12 .587l3.668 7.568L24 9.42l-6.28 6.111L19.4 24 12 20.21l-7.4 3.79 1.68-8.462L0 9.42l8.332-1.265L12 .587z"/></svg>
                </button>
                <button class="rating-button text-gray-400 hover:text-yellow-500 transition-all duration-200 focus:outline-none" data-rating="3">
                    <svg class="w-12 h-12 transition-transform duration-200" fill="currentColor" viewBox="0 0 24 24"><path d="M12 .587l3.668 7.568L24 9.42l-6.28 6.111L19.4 24 12 20.21l-7.4 3.79 1.68-8.462L0 9.42l8.332-1.265L12 .587z"/></svg>
                </button>
                <button class="rating-button text-gray-400 hover:text-yellow-500 transition-all duration-200 focus:outline-none" data-rating="4">
                    <svg class="w-12 h-12 transition-transform duration-200" fill="currentColor" viewBox="0 0 24 24"><path d="M12 .587l3.668 7.568L24 9.42l-6.28 6.111L19.4 24 12 20.21l-7.4 3.79 1.68-8.462L0 9.42l8.332-1.265L12 .587z"/></svg>
                </button>
                <button class="rating-button text-gray-400 hover:text-yellow-500 transition-all duration-200 focus:outline-none" data-rating="5">
                    <svg class="w-12 h-12 transition-transform duration-200" fill="currentColor" viewBox="0 0 24 24"><path d="M12 .587l3.668 7.568L24 9.42l-6.28 6.111L19.4 24 12 20.21l-7.4 3.79 1.68-8.462L0 9.42l8.332-1.265L12 .587z"/></svg>
                </button>
            </div>
        </section>

        

    </main>
    
    <!-- Modal for fullscreen screenshot view -->
    <div id="screenshotModal" class="modal" onclick="closeModal()">
        <button id="modalClose" class="modal-close" aria-label="Close modal">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
        </button>
        <button id="modalPrev" class="modal-nav prev" aria-label="Previous image">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" /></svg>
        </button>
        <img id="modalImage" class="modal-content" src="">
        <button id="modalNext" class="modal-nav next" aria-label="Next image">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" /></svg>
        </button>
    </div>

    <!-- Footer -->
    <footer class="bg-primary text-white text-center p-6 mt-12">
        <p class="text-sm">&copy; 2024 Timetable Alarm Pro. All rights reserved.</p>
    </footer>

    <script>
        // --- Dark Mode Toggle Logic ---
        const themeToggle = document.getElementById('themeToggle');
        const theme = localStorage.getItem('theme') || (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
        document.documentElement.setAttribute('data-theme', theme);
        const setIcon = (theme) => {
            document.getElementById('sun-icon').style.display = theme === 'dark' ? 'block' : 'none';
            document.getElementById('moon-icon').style.display = theme === 'light' ? 'block' : 'none';
        };
        setIcon(theme);

        themeToggle.addEventListener('click', () => {
            let currentTheme = document.documentElement.getAttribute('data-theme');
            let newTheme = currentTheme === 'dark' ? 'light' : 'dark';
            document.documentElement.setAttribute('data-theme', newTheme);
            localStorage.setItem('theme', newTheme);
            setIcon(newTheme);
        });

        // --- Download Button Logic ---
        const initialDownloadButton = document.getElementById('initialDownloadButton');
        const otherDownloadButtonsContainer = document.getElementById('otherDownloadButtons');
        const downloadLink = 'https://drive.google.com/file/d/1uk_Hn3poxTL-0XrEHya1f_Z85T8DaeGD/view?usp=sharing';
        
        initialDownloadButton.addEventListener('click', (e) => {
            e.preventDefault(); // This prevents the immediate redirect
            initialDownloadButton.classList.add('hidden');
            otherDownloadButtonsContainer.classList.remove('hidden');
        });

        const downloadButtons = {
            1: document.getElementById('downloadButton1'),
            2: document.getElementById('downloadButton2'),
            3: document.getElementById('downloadButton3'),
            4: document.getElementById('downloadButton4')
        };
        
        Object.keys(downloadButtons).forEach(id => {
            downloadButtons[id].addEventListener('click', (e) => {
                e.preventDefault();
                window.location.href = downloadLink;
            });
        });
        
        // --- Screenshot Modal Logic ---
        const screenshots = Array.from(document.querySelectorAll('.screenshot'));
        const modal = document.getElementById('screenshotModal');
        const modalImage = document.getElementById('modalImage');
        const modalPrev = document.getElementById('modalPrev');
        const modalNext = document.getElementById('modalNext');
        let currentImageIndex = 0;

        screenshots.forEach((screenshot, index) => {
            screenshot.addEventListener('click', () => {
                currentImageIndex = index;
                updateModalImage();
                modal.classList.add('open');
            });
        });

        modalPrev.addEventListener('click', (e) => {
            e.stopPropagation(); // Prevent the modal from closing
            currentImageIndex = (currentImageIndex - 1 + screenshots.length) % screenshots.length;
            updateModalImage();
        });

        modalNext.addEventListener('click', (e) => {
            e.stopPropagation(); // Prevent the modal from closing
            currentImageIndex = (currentImageIndex + 1) % screenshots.length;
            updateModalImage();
        });

        function updateModalImage() {
            modalImage.src = screenshots[currentImageIndex].src;
        }
        
        // Close modal by clicking outside the content
        modal.addEventListener('click', (e) => {
            if (e.target === modal) {
                closeModal();
            }
        });
        
        // Close modal by clicking the 'X' button
        document.getElementById('modalClose').addEventListener('click', (e) => {
            e.stopPropagation(); // Prevent modal from closing if user clicks on the X itself
            closeModal();
        });

        function closeModal() {
            modal.classList.remove('open');
            modalImage.src = '';
        }

        // --- Section fade-in animation on scroll ---
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('is-visible');
                    observer.unobserve(entry.target);
                }
            });
        }, { threshold: 0.1 });

        document.querySelectorAll('.animate-on-scroll').forEach(section => {
            observer.observe(section);
        });
    </script>
</body>
</html>
