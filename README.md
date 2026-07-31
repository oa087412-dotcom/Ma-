# Ma-
Fot you 
<!DOCTYPE html>
<html lang="my">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Romantic Interactive App</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Pyidaungsu', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    body {
      background-color: #0f172a;
      color: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      overflow: hidden;
    }

    /* Container Box */
    .card {
      background: #1e293b;
      border: 1px solid #334155;
      border-radius: 24px;
      width: 350px;
      padding: 24px;
      text-align: center;
      box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.5);
      position: relative;
    }

    .hidden {
      display: none !important;
    }

    /* Avatar & Image */
    .avatar-img, .photo-img {
      width: 120px;
      height: 120px;
      border-radius: 20px;
      object-fit: cover;
      margin-bottom: 16px;
    }

    /* Titles & Texts */
    h2 {
      font-size: 1.4rem;
      margin-bottom: 20px;
      color: #4ade80;
    }
    .error-title {
      color: #ef4444;
    }
    p {
      font-size: 0.9rem;
      color: #cbd5e1;
      margin-bottom: 20px;
      line-height: 1.5;
    }

    /* Buttons */
    .btn {
      width: 100%;
      padding: 12px;
      margin: 8px 0;
      border: none;
      border-radius: 12px;
      background: #334155;
      color: #fff;
      font-size: 1rem;
      cursor: pointer;
      transition: all 0.2s;
    }
    .btn:hover {
      background: #475569;
    }
    .btn-green {
      background: linear-gradient(135deg, #22c55e, #16a34a);
    }
    .btn-green:hover {
      background: linear-gradient(135deg, #16a34a, #15803d);
    }

    /* Canvas Game Area */
    #gameCanvas {
      background: #090d16;
      border-radius: 16px;
      display: block;
      margin: 0 auto;
    }
  </style>
</head>
<body>

  <!-- STEP 1: Main Question -->
  <div id="step1" class="card">
    <img src="https://via.placeholder.com/120" class="avatar-img" alt="Cute Avatar" />
    <h2>ပြန်ချစ်ပါလားဟင်?</h2>
    <button class="btn" onclick="showPopup('မချစ်ဘူး ငြင်းတယ်')">မချစ်ဘူး ငြင်းတယ်</button>
    <button class="btn" onclick="goToStep('stepSuccess')">ချစ်တယ်</button>
    <button class="btn" onclick="showPopup('စဉ်းစားဦးမယ်')">စဉ်းစားဦးမယ်</button>
  </div>

  <!-- POPUP: Error Alert (For rejecting/thinking) -->
  <div id="popupError" class="card hidden" style="position: absolute; z-index: 10;">
    <h2 class="error-title">အို... မှားနေတယ်?</h2>
    <p>Error တက်နေလို့ ပြန်ရွေးလိုက်ပါဦး</p>
    <button class="btn btn-green" onclick="closePopup()">ပြန်ရွေးမယ်</button>
  </div>

  <!-- STEP 2: Success Confirmation -->
  <div id="stepSuccess" class="card hidden">
    <img src="https://via.placeholder.com/120" class="avatar-img" alt="Cat Love" />
    <h2>တကယ်ကြီးလား! 🎉</h2>
    <p>ထင်တော့ထင်နေပါတယ်... ပျော်လိုက်တာ ❤️</p>
    <button class="btn btn-green" onclick="startMiniGame()">ဆက်ကြည့်မယ်</button>
  </div>

  <!-- STEP 3: Game Intro Step -->
  <div id="stepGameIntro" class="card hidden">
    <h2>အဖြေမှန်သွားပြီးဆိုတော့ Mini Game လေးဆော့ရအောင် ✨</h2>
    <p>Target မှတ်စနစ် 10 ခု ပြည့်အောင် ပစ်ပေးရမယ် 🎯</p>
    <button class="btn btn-green" onclick="goToStep('stepGame')">ဂိမ်းဆော့မယ် 🏹</button>
  </div>

  <!-- STEP 4: Archery Mini Game -->
  <div id="stepGame" class="card hidden" style="width: 380px;">
    <h3 style="margin-bottom: 10px;">Mission: <span id="scoreText">0</span> / 10</h3>
    <canvas id="gameCanvas" width="330" height="400"></canvas>
  </div>

  <!-- STEP 5: Final Memory Screen -->
  <div id="stepFinal" class="card hidden">
    <h2>I Love You 👑</h2>
    <img src="https://via.placeholder.com/200x200" class="photo-img" style="width: 100%; height: 180px;" alt="Memory Photo" />
    <p style="font-size: 0.85rem; text-align: justify;">
      အမြဲတမ်း ဘေးနားမှာရှိနေပေးလို့ ကျေးဇူးတင်ပါတယ်။ အမှတ်တရလေးတွေ အများကြီး ဖန်တီးသွားကြတာပေါ့... ❤️
    </p>
    <button class="btn btn-green" onclick="showKissPopup()">ချစ်တယ် 💖</button>
  </div>

  <!-- POPUP: Final Kiss Dialog -->
  <div id="popupKiss" class="card hidden" style="position: absolute; z-index: 10;">
    <h2>*Muah* 💋</h2>
    <p>အများကြီး ချစ်တယ်! 🥰<br>အမြဲတမ်း စိတ်ချမ်းသာအောင် ထားမှာမို့ အနားမှာပဲ ရှိပေးပါ</p>
    <button class="btn btn-green" onclick="closeKissPopup()">Okay ပါရှင့် 💖</button>
  </div>

  <script>
    // Navigation Functions
    function goToStep(stepId) {
      document.querySelectorAll('.card').forEach(card => card.classList.add('hidden'));
      document.getElementById(stepId).classList.remove('hidden');
    }

    function showPopup(text) {
      document.getElementById('popupError').classList.remove('hidden');
    }

    function closePopup() {
      document.getElementById('popupError').classList.add('hidden');
      document.getElementById('step1').classList.remove('hidden');
    }

    function startMiniGame() {
      goToStep('stepGameIntro');
    }

    function showKissPopup() {
      document.getElementById('popupKiss').classList.remove('hidden');
    }

    function closeKissPopup() {
      document.getElementById('popupKiss').classList.add('hidden');
    }

    // Canvas Mini Game Logic
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    let score = 0;
    let targets = [];

    function initGame() {
      score = 0;
      document.getElementById('scoreText').innerText = score;
      targets = [];
      // Heart targets တည်ဆောက်ခြင်း
      for (let i = 0; i < 5; i++) {
        targets.push({
          x: Math.random() * (canvas.width - 40) + 20,
          y: Math.random() * 150 + 30,
          speed: Math.random() * 1.5 + 1
        });
      }
      requestAnimationFrame(gameLoop);
    }

    // Canvas ပေါ်မှာ နှလုံးသားပုံ နှိပ်ရင် မှတ်တက်ရန်
    canvas.addEventListener('click', (e) => {
      const rect = canvas.getBoundingClientRect();
      const clickX = e.clientX - rect.left;
      const clickY = e.clientY - rect.top;

      targets.forEach((target, index) => {
        const dist = Math.hypot(clickX - target.x, clickY - target.y);
        if (dist < 25) { // Hit target
          score++;
          document.getElementById('scoreText').innerText = score;
          target.y = -20;
          target.x = Math.random() * (canvas.width - 40) + 20;
          
          if (score >= 10) {
            setTimeout(() => {
              goToStep('stepFinal');
            }, 300);
          }
        }
      });
    });

    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Draw Background Moon/Stars
      ctx.fillStyle = "#fff";
      ctx.beginPath();
      ctx.arc(280, 50, 15, 0, Math.PI * 2);
      ctx.fill();

      // Draw Heart Targets
      targets.forEach(t => {
        t.y += t.speed;
        if (t.y > canvas.height) t.y = -20;

        ctx.fillStyle = "#ff4757";
        ctx.font = "24px sans-serif";
        ctx.fillText("❤️", t.x - 12, t.y + 8);
      });

      if (score < 10) {
        requestAnimationFrame(gameLoop);
      }
    }

    // Step Switch အလိုက် ဂိမ်း စတင်ခြင်း
    const observer = new MutationObserver(() => {
      if (!document.getElementById('stepGame').classList.contains('hidden')) {
        initGame();
      }
    });
    observer.observe(document.getElementById('stepGame'), { attributes: true });

  </script>
</body>
</html>
