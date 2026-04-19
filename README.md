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
<!DOCTYPE html>
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
:root {
    --bg-dark: #121212;
    --sidebar-bg: #1e1e1e;
    --accent: #4CAF50;
    --text: #e0e0e0;
}

* { box-sizing: border-box; }
body, html { margin: 0; padding: 0; height: 100%; font-family: 'Segoe UI', sans-serif; background: var(--bg-dark); color: var(--text); overflow: hidden; }

.app-container { display: flex; height: 100vh; }

.sidebar {
    width: 300px;
    background: var(--sidebar-bg);
    padding: 25px;
    border-right: 2px solid #333;
    display: flex;
    flex-direction: column;
    z-index: 10;
}

.title { color: var(--accent); margin-top: 0; font-size: 1.5rem; text-transform: uppercase; letter-spacing: 1px; }

.input-group { margin: 20px 0; }
input {
    width: 100%; padding: 12px; border-radius: 6px; border: 1px solid #444;
    background: #2a2a2a; color: white; margin-bottom: 10px; font-size: 1rem;
}

button {
    width: 100%; padding: 12px; background: var(--accent); border: none;
    color: white; font-weight: bold; border-radius: 6px; cursor: pointer; transition: 0.3s;
}
button:hover { background: #45a049; transform: translateY(-2px); }

.info-box {
    background: #000; padding: 15px; border-radius: 8px; font-family: monospace;
    margin-bottom: 20px; border-left: 4px solid var(--accent);
}
.coord-item { font-size: 1.1rem; margin: 5px 0; }

.controls label { display: block; margin: 12px 0; cursor: pointer; color: #bbb; }

.map-area { flex-grow: 1; position: relative; background: #0b0b0b; cursor: crosshair; }
canvas { width: 100%; height: 100%; }
        <main class="map-area" id="mapParent">
            <canvas id="mapCanvas"></canvas>
        </main>
    </div>
    <script src="app.js"></script>
</body>
</html>
const canvas = document.getElementById('mapCanvas');
const ctx = canvas.getContext('2d');
const seedInput = document.getElementById('seedInput');
const generateBtn = document.getElementById('generateBtn');
const coordX = document.getElementById('coordX');
const coordZ = document.getElementById('coordZ');

let view = {
    x: 0,
    y: 0,
    zoom: 1.5,
    isDragging: false,
    lastMouse: { x: 0, y: 0 }
};

function resize() {
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;
    render();
}

// Generador de color de bioma basado en la semilla
function getBiome(x, z, seed) {
    const n = Math.sin(x * 0.05 + seed) * Math.cos(z * 0.05 + seed);
    if (n > 0.4) return '#1976d2'; // Océano
    if (n > 0.1) return '#2e7d32'; // Bosque
    if (n > -0.2) return '#4caf50'; // Planicie
    return '#fbc02d'; // Desierto
}

function render() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    const seed = parseInt(seedInput.value) || 0;
    const cellSize = 20 * view.zoom;

    // Solo dibujamos los cuadros que están visibles en pantalla
    const startX = Math.floor((-view.x) / cellSize) - 5;
    const endX = Math.ceil((canvas.width - view.x) / cellSize) + 5;
    const startZ = Math.floor((-view.y) / cellSize) - 5;
    const endZ = Math.ceil((canvas.height - view.y) / cellSize) + 5;

    for (let x = startX; x < endX; x++) {
        for (let z = startZ; z < endZ; z++) {
            ctx.fillStyle = getBiome(x, z, seed);
            ctx.fillRect(x * cellSize + view.x, z * cellSize + view.y, cellSize + 0.5, cellSize + 0.5);
        }
    }
}

// Controles de movimiento
canvas.addEventListener('mousedown', e => {
    view.isDragging = true;
    view.lastMouse = { x: e.clientX, y: e.clientY };
    canvas.style.cursor = 'grabbing';
});

window.addEventListener('mouseup', () => {
    view.isDragging = false;
    canvas.style.cursor = 'crosshair';
});

window.addEventListener('mousemove', e => {
    if (view.isDragging) {
        view.x += e.clientX - view.lastMouse.x;
        view.y += e.clientY - view.lastMouse.y;
        view.lastMouse = { x: e.clientX, y: e.clientY };
        render();
    }
    
    // Calcular coordenadas de Minecraft
    const mcX = Math.floor((e.clientX - canvas.offsetLeft - view.x) / (20 * view.zoom)) * 16;
    const mcZ = Math.floor((e.clientY - canvas.offsetTop - view.y) / (20 * view.zoom)) * 16;
    coordX.innerText = mcX;
    coordZ.innerText = mcZ;
});

canvas.addEventListener('wheel', e => {
    e.preventDefault();
    const zoomFactor = e.deltaY > 0 ? 0.9 : 1.1;
    view.zoom = Math.min(Math.max(view.zoom * zoomFactor, 0.2), 10);
    render();
}, { passive: false });

generateBtn.addEventListener('click', () => {
    if(!seedInput.value) seedInput.value = Math.floor(Math.random() * 999999);
    render();
});

window.addEventListener('resize', resize);
// Iniciar aplicación
resize();
