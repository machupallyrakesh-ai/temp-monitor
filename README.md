<!DOCTYPE html>

<html>
<head>
  <title>Temperature Monitor</title>

  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase.js"></script>

  <style>
    body {
      font-family: Arial;
      text-align: center;
      background: #f4f4f4;
      margin-top: 50px;
    }

    .box {
      background: white;
      padding: 30px;
      margin: auto;
      width: 300px;
      border-radius: 15px;
      box-shadow: 0 0 10px gray;
    }

    .temp {
      font-size: 40px;
    }

    .status {
      margin-top: 15px;
      padding: 10px;
      border-radius: 10px;
    }
  </style>

</head>

<body>

<h1>🌡 Temperature Monitor</h1>

<div class="box">
  <div id="current" class="temp">--</div>
  <div id="average">Average: --</div>
  <div id="status" class="status">Waiting...</div>
</div>

<script>
  // 🔥 PUT YOUR REAL FIREBASE DETAILS HERE
  var firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT.firebaseio.com",
  };

  firebase.initializeApp(firebaseConfig);
  var db = firebase.database();

  let state = "normal";

  db.ref("temp").on("value", function(snapshot) {
    var data = snapshot.val();
    if (!data) return;

    var current = data.current || 0;
    var avg = data.average || 0;
    var upper = data.upper || 101.4;
    var lower = data.lower || 95.0;

    document.getElementById("current").innerHTML = current.toFixed(1) + " °F";
    document.getElementById("average").innerHTML = "Average: " + avg.toFixed(1) + " °F";

    let status = document.getElementById("status");

    if (avg >= upper) {
      status.style.background = "#ffcccc";
      status.innerHTML = "🔥 HIGH TEMPERATURE!";
      if (state !== "high") {
        alert("High Temperature Alert!");
        state = "high";
      }
    }
    else if (avg <= lower) {
      status.style.background = "#ccccff";
      status.innerHTML = "❄ LOW TEMPERATURE!";
      if (state !== "low") {
        alert("Low Temperature Alert!");
        state = "low";
      }
    }
    else {
      status.style.background = "#ccffcc";
      status.innerHTML = "✅ NORMAL";
      state = "normal";
    }
  });
</script>

</body>
</html>
