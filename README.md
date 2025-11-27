<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Mini WorldBox HTML</title>
    <style>
        :root {
            --bg: #0b0b10;
            --panel: #14141c;
            --border: #222230;
            --text: #f5f5f5;
            --text-muted: #a0a4b0;
            --accent: #3fa7ff;
            --accent-soft: #263547;
            --danger: #ff4757;
        }

        * {
            box-sizing: border-box;
        }

        html, body {
            margin: 0;
            padding: 0;
            height: 100%;
            background: radial-gradient(circle at top, #1c1c28 0, #05050a 55%);
            color: var(--text);
            font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
        }

        body {
            display: flex;
            flex-direction: column;
        }

        header {
            padding: 8px 12px;
            background: #05050a;
            border-bottom: 1px solid var(--border);
            display: flex;
            align-items: baseline;
            justify-content: space-between;
            gap: 12px;
        }

        header h1 {
            margin: 0;
            font-size: 16px;
            font-weight: 600;
            letter-spacing: 0.04em;
            text-transform: uppercase;
        }

        header small {
            color: var(--text-muted);
            font-size: 11px;
        }

        #root {
            flex: 1;
            display: grid;
            grid-template-columns: 260px 1fr 220px;
            gap: 8px;
            padding: 8px;
            min-height: 0;
        }

        @media (max-width: 1024px) {
            #root {
                grid-template-columns: 240px 1fr;
                grid-template-rows: minmax(0, 1fr) 160px;
            }
            #right-panel {
                grid-column: 1 / span 2;
            }
        }

        .panel {
            background: linear-gradient(145deg, #14141f, #090912);
            border-radius: 8px;
            border: 1px solid var(--border);
            box-shadow:
                0 0 0 1px rgba(255,255,255,0.02),
                0 18px 45px rgba(0,0,0,0.7);
            padding: 10px;
            display: flex;
            flex-direction: column;
            gap: 8px;
            min-height: 0;
        }

        .panel h2 {
            margin: 0;
            font-size: 13px;
            font-weight: 600;
            letter-spacing: 0.08em;
            text-transform: uppercase;
            color: var(--text-muted);
        }

        .panel-section {
            border-radius: 6px;
            background: radial-gradient(circle at top left, rgba(63,167,255,0.06), transparent 55%);
            padding: 6px;
            border: 1px solid rgba(255,255,255,0.02);
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .panel-section-title {
            font-size: 11px;
            text-transform: uppercase;
            letter-spacing: 0.1em;
            color: var(--text-muted);
        }

        .btn-group {
            display: flex;
            flex-wrap: wrap;
            gap: 4px;
        }

        button.tool-btn {
            border-radius: 999px;
            border: 1px solid rgba(255,255,255,0.06);
            background: radial-gradient(circle at top, #1e1e2a 0, #101018 70%);
            color: var(--text);
            font-size: 11px;
            padding: 4px 8px;
            display: inline-flex;
            align-items: center;
            gap: 4px;
            cursor: pointer;
            transition: transform 80ms ease-out, box-shadow 80ms ease-out, border-color 120ms ease-out, background 120ms ease-out;
            white-space: nowrap;
        }

        button.tool-btn span.dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--accent);
            box-shadow: 0 0 8px rgba(63,167,255,0.8);
        }

        button.tool-btn[data-kind="danger"] span.dot {
            background: var(--danger);
            box-shadow: 0 0 8px rgba(255,71,87,0.9);
        }

        button.tool-btn[data-kind="life"] span.dot {
            background: #2ed573;
            box-shadow: 0 0 8px rgba(46,213,115,0.9);
        }

        button.tool-btn:hover {
            transform: translateY(-1px);
            border-color: rgba(255,255,255,0.14);
            box-shadow: 0 6px 18px rgba(0,0,0,0.6);
        }

        button.tool-btn.active {
            background: linear-gradient(135deg, #2a3b58, #182033);
            border-color: var(--accent);
            box-shadow: 0 0 0 1px rgba(63,167,255,0.5), 0 10px 24px rgba(0,0,0,0.7);
        }

        .slider-row {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 11px;
            color: var(--text-muted);
        }

        .slider-row input[type="range"] {
            flex: 1;
        }

        .slider-row span.value {
            min-width: 32px;
            text-align: right;
            color: var(--text);
        }

        #world-wrapper {
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
            background: radial-gradient(circle at top, #151522, #05050a 60%);
            border-radius: 10px;
            overflow: hidden;
            border: 1px solid var(--border);
        }

        #world {
            image-rendering: pixelated;
            border-radius: 8px;
            background: #000;
        }

        #world-overlay {
            position: absolute;
            left: 8px;
            top: 8px;
            font-size: 11px;
            padding: 4px 8px;
            border-radius: 999px;
            background: radial-gradient(circle at top left, rgba(0,0,0,0.85), rgba(0,0,0,0.65));
            border: 1px solid rgba(255,255,255,0.15);
            color: var(--text-muted);
            backdrop-filter: blur(8px);
        }

        #world-overlay strong {
            color: var(--accent);
        }

        #right-panel-content {
            font-size: 11px;
            color: var(--text-muted);
            display: flex;
            flex-direction: column;
            gap: 6px;
            overflow: auto;
        }

        .stats-row {
            display: flex;
            justify-content: space-between;
        }

        .chip {
            display: inline-flex;
            align-items: center;
            gap: 4px;
            padding: 2px 6px;
            border-radius: 999px;
            border: 1px solid rgba(255,255,255,0.08);
            background: radial-gradient(circle at top, rgba(63,167,255,0.12), transparent 60%);
        }

        .chip-label {
            font-size: 10px;
            color: var(--text-muted);
        }

        .chip-value {
            font-size: 11px;
            color: var(--text);
        }

        ul {
            margin: 0 0 0 16px;
            padding: 0;
        }

        li {
            margin-bottom: 2px;
        }

        code {
            font-family: "JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
            font-size: 11px;
            padding: 1px 3px;
            border-radius: 4px;
            background: rgba(0,0,0,0.6);
            border: 1px solid rgba(255,255,255,0.08);
        }
    </style>
