<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Portal Comunitário - BES</title>
    
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
            font-family: 'Segoe UI', Tahoma, sans-serif;
            overflow: hidden;
        }

        /* --- TELA DE MENU (RESPONSIVA) --- */
        #menu-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            width: 100%;
            background: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), 
                        url('fundo.jpg') center/cover no-repeat; 
            color: white;
            padding: 20px;
        }

        .menu-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 20px;
            width: 100%;
            max-width: 1100px;
            margin-top: 30px;
        }

        .menu-card {
            background: white;
            color: var(--primary);
            padding: 30px;
            border-radius: 20px;
            cursor: pointer;
            transition: transform 0.2s;
            display: flex;
            flex-direction: column;
            align-items: center;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
        }

        .menu-card:active { transform: scale(0.95); }
        .menu-card span { font-size: 50px; margin-bottom: 10px; }

        /* --- TELA DO MAPA (TELA CHEIA) --- */
        #map-screen { 
            display: none; 
            height: 100vh; 
            width: 100vw;
            flex-direction: column; 
        }

        header {
            background: white;
            padding: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            z-index: 1000;
        }

        main { 
            display: flex; 
            flex: 1; 
            overflow: hidden; 
        }

        /* PC: Lado a Lado | Celular: Empilhado */
        #sidebar {
            width: 350px;
            background: white;
            padding: 20px;
            display: flex;
            flex-direction: column;
            z-index: 999;
            box-shadow: 2px 0 10px rgba(0,0,0,0.05);
        }

        #map { 
            flex: 1; 
            height: 100%; 
            width: 100%;
        }

        .btn-action {
            width: 100%;
            padding: 18px;
            border: none;
            border-radius: 12px;
            color: white;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
            margin-bottom: 20px;
        }

        #feed-lista { flex: 1; overflow-y: auto; }

        /* --- MÁGICA DA VERTICALIDADE (CELULAR) --- */
        @media (max-width: 768px) {
            main { flex-direction: column; } /* Mapa em cima, lista embaixo */
            
            #sidebar {
                width: 100%;
                height: 45%; /* Ocupa quase metade da tela na vertical */
                border-right: none;
                border-top: 2px solid #ddd;
                padding: 15px;
            }

            .menu-container {
                grid-template-columns: 1fr; /* Cards ocupam a largura toda */
            }

            h1 { font-size: 1.6rem; }
        }
    </style>
</head>
<body>

    <div id="menu-screen">
        <h1>Portal Boa Esperança do Sul</h1>
        <p>Inovação e Cidadania - JEPP 2026</p>
        <div class="menu-container">
            <div class="menu-card" style="border-bottom: 10px solid var(--animal)" onclick="abrirMapa('Animais', '#e74c3c')">
                <span>🐾</span><h3>Animais</h3><small>Achados e Perdidos</small>
            </div>
            <div class="menu-card" style="border-bottom: 10px solid var(--lixo)" onclick="abrirMapa('Descarte', '#f39c12')">
                <span>🗑️</span><h3>Descarte</h3><small>Lixo e Entulho</small>
            </div>
            <div class="menu-card" style="border-bottom: 10px solid var(--coleta)" onclick="abrirMapa('Pontos de Coleta', '#27ae60')">
                <span>♻️</span><h3>Coleta</h3><small>Reciclagem Seletiva</small>
            </div>
        </div>
    </div>

    <div id="map-screen">
        <header>
            <button style="padding: 8px 15px; border-radius: 5px; border: 1px solid #ddd; cursor:pointer;" onclick="voltarMenu()">← Menu</button>
            <h3 id="map-title" style="margin:0; color: var(--primary);">Mapa</h3>
            <div style="text-align: right; line-height: 1;">
                <small>8º Ano</small><br><strong>JEPP</strong>
            </div>
        </header>
        <main>
            <section id="sidebar">
                <button id="main-action-btn" class="btn-action" onclick="ativarRegistro()">REGISTRAR AGORA</button>
                <div id="feed-lista">
                    <p style="text-align: center; color: #999; margin-top: 20px;">Nenhum registro visualizado.</p>
                </div>
            </section>
            <div id="map"></div>
        </main>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        let currentCategory = "";
        let currentColor = "";
        let map;

        function abrirMapa(categoria, cor) {
            currentCategory = categoria;
            currentColor = cor;
            document.getElementById('menu-screen').style.display = 'none';
            document.getElementById('map-screen').style.display = 'flex';
            document.getElementById('map-title').innerText = categoria;
            document.getElementById('main-action-btn').style.backgroundColor = cor;

            if (!map) {
                map = L.map('map', { zoomControl: false }).setView([-21.9932, -48.3906], 15);
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
                L.control.zoom({ position: 'topright' }).addTo(map);
            }
            // Força o mapa a preencher a tela cheia ao abrir
            setTimeout(() => { map.invalidateSize(); }, 300);
        }

        function voltarMenu() {
            document.getElementById('menu-screen').style.display = 'flex';
            document.getElementById('map-screen').style.display = 'none';
        }

        function ativarRegistro() {
            alert("Toque no local do mapa para marcar " + currentCategory);
            map.once('click', function(e) {
                const desc = prompt("Detalhes do registro:");
                if(desc) {
                    L.marker(e.latlng).addTo(map).bindPopup(`<b>${currentCategory}</b><br>${desc}`).openPopup();
                    const feed = document.getElementById('feed-lista');
                    if(feed.querySelector('p')) feed.innerHTML = "";
                    
                    const item = document.createElement('div');
                    item.style.padding = "15px";
                    item.style.marginBottom = "10px";
                    item.style.borderRadius = "8px";
                    item.style.background = "#f8f9fa";
                    item.style.borderLeft = `6px solid ${currentColor}`;
                    item.innerHTML = `<strong>Novo Registro:</strong><br>${desc}`;
                    feed.prepend(item);
                }
            });
        }
    </script>
</body>
</html>
