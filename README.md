<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Camera Game - Bóng Bay Tiếng Anh</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* ===== RESET VÀ CƠ BẢN ===== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            color: white;
            height: 100vh;
            overflow: hidden;
            position: relative;
        }

        /* ===== VIDEO VÀ NỀN ===== */
        .video-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 2;
            opacity: 0.7;
        }
        
        #video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transform: scaleX(-1); /* Sửa lỗi màn hình ngược */
        }
        
        .video-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(15, 12, 41, 0.8), rgba(48, 43, 99, 0.8));
            z-index: 3;
            mix-blend-mode: overlay;
        }

        /* ===== MENU CHÍNH ===== */
        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(10, 8, 25, 0.95);
            z-index: 10;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: opacity 0.5s ease;
        }
        
        .logo {
            font-size: 4.5rem;
            margin-bottom: 20px;
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4, #45b7d1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-weight: 800;
            letter-spacing: 3px;
            text-shadow: 0 0 30px rgba(78, 205, 196, 0.5);
            text-align: center;
        }
        
        .tagline {
            font-size: 1.3rem;
            margin-bottom: 50px;
            color: rgba(255, 255, 255, 0.8);
            text-align: center;
            max-width: 600px;
            line-height: 1.6;
        }

        /* ===== CHỌN ĐỘ KHÓ ===== */
        .difficulty-selector {
            margin-bottom: 40px;
            text-align: center;
        }
        
        .difficulty-title {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #4ecdc4;
        }
        
        .difficulty-buttons {
            display: flex;
            gap: 20px;
            justify-content: center;
        }
        
        .difficulty-btn {
            padding: 15px 30px;
            border: none;
            border-radius: 50px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
            border: 2px solid transparent;
        }
        
        .difficulty-btn:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
        }
        
        .difficulty-btn.active {
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4);
            border-color: rgba(255, 255, 255, 0.3);
            box-shadow: 0 0 20px rgba(255, 107, 107, 0.5);
        }

        /* ===== NÚT CHÍNH ===== */
        .buttons {
            display: flex;
            gap: 25px;
            margin-top: 20px;
        }
        
        .btn {
            padding: 18px 40px;
            border: none;
            border-radius: 50px;
            background: rgba(255, 255, 255, 0.15);
            color: white;
            font-size: 1.3rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.4s ease;
            backdrop-filter: blur(15px);
            display: flex;
            align-items: center;
            gap: 12px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
            border: 2px solid rgba(255, 255, 255, 0.2);
        }
        
        .btn:hover {
            background: rgba(255, 255, 255, 0.25);
            transform: translateY(-5px);
            box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4);
        }
        
        .primary-btn {
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4);
        }
        
        .primary-btn:hover {
            background: linear-gradient(135deg, #ff5252, #3dbeb6);
        }

        /* ===== MODAL/PANEL ===== */
        .mode-menu, .settings-panel, .question-panel {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(20, 18, 40, 0.95);
            border-radius: 25px;
            padding: 40px;
            width: 90%;
            max-width: 600px;
            z-index: 20;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255, 255, 255, 0.1);
            display: none;
        }
        
        .mode-menu h2, .settings-panel h2, .question-panel h2 {
            text-align: center;
            margin-bottom: 30px;
            font-size: 2.2rem;
            color: #4ecdc4;
        }

        /* ===== CHẾ ĐỘ CHƠI ===== */
        .mode-options {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        
        .mode-btn {
            padding: 20px;
            border: none;
            border-radius: 15px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 1.3rem;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }
        
        .mode-btn:hover {
            background: rgba(78, 205, 196, 0.3);
            transform: translateY(-3px);
        }
        
        .mode-btn.active {
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4);
        }

        /* ===== CÀI ĐẶT ===== */
        .settings-content {
            display: flex;
            flex-direction: column;
            gap: 25px;
        }
        
        .setting-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .setting-label {
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        
        .slider {
            width: 180px;
            height: 8px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 5px;
            outline: none;
        }
        
        .slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 24px;
            height: 24px;
            border-radius: 50%;
            background: #4ecdc4;
            cursor: pointer;
        }

        /* ===== NÚT ĐÓNG ===== */
        .close-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .close-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        /* ===== ĐIỀU KHIỂN GAME ===== */
        .controls {
            position: absolute;
            bottom: 40px;
            left: 0;
            width: 100%;
            display: flex;
            justify-content: center;
            gap: 30px;
            z-index: 5;
        }
        
        .control-btn {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            border: none;
            background: rgba(255, 255, 255, 0.15);
            color: white;
            font-size: 1.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
            backdrop-filter: blur(15px);
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .control-btn:hover {
            background: rgba(255, 255, 255, 0.25);
            transform: scale(1.1);
        }
        
        .record-btn {
            background: rgba(255, 0, 0, 0.7);
            width: 90px;
            height: 90px;
        }
        
        .record-btn.recording {
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(255, 0, 0, 0.7); }
            70% { box-shadow: 0 0 0 25px rgba(255, 0, 0, 0); }
            100% { box-shadow: 0 0 0 0 rgba(255, 0, 0, 0); }
        }

        /* ===== THANH TRẠNG THÁI ===== */
        .status-bar {
            position: absolute;
            top: 25px;
            left: 0;
            width: 100%;
            display: flex;
            justify-content: space-between;
            padding: 0 30px;
            z-index: 5;
        }
        
        .mode-indicator, .score-display, .timer {
            background: rgba(0, 0, 0, 0.5);
            padding: 12px 25px;
            border-radius: 30px;
            font-size: 1.1rem;
            backdrop-filter: blur(15px);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        /* ===== BÓNG BAY ===== */
        .balloon {
            position: absolute;
            width: 80px;
            height: 100px;
            cursor: pointer;
            z-index: 6;
            transition: transform 0.3s;
            display: flex;
            justify-content: center;
            align-items: center;
            font-weight: bold;
            font-size: 1.6rem;
            animation: float 8s ease-in-out infinite;
            border-radius: 50%;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        
        .balloon:hover {
            transform: scale(1.2);
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-30px) rotate(5deg); }
        }

        .balloon.danger {
            animation: floatDanger 6s ease-in-out infinite;
            border: 3px solid #ff4444;
            box-shadow: 0 0 20px #ff4444;
        }
        
        @keyframes floatDanger {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-40px) rotate(8deg); }
        }

        /* ===== CÂU HỎI ===== */
        .question-content {
            text-align: center;
        }
        
        .question-text {
            font-size: 1.5rem;
            margin-bottom: 30px;
            line-height: 1.6;
            background: rgba(255, 255, 255, 0.05);
            padding: 25px;
            border-radius: 15px;
        }
        
        .answer-input {
            width: 100%;
            padding: 18px 25px;
            border: none;
            border-radius: 15px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 1.3rem;
            margin-bottom: 30px;
            text-align: center;
        }
        
        .submit-btn {
            width: 100%;
            padding: 18px;
            border: none;
            border-radius: 15px;
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4);
            color: white;
            font-size: 1.3rem;
            font-weight: 600;
            cursor: pointer;
        }
        
        .feedback {
            margin-top: 25px;
            font-size: 1.3rem;
            font-weight: 600;
            padding: 15px;
            border-radius: 12px;
        }
        
        .correct {
            color: #4CAF50;
            background: rgba(76, 175, 80, 0.1);
        }
        
        .incorrect {
            color: #f44336;
            background: rgba(244, 67, 54, 0.1);
        }

        .penalty {
            color: #ff9800;
            background: rgba(255, 152, 0, 0.1);
            animation: penaltyFlash 0.5s 3;
        }
        
        @keyframes penaltyFlash {
            0%, 100% { background: rgba(255, 152, 0, 0.1); }
            50% { background: rgba(255, 152, 0, 0.3); }
        }

        .hidden {
            display: none;
        }

        /* ===== THÔNG BÁO TỨC THỜI ===== */
        .notification {
            position: fixed;
            top: 100px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(255, 68, 68, 0.9);
            color: white;
            padding: 15px 30px;
            border-radius: 25px;
            font-weight: bold;
            z-index: 100;
            animation: slideDown 0.5s ease;
        }
        
        @keyframes slideDown {
            from { top: -100px; }
            to { top: 100px; }
        }
    </style>
