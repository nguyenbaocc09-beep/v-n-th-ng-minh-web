<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Điều khiển tưới cây ESP8266</title>

<!-- Firebase SDK v8 -->
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>

<style>
  body {
    font-family: Arial;
    background: #eef2f3;
    text-align: center;
  }
  .box {
    background: white;
    width: 360px;
    margin: auto;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px #aaa;
  }
  button {
    width: 120px;
    height: 40px;
    font-size: 16px;
    margin: 5px;
  }
</style>
</head>

<body>

<h2> DỰ ÁN VƯỜN THÔNG MINH </h2>

<div class="box">

  <h3> DỮ LIỆU TỪ FILEBASE </h3>
  <p>Độ ẩm đất : <b id="soil">--</b></p>
  <p>Nhiệt độ: <b id="temp">--</b> °C</p>
  <p>Độ ẩm KK: <b id="hum">--</b> %</p>

  <hr>

  <h3> BƠM (AUTO)</h3>
  <p>Trạng thái bơm: <b id="pump">--</b></p>

  <hr>

  <h3> ĐÈN </h3>
  <p>Trạng thái đèn: <b id="light">--</b></p>
  <button onclick="setLight(1)">BẬT ĐÈN</button>
  <button onclick="setLight(0)">TẮT ĐÈN</button>

</div>

<script>
  // ===== CẤU HÌNH FIREBASE =====
  var firebaseConfig = {
    databaseURL: "https://iot-9d354-default-rtdb.asia-southeast1.firebasedatabase.app"
  };

  firebase.initializeApp(firebaseConfig);
  var db = firebase.database();

  // ===== GỬI LỆNH BẬT / TẮT ĐÈN =====
  function setLight(state) {
    db.ref("Sensor/light").set(state);
  }

  // ===== ĐỌC DỮ LIỆU REALTIME =====
  db.ref("Sensor").on("value", function(snapshot) {
    var data = snapshot.val();
    if (!data) return;

    document.getElementById("soil").innerHTML = data.soil;
    document.getElementById("temp").innerHTML = data.temperature;
    document.getElementById("hum").innerHTML  = data.humidity;

    document.getElementById("pump").innerHTML =
      data.pump ? "ĐANG BẬT" : "ĐANG TẮT";

    document.getElementById("light").innerHTML =
      data.light ? "ĐANG BẬT" : "ĐANG TẮT";
  });
</script>

</body>
</html>
