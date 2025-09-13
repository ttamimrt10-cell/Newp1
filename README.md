<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ads on Monetag mini app</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background-color: #121212;
      color: #fff;
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .container {
      text-align: center;
      background: #1f1f1f;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.6);
      width: 340px;
    }
    .container h1 {
      font-size: 22px;
      color: #00ff88;
      margin-bottom: 12px;
    }
    .developer {
      font-size: 12px;
      background: #00ff88;
      padding: 5px 10px;
      border-radius: 5px;
      margin-bottom: 15px;
      display: inline-block;
      cursor: pointer;
      color: #000;
      font-weight: bold;
    }
    .stats p {
      margin: 5px 0;
      font-size: 15px;
    }
    .progress-circle {
      width: 90px;
      height: 90px;
      border: 5px solid #00ff88;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 12px auto;
      font-size: 18px;
      font-weight: bold;
      color: #00ff88;
    }
    .buttons button {
      width: 95%;
      margin: 6px 0;
      padding: 10px;
      font-size: 15px;
      border: none;
      border-radius: 6px;
      color: #fff;
      background: #00b894;
      cursor: pointer;
      transition: 0.3s;
    }
    .buttons button:hover {
      background: #fd7e14;
    }
    .withdraw-section {
      margin-top: 20px;
      display: none;
      padding: 15px;
      background-color: #2a2a2a;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.5);
    }
    .withdraw-section input, .withdraw-section select {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 5px;
      border: 2px solid #555;
      background: #333;
      color: #fff;
    }
    .withdraw-section button {
      background-color: #00ff88;
      width: 100%;
      padding: 12px;
      font-size: 16px;
      margin-top: 12px;
      border-radius: 6px;
      font-weight: bold;
      color: #000;
      cursor: pointer;
    }
    .withdraw-section button:hover {
      background-color: #00b894;
      color: #fff;
    }
    #withdraw-status {
      color: orange;
      font-size: 14px;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Watch ads and earn</h1>
    <div class="developer" onclick="window.location.href='https://t.me/dkwin_tamim'">Developer by Md Srabon Ahmed</div>

    <div class="user-info">
      <p>Welcome, <span id="user-name"></span></p>
    </div>

    <div class="stats">
      <p>Watched Ads: <span id="watched-ads">0</span></p>
      <p>Earned Points: <span id="earned-points">0</span></p>
    </div>

    <div class="progress-circle">
      <span id="ads-progress">0%</span>
    </div>

    <div class="buttons">
      <button onclick="watchAd()">Watch Ad</button>
      <button id="auto-ad-btn" onclick="startAutoAds()">Auto Show Ads</button>
      <button id="stop-auto-btn" onclick="stopAutoAds()" disabled>Stop Auto Ads</button>
      <button onclick="showWithdrawForm()">Withdraw</button>
    </div>

    <!-- Withdraw Section -->
    <div class="withdraw-section" id="withdraw-section">
      <h3 style="color: orange; margin-bottom: 10px;">Withdrawal Request</h3>
      <input type="number" id="withdraw-amount" placeholder="Enter Points to Withdraw" />
      <select id="payment-method">
        <option value="bkash">Bkash</option>
        <option value="nagad">Nagad</option>
        <option value="manual">Manual</option>
      </select>
      <input type="text" id="withdraw-phone" placeholder="Enter Phone Number" />
      <button onclick="withdrawPoints()">Submit</button>
      <p id="withdraw-status"></p>
    </div>
  </div>

  <!-- Monetag Script -->
  <script src='//jagnaimsee.net/vignette.min.js' data-zone='9865931' data-sdk='show_9865931'></script>

  <script>
    const MIN_WITHDRAW_POINTS = 5;
    const ADMIN_USER_ID = 8076306164;  // তোমার chat id
    const BOT_TOKEN = "YOUR_BOT_TOKEN"; // এখানে তোমার আসল বট টোকেন বসাও

    let watchedAdsCount = 0;
    let earnedPoints = 0.00;
    let autoAdInterval;

    // Simulate username (বাস্তবে Telegram WebApp থেকে নিতে হবে)
    const telegramUserId = "123456"; 
    const telegramUserName = "@exampleUser";

    document.getElementById("user-name").textContent = telegramUserName;

    // LocalStorage থেকে আগের ডাটা লোড
    if (localStorage.getItem('watchedAdsCount')) {
      watchedAdsCount = parseInt(localStorage.getItem('watchedAdsCount'));
      earnedPoints = parseFloat(localStorage.getItem('earnedPoints'));
      document.getElementById('watched-ads').textContent = watchedAdsCount;
      document.getElementById('earned-points').textContent = earnedPoints.toFixed(2);
      updateProgressCircle();
    }

    // Ad দেখার ফাংশন
    function watchAd() {
      if (typeof show_9865931 === 'function') {
        show_9865931().then(() => {
          watchedAdsCount++;
          earnedPoints += 0.50;
          document.getElementById('watched-ads').textContent = watchedAdsCount;
          document.getElementById('earned-points').textContent = earnedPoints.toFixed(2);
          localStorage.setItem('watchedAdsCount', watchedAdsCount);
          localStorage.setItem('earnedPoints', earnedPoints.toFixed(2));
          updateProgressCircle();
        });
      }
    }

    // Progress Circle Update
    function updateProgressCircle() {
      const percentage = Math.min((watchedAdsCount / 10) * 100, 100);
      document.getElementById('ads-progress').textContent = `${percentage}%`;
    }

    // Auto Ads Start
    function startAutoAds() {
      autoAdInterval = setInterval(watchAd, 5000);
      document.getElementById('auto-ad-btn').disabled = true;
      document.getElementById('stop-auto-btn').disabled = false;
    }

    // Auto Ads Stop
    function stopAutoAds() {
      clearInterval(autoAdInterval);
      document.getElementById('auto-ad-btn').disabled = false;
      document.getElementById('stop-auto-btn').disabled = true;
    }

    // Withdraw Form Show
    function showWithdrawForm() {
      document.getElementById('withdraw-section').style.display = 'block';
    }

    // Withdraw Function
    function withdrawPoints() {
      const amount = document.getElementById('withdraw-amount').value;
      const paymentMethod = document.getElementById('payment-method').value;
      const phoneNumber = document.getElementById('withdraw-phone').value;

      if (amount < MIN_WITHDRAW_POINTS) {
        document.getElementById('withdraw-status').textContent = `Minimum ${MIN_WITHDRAW_POINTS} points required.`;
        return;
      }
      if (amount > earnedPoints) {
        document.getElementById('withdraw-status').textContent = `Insufficient balance. You have ${earnedPoints.toFixed(2)} points.`;
        return;
      }

      // Balance Update
      earnedPoints -= parseFloat(amount);
      document.getElementById('earned-points').textContent = earnedPoints.toFixed(2);
      localStorage.setItem('earnedPoints', earnedPoints.toFixed(2));

      // Admin Message
      const message = `💰 New Withdraw Request\n\n👤 UserID: ${telegramUserId}\n💵 Amount: ${amount}\n📱 Number: ${phoneNumber}\n💳 Type: ${paymentMethod}\n🔗 Username: ${telegramUserName}\n⏰ Time: ${new Date().toLocaleString()}`;

      sendWithdrawRequestToAdmin(message);

      // User Message
      document.getElementById('withdraw-status').textContent = "✅ Withdraw request sent!";
      document.getElementById('withdraw-amount').value = '';
      document.getElementById('withdraw-phone').value = '';
    }

    // Withdraw Request to Admin
    function sendWithdrawRequestToAdmin(message) {
      fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage?chat_id=${ADMIN_USER_ID}&text=${encodeURIComponent(message)}`)
        .then(r => r.json())
        .then(d => console.log("Sent to Admin:", d))
        .catch(e => console.error("Error:", e));
    }
  </script>
</body>
</html>
