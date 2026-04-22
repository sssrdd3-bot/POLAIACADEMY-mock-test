<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Polai Academy - Professional Mock Test</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --primary: #2c3e50;
            --accent: #e67e22;
            --success: #27ae60;
            --danger: #e74c3c;
            --white: #ffffff;
            --bg-color: #f4f7f6;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }

        body { background-color: var(--bg-color); transition: background 0.5s ease; overflow-x: hidden; }

        /* Welcome Screen */
        #start-screen {
            height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; text-align: center;
        }

        .btn-start {
            padding: 15px 40px; font-size: 1.5rem; background: var(--accent); color: white;
            border: none; border-radius: 50px; cursor: pointer; margin-top: 20px; transition: transform 0.3s;
        }

        .btn-start:hover { transform: scale(1.1); }

        /* Main Container */
        #test-container { display: none; height: 100vh; grid-template-columns: 1fr 300px; }

        /* Sidebar - Question Palette */
        .sidebar {
            background: var(--white); border-left: 2px solid #ddd; padding: 20px;
            display: flex; flex-direction: column; box-shadow: -2px 0 10px rgba(0,0,0,0.1);
        }

        .profile-section { text-align: center; margin-bottom: 20px; border-bottom: 1px solid #eee; padding-bottom: 10px; }
        .palette-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-top: 20px; }
        .q-circle {
            width: 40px; height: 40px; border-radius: 50%; border: 1px solid #ccc;
            display: flex; align-items: center; justify-content: center; cursor: pointer; font-weight: bold;
        }
        .q-circle.attempted { background-color: var(--success); color: white; border: none; }
        .q-circle.current { border: 2px solid var(--accent); color: var(--accent); }

        /* Main Area */
        .main-content { padding: 40px; display: flex; flex-direction: column; position: relative; }
        .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 30px; background: white; padding: 15px; border-radius: 10px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); }
        .timer { font-size: 1.2rem; font-weight: bold; color: var(--danger); }

        .question-card { background: white; padding: 40px; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); flex-grow: 1; }
        .question-text { font-size: 1.4rem; margin-bottom: 25px; color: #333; }
        .options-container { display: grid; grid-template-columns: 1fr; gap: 15px; }
        .option {
            padding: 15px 25px; border: 2px solid #eee; border-radius: 10px; cursor: pointer;
            transition: all 0.2s; font-size: 1.1rem; display: flex; align-items: center;
        }
        .option:hover { border-color: var(--accent); background: #fffaf5; }
        .option.selected { background-color: var(--accent); color: white; border-color: var(--accent); }

        .footer-nav { margin-top: 30px; display: flex; justify-content: space-between; }
        .nav-btn { padding: 12px 30px; border-radius: 5px; border: none; cursor: pointer; font-weight: bold; }
        .btn-next { background: var(--primary); color: white; }
        .btn-submit { background: var(--success); color: white; }

        /* Result Screen */
        #result-screen {
            display: none; height: 100vh; background: #f0f2f5; padding: 40px;
            flex-direction: column; align-items: center; overflow-y: auto;
        }
        .score-card { background: white; padding: 40px; border-radius: 20px; box-shadow: 0 15px 35px rgba(0,0,0,0.1); width: 100%; max-width: 600px; text-align: center; }
        .final-score { font-size: 4rem; color: var(--accent); font-weight: 800; }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px; margin: 30px 0; }
        .stat-box { padding: 15px; border-radius: 10px; color: white; }
        .stat-correct { background: var(--success); }
        .stat-wrong { background: var(--danger); }
        .stat-skipped { background: #7f8c8d; }

        .polai-footer { margin-top: 40px; text-align: center; }
        .yt-link { color: #ff0000; font-size: 1.5rem; text-decoration: none; font-weight: bold; }

        /* Background Colors per Question */
        .theme-1 { background-color: #f1f8e9; }
        .theme-2 { background-color: #e3f2fd; }
        .theme-3 { background-color: #fff3e0; }
        .theme-4 { background-color: #f3e5f5; }
    </style>
</head>
<body>

<div id="start-screen">
    <img src="https://img.icons8.com/color/96/000000/test-passed.png" alt="Test Icon">
    <h1 style="font-size: 3.5rem; margin: 20px 0;">Polai Academy Mock Test</h1>
    <div style="background: rgba(255,255,255,0.2); padding: 20px; border-radius: 15px; width: 350px;">
        <p>Total Questions: 13</p>
        <p>Time: 25 Minutes</p>
        <p>Correct: +2 | Wrong: -1</p>
    </div>
    <button class="btn-start" onclick="startTest()">Start Test Now</button>
</div>

<div id="test-container">
    <div class="main-content" id="main-content">
        <div class="header">
            <h2 id="question-number-title">Question 1</h2>
            <div class="timer" id="timer">Time Remaining: 25:00</div>
        </div>

        <div class="question-card">
            <div class="question-text" id="q-text">Loading question...</div>
            <div class="options-container" id="options">
                </div>
        </div>

        <div class="footer-nav">
            <button class="nav-btn btn-next" onclick="prevQuestion()" id="btn-prev">Previous</button>
            <button class="nav-btn btn-next" onclick="nextQuestion()" id="btn-next">Save & Next</button>
            <button class="nav-btn btn-submit" style="display:none;" id="submit-test" onclick="finishTest()">Final Submit</button>
        </div>
    </div>

    <div class="sidebar">
        <div class="profile-section">
            <i class="fas fa-user-circle fa-4x" style="color: #ccc;"></i>
            <h3 style="margin-top:10px;">Polai Academy Student</h3>
        </div>
        <h4>Question Palette</h4>
        <div class="palette-grid" id="palette">
            </div>
        <div style="margin-top: auto; font-size: 0.9rem; color: #666;">
            <p><span style="color:var(--success)">●</span> Answered</p>
            <p><span style="color:#ccc">●</span> Not Visited</p>
        </div>
    </div>
</div>

<div id="result-screen">
    <div class="score-card">
        <h2 style="color: var(--primary);">Mock Test Result</h2>
        <div class="final-score" id="score-val">0</div>
        <p style="font-size: 1.2rem; color: #666;">Total Score</p>
        
        <div class="stats-grid">
            <div class="stat-box stat-correct">
                <h3 id="res-correct">0</h3>
                <p>Correct</p>
            </div>
            <div class="stat-box stat-wrong">
                <h3 id="res-wrong">0</h3>
                <p>Wrong</p>
            </div>
            <div class="stat-box stat-skipped">
                <h3 id="res-skipped">0</h3>
                <p>Skipped</p>
            </div>
        </div>

        <button class="btn-start" style="background: var(--primary);" onclick="location.reload()">Restart Test</button>
        
        <div class="polai-footer">
            <h3>Follow us for more updates:</h3>
            <a href="https://youtube.com/@polaiacademy?si=BfG0TcgHpJOsh34G" target="_blank" class="yt-link">
                <i class="fab fa-youtube"></i> Subscribe Polai Academy
            </a>
        </div>
    </div>
</div>

<script>
    const questions = [
        { q: "Mr. Ram Kumar, the cashier are / on temporary withdrawal / from service.", options: ["On temporary withdrawal", "Mr. Ram Kumar, the cashier are", "From service", "No error"], correct: 1 },
        { q: "Without the greenhouse effect, / Earth temperature would be below freezing.", options: ["Earths temperature would", "Be below freezing", "Without the greenhouse effect", "No error"], correct: 0 },
        { q: "Peoples might confess / just to get out of / the interrogation room.", options: ["No error", "Peoples might confess", "Just to get out of", "The interrogation room"], correct: 1 },
        { q: "Many female mammals, including rabbits and cats ovulate / only when they mate.", options: ["No error", "Many female mammals", "Only when they mate", "Including rabbits and cats, ovulate"], correct: 3 },
        { q: "The baby's faces are expressionless and their pupils aren't visible...", options: ["No error", "The appearance of lifelessness", "Their pupils aren't visible", "The baby's faces are expressionless"], correct: 3 },
        { q: "Balloons filled with helium / travel hundreds or / even thousands of mile.", options: ["Balloons filled with helium", "Travel hundreds or", "No error", "Even thousands of mile"], correct: 3 },
        { q: "And so for the sakes / of his son he remained / absolutely calm.", options: ["Absolutely calm", "No error", "Of his son he remained", "And so for the sakes"], correct: 3 },
        { q: "Workers and companies in all sectors can contribute their skills to meet society new needs.", options: ["Sectors can contribute their skills", "No error", "To meet society new needs", "Workers and companies"], correct: 2 },
        { q: "Whatever a writer puts into a story probably comes with his own life experience.", options: ["With his own life experience", "No error", "Into a story probably comes", "Whatever a writer puts"], correct: 0 },
        { q: "Hundreds of migrants work in private firms in Erode.", options: ["Firms in Erode", "No error", "Hundreds of migrants", "Work in private"], correct: 1 },
        { q: "My friend Jatin usually wears a spectacle.", options: ["My friend Jatin", "Usually wears", "A spectacle", "No error"], correct: 2 },
        { q: "Uttar Pradesh accounts for about 16 per cent / of the country population.", options: ["Of the", "Country population", "Uttar Pradesh", "Accounts for about 16 per cent"], correct: 1 },
        { q: "Telecom is one of the Indias globally recognised success stories.", options: ["Recognised success stories", "Telecom is one of", "No error", "Indias globally"], correct: 3 }
    ];

    let currentIdx = 0;
    let answers = new Array(questions.length).fill(null);
    let timeLeft = 25 * 60; // 25 minutes
    let timerId;

    const bgThemes = ['theme-1', 'theme-2', 'theme-3', 'theme-4'];

    function startTest() {
        document.getElementById('start-screen').style.display = 'none';
        document.getElementById('test-container').style.display = 'grid';
        renderPalette();
        showQuestion();
        startTimer();
    }

    function startTimer() {
        timerId = setInterval(() => {
            timeLeft--;
            let mins = Math.floor(timeLeft / 60);
            let secs = timeLeft % 60;
            document.getElementById('timer').innerText = `Time Remaining: ${mins}:${secs < 10 ? '0' : ''}${secs}`;
            if (timeLeft <= 0) finishTest();
        }, 1000);
    }

    function renderPalette() {
        const palette = document.getElementById('palette');
        palette.innerHTML = '';
        questions.forEach((_, i) => {
            const div = document.createElement('div');
            div.className = `q-circle ${answers[i] !== null ? 'attempted' : ''} ${i === currentIdx ? 'current' : ''}`;
            div.innerText = i + 1;
            div.onclick = () => jumpToQuestion(i);
            palette.appendChild(div);
        });
    }

    function showQuestion() {
        const q = questions[currentIdx];
        document.getElementById('q-text').innerText = q.q;
        document.getElementById('question-number-title').innerText = `Question ${currentIdx + 1}`;
        
        // Change Theme
        document.body.className = bgThemes[currentIdx % bgThemes.length];

        const optContainer = document.getElementById('options');
        optContainer.innerHTML = '';
        q.options.forEach((opt, i) => {
            const div = document.createElement('div');
            div.className = `option ${answers[currentIdx] === i ? 'selected' : ''}`;
            div.innerText = `${String.fromCharCode(65 + i)}. ${opt}`;
            div.onclick = () => selectOption(i);
            optContainer.appendChild(div);
        });

        document.getElementById('btn-prev').disabled = currentIdx === 0;
        if (currentIdx === questions.length - 1) {
            document.getElementById('btn-next').style.display = 'none';
            document.getElementById('submit-test').style.display = 'block';
        } else {
            document.getElementById('btn-next').style.display = 'block';
            document.getElementById('submit-test').style.display = 'none';
        }
        renderPalette();
    }

    function selectOption(idx) {
        answers[currentIdx] = idx;
        showQuestion();
    }

    function nextQuestion() {
        if (currentIdx < questions.length - 1) {
            currentIdx++;
            showQuestion();
        }
    }

    function prevQuestion() {
        if (currentIdx > 0) {
            currentIdx--;
            showQuestion();
        }
    }

    function jumpToQuestion(i) {
        currentIdx = i;
        showQuestion();
    }

    function finishTest() {
        clearInterval(timerId);
        document.getElementById('test-container').style.display = 'none';
        document.getElementById('result-screen').style.display = 'flex';

        let correct = 0;
        let wrong = 0;
        let skipped = 0;

        answers.forEach((ans, i) => {
            if (ans === null) skipped++;
            else if (ans === questions[i].correct) correct++;
            else wrong++;
        });

        let totalScore = (correct * 2) - (wrong * 1);
        document.getElementById('score-val').innerText = totalScore;
        document.getElementById('res-correct').innerText = correct;
        document.getElementById('res-wrong').innerText = wrong;
        document.getElementById('res-skipped').innerText = skipped;
    }
</script>

</body>
</html>
