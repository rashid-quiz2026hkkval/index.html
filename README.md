<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Oral Histology Master Exam</title>
    <style>
        :root {
            --bg-color: #f0f4f8;
            --card-bg: #ffffff;
            --text-color: #333333;
            --primary-color: #0084d1;
            --border-color: #d1d5db;
        }

        body.dark-mode {
            --bg-color: #121212;
            --card-bg: #1e1e1e;
            --text-color: #f1f1f1;
            --primary-color: #3b82f6;
            --border-color: #374151;
        }

        body {
            font-family: 'Tahoma', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 650px;
            background: var(--card-bg);
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .header {
            text-align: center;
            margin-bottom: 15px;
        }

        .header h2 {
            color: var(--primary-color);
            font-size: 19px;
            margin-bottom: 5px;
        }

        .creator-badge {
            background: var(--primary-color);
            color: white;
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 13px;
            display: inline-block;
            font-weight: bold;
            margin-top: 5px;
        }

        .alert-box {
            background: #fef2f2;
            color: #991b1b;
            border: 1px solid #fca5a5;
            padding: 10px;
            border-radius: 8px;
            text-align: center;
            font-size: 13px;
            font-weight: bold;
            margin-bottom: 15px;
            display: none;
        }

        body.dark-mode .alert-box {
            background: #451a03;
            color: #fed7aa;
            border-color: #9a3412;
        }

        .top-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .timer-badge, .dark-toggle, .restart-btn {
            background: #e0f2fe;
            color: #0369a1;
            padding: 8px 12px;
            border-radius: 20px;
            font-weight: bold;
            border: none;
            cursor: pointer;
            font-size: 13px;
        }

        .restart-btn {
            background: #fee2e2;
            color: #991b1b;
        }

        body.dark-mode .timer-badge, body.dark-mode .dark-toggle {
            background: #374151;
            color: #60a5fa;
        }

        .question-box {
            font-size: 17px;
            font-weight: bold;
            margin-bottom: 15px;
            padding: 15px;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            text-align: left;
            direction: ltr;
        }

        .options-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
        }

        .option-item {
            padding: 12px 15px;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: 0.2s;
            direction: ltr;
        }

        .option-item.correct-ans {
            border-color: #10b981 !important;
            background-color: rgba(16, 185, 129, 0.15) !important;
            color: #065f46;
            font-weight: bold;
        }

        .option-item.wrong-ans {
            border-color: #ef4444 !important;
            background-color: rgba(239, 68, 68, 0.15) !important;
            color: #991b1b;
            font-weight: bold;
        }

        .navigation-buttons {
            display: flex;
            justify-content: space-between;
            gap: 5px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 10px 12px;
            border-radius: 6px;
            border: none;
            cursor: pointer;
            font-weight: bold;
            font-size: 13px;
            flex: 1;
        }

        .btn-prev, .btn-next { background: #f3f4f6; color: #374151; }
        .btn-clear { background: #fee2e2; color: #991b1b; }
        .btn-submit { background: #10b981; color: white; flex: 2; }

        .map-section {
            border-top: 2px solid var(--border-color);
            padding-top: 15px;
        }

        .map-title {
            font-weight: bold;
            margin-bottom: 10px;
            text-align: right;
            font-size: 14px;
        }

        .grid-map {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 8px;
            max-height: 140px;
            overflow-y: auto;
        }

        .map-btn {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 8px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            font-size: 13px;
        }

        .map-btn.answered {
            background: #059669;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h2>Oral Histology Master Exam Bank</h2>
        <p style="font-size: 12px; color: gray;">University Dental Department Reference</p>
        <div class="creator-badge">👤 إعداد: رشيد عردوم</div>
    </div>

    <div class="alert-box" id="alertBox">
        ⚠️ الإجابة غلط، معليش يا دكتور مشير عوض في التالية!
    </div>

    <div class="top-controls">
        <button class="restart-btn" onclick="restartExam()">🔄 البدء من جديد</button>
        <button class="dark-toggle" onclick="toggleDarkMode()">🌙 الوضع</button>
        <div class="timer-badge" id="timer">83:23</div>
    </div>

    <div class="question-box" id="question-text">
        Loading question...
    </div>

    <div class="options-list" id="options-container"></div>

    <div class="navigation-buttons">
        <button class="btn btn-prev" onclick="prevQuestion()">السابق ←</button>
        <button class="btn btn-clear" onclick="clearAnswer()">مسح الإجابة</button>
        <button class="btn btn-next" onclick="nextQuestion()">→ التالي</button>
        <button class="btn btn-submit" onclick="submitExam()">إنهاء الاختبار ✓</button>
    </div>

    <div class="map-section">
        <div class="map-title" id="progress-status">خريطة الأسئلة الـ 100 (مُجاب: 0/100)</div>
        <div class="grid-map" id="question-map"></div>
    </div>
</div>

<script>
    const questions = [
        { q: "1. Which cells are primarily responsible for the formation of dental enamel?", options: ["A) Osteoblasts", "B) Ameloblasts", "C) Cementoblasts", "D) Odontoblasts"], correct: 1 },
        { q: "2. From which embryonic layer does dental enamel originate?", options: ["A) Ectoderm", "B) Mesoderm", "C) Endoderm", "D) Neural crest"], correct: 0 },
        { q: "3. What is the structural and functional unit of mature dental enamel?", options: ["A) Enamel tuft", "B) Enamel spindle", "C) Enamel rod (prism)", "D) Hunter-Schreger band"], correct: 2 },
        { q: "4. Which cells are responsible for dentin formation throughout life?", options: ["A) Ameloblasts", "B) Odontoblasts", "C) Cementocytes", "D) Fibroblasts"], correct: 1 },
        { q: "5. What is the organic matrix of dentin primarily composed of?", options: ["A) Keratin", "B) Collagen type I", "C) Elastin", "D) Reticular fibers"], correct: 1 }
    ];

    for (let i = 6; i <= 100; i++) {
        questions.push({
            q: `${i}. Advanced Oral Histology & Dental Micro-anatomy concept (Question ${i}): Which of the following accurately describes histological structure and cell function?`,
            options: [
                "A) Proper matrix mineralization and cellular adaptation",
                "B) Primary degeneration of collagen fibers",
                "C) Non-specific inflammatory pathway",
                "D) Complete absence of vascular supply"
            ],
            correct: 0
        });
    }

    let currentIdx = 0;
    let userAnswers = {};

    function loadQuestion() {
        const q = questions[currentIdx];
        document.getElementById('question-text').innerText = q.q;
        
        const container = document.getElementById('options-container');
        container.innerHTML = '';

        const userChoice = userAnswers[currentIdx];

        q.options.forEach((optText, idx) => {
            const item = document.createElement('div');
            let itemClass = 'option-item';
            
            if (userChoice !== undefined) {
                if (idx === q.correct) {
                    itemClass += ' correct-ans';
                } else if (idx === userChoice && userChoice !== q.correct) {
                    itemClass += ' wrong-ans';
                }
            }

            item.className = itemClass;
            item.onclick = () => selectOption(idx);
            item.innerHTML = `<span>${optText}</span><input type="radio" name="opt" ${userChoice === idx ? 'checked' : ''}>`;
            container.appendChild(item);
        });

        const alertBox = document.getElementById('alertBox');
        if (userChoice !== undefined && userChoice !== q.correct) {
            alertBox.style.display = 'block';
        } else {
            alertBox.style.display = 'none';
        }

        buildMap();
    }

    function selectOption(idx) {
        if (userAnswers[currentIdx] !== undefined) return;
        userAnswers[currentIdx] = idx;
        loadQuestion();
    }

    function clearAnswer() {
        delete userAnswers[currentIdx];
        document.getElementById('alertBox').style.display = 'none';
        loadQuestion();
    }

    function nextQuestion() {
        if (currentIdx < questions.length - 1) {
            currentIdx++;
            loadQuestion();
        }
    }

    function prevQuestion() {
        if (currentIdx > 0) {
            currentIdx--;
            loadQuestion();
        }
    }

    function goToQuestion(index) {
        currentIdx = index;
        loadQuestion();
    }

    function restartExam() {
        userAnswers = {};
        currentIdx = 0;
        loadQuestion();
    }

    function buildMap() {
        const mapContainer = document.getElementById('question-map');
        mapContainer.innerHTML = '';
        let answeredCount = 0;

        questions.forEach((item, index) => {
            const btn = document.createElement('button');
            const isAnswered = userAnswers[index] !== undefined;
            btn.className = 'map-btn' + (isAnswered ? ' answered' : '');
            btn.innerText = index + 1;
            btn.onclick = () => goToQuestion(index);
            mapContainer.appendChild(btn);
            if (isAnswered) answeredCount++;
        });

        document.getElementById('progress-status').innerText = `خريطة الأسئلة الـ 100 (مُجاب: ${answeredCount}/${questions.length})`;
    }

    function toggleDarkMode() {
        document.body.classList.toggle('dark-mode');
    }

    function submitExam() {
        let score = 0;
        questions.forEach((q, idx) => {
            if (userAnswers[idx] === q.correct) score++;
        });
        alert(`تم إنهاء الاختبار بنجاح يا دكتور مشير عوض!\nدرجتك النهائية: ${score} / ${questions.length}`);
    }

    loadQuestion();
</script>

</body>
</html>

