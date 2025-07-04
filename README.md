<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Scan Barcode Project</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background-color: #f4f6f8;
      margin: 0;
      padding: 30px;
    }
    h2 {
      color: #007bff;
    }
    #reader {
      width: 90%;
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

    // Cek status izin kamera
    if (navigator.permissions) {
      navigator.permissions.query({ name: 'camera' }).then(permissionStatus => {
        console.log("Status izin kamera:", permissionStatus.state); // granted / prompt / denied

        // Jika mau, kamu bisa tampilkan pesan ke user berdasarkan status ini
        if (permissionStatus.state === 'denied') {
          alert("Akses kamera ditolak. Harap izinkan akses dari pengaturan browser.");
        }
      }).catch(err => {
        console.warn("Tidak bisa membaca status izin kamera:", err);
      });
    } else {
      console.warn("Browser ini tidak mendukung navigator.permissions");
    }

    document.getElementById("start-btn").addEventListener("click", function () {
      const readerDiv = document.getElementById("reader");
      readerDiv.style.display = "block";
      this.style.display = "none";

      const html5QrCode = new Html5Qrcode("reader");

      html5QrCode.start(
        {
          facingMode: { exact: "environment" },
          width: { ideal: 1920 },
          height: { ideal: 1080 }
        },
        {
          fps: 30,
          qrbox: 350,
          aspectRatio: 1.0
        },
        (decodedText) => {
          if (!scannedText) {
            scannedText = decodedText;

            document.getElementById("result-text").textContent = scannedText;
            document.getElementById("result-box").style.display = "block";

            html5QrCode.stop().then(() => {
              console.log("Scanner berhenti.");
            }).catch((err) => {
              console.error("Gagal stop:", err);
            });
          }
        },
        (errorMessage) => {
          // opsional log kesalahan
          // console.warn(`Scan error: ${errorMessage}`);
        }
      ).catch((err) => {
        alert("Gagal mengakses kamera belakang. Periksa izin kamera atau gunakan browser lain.");
        console.error("Camera error:", err);
      });
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
