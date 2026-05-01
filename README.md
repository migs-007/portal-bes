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
            --light: #ecf0f1;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body, html { height: 100%; width: 100%; font-family: 'Segoe UI', sans-serif; overflow: hidden; }

        /* --- CORREÇÃO DA IMAGEM (PROPORÇÃO CELULAR) --- */
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
            object-fit: cover; /* Faz a imagem cobrir tudo sem esticar */
            z-index: -2;
        }

        .overlay {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.65);
            z-index: -1;
        }

        .menu-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            width: 90%;
            max-width: 1000px;
        }

        .menu-card {
            background: white;
            color: var(--primary);
            padding: 25px;
            border-radius: 15px;
            cursor: pointer;
            box-shadow: 0 10px 20px rgba(0,0,0,0.4);
        }

        /* --- ALERTA DE NOTIFICAÇÃO --- */
        #global-alert {
            position: fixed;
            top: -100px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 400px;
            background: var(--animal);
            color: white;
            padding: 15px;
            border-radius: 10px;
            z-index: 2000;
            text-align: center;
            font-weight: bold;
            transition: 0.5s;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        #global-alert.active { top: 20px; }

        /* --- MAPA E SIDEBAR --- */
        #map-screen { display: none; height: 100vh; flex-direction: column; }
        
        main { display: flex; flex: 1; overflow: hidden; }

        #sidebar { width: 350px; background: white; padding: 20px; display: flex; flex-direction: column; }

        #map { flex: 1; height: 100%; width: 100%; }

        /* RESPONSIVIDADE VERTICAL (CELULAR) */
        @media (max-width: 768px) {
            main { flex-direction: column; }
            #sidebar { width: 100%; height: 40%; border-top: 3px solid #ddd; }
            h1 { font-size: 1.5rem; }
        }
    </style>
</head>
<body>

    <!-- Aviso Global -->
    <div id="global-alert">🚨 NOVO ALERTA: Animal avistado na região!</div>

    <div id="menu-screen">
        <img src="fundo.jpg" alt="Fundo" class="bg-fundo">
        <div class="overlay"></div>
        
        <h1>Portal Comunitário BES</h1>
        <p style="margin-bottom: 30px;">JEPP 2026 - 8º Ano</p>

        <div class="menu-container">
            <div class="menu-card" onclick="abrirMapa('Animais', '#e74c3c')">
                <span style="font-size: 40px;">🐾</span>
                <h3>Animais</h3>
            </div>
            <div class="menu-card" onclick="abrirMapa('Descarte', '#f39c12')">
                <span style="font-size: 40px;">🗑️</span>
                <h3>Lixo/Entulho</h3>
            </div>
        </div>
    </div>

    <div id="map-screen">
        <header style="background: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #ddd;">
            <button onclick="location.reload()" style="padding: 10px; cursor: pointer;">← Voltar</button>
            <h2 id="map-title">Mapa</h2>
            <div></div>
        </header>
        <main>
            <section id="sidebar">
                <button id="btn-reg" style="padding: 20px; border: none; border-radius: 10px; color: white; font-weight: bold; cursor: pointer; margin-bottom: 20px;" onclick="simularAlerta()">REGISTRAR OCORRÊNCIA</button>
                <div id="lista" style="overflow-y: auto; flex: 1;"></div>
            </section>
            <div id="map"></div>
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

        // FUNÇÃO DE ALERTA "GERAL"
        function simularAlerta() {
            const animal = prompt("Qual animal ou objeto você viu?");
            if (animal) {
                // Alerta Visual (Simula a chegada em outros aparelhos)
                const alertBox = document.getElementById('global-alert');
                alertBox.innerText = `🚨 AVISO: ${animal.toUpperCase()} registrado agora!`;
                alertBox.classList.add('active');
                
                // Remove o alerta após 5 segundos
                setTimeout(() => { alertBox.classList.remove('active'); }, 5000);

                // Adiciona ao mapa na posição central (exemplo)
                L.marker(map.getCenter()).addTo(map).bindPopup(`<b>Alerta:</b> ${animal}`).openPopup();
                
                const item = document.createElement('div');
                item.style.padding = "10px; border-bottom: 1px solid #eee";
                item.innerHTML = `<strong>AVISO:</strong> ${animal}`;
                document.getElementById('lista').prepend(item);
            }
        }
    </script>
</body>
</html>
