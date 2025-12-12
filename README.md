# quiz
HTML quiuz
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>前端知識對對碰 - 期末測驗（修正版）</title>
    <style>
/* 主題與背景 */
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f0f4f8;
    color: #333;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
}

#quiz-app {
    background-color: white;
    padding: 40px;
    border-radius: 12px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
    width: 90%;
    max-width: 600px;
}

/* 標題 */
h1 {
    color: #1e90ff; /* 冷靜藍 */
    text-align: center;
    border-bottom: 2px solid #1e90ff;
    padding-bottom: 10px;
    margin-bottom: 30px;
}

/* 狀態欄 */
#status-bar {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 1.1em;
    padding: 10px;
    background-color: #e6f0ff;
    border-radius: 8px;
}

/* 測驗卡片 */
#quiz-card {
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
}

#question-text {
    min-height: 80px; /* 確保問題區域高度一致 */
    color: #333;
}

/* 選項按鈕美編 */
.option-btn {
    display: block;
    width: 100%;
    padding: 12px 20px;
    margin: 10px 0;
    border: 2px solid #1e90ff;
    border-radius: 6px;
    background-color: transparent;
    color: #1e90ff;
    font-size: 1em;
    text-align: left;
    cursor: pointer;
    transition: all 0.2s ease-in-out;
}

.option-btn:hover:not(:disabled) {
    background-color: #1e90ff;
    color: white;
    transform: translateY(-2px);
    box-shadow: 0 4px 6px rgba(30, 144, 255, 0.3);
}

/* 答題回饋顏色 */
.correct {
    background-color: #4CAF50 !important; /* 綠色 */
    color: white !important;
    border-color: #4CAF50 !important;
}

.incorrect {
    background-color: #f44336 !important; /* 紅色 */
    color: white !important;
    border-color: #f44336 !important;
}

/* 訊息與下一題按鈕 */
#feedback {
    margin-top: 15px;
    font-size: 1.2em;
    font-weight: bold;
}

.hidden {
    display: none;
}

#next-btn {
    float: right;
    padding: 10px 25px;
    background-color: #ff6347; /* 暖紅色 */
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-size: 1em;
    transition: background-color 0.2s;
}

#next-btn:hover {
    background-color: #e55333;
}

/* 結束畫面 */
#result-screen {
    text-align: center;
    padding: 30px;
}

#result-screen h2 {
    color: #ff6347;
}
#final-score {
    font-size: 2em;
    color: #1e90ff;
}
#rating {
    font-weight: bold;
    font-size: 1.5em;
    color: #4CAF50;
}

#result-screen button {
    /* 主色調：使用強調色 (與您 Next Button 相似的暖紅) */
    background-color: #ff6347; 
    color: white;
    padding: 12px 30px; /* 加大按鈕尺寸 */
    border: none;
    border-radius: 8px; /* 圓角 */
    font-size: 1.1em;
    font-weight: bold;
    cursor: pointer;
    margin-top: 25px; /* 與上方文字拉開距離 */
    transition: all 0.3s ease; /* 啟用平滑過渡動畫 */
    box-shadow: 0 4px 10px rgba(255, 99, 71, 0.4); /* 增加陰影立體感 */
}

#result-screen button:hover {
    background-color: #e55333; /* 懸停時顏色變深 */
    transform: translateY(-2px); /* 輕微上浮動畫 */
    box-shadow: 0 8px 15px rgba(255, 99, 71, 0.6); /* 陰影更明顯 */
}

#result-actions {
    display: flex; /* 使用 Flexbox 排列 */
    justify-content: center; /* 水平居中 */
    gap: 20px; /* 按鈕間距 */
}

/* 顯示解析按鈕 (次要按鈕，使用品牌藍色) */
#analysis-btn {
    background-color: #1e90ff !important; 
    box-shadow: 0 4px 10px rgba(30, 144, 255, 0.4) !important;
}

#analysis-btn:hover {
    background-color: #1a7ae2 !important;
    transform: translateY(-2px);
    box-shadow: 0 8px 15px rgba(30, 144, 255, 0.6) !important;
}

