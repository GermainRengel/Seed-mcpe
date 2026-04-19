#<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bedrock Seed Viewer | Final</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="app-container">
        <aside class="sidebar">
            <h2 class="title">Bedrock Mapper</h2>
            
            <div class="input-group">
                <label>Semilla de Mundo</label>
                <input type="number" id="seedInput" placeholder="Ej: 849203">
                <button id="generateBtn">Actualizar Mapa</button>
            </div>

            <div class="info-box">
                <div class="coord-item">X: <span id="coordX">0</span></div>
                <div class="coord-item">Z: <span id="coordZ">0</span></div>
            </div>

            <div class="controls">
                <h3>Capas Visuales</h3>
                <label><input type="checkbox" checked> Biomas</label>
                <label><input type="checkbox"> Slime Chunks</label>
                <label><input type="checkbox"> Aldeas (Próximamente)</label>
            </div>
            
            <footer class="footer">
                Prototipo v1.0
            </footer>
        </aside>

        <main class="map-area" id="mapParent">
            <canvas id="mapCanvas"></canvas>
        </main>
    </div>
    <script src="app.js"></script>
</body>
</html> Seed-mcpe
Un lugar para ver las semillas de Minecraft Bedrock
