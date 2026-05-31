<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>宇宙讯号</title>
    <style>
        :root {
            --bg-dark: #0a0306;
            --bg-mid: #17050d;
            --bg-light: #290a18;
            --accent-pink: #ff7eb3;
            --accent-gold: #d4af37;
            --text-light: #fce8f0;
            --text-muted: #b58399;
            --card-width: 150px;
            --card-height: 260px;
        }

        body {
            background: radial-gradient(circle at center, var(--bg-light) 0%, var(--bg-mid) 40%, var(--bg-dark) 100%);
            color: var(--text-light);
            font-family: 'Georgia', 'Times New Roman', serif;
            min-height: 100vh;
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 30px 20px;
            box-sizing: border-box;
            overflow-x: hidden;
        }

        /* ====== 通用动画 ====== */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        h1 {
            color: var(--accent-pink);
            text-shadow: 0 0 20px rgba(255, 126, 179, 0.4);
            margin-bottom: 5px;
            font-size: 2.2rem;
            letter-spacing: 3px;
            font-weight: normal;
            text-align: center;
        }

        .date-indicator {
            font-size: 0.9rem;
            color: var(--text-muted);
            margin-bottom: 40px;
            font-family: 'Segoe UI', Tahoma, sans-serif;
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        /* ====== 名字输入欢迎页 ====== */
        #intro-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 70vh;
            width: 100%;
            animation: fadeIn 1s ease forwards;
        }

        .intro-desc {
            color: var(--text-muted);
            font-size: 1.1rem;
            margin-bottom: 40px;
            letter-spacing: 2px;
            text-align: center;
        }

        .name-input-wrapper {
            position: relative;
            margin-bottom: 40px;
        }

        #user-name-input {
            background: rgba(20, 5, 13, 0.6);
            border: 1px solid rgba(255, 126, 179, 0.4);
            border-radius: 30px;
            padding: 15px 30px;
            color: var(--accent-pink);
            font-size: 1.2rem;
            text-align: center;
            width: 280px;
            outline: none;
            box-shadow: inset 0 0 15px rgba(255, 126, 179, 0.05);
            transition: all 0.4s ease;
            font-family: 'Georgia', serif;
            letter-spacing: 1px;
        }

        #user-name-input::placeholder {
            color: rgba(181, 131, 153, 0.5);
        }

        #user-name-input:focus {
            border-color: var(--accent-pink);
            box-shadow: 0 0 20px rgba(255, 126, 179, 0.3), inset 0 0 15px rgba(255, 126, 179, 0.1);
            width: 300px;
        }

        .enter-btn {
            padding: 12px 40px;
            background: linear-gradient(135deg, rgba(255, 126, 179, 0.2), transparent);
            border: 1px solid var(--accent-pink);
            color: var(--accent-pink);
            font-size: 1.1rem;
            border-radius: 30px;
            cursor: pointer;
            transition: all 0.4s ease;
            font-family: 'Segoe UI', Tahoma, sans-serif;
            letter-spacing: 3px;
            text-transform: uppercase;
        }

        .enter-btn:hover {
            background: var(--accent-pink);
            color: var(--bg-dark);
            box-shadow: 0 0 25px rgba(255, 126, 179, 0.6);
            transform: translateY(-2px);
        }

        /* ====== 主塔罗界面 ====== */
        #main-content {
            display: none;
            flex-direction: column;
            align-items: center;
            width: 100%;
            animation: fadeIn 1s ease forwards;
        }

        .card-container {
            display: flex;
            gap: 25px;
            justify-content: center;
            perspective: 1200px;
            margin-bottom: 50px;
            width: 100%;
            max-width: 700px;
        }

        .tarot-card {
            width: var(--card-width);
            height: var(--card-height);
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
            cursor: pointer;
        }

        .tarot-card:hover:not(.disabled) {
            transform: translateY(-20px) scale(1.05);
            box-shadow: 0 25px 50px rgba(255, 126, 179, 0.15);
        }

        .tarot-card.disabled {
            cursor: not-allowed;
            opacity: 0.5;
        }

        .card-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            border-radius: 8px;
            box-sizing: border-box;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.9);
            overflow: hidden;
        }

        .card-back {
            background-color: var(--bg-dark);
            background-image: 
                linear-gradient(135deg, rgba(26, 6, 15, 0.85) 0%, rgba(10, 3, 6, 0.92) 100%),
                url('http://googleusercontent.com/image_generation_content/170');
            background-size: cover;
            background-position: center;
            border: 2px solid #5a263c;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .card-back::before {
            content: '';
            position: absolute;
            top: 6px; left: 6px; right: 6px; bottom: 6px;
            border: 1px solid rgba(255, 126, 179, 0.25);
            border-radius: 4px;
        }
        
        .card-back::after {
            content: '✧';
            font-size: 2.5rem;
            color: rgba(255, 126, 179, 0.35);
            filter: drop-shadow(0 0 5px rgba(255, 126, 179, 0.5));
        }

        .card-front {
            background: #1f0813;
            border: 2px solid var(--accent-pink);
            transform: rotateY(180deg);
            padding: 8px;
            display: flex;
            flex-direction: column;
        }

        .card-inner-border {
            border: 1px solid rgba(255, 126, 179, 0.6);
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
            background: linear-gradient(to bottom, #2d0e1d 0%, #17050d 100%);
            border-radius: 4px;
        }

        .card-number {
            font-size: 1.1rem;
            color: var(--accent-gold);
            margin: 8px 0;
            font-family: 'Times New Roman', serif;
            letter-spacing: 2px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.8);
        }

        .card-illustration {
            width: 85%;
            flex: 1;
            margin-bottom: 8px;
            background: radial-gradient(circle at center, #240b17 0%, #050103 100%);
            border: 1px solid rgba(255, 126, 179, 0.3);
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            box-shadow: inset 0 0 20px rgba(255, 126, 179, 0.1);
        }

        .halo {
            position: absolute;
            width: 150%;
            height: 150%;
            background: conic-gradient(from 0deg, transparent 0deg, rgba(255, 126, 179, 0.12) 60deg, transparent 120deg, rgba(212, 175, 55, 0.08) 180deg, transparent 240deg, rgba(255, 126, 179, 0.12) 300deg, transparent 360deg);
            animation: rotateHalo 20s linear infinite;
        }

        .card-seal {
            position: relative;
            width: 80px;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2;
        }

        .seal-bg {
            position: absolute;
            width: 100%;
            height: 100%;
            border: 1px solid rgba(212, 175, 55, 0.4);
            border-radius: 50%;
            box-sizing: border-box;
        }

        .seal-bg::before {
            content: '';
            position: absolute;
            top: 5px; left: 5px; right: 5px; bottom: 5px;
            border: 1px dashed rgba(255, 126, 179, 0.6);
            border-radius: 50%;
            animation: spinLeft 15s linear infinite;
        }

        .seal-bg::after {
            content: '';
            position: absolute;
            top: 15px; left: 15px; right: 15px; bottom: 15px;
            border: 1px solid rgba(212, 175, 55, 0.25);
            box-sizing: border-box;
            animation: spinRight 25s linear infinite;
        }

        .seal-star {
            position: absolute;
            top: 15px; left: 15px; right: 15px; bottom: 15px;
            border: 1px solid rgba(255, 126, 179, 0.25);
            transform: rotate(45deg);
            box-sizing: border-box;
            animation: spinRight 25s linear infinite;
        }

        .seal-symbol {
            font-size: 2.2rem;
            color: var(--accent-gold);
            text-shadow: 0 0 10px rgba(255, 126, 179, 0.6), 0 0 20px rgba(212, 175, 55, 0.3);
            z-index: 3;
            font-family: 'Georgia', serif;
        }

        @keyframes rotateHalo { 100% { transform: rotate(360deg); } }
        @keyframes spinRight { 100% { transform: rotate(360deg); } }
        @keyframes spinLeft { 100% { transform: rotate(-360deg); } }

        .card-name-banner {
            width: 90%;
            background: rgba(10, 3, 6, 0.85);
            border: 1px solid var(--accent-pink);
            padding: 6px 0;
            margin-bottom: 10px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.5);
        }

        .card-front .name {
            font-size: 0.95rem;
            color: var(--text-light);
            letter-spacing: 1px;
            margin-bottom: 2px;
        }

        .card-front .en-name {
            font-size: 0.55rem;
            color: var(--accent-gold);
            text-transform: uppercase;
            letter-spacing: 1px;
            font-family: 'Segoe UI', Tahoma, sans-serif;
        }

        .tarot-card.flipped {
            transform: rotateY(180deg) scale(1.1);
            box-shadow: 0 0 40px rgba(255, 126, 179, 0.3);
            z-index: 10;
        }

        /* ====== 结果面板 ====== */
        .result-panel {
            max-width: 700px;
            width: 100%;
            background: rgba(20, 5, 13, 0.7);
            border: 1px solid rgba(255, 126, 179, 0.2);
            border-radius: 12px;
            padding: 35px;
            display: none;
            box-shadow: 0 20px 50px rgba(0,0,0,0.8);
            animation: slideUpFade 0.8s ease forwards;
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            position: relative;
        }

        .result-header {
            text-align: center;
            border-bottom: 1px solid rgba(255, 126, 179, 0.15);
            padding-bottom: 20px;
            margin-bottom: 30px;
        }

        .fortune-title {
            font-size: 1.5rem;
            color: var(--accent-pink);
            margin: 15px 0 5px;
            line-height: 1.6;
            font-style: italic;
        }

        .grid-info {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
            margin-bottom: 35px;
        }

        .info-item {
            background: linear-gradient(90deg, rgba(255, 126, 179, 0.08) 0%, transparent 100%);
            padding: 15px 20px;
            border-radius: 8px;
            border-left: 2px solid var(--accent-pink);
        }

        .info-label {
            font-size: 0.8rem;
            color: var(--text-muted);
            margin-bottom: 8px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .info-value {
            font-size: 1.1rem;
            color: var(--text-light);
        }

        .dos-and-donts {
            display: flex;
            gap: 25px;
            margin-bottom: 25px;
        }

        .action-box {
            flex: 1;
            padding: 25px;
            border-radius: 8px;
            background: rgba(0,0,0,0.4);
            border: 1px solid rgba(255, 126, 179, 0.1);
        }

        .action-title {
            font-weight: normal;
            font-size: 1.2rem;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 8px;
            letter-spacing: 1px;
        }

        .action-box.do .action-title { color: var(--accent-pink); }
        .action-box.dont .action-title { color: #8a6a78; }

        .action-list {
            line-height: 1.8;
            color: #e0c8d3;
            font-size: 0.95rem;
        }

        /* ====== 新增：日常小叮嘱区域 ====== */
        .daily-reminders {
            background: rgba(255, 126, 179, 0.04);
            border: 1px dashed rgba(255, 126, 179, 0.25);
            border-radius: 8px;
            padding: 18px 25px;
            margin-bottom: 35px;
            position: relative;
        }

        .reminders-title {
            color: var(--accent-gold);
            font-size: 1rem;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
            font-family: 'Segoe UI', Tahoma, sans-serif;
            letter-spacing: 1px;
        }

        .reminders-list {
            color: var(--text-light);
            font-size: 0.95rem;
            line-height: 1.8;
            letter-spacing: 1px;
            margin: 0;
            padding-left: 20px;
        }

        .reminders-list li {
            margin-bottom: 5px;
        }
        .reminders-list li::marker {
            color: var(--accent-pink);
        }

        /* ====== 恋人秘语区域 ====== */
        .lover-whisper {
            border-top: 1px solid rgba(255, 126, 179, 0.15);
            padding-top: 35px;
            margin-top: 20px;
            text-align: center;
            position: relative;
        }

        .lover-whisper::before {
            content: '💬';
            position: absolute;
            top: -15px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--bg-mid);
            padding: 0 15px;
            color: var(--accent-pink);
            font-size: 1.2rem;
        }

        .whisper-title {
            font-size: 0.85rem;
            color: var(--accent-gold);
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 25px;
            position: relative;
            display: inline-block;
            padding-bottom: 8px;
            border-bottom: 1px dashed rgba(212, 175, 55, 0.4);
            font-family: 'Segoe UI', Tahoma, sans-serif;
            opacity: 0.9;
        }

        .whisper-text {
            font-size: 1.05rem;
            color: var(--text-light);
            line-height: 1.9;
            letter-spacing: 1px;
            max-width: 95%;
            margin: 0 auto;
            text-align: left;
            text-indent: 2em; 
        }

        .restart-btn {
            display: block;
            margin: 40px auto 0;
            padding: 12px 35px;
            background: transparent;
            border: 1px solid var(--accent-pink);
            color: var(--accent-pink);
            font-size: 1rem;
            border-radius: 30px;
            cursor: pointer;
            transition: all 0.4s ease;
            font-family: 'Segoe UI', Tahoma, sans-serif;
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        .restart-btn:hover {
            background: var(--accent-pink);
            color: var(--bg-dark);
            box-shadow: 0 0 20px rgba(255, 126, 179, 0.6);
        }

        @media (max-width: 600px) {
            :root {
                --card-width: 105px;
                --card-height: 185px;
            }
            .card-container { gap: 12px; }
            .card-seal { width: 55px; height: 55px; }
            .seal-symbol { font-size: 1.5rem; }
            .card-number { font-size: 0.8rem; margin: 4px 0;}
            .card-front .name { font-size: 0.75rem; }
            .card-front .en-name { font-size: 0.45rem; }
            .card-name-banner { padding: 4px 0; margin-bottom: 6px;}
            .grid-info { grid-template-columns: 1fr; }
            .dos-and-donts { flex-direction: column; }
            .daily-reminders { padding: 15px; }
            .reminders-list { font-size: 0.9rem; }
            .whisper-title { font-size: 0.75rem; letter-spacing: 1px; }
            .whisper-text { font-size: 1rem; text-indent: 1.5em; }
        }
    </style>
</head>
<body>

    <!-- 修改为新的标题 -->
    <h1>今天你会收到从哪里来的波段？</h1>
    <div class="date-indicator" id="current-date"></div>

    <!-- 1. 名字输入欢迎页 -->
    <div id="intro-screen">
        <div class="intro-desc">在命运的星盘转动前<br>请告诉我，我该呼唤谁的名字？</div>
        <div class="name-input-wrapper">
            <input type="text" id="user-name-input" placeholder="输入你的名字..." autocomplete="off">
        </div>
        <button class="enter-btn" onclick="startJourney()">缔结契约</button>
    </div>

    <!-- 2. 主塔罗界面 -->
    <div id="main-content">
        <div class="card-container" id="card-stage">
            <div class="tarot-card" onclick="drawCard(0)">
                <div class="card-face card-back"></div>
                <div class="card-face card-front" id="front-0"></div>
            </div>
            <div class="tarot-card" onclick="drawCard(1)">
                <div class="card-face card-back"></div>
                <div class="card-face card-front" id="front-1"></div>
            </div>
            <div class="tarot-card" onclick="drawCard(2)">
                <div class="card-face card-back"></div>
                <div class="card-face card-front" id="front-2"></div>
            </div>
        </div>

        <div class="result-panel" id="result-panel">
            <div class="result-header">
                <div id="result-card-name" style="color: var(--text-muted); font-size: 1.1rem; letter-spacing: 3px; text-transform: uppercase;"></div>
                <div class="fortune-title" id="fortune-summary"></div>
            </div>

            <div class="grid-info">
                <div class="info-item">
                    <div class="info-label">🎨 幸运灵色</div>
                    <div class="info-value" id="luck-color"></div>
                </div>
                <div class="info-item">
                    <div class="info-label">🧸 幸运契物</div>
                    <div class="info-value" id="luck-item"></div>
                </div>
                <div class="info-item">
                    <div class="info-label">🧭 能量方位</div>
                    <div class="info-value" id="luck-direction"></div>
                </div>
                <div class="info-item">
                    <div class="info-label">🥐 疗愈本味</div>
                    <div class="info-value" id="luck-food"></div>
                </div>
                <div class="info-item" style="grid-column: 1 / -1;">
                    <div class="info-label">📿 开运佩饰</div>
                    <div class="info-value" id="luck-jewelry"></div>
                </div>
            </div>

            <div class="dos-and-donts">
                <div class="action-box do">
                    <div class="action-title">✦ 顺势而为 (宜)</div>
                    <div class="action-list" id="today-do"></div>
                </div>
                <div class="action-box dont">
                    <div class="action-title">✧ 避其锋芒 (忌)</div>
                    <div class="action-list" id="today-dont"></div>
                </div>
            </div>

            <!-- 日常小叮嘱区域 -->
            <div class="daily-reminders">
                <div class="reminders-title" id="reminders-title"></div>
                <ul class="reminders-list" id="daily-reminders-text">
                    <!-- 动态插入 -->
                </ul>
            </div>

            <div class="lover-whisper">
                <div class="whisper-title" id="whisper-title"></div>
                <div class="whisper-text" id="whisper-text"></div>
            </div>
            
            <button class="restart-btn" onclick="resetStage()">重新洗牌占卜</button>
        </div>
    </div>

    <script>
        let userName = "宝贝"; // 默认称呼

        document.getElementById('user-name-input').addEventListener('keypress', function (e) {
            if (e.key === 'Enter') {
                startJourney();
            }
        });

        function startJourney() {
            const inputVal = document.getElementById('user-name-input').value.trim();
            if (inputVal !== "") {
                userName = inputVal;
            }
            
            document.getElementById('intro-screen').style.display = 'none';
            document.getElementById('main-content').style.display = 'flex';
        }

        const tarotDeck = [
            { num: "0", name: "愚者", en: "The Fool", icon: "⟡", summary: "新的开始，充满未知的冒险，跟随你内心的直觉去行动。" },
            { num: "I", name: "魔术师", en: "The Magician", icon: "☿", summary: "灵感迸发之日，你拥有将想法化为现实的强大魔力。" },
            { num: "II", name: "女祭司", en: "The High Priestess", icon: "☾", summary: "倾听内心的直觉，保持神秘与冷静，静观其变。" },
            { num: "III", name: "皇后", en: "The Empress", icon: "♀", summary: "充满丰盈与爱意的一天，尽情享受生活的美好与滋养。" },
            { num: "IV", name: "皇帝", en: "The Emperor", icon: "♃", summary: "展现你的掌控力与领导力，建立秩序，稳步向前。" },
            { num: "V", name: "教皇", en: "The Hierophant", icon: "☨", summary: "贵人指路，遵循传统智慧与心灵导师会带来精神启迪。" },
            { num: "VI", name: "恋人", en: "The Lovers", icon: "⚭", summary: "完美的契合与关系升温，面临遵从内心的重要抉择。" },
            { num: "VII", name: "战车", en: "The Chariot", icon: "⛤", summary: "意志力战胜阻碍！掌控节奏，你将迎来凯旋。" },
            { num: "VIII", name: "力量", en: "Strength", icon: "∞", summary: "以柔克刚的智慧，内在的勇气与耐心能帮你克服一切。" },
            { num: "IX", name: "隐士", en: "The Hermit", icon: "⚚", summary: "向内探索的时刻，适合独处深思，寻找内心的光芒。" },
            { num: "X", name: "命运之轮", en: "Wheel of Fortune", icon: "☸", summary: "命运的齿轮开始转动，顺应未知的惊喜，好运即将降临。" },
            { num: "XI", name: "正义", en: "Justice", icon: "⚖", summary: "保持理性与客观，付出终有回报，平衡是今日的关键。" },
            { num: "XII", name: "倒吊人", en: "The Hanged Man", icon: "▽", summary: "换个角度看问题，暂时的停滞与牺牲是为了更好的觉醒。" },
            { num: "XIII", name: "死神", en: "Death", icon: "♄", summary: "破茧成蝶的蜕变，结束旧的羁绊，迎接新生的契机。" },
            { num: "XIV", name: "节制", en: "Temperance", icon: "⟁", summary: "达到身心的和谐与平衡，沟通顺畅，情绪稳定。" },
            { num: "XV", name: "恶魔", en: "The Devil", icon: "⛧", summary: "警惕诱惑与深陷执念，是时候打破束缚，直面真实欲望。" },
            { num: "XVI", name: "高塔", en: "The Tower", icon: "♅", summary: "突如其来的转变，打破旧有的舒适圈，在废墟中重建认知。" },
            { num: "XVII", name: "星星", en: "The Star", icon: "✶", summary: "充满希望与治愈，剥离伪装，展现最真实的自我。" },
            { num: "XVIII", name: "月亮", en: "The Moon", icon: "☽", summary: "倾听潜意识的低语，拨开迷雾，直面内心的不安与幻象。" },
            { num: "XIX", name: "太阳", en: "The Sun", icon: "☼", summary: "能量满格，活力四射，驱散一切阴霾，今日诸事皆宜。" },
            { num: "XX", name: "审判", en: "Judgement", icon: "♇", summary: "深刻的觉醒与自我救赎，过去的努力终将得到命运的回音。" },
            { num: "XXI", name: "世界", en: "The World", icon: "❂", summary: "圆满与达成，一个阶段的完美收官，享受成功的喜悦。" }
        ];

        const loverWhispers = [
            "今天工作累不累？要是遇到烦心事，就先放一放，别把自己逼得太紧。今晚我熬了点暖胃的汤，等回家喝完，我们就舒舒服服地窝在沙发上看个老电影，什么都不去想，好不好？",
            "刚才看你在发呆，是不是又在偷偷给自己施加压力了？其实你已经做得很好了，别总是拿别人的标准来为难自己。慢慢来，不管遇到多大的事，回头看看，都有我在身后稳稳地撑着呢。",
            "看天气预报说这两天要降温了，出门一定要记得多带件厚外套，别总是不当回事让我操心。要是晚上起风觉得冷了，就立刻给我发消息，不管多晚在哪儿我都去接你回家。",
            "今天是不是又因为忙忘了好好吃饭？就算全世界都在催着往前跑，我也只关心饿不饿、累不累。晚上想吃什么记得提前告诉我，我买好菜在家里等你，我们一起慢慢吃顿饭。",
            "无论今天塔罗牌说了什么，我都希望你能拥有一个平稳的好心情。如果你觉得开心，我就陪一起笑；如果你觉得难过，我的肩膀随时准备好给靠。别忘了，从来不需要独自面对一切的。",
            "早上看你急匆匆出门，都没来得及好好抱抱。今天不管遇到什么难题，记得先深呼吸。实在搞不定的事情就交给我，或者晚上回来对我发发牢骚也没关系，我一直都在认真听。",
            "已经很晚了，别再对着屏幕发愁啦。把脑子里的那些小烦恼都统统关掉，安心躲进被窝里。我会一直在这里静静地陪着你，等睡熟了我再睡，祝今晚做个甜甜的梦。",
            "今天突然很想跟你说声谢谢。谢谢每天都这么努力地生活，也谢谢愿意把软弱和疲惫的一面毫无保留地交给我。你不知道，每次看到卸下防备依赖我的样子，我就觉得心里特别踏实。",
            "到了周末就彻底给自己放个假吧，把手机静音，谁的消息都可以暂时不回。我们去附近那个你念叨了很久的公园走走，或者干脆就在家里躺一整天。只要是跟你待在一起，就算是虚度光阴我也觉得很美好。",
            "有时候看你闷闷不乐的，我就特别心疼。想告诉你，在我面前永远都不用伪装坚强，也不用强撑着笑。哪怕什么都不做，什么都不说，只要还愿意待在我身边，对我来说就已经足够好了。",
            "厨房的锅里炖着你最爱喝的粥，马上就要咕嘟咕嘟冒泡啦。在外面再怎么像个冲锋陷阵的大人，回到家只要做个安心等着吃饭的小朋友就好了。快去洗洗手，准备开饭啦。",
            "刚才路过一家花店，看到一束花开得特别温柔，就觉得它好适合你，于是顺手买下来了。其实也没有什么特别的理由，就是突然间很想你，想早点见到你，想看收到花时笑起来的样子。",
            "知道你今天受委屈了，心里难受不想说话也没关系。我就在这里静静地陪着，给倒杯温水。等什么时候觉得想开口了，我就是最忠实的听众，永远坚定地站在这一边。",
            "以前我总觉得日子就是一天一天平淡地过，但现在，我开始无比期待每一个有你的明天。不管未来有什么变数，我想给你一个安稳的家，一个可以永远卸下防备、安心休息的避风港。",
            "最近好像总有点心事，眉头老是微微皱着。虽然什么都没跟我说，但我其实全都看在眼里。如果觉得累了，记得回头看看我，我一直在离最近的地方，随时准备给一个大大的拥抱。",
            "那个……杯子里我给装了热水，就放在手边，记得要喝啊，别总是非要等到口渴了才想起来找水。要是再学不会照顾自己，我可是要生气的，虽然最后还是只能无奈地继续照顾你。",
            "别总是拿条条框框的规矩来要求自己啦，那些都不重要。你已经特别棒、特别可爱了，在我眼里永远都在闪闪发光。所以，偶尔也多心疼心疼自己，顺着自己的心意来，好不好？",
            "不管今天下班有多晚，不管外面天多黑，家里的这盏灯我一定会为留着。回来的时候路上小心点，要是觉得害怕就给我打电话，我陪一直聊天，直到听到推开家门的声音。",
            "这张牌抽得好不好都没关系，把那些不好的预示都当做一阵风，吹过去就算了。有我在身边，每天的运势都只会是‘宜被宠爱’。听话，不去想那些烦心的事了，一切都有我呢。",
            "有时候觉得文字真的太单薄了，真想现在就能穿过屏幕去摸摸你的头。今天辛苦啦，如果觉得肩颈酸痛，晚上回家我帮好好揉一揉，让能彻底放松下来，睡个安稳的好觉。",
            "偶尔的发脾气、小任性，在我看来都特别生动真实。我爱的就是这个完整、会哭会笑的你，所以千万别害怕把最真实的一面给我看，不管什么样的情绪，我都会稳稳地把它们接住的。",
            "知道你最近在为什么事情发愁，这阵子确实有些难熬。但我对特别有信心，你比想象的还要勇敢得多。就算事情真的搞砸了也没事，我永远给托底，大不了我们从头再来呗。",
            "昨天整理手机相册，翻到了我们刚认识时候的照片。看着看着就忍不住笑了，时间过得真快啊。但幸好，现在陪在我身边的还是你，而且我觉得，现在的我比昨天还要更爱了。",
            "如果我今天因为忙碌忽略了你，或者说话没注意语气惹不开心了，一定要直接告诉我。我不想让带着哪怕一点点委屈去睡觉。你在我这里永远是第一位的，没有什么比的情绪更重要。",
            "今天下班路过那家一直说想去尝尝的甜品店，我排了好久的队才买到最后一份。现在它正乖乖躺在冰箱里等回来呢。哪怕是平平淡淡的日子，我也想尽量多给一点小小的期待。",
            "看着你留在沙发上的外套、桌上没喝完的半杯水，就觉得这样的生活特别踏实。有的气息充满整个房间，哪怕我们只是安静地各忙各的，连空气我都觉得是甜的。",
            "外面的世界太嘈杂了，总是要求你要懂事、要成熟、要情绪稳定。但在我这里，你可以永远不用长大。我想把你保护得好好的，让能一直保留那份天真和柔软，做想做的事。",
            "今天是不是遇到什么有趣的事了？我看你回消息的语气里都带着笑意。只要你开心，我这一整天的心情就都跟着好起来了。晚上吃饭的时候，一定要仔仔细细地把开心事讲给我听啊。",
            "醒了吗？昨晚看睡得那么沉，像个小猪一样，都没忍心叫。早饭我在厨房热着呢，洗漱完记得赶紧趁热吃。今天又是新的一天啦，带着我的牵挂，去好好生活吧，我在家里等你。",
            "不管今天你经历了什么，是好是坏，都已经过去了。现在闭上眼睛，深呼吸，感受一下此刻的宁静。我就在离不远的地方，用所有的温柔注视着。晚安，我最珍贵的你。"
        ];

        const dailyRemindersData = [
            "你今天记得多喝温水，别总是等渴了才喝呀。",
            "降温了，你要记得多穿件衣服，千万别感冒了。",
            "你下班回家路上注意安全，别边走边看手机。",
            "你今晚早点睡，不许再熬夜刷短视频了听到没。",
            "你出门前记得检查一下钥匙和手机带没带齐。",
            "不管多忙，你也要记得按时吃午饭哦。",
            "你今天如果有快递，下班顺手拿一下吧，别忘了。",
            "盯着电脑太久的话，你记得站起来活动一下脖子。",
            "回家记得先洗手，你换下来的衣服别乱扔，挂好哦。",
            "如果你觉得累了，今天就点外卖吧，别硬撑着做饭了。",
            "你今天别喝太多冰奶茶了，要多照顾自己的胃。",
            "晚上洗完澡，你记得吹干头发再睡，不然容易头痛。",
            "你出门别忘了带把伞，就算不下雨也可以遮遮太阳。",
            "手机电量充满了吗？你出门最好带上充电宝以防万一。",
            "你别总是揉眼睛，眼睛酸了就闭上休息会儿。",
            "如果你今天觉得不开心，记得随时给我打电话。",
            "走路的时候看路，你别总低着头想事情，小心绊倒。",
            "水果买回来记得吃，你别又放坏了呀。",
            "你多伸伸懒腰，别把自己崩得太紧了。",
            "你今天天气有点干，记得在桌上放杯水，时不时喝一口。",
            "晚上如果你觉得饿了，就稍微吃点热乎的，别总是空肚子睡觉。",
            "你今天出门前记得照照镜子，告诉自己你是最棒的！",
            "别总是为了工作委屈自己，你偶尔也要买点喜欢的小零食犒劳一下。",
            "如果你今天感觉很累，回家就直接躺下休息，家务活放着以后做也没关系。",
            "今天要是遇到不开心的事，你就在心里默念三遍不跟烂人计较。",
            "晚上洗漱完，你可以涂点香香的身体乳，睡得更舒服。",
            "你别总是习惯性熬夜，今晚试着早睡半小时，明天精神会更好。",
            "下午要是容易犯困，你记得给自己泡杯茶或者咖啡提提神。",
            "今天不管谁惹你生气，你都别气坏了自己，不值得。",
            "要是今天太阳不错，你记得抽空去晒晒太阳，补补钙。",
            "你晚上别看太久手机，特别是关了灯之后，很伤眼睛的。",
            "今天你要是想吃甜点就去买吧，开心最重要，别老想着减肥。",
            "你回家路上慢点走，看看路边的风景，放松一下心情。",
            "你睡觉前记得锁好门窗，盖好被子，别着凉了。",
            "如果你今天觉得特别顺利，那你就奖励自己一顿好吃的吧！",
            "你别把什么事情都憋在心里，不开心了要记得发泄出来。",
            "今天你尽量不要喝冰水了，喝点温热的，听话。",
            "你记得给手机充好电，别总是等到百分之一了才找充电器。",
            "要是你今天觉得很迷茫，就什么都别想，先好好睡一觉再说。",
            "你今天要是看到好笑的段子，记得存下来，以后分享给我。"
        ];

        const colors = ["暗夜玫瑰粉", "迷雾灰紫", "黑曜石色", "星空蓝", "香槟金", "浆果复古红", "珍珠白"];
        const items = ["复古丝绒发圈", "玫瑰香薰精油", "金属细边框墨镜", "黑胶唱片机", "手作皮革手账", "暗黑系美甲", "冷调香水"];
        const directions = ["正东方", "正南方", "西北方", "东北方", "西南方", "东南方"];
        const foods = ["玫瑰荔枝慕斯", "黑刺李果茶", "红酒炖牛肉", "海盐黑巧曲奇", "樱桃气泡水", "法式可丽露"];
        const jewelries = ["粉晶碎石手链", "做旧银质锁骨链", "复古黑玛瑙戒指", "巴洛克珍珠耳坠", "玫瑰金星月胸针"];
        const dos = ["享受一次独处", "换一款新香水", "安排精致的下午茶", "记录梦境", "给房间换个光源", "穿戴粉色单品", "整理旧物", "给绿植浇水"];
        const donts = ["参与八卦讨论", "冲动大额消费", "过度自我内耗", "打乱作息规律", "去喧闹拥挤处", "强行解释误会", "熬夜看手机", "不吃早饭"];

        const today = new Date();
        const dateString = `${today.getFullYear()}.${(today.getMonth() + 1).toString().padStart(2, '0')}.${today.getDate().toString().padStart(2, '0')}`;
        document.getElementById('current-date').innerText = `DAILY TAROT • ${dateString}`;

        function getRandomElements(arr, count) {
            let shuffled = [...arr].sort(() => Math.random() - 0.5);
            return shuffled.slice(0, count);
        }

        let sessionUsedIndexes = []; 
        function getUniqueWhisper() {
            let usedIndexes = [];
            try {
                const stored = localStorage.getItem('usedTarotWhispers');
                if (stored) {
                    usedIndexes = JSON.parse(stored);
                }
            } catch (e) {
                usedIndexes = sessionUsedIndexes;
            }
            if (usedIndexes.length >= loverWhispers.length) {
                usedIndexes = [];
            }
            let availableIndexes = [];
            for (let i = 0; i < loverWhispers.length; i++) {
                if (!usedIndexes.includes(i)) {
                    availableIndexes.push(i);
                }
            }
            const randomIndex = availableIndexes[Math.floor(Math.random() * availableIndexes.length)];
            usedIndexes.push(randomIndex);
            try {
                localStorage.setItem('usedTarotWhispers', JSON.stringify(usedIndexes));
            } catch (e) {
                sessionUsedIndexes = usedIndexes;
            }
            return loverWhispers[randomIndex];
        }

        let todaysCards = getRandomElements(tarotDeck, 3);
        let hasDrawn = false;

        function drawCard(selectedIndex) {
            if (hasDrawn) return;
            hasDrawn = true;

            const cards = document.querySelectorAll('.tarot-card');
            
            cards.forEach((card, index) => {
                const frontFace = document.getElementById(`front-${index}`);
                const cardData = todaysCards[index];
                
                frontFace.innerHTML = `
                    <div class="card-inner-border">
                        <div class="card-number">${cardData.num}</div>
                        <div class="card-illustration">
                            <div class="halo"></div>
                            <div class="card-seal">
                                <div class="seal-bg"></div>
                                <div class="seal-star"></div>
                                <div class="seal-symbol">${cardData.icon}</div>
                            </div>
                        </div>
                        <div class="card-name-banner">
                            <div class="name">${cardData.name}</div>
                            <div class="en-name">${cardData.en}</div>
                        </div>
                    </div>
                `;

                if (index === selectedIndex) {
                    card.classList.add('flipped');
                } else {
                    card.classList.add('disabled');
                }
            });

            const selectedCard = todaysCards[selectedIndex];
            
            const processText = (text) => text.replace(/你/g, userName);

            document.getElementById('result-card-name').innerText = `${selectedCard.num} • ${selectedCard.name}`;
            document.getElementById('fortune-summary').innerText = `“${processText(selectedCard.summary)}”`;
            
            document.getElementById('luck-color').innerText = getRandomElements(colors, 1)[0];
            document.getElementById('luck-item').innerText = getRandomElements(items, 1)[0];
            document.getElementById('luck-direction').innerText = getRandomElements(directions, 1)[0];
            document.getElementById('luck-food').innerText = getRandomElements(foods, 1)[0];
            document.getElementById('luck-jewelry').innerText = getRandomElements(jewelries, 1)[0];
            
            const selectedDos = getRandomElements(dos, 2);
            const selectedDonts = getRandomElements(donts, 2);
            document.getElementById('today-do').innerHTML = selectedDos.map(d => `• ${processText(d)}`).join('<br>');
            document.getElementById('today-dont').innerHTML = selectedDonts.map(d => `• ${processText(d)}`).join('<br>');

            document.getElementById('reminders-title').innerText = `📌 某人为${userName}留下了今日便签`;

            const selectedReminders = getRandomElements(dailyRemindersData, 2);
            document.getElementById('daily-reminders-text').innerHTML = selectedReminders.map(r => `<li>${processText(r)}</li>`).join('');

            const rawTitle = "你收到了一条来自宇宙的波段，让我来帮你翻译成句子：";
            document.getElementById('whisper-title').innerText = processText(rawTitle);

            let rawWhisper = getUniqueWhisper();
            document.getElementById('whisper-text').innerText = processText(rawWhisper);

            setTimeout(() => {
                const panel = document.getElementById('result-panel');
                panel.style.display = 'block';
                panel.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
            }, 800);
        }

        function resetStage() {
            document.getElementById('result-panel').style.display = 'none';
            
            const cards = document.querySelectorAll('.tarot-card');
            cards.forEach((card, index) => {
                card.classList.remove('flipped', 'disabled');
                setTimeout(() => {
                    document.getElementById(`front-${index}`).innerHTML = '';
                }, 500); 
            });

            todaysCards = getRandomElements(tarotDeck, 3);
            hasDrawn = false;

            window.scrollTo({ top: 0, behavior: 'smooth' });
        }
    </script>
</body>
</html>