/* 解析畫面樣式 */
#analysis-screen h2 {
    color: #1e90ff;
    text-align: center;
}
.analysis-item {
    margin-bottom: 25px;
    padding: 15px;
    border-left: 5px solid #1e90ff;
    background-color: #f8f8f8;
    border-radius: 4px;
}
.analysis-item strong {
    color: #4CAF50; /* 答案顏色 */
    font-weight: bold;
}
.analysis-item p {
    margin-top: 5px;
    font-size: 0.95em;
    line-height: 1.5;
}
    </style>
</head>
<body>
    <div id="quiz-app">
        <h1>《HTML期末考複習》</h1>
        
        <div id="status-bar">
            <span id="score">分數: 0</span>
            <span id="timer-display">時間: 15s</span>
            <span id="q-counter">題號: 1 / 10</span>
        </div>
        
        <div id="quiz-card">
            <h2 id="question-text">載入中...</h2>
            <div id="options-container"></div>
            <p id="feedback" class="hidden"></p>
            <button id="next-btn" class="hidden">下一題 &gt;</button>
        </div>

        <div id="result-screen" class="hidden">
            <h2>測驗結果</h2>
            <p>您的最終得分是: <span id="final-score">0</span>分</p>
            <p>恭喜您獲得: <span id="rating"></span></p>
            <div id="result-actions">
                <button onclick="window.location.reload()">重新挑戰</button>
                <button id="analysis-btn">顯示解析</button>
            </div>
        </div>

        <div id="analysis-screen" class="hidden">
            <h2>測驗解析</h2>
             <div id="analysis-content"></div>
            <button onclick="window.location.reload()">返回首頁/重新挑戰</button>
        </div>
    </div>

    <script>
        const questions = [
    {
        q: "下列何者可以用來定義網頁的內容？",
        options: ["C++", "Javascript", "HTML", "CSS"],
        answer: "HTML",
        topic: "HTML 基礎"
    },
    {
        q: "下列何者不屬於行內層級 (inline level) 的 HTML 元素？",
        options: ["<span>", "<em>", "<strong>", "<div>"],
        answer: "<div>",
        topic: "HTML 元素"
    },
    {
        q: "下列哪個元素可以放在 <head> 裡面？",
        options: ["<title>", "<html>", "<p>", "<h1>"],
        answer: "<title>",
        topic: "HTML 結構"
    },
    {
        q: "為避免圖片無法顯示，`<img>` 元素裡用於指定替代顯示文字的屬性為何？",
        options: ["note", "pic", "alt", "illustrate"],
        answer: "alt",
        topic: "HTML 屬性"
    },
    {
        q: "下列關於 HTML 與 CSS 的敘述何者錯誤？",
        options: ["HTML 定義內容", "CSS 定義外觀", "HTML 不區分大小寫", "CSS 不會區分大小寫"],
        answer: "CSS 不會區分大小寫",
        topic: "HTML/CSS 區別"
    },
    {
        q: "若要將文字全部轉換成大寫，常用的 CSS 類別或屬性值為何？",
        options: [".text-nowrpe", ".text-uppercase", ".text-capitalize", ".text-lowercase"],
        answer: ".text-uppercase",
        topic: "CSS 樣式"
    },
    {
        q: "若要設定文字水平置中，可以使用下列哪個類別？",
        options: ["text-center", "text-nowrap", "text-justify", "text-left"],
        answer: "text-center",
        topic: "CSS 排版"
    },
    {
        q: "CSS `font-family` 屬性中，多個字型名稱之間應使用何種符號隔開？",
        options: ["分號 (;)", "逗號 (,)", "冒號 (:)", "空格 ( )"],
        answer: "逗號 (,)",
        topic: "CSS 屬性"
    },
    {
        q: "下列哪個屬性可以用來進行位移、縮放、旋轉等變形處理？",
        options: ["transform-origin", "transform", "perspective", "transform-style"],
        answer: "transform",
        topic: "CSS Transform"
    },
    {
        q: "JavaScript 運算式 `123 === \"123\"` 的結果為何？",
        options: ["true", "false", "undefined", "123"],
        answer: "false",
        topic: "JavaScript 邏輯"
    }
];

