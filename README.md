<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>MY BIRTHDAY</title>

</head>

<body>

<link rel="stylesheet" href="style.css">



<div class="cake">

    <div class="plate"></div>

    <div class="layer layer-bottom"></div>

    <div class="layer layer-middle"></div>

    <div class="layer layer-top"></div>

    <div class="icing"></div>

    <div class="drip drip1"></div>

    <div class="drip drip2"></div>

    <div class="drip drip3"></div>

    <div class="candle">

        <div id="flame"></div>

    </div>

</div>

</body>

</html>



<script>

      const flame = document.getElementById('flame');



navigator.mediaDevices.getUserMedia({ audio: true })

    .then((stream) => {

        const audioContext = new AudioContext();

        const analyser = audioContext.createAnalyser();

        const microphone = audioContext.createMediaStreamSource(stream);

        

        microphone.connect(analyser);

        analyser.connect(audioContext.destination);



        analyser.fftSize = 256;

        const bufferLength = analyser.frequencyBinCount;

        const dataArray = new Uint8Array(bufferLength);



        function detectBlow() {

            analyser.getByteFrequencyData(dataArray);

            const average = dataArray.reduce((acc, val) => acc + val, 0) / bufferLength;



            // Adjust this threshold according to your environment

            const threshold = 50;



            if (average > threshold) {

                // Hide the flame

                flame.style.opacity = 0;

            } 

            //else {

                // Show the flame

              //  flame.style.opacity = 1;

            //}



            requestAnimationFrame(detectBlow);

        }



        detectBlow();

    })

    .catch((error) => {

        console.error('Error accessing microphone:', error);

    });

</script>
