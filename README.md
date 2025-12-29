<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Efsane Yılan Oyunu</title>
    <style>
        /* Görsel Tasarım */
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #2c3e50;
            color: white;
            font-family: Arial, sans-serif;
            flex-direction: column;
        }
        canvas {
            border: 5px solid #ecf0f1;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
            background-color: #000;
        }
        .skor {
            font-size: 24px;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>

    <div class="skor">Skor: <span id="skorDeger">0</span></div>
    <canvas id="yilanOyunu" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById("yilanOyunu");
        const ctx = canvas.getContext("2d");
        const skorTablosu = document.getElementById("skorDeger");

        let kutu = 20; // Her bir kare boyutu
        let yilan = [{ x: 10 * kutu, y: 10 * kutu }];
        let yemek = {
            x: Math.floor(Math.random() * 19 + 1) * kutu,
            y: Math.floor(Math.random() * 19 + 1) * kutu
        };
        let skor = 0;
        let d;

        // Kontroller
        document.addEventListener("keydown", yonDegistir);

        function yonDegistir(event) {
            if(event.keyCode == 37 && d != "SAG") d = "SOL";
            else if(event.keyCode == 38 && d != "ASAGI") d = "YUKARI";
            else if(event.keyCode == 39 && d != "SOL") d = "SAG";
            else if(event.keyCode == 40 && d != "YUKARI") d = "ASAGI";
        }

        function carpisma(kafa, dizi) {
            for(let i = 0; i < dizi.length; i++) {
                if(kafa.x == dizi[i].x && kafa.y == dizi[i].y) return true;
            }
            return false;
        }

        function ciz() {
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            for(let i = 0; i < yilan.length; i++) {
                ctx.fillStyle = (i == 0) ? "#2ecc71" : "#27ae60";
                ctx.fillRect(yilan[i].x, yilan[i].y, kutu, kutu);
                ctx.strokeStyle = "black";
                ctx.strokeRect(yilan[i].x, yilan[i].y, kutu, kutu);
            }

            ctx.fillStyle = "#e74c3c";
            ctx.fillRect(yemek.x, yemek.y, kutu, kutu);

            let yilanX = yilan[0].x;
            let yilanY = yilan[0].y;

            if( d == "SOL") yilanX -= kutu;
            if( d == "YUKARI") yilanY -= kutu;
            if( d == "SAG") yilanX += kutu;
            if( d == "ASAGI") yilanY += kutu;

            if(yilanX == yemek.x && yilanY == yemek.y) {
                skor++;
                skorTablosu.innerHTML = skor;
                yemek = {
                    x: Math.floor(Math.random() * 19 + 1) * kutu,
                    y: Math.floor(Math.random() * 19 + 1) * kutu
                };
            } else {
                yilan.pop();
            }

            let yeniKafa = { x: yilanX, y: yilanY };

            // Oyun Bitti Kontrolü
            if(yilanX < 0 || yilanX >= canvas.width || yilanY < 0 || yilanY >= canvas.height || carpisma(yeniKafa, yilan)) {
                clearInterval(oyun);
                alert("OYUN BİTTİ! Skorun: " + skor);
                location.reload(); // Sayfayı yeniler
            }

            yilan.unshift(yeniKafa);
        }

        let oyun = setInterval(ciz, 100); // 100ms hız
    </script>
</body>
</html>
