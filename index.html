<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Portal BES - JEPP 2026</title>
    
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    
    <style>
        :root {
            /* Cores mais escuras e profissionais */
            --primary: #1a252f;
            --animal: #c0392b; /* Vermelho escuro */
            --lixo: #d35400;   /* Laranja queimado */
            --bg-dark: #2c3e50;
            --text-light: #ecf0f1;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body, html { 
            height: 100%; 
            width: 100%; 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background: var(--primary);
            overflow: hidden; 
        }

        /* --- TELA INICIAL --- */
        #menu-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            width: 100vw;
            position: relative;
            color: white;
            text-align: center;
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
            background: rgba(0, 0, 0, 0.75); /* Escurecido para destacar o texto */
            z-index: -1;
        }

        .menu-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 25px;
            width: 90%;
            max-width: 1100px;
        }

        .menu-card {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: white;
            padding: 40px 20px;
            border-radius: 24px;
            cursor: pointer;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            transition: all 0.3s ease;
        }
        .menu-card:hover { background: rgba(255, 255, 255, 0.15); transform: translateY(-5px); }
        .menu-card:active { transform: scale(0.95); }

        /* --- ALERTA --- */
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
            border-radius: 16px;
            z-index: 5000;
            text-align: center;
            font-weight: bold;
            transition: 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
            box-shadow: 0 10px 30px rgba(0,0,0,0.6);
        }
        #global-alert.active { top: 20px; }

        /* --- TELA DO MAPA --- */
        #map-screen { display: none; height: 100vh; flex-direction: column; background: #111; }

        header {
            background: #1a1a1a; 
            height: 75px;
            padding: 0 25px; 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            border-bottom: 2px solid #333;
            color: white;
            z-index: 1000;
        }

        main { display: flex; flex: 1; overflow: hidden; }

        #sidebar { 
            width: 400px; 
            background: #1e1e1e; 
            padding: 25px; 
            display: flex; 
            flex-direction: column; 
            border-right: 1px solid #333;
            color: #ccc;
        }

        #map { flex: 1; height: 100%; width: 100%; filter: grayscale(0.2) contrast(1.1); }

        .btn-reg {
            padding: 22px; 
            border: none; 
            border-radius: 15px; 
            color: white; 
            font-weight: bold; 
            font-size: 1.1rem;
            cursor: pointer; 
            margin-bottom: 25px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        #lista { overflow-y: auto; flex: 1; }
        .alert-item {
            background: #252525;
            padding: 18px;
            border-radius: 12px;
            margin-bottom: 15px;
            border-left: 6px solid #555;
            color: #eee;
        }

        /* --- MOBILE --- */
        @media (max-width: 768px) {
            body, html { overflow: auto; }
            #map-screen { height: auto; display: none; }
            main { display: block; height: auto; }
            #map { height: 60vh; }
            #sidebar { width: 100%; height: auto; border-right: none; }
            header { position: sticky; top: 0; }
        }
    </style>
</head>
<body>

    <div id="global-alert">🚨 ALERTA ATIVO</div>

    <!-- TELA INICIAL -->
    <div id="menu-screen">
        <img src="fundo.jpg" alt="Boa Esperança" class="bg-fundo">
        <div class="overlay"></div>
        
        <h1 style="font-size: 2.8rem; margin-bottom: 10px;">Portal Comunitário BES</h1>
        <p style="margin-bottom: 50px; font-size: 1.1rem; color: #bbb; letter-spacing: 2px;">JEPP 2026 • 8º ANO</p>

        <div class="menu-container">
            <div class="menu-card" onclick="abrirMapa('SOS Animais', 'var(--animal)')">
                <span style="font-size: 60px;">🐾</span>
                <h2 style="margin: 15px 0 10px 0;">SOS Animais</h2>
                <p style="color: #aaa; font-size: 0.9rem;">Cachorros, Gatos e Outros</p>
            </div>
            <div class="menu-card" onclick="abrirMapa('Eco-Ponto', 'var(--lixo)')">
                <span style="font-size: 60px;">🗑️</span>
                <h2 style="margin: 15px 0 10px 0;">Lixo e Entulho</h2>
                <p style="color: #aaa; font-size: 0.9rem;">Pontos de Descarte Regular</p>
            </div>
        </div>
    </div>

    <!-- TELA DO MAPA -->
    <div id="map-screen">
        <header>
            <button onclick="location.reload()" style="background: transparent; border: 1px solid #444; color: white; padding: 10px 20px; border-radius: 10px; cursor: pointer; font-weight: bold;">← VOLTAR</button>
            <h2 id="map-title">MAPA</h2>
            <div style="width: 50px;"></div>
        </header>

        <main>
            <div id="map"></div>

            <section id="sidebar">
                <button id="btn-reg" class="btn-reg" onclick="lancarAlerta()">+ Registrar Ocorrência</button>
                
                <h3 style="margin-bottom: 20px; font-size: 1.1rem; color: white;">HISTÓRICO LOCAL</h3>
                <div id="lista">
                    <p style="text-align: center; opacity: 0.5; padding: 20px;">Aguardando novos registros...</p>
                </div>
            </section>
        </main>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        let map;
        let corAtual;

        function abrirMapa(titulo, cor) {
            corAtual = cor;
            document.getElementById('menu-screen').style.display = 'none';
            document.getElementById('map-screen').style.display = 'flex';
            document.getElementById('map-title').innerText = titulo.toUpperCase();
            document.getElementById('btn-reg').style.backgroundColor = cor;

            if (!map) {
                map = L.map('map').setView([-21.9932, -48.3906], 15);
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                    attribution: '© OpenStreetMap'
                }).addTo(map);
            }

            setTimeout(() => { map.invalidateSize(); }, 400);
        }

        function lancarAlerta() {
            const info = prompt("Descreva a situação detalhadamente:");
            
            if (info) {
                // Notificação Visual Estilo Mobile
                const box = document.getElementById('global-alert');
                box.style.backgroundColor = corAtual;
                box.innerText = `🚨 NOVO REGISTRO: ${info.toUpperCase()}`;
                box.classList.add('active');
                setTimeout(() => { box.classList.remove('active'); }, 5000);

                // Marcador no Mapa
                L.marker(map.getCenter()).addTo(map)
                    .bindPopup(`<b>Registro:</b><br>${info}`)
                    .openPopup();

                // Adiciona ao Feed Lateral
                const feed = document.getElementById('lista');
                if (feed.innerText.includes("Aguardando")) feed.innerHTML = "";
                
                const div = document.createElement('div');
                div.className = 'alert-item';
                div.style.borderLeftColor = corAtual;
                div.innerHTML = `<strong>${info}</strong><br><small style="color:#777;">Publicado agora por Comunidade BES</small>`;
                feed.prepend(div);

                if (window.innerWidth < 768) {
                    div.scrollIntoView({ behavior: 'smooth' });
                }
            }
        }
    </script>
</body>
</html>
