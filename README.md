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
    #start-btn {
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

  <script>
    document.getElementById("start-btn").addEventListener("click", function () {
      const readerDiv = document.getElementById("reader");
      readerDiv.style.display = "block";
      this.style.display = "none"; // sembunyikan tombol

      const html5QrCode = new Html5Qrcode("reader");

      html5QrCode.start(
        { facingMode: "environment" },
        { fps: 10, qrbox: 250 },
        (decodedText) => {
          if (!window.scanned) {
            window.scanned = true;

            // Salin hasil scan
            navigator.clipboard.writeText(decodedText)
              .then(() => {
                console.log("Tersalin:", decodedText);
              })
              .catch(err => {
                alert("Gagal menyalin: " + err);
              });

            // Redirect setelah delay
            setTimeout(() => {
              const targetURL = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(decodedText)}`;
              window.location.href = targetURL;
            }, 1000);
          }
        },
        (errorMessage) => {
          // bisa tampilkan pesan error kalau mau
        }
      );
    });
  </script>
</body>
</html>
