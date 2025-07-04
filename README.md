<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Scanner Barcode</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #f2f2f2;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      height: 100vh;
      padding: 20px;
    }

    h1 {
      color: #333;
      font-size: 1.5rem;
      margin-top: 20px;
    }

    #reader {
      width: 90%;
      max-width: 400px;
      margin: 30px auto;
      border: 2px solid #ddd;
      border-radius: 10px;
      overflow: hidden;
    }

    .info {
      color: #666;
      font-size: 0.95rem;
      margin-top: -10px;
    }

    footer {
      position: fixed;
      bottom: 10px;
      font-size: 0.8rem;
      color: #aaa;
    }
  </style>
</head>
<body>

  <h1>🔍 Scan Barcode Project</h1>
  <p class="info">Arahkan kamera ke barcode. Setelah berhasil, kamu akan langsung diarahkan.</p>

  <div id="reader"></div>

  <footer>© 2025 fajri2714</footer>

  <script>
    function onScanSuccess(decodedText) {
      if (!window.scanned) {
        window.scanned = true;

        // Salin hasil ke clipboard (jika diizinkan)
        navigator.clipboard.writeText(decodedText).catch((err) => {
          console.warn("Gagal menyalin ke clipboard:", err);
        });

        // Redirect ke halaman tujuan
        const targetURL = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(decodedText)}`;
        setTimeout(() => {
          window.location.href = targetURL;
        }, 500); // 0.5 detik delay
      }
    }

    function onScanError(errorMessage) {
      // Silent error
    }

    const html5QrCode = new Html5Qrcode("reader");
    html5QrCode.start(
      { facingMode: "environment" },
      {
        fps: 10,
        qrbox: { width: 250, height: 250 }
      },
      onScanSuccess,
      onScanError
    );
  </script>
</body>
</html>