// 詳細解析陣列
const detailedAnalysis = [
    "HTML 負責定義結構，CSS 定義外觀，JavaScript 定義行為。",
    "<div> 是區塊元素 (Block-level)，預設會獨佔一行並換行；而選項中的 <span>, <em>, <strong> 都是行內元素 (Inline-level)。",
    "選項中，<title> 是放置在 <head> 區塊的元數據元素，<head> 不可包含 <body> 內容。其他如 <html> 是根元素， <p> 和 <h1> 則屬於可視的 <body> 內容。",
    "圖片的 alt (Alternative Text) 屬性提供替代文字，對無障礙使用非常重要。",
    "CSS 的選擇器（特別是 Class 或 ID）區分大小寫，因此「CSS 不會區分大小寫」為錯誤敘述。",
    "`.text-uppercase` 對應 CSS `text-transform: uppercase`，能讓文字全大寫。",
    "`text-center` 對應 `text-align: center`，讓文字水平置中。",
    "`font-family` 指定多個字型時，必須使用逗號 `,` 分隔。",
    "`transform` 可進行旋轉、縮放、位移等 2D/3D 變形。",
    "`===` 嚴格相等會比較值與型別，因此 123 與 \"123\" 不相等。"
];


let currentQIndex = 0;
let score = 0;
let timer = 15;
let countdown;
const totalQuestions = questions.length;
const questionTime = 15; // 每題 15 秒

// DOM 元素快取
const questionText = document.getElementById('question-text');
const optionsContainer = document.getElementById('options-container');
const feedback = document.getElementById('feedback');
const nextBtn = document.getElementById('next-btn');
const scoreDisplay = document.getElementById('score');
const qCounter = document.getElementById('q-counter');
const timerDisplay = document.getElementById('timer-display');
const quizCard = document.getElementById('quiz-card');
const resultScreen = document.getElementById('result-screen');
const analysisBtn = document.getElementById('analysis-btn');
const analysisScreen = document.getElementById('analysis-screen');
const analysisContent = document.getElementById('analysis-content');

// 安全的 HTML escape 函式（用於顯示含有 < > 的字串）
function escapeHTML(str) {
    if (str === null || str === undefined) return '';
    return String(str)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#39;');
}

// 啟動倒數計時器
function startTimer() {
    timer = questionTime;
    timerDisplay.textContent = `時間: ${timer}s`;
    
    clearInterval(countdown);
    countdown = setInterval(() => {
        timer--;
        timerDisplay.textContent = `時間: ${timer}s`;
        
        // 倒數結束
        if (timer <= 0) {
            clearInterval(countdown);
            handleAnswer(null); // 視為答錯（時間到）
        }
    }, 1000);
}

// 顯示當前題目（使用 textContent 避免被解析）
function displayQuestion() {
    if (currentQIndex >= totalQuestions) {
        showResults();
        return;
    }

    const currentQ = questions[currentQIndex];
    
    // 重設 UI
    feedback.classList.add('hidden');
    nextBtn.classList.add('hidden');
    optionsContainer.innerHTML = '';
    quizCard.style.pointerEvents = 'auto'; // 啟用點擊

    optionsContainer.style.pointerEvents = 'auto'; //
    // 更新計數器
    qCounter.textContent = `題號: ${currentQIndex + 1} / ${totalQuestions}`;

    // 使用 textContent 而非 innerHTML（避免把字串當成標記解析）
    questionText.textContent = currentQ.q;

    // 生成選項按鈕
    currentQ.options.forEach(option => {
        const button = document.createElement('button');
        button.textContent = option;
        button.classList.add('option-btn');
        button.onclick = () => handleAnswer(option, button);
        optionsContainer.appendChild(button);
    });

    startTimer();
}