</head>
<body>
<header>
    <div>
        <h1>Mini WorldBox HTML</h1>
        <small>Sandbox procédural minimal, prêt pour ajouts personnalisés.</small>
    </div>
    <div class="chip">
        <span class="chip-label">Ticks</span>
        <span class="chip-value" id="tickCounter">0</span>
    </div>
</header>

<div id="root">
    <aside class="panel" id="left-panel">
        <h2>Outils</h2>

        <div class="panel-section">
            <div class="panel-section-title">Pinceaux terrain</div>
            <div class="btn-group" id="terrain-tools">
                <button class="tool-btn" data-tool="land">
                    <span class="dot"></span>
                    Terre
                </button>
                <button class="tool-btn" data-tool="water">
                    <span class="dot"></span>
                    Eau
                </button>
                <button class="tool-btn" data-tool="forest">
                    <span class="dot"></span>
                    Forêt
                </button>
                <button class="tool-btn" data-tool="mountain">
                    <span class="dot"></span>
                    Montagne
                </button>
                <button class="tool-btn" data-tool="burn">
                    <span class="dot" data-kind="danger"></span>
                    Brûlé
                </button>
            </div>
        </div>

        <div class="panel-section">
            <div class="panel-section-title">Créatures</div>
            <div class="btn-group" id="life-tools">
                <button class="tool-btn" data-tool="spawn-human">
                    <span class="dot" data-kind="life"></span>
                    Humain
                </button>
                <button class="tool-btn" data-tool="spawn-orc">
                    <span class="dot" data-kind="life"></span>
                    Orc
                </button>
                <button class="tool-btn" data-tool="spawn-elf">
                    <span class="dot" data-kind="life"></span>
                    Elfe
                </button>
                <button class="tool-btn" data-tool="erase-life">
                    <span class="dot" data-kind="danger"></span>
                    Effacer vie
                </button>
            </div>
        </div>

        <div class="panel-section">
            <div class="panel-section-title">Pouvoirs divins</div>
            <div class="btn-group" id="god-tools">
                <button class="tool-btn" data-tool="smite">
                    <span class="dot" data-kind="danger"></span>
                    Foudre
                </button>
                <button class="tool-btn" data-tool="heal-land">
                    <span class="dot"></span>
                    Guérir terrain
                </button>
                <button class="tool-btn" data-tool="bless">
                    <span class="dot"></span>
                    Bénédiction zone
                </button>
            </div>
        </div>

        <div class="panel-section">
            <div class="panel-section-title">Paramètres</div>
            <div class="slider-row">
                <span>Rayon pinceau</span>
                <input type="range" id="brushSize" min="1" max="10" value="3">
                <span class="value" id="brushSizeValue">3</span>
            </div>
            <div class="slider-row">
                <span>Vitesse sim</span>
                <input type="range" id="simSpeed" min="1" max="5" value="3">
                <span class="value" id="simSpeedValue">3x</span>
            </div>
            <div class="btn-group">
                <button class="tool-btn" id="regenWorld">
                    <span class="dot"></span>
                    Régénérer monde
                </button>
                <button class="tool-btn" id="clearCreatures">
                    <span class="dot" data-kind="danger"></span>
                    Effacer créatures
                </button>
            </div>
        </div>
    </aside>

    <main class="panel" id="center-panel">
        <div id="world-wrapper">
            <canvas id="world" width="1920" height="1080"></canvas>
            <div id="world-overlay">
                <strong>Outil:</strong> <span id="currentToolLabel">Terre</span> ·
                <span id="worldStats"></span>
            </div>
        </div>
    </main>

    <aside class="panel" id="right-panel">
        <h2>État du monde</h2>
        <div id="right-panel-content">
            <div class="stats-row">
                <span>Population totale</span>
                <span id="populationTotal">0</span>
            </div>
            <div class="stats-row">
                <span>Humains</span>
                <span id="populationHumans">0</span>
            </div>
            <div class="stats-row">
                <span>Orcs</span>
                <span id="populationOrcs">0</span>
            </div>
            <div class="stats-row">
                <span>Elfes</span>
                <span id="populationElves">0</span>
            </div>

            <hr style="border-color: rgba(255,255,255,0.06); border-style: solid;">

            <div>
                <strong>Biomes (tiles)</strong>
                <ul>
                    <li>Terre / prairie : support vie, croissance forêt.</li>
                    <li>Forêt : nourrit créatures, se propage avec humidité.</li>
                    <li>Eau : bloque marche, refroidit zones brûlées.</li>
                    <li>Montagne : obstacle, plus de minerais (logique à ajouter).</li>
                    <li>Brûlé : terrain mort, peut guérir avec <code>Guérir terrain</code>.</li>
                </ul>
            </div>

            <div>
                <strong>Pouvoirs personnalisés (différents du jeu original)</strong>
                <ul>
                    <li><code>Bénédiction zone</code> : booste vitesse / régénération dans une zone temporaire.</li>
                    <li>Corruption implicite : surpopulation entraîne mort lente (voir logique créatures).</li>
                    <li>Terrain auto-régénérant : brûlé → terre si conditions remplies.</li>
                </ul>
            </div>

            <div>
                <strong>Hooks pour ajouts futurs</strong>
                <ul>
                    <li>Fonction <code>updateWorld()</code> : logique météo, saisons, pollution.</li>
                    <li>Fonction <code>updateCreatures()</code> : traits, métiers, guerres de tribus.</li>
                    <li>Tableaux <code>SPECIES</code> et <code>TILE_TYPES</code> : data-driven.</li>
                </ul>
            </div>
        </div>
    </aside>
