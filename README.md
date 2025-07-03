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
    #start-scan {
      padding: 10px 20px;
      font-size: 16px;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <h2>Scan Barcode Project</h2>
  <p>Tekan tombol di bawah untuk memulai scan</p>
  <button id="start-scan">Mulai Scan</button>
  <div id="reader"></div>
  <p>Setelah scan, barcode otomatis disalin dan diarahkan ke alat...</p>

  <script>
    document.getElementById("start-scan").addEventListener("click", function () {
      document.getElementById("reader").style.display = "block";

      const html5QrCode = new Html5Qrcode("reader");

      function onScanSuccess(decodedText) {
        if (!window.scanned) {
          window.scanned = true;

          // Salin hasil scan ke clipboard
          navigator.clipboard.writeText(decodedText)
            .then(() => console.log("Disalin ke clipboard:", decodedText))
            .catch(err => console.error("Gagal salin:", err));

          // Arahkan ke URL alat
          const targetURL = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(decodedText)}`;
          setTimeout(() => {
            window.location.href = targetURL;
          }, 500); // jeda 0.5 detik agar sempat disalin
        }
      }

      html5QrCode.start(
        { facingMode: "environment" },
        { fps: 10, qrbox: 250 },
        onScanSuccess,
        (err) => {} // Abaikan error scan
      );
    });
  </script>
</body>
</html>
