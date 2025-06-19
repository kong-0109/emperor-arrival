```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>皇上驾到 - 宫廷礼仪模拟器</title>
    <link href="https://fonts.googleapis.com/css2?family=Ma+Shan+Zheng&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }
        
        body {
            background: linear-gradient(135deg, #1a0a47 0%, #3a1c71 50%, #1a0a47 100%);
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            font-family: 'Ma Shan Zheng', 'SimSun', serif;
            color: #ffd700;
            overflow: hidden;
            position: relative;
        }
        
        .palace {
            position: absolute;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(circle at 10% 20%, rgba(255, 215, 0, 0.05) 0%, transparent 20%),
                radial-gradient(circle at 90% 80%, rgba(255, 215, 0, 0.05) 0%, transparent 20%),
                repeating-linear-gradient(45deg, rgba(0, 0, 0, 0.1) 0px, rgba(0, 0, 0, 0.1) 1px, transparent 1px, transparent 11px),
                repeating-linear-gradient(-45deg, rgba(0, 0, 0, 0.1) 0px, rgba(0, 0, 0, 0.1) 1px, transparent 1px, transparent 11px);
            background-size: 100% 100%, 100% 100%, 100px 100px, 100px 100px;
            z-index: -1;
        }
        
        .title-container {
            text-align: center;
            margin-bottom: 50px;
            position: relative;
            z-index: 10;
            animation: fadeIn 1s ease-out;
        }
        
        h1 {
            font-size: 5rem;
            text-shadow: 0 0 20px rgba(255, 215, 0, 0.7), 0 0 40px rgba(255, 0, 0, 0.5);
            letter-spacing: 10px;
            margin-bottom: 10px;
            position: relative;
            animation: titleFloat 3s infinite ease-in-out;
            background: linear-gradient(to bottom, #ffd700, #daa520);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 10px rgba(0, 0, 0, 0.7));
        }
        
        .subtitle {
            font-size: 1.8rem;
            color: rgba(255, 255, 255, 0.9);
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
            margin-top: 20px;
        }
        
        .buttons-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 30px;
            animation: slideUp 1s ease-out;
        }
        
        .kneel-btn {
            font-size: 2.5rem;
            padding: 20px 80px;
            background: linear-gradient(to bottom, #c62828, #8e0000);
            color: #ffd700;
            border: 5px solid #ffd700;
            border-radius: 15px;
            cursor: pointer;
            font-family: 'Ma Shan Zheng', serif;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
            font-weight: bold;
            letter-spacing: 5px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .kneel-btn:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 25px rgba(0, 0, 0, 0.4);
            background: linear-gradient(to bottom, #d32f2f, #b71c1c);
        }
        
        .kneel-btn:active {
            transform: translateY(5px);
        }
        
        .kneel-btn::after {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: rgba(255, 255, 255, 0.1);
            transform: rotate(30deg);
            transition: all 0.5s ease;
        }
        
        .kneel-btn:hover::after {
            transform: rotate(30deg) translate(100px, 100px);
        }
        
        .refuse-btn {
            font-size: 1.8rem;
            padding: 15px 60px;
            background: linear-gradient(to bottom, #2e7d32, #1b5e20);
            color: white;
            border: 3px solid #a5d6a7;
            border-radius: 10px;
            cursor: pointer;
            font-family: 'Ma Shan Zheng', serif;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            letter-spacing: 3px;
            text-shadow: 0 1px 3px rgba(0, 0, 0, 0.4);
        }
        
        .refuse-btn:hover {
            background: linear-gradient(to bottom, #388e3c, #2e7d32);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
        }
        
        .refuse-btn:active {
            transform: translateY(3px);
        }
        
        .emperor {
            position: absolute;
            top: -100px;
            font-size: 10rem;
            animation: emperorEnter 2s forwards;
            text-shadow: 0 0 30px rgba(255, 215, 0, 0.8);
            filter: drop-shadow(0 0 20px rgba(255, 215, 0, 0.8));
            z-index: 5;
        }
        
        .message {
            position: absolute;
            bottom: 50px;
            font-size: 1.8rem;
            background: rgba(0, 0, 0, 0.7);
            padding: 15px 30px;
            border-radius: 10px;
            opacity: 0;
            transition: opacity 0.5s;
            text-align: center;
            max-width: 80%;
            border: 2px solid #ffd700;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
            z-index: 20;
        }
        
        .dragon {
            position: absolute;
            width: 100px;
            height: 100px;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><path d="M20,50 Q40,30 60,50 T100,50" stroke="%23ffd700" fill="none" stroke-width="2"/><circle cx="20" cy="50" r="5" fill="%23ffd700"/></svg>');
            background-size: contain;
            background-repeat: no-repeat;
            opacity: 0.3;
            animation: dragonMove 15s infinite linear;
            z-index: 1;
        }
        
        @keyframes emperorEnter {
            0% { 
                top: -100px;
                opacity: 0;
                transform: scale(0.5);
            }
            100% { 
                top: 80px;
                opacity: 1;
                transform: scale(1);
            }
        }
        
        @keyframes titleFloat {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-15px); }
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(50px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(100px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes dragonMove {
            0% { transform: translateX(-100px) translateY(0); }
            25% { transform: translateX(calc(100vw + 100px)) translateY(100px); }
            50% { transform: translateX(calc(100vw + 100px)) translateY(200px); }
            75% { transform: translateX(-100px) translateY(100px); }
            100% { transform: translateX(-100px) translateY(0); }
        }
        
        .kneeling-effect {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 100;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.5s;
        }
        
        .kneeling-effect.show {
            opacity: 1;
            pointer-events: all;
        }
        
        .kneeling-effect h2 {
            font-size: 4rem;
            margin-bottom: 30px;
            color: #ffd700;
            text-shadow: 0 0 20px rgba(255, 215, 0, 0.8);
            letter-spacing: 5px;
            animation: textGlow 2s infinite alternate;
        }
        
        .courtier {
            font-size: 3rem;
            margin: 10px;
            animation: bow 2s infinite alternate;
            filter: drop-shadow(0 0 10px rgba(255, 215, 0, 0.5));
        }
        
        @keyframes bow {
            0% { transform: translateY(0) rotate(0deg); }
            100% { transform: translateY(20px) rotate(5deg); }
        }
        
        @keyframes textGlow {
            0% { text-shadow: 0 0 10px rgba(255, 215, 0, 0.8); }
            100% { text-shadow: 0 0 30px rgba(255, 215, 0, 1), 0 0 50px rgba(255, 100, 0, 0.8); }
        }
        
        .continue-btn {
            margin-top: 30px;
            font-size: 1.5rem;
            padding: 10px 30px;
            background: #ffd700;
            color: #8e0000;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            font-family: 'Ma Shan Zheng', serif;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
        }
        
        .continue-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
            background: #ffec8b;
        }
        
        .deploy-info {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background: rgba(0, 0, 0, 0.5);
            padding: 10px 15px;
            border-radius: 5px;
            font-size: 0.9rem;
            color: #aaa;
            z-index: 10;
        }
        
        .deploy-info a {
            color: #ffd700;
            text-decoration: none;
        }
        
        .deploy-info a:hover {
            text-decoration: underline;
        }
        
        .floating-text {
            position: absolute;
            font-size: 1.2rem;
            color: rgba(255, 215, 0, 0.7);
            animation: floatText 15s linear infinite;
            z-index: 2;
        }
        
        @keyframes floatText {
            0% {
                transform: translateY(100vh) rotate(0deg);
                opacity: 0;
            }
            10% {
                opacity: 1;
            }
            90% {
                opacity: 1;
            }
            100% {
                transform: translateY(-100px) rotate(5deg);
                opacity: 0;
            }
        }
        
        @media (max-width: 768px) {
            h1 { font-size: 3rem; }
            .subtitle { font-size: 1.4rem; }
            .kneel-btn { font-size: 2rem; padding: 15px 50px; }
            .refuse-btn { font-size: 1.5rem; padding: 12px 40px; }
            .emperor { font-size: 7rem; }
            .kneeling-effect h2 { font-size: 2.5rem; }
        }
    </style>
</head>
<body>
    <div class="palace"></div>
    
    <div class="emperor">👑</div>
    
    <div class="title-container">
        <h1>皇上驾到</h1>
        <div class="subtitle">文武百官速速接驾！</div>
    </div>
    
    <div class="buttons-container">
        <button id="kneelBtn" class="kneel-btn">跪下</button>
        <button id="refuseBtn" class="refuse-btn">不跪</button>
    </div>
    
    <div id="message" class="message"></div>
    
    <div class="kneeling-effect" id="kneelingEffect">
        <h2>吾皇万岁万岁万万岁！</h2>
        <div class="courtier">🧎</div>
        <div class="courtier">🧎</div>
        <div class="courtier">🧎</div>
        <button class="continue-btn">平身</button>
    </div>
    
    <div class="deploy-info">
        公网部署提示：将此HTML文件上传至 <a href="https://pages.github.com/" target="_blank">GitHub Pages</a>, 
        <a href="https://vercel.com/" target="_blank">Vercel</a> 或 
        <a href="https://www.netlify.com/" target="_blank">Netlify</a>
    </div>
    
    <audio id="gongSound" preload="auto">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-medieval-show-fanfare-announcement-226.mp3" type="audio/mpeg">
    </audio>
    <audio id="kneelSound" preload="auto">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-musical-hit-2333.mp3" type="audio/mpeg">
    </audio>
    <audio id="warningSound" preload="auto">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-unlock-game-notification-253.mp3" type="audio/mpeg">
    </audio>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const kneelBtn = document.getElementById('kneelBtn');
            const refuseBtn = document.getElementById('refuseBtn');
            const message = document.getElementById('message');
            const kneelingEffect = document.getElementById('kneelingEffect');
            const gongSound = document.getElementById('gongSound');
            const kneelSound = document.getElementById('kneelSound');
            const warningSound = document.getElementById('warningSound');
            let kneelSize = 1;
            let refuseCount = 0;
            
            // 播放背景音效
            setTimeout(() => {
                gongSound.play().catch(e => console.log("Audio play prevented by browser policy"));
            }, 500);
            
            // 创建漂浮文字
            function createFloatingText(text) {
                const floatingText = document.createElement('div');
                floatingText.className = 'floating-text';
                floatingText.textContent = text;
                floatingText.style.left = `${Math.random() * 90}%`;
                floatingText.style.animationDuration = `${15 + Math.random() * 15}s`;
                floatingText.style.fontSize = `${0.8 + Math.random() * 1.2}rem`;
                document.body.appendChild(floatingText);
                
                // 动画结束后移除元素
                setTimeout(() => {
                    floatingText.remove();
                }, 20000);
            }
            
            // 初始化漂浮文字
            const floatingTexts = [
                "奉天承运，皇帝诏曰",
                "文武百官，叩首行礼",
                "天子威严，不可侵犯",
                "御驾亲临，众生跪迎",
                "皇恩浩荡，泽被苍生"
            ];
            
            setInterval(() => {
                createFloatingText(floatingTexts[Math.floor(Math.random() * floatingTexts.length)]);
            }, 3000);
            
            // 跪下按钮事件
            kneelBtn.addEventListener('click', function() {
                kneelSound.play().catch(e => console.log("Audio play prevented by browser policy"));
                kneelingEffect.classList.add('show');
                message.textContent = "";
                refuseCount = 0;
                kneelSize = 1;
                kneelBtn.style.transform = `scale(${kneelSize})`;
                kneelBtn.style.fontSize = `${2.5 * kneelSize}rem`;
            });
            
            // 不跪按钮事件
            refuseBtn.addEventListener('click', function() {
                kneelSize += 0.2;
                refuseCount++;
                kneelBtn.style.transform = `scale(${kneelSize})`;
                kneelBtn.style.fontSize = `${2.5 * kneelSize}rem`;
                
                // 播放警告音效
                warningSound.currentTime = 0;
                warningSound.play().catch(e => console.log("Audio play prevented by browser policy"));
                
                // 根据拒绝次数显示不同的消息
                const messages = [
                    "大胆！竟敢不跪？！",
                    "欺君之罪，当诛九族！",
                    "侍卫！拿下这个逆贼！",
                    "来人啊！有人大不敬！",
                    "皇威浩荡，岂容尔等放肆！"
                ];
                
                let selectedMessage;
                if (refuseCount < 3) {
                    selectedMessage = messages[refuseCount - 1] || messages[0];
                } else {
                    selectedMessage = messages[Math.floor(Math.random() * messages.length)];
                }
                
                message.textContent = selectedMessage;
                message.style.opacity = "1";
                
                // 5秒后隐藏消息
                setTimeout(() => {
                    message.style.opacity = "0";
                }, 3000);
            });
            
            // 平身按钮事件
            document.querySelector('.continue-btn').addEventListener('click', function() {
                kneelingEffect.classList.remove('show');
            });
            
            // 添加一些随机移动的龙元素
            setInterval(() => {
                const dragon = document.createElement('div');
                dragon.className = 'dragon';
                dragon.style.top = `${Math.random() * 100}%`;
                dragon.style.left = '-100px';
                dragon.style.animationDelay = `${Math.random() * -15}s`;
                dragon.style.opacity = `${0.1 + Math.random() * 0.3}`;
                dragon.style.transform = `scale(${0.5 + Math.random() * 1})`;
                document.body.appendChild(dragon);
                
                // 20秒后移除
                setTimeout(() => {
                    dragon.remove();
                }, 20000);
            }, 2000);
        });
    </script>
</body>
</html>
```