# -
هو موقع يتيح المستخدمين لتحويل نص إلى صوت بالذكاء الاصطناعي 
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IDRES KHDR Voice Studio</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Almarai:wght@700&family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0a0a0a; /* Very dark background */
            color: #e0e0e0; /* Light grey text */
            overflow-x: hidden; /* Prevent horizontal scroll */
        }

        /* Starry background animation */
        .star-background {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background:
                url('https://placehold.co/1x1/ffffff/ffffff?text=.') repeat top left, /* Small white dots for stars */
                url('https://placehold.co/2x2/ffffff/ffffff?text=.') repeat top left,
                url('https://placehold.co/3x3/ffffff/ffffff?text=.') repeat top left;
            background-size: 1px 1px, 2px 2px, 3px 3px;
            animation: animateStars 100s linear infinite;
            z-index: -1;
        }

        @keyframes animateStars {
            from { background-position: 0 0, 0 0, 0 0; }
            to { background-position: 10000px 5000px, 8000px 4000px, 6000px 3000px; }
        }

        /* Glowing button effect */
        .glow-button {
            position: relative;
            overflow: hidden;
            background: linear-gradient(45deg, #8a2be2, #4b0082, #8a2be2); /* Purple gradient */
            background-size: 200% 200%;
            animation: glowing 3s infinite alternate;
        }

        @keyframes glowing {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        /* Custom scrollbar for better aesthetics */
        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: #1a1a1a;
        }

        ::-webkit-scrollbar-thumb {
            background: #8a2be2;
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: #6a0dad;
        }

        /* Modal backdrop */
        .modal-backdrop {
            background-color: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(5px);
        }

        /* Star rating */
        .star-rating input[type="radio"] {
            display: none;
        }
        .star-rating label {
            font-size: 2.5rem; /* Larger stars */
            color: #555;
            cursor: pointer;
            transition: color 0.2s;
        }
        .star-rating label:hover,
        .star-rating label:hover ~ label,
        .star-rating input[type="radio"]:checked ~ label {
            color: #FFD700; /* Gold color */
        }

        /* Scrolling footer text */
        .scrolling-text-container {
            width: 100%;
            overflow: hidden;
            white-space: nowrap;
        }
        .scrolling-text {
            display: inline-block;
            animation: scrollText 30s linear infinite;
            padding-left: 100%; /* Start off-screen */
        }
        @keyframes scrollText {
            0% { transform: translateX(0%); }
            100% { transform: translateX(-100%); }
        }

        /* Responsive adjustments */
        @media (max-width: 768px) {
            .main-title {
                font-size: 2.5rem; /* Adjust font size for smaller screens */
            }
            .glow-button {
                padding: 1rem 2rem; /* Adjust padding for smaller screens */
                font-size: 1.25rem;
            }
            .modal-content {
                width: 90%;
                padding: 1.5rem;
            }
            .tts-container {
                padding: 1rem;
            }
            .tts-textarea {
                min-height: 150px;
            }
            .star-rating label {
                font-size: 2rem;
            }
            .modal-content h2 {
                font-size: 2rem;
            }
            .modal-content p {
                font-size: 1rem;
            }
            .modal-content button {
                font-size: 1rem;
            }
        }
    </style>
</head>
<body class="relative min-h-screen flex flex-col justify-between items-center text-center">
    <div class="star-background"></div>

    <div id="landing-page" class="flex flex-col justify-center items-center h-screen w-full p-4">
        <h1 class="main-title text-5xl md:text-6xl lg:text-7xl font-extrabold text-white mb-12" style="font-family: 'Almarai', sans-serif;">
            حوّل النص إلى صوت مجاني واحترافي تمامًا
        </h1>
        <button id="start-now-button" class="glow-button px-8 py-4 text-2xl font-bold text-white rounded-full shadow-lg transition-all duration-300 hover:scale-105">
            ابدأ الآن
        </button>
    </div>

    <div id="youtube-modal" class="fixed inset-0 flex items-center justify-center z-50 hidden modal-backdrop">
        <div class="modal-content bg-gradient-to-br from-purple-900 to-indigo-900 p-8 rounded-2xl shadow-xl w-11/12 md:w-2/3 lg:w-1/2 max-w-2xl border border-purple-700 transform transition-all duration-300 scale-95 opacity-0">
            <h2 class="text-3xl font-bold text-white mb-6" style="font-family: 'Almarai', sans-serif;">
                اشترك في قناة IDRES KHDR على يوتيوب
            </h2>
            <p class="text-lg text-gray-200 mb-6">
                للوصول إلى جميع ميزات الموقع، يرجى الاشتراك في قناتنا.
            </p>
            <div class="aspect-video w-full bg-black rounded-lg mb-6 flex items-center justify-center text-gray-400">
                <img src="https://placehold.co/400x225/333333/ffffff?text=فيديو+تعريفي+للقناة" alt="Placeholder video" class="w-full h-auto rounded-lg">
            </div>
            <button id="subscribe-button" class="w-full py-3 bg-red-600 text-white font-bold text-xl rounded-lg hover:bg-red-700 transition-colors duration-300 shadow-md mb-4">
                اشترك الآن
            </button>
            <button id="confirm-subscription-button" class="w-full py-3 bg-green-600 text-white font-bold text-xl rounded-lg hover:bg-green-700 transition-colors duration-300 shadow-md hidden">
                تأكيد الاشتراك والمتابعة
            </button>
            <p id="subscription-message" class="text-red-300 text-sm mt-4 hidden">
                الرجاء الضغط على "اشترك الآن" ثم "تأكيد الاشتراك والمتابعة".
            </p>
        </div>
    </div>

    <div id="tts-page" class="hidden flex flex-col items-center justify-center w-full min-h-screen p-4 py-16">
        <div class="tts-container bg-gradient-to-br from-purple-800 to-indigo-800 p-8 rounded-2xl shadow-xl w-11/12 md:w-3/4 lg:w-2/3 max-w-4xl border border-purple-600">
            <h2 class="text-4xl font-bold text-white mb-8" style="font-family: 'Almarai', sans-serif;">
                أداة تحويل النص إلى صوت
            </h2>
            <textarea id="text-input" class="tts-textarea w-full p-4 mb-6 bg-gray-900 text-white rounded-lg border border-gray-700 focus:outline-none focus:ring-2 focus:ring-purple-500 transition-all duration-200" rows="8" placeholder="اكتب نصك هنا ليتم تحويله إلى صوت..."></textarea>

            <div class="flex flex-col md:flex-row items-center justify-between mb-8 space-y-4 md:space-y-0 md:space-x-4">
                <select id="voice-select" class="w-full md:w-1/2 p-3 bg-gray-900 text-white rounded-lg border border-gray-700 focus:outline-none focus:ring-2 focus:ring-purple-500 transition-all duration-200 cursor-pointer">
                    <option value="">اختر صوتًا احترافيًا</option>
                    </select>
                <button id="generate-audio-button" class="w-full md:w-1/2 py-3 bg-purple-600 text-white font-bold text-xl rounded-lg hover:bg-purple-700 transition-colors duration-300 shadow-md">
                    إنشاء الصوت
                </button>
            </div>

            <div id="audio-output" class="hidden flex flex-col items-center justify-center mt-8 p-4 bg-gray-900 rounded-lg border border-gray-700">
                <button id="play-audio-button" class="py-3 px-6 bg-green-600 text-white font-bold text-lg rounded-lg hover:bg-green-700 transition-colors duration-300 shadow-md hidden">
                    تشغيل الصوت
                </button>
                <div id="loading-indicator" class="hidden text-purple-300 text-lg">
                    جاري إنشاء الصوت...
                </div>
            </div>
        </div>
    </div>

    <div id="rating-modal" class="fixed inset-0 flex items-center justify-center z-50 hidden modal-backdrop">
        <div class="modal-content bg-gradient-to-br from-purple-900 to-indigo-900 p-8 rounded-2xl shadow-xl w-11/12 md:w-2/3 lg:w-1/2 max-w-2xl border border-purple-700 transform transition-all duration-300 scale-95 opacity-0">
            <h2 class="text-3xl font-bold text-white mb-6" style="font-family: 'Almarai', sans-serif;">
                قيم تجربتك من خمس نجمات
            </h2>
            <div class="star-rating flex justify-center mb-8">
                <input type="radio" id="star5" name="rating" value="5" /><label for="star5" title="5 نجوم">★</label>
                <input type="radio" id="star4" name="rating" value="4" /><label for="star4" title="4 نجوم">★</label>
                <input type="radio" id="star3" name="rating" value="3" /><label for="star3" title="3 نجوم">★</label>
                <input type="radio" id="star2" name="rating" value="2" /><label for="star2" title="2 نجوم">★</label>
                <input type="radio" id="star1" name="rating" value="1" /><label for="star1" title="1 نجمة">★</label>
            </div>
            <p id="rating-message" class="text-xl text-green-300 font-semibold hidden">
                شكرًا لإبداعك يا نجم!
            </p>
        </div>
    </div>

    <footer class="w-full p-6 bg-gradient-to-t from-gray-900 to-transparent text-gray-400 text-sm mt-auto">
        <div class="text-center mb-2">
            <p class="text-lg font-semibold text-purple-300">إدريس محمد بشار خضر</p>
            <p class="text-sm text-gray-500">idreskhdr@gmail.com</p>
        </div>
        <div class="scrolling-text-container border-t border-gray-700 pt-2 mt-2">
            <div class="scrolling-text text-gray-500 text-sm">
                صوتك هو القوة • الإبداع هو الطريق • النجومية تبدأ من هنا • حوّل أفكارك إلى أصوات ساحرة • اكتشف إمكانيات صوتك الحقيقية • IDRES KHDR Voice Studio •
            </div>
        </div>
    </footer>

    <script>
        // Get elements
        const landingPage = document.getElementById('landing-page');
        const startNowButton = document.getElementById('start-now-button');
        const youtubeModal = document.getElementById('youtube-modal');
        const youtubeModalContent = youtubeModal.querySelector('.modal-content');
        const subscribeButton = document.getElementById('subscribe-button');
        const confirmSubscriptionButton = document.getElementById('confirm-subscription-button');
        const subscriptionMessage = document.getElementById('subscription-message');
        const ttsPage = document.getElementById('tts-page');
        const textInput = document.getElementById('text-input');
        const voiceSelect = document.getElementById('voice-select');
        const generateAudioButton = document.getElementById('generate-audio-button');
        const audioOutput = document.getElementById('audio-output');
        const playAudioButton = document.getElementById('play-audio-button');
        const loadingIndicator = document.getElementById('loading-indicator');
        const ratingModal = document.getElementById('rating-modal');
        const ratingModalContent = ratingModal.querySelector('.modal-content');
        const ratingStars = document.querySelectorAll('.star-rating input[type="radio"]');
        const ratingMessage = document.getElementById('rating-message');

        // State variables
        let hasVisitedYouTube = false; // New state to track if user has clicked the YouTube link
        let hasRated = false;
        let audioPlayedOnce = false;
        const synth = window.speechSynthesis; // Web Speech API SpeechSynthesis object
        let currentUtterance = null; // To store the current speech utterance

        // YouTube channel URL (UPDATED)
        const YOUTUBE_CHANNEL_URL = 'https://youtube.com/@idres_khdr?si=B3SLv6EfAWKM7Nud'; 

        // Function to show a modal with animation
        function showModal(modalElement, contentElement) {
            modalElement.classList.remove('hidden');
            setTimeout(() => {
                contentElement.classList.remove('scale-95', 'opacity-0');
                contentElement.classList.add('scale-100', 'opacity-100');
            }, 10); // Small delay to trigger transition
        }

        // Function to hide a modal with animation
        function hideModal(modalElement, contentElement) {
            contentElement.classList.remove('scale-100', 'opacity-100');
            contentElement.classList.add('scale-95', 'opacity-0');
            setTimeout(() => {
                modalElement.classList.add('hidden');
            }, 300); // Match transition duration
        }

        // Function to display a temporary message (replaces alert())
        function showMessageBox(message, type = 'error') {
            const messageBox = document.createElement('div');
            messageBox.className = `fixed top-4 right-4 p-4 rounded-lg shadow-lg z-50 text-white ${type === 'error' ? 'bg-red-700' : 'bg-green-700'}`;
            messageBox.textContent = message;
            document.body.appendChild(messageBox);
            setTimeout(() => {
                messageBox.remove();
            }, 3000); // Remove after 3 seconds
        }

        // Populate voices for TTS
        function populateVoiceList() {
            const voices = synth.getVoices();
            // Clear existing options except the first "Choose voice"
            voiceSelect.innerHTML = '<option value="">اختر صوتًا احترافيًا</option>';

            // Filter for Arabic voices first, then add others
            const arabicVoices = voices.filter(voice => voice.lang.startsWith('ar'));
            const otherVoices = voices.filter(voice => !voice.lang.startsWith('ar'));

            arabicVoices.forEach(voice => {
                const option = document.createElement('option');
                option.textContent = `${voice.name} (${voice.lang})`;
                option.value = voice.name;
                // Try to set a default Arabic voice if available
                if (voice.default) {
                    option.selected = true;
                }
                voiceSelect.appendChild(option);
            });

            otherVoices.forEach(voice => {
                const option = document.createElement('option');
                option.textContent = `${voice.name} (${voice.lang})`;
                option.value = voice.name;
                voiceSelect.appendChild(option);
            });

            // If no Arabic voices found, try to select a default system voice
            if (arabicVoices.length === 0 && voices.length > 0) {
                voiceSelect.value = voices[0].name;
            }
        }

        // Event listener for voiceschanged (important for populating voices)
        synth.onvoiceschanged = populateVoiceList;
        // Call it once in case voices are already loaded
        populateVoiceList();


        // Event listener for "ابدأ الآن" button
        startNowButton.addEventListener('click', () => {
            showModal(youtubeModal, youtubeModalContent);
            // Reset state for new session
            hasVisitedYouTube = false;
            subscribeButton.disabled = false;
            subscribeButton.classList.remove('bg-gray-500', 'cursor-not-allowed');
            confirmSubscriptionButton.classList.add('hidden');
            subscriptionMessage.classList.add('hidden');
        });

        // Event listener for "اشترك الآن" button in YouTube modal
        subscribeButton.addEventListener('click', () => {
            window.open(YOUTUBE_CHANNEL_URL, '_blank'); // Open YouTube channel in a new tab
            hasVisitedYouTube = true; // Mark that user has visited YouTube
            subscribeButton.classList.add('bg-gray-500', 'cursor-not-allowed'); // Visually indicate clicked
            subscribeButton.disabled = true; // Disable to prevent multiple clicks
            confirmSubscriptionButton.classList.remove('hidden'); // Show continue button
            subscriptionMessage.classList.add('hidden'); // Hide any previous error message
        });

        // Event listener for "تأكيد الاشتراك والمتابعة" button
        confirmSubscriptionButton.addEventListener('click', () => {
            if (hasVisitedYouTube) {
                hideModal(youtubeModal, youtubeModalContent);
                landingPage.classList.add('hidden');
                ttsPage.classList.remove('hidden');
                ttsPage.classList.add('flex'); // Ensure flex display for centering
            } else {
                subscriptionMessage.classList.remove('hidden'); // Show message if YouTube not visited
            }
        });

        // Event listener for "إنشاء الصوت" button
        generateAudioButton.addEventListener('click', () => {
            const text = textInput.value.trim();
            const selectedVoiceName = voiceSelect.value;

            if (text === "") {
                showMessageBox('الرجاء إدخال النص لتحويله إلى صوت.', 'error');
                return;
            }
            if (selectedVoiceName === "") {
                showMessageBox('الرجاء اختيار صوت احترافي.', 'error');
                return;
            }

            // Cancel any ongoing speech
            if (synth.speaking) {
                synth.cancel();
            }

            // Hide previous audio output and show loading
            playAudioButton.classList.add('hidden');
            loadingIndicator.classList.remove('hidden');
            generateAudioButton.disabled = true; // Disable button during generation

            currentUtterance = new SpeechSynthesisUtterance(text);
            const voices = synth.getVoices();
            const selectedVoice = voices.find(voice => voice.name === selectedVoiceName);

            if (selectedVoice) {
                currentUtterance.voice = selectedVoice;
            } else {
                showMessageBox('الصوت المختار غير متاح في متصفحك. سيتم استخدام الصوت الافتراضي.', 'warning');
            }

            currentUtterance.onend = () => {
                loadingIndicator.classList.add('hidden');
                generateAudioButton.disabled = false;
                playAudioButton.classList.remove('hidden'); // Show play button
                
                // Show rating modal only after first successful audio playback and if not yet rated
                if (!audioPlayedOnce && !hasRated) {
                    audioPlayedOnce = true;
                    showModal(ratingModal, ratingModalContent);
                }
            };

            currentUtterance.onerror = (event) => {
                loadingIndicator.classList.add('hidden');
                generateAudioButton.disabled = false;
                showMessageBox(`حدث خطأ أثناء إنشاء الصوت: ${event.error}`, 'error');
                console.error('SpeechSynthesisUtterance error:', event);
            };

            synth.speak(currentUtterance);
        });

        // Event listener for "تشغيل الصوت" button (to replay the last generated speech)
        playAudioButton.addEventListener('click', () => {
            if (currentUtterance) {
                // If speech is already playing, cancel it and restart
                if (synth.speaking && synth.paused) {
                    synth.resume();
                } else if (!synth.speaking) {
                    synth.speak(currentUtterance);
                }
            } else {
                showMessageBox('يرجى إنشاء الصوت أولاً.', 'error');
            }
        });


        // Event listeners for rating stars
        ratingStars.forEach(star => {
            star.addEventListener('change', () => {
                if (!hasRated) {
                    hasRated = true;
                    ratingMessage.classList.remove('hidden');
                    showMessageBox('شكرًا لإبداعك يا نجم!', 'success');
                    // Disable stars after rating
                    ratingStars.forEach(s => s.disabled = true);
                    // Hide modal after a short delay
                    setTimeout(() => {
                        hideModal(ratingModal, ratingModalContent);
                    }, 2000);
                }
            });
        });

        // Ensure modals are hidden on initial load
        window.addEventListener('load', () => {
            youtubeModal.classList.add('hidden');
            ratingModal.classList.add('hidden');
        });

    </script>
</body>
</html>

