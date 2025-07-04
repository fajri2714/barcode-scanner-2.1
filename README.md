<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Barcode & QR Scanner</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background: #f9f9f9;
      margin: 0;
      padding: 20px;
    }
    #reader {
      width: 100%;
      max-width: 400px;
      margin: 20px auto;
      border-radius: 10px;
    }
    #result-box {
      display: none;
      background: #fffbe0;
      border: 2px solid #ffcc00;
      border-radius: 8px;
      padding: 15px;
      margin-top: 20px;
      max-width: 360px;
      margin-left: auto;
      margin-right: auto;
    }
    #result-text {
      font-weight: bold;
      color: #333;
    }
    button {
      margin-top: 15px;
      padding: 10px 20px;
      font-size: 16px;
      border: none;
      background-color: #007bff;
      color: white;
      border-radius: 6px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
  </style>
</head>
<body>

  <h2>Scan Barcode / QR</h2>
  <div id="reader"></div>

  <div id="result-box">
    <p>📦 Hasil Deteksi:</p>
    <p id="result-text">-</p>
    <button id="ok-btn">OK ➜ Lanjutkan</button>
  </div>

  <!-- Audio bip -->
  <audio id="beep-sound" src="https://freesound.org/data/previews/522/522327_11567197-lq.mp3" preload="auto"></audio>

  <script>
    let scannedCode = "";
    const beepSound = document.getElementById("beep-sound");
    const html5QrCode = new Html5Qrcode("reader");

    html5QrCode.start(
      { facingMode: "environment" },
      {
        fps: 10,
        qrbox: { width: 250, height: 250 },
        formatsToSupport: [
          Html5QrcodeSupportedFormats.QR_CODE,
          Html5QrcodeSupportedFormats.CODE_128,
          Html5QrcodeSupportedFormats.EAN_13,
          Html5QrcodeSupportedFormats.UPC_A
        ]
      },
      (decodedText, decodedResult) => {
        if (!scannedCode) {
          scannedCode = decodedText;
          beepSound.play(); // ✅ mainkan suara bip
          document.getElementById("result-text").textContent = decodedText;
          document.getElementById("result-box").style.display = "block";
        }
      },
      (errorMessage) => {
        // abaikan error scan
      }
    ).catch(err => {
      alert("Gagal membuka kamera: " + err);
    });

    document.getElementById("ok-btn").addEventListener("click", () => {
      if (scannedCode) {
        navigator.clipboard.writeText(scannedCode).then(() => {
          const target = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(scannedCode)}`;
          window.location.href = target;
        });
      }
    });
  </script>

</body>
</html>