</head>
<body>
    <!-- VIDEO VÀ NỀN -->
    <div class="video-container">
        <video id="video" autoplay playsinline></video>
        <div class="video-overlay"></div>
    </div>
    
    <!-- KHU VỰC BÓNG BAY -->
    <div id="balloons-container"></div>
    
    <!-- MENU CHÍNH -->
    <div class="overlay" id="overlay">
        <div class="logo"><i class="fas fa-gamepad"></i> CAMERA GAME</div>
        <p class="tagline">Học tiếng Anh thật vui với trò chơi bóng bay tương tác!<br>Chạm vào bóng bay để trả lời câu hỏi, tránh bóng bay chạm cạnh!</p>
        
        <div class="difficulty-selector">
            <div class="difficulty-title">Chọn Độ Khó</div>
            <div class="difficulty-buttons">
                <button class="difficulty-btn active" data-difficulty="easy">
                    <i class="fas fa-star"></i> DỄ
                </button>
                <button class="difficulty-btn" data-difficulty="medium">
                    <i class="fas fa-star-half-alt"></i> TRUNG BÌNH
                </button>
                <button class="difficulty-btn" data-difficulty="hard">
                    <i class="fas fa-fire"></i> KHÓ
                </button>
            </div>
        </div>
        
        <div class="buttons">
            <button class="btn primary-btn" id="playBtn">
                <i class="fas fa-play-circle"></i> BẮT ĐẦU
            </button>
            <button class="btn" id="settingsBtn">
                <i class="fas fa-cog"></i> CÀI ĐẶT
            </button>
        </div>
    </div>
    
    <!-- MENU CHẾ ĐỘ -->
    <div class="mode-menu" id="modeMenu">
        <h2><i class="fas fa-gamepad"></i> CHỌN CHẾ ĐỘ</h2>
        <div class="mode-options">
            <button class="mode-btn" data-mode="easy">
                <i class="fas fa-star"></i> CHẾ ĐỘ DỄ
            </button>
            <button class="mode-btn" data-mode="medium">
                <i class="fas fa-star-half-alt"></i> CHẾ ĐỘ TRUNG BÌNH
            </button>
            <button class="mode-btn" data-mode="hard">
                <i class="fas fa-fire"></i> CHẾ ĐỘ KHÓ
            </button>
        </div>
        <button class="close-btn" id="closeModeMenu">
            <i class="fas fa-times"></i>
        </button>
    </div>
    
    <!-- CÀI ĐẶT -->
    <div class="settings-panel" id="settingsPanel">
        <h2><i class="fas fa-sliders-h"></i> CÀI ĐẶT</h2>
        <div class="settings-content">
            <div class="setting-item">
                <span class="setting-label">
                    <i class="fas fa-volume-up"></i> Âm lượng
                </span>
                <input type="range" min="0" max="100" value="80" class="slider" id="volumeSlider">
            </div>
            <div class="setting-item">
                <span class="setting-label">
                    <i class="fas fa-hd"></i> Chất lượng
                </span>
                <select id="qualitySelect">
                    <option value="low">Thấp</option>
                    <option value="medium" selected>Trung bình</option>
                    <option value="high">Cao</option>
                </select>
            </div>
            <div class="setting-item">
                <span class="setting-label">
                    <i class="fas fa-paint-brush"></i> Hiệu ứng
                </span>
                <select id="effectSelect">
                    <option value="none">Không</option>
                    <option value="sepia">Sepia</option>
                    <option value="grayscale">Đen trắng</option>
                </select>
            </div>
        </div>
        <button class="close-btn" id="closeSettings">
            <i class="fas fa-times"></i>
        </button>
    </div>
    
    <!-- CÂU HỎI -->
    <div class="question-panel" id="questionPanel">
        <h2><i class="fas fa-question-circle"></i> CÂU HỎI TIẾNG ANH</h2>
        <div class="question-content">
            <div class="question-text" id="questionText">Câu hỏi sẽ hiển thị ở đây</div>
            <input type="text" class="answer-input" id="answerInput" placeholder="Nhập câu trả lời của bạn...">
            <button class="submit-btn" id="submitAnswer">
                <i class="fas fa-paper-plane"></i> GỬI ĐÁP ÁN
            </button>
            <div class="feedback" id="feedback"></div>
        </div>
        <button class="close-btn" id="closeQuestion">
            <i class="fas fa-times"></i>
        </button>
    </div>
    
    <!-- THANH TRẠNG THÁI -->
    <div class="status-bar">
        <div class="mode-indicator" id="modeIndicator">
            <i class="fas fa-flag"></i> Chế độ: Chưa chọn
        </div>
        <div class="score-display" id="scoreDisplay">
            <i class="fas fa-trophy"></i> Điểm: 0
        </div>
        <div class="timer" id="timer">
            <i class="fas fa-clock"></i> 00:00:00
        </div>
    </div>
    
    <!-- ĐIỀU KHIỂN -->
    <div class="controls hidden" id="controls">
        <button class="control-btn" id="switchCameraBtn">
            <i class="fas fa-sync-alt"></i>
        </button>
        <button class="control-btn record-btn" id="recordBtn">
            <i class="fas fa-video"></i>
        </button>
        <button class="control-btn" id="toggleSettingsBtn">
            <i class="fas fa-cog"></i>
        </button>
    </div>

    <script>
        // =============================================================================
        // PHẦN CÂU HỎI TIẾNG ANH - CÓ THỂ CHỈNH SỬA Ở ĐÂY
        // =============================================================================
        
        // THÊM CÂU HỎI MỚI: Copy mẫu dưới và dán vào mảng englishQuestions
        // {
        //     question: "Câu hỏi của bạn ở đây?",
        //     answer: "câu trả lời ở đây"
        // },
        
        const englishQuestions = [
            // ===== CÂU HỎI DỄ =====
            {
                question: "What is the past tense of 'go'?",
                answer: "went",
                difficulty: "easy"
            },
            {
                question: "What is the plural of 'child'?",
                answer: "children",
                difficulty: "easy"
            },
            {
                question: "What color is the sky?",
                answer: "blue",
                difficulty: "easy"
            },
            {
                question: "How many days are in a week?",
                answer: "7",
                difficulty: "easy"
            },
            
            // ===== CÂU HỎI TRUNG BÌNH =====
            {
                question: "Complete the sentence: She ___ to school every day.",
                answer: "goes",
                difficulty: "medium"
            },
            {
                question: "What is the opposite of 'beautiful'?",
                answer: "ugly",
                difficulty: "medium"
            },
            {
                question: "What do you call a baby dog?",
                answer: "puppy",
                difficulty: "medium"
            },
            {
                question: "What is 3 times 4?",
                answer: "12",
                difficulty: "medium"
            },
            
            // ===== CÂU HỎI KHÓ =====
            {
                question: "What is the capital of England?",
                answer: "london",
                difficulty: "hard"
            },
            {
                question: "What is the largest mammal in the world?",
                answer: "whale",
                difficulty: "hard"
            },
            {
                question: "Which planet is known as the Red Planet?",
                answer: "mars",
                difficulty: "hard"
            },
            {
                question: "What is the main language spoken in Brazil?",
                answer: "portuguese",
                difficulty: "hard"
            },
            {
                question: "What is the chemical symbol for gold?",
                answer: "au",
                difficulty: "hard"
            },
            {
                question: "Who wrote 'Romeo and Juliet'?",
                answer: "shakespeare",
                difficulty: "hard"
            }
        ];
        
        // =============================================================================
        // BIẾN TOÀN CỤC - KHÔNG CẦN CHỈNH SỬA
        // =============================================================================
        let stream = null;
        let mediaRecorder = null;
        let recordedChunks = [];
        let isRecording = false;
        let useFrontCamera = true;
        let currentMode = 'easy';
        let recordingTime = 0;
        let timerInterval = null;
        let score = 0;
        let balloons = [];
        let currentQuestion = null;
        let balloonInterval = null;
        let gameStarted = false;
        let balloonMoveInterval = null;

        // =============================================================================
        // HÀM KHỞI TẠO ỨNG DỤNG - KHÔNG CẦN CHỈNH SỬA
        // =============================================================================
        document.addEventListener('DOMContentLoaded', function() {
            setupEventListeners();
            createParticles();
            loadGameSettings();
        });

        // =============================================================================
        // THIẾT LẬP SỰ KIỆN
        // =============================================================================
        function setupEventListeners() {
            document.getElementById('playBtn').addEventListener('click', showModeMenu);
            document.getElementById('settingsBtn').addEventListener('click', showSettings);
            
            document.getElementById('closeModeMenu').addEventListener('click', closeModeMenu);
            document.getElementById('closeSettings').addEventListener('click', closeSettings);
            document.getElementById('closeQuestion').addEventListener('click', closeQuestion);
            
            document.querySelectorAll('.mode-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    selectMode(this.dataset.mode);
                });
            });
            
            document.querySelectorAll('.difficulty-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    selectDifficulty(this.dataset.difficulty);
                });
            });
            
            document.getElementById('submitAnswer').addEventListener('click', checkAnswer);
            document.getElementById('answerInput').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') checkAnswer();
            });
            
            document.getElementById('recordBtn').addEventListener('click', toggleRecording);
            document.getElementById('switchCameraBtn').addEventListener('click', switchCamera);
            document.getElementById('toggleSettingsBtn').addEventListener('click', showSettings);
            
            document.getElementById('volumeSlider').addEventListener('input', updateVolume);
            document.getElementById('effectSelect').addEventListener('change', applyEffect);
        }

        // =============================================================================
        // HIỆU ỨNG HẠT NỀN
        // =============================================================================
        function createParticles() {
            const container = document.getElementById('balloons-container');
            for (let i = 0; i < 20; i++) {
                const particle = document.createElement('div');
                particle.style.cssText = `
                    position: absolute;
                    width: ${Math.random() * 6 + 2}px;
                    height: ${Math.random() * 6 + 2}px;
                    background: ${['#ff6b6b, #4ecdc4, #45b7d1'][Math.floor(Math.random() * 3)]};
                    border-radius: 50%;
                    animation: float ${Math.random() * 20 + 10}s linear infinite;
                    left: ${Math.random() * 100}%;
                    animation-delay: ${Math.random() * 10}s;
                    opacity: ${Math.random() * 0.5 + 0.2};
                `;
                container.appendChild(particle);
            }
        }

        // =============================================================================
        // HIỂN THỊ MODAL
        // =============================================================================
        function showModeMenu() {
            document.getElementById('modeMenu').style.display = 'block';
        }

        function closeModeMenu() {
            document.getElementById('modeMenu').style.display = 'none';
        }

        function showSettings() {
            document.getElementById('settingsPanel').style.display = 'block';
        }

        function closeSettings() {
            document.getElementById('settingsPanel').style.display = 'none';
        }

        function closeQuestion() {
            document.getElementById('questionPanel').style.display = 'none';
            document.getElementById('feedback').textContent = '';
            document.getElementById('answerInput').value = '';
        }

        // =============================================================================
        // CHỌN CHẾ ĐỘ VÀ ĐỘ KHÓ
        // =============================================================================
        function selectMode(mode) {
            currentMode = mode;
            
            document.querySelectorAll('.mode-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            closeModeMenu();
            startGame();
        }

        function selectDifficulty(difficulty) {
            document.querySelectorAll('.difficulty-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            localStorage.setItem('gameDifficulty', difficulty);
        }

        // =============================================================================
        // BẮT ĐẦU GAME
        // =============================================================================
        function startGame() {
            const overlay = document.getElementById('overlay');
            const controls = document.getElementById('controls');
            
            overlay.style.opacity = '0';
            setTimeout(() => {
                overlay.style.display = 'none';
                controls.classList.remove('hidden');
            }, 500);
            
            initGame();
        }

        function initGame() {
            gameStarted = true;
            score = 0;
            updateScore();
            
            startCamera();
            startBalloonGame();
            startTimer();
            updateModeIndicator();
        }

        // =============================================================================
        // CAMERA VÀ GHI HÌNH
        // =============================================================================
        async function startCamera() {
            try {
                const constraints = {
                    video: {
                        facingMode: useFrontCamera ? 'user' : 'environment',
                        width: { ideal: 1280 },
                        height: { ideal: 720 }
                    },
                    audio: true
                };
                
                stream = await navigator.mediaDevices.getUserMedia(constraints);
                document.getElementById('video').srcObject = stream;
                
                setupMediaRecorder(stream);
                
            } catch (error) {
                console.error('Lỗi camera:', error);
                alert('Không thể truy cập camera. Vui lòng cho phép quyền camera và tải lại trang.');
            }
        }

        function setupMediaRecorder(stream) {
            recordedChunks = [];
            
            try {
                mediaRecorder = new MediaRecorder(stream, { mimeType: 'video/webm; codecs=vp9' });
            } catch (e) {
                try {
                    mediaRecorder = new MediaRecorder(stream, { mimeType: 'video/webm; codecs=vp8' });
                } catch (e) {
                    mediaRecorder = new MediaRecorder(stream);
                }
            }
            
            mediaRecorder.ondataavailable = function(event) {
                if (event.data.size > 0) {
                    recordedChunks.push(event.data);
                }
            };
            
            mediaRecorder.onstop = function() {
                const blob = new Blob(recordedChunks, { type: mediaRecorder.mimeType });
                const url = URL.createObjectURL(blob);
                
                const a = document.createElement('a');
                a.href = url;
                a.download = `recording-${new Date().getTime()}.mp4`;
                a.click();
                
                setTimeout(() => URL.revokeObjectURL(url), 100);
            };
        }

        // =============================================================================
        // TRÒ CHƠI BÓNG BAY - ĐÃ CẬP NHẬT TỐC ĐỘ THEO YÊU CẦU
        // =============================================================================
        function startBalloonGame() {
            clearBalloons();
            
            // TỐC ĐỘ TẠO BÓNG BAY THEO ĐỘ KHÓ - CHẾ ĐỘ KHÓ 5 GIÂY MỚI XUẤT HIỆN
            let intervalTime;
            switch(currentMode) {
                case 'easy': intervalTime = 3000; break;    // 3 giây
                case 'medium': intervalTime = 4000; break;  // 4 giây  
                case 'hard': intervalTime = 5000; break;    // 5 giây - ĐÃ CHỈNH SỬA
            }
            
            balloonInterval = setInterval(createBalloon, intervalTime);
            
            // Bắt đầu di chuyển bóng bay
            startBalloonMovement();
            
            // Tạo bóng bay ban đầu
            for (let i = 0; i < 2; i++) {
                setTimeout(() => createBalloon(), i * 1000);
            }
        }

        function createBalloon() {
            if (!gameStarted || balloons.length >= 8) return;
            
            const balloon = document.createElement('div');
            balloon.className = 'balloon';
            
            // Tỷ lệ bóng bay nguy hiểm theo độ khó
            let isDangerous = false;
            switch(currentMode) {
                case 'easy': isDangerous = Math.random() < 0.2; break;    // 20% nguy hiểm
                case 'medium': isDangerous = Math.random() < 0.4; break;  // 40% nguy hiểm
                case 'hard': isDangerous = Math.random() < 0.6; break;    // 60% nguy hiểm - ĐÃ CHỈNH SỬA
            }
            
            if (isDangerous) {
                balloon.classList.add('danger');
            }
            
            // Màu sắc bóng bay
            const colors = [
                'radial-gradient(circle at 30% 30%, #ff6b6b, #ff8e8e)',
                'radial-gradient(circle at 30% 30%, #4ecdc4, #6ed9d1)',
                'radial-gradient(circle at 30% 30%, #45b7d1, #67c7e0)',
                'radial-gradient(circle at 30% 30%, #96ceb4, #b4dec9)',
                'radial-gradient(circle at 30% 30%, #ffeaa7, #fff0c1)',
                'radial-gradient(circle at 30% 30%, #dda0dd, #e6b3e6)'
            ];
            balloon.style.background = isDangerous 
                ? 'radial-gradient(circle at 30% 30%, #ff4444, #ff6666)'
                : colors[Math.floor(Math.random() * colors.length)];
            
            // Vị trí ngẫu nhiên - tránh cạnh
            const margin = 50;
            const maxX = window.innerWidth - 100 - margin * 2;
            const maxY = window.innerHeight - 150 - margin * 2;
            const x = Math.floor(Math.random() * maxX) + margin;
            const y = Math.floor(Math.random() * maxY) + margin;
            
            balloon.style.left = `${x}px`;
            balloon.style.top = `${y}px`;
            
            // Tốc độ di chuyển
            const speed = isDangerous ? 2 : 1;
            balloon.dataset.speedX = (Math.random() - 0.5) * speed;
            balloon.dataset.speedY = (Math.random() - 0.5) * speed;
            balloon.dataset.isDangerous = isDangerous;
            
            // Chữ cái ngẫu nhiên
            const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
            const letter = letters[Math.floor(Math.random() * letters.length)];
            balloon.textContent = letter;
            
            // Sự kiện click - ĐÃ SỬA: Thêm sự kiện touchstart cho thiết bị cảm ứng
            balloon.addEventListener('click', function() {
                onBalloonClick(this);
            });
            balloon.addEventListener('touchstart', function(e) {
                e.preventDefault(); // Ngăn hành vi mặc định
                onBalloonClick(this);
            });
            
            document.getElementById('balloons-container').appendChild(balloon);
            balloons.push(balloon);
            
            // Tự động xóa sau 15 giây
            setTimeout(() => {
                if (balloon.parentNode) {
                    removeBalloon(balloon);
                }
            }, 15000);
        }

        // =============================================================================
        // DI CHUYỂN BÓNG BAY VÀ KIỂM TRA CHẠM CẠNH - TÍNH NĂNG MỚI
        // =============================================================================
        function startBalloonMovement() {
            balloonMoveInterval = setInterval(() => {
                balloons.forEach(balloon => {
                    moveBalloon(balloon);
                    checkBalloonEdge(balloon);
                });
            }, 50); // Cập nhật vị trí mỗi 50ms
        }

        function moveBalloon(balloon) {
            const speedX = parseFloat(balloon.dataset.speedX);
            const speedY = parseFloat(balloon.dataset.speedY);
            
            let currentX = parseFloat(balloon.style.left);
            let currentY = parseFloat(balloon.style.top);
            
            // Cập nhật vị trí
            balloon.style.left = `${currentX + speedX}px`;
            balloon.style.top = `${currentY + speedY}px`;
        }

        function checkBalloonEdge(balloon) {
            const rect = balloon.getBoundingClientRect();
            const isDangerous = balloon.dataset.isDangerous === 'true';
            
            // Kiểm tra chạm cạnh
            if (rect.left <= 0 || rect.right >= window.innerWidth || 
                rect.top <= 0 || rect.bottom >= window.innerHeight) {
                
                if (isDangerous) {
                    // Bóng bay nguy hiểm chạm cạnh - trừ điểm
                    applyPenalty(balloon);
                } else {
                    // Đổi hướng bóng bay thường
                    bounceBalloon(balloon);
                }
            }
        }

        function bounceBalloon(balloon) {
            const rect = balloon.getBoundingClientRect();
            
            // Đổi hướng khi chạm cạnh
            if (rect.left <= 0 || rect.right >= window.innerWidth) {
                balloon.dataset.speedX = -parseFloat(balloon.dataset.speedX);
            }
            if (rect.top <= 0 || rect.bottom >= window.innerHeight) {
                balloon.dataset.speedY = -parseFloat(balloon.dataset.speedY);
            }
        }

        function applyPenalty(balloon) {
            if (!balloon.penaltyApplied) {
                balloon.penaltyApplied = true;
                
                // Trừ 10 điểm
                score = Math.max(0, score - 10);
                updateScore();
                
                // Hiệu ứng trừ điểm
                showPenaltyNotification();
                
                // Hiệu ứng visual
                balloon.style.animation = 'none';
                setTimeout(() => {
                    balloon.style.animation = 'floatDanger 6s ease-in-out infinite';
                }, 100);
                
                // Xóa bóng bay sau khi trừ điểm
                setTimeout(() => {
                    if (balloon.parentNode) {
                        removeBalloon(balloon);
                    }
                }, 1000);
            }
        }

        function showPenaltyNotification() {
            const notification = document.createElement('div');
            notification.className = 'notification';
            notification.innerHTML = '<i class="fas fa-exclamation-triangle"></i> -10 điểm! Bóng bay chạm cạnh!';
            document.body.appendChild(notification);
            
            setTimeout(() => {
                notification.remove();
            }, 2000);
        }

        function onBalloonClick(balloon) {
            const isDangerous = balloon.dataset.isDangerous === 'true';
            
            if (!isDangerous) {
                // Hiệu ứng biến mất
                balloon.style.transform = 'scale(0)';
                balloon.style.opacity = '0';
                
                setTimeout(() => {
                    if (balloon.parentNode) {
                        balloon.remove();
                        balloons = balloons.filter(b => b !== balloon);
                    }
                }, 300);
                
                // Hiển thị câu hỏi - ĐÃ SỬA: Đảm bảo câu hỏi luôn hiển thị
                showRandomQuestion();
            } else {
                // Bóng bay nguy hiểm - không thể click
                balloon.style.animation = 'none';
                setTimeout(() => {
                    balloon.style.animation = 'floatDanger 6s ease-in-out infinite';
                }, 100);
            }
        }

        function removeBalloon(balloon) {
            if (balloon.parentNode) {
                balloon.style.transition = 'all 0.5s ease';
                balloon.style.transform = 'translateY(100px) scale(0)';
                balloon.style.opacity = '0';
                
                setTimeout(() => {
                    if (balloon.parentNode) {
                        balloon.remove();
                        balloons = balloons.filter(b => b !== balloon);
                    }
                }, 500);
            }
        }

        function clearBalloons() {
            document.getElementById('balloons-container').innerHTML = '';
            balloons = [];
            
            if (balloonInterval) {
                clearInterval(balloonInterval);
            }
            if (balloonMoveInterval) {
                clearInterval(balloonMoveInterval);
            }
        }

        // =============================================================================
        // HỆ THỐNG CÂU HỎI
        // =============================================================================
        function showRandomQuestion() {
            if (englishQuestions.length === 0) {
                alert('Chưa có câu hỏi nào! Vui lòng thêm câu hỏi trong code.');
                return;
            }
            
            // Lọc câu hỏi theo độ khó
            const filteredQuestions = englishQuestions.filter(q => 
                q.difficulty === currentMode || 
                (currentMode === 'hard' && q.difficulty === 'medium') ||
                (currentMode === 'medium' && q.difficulty === 'easy')
            );
            
            if (filteredQuestions.length === 0) {
                // Nếu không có câu hỏi phù hợp, dùng tất cả
                const question = englishQuestions[Math.floor(Math.random() * englishQuestions.length)];
                currentQuestion = question;
            } else {
                const question = filteredQuestions[Math.floor(Math.random() * filteredQuestions.length)];
                currentQuestion = question;
            }
            
            document.getElementById('questionText').textContent = currentQuestion.question;
            document.getElementById('questionPanel').style.display = 'block';
            document.getElementById('answerInput').focus();
        }

        function checkAnswer() {
            const userAnswer = document.getElementById('answerInput').value.trim().toLowerCase();
            const correctAnswer = currentQuestion.answer.toLowerCase();
            const feedback = document.getElementById('feedback');
            
            if (userAnswer === correctAnswer) {
                // ĐÚNG - +10 điểm
                feedback.innerHTML = '<i class="fas fa-check-circle"></i> Chính xác! +10 điểm';
                feedback.className = 'feedback correct';
                score += 10;
                updateScore();
                
                setTimeout(() => {
                    closeQuestion();
                }, 1500);
            } else {
                // SAI - không cộng điểm
                feedback.innerHTML = `<i class="fas fa-times-circle"></i> Sai rồi! Đáp án: ${currentQuestion.answer}`;
                feedback.className = 'feedback incorrect';
                
                setTimeout(() => {
                    closeQuestion();
                }, 2000);
            }
        }

        // =============================================================================
        // CẬP NHẬT GIAO DIỆN
        // =============================================================================
        function updateScore() {
            document.getElementById('scoreDisplay').innerHTML = `<i class="fas fa-trophy"></i> Điểm: ${score}`;
            
            // Hiệu ứng khi điểm thay đổi
            const scoreDisplay = document.getElementById('scoreDisplay');
            scoreDisplay.classList.add('penalty');
            setTimeout(() => {
                scoreDisplay.classList.remove('penalty');
            }, 500);
        }

        function updateModeIndicator() {
            let modeText = '';
            switch(currentMode) {
                case 'easy': modeText = 'DỄ'; break;
                case 'medium': modeText = 'TRUNG BÌNH'; break;
                case 'hard': modeText = 'KHÓ'; break;
            }
            document.getElementById('modeIndicator').innerHTML = `<i class="fas fa-flag"></i> Chế độ: ${modeText}`;
        }

        // =============================================================================
        // ĐIỀU KHIỂN GAME
        // =============================================================================
        function toggleRecording() {
            if (!mediaRecorder) return;
            
            const recordBtn = document.getElementById('recordBtn');
            
            if (!isRecording) {
                recordedChunks = [];
                mediaRecorder.start();
                isRecording = true;
                recordBtn.classList.add('recording');
                startTimer();
            } else {
                mediaRecorder.stop();
                isRecording = false;
                recordBtn.classList.remove('recording');
                stopTimer();
            }
        }

        function switchCamera() {
            useFrontCamera = !useFrontCamera;
            
            if (stream) {
                stream.getTracks().forEach(track => track.stop());
            }
            
            startCamera();
        }

        // =============================================================================
        // BỘ ĐẾM THỜI GIAN
        // =============================================================================
        function startTimer() {
            recordingTime = 0;
            timerInterval = setInterval(() => {
                recordingTime++;
                updateTimerDisplay();
            }, 1000);
        }

        function stopTimer() {
            if (timerInterval) {
                clearInterval(timerInterval);
            }
        }

        function updateTimerDisplay() {
            const hours = Math.floor(recordingTime / 3600);
            const minutes = Math.floor((recordingTime % 3600) / 60);
            const seconds = recordingTime % 60;
            
            document.getElementById('timer').innerHTML = 
                `<i class="fas fa-clock"></i> ${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        // =============================================================================
        // CÀI ĐẶT
        // =============================================================================
        function updateVolume() {
            const volume = document.getElementById('volumeSlider').value;
            console.log(`Âm lượng: ${volume}%`);
        }

        function applyEffect() {
            const effect = document.getElementById('effectSelect').value;
            const video = document.getElementById('video');
            
            switch(effect) {
                case 'sepia':
                    video.style.filter = 'sepia(1)';
                    break;
                case 'grayscale':
                    video.style.filter = 'grayscale(1)';
                    break;
                default:
                    video.style.filter = 'none';
            }
        }

        // =============================================================================
        // LƯU VÀ TẢI CÀI ĐẶT
        // =============================================================================
        function loadGameSettings() {
            const savedDifficulty = localStorage.getItem('gameDifficulty');
            if (savedDifficulty) {
                document.querySelector(`[data-difficulty="${savedDifficulty}"]`).click();
            }
        }

        // =============================================================================
        // DỌN DẸP KHI THOÁT
        // =============================================================================
        window.addEventListener('beforeunload', () => {
            if (stream) {
                stream.getTracks().forEach(track => track.stop());
            }
            clearBalloons();
            stopTimer();
        });

        // Xử lý resize window
        window.addEventListener('resize', () => {
            // Điều chỉnh vị trí bóng bay khi resize
            balloons.forEach(balloon => {
                const rect = balloon.getBoundingClientRect();
                if (rect.right > window.innerWidth) {
                    balloon.style.left = `${window.innerWidth - 100}px`;
                }
                if (rect.bottom > window.innerHeight) {
                    balloon.style.top = `${window.innerHeight - 150}px`;
                }
            });
        });
    </script>
</body>
</html>
