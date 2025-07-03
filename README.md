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
    }
  </style>
</head>
<body>
  <h2>Scan Barcode Project</h2>
  <div id="reader"></div>
  <p>Setelah scan, hasil akan otomatis disalin dan kamu diarahkan...</p>

  <script>
    function onScanSuccess(decodedText) {
      if (!window.scanned) {
        window.scanned = true;

        // Salin hasil scan ke clipboard
        navigator.clipboard.writeText(decodedText)
          .then(() => {
            console.log("Barcode copied to clipboard:", decodedText);
          })
          .catch(err => {
            console.error("Gagal menyalin barcode ke clipboard:", err);
          });

        // Arahkan ke halaman akhir
        const targetURL = http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(decodedText)};
        setTimeout(() => {
          window.location.href = targetURL;
        }, 500); // kasih delay 0.5 detik biar sempat tersalin
      }
    }

    function onScanError(errorMessage) {
      // Tidak perlu aksi saat gagal scan
    }

    const html5QrCode = new Html5Qrcode("reader");
    html5QrCode.start(
      { facingMode: "environment" },
      { fps: 10, qrbox: 250 },
      onScanSuccess,
      onScanError
    );
  </script>
</body>
</html>
