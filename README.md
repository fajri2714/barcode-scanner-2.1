<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Scan Barcode Project</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      padding: 20px;
    }
    #reader {
      width: 300px;
      margin: 20px auto;
      display: none;
    }
    #result-box {
      margin-top: 20px;
      display: none;
    }
    button, select {
      padding: 10px 20px;
      font-size: 16px;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h2>Scan Barcode Project</h2>

  <label for="stage">Pilih Tahapan Produksi:</label>
  <br>
  <select id="stage">
    <option value="Printing">Printing</option>
    <option value="Positioning">Positioning</option>
    <option value="Quality Control">Quality Control</option>
    <option value="Heat Press">Heat Press</option>
    <option value="Finish">Finish</option>
  </select>

  <br><br>
  <button id="start-btn">Mulai Scan</button>
  <div id="reader"></div>

  <div id="result-box">
    <p><strong>Hasil Scan:</strong> <span id="result-text"></span></p>
    <button id="copy-btn">Salin & Buka Tujuan</button>
  </div>

  <script>
    let scannedText = "";

    document.getElementById("start-btn").addEventListener("click", () => {
      document.getElementById("reader").style.display = "block";
      document.getElementById("start-btn").style.display = "none";

      const html5QrCode = new Html5Qrcode("reader");
      html5QrCode.start(
        { facingMode: "environment" },
        { fps: 10, qrbox: 250 },
        (decodedText) => {
          if (!scannedText) {
            scannedText = decodedText;
            document.getElementById("result-text").textContent = scannedText;
            document.getElementById("result-box").style.display = "block";
            html5QrCode.stop();
          }
        },
        (errorMessage) => {
          // ignore error
        }
      );
    });

    document.getElementById("copy-btn").addEventListener("click", () => {
      const stage = document.getElementById("stage").value;
      if (scannedText) {
        navigator.clipboard.writeText(scannedText).catch(() => {
          // gagal salin? lanjut saja ke redirect
        });

        const targetURL = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(scannedText)}&stage=${encodeURIComponent(stage)}`;
        window.location.href = targetURL;
      }
    });
  </script>
</body>
</html>
