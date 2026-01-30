
[valentine.html](https://github.com/user-attachments/files/24956562/valentine.html)
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Ximena, be my Valentine? 💘</title>
  <style>
    :root { --bg1:#ffdde1; --bg2:#ee9ca7; --card:#ffffffcc; --text:#2b2b2b; }
    * { box-sizing: border-box; font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; }
    body{
      margin:0; min-height:100vh; display:grid; place-items:center;
      background: radial-gradient(circle at 20% 20%, #fff 0, transparent 40%),
                  radial-gradient(circle at 80% 30%, #fff 0, transparent 35%),
                  linear-gradient(135deg,var(--bg1),var(--bg2));
      overflow:hidden;
    }
    .card{
      width:min(600px,92vw);
      background: var(--card);
      border: 1px solid #ffffff80;
      backdrop-filter: blur(10px);
      border-radius: 24px;
      padding: 28px;
      box-shadow: 0 20px 60px #0000001f;
      text-align:center;
      position:relative;
    }
    h1{ margin:0 0 10px; font-size: clamp(26px, 5vw, 42px); color:var(--text); }
    h2{ margin:0 0 10px; font-size: clamp(22px, 4.5vw, 34px); color:var(--text); }
    p{ margin:0 0 14px; font-size: 16px; color:#444; line-height: 1.45; }
    .hearts{ font-size: 28px; letter-spacing: 6px; margin-bottom: 14px; }
    .buttons{
      display:flex; gap:12px; justify-content:center; flex-wrap:wrap;
      margin-top: 14px;
    }
    button{
      border:0; border-radius: 14px; padding: 14px 18px;
      font-size: 16px; cursor:pointer; transition: transform .15s ease;
      box-shadow: 0 10px 25px #0000001a;
      user-select:none;
    }
    .primary{
      background: #ff3b7a; color:white;
    }
    .primary:hover{ transform: translateY(-2px) scale(1.02); }
    .ghost{
      background: #ffffff; color:#222; border:1px solid #00000014;
      position:relative;
    }
    .footer{
      margin-top: 16px;
      font-size: 13px; color:#555;
    }
    .toast{
      position: fixed; bottom: 18px; left: 50%; transform: translateX(-50%);
      background:#111; color:#fff; padding: 12px 14px; border-radius: 999px;
      opacity:0; pointer-events:none; transition: opacity .2s ease;
      font-size: 14px;
      max-width: 92vw;
      text-align:center;
    }
    .toast.show{ opacity:1; }
    .confetti{
      position: fixed; inset:0; pointer-events:none; overflow:hidden;
    }
    .piece{
      position:absolute; top:-20px; width:10px; height:16px;
      border-radius: 4px;
      animation: fall linear forwards;
      opacity:.9;
    }
    @keyframes fall{
      to{ transform: translateY(110vh) rotate(720deg); }
    }
    .small{ font-size: 14px; opacity:.95; }

    /* simple "page" switch */
    .page{ display:none; }
    .page.active{ display:block; }
    .pill{
      display:inline-block;
      padding: 8px 12px;
      border-radius: 999px;
      background: #ffffff;
      border: 1px solid #00000014;
      font-size: 14px;
      color:#333;
      box-shadow: 0 10px 25px #00000010;
      margin: 6px 6px 0 6px;
    }
  </style>
</head>
<body>
  <!-- ASK PAGE -->
  <div class="card page active" id="askPage">
    <div class="hearts">💗 💘 💗</div>
    <h1>Ximena… will you be my Valentine?</h1>

    <p>
      I'm not really the best at typing but your gorgeous princesa prettier than the sunset and shining like the sun
    </p>
    <p>
      So here’s the truth: I really like you princess hermosa, and I’d love to spend Valentine’s with you 💞
    </p>
    <p class="small">
      Warning: clicking “Yes” may cause me to start streaming and crying and jumping around like a little kid 
    </p>

    <div class="buttons">
      <button class="primary" id="yesBtn">Yes! 💞</button>
      <button class="ghost" id="noBtn">No 😅</button>
    </div>

    <div class="footer">Made with love (and a little panic) 💌</div>
  </div>

  <!-- YES PAGE -->
  <div class="card page" id="yesPage">
    <div class="hearts">💖 ✨ 💖</div>
    <h2>AHHH YESSS!!! 😍</h2>
    <p>
      Ximena, you just made me the happiest person ever. Te amp princess
    </p>

    <p><strong>Official Valentine date plan:</strong></p>
    <p>
      🌟 <strong>Watching the stars</strong><br/>
      🍗 <strong>Wingstop</strong><br/>
      🛍️ <strong>Taking you shopping</strong><br/>
      (because you deserve it, obviously 💅)
    </p>

    <div>
      <span class="pill">Cute vibes only</span>
      <span class="pill">Extra heart emojis</span>
      <span class="pill">Zero “No” allowed</span>
    </div>

    <div class="buttons" style="margin-top:18px;">
      <button class="primary" id="copyBtn">Copy this to text you 💌</button>
      <button class="ghost" id="backBtn">Back</button>
    </div>

    <div class="footer">Now I owe you the best date ever 😌</div>
  </div>

  <div class="toast" id="toast">Copied! 💖</div>
  <div class="confetti" id="confetti"></div>

  <script>
    const askPage = document.getElementById("askPage");
    const yesPage = document.getElementById("yesPage");

    const noBtn = document.getElementById("noBtn");
    const yesBtn = document.getElementById("yesBtn");
    const backBtn = document.getElementById("backBtn");
    const copyBtn = document.getElementById("copyBtn");

    const toast = document.getElementById("toast");
    const confetti = document.getElementById("confetti");

    function rand(min, max){ return Math.random() * (max - min) + min; }

    // Make the "No" button run away (within the card area)
    function dodge(){
      const cardRect = askPage.getBoundingClientRect();
      const btnRect = noBtn.getBoundingClientRect();

      const padding = 10;
      const maxX = cardRect.width - btnRect.width - padding;
      const maxY = cardRect.height - btnRect.height - padding;

      const x = rand(padding, Math.max(padding, maxX));
      const y = rand(padding, Math.max(padding, maxY));

      noBtn.style.position = "absolute";
      noBtn.style.left = `${x}px`;
      noBtn.style.top  = `${y}px`;
    }
    noBtn.addEventListener("mouseenter", dodge);
    noBtn.addEventListener("touchstart", (e) => { e.preventDefault(); dodge(); });

    function showYesPage(){
      askPage.classList.remove("active");
      yesPage.classList.add("active");
      launchConfetti();
    }

    function showAskPage(){
      yesPage.classList.remove("active");
      askPage.classList.add("active");
    }

    yesBtn.addEventListener("click", showYesPage);
    backBtn.addEventListener("click", showAskPage);

    function launchConfetti(){
      confetti.innerHTML = "";
      const count = 140;
      for(let i=0; i<count; i++){
        const d = document.createElement("div");
        d.className = "piece";
        d.style.left = `${rand(0, 100)}vw`;
        d.style.animationDuration = `${rand(1.6, 3.8)}s`;
        d.style.animationDelay = `${rand(0, 0.25)}s`;
        d.style.transform = `rotate(${rand(0, 360)}deg)`;
        d.style.background = `hsl(${rand(0, 360)}, 90%, 60%)`;
        confetti.appendChild(d);
      }
    }

    function showToast(msg){
      toast.textContent = msg;
      toast.classList.add("show");
      setTimeout(() => toast.classList.remove("show"), 1800);
    }

    copyBtn.addEventListener("click", async () => {
      const message =
`Ximena 😍
You said YES to being my Valentine 💖
Date plan: watching the stars ✨ + Wingstop 🍗 + taking you shopping 🛍️
When are you free?`;

      try{
        await navigator.clipboard.writeText(message);
        showToast("Copied! 💖");
      }catch(e){
        // fallback if clipboard not allowed
        showToast("Copy blocked—just screenshot 😅");
      }
    });
  </script>
</body>
</html>
