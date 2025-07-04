<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Scan Barcode - Versi Fix</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background-color: #f7f9fc;
      margin: 0;
      padding: 0;
    }
    h1 {
      color: #007bff;
      margin-top: 20px;
    }
    #reader {
      width: 90%;
      max-width: 400px;
      margin: 0 auto;
      border-radius: 12px;
      overflow: hidden;
    }
    p {
      font-size: 15px;
      color: #333;
      margin: 20px;
    }
  </style>
</head>
<body>

  <h1>Scan Barcode</h1>
  <div id="reader"></div>
  <p>Setelah scan, hasil otomatis disalin dan kamu diarahkan ke halaman kerja.</p>

  <script>
    const html5QrCode = new Html5Qrcode("reader");

    function onScanSuccess(decodedText, decodedResult) {
      console.log(`Hasil: ${decodedText}`);

      // Salin ke clipboard
      navigator.clipboard.writeText(decodedText).then(() => {
        console.log("Disalin ke clipboard:", decodedText);
      });

      // Redirect ke halaman kerja
      window.location.href = "http://52.74.69.49/admin/#.login";
    }

    const config = {
      fps: 15,
      qrbox: { width: 250, height: 250 },
      aspectRatio: 1.0
    };

    html5QrCode.start(
      { facingMode: "environment" },
      config,
      onScanSuccess
    ).catch(err => {
      console.error("Gagal memulai kamera belakang:", err);
      alert("Tidak bisa mengakses kamera belakang. Coba izinkan dari pengaturan browser.");
    });
  </script>

</body>
</html>
