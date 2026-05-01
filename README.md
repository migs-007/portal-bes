<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portal Comunitário - Boa Esperança do Sul</title>
    
    <!-- Leaflet CSS para o Mapa -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    
    <style>
        :root {
            --primary: #2c3e50;
            --animal: #e74c3c;
            --lixo: #f39c12;
            --coleta: #27ae60;
            --light: #ecf0f1;
        }

        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: var(--light);
        }

        /* TELA DE MENU INICIAL COM IMAGEM DE FUNDO */
        #menu-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            text-align: center;
            
            /* A imagem deve estar na mesma pasta com o nome file:///C:/Users/Usu%C3%A1rio/Pictures/fundo.png.png */
            background: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), 
                        url('file:///C:/Users/Usu%C3%A1rio/Pictures/fundo.png.png'); 
            
            background-size: cover;
            background-position: center;
            color: white;
            transition: 0.5s;
        }

        h1 { font-size: 2.5rem; margin-bottom: 10px; text-shadow: 2px 2px 4px rgba(0,0,0,0.5); }
        p { font-size: 1.1rem; margin-bottom: 30px; opacity: 0.9; }

        .menu-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 25px;
            max-width: 1000px;
            padding: 20px;
        }

        .menu-card {
            background: white;
            color: var(--primary);
            padding: 40px 20px;
            border-radius: 20px;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 15px 35px rgba(0,0,0,0.3);
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .menu-card:hover { 
            transform: translateY(-10px); 
            box-shadow: 0 20px 40px rgba(0,0,0,0.4);
        }

        .menu-card span { font-size: 50px; margin-bottom: 15px; }
        .menu-card h3 { margin: 10px 0; font-size: 1.3rem; }
        
        .card-pet { border-top: 10px solid var(--animal); }
        .card-lixo { border-top: 10px solid var(--lixo); }
        .card-coleta { border-top: 10px solid var(--coleta); }

        /* TELA DO MAPA */
        #map-screen { display: none; height: 100vh; flex-direction: column; }

        header {
            background: white;
            padding: 10px 25px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            z-index: 1000;
        }

        .btn-voltar {
            background: #f1f1f1;
            border: 1px solid #ddd;
            padding: 10px 18px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
        }

        main { display: flex; flex: 1; overflow: hidden; }

        #sidebar {
            width: 320px;
            background: white;
            padding: 25px;
            border-right: 1px solid #ddd;
            display: flex;
            flex-direction: column;
            z-index: 999;
        }

        #map { flex: 1; background: #ddd; }

        .btn-action {
            width: 100%;
            padding: 18px;
            border: none;
            border-radius: 10px;
            color: white;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
            margin-bottom: 25px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        #feed-lista { flex: 1; overflow-y: auto; }

        .card-item {
            background: #fdfdfd;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 12px;
            font-size: 0.9rem;
            border-left: 6px solid var(--color);
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }
    </style>
</head>
<body>

    <!-- TELA DE MENU -->
    <div id="menu-screen">
        <h1>Boa Esperança do Sul Colaborativa</h1>
        <p>Escolha uma categoria para começar</p>
        
        <div class="menu-container">
            <div class="menu-card card-pet" onclick="abrirMapa('Animais', '#e74c3c')">
                <span>🐾</span>
                <h3>Animais</h3>
                <small>Resgates e Adoção</small>
            </div>
            <div class="menu-card card-lixo" onclick="abrirMapa('Descarte', '#f39c12')">
                <span>🗑️</span>
                <h3>Lixo e Entulho</h3>
                <small>Pontos de Descarte</small>
            </div>
            <div class="menu-card card-coleta" onclick="abrirMapa('Coleta Seletiva', '#27ae60')">
                <span>♻️</span>
                <h3>Reciclagem</h3>
                <small>Pontos de Coleta</small>
            </div>
        </div>
    </div>

    <!-- TELA DO MAPA -->
    <div id="map-screen">
        <header>
            <button class="btn-voltar" onclick="voltarMenu()">← Voltar</button>
            <h2 id="map-title">Mapa</h2>
            <div style="text-align: right;">
                <strong style="color: var(--primary);">JEPP 2026</strong><br>
                <small>Boa Esperança do Sul</small>
            </div>
        </header>

        <main>
            <section id="sidebar">
                <button id="main-action-btn" class="btn-action" onclick="ativarRegistro()">REGISTRAR NOVO PONTO</button>
                <div id="feed-lista">
                    <p style="color:#bbb; text-align:center; margin-top: 20px;">Nenhum registro local.</p>
                </div>
            </section>
            <div id="map"></div>
        </main>
    </div>

    <!-- Scripts Leaflet JS -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

    <script>
        let currentCategory = "";
        let currentColor = "";
        let map;

        // Correção de ícones do Leaflet
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
            document.getElementById('feed-lista').innerHTML = ""; // Inicia limpo

            if (!map) {
                // Foco em Boa Esperança do Sul
                map = L.map('map').setView([-21.9932, -48.3906], 15);
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
            }
            
            // Corrige o tamanho do mapa ao abrir
            setTimeout(() => { map.invalidateSize(); }, 300);
        }

        function voltarMenu() {
            document.getElementById('menu-screen').style.display = 'flex';
            document.getElementById('map-screen').style.display = 'none';
        }

        function ativarRegistro() {
            alert(`Clique no mapa para marcar um ponto de: ${currentCategory}`);
            
            map.once('click', function(e) {
                const desc = prompt(`Informação sobre este ponto de ${currentCategory}:`);
                if(desc && desc.trim() !== "") {
                    // Marcador
                    L.marker(e.latlng).addTo(map).bindPopup(`<b>${currentCategory}</b><br>${desc}`).openPopup();
                    
                    // Feed
                    const feed = document.getElementById('feed-lista');
                    const item = document.createElement('div');
                    item.className = 'card-item';
                    item.style.borderLeftColor = currentColor;
                    item.innerHTML = `<strong>Registro:</strong><br>${desc}`;
                    feed.prepend(item);
                }
            });
        }
    </script>
</body>
</html>
