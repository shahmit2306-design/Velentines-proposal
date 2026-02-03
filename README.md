# Velentines-proposal
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Valentine 💖</title>

<style>
  body {
    margin: 0;
    font-family: "Times New Roman", Times, serif;
    background: #fff5f8;
    color: #333;
    height: 100vh;
    overflow: hidden;
  }

  .screen {
    position: absolute;
    inset: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
  }

  .hidden {
    display: none;
  }

  h1 {
    font-size: 2.6rem;
    margin-bottom: 2rem;
  }

  .buttons {
    display: flex;
    gap: 1.5rem;
  }

  .btn {
    padding: 1rem 2.5rem;
    font-size: 1.3rem;
    font-weight: bold;
    border-radius: 14px;
    border: 3px solid #ff6fa8;
    background: white;
    cursor: pointer;
    position: relative;
    user-select: none;
  }

  /* YES text only pink */
  #yes span {
    color: #ff3e8e;
  }

  #no span {
    color: black;
  }

  /* Floating animation for NO */
  @keyframes runAway {
    0%   { transform: translate(-50%, -50%); }
    20%  { transform: translate(60vw, -40vh); }
    40%  { transform: translate(-60vw, 30vh); }
    60%  { transform: translate(50vw, 50vh); }
    80%  { transform: translate(-50vw, -20vh); }
    100% { transform: translate(-50%, -50%); }
  }

  .floating {
    position: fixed;
    top: 50%;
    left: 50%;
    animation: runAway 4s linear infinite;
    z-index: 999;
  }

  .bottom-text {
    position: absolute;
    bottom: 20px;
    width: 100%;
    font-size: 1.1rem;
  }

  /* YAY screen */
  #yay h1 {
    font-size: 3.2rem;
    color: #ff3e8e;
  }

  #yay img {
    margin-top: 1.5rem;
    max-width: 80%;
    max-height: 60%;
    border-radius: 18px;
  }
</style>
</head>

<body>

<!-- FIRST SLIDE -->
<div id="ask" class="screen">
  <h1>Mokshi will you be my velentine? 🧸</h1>

  <div class="buttons">
    <div id="yes" class="btn"><span>Yes</span></div>
    <div id="no" class="btn"><span>No</span></div>
  </div>

  <div class="bottom-text">
    Dum hai to No click ker ke dikha
  </div>
</div>

<!-- SECOND SLIDE -->
<div id="yay" class="screen hidden">
  <h1>Yay!</h1>
  <!-- 🔴 Replace this GIF link with your own -->
  <img src="YOUR_GIF_URL_HERE" alt="Yay GIF">
</div>

<script>
  const yesBtn = document.getElementById("yes");
  const noBtn = document.getElementById("no");
  const askScreen = document.getElementById("ask");
  const yayScreen = document.getElementById("yay");

  yesBtn.addEventListener("click", () => {
    askScreen.classList.add("hidden");
    yayScreen.classList.remove("hidden");
  });

  noBtn.addEventListener("click", () => {
    noBtn.classList.add("floating");
  });
</script>

</body>
</html>