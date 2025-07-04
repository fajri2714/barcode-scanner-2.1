<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Scan Barcode Project</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #f9f9f9;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
    }

    h1 {
      margin-top: 20px;
      color: #2c3e50;
    }

    #reader {
      width: 90%;
      max-width: 400px;
      border-radius: 10px;
      overflow: hidden;
      margin-top: 20px;
    }

    #result-box {
      margin-top: 20px;
      display: none;
      text-align: center;
    }

    #barcode-text {
      font-size: 1rem;
      padding: 10px;
      background: #eee;
      border: 1px solid #ccc;
      border-radius: 5px;
      margin-bottom: 10px;
      word-break: break-all;
    }

    #copy-button {
      background: #2980b9;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      font-size: 1rem;
      cursor: pointer;
    }

    #copy-button:hover {
      background: #1c5980;
    }

    footer {
      margin-top: 30px;
      font-size: 0.8rem;
      color: #999;
    }
  </style>
</head>
<body>
  <h1>🔍 Scan Barcode Project</h1>

  <div id="reader"></div>

  <div id="result-box">
    <div id="barcode-text"></div>
    <button id="copy-button">Salin & Buka</button>
  </div>

  <footer>© 2025 fajri2714</footer>

  <script>
    let lastResult = null;
    const resultBox = document.getElementById("result-box");
    const barcodeText = document.getElementById("barcode-text");
    const copyButton = document.getElementById("copy-button");

    function onScanSuccess(decodedText) {
      if (decodedText !== lastResult) {
        lastResult = decodedText;
        barcodeText.textContent = decodedText;
        resultBox.style.display = "block";
      }
    }

    function onScanError(error) {
      // Bisa diabaikan
    }

    copyButton.addEventListener("click", () => {
      if (lastResult) {
        navigator.clipboard.writeText(lastResult).catch(() => {});
        const targetURL = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(lastResult)}`;
        window.location.href = targetURL;
      }
    });

    const html5QrCode = new Html5Qrcode("reader");
    html5QrCode.start(
      { facingMode: "environment" },
      { fps: 10, qrbox: { width: 250, height: 250 } },
      onScanSuccess,
      onScanError
    );
  </script>
</body>
</html>