</div>

<script type="module">
/*
    Mini WorldBox HTML
    - World grid basé tiles
    - Créatures simples (humain, orc, elfe)
    - Outils de "dieu" et pinceaux
    - Logique data-driven prête à étendre
*/

const TILE_SIZE = 6;
const WORLD_TILES_W = 1920 / TILE_SIZE;
const WORLD_TILES_H = 1080 / TILE_SIZE;

// RNG déterministe
function createRng(seed) {
    return function rng() {
        seed |= 0;
        seed = seed + 0x6D2B79F5 | 0;
        let t = Math.imul(seed ^ seed >>> 15, 1 | seed);
        t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t;
        return ((t ^ t >>> 14) >>> 0) / 4294967296;
    };
}

const TILE_TYPES = {
    WATER: 0,
    SAND: 1,
    GRASS: 2,
    FOREST: 3,
    MOUNTAIN: 4,
    BURNT: 5,
};

const TILE_COLORS = {
    [TILE_TYPES.WATER]: '#2364aa',
    [TILE_TYPES.SAND]: '#e1c16e',
    [TILE_TYPES.GRASS]: '#4caf50',
    [TILE_TYPES.FOREST]: '#2e7d32',
    [TILE_TYPES.MOUNTAIN]: '#8d6e63',
    [TILE_TYPES.BURNT]: '#3e2723',
};

const SPECIES = {
    HUMAN: 'human',
    ORC: 'orc',
    ELF: 'elf',
};

const SPECIES_CONFIG = {
    [SPECIES.HUMAN]: {
        color: '#f5f5f5',
        maxHp: 100,
        speed: 1,
        diet: 'omnivore',
    },
    [SPECIES.ORC]: {
        color: '#43a047',
        maxHp: 120,
        speed: 0.9,
        diet: 'omnivore',
    },
    [SPECIES.ELF]: {
        color: '#81d4fa',
        maxHp: 90,
        speed: 1.1,
        diet: 'herbivore',
    },
};

let rng = createRng(123456789);

// Monde
/** @type {Uint8Array} */
let tiles = new Uint8Array(WORLD_TILES_W * WORLD_TILES_H);

/** @typedef {{
 *  x: number;
 *  y: number;
 *  hp: number;
 *  species: string;
 *  age: number;
 *  hunger: number;
 *  blessedTicks: number;
 * }} Creature
 */

/** @type {Creature[]} */
let creatures = [];

