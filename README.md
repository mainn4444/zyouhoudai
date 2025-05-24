<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>自動車駆動方式 確認問題</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.1);
            max-width: 800px;
            width: 100%;
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2rem;
            margin-bottom: 10px;
        }

        .header p {
            opacity: 0.9;
            font-size: 1.1rem;
        }

        .quiz-container {
            padding: 30px;
        }

        .start-screen, .result-screen {
            text-align: center;
        }

        .start-screen h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 1.5rem;
        }

        .start-screen p {
            color: #666;
            margin-bottom: 30px;
            line-height: 1.6;
        }

        .btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            margin: 10px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        .question-screen {
            display: none;
        }

        .progress-bar {
            background: #f0f0f0;
            height: 8px;
            border-radius: 4px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress-fill {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            height: 100%;
            border-radius: 4px;
            transition: width 0.3s ease;
        }

        .question-number {
            color: #666;
            font-size: 0.9rem;
            margin-bottom: 10px;
        }

        .question {
            font-size: 1.3rem;
            color: #333;
            margin-bottom: 25px;
            line-height: 1.5;
        }

        .options {
            display: grid;
            gap: 15px;
            margin-bottom: 30px;
        }

        .option {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 12px;
            padding: 15px 20px;
            cursor: pointer;
            transition: all 0.2s;
            text-align: left;
        }

        .option:hover {
            background: #e3f2fd;
            border-color: #2196f3;
        }

        .option.selected {
            background: #e3f2fd;
            border-color: #2196f3;
            color: #1976d2;
        }

        .option.correct {
            background: #e8f5e8;
            border-color: #4caf50;
            color: #2e7d32;
        }

        .option.incorrect {
            background: #ffebee;
            border-color: #f44336;
            color: #c62828;
        }

        .explanation {
            background: #f0f7ff;
            border-left: 4px solid #2196f3;
            padding: 15px;
            margin: 20px 0;
            border-radius: 0 8px 8px 0;
            display: none;
        }

        .explanation.show {
            display: block;
        }

        .result-screen {
            display: none;
        }

        .score {
            font-size: 3rem;
            color: #4caf50;
            margin: 20px 0;
        }

        .score-message {
            font-size: 1.2rem;
            color: #333;
            margin-bottom: 30px;
        }

        .drive-system-icon {
            font-size: 2rem;
            margin: 0 10px;
        }

        @media (max-width: 600px) {
            .container {
                margin: 10px;
            }
            
            .header h1 {
                font-size: 1.5rem;
            }
            
            .question {
                font-size: 1.1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🚗 自動車駆動方式 確認問題</h1>
            <p>FF・FR・AWD（4WD）・MRの理解度をチェックしよう！</p>
        </div>

        <div class="quiz-container">
            <!-- スタート画面 -->
            <div class="start-screen" id="startScreen">
                <h2>駆動方式マスターへの道</h2>
                <p>
                    <span class="drive-system-icon">🚙</span>FF（前輪駆動）<br>
                    <span class="drive-system-icon">🏎️</span>FR（後輪駆動）<br>
                    <span class="drive-system-icon">🚐</span>AWD/4WD（全輪駆動）<br>
                    <span class="drive-system-icon">🏁</span>MR（ミッドシップ）<br><br>
                    全20問の確認問題に挑戦して、自動車の駆動方式について理解を深めましょう！
                </p>
                <button class="btn" onclick="startQuiz()">クイズを開始する</button>
            </div>

            <!-- 問題画面 -->
            <div class="question-screen" id="questionScreen">
                <div class="progress-bar">
                    <div class="progress-fill" id="progressFill"></div>
                </div>
                <div class="question-number" id="questionNumber"></div>
                <div class="question" id="questionText"></div>
                <div class="options" id="optionsContainer"></div>
                <div class="explanation" id="explanation"></div>
                <button class="btn" id="nextBtn" onclick="nextQuestion()" style="display: none;">次の問題へ</button>
            </div>

            <!-- 結果画面 -->
            <div class="result-screen" id="resultScreen">
                <h2>お疲れ様でした！</h2>
                <div class="score" id="finalScore"></div>
                <div class="score-message" id="scoreMessage"></div>
                <button class="btn" onclick="restartQuiz()">もう一度挑戦する</button>
            </div>
        </div>
    </div>

    <script>
        const questions = [
            {
                question: "FFとは何の略称でしょうか？",
                options: ["Front Engine - Front Wheel Drive", "Four Wheel Drive", "Fast and Furious", "Final Fantasy"],
                correct: 0,
                explanation: "FFは「Front Engine - Front Wheel Drive」の略で、エンジンが前にあり前輪で駆動する方式です。"
            },
            {
                question: "FR駆動の特徴として正しいものはどれでしょうか？",
                options: ["前輪で駆動する", "後輪で駆動する", "全輪で駆動する", "エンジンが後ろにある"],
                correct: 1,
                explanation: "FR（Front Engine - Rear Wheel Drive）は前にエンジンがあり、後輪で駆動する方式です。"
            },
            {
                question: "AWDとは何の略称でしょうか？",
                options: ["Automatic Wheel Drive", "All Wheel Drive", "Advanced Wheel Drive", "Auto Wind Drive"],
                correct: 1,
                explanation: "AWDは「All Wheel Drive」の略で、全ての車輪が駆動する方式です。"
            },
            {
                question: "MR駆動方式の「M」は何を表していますか？",
                options: ["Motor", "Manual", "Mid", "Maximum"],
                correct: 2,
                explanation: "MRの「M」は「Mid」を表し、エンジンが車体の中央付近に配置される「ミッドシップ」を意味します。"
            },
            {
                question: "FF車の利点として正しいものはどれでしょうか？",
                options: ["燃費が悪い", "室内空間が狭い", "製造コストが安い", "雪道に弱い"],
                correct: 2,
                explanation: "FF車はエンジンと駆動系をコンパクトにまとめられるため、製造コストが安く、室内空間も広く取れます。"
            },
            {
                question: "FR車が得意とする場面はどれでしょうか？",
                options: ["燃費重視の運転", "スポーツ走行", "狭い場所での取り回し", "雪道走行"],
                correct: 1,
                explanation: "FR車は前後の重量配分が良く、後輪駆動により旋回性能に優れるため、スポーツ走行に適しています。"
            },
            {
                question: "4WDとAWDの違いについて正しい説明はどれでしょうか？",
                options: ["全く同じもの", "4WDは悪路専用、AWDは常時全輪駆動", "4WDは4輪、AWDは6輪", "4WDは手動、AWDは自動"],
                correct: 1,
                explanation: "一般的に4WDは悪路走行時に切り替える方式、AWDは常時全輪に駆動力を配分する方式を指します。"
            },
            {
                question: "MR車の代表的な特徴はどれでしょうか？",
                options: ["燃費が最も良い", "室内空間が最も広い", "運動性能が高い", "製造コストが最も安い"],
                correct: 2,
                explanation: "MR車はエンジンが車体中央にあるため重心が低く、理想的な重量配分により優れた運動性能を発揮します。"
            },
            {
                question: "雪道でのトラクション（駆動力）が最も期待できる駆動方式はどれでしょうか？",
                options: ["FF", "FR", "AWD", "MR"],
                correct: 2,
                explanation: "AWD（4WD）は全ての車輪で駆動するため、雪道などの滑りやすい路面で最も安定したトラクションを得られます。"
            },
            {
                question: "FF車のエンジン配置で最も一般的なものはどれでしょうか？",
                options: ["エンジンが縦置き", "エンジンが横置き", "エンジンが斜め置き", "エンジンが上下置き"],
                correct: 1,
                explanation: "FF車はコンパクト性を重視するため、エンジンを横置きに配置するのが一般的です。"
            },
            {
                question: "FR車で起こりやすい現象はどれでしょうか？",
                options: ["アンダーステア", "オーバーステア", "ニュートラルステア", "ブレーキング"],
                correct: 1,
                explanation: "FR車は後輪駆動のため、加速時に後輪が滑りやすく、オーバーステア（後輪が外に流れる現象）が起こりやすいです。"
            },
            {
                question: "AWD車の燃費について正しい説明はどれでしょうか？",
                options: ["最も燃費が良い", "FF車より燃費が良い", "FR車より燃費が悪い", "駆動系が複雑で燃費が悪くなりがち"],
                correct: 3,
                explanation: "AWD車は全輪を駆動するため駆動系が複雑で重量も増加し、一般的に燃費は悪くなりがちです。"
            },
            {
                question: "MR車の欠点として最も当てはまるものはどれでしょうか？",
                options: ["燃費が悪い", "乗り心地が悪い", "荷物を積むスペースが少ない", "価格が安すぎる"],
                correct: 2,
                explanation: "MR車はエンジンが中央にあるため、トランクスペースやキャビンスペースが制限され、実用性に劣ります。"
            },
            {
                question: "FF車が得意とする場面はどれでしょうか？",
                options: ["サーキット走行", "登坂路での発進", "燃費重視の街乗り", "ドリフト走行"],
                correct: 2,
                explanation: "FF車は燃費が良く、前輪に駆動力と操舵を集約しているため、日常的な街乗りに最も適しています。"
            },
            {
                question: "FR車のトランクスペースの特徴はどれでしょうか？",
                options: ["非常に狭い", "広くて深い", "全くない", "エンジンと共有している"],
                correct: 1,
                explanation: "FR車はエンジンが前方にあり、リアに駆動系があるものの、比較的広いトランクスペースを確保できます。"
            },
            {
                question: "AWD車が最も活躍する場面はどれでしょうか？",
                options: ["高速道路での燃費走行", "悪路や雪道での走行", "市街地での駐車", "エンジンブレーキの使用"],
                correct: 1,
                explanation: "AWD車は全輪駆動により、雪道、砂地、泥道などの悪路で優れたトラクションを発揮します。"
            },
            {
                question: "MR車でエンジンが配置される位置はどこでしょうか？",
                options: ["フロントアクスルより前", "フロントアクスルとリアアクスルの間", "リアアクスルより後", "車両の上部"],
                correct: 1,
                explanation: "MR（ミッドシップ）車は、エンジンをフロントアクスルとリアアクスルの間に配置します。"
            },
            {
                question: "FF車の重量配分の特徴はどれでしょうか？",
                options: ["前後50:50", "前が重い", "後ろが重い", "左右で異なる"],
                correct: 1,
                explanation: "FF車はエンジン、トランスミッション、ディファレンシャルがすべて前方にあるため、前が重くなります。"
            },
            {
                question: "FR車の代表的な用途はどれでしょうか？",
                options: ["エコカー", "軽自動車", "スポーツカー・高級車", "商用バン"],
                correct: 2,
                explanation: "FR車は運動性能に優れ、高級感のある走りを提供するため、スポーツカーや高級車に多く採用されます。"
            },
            {
                question: "次のうち、実在する駆動方式はどれでしょうか？",
                options: ["RR（リアエンジン・リア駆動）", "RF（リアエンジン・フロント駆動）", "FM（フロントエンジン・ミドル駆動）", "AA（オールエンジン・オール駆動）"],
                correct: 0,
                explanation: "RR（Rear Engine - Rear Wheel Drive）は実在する駆動方式で、ポルシェ911などに採用されています。"
            }
        ];

        let currentQuestion = 0;
        let score = 0;
        let selectedAnswer = null;
        let answered = false;

        function startQuiz() {
            document.getElementById('startScreen').style.display = 'none';
            document.getElementById('questionScreen').style.display = 'block';
            currentQuestion = 0;
            score = 0;
            showQuestion();
        }

        function showQuestion() {
            const question = questions[currentQuestion];
            document.getElementById('questionNumber').textContent = `問題 ${currentQuestion + 1} / ${questions.length}`;
            document.getElementById('questionText').textContent = question.question;
            
            const optionsContainer = document.getElementById('optionsContainer');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.className = 'option';
                optionElement.textContent = option;
                optionElement.onclick = () => selectAnswer(index);
                optionsContainer.appendChild(optionElement);
            });

            document.getElementById('explanation').style.display = 'none';
            document.getElementById('nextBtn').style.display = 'none';
            
            const progress = ((currentQuestion) / questions.length) * 100;
            document.getElementById('progressFill').style.width = progress + '%';
            
            selectedAnswer = null;
            answered = false;
        }

        function selectAnswer(answerIndex) {
            if (answered) return;
            
            selectedAnswer = answerIndex;
            answered = true;
            
            const options = document.querySelectorAll('.option');
            const question = questions[currentQuestion];
            
            options.forEach((option, index) => {
                if (index === question.correct) {
                    option.classList.add('correct');
                } else if (index === selectedAnswer) {
                    option.classList.add('incorrect');
                }
                option.style.pointerEvents = 'none';
            });
            
            if (selectedAnswer === question.correct) {
                score++;
            }
            
            document.getElementById('explanation').textContent = question.explanation;
            document.getElementById('explanation').style.display = 'block';
            document.getElementById('nextBtn').style.display = 'inline-block';
        }

        function nextQuestion() {
            currentQuestion++;
            if (currentQuestion < questions.length) {
                showQuestion();
            } else {
                showResult();
            }
        }

        function showResult() {
            document.getElementById('questionScreen').style.display = 'none';
            document.getElementById('resultScreen').style.display = 'block';
            
            const percentage = Math.round((score / questions.length) * 100);
            document.getElementById('finalScore').textContent = `${score} / ${questions.length} (${percentage}%)`;
            
            let message = '';
            if (percentage >= 90) {
                message = '🏆 素晴らしい！駆動方式マスターです！';
            } else if (percentage >= 80) {
                message = '🎉 とても良くできました！';
            } else if (percentage >= 70) {
                message = '👏 良い成績です！';
            } else if (percentage >= 60) {
                message = '📚 もう少し復習してみましょう';
            } else {
                message = '💪 基本から復習して再挑戦しましょう！';
            }
            
            document.getElementById('scoreMessage').textContent = message;
        }

        function restartQuiz() {
            document.getElementById('resultScreen').style.display = 'none';
            document.getElementById('startScreen').style.display = 'block';
        }
    </script>
</body>
</html># zyouhoudai
