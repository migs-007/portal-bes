<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Portal Comunitário - BES</title>
    
    <!-- Leaflet CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    
    <style>
        :root {
            --primary: #2c3e50;
            --animal: #e74c3c;
            --lixo: #f39c12;
            --coleta: #27ae60;
            --light: #ecf0f1;
        }

        * { box-sizing: border-box; }

        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            width: 100%;
            font-family: 'Segoe UI', sans-serif;
            background: var(--light);
            overflow: hidden; /* Evita rolagem dupla no celular */
        }

        /* --- TELA DE MENU --- */
        #menu-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            text-align: center;
            background: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), 
                        url('fundo.jpg'); 
            background-size: cover;
            background-position: center;
            color: white;
            padding: 20px;
        }

        .menu-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            width: 100%;
            max-width: 900px;
        }

        .menu-card {
            background: white;
            color: var(--primary);
            padding: 20px;
            border-radius: 15px;
            cursor: pointer;
            transition: 0.3s;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .menu-card span { font-size: 40px; }

        /* --- TELA DO MAPA --- */
        #map-screen { display: none; height: 100vh; flex-direction: column; }

        header {
            background: white;
            padding: 10px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            z-index: 1000;
        }

        main { display: flex; flex: 1; flex-direction: row; }

        #sidebar {
            width: 300px;
            background: white;
            padding: 15px;
            border-right: 1px solid #ddd;
            display: flex;
            flex-direction: column;
            z-index: 999;
        }

        #map { flex: 1; z-index: 1; width: 100%; height: 100%; }

        .btn-action {
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 8px;
            color: white;
            font-weight: bold;
            cursor: pointer;
            margin-bottom: 15px;
        }

        #feed-lista { flex: 1; overflow-y: auto; }

        /* --- RESPONSIVIDADE (CELULAR) --- */
        @media (max-width: 768px) {
            main { flex-direction: column; } /* Barra lateral vai para baixo */
            
            #sidebar {
                width: 100%;
                height: 40%; /* Divide a tela com o mapa */
                border-right: none;
                border-top: 1px solid #ddd;
            }

            h1 { font-size: 1.5rem; }
            .menu-container { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>

    <!-- MENU -->
    <div id="menu-screen">
        <h1>Portal Boa Esperança do Sul</h1>
        <p>Selecione uma categoria:</p>
        <div class="menu-container">
            <div class="menu-card" style="border-top: 8px solid var(--animal)" onclick="abrirMapa('Animais', '#e74c3c')">
                <span>🐾</span><h3>Animais</h3>
            </div>
            <div class="menu-card" style="border-top: 8px solid var(--lixo)" onclick="abrirMapa('Descarte', '#f39c12')">
                <span>🗑️</span><h3>Lixo</h3>
            </div>
            <div class="menu-card" style="border-top: 8px solid var(--coleta)" onclick="abrirMapa('Reciclagem', '#27ae60')">
                <span>♻️</span><h3>Coleta</h3>
            </div>
        </div>
    </div>

    <!-- MAPA -->
    <div id="map-screen">
        <header>
            <button onclick="voltarMenu()">← Voltar</button>
            <h3 id="map-title" style="margin:0">Mapa</h3>
            <small>JEPP 2026</small>
        </header>
        <main>
            <section id="sidebar">
                <button id="main-action-btn" class="btn-action" onclick="ativarRegistro()">REGISTRAR PONTO</button>
                <div id="feed-lista"></div>
            </section>
            <div id="map"></div>
        </main>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        let currentCategory = "";
        let currentColor = "";
        let map;

        // Corrige ícones que podem dar erro 404
        delete L.Icon.Default.prototype._getIconUrl;
        L.Icon.Default.mergeOptions({
            iconRetinaUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-icon-2x.png',
            iconUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-icon.png',
            shadowUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-shadow.png',
        });

        function abrirMapa(categoria, cor) {
            currentCategory = categoria;
            currentColor = cor;
            document.getElementById('menu-screen').style.display = 'none';
            document.getElementById('map-screen').style.display = 'flex';
            document.getElementById('map-title').innerText = categoria;
            document.getElementById('main-action-btn').style.backgroundColor = cor;

            if (!map) {
                map = L.map('map').setView([-21.9932, -48.3906], 15);
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
            }
            setTimeout(() => { map.invalidateSize(); }, 400);
        }

        function voltarMenu() {
            document.getElementById('menu-screen').style.display = 'flex';
            document.getElementById('map-screen').style.display = 'none';
        }

        function ativarRegistro() {
            alert("Toque no mapa para registrar!");
            map.once('click', function(e) {
                const desc = prompt("Descrição:");
                if(desc) {
                    L.marker(e.latlng).addTo(map).bindPopup(desc).openPopup();
                    const item = document.createElement('div');
                    item.style.padding = "10px";
                    item.style.borderBottom = "1px solid #eee";
                    item.innerHTML = `<strong>${currentCategory}:</strong> ${desc}`;
                    document.getElementById('feed-lista').prepend(item);
                }
            });
        }
    </script>
</body>
</html>
