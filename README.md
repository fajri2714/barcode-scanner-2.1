<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Scan Barcode Project</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background-color: #f4f6f8;
      margin: 0;
      padding: 20px;
    }
    h2 {
      color: #007bff;
    }
    #reader {
      width: 100%;
      max-width: 400px;
      margin: 20px auto;
      border-radius: 12px;
      display: none;
    }
    #result-box {
      margin-top: 20px;
      display: none;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      margin-top: 15px;
      border: none;
      border-radius: 8px;
      background-color: #007bff;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    #permission-info {
      font-size: 14px;
      color: #555;
      margin-top: 10px;
    }
  </style>
</head>
<body>

  <h2>Scan Barcode Project</h2>
  <p>Tekan tombol di bawah untuk mulai scan.</p>
  <button id="start-btn">Mulai Scan</button>
  <p id="permission-info"></p>

  <div id="reader"></div>

  <div id="result-box">
    <p><strong>Hasil Scan:</strong> <span id="result-text"></span></p>
    <button id="copy-btn">Salin & Buka Link</button>
  </div>

  <script>
    let scannedText = "";

    // 1. Cek izin kamera
    if (navigator.permissions) {
      navigator.permissions.query({ name: 'camera' }).then(status => {
        console.log("Status izin kamera:", status.state);
        document.getElementById("permission-info").textContent =
          "Status izin kamera: " + status.state;

        if (status.state === "denied") {
          alert("Akses kamera ditolak. Silakan aktifkan dari pengaturan browser.");
        }
      }).catch(err => {
        console.warn("Tidak bisa membaca status izin kamera:", err);
      });
    }

    // 2. Inisialisasi saat tombol diklik
    document.getElementById("start-btn").addEventListener("click", function () {
      const readerDiv = document.getElementById("reader");
      readerDiv.style.display = "block";
      this.style.display = "none";

      const html5QrCode = new Html5Qrcode("reader");

      html5QrCode.start(
        { facingMode: "environment" }, // Lebih aman dan kompatibel
        {
          fps: 15,
          qrbox: { width: 250, height: 250 },
          aspectRatio: 1.0
        },
        (decodedText) => {
          if (!scannedText) {
            scannedText = decodedText;

            document.getElementById("result-text").textContent = scannedText;
            document.getElementById("result-box").style.display = "block";

            html5QrCode.stop()
              .then(() => console.log("Scanner dihentikan."))
              .catch(err => console.error("Gagal stop scanner:", err));
          }
        },
        (errorMessage) => {
          // console.warn("Scan gagal:", errorMessage); // opsional log
        }
      ).catch((err) => {
        alert("❌ Gagal mengakses kamera. Coba browser lain atau periksa izin kamera.");
        console.error("Camera error:", err);
      });
    });

    // 3. Salin hasil dan redirect
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
