<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Scan Barcode Project</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <script src="https://cdn.jsdelivr.net/npm/jsqr/dist/jsQR.js"></script>
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
      margin: 10px 5px;
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
    }
    canvas, video {
      display: none;
    }
  </style>
</head>
<body>

  <h2>Scan Barcode Project</h2>
  <p>Tekan tombol di bawah untuk mulai scan.</p>
  <button id="start-btn">Mulai Scan Langsung</button>
  <button id="photo-btn">📸 Ambil Foto</button>
  <p id="permission-info"></p>

  <div id="reader"></div>
  <video id="camera" autoplay playsinline></video>
  <canvas id="canvas"></canvas>

  <div id="result-box">
    <p><strong>Hasil Scan:</strong> <span id="result-text"></span></p>
    <button id="copy-btn">Salin & Buka Link</button>
  </div>

  <script>
    let scannedText = "";
    let stream;

    // Cek izin kamera
    if (navigator.permissions) {
      navigator.permissions.query({ name: 'camera' }).then(status => {
        document.getElementById("permission-info").textContent =
          "Status izin kamera: " + status.state;
      }).catch(err => {
        console.warn("Tidak bisa membaca status izin kamera:", err);
      });
    }

    // Scan Live Mode
    document.getElementById("start-btn").addEventListener("click", () => {
      const readerDiv = document.getElementById("reader");
      readerDiv.style.display = "block";
      document.getElementById("start-btn").style.display = "none";

      const html5QrCode = new Html5Qrcode("reader");

      html5QrCode.start(
        { facingMode: "environment" },
        { fps: 15, qrbox: { width: 250, height: 250 }, aspectRatio: 1.0 },
        (decodedText) => {
          if (!scannedText) {
            scannedText = decodedText;
            document.getElementById("result-text").textContent = scannedText;
            document.getElementById("result-box").style.display = "block";
            html5QrCode.stop();
          }
        },
        (errorMessage) => {
          // Optional: console.warn("Scan error", errorMessage);
        }
      ).catch(err => {
        alert("Gagal membuka kamera: " + err);
      });
    });

    // Tombol Foto → proses QR dari gambar frame
    document.getElementById("photo-btn").addEventListener("click", async () => {
      const video = document.getElementById("camera");
      const canvas = document.getElementById("canvas");
      const ctx = canvas.getContext("2d");

      try {
        if (!stream) {
          stream = await navigator.mediaDevices.getUserMedia({
            video: { facingMode: "environment" }
          });
          video.srcObject = stream;
          await new Promise(res => video.onloadedmetadata = res);
        }

        canvas.width = video.videoWidth;
        canvas.height = video.videoHeight;
        ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

        const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
        const code = jsQR(imageData.data, canvas.width, canvas.height);

        if (code) {
          scannedText = code.data;
          document.getElementById("result-text").textContent = scannedText;
          document.getElementById("result-box").style.display = "block";
          video.srcObject.getTracks().forEach(track => track.stop());
        } else {
          alert("Gagal menemukan QR/barcode. Coba dekatkan kamera ke barcode.");
        }
      } catch (err) {
        alert("Tidak bisa akses kamera: " + err.message);
      }
    });

    // Tombol Salin & Redirect
    document.getElementById("copy-btn").addEventListener("click", () => {
      if (scannedText) {
        navigator.clipboard.writeText(scannedText)
          .then(() => {
            const targetURL = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(scannedText)}`;
            window.location.href = targetURL;
          })
          .catch(err => alert("Gagal menyalin: " + err));
      }
    });
  </script>
</body>
</html>
