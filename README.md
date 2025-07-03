<button id="start-scan">Mulai Scan</button>
<div id="reader" style="display:none;"></div>

<script>
  document.getElementById("start-scan").addEventListener("click", function () {
    document.getElementById("reader").style.display = "block";
    
    const html5QrCode = new Html5Qrcode("reader");

    function onScanSuccess(decodedText) {
      if (!window.scanned) {
        window.scanned = true;

        navigator.clipboard.writeText(decodedText)
          .then(() => console.log("Copied:", decodedText))
          .catch(err => console.error("Copy failed:", err));

        const targetURL = `http://52.74.69.49/admin/#/admin/orderprojectscan?code=${encodeURIComponent(decodedText)}`;
        setTimeout(() => {
          window.location.href = targetURL;
        }, 500);
      }
    }

    html5QrCode.start(
      { facingMode: "environment" },
      { fps: 10, qrbox: 250 },
      onScanSuccess
    );
  });
</script>
