<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Scan Barcode</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f5f6fa;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
      color: #333;
    }
    h2 {
      margin-bottom: 10px;
      color: #2f80ed;
    }
    #reader {
      width: 100%;
      max-width: 400px;
      margin: 20px auto;
      border-radius: 10px;
      overflow: hidden;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    p {
      font-size: 15px;
      color: #555;
    }
    .status {
      margin-top: 10px;
      color: green;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h2>Scan Barcode</h2>
  <div id="reader"></div>
  <p>Setelah scan, hasil otomatis disalin dan kamu diarahkan ke halaman kerja.</p>
  <p class="status" id="statusText"></p>

  <script>
    const html5QrCode = new Html5Qrcode("reader");

    function onScanSuccess(decodedText) {
      if (!window.scanned) {
        window.scanned = true;
        document.getElementById("statusText").innerText = "✅ Terscan: " + decodedText;

        // Salin hasil scan
        navigator.clipboard.writeText(decodedText).then(() => {
          console.log("📋 Barcode disalin: " + decodedText);
        }).catch(err => {
          console.warn("❌ Gagal salin:", err);
        });

        // Arahkan ke tujuan
        const tujuan = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(decodedText)}`;
        setTimeout(() => {
          window.location.href = tujuan;
        }, 700);
      }
    }

    function onScanError(errorMessage) {
      // Bisa abaikan atau tampilkan jika perlu
      console.warn("Scan gagal: ", errorMessage);
    }

    // Mulai scan dengan pengaturan akurasi tinggi
    html5QrCode.start(
      { facingMode: "environment" },
      { fps: 15, qrbox: 300 },
      onScanSuccess,
      onScanError
    ).catch(err => {
      document.getElementById("statusText").innerText = "❌ Kamera tidak tersedia: " + err;
    });
  </script>
</body>
</html>
