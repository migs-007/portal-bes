<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portal BES - JEPP 2026</title>
    
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    
    <style>
        :root {
            --primary: #2c3e50;
            --animal: #e74c3c;
            --light: #f4f7f6;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body, html { 
            height: 100%; 
            width: 100%; 
            font-family: 'Segoe UI', sans-serif; 
            background: var(--light);
        }

        /* --- MENU INICIAL --- */
        #menu-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            width: 100%;
            position: relative;
            color: white;
            text-align: center;
            padding: 20px;
        }

        .bg-fundo {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            object-fit: cover;
            z-index: -2;
        }

        .overlay {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.6);
            z-index: -1;
        }

        .menu-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            width: 100%;
            max-width: 800px;
        }

        .menu-card {
            background: white;
            color: var(--primary);
            padding: 25px;
            border-radius: 15px;
            cursor: pointer;
        }

        /* --- TELA DO MAPA --- */
        #map-screen { 
            display: none; 
            flex-direction: column; 
            min-height: 100vh; 
        }

        header {
            background: white;
            padding: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 2000;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        /* --- ESTRUTURA PC (Lado a Lado) --- */
        main { 
            display: flex; 
            flex: 1; 
        }

        #map { 
            flex: 1; 
            height: calc(100vh - 70px); /* Altura total menos o header */
            position: sticky;
            top: 70px;
        }

        #sidebar { 
            width: 350px; 
            background: white; 
            padding: 20px; 
            overflow-y: auto;
        }

        .btn-reg {
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 10px;
            color: white;
            font-weight: bold;
            margin-bottom: 20px;
            cursor: pointer;
        }

        /* --- ESTRUTURA CELULAR (Vertical com Rolagem) --- */
        @media (max-width: 768px) {
            #map-screen { 
                overflow-y: auto; /* Permite rolar a tela toda */
                display: none; 
            }

            main { 
                flex-direction: column; 
                display: block; /* Quebra a trava do flex */
            }

            #map { 
                width: 100%; 
                height: 60vh; /* Mapa maior (60% da altura da tela) */
                position: relative;
                top: 0;
            }

            #sidebar { 
                width: 100%; 
                min-height: 40vh; 
                overflow: visible; /* Deixa o conteúdo esticar a página */
            }

            .menu-container {
                grid-template-columns: 1fr;
            }
        }

        /* Alerta Global */
        #global-alert {
            position: fixed;
            top: -100px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            background: var(--animal);
            color: white;
            padding: 15px;
            border-radius: 10px;
            z-index: 3000;
            text-align: center;
            transition: 0.5s;
        }

        #global-alert.active { top: 80px; }
    </style>
</head>
<body>

    <div id="global-alert">🚨 NOVO ALERTA ENVIADO!</div>

    <div id="menu-screen">
        <img src="fundo.jpg" alt="" class="bg-fundo">
        <div class="overlay"></div>
        <h1>Portal Comunitário BES</h1>
        <p style="margin-bottom: 30px;">Inovação JEPP 2026</p>
        <div class="menu-container">
            <div class="menu-card" onclick="abrirMapa('Animais', '#e74c3c')"><span>🐾</span><h3>Animais</h3></div>
            <div class="menu-card" onclick="abrirMapa('Descarte', '#f39c12')"><span>🗑️</span><h3>Descarte</h3></div>
        </div>
    </div>

    <div id="map-screen">
        <header>
            <button onclick="location.reload()" style="padding: 8px 15px; cursor: pointer;">← Sair</button>
            <h3 id="map-title">Mapa</h3>
            <small>8º Ano</small>
        </header>
        <main>
            <div id="map"></div>
            <section id="sidebar">
                <button id="btn-reg" class="btn-reg" onclick="registrar()">REGISTRAR OCORRÊNCIA</button>
                <div id="lista-feed">
                    <p style="color: #999; text-align: center;">Role para ver os registros recentes.</p>
                </div>
            </section>
        </main>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        let map;

        function abrirMapa(cat, cor) {
            document.getElementById('menu-screen').style.display = 'none';
            document.getElementById('map-screen').style.display = 'flex';
            document.getElementById('map-title').innerText = cat;
            document.getElementById('btn-reg').style.backgroundColor = cor;

            if (!map) {
                map = L.map('map').setView([-21.9932, -48.3906], 15);
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
            }
            setTimeout(() => { map.invalidateSize(); }, 300);
        }

        function registrar() {
            const animal = prompt("O que você deseja alertar?");
            if (animal) {
                // Simulação de Notificação Global
                const alerta = document.getElementById('global-alert');
                alerta.innerText = `🚨 ALERTA GERAL: ${animal.toUpperCase()}!`;
                alerta.classList.add('active');
                setTimeout(() => alerta.classList.remove('active'), 4000);

                // Marcador no centro do mapa atual
                L.marker(map.getCenter()).addTo(map).bindPopup(animal).openPopup();

                // Adiciona ao feed (embaixo do mapa no celular)
                const item = document.createElement('div');
                item.style.cssText = "padding: 15px; background: white; border-radius: 10px; margin-bottom: 10px; border-left: 5px solid red; box-shadow: 0 2px 5px rgba(0,0,0,0.05)";
                item.innerHTML = `<strong>Ocorrência:</strong> ${animal}<br><small>Agora mesmo</small>`;
                document.getElementById('lista-feed').prepend(item);
                
                // No celular, rola automaticamente para o feed após registrar
                if(window.innerWidth < 768) {
                    document.getElementById('sidebar').scrollIntoView({ behavior: 'smooth' });
                }
            }
        }
    </script>
</body>
</html>