// Pouvoirs temporaires (ex: zones bénies)
const blessingZones = []; // {x, y, radius, ttl}

// Canvas
const canvas = /** @type {HTMLCanvasElement} */ (document.getElementById('world'));
const ctx = canvas.getContext('2d', { alpha: false });

// UI éléments
const brushSizeInput = document.getElementById('brushSize');
const brushSizeValue = document.getElementById('brushSizeValue');
const simSpeedInput = document.getElementById('simSpeed');
const simSpeedValue = document.getElementById('simSpeedValue');
const tickCounterEl = document.getElementById('tickCounter');
const populationTotalEl = document.getElementById('populationTotal');
const populationHumansEl = document.getElementById('populationHumans');
const populationOrcsEl = document.getElementById('populationOrcs');
const populationElvesEl = document.getElementById('populationElves');
const worldStatsEl = document.getElementById('worldStats');
const currentToolLabelEl = document.getElementById('currentToolLabel');

const regenWorldBtn = document.getElementById('regenWorld');
const clearCreaturesBtn = document.getElementById('clearCreatures');

let currentTool = 'land';
let brushRadius = Number(brushSizeInput.value);
let simSpeed = Number(simSpeedInput.value);

// Génération monde
function generateWorld() {
    rng = createRng((Math.random() * 0xffffffff) >>> 0);

    const noise1 = [];
    const noise2 = [];
    for (let i = 0; i < tiles.length; i++) {
        noise1[i] = rng();
        noise2[i] = rng();
    }

    function getNoise(noiseArr, x, y, scale) {
        const xf = x * scale;
        const yf = y * scale;
        const x0 = Math.floor(xf);
        const y0 = Math.floor(yf);
        const x1 = x0 + 1;
        const y1 = y0 + 1;
        const sx = xf - x0;
        const sy = yf - y0;

        function val(xx, yy) {
            const ix = ((yy + WORLD_TILES_H) % WORLD_TILES_H) * WORLD_TILES_W +
                       ((xx + WORLD_TILES_W) % WORLD_TILES_W);
            return noiseArr[ix];
        }

        const n00 = val(x0, y0);
        const n10 = val(x1, y0);
        const n01 = val(x0, y1);
        const n11 = val(x1, y1);

        const nx0 = n00 + (n10 - n00) * sx;
        const nx1 = n01 + (n11 - n01) * sx;
        return nx0 + (nx1 - nx0) * sy;
    }

    for (let y = 0; y < WORLD_TILES_H; y++) {
        for (let x = 0; x < WORLD_TILES_W; x++) {
            const i = y * WORLD_TILES_W + x;
            const h = getNoise(noise1, x, y, 0.05) * 0.7 + getNoise(noise2, x, y, 0.15) * 0.3;

            let t;
            if (h < 0.38) t = TILE_TYPES.WATER;
            else if (h < 0.42) t = TILE_TYPES.SAND;
            else if (h < 0.70) t = TILE_TYPES.GRASS;
            else if (h < 0.85) t = TILE_TYPES.FOREST;
            else t = TILE_TYPES.MOUNTAIN;
            tiles[i] = t;
        }
    }

    creatures.length = 0;
    blessingZones.length = 0;
    tick = 0;
}

function setTile(x, y, type) {
    if (x < 0 || y < 0 || x >= WORLD_TILES_W || y >= WORLD_TILES_H) return;
    tiles[y * WORLD_TILES_W + x] = type;
}

function getTile(x, y) {
    if (x < 0 || y < 0 || x >= WORLD_TILES_W || y >= WORLD_TILES_H) return TILE_TYPES.WATER;
    return tiles[y * WORLD_TILES_W + x];
}

// Créatures
function spawnCreature(x, y, species) {
    const cfg = SPECIES_CONFIG[species];
    if (!cfg) return;
    if (!isWalkableTile(getTile(x, y))) return;

    const c = /** @type {Creature} */ ({
        x, y,
        hp: cfg.maxHp,
        species,
        age: 0,
        hunger: 0,
        blessedTicks: 0,
    });

    creatures.push(c);
}

function isWalkableTile(tileType) {
    return tileType !== TILE_TYPES.WATER && tileType !== TILE_TYPES.MOUNTAIN;
}

function distanceSq(ax, ay, bx, by) {
    const dx = ax - bx;
    const dy = ay - by;
    return dx * dx + dy * dy;
}

// Simulation
let tick = 0;

function updateBlessingZones() {
    for (let i = blessingZones.length - 1; i >= 0; i--) {
        const z = blessingZones[i];
        z.ttl -= 1;
        if (z.ttl <= 0) blessingZones.splice(i, 1);
    }
}

function isInBlessedZone(x, y) {
    for (const z of blessingZones) {
     