// 處理答案
function handleAnswer(selectedOption, clickedButton = null) {
    clearInterval(countdown); // 停止計時
    optionsContainer.style.pointerEvents = 'none'; // 禁用所有選項點擊

    const currentQ = questions[currentQIndex];
    const isCorrect = (selectedOption === currentQ.answer);

    // 標記所有按鈕 (確保所有按鈕都被檢查，即使是時間到)
    optionsContainer.querySelectorAll('.option-btn').forEach(btn => {
        btn.disabled = true; // 禁用所有按鈕
        if (btn.textContent === currentQ.answer) {
            btn.classList.add('correct'); // 正確答案顯示綠色
        } else if (btn === clickedButton) {
            btn.classList.add('incorrect'); // 錯誤選擇顯示紅色
        }
    });

    if (isCorrect) {
        score++;
        feedback.textContent = "✔ 正確！";
        feedback.style.color = '#4CAF50';
    } else if (selectedOption === null) {
        // 時間到
        feedback.textContent = `❌ 時間到！正確答案是: ${currentQ.answer}`;
        feedback.style.color = '#ff6347';
    } else {
        feedback.textContent = `❌ 答錯了！正確答案是: ${currentQ.answer}`;
        feedback.style.color = '#ff6347';
    }

    // 更新分數顯示
    scoreDisplay.textContent = `分數: ${score}`;
    
    // 顯示回饋與下一題按鈕
    feedback.classList.remove('hidden');
    nextBtn.classList.remove('hidden');
}

// 進入下一題或顯示結果
nextBtn.onclick = () => {
    currentQIndex++;
    displayQuestion();
};

// 顯示結果畫面
function showResults() {
    quizCard.classList.add('hidden');
    document.getElementById('status-bar').classList.add('hidden');
    resultScreen.classList.remove('hidden');

    document.getElementById('final-score').textContent = score;

    let ratingText = "";
    if (score === totalQuestions) {
        ratingText = "全對超強！";
        document.getElementById('rating').style.color = 'gold';
    } else if (score >= totalQuestions * 0.8) {
        ratingText = "讚讚！";
        document.getElementById('rating').style.color = '#4CAF50';
    } else if (score >= totalQuestions * 0.6) {
        ratingText = "繼續加油！";
        document.getElementById('rating').style.color = '#1e90ff';
    } else {
        ratingText = "哈哈你好爛！";
        document.getElementById('rating').style.color = '#f44336';
    }
    document.getElementById('rating').textContent = ratingText;
    analysisBtn.onclick = showAnalysis;
}

function showAnalysis() {
    resultScreen.classList.add('hidden');
    analysisScreen.classList.remove('hidden');

    analysisContent.innerHTML = ''; 

    questions.forEach((q, index) => {
        const wrapper = document.createElement('div');
        wrapper.className = 'analysis-item';

        // 題目標題
        const title = document.createElement('h4');
        title.textContent = `Q${index + 1}: ${q.q}`;
        wrapper.appendChild(title);

        // 🌟 新增：選項列表
        const optionList = document.createElement('ul');
        optionList.style.marginLeft = "20px";

       q.options.forEach((opt, i) => {
    const optDiv = document.createElement('div');
    // 用字母 A-D 標示
    const letter = String.fromCharCode(65 + i); // 65 = 'A'
    optDiv.textContent = `${letter}. ${opt}`;
    optDiv.style.marginBottom = "6px";
    wrapper.appendChild(optDiv);
});



        wrapper.appendChild(optionList);

        // 正確答案
        const answerP = document.createElement('p');
        const strongAns = document.createElement('strong');
        strongAns.textContent = '正確答案： ';
        answerP.appendChild(strongAns);
        answerP.appendChild(document.createTextNode(q.answer));
        wrapper.appendChild(answerP);

        // 解析
        const explP = document.createElement('p');
        const strongExpl = document.createElement('strong');
        strongExpl.textContent = '解析： ';
        explP.appendChild(strongExpl);
        explP.appendChild(document.createTextNode(detailedAnalysis[index]));
        wrapper.appendChild(explP);

        analysisContent.appendChild(wrapper);
    });
}





// 程式啟動
document.addEventListener('DOMContentLoaded', displayQuestion);
    </script>
</body>
</html>
