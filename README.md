# Giftcardwishw8wish

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pohon Natal Interaktif</title>
    <style>
        /* --- CSS --- */
        body {
            background-color: #050510;
            color: white;
            font-family: 'Arial', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }

        h1 { color: #ffd700; text-shadow: 0 0 10px #ff0000; margin-bottom: 50px; }

        /* Container Utama */
        .container { position: relative; width: 400px; height: 500px; display: flex; flex-direction: column; align-items: center; }

        /* Struktur Pohon (3 Segitiga) */
        .tree-part {
            width: 0; height: 0;
            border-left: 100px solid transparent;
            border-right: 100px solid transparent;
            border-bottom: 120px solid #1b4d3e;
            position: absolute;
        }
        .top    { top: 0; border-left-width: 60px; border-right-width: 60px; border-bottom-width: 80px; z-index: 3; }
        .mid    { top: 50px; border-left-width: 100px; border-right-width: 100px; border-bottom-width: 120px; z-index: 2; }
        .bottom { top: 130px; border-left-width: 150px; border-right-width: 150px; border-bottom-width: 180px; z-index: 1; }

        .trunk {
            width: 50px; height: 60px;
            background: #4b2e1e;
            position: absolute; top: 310px; z-index: 0;
        }

        /* Area Penempatan Sticky Notes */
        #notes-area {
            position: absolute;
            top: 40px;
            width: 280px;
            height: 300px;
            z-index: 10;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 10px;
            pointer-events: none;
        }

        .sticky-note {
            background: #ffeb3b;
            color: #333;
            padding: 8px;
            width: 70px;
            min-height: 50px;
            font-size: 11px;
            box-shadow: 3px 3px 7px rgba(0,0,0,0.4);
            transform: rotate(var(--r));
            pointer-events: auto;
            border-bottom-right-radius: 10px;
            overflow: hidden;
            animation: fadeIn 0.5s ease;
        }

        /* Dekorasi Lampu */
        .light {
            position: absolute; width: 8px; height: 8px; border-radius: 50%;
            animation: blink 1s infinite alternate; z-index: 5;
        }

        @keyframes blink { from { opacity: 0.3; } to { opacity: 1; box-shadow: 0 0 10px #fff; } }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.5); } to { opacity: 1; transform: scale(1); } }

        /* Panel Kontrol */
        .controls {
            margin-top: 20px;
            background: rgba(255,255,255,0.1);
            padding: 20px;
            border-radius: 15px;
            backdrop-filter: blur(5px);
            text-align: center;
        }

        input { padding: 10px; border-radius: 5px; border: none; width: 200px; }
        button { padding: 10px 15px; border-radius: 5px; border: none; cursor: pointer; font-weight: bold; }
        .btn-add { background: #2ecc71; color: white; }
        .btn-clear { background: #e74c3c; color: white; margin-top: 10px; }
    </style>
</head>
<body>

    <h1>🎄 Digital Wish Tree 🎄</h1>

    <div class="container">
        <div class="tree-part top"></div>
        <div class="tree-part mid"></div>
        <div class="tree-part bottom"></div>
        <div class="trunk"></div>

        <div class="light" style="top: 20px; left: 195px; background: red;"></div>
        <div class="light" style="top: 80px; left: 150px; background: yellow; animation-delay: 0.2s"></div>
        <div class="light" style="top: 150px; left: 250px; background: cyan; animation-delay: 0.4s"></div>
        <div class="light" style="top: 250px; left: 120px; background: magenta; animation-delay: 0.6s"></div>

        <div id="notes-area"></div>
    </div>

    <div class="controls">
        <input type="text" id="wishInput" placeholder="Tulis harapanmu..." maxlength="40">
        <button class="btn-add" onclick="saveWish()">Tempel!</button>
        <br>
        <button class="btn-clear" onclick="clearAll()">Hapus Semua Kenangan</button>
    </div>

    <script>
        /* --- JAVASCRIPT --- */
        
        // Menampilkan pesan saat pertama kali dibuka
        document.addEventListener('DOMContentLoaded', loadWishes);

        function saveWish() {
            const input = document.getElementById('wishInput');
            const wishText = input.value.trim();

            if (wishText === "") return;

            // Ambil data yang ada di LocalStorage
            let wishes = JSON.parse(localStorage.getItem('myChristmasWishes')) || [];
            
            // Tambahkan data baru
            wishes.push({
                text: wishText,
                rotation: (Math.random() * 20 - 10) + 'deg' // Rotasi acak agar natural
            });

            // Simpan kembali ke LocalStorage
            localStorage.setItem('myChristmasWishes', JSON.stringify(wishes));

            input.value = "";
            loadWishes();
        }

        function loadWishes() {
            const area = document.getElementById('notes-area');
            const wishes = JSON.parse(localStorage.getItem('myChristmasWishes')) || [];
            
            area.innerHTML = "";

            wishes.forEach(wish => {
                const note = document.createElement('div');
                note.className = 'sticky-note';
                note.style.setProperty('--r', wish.rotation);
                note.innerText = wish.text;
                area.appendChild(note);
            });
        }

        function clearAll() {
            if(confirm("Hapus semua catatan di pohon?")) {
                localStorage.removeItem('myChristmasWishes');
                loadWishes();
            }
        }
    </script>
</body>
</html>
