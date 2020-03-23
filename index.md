<!DOCTYPE html>
<html>

<head>
    <title>Instascan</title>
    <script type="text/javascript" src="instascan.min.js"></script>
</head>

<body>
    <video id="preview"></video>
    <video id="video"></video>
    <h1 id="info"></h1>
    <script type="text/javascript">
        /* let scanner = new Instascan.Scanner({ video: document.getElementById('preview') });
        console.log(scanner);

        scanner.addListener('active', async () => {

            let stream = await document.querySelector('#preview').srcObject;
            let tracks = stream.getTracks();

            tracks.forEach(function (track) {
                track.stop();
            });

        });

        scanner.addListener('scan', function (content) {
            console.log(content);
        }); */
/* 
        let myPreferredCameraDeviceId;
        navigator.mediaDevices.enumerateDevices()
            .then(function (devices) {
                let camerasBack = [];
                let camerasFront = [];
                devices.forEach(function (device) {
                    if (device.kind == 'videoinput') {
                        device.label.indexOf('back') > -1 ? camerasBack.push(device.deviceId) : camerasFront.push(device.deviceId);

                        console.log(device);
                        
                        console.log(device.kind + ": " + device.label + " id = " + device.deviceId);
                    }
                console.log(`Núm frontales: ${camerasFront.length}, Núm traseras: ${camerasBack.length}`);
                    console.log(camerasBack[0])
                    myPreferredCameraDeviceId = camerasBack[0];
                });
            })
            .catch(function (err) {
                console.log(err.name + ": " + err.message);
            });

        // Prefer camera resolution nearest to 1280x720.
        var constraints = { video: { width: 1280, height: 720 }, deviceId: {exact: myPreferredCameraDeviceId} }; 

        navigator.mediaDevices.getUserMedia(constraints)
            .then(function (mediaStream) {console.log(mediaStream)
                var video = document.querySelector('video');
                video.srcObject = mediaStream;
                video.onloadedmetadata = function (e) {
                    video.play();
                };
            })
            .catch(function (err) { console.log(err.name + ": " + err.message); }); // always check for errors at the end.
 */



        let scanner = new Instascan.Scanner({ video: document.getElementById('preview') });
        console.log(scanner);
        Instascan.Camera.getCameras().then(cameras => {

            if (cameras.length > 0) {
                let camerasBack = [];
                let camerasFront = [];
                for (let i = 0; i < cameras.length; i++) {
                    document.getElementById('info').innerHTML += `Nombre: ${cameras[i].name}<br>`;
                    console.log(cameras[i].name);
                    cameras[i].name.indexOf('back') > -1 || cameras[i].name.indexOf('trasera') > -1 ? camerasBack.push(cameras[i]) : camerasFront.push(cameras[i]);

                }
                //console.log(`Núm frontales: ${camerasFront.length}, Núm traseras: ${camerasBack.length}`);
                document.getElementById('info').innerHTML += `Núm frontales: ${camerasFront.length}, Núm traseras: ${camerasBack.length}<br>`;
                camerasBack.length > 0 ? scanner.start(camerasBack[0]) : scanner.start(camerasFront[0]);
            } else {
                console.error("Cámara NO disponible!");
                document.getElementById('info').innerHTML += `<br>Cámara NO disponible!<br>`;

            }

        }).catch(e => {
            console.log(e);
            /* if (e.message) {
                window.alert('check ' + e.message);
            } */
        })
    </script>
</body>

</html>
