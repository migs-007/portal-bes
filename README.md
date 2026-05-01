<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Portal BES - JEPP 2026</title>
    
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    
    <style>
        :root {
            --primary: #2c3e50;
            --animal: #e74c3c;
            --light: #f4f7f6;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        /* Ajuste base para não bugar o PC */
        html, body { 
            height: 100%; 
            width: 100%; 
            font-family: 'Segoe UI', sans-serif;
            background: var(--light);
            overflow: hidden; /* Trava no PC, liberamos no Celular via Media Query */
        }

        /* --- MENU INICIAL --- */
        #menu-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            width: 100%;
            position: relative;
            color: white;
            padding: 20px;
            z-index: 5000;
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
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            width: 100%;
            max-width: 1000px;
        }

        .menu-card {
            background: white;
            color: var(--primary);
            padding: 30px;
            border-radius: 20px;
            cursor: pointer;
            text-align: center;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        /* --- TELA DO MAPA --- */
        #map-screen { 
            display: none; 
            flex-direction: column; 
            height: 100vh; 
            width: 100%;
        }

        header {
            height: 70px;
            background: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 20px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            z-index: 1000;
        }

        /* Estrutura Principal */
        main { 
            display: flex; 
            flex: 1; 
            height: calc(100vh - 70px);
        }

        #map { 
            flex: 1; 
            height: 100%; 
        }

        #sidebar { 
            width: 380px; 
            background: white; 
            padding: 20px; 
            display: flex;
            flex-direction: column;
            border-left: 1px solid #ddd;
        }

        .btn-reg {
            width: 100%;
            padding: 20px;
            border: none;
            border-radius: 12px;
            color: white;
            font-weight: bold;
            font-size: 1.1rem;
            margin-bottom: 20px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        #lista-feed {
            flex: 1;
            overflow-y: auto;
        }

        /* --- VERSÃO CELULAR (MOBILE DEFINITIVO) --- */
        @media (max-width: 768px) {
            html, body { overflow: auto; } /* Libera a rolagem do dedo */
            
            #map-screen { height: auto; display: none; }
            
            main { 
                flex-direction: column; 
                height: auto; 
                display: block; 
            }

            #map { 
                width: 100%; 
                height: 70vh; /* Mapa ocupa 70% da altura da tela inicial */
                position: relative;
            }

            #sidebar { 
                width: 100%; 
                height: auto; 
                padding: 20px;
                border-left: none;
            }

            .menu-container { grid-template-columns: 1fr; }
        }

        /* Notificação */
        #global-alert {
            position: fixed;
            top: -100px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 500px;
            background: var(--animal);
            color: white;
            padding: 20px;
            border-radius: 15px;
            z-index: 9999;
            text-align: center;
            font-weight: bold;
            transition: 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        #global-alert.active { top: 20px; }
    </style>
</head>
<body>

    <div id="global-alert">🚨 ALERTA GERAL: NOVO REGISTRO!</div>

    <!-- MENU -->
    <div id="menu-screen">
        <img src="fundo.jpg" class="bg-fundo" alt="Boa Esperança do Sul">
        <div class="overlay"></div>
        <h1 style="font-size: 2.5rem; margin-bottom: 10px;">Portal Comunitário</h1>
        <p style="font-size: 1.2rem; margin-bottom: 40px; opacity: 0.9;">JEPP 2026 - Boa Esperança do Sul</p>
        
        <div class="menu-container">
            <div class="menu-card" onclick="abrirMapa('Animais', '#e74c3c')">
                <span style="font-size: 3rem;">🐾</span>
                <h3 style="margin-top:10px">SOS Animais</h3>
            </div>
            <div class="menu-card" onclick="abrirMapa('Descarte', '#f39c12')">
                <span style="font-size: 3rem;">🗑️</span>
                <h3 style="margin-top:10px">Lixo / Entulho</h3>
            </div>
        </div>
    </div>

    <!-- MAPA -->
    <div id="map-screen">
        <header>
            <button onclick="location.reload()" style="padding: 10px 20px; border-radius: 8px; border: 1px solid #ddd; cursor:pointer; font-weight:bold;">← SAIR</button>
            <h2 id="map-title" style="color: var(--primary);">Mapa</h2>
            <div style="text-align: right; line-height: 1;">
                <strong style="color: var(--animal);">LIVE</strong><br><small>Sincronizado</small>
            </div>
        </header>
        
        <main>
            <div id="map"></div>
            
            <section id="sidebar">
                <button id="btn-reg" class="btn-reg" onclick="lancarAlerta()">REGISTRAR OCORRÊNCIA</button>
                <div id="lista-feed">
                    <p style="text-align:center; color:#999; padding: 20px;">Role para ver o histórico de alertas ↓</p>
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
                // Foco em Boa Esperança do Sul
                map = L.map('map').setView([-21.9932, -48.3906], 15);
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
            }
            
            setTimeout(() => { map.invalidateSize(); }, 400);
        }

        function lancarAlerta() {
            const msg = prompt("O que você deseja alertar a todos os usuários?");
            if (msg) {
                // Efeito de Notificação "Push" (Aparece em cima)
                const alerta = document.getElementById('global-alert');
                alerta.innerText = `🚨 ALERTA GERAL: ${msg.toUpperCase()}!`;
                alerta.classList.add('active');
                
                // Remove o alerta após 5 segundos
                setTimeout(() => { alerta.classList.remove('active'); }, 5000);

                // Coloca o marcador no mapa
                L.marker(map.getCenter()).addTo(map)
                    .bindPopup(`<b>ALERTA:</b><br>${msg}`)
                    .openPopup();

                // Adiciona ao feed de notícias
                const feed = document.getElementById('lista-feed');
                const box = document.createElement('div');
                box.style.cssText = `
                    padding: 15px; 
                    background: #fff; 
                    border-radius: 10px; 
                    margin-bottom: 15px; 
                    border-left: 8px solid ${document.getElementById('btn-reg').style.backgroundColor};
                    box-shadow: 0 4px 10px rgba(0,0,0,0.05);
                `;
                box.innerHTML = `<strong>${msg}</strong><br><small>Postado agora por um morador</small>`;
                feed.prepend(box);

                // Se for celular, rola a tela para mostrar o feed novo
                if(window.innerWidth < 768) {
                    box.scrollIntoView({ behavior: 'smooth' });
                }
            }
        }
    </script>
</body>
</html>
