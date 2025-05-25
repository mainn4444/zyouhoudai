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
            background-color: #f8f9fa;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            color: #333;
        }

        .container {
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            max-width: 800px;
            width: 100%;
            overflow: hidden;
            border: 1px solid #e9ecef;
        }

        .header {
            background-color: #333;
            color: white;
            padding: 30px;
            text-align: center;
            border-bottom: 1px solid #dee2e6;
        }

        .header h1 {
            font-size: 1.8rem;
            margin-bottom: 10px;
            font-weight: 600;
        }

        .header p {
            opacity: 0.9;
            font-size: 1rem;
            font-weight: 300;
        }

        .quiz-container {
            padding: 40px;
        }

        .start-screen, .result-screen {
            text-align: center;
        }

        .start-screen h2 {
            color: #333;
            margin-bottom: 25px;
            font-size: 1.4rem;
            font-weight: 500;
        }

        .start-screen p {
            color: #666;
            margin-bottom: 35px;
            line-height: 1.7;
            font-size: 0.95rem;
        }

        .drive-systems {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 15px;
            margin: 25px 0;
            text-align: left;
        }

        .drive-system {
            padding: 15px;
            border: 1px solid #dee2e6;
            border-radius: 4px;
            background-color: #fafafa;
        }

        .drive-system-name {
            font-weight: 600;
            color: #333;
            margin-bottom: 5px;
        }

        .drive-system-desc {
            font-size: 0.85rem;
            color: #666;
        }

        .btn {
            background-color: #333;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 4px;
            font-size: 1rem;
            cursor: pointer;
            transition: background-color 0.2s;
            margin: 10px;
            font-weight: 500;
        }

        .btn:hover {
            background-color: #555;
        }

        .question-screen {
            display: none;
        }

        .progress-bar {
            background: #e9ecef;
            height: 6px;
            border-radius: 3px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress-fill {
            background: #333;
            height: 100%;
            border-radius: 3px;
            transition: width 0.3s ease;
        }

        .question-number {
            color: #666;
            font-size: 0.9rem;
            margin-bottom: 15px;
            font-weight: 500;
        }

        .question {
            font-size: 1.2rem;
            color: #333;
            margin-bottom: 30px;
            line-height: 1.5;
            font-weight: 500;
        }

        .options {
            display: grid;
            gap: 12px;
            margin-bottom: 30px;
        }

        .option {
            background: white;
            border: 2px solid #dee2e6;
            border-radius: 4px;
            padding: 15px 20px;
            cursor: pointer;
            transition: all 0.2s;
            text-align: left;
            font-size: 0.95rem;
        }

        .option:hover {
            background: #f8f9fa;
            border-color: #adb5bd;
        }

        .option.selected {
            background: #f8f9fa;
            border-color: #6c757d;
        }

        .option.correct {
            background: #d4edda;
            border-color: #28a745;
            color: #155724;
        }

        .option.incorrect {
            background: #f8d7da;
            border-color: #dc3545;
            color: #721c24;
        }

        .explanation {
            background: #f8f9fa;
            border-left: 4px solid #6c757d;
            padding: 20px;
            margin: 20px 0;
            border-radius: 0 4px 4px 0;
            display: none;
            font-size: 0.95rem;
            line-height: 1.6;
            color: #495057;
        }

        .explanation.show {
            display: block;
        }

        .result-screen {
            display: none;
        }

        .score {
            font-size: 2.5rem;
            color: #333;
            margin: 20px 0;
            font-weight: 600;
        }

        .score-message {
            font-size: 1.1rem;
            color: #495057;
            margin-bottom: 30px;
            line-height: 1.5;
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

            .quiz-container {
                padding: 30px 20px;
            }

            .drive-systems {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>自動車駆動方式 確認問題</h1>
            <p>FF・FR・AWD（4WD）・MRの理解度をチェックしよう</p>
        </div>

        <div class="quiz-container">
            <!-- スタート画面 -->
            <div class="start-screen" id="startScreen">
                <h2>駆動方式の基礎知識確認</h2>
                <p>
                    自動車の駆動方式について学んだ内容の理解度を確認するクイズです。<br>
                    全16問の問題に挑戦して、知識を定着させましょう。
                </p>
                
                <div class="drive-systems">
                    <div class="drive-system">
                        <div class="drive-system-name">FF (前輪駆動)</div>
                        <div class="drive-system-desc">Front Engine - Front Wheel Drive</div>
                    </div>
                    <div class="drive-system">
                        <div class="drive-system-name">FR (後輪駆動)</div>
                        <div class="drive-system-desc">Front Engine - Rear Wheel Drive</div>
                    </div>
                    <div class="drive-system">
                        <div class="drive-system-name">AWD/4WD (全輪駆動)</div>
                        <div class="drive-system-desc">All Wheel Drive / 4 Wheel Drive</div>
                    </div>
                    <div class="drive-system">
                        <div class="drive-system-name">MR (ミッドシップ)</div>
                        <div class="drive-system-desc">Mid Engine - Rear Wheel Drive</div>
                    </div>
                </div>
                
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
                <h2>お疲れ様でした</h2>
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
                question: "FR車で起こりやすい現象はどれでしょうか？",
                options: ["アンダーステア", "オーバーステア", "ニュートラルステア", "ブレーキング"],
                correct: 1,
                explanation: "FR車は後輪駆動のため、加速時に後輪が滑りやすく、オーバーステア（後輪が外に流れる現象）が起こりやすいです。"
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
                message = '素晴らしい成績です。駆動方式について十分に理解できています。';
            } else if (percentage >= 80) {
                message = 'とても良い成績です。基本的な理解はしっかりとできています。';
            } else if (percentage >= 70) {
                message = '良い成績です。いくつかの重要なポイントを復習してみましょう。';
            } else if (percentage >= 60) {
                message = '基本的な部分は理解できています。もう少し詳しく復習してみましょう。';
            } else {
                message = '基本から復習して、もう一度挑戦してみましょう。';
            }
            
            document.getElementById('scoreMessage').textContent = message;
        }

        function restartQuiz() {
            document.getElementById('resultScreen').style.display = 'none';
            document.getElementById('startScreen').style.display = 'block';
        }
    </script>
</body>
</html>
