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
      margin: auto;
      display: none;
    }
    #result-box {
      margin-top: 20px;
      display: none;
    }
    #start-btn, #copy-btn {
      padding: 10px 20px;
      font-size: 16px;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h2>Scan Barcode Project</h2>
  <p>Tekan tombol di bawah untuk mulai scan.</p>
  <button id="start-btn">Mulai Scan</button>
  <div id="reader"></div>

  <div id="result-box">
    <p><strong>Hasil Scan:</strong> <span id="result-text"></span></p>
    <button id="copy-btn">Salin & Buka Link</button>
  </div>

  <script>
    let scannedText = "";
    
    document.getElementById("start-btn").addEventListener("click", function () {
      const readerDiv = document.getElementById("reader");
      readerDiv.style.display = "block";
      this.style.display = "none";

      const html5QrCode = new Html5Qrcode("reader");

      html5QrCode.start(
        { facingMode: "environment" },
        { fps: 10, qrbox: 250 },
        (decodedText) => {
          if (!scannedText) {
            scannedText = decodedText;

            document.getElementById("result-text").textContent = scannedText;
            document.getElementById("result-box").style.display = "block";
            html5QrCode.stop(); // stop scanner
          }
        },
        (errorMessage) => {
          // do nothing
        }
      );
    });

    document.getElementById("copy-btn").addEventListener("click", () => {
      if (scannedText) {
        navigator.clipboard.writeText(scannedText)
          .then(() => {
            const targetURL = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(scannedText)}`;
            window.location.href = targetURL;
          })
          .catch(err => {
            alert("Gagal menyalin ke clipboard: " + err);
          });
      }
    });
  </script>
</body>
</html>
