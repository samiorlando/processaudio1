# processaudio1
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BROADCAST PROCESSOR | Optimod-Style DSP Engine</title>
<style>
:root {
--bg-primary: #0a0a0f;
--bg-secondary: #12121a;
--bg-tertiary: #1a1a25;
--bg-panel: #151520;
--border-color: #2a2a3a;
--border-glow: #3a3a5a;
--text-primary: #e0e0e8;
--text-secondary: #8888a0;
--text-dim: #555570;
--amber: #ffaa00;
--amber-glow: rgba(255, 170, 0, 0.3);
--green: #00ff66;
--green-glow: rgba(0, 255, 102, 0.3);
--green-dim: #00aa44;
--red: #ff3344;
--red-glow: rgba(255, 51, 68, 0.3);
--blue: #4488ff;
--blue-glow: rgba(68, 136, 255, 0.3);
--knob-bg: #1a1a28;
--knob-ring: #333348;
--knob-indicator: #ffaa00;
--meter-green: #00cc44;
--meter-yellow: #cccc00;
--meter-red: #cc2200;
--shadow-inner: inset 0 2px 8px rgba(0,0,0,0.5);
--shadow-outer: 0 4px 12px rgba(0,0,0,0.4);
--shadow-glow-amber: 0 0 15px rgba(255,170,0,0.2);
--shadow-glow-green: 0 0 15px rgba(0,255,102,0.2);
}

* { margin:0; padding:0; box-sizing:border-box; }

body {
background: var(--bg-primary);
color: var(--text-primary);
font-family: 'Segoe UI', 'Roboto', monospace;
overflow-x: hidden;
min-height: 100vh;
}

/* === SCROLLBAR === */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg-primary); }
::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 3px; }

/* === HEADER === */
.header {
background: linear-gradient(180deg, #1a1a28 0%, #0f0f18 100%);
border-bottom: 1px solid var(--border-color);
padding: 8px 20px;
display: flex;
align-items: center;
justify-content: space-between;
position: sticky;
top: 0;
z-index: 100;
box-shadow: 0 2px 20px rgba(0,0,0,0.5);
}

.header-brand {
display: flex;
align-items: center;
gap: 12px;
}

.header-logo {
width: 40px;
height: 40px;
background: linear-gradient(135deg, var(--amber), #ff6600);
border-radius: 8px;
display: flex;
align-items: center;
justify-content: center;
font-weight: 900;
font-size: 18px;
color: #000;
box-shadow: var(--shadow-glow-amber);
}

.header-title {
font-size: 16px;
font-weight: 700;
letter-spacing: 2px;
color: var(--text-primary);
}

.header-subtitle {
font-size: 10px;
color: var(--text-secondary);
letter-spacing: 3px;
text-transform: uppercase;
}

.header-controls {
display: flex;
align-items: center;
gap: 12px;
}

.btn {
padding: 6px 16px;
border: 1px solid var(--border-color);
background: var(--bg-tertiary);
color: var(--text-primary);
border-radius: 4px;
cursor: pointer;
font-size: 11px;
font-weight: 600;
letter-spacing: 1px;
text-transform: uppercase;
transition: all 0.2s;
}

.btn:hover {
border-color: var(--amber);
color: var(--amber);
box-shadow: var(--shadow-glow-amber);
}

.btn.active {
background: var(--amber);
color: #000;
border-color: var(--amber);
}

.btn-power {
width: 48px;
height: 48px;
border-radius: 50%;
border: 2px solid var(--border-color);
background: radial-gradient(circle at 35% 35%, #2a2a3a, #1a1a28);
color: var(--text-dim);
font-size: 20px;
cursor: pointer;
display: flex;
align-items: center;
justify-content: center;
transition: all 0.3s;
box-shadow: var(--shadow-outer);
}

.btn-power.on {
border-color: var(--green);
color: var(--green);
box-shadow: var(--shadow-glow-green), inset 0 0 20px rgba(0,255,102,0.1);
}

.btn-power:hover {
transform: scale(1.05);
}

/* === MAIN LAYOUT === */
.main-container {
display: grid;
grid-template-columns: 280px 1fr 260px;
gap: 0;
min-height: calc(100vh - 60px);
}

/* === LEFT PANEL - INPUT & EQ === */
.left-panel {
background: var(--bg-secondary);
border-right: 1px solid var(--border-color);
overflow-y: auto;
max-height: calc(100vh - 60px);
}

.panel-section {
border-bottom: 1px solid var(--border-color);
padding: 12px;
}

.panel-header {
display: flex;
align-items: center;
justify-content: space-between;
margin-bottom: 10px;
}

.panel-title {
font-size: 10px;
font-weight: 700;
letter-spacing: 2px;
text-transform: uppercase;
color: var(--amber);
}

.panel-bypass {
width: 16px;
height: 16px;
border-radius: 50%;
border: 1px solid var(--border-color);
background: var(--bg-tertiary);
cursor: pointer;
transition: all 0.2s;
}

.panel-bypass.active {
background: var(--green);
border-color: var(--green);
box-shadow: 0 0 8px rgba(0,255,102,0.3);
}

/* === 3D KNOB === */
.knob-container {
display: flex;
flex-direction: column;
align-items: center;
gap: 4px;
}

.knob {
width: 56px;
height: 56px;
border-radius: 50%;
background: conic-gradient(from 220deg, #2a2a3a, #1a1a28, #2a2a3a, #333348, #2a2a3a);
position: relative;
cursor: pointer;
box-shadow: 
0 4px 12px rgba(0,0,0,0.5),
inset 0 1px 2px rgba(255,255,255,0.05),
inset 0 -1px 2px rgba(0,0,0,0.3);
transition: box-shadow 0.2s;
}

.knob:hover {
box-shadow: 
0 4px 16px rgba(0,0,0,0.6),
0 0 20px rgba(255,170,0,0.1),
inset 0 1px 2px rgba(255,255,255,0.05),
inset 0 -1px 2px rgba(0,0,0,0.3);
}

.knob-indicator {
position: absolute;
top: 4px;
left: 50%;
transform: translateX(-50%);
width: 3px;
height: 16px;
background: var(--amber);
border-radius: 2px;
box-shadow: 0 0 6px rgba(255,170,0,0.5);
transform-origin: bottom center;
}

.knob-ring {
position: absolute;
top: -3px;
left: -3px;
right: -3px;
bottom: -3px;
border-radius: 50%;
border: 1px solid var(--knob-ring);
opacity: 0.5;
}

.knob-value {
font-size: 10px;
color: var(--text-secondary);
font-family: 'Consolas', monospace;
min-width: 50px;
text-align: center;
}

.knob-label {
font-size: 8px;
color: var(--text-dim);
text-transform: uppercase;
letter-spacing: 1px;
}

.knobs-row {
display: flex;
gap: 8px;
justify-content: center;
flex-wrap: wrap;
}

/* === EQ BANDS === */
.eq-band {
background: var(--bg-tertiary);
border: 1px solid var(--border-color);
border-radius: 6px;
padding: 8px;
margin-bottom: 6px;
}

.eq-band-header {
display: flex;
align-items: center;
justify-content: space-between;
margin-bottom: 6px;
}

.eq-band-label {
font-size: 9px;
font-weight: 700;
letter-spacing: 1px;
color: var(--text-secondary);
}

.eq-band-type {
font-size: 8px;
padding: 2px 6px;
background: var(--bg-primary);
border-radius: 3px;
color: var(--amber);
border: 1px solid var(--border-color);
}

.eq-band-controls {
display: grid;
grid-template-columns: 1fr 1fr 1fr;
gap: 6px;
}

/* === CENTER PANEL - MAIN DISPLAY === */
.center-panel {
display: flex;
flex-direction: column;
gap: 0;
background: var(--bg-primary);
}

/* === METER BRIDGE === */
.meter-bridge {
display: grid;
grid-template-columns: 1fr 1fr 1fr;
gap: 1px;
background: var(--border-color);
border-bottom: 1px solid var(--border-color);
}

.meter-panel {
background: var(--bg-secondary);
padding: 10px;
}

.meter-label {
font-size: 9px;
font-weight: 700;
letter-spacing: 2px;
text-transform: uppercase;
color: var(--text-secondary);
margin-bottom: 8px;
text-align: center;
}

/* === VU METER === */
.vu-meter {
display: flex;
gap: 3px;
height: 20px;
align-items: flex-end;
justify-content: center;
}

.vu-channel {
display: flex;
flex-direction: column;
align-items: center;
gap: 2px;
}

.vu-bar {
width: 120px;
height: 16px;
background: var(--bg-primary);
border-radius: 2px;
overflow: hidden;
border: 1px solid var(--border-color);
position: relative;
}

.vu-fill {
height: 100%;
width: 0%;
background: linear-gradient(90deg, 
var(--meter-green) 0%, 
var(--meter-green) 70%, 
var(--meter-yellow) 85%, 
var(--meter-red) 100%);
transition: width 0.05s ease-out;
border-radius: 1px;
}

.vu-fill.peak-hold {
background: var(--red);
position: absolute;
top: 0;
right: 0;
width: 3px;
height: 100%;
transition: right 0.3s;
}

.vu-channel-label {
font-size: 8px;
color: var(--text-dim);
}

/* === PPM METER === */
.ppm-meter {
display: flex;
flex-direction: column;
gap: 2px;
}

.ppm-row {
display: flex;
align-items: center;
gap: 4px;
}

.ppm-label {
font-size: 7px;
color: var(--text-dim);
width: 20px;
text-align: right;
font-family: monospace;
}

.ppm-bar-container {
flex: 1;
height: 8px;
background: var(--bg-primary);
border-radius: 1px;
overflow: hidden;
border: 1px solid var(--border-color);
}

.ppm-bar-fill {
height: 100%;
width: 0%;
transition: width 0.03s;
}

.ppm-bar-fill.l { background: var(--green); }
.ppm-bar-fill.r { background: var(--green); }

/* === LUFS METER === */
.lufs-display {
text-align: center;
padding: 8px;
}

.lufs-value {
font-size: 32px;
font-weight: 900;
font-family: 'Consolas', monospace;
color: var(--green);
text-shadow: 0 0 20px rgba(0,255,102,0.3);
}

.lufs-unit {
font-size: 10px;
color: var(--text-secondary);
margin-left: 4px;
}

.lufs-info {
display: grid;
grid-template-columns: 1fr 1fr 1fr;
gap: 8px;
margin-top: 8px;
}

.lufs-stat {
text-align: center;
}

.lufs-stat-label {
font-size: 7px;
color: var(--text-dim);
text-transform: uppercase;
letter-spacing: 1px;
}

.lufs-stat-value {
font-size: 14px;
font-weight: 700;
font-family: 'Consolas', monospace;
color: var(--text-primary);
}

/* === VISUALIZER AREA === */
.visualizer-area {
flex: 1;
display: grid;
grid-template-rows: 1fr 1fr;
gap: 1px;
background: var(--border-color);
}

.visualizer-panel {
background: var(--bg-secondary);
position: relative;
overflow: hidden;
}

.visualizer-header {
position: absolute;
top: 0;
left: 0;
right: 0;
padding: 6px 10px;
background: linear-gradient(180deg, rgba(15,15,24,0.9) 0%, transparent 100%);
display: flex;
align-items: center;
justify-content: space-between;
z-index: 2;
}

.visualizer-title {
font-size: 9px;
font-weight: 700;
letter-spacing: 2px;
text-transform: uppercase;
color: var(--amber);
}

.visualizer-canvas {
width: 100%;
height: 100%;
display: block;
}

/* === DSP CHAIN DISPLAY === */
.dsp-chain {
display: flex;
align-items: center;
gap: 4px;
padding: 8px 12px;
background: var(--bg-secondary);
border-top: 1px solid var(--border-color);
overflow-x: auto;
}

.dsp-block {
padding: 4px 10px;
background: var(--bg-tertiary);
border: 1px solid var(--border-color);
border-radius: 3px;
font-size: 8px;
font-weight: 700;
letter-spacing: 1px;
text-transform: uppercase;
color: var(--text-secondary);
white-space: nowrap;
cursor: pointer;
transition: all 0.2s;
}

.dsp-block.active {
color: var(--green);
border-color: var(--green-dim);
box-shadow: 0 0 8px rgba(0,255,102,0.1);
}

.dsp-arrow {
color: var(--text-dim);
font-size: 10px;
}

/* === RIGHT PANEL - PROCESSING === */
.right-panel {
background: var(--bg-secondary);
border-left: 1px solid var(--border-color);
overflow-y: auto;
max-height: calc(100vh - 60px);
}

/* === COMPRESSOR SECTION === */
.compressor-section {
background: var(--bg-tertiary);
border: 1px solid var(--border-color);
border-radius: 6px;
padding: 10px;
margin-bottom: 8px;
}

.comp-header {
display: flex;
align-items: center;
justify-content: space-between;
margin-bottom: 8px;
}

.comp-title {
font-size: 10px;
font-weight: 700;
letter-spacing: 1px;
color: var(--text-primary);
}

.comp-badge {
font-size: 7px;
padding: 2px 5px;
background: var(--amber);
color: #000;
border-radius: 2px;
font-weight: 700;
}

.comp-controls {
display: grid;
grid-template-columns: 1fr 1fr;
gap: 6px;
}

.comp-control {
display: flex;
flex-direction: column;
align-items: center;
gap: 2px;
}

.comp-control-label {
font-size: 8px;
color: var(--text-dim);
text-transform: uppercase;
letter-spacing: 1px;
}

.comp-control-value {
font-size: 11px;
font-weight: 700;
color: var(--text-primary);
font-family: 'Consolas', monospace;
}

/* === RTA === */
.rta-display {
position: relative;
height: 100px;
background: var(--bg-primary);
border: 1px solid var(--border-color);
border-radius: 4px;
margin-top: 8px;
overflow: hidden;
}

.rta-canvas {
width: 100%;
height: 100%;
}

/* === PRESETS === */
.preset-bar {
display: flex;
gap: 4px;
padding: 8px 12px;
background: var(--bg-secondary);
border-top: 1px solid var(--border-color);
flex-wrap: wrap;
}

.preset-btn {
padding: 4px 10px;
background: var(--bg-tertiary);
border: 1px solid var(--border-color);
border-radius: 3px;
color: var(--text-secondary);
font-size: 9px;
font-weight: 600;
cursor: pointer;
transition: all 0.2s;
letter-spacing: 0.5px;
}

.preset-btn:hover {
border-color: var(--amber);
color: var(--amber);
}

.preset-btn.active {
background: var(--amber);
color: #000;
border-color: var(--amber);
}

/* === SLIDER === */
.slider-container {
display: flex;
flex-direction: column;
align-items: center;
gap: 4px;
}

.slider {
-webkit-appearance: none;
appearance: none;
width: 80px;
height: 4px;
background: var(--bg-primary);
border-radius: 2px;
outline: none;
border: 1px solid var(--border-color);
}

.slider::-webkit-slider-thumb {
-webkit-appearance: none;
appearance: none;
width: 14px;
height: 14px;
border-radius: 50%;
background: var(--amber);
cursor: pointer;
box-shadow: 0 0 8px rgba(255,170,0,0.3);
}

.slider::-moz-range-thumb {
width: 14px;
height: 14px;
border-radius: 50%;
background: var(--amber);
cursor: pointer;
border: none;
}

/* === STATUS BAR === */
.status-bar {
display: flex;
align-items: center;
justify-content: space-between;
padding: 4px 16px;
background: var(--bg-primary);
border-top: 1px solid var(--border-color);
font-size: 9px;
color: var(--text-dim);
}

.status-item {
display: flex;
align-items: center;
gap: 4px;
}

.status-dot {
width: 6px;
height: 6px;
border-radius: 50%;
background: var(--text-dim);
}

.status-dot.active {
background: var(--green);
box-shadow: 0 0 6px rgba(0,255,102,0.5);
}

/* === INPUT STAGE === */
.input-stage {
display: flex;
gap: 10px;
align-items: center;
justify-content: center;
padding: 8px;
background: var(--bg-tertiary);
border: 1px solid var(--border-color);
border-radius: 6px;
margin-bottom: 8px;
}

.input-select {
background: var(--bg-primary);
border: 1px solid var(--border-color);
color: var(--text-primary);
padding: 4px 8px;
border-radius: 3px;
font-size: 10px;
outline: none;
}

.input-select:focus {
border-color: var(--amber);
}

/* === MULTIBAND DISPLAY === */
.multiband-display {
display: grid;
grid-template-columns: 1fr 1fr 1fr 1fr;
gap: 4px;
margin-top: 8px;
}

.multiband-channel {
background: var(--bg-primary);
border: 1px solid var(--border-color);
border-radius: 4px;
padding: 6px;
text-align: center;
}

.multiband-label {
font-size: 8px;
color: var(--text-dim);
text-transform: uppercase;
letter-spacing: 1px;
margin-bottom: 4px;
}

.multiband-meter {
width: 100%;
height: 60px;
background: var(--bg-primary);
border: 1px solid var(--border-color);
border-radius: 2px;
position: relative;
overflow: hidden;
}

.multiband-meter-fill {
position: absolute;
bottom: 0;
left: 0;
right: 0;
height: 0%;
transition: height 0.05s;
}

.multiband-meter-fill.bass { background: linear-gradient(to top, #cc2200, #ff4400); }
.multiband-meter-fill.low { background: linear-gradient(to top, #ff8800, #ffaa00); }
.multiband-meter-fill.high { background: linear-gradient(to top, #88cc00, #aadd00); }
.multiband-meter-fill.presence { background: linear-gradient(to top, #00cc44, #00ff66); }

.multiband-gr {
font-size: 10px;
font-weight: 700;
color: var(--text-primary);
font-family: monospace;
margin-top: 4px;
}

/* === TOOLTIP === */
.tooltip {
position: absolute;
background: rgba(0,0,0,0.9);
color: var(--text-primary);
padding: 4px 8px;
border-radius: 3px;
font-size: 10px;
pointer-events: none;
z-index: 1000;
border: 1px solid var(--border-color);
display: none;
}

/* === RESPONSIVE === */
@media (max-width: 1200px) {
.main-container {
grid-template-columns: 1fr;
}
.left-panel, .right-panel {
max-height: none;
}
}

@media (max-width: 768px) {
.header {
padding: 6px 12px;
flex-wrap: wrap;
gap: 8px;
}
.header-title { font-size: 13px; }
.meter-bridge {
grid-template-columns: 1fr;
}
}

/* === ANIMATIONS === */
@keyframes pulse {
0%, 100% { opacity: 1; }
50% { opacity: 0.5; }
}

@keyframes glow {
0%, 100% { box-shadow: 0 0 5px rgba(0,255,102,0.2); }
50% { box-shadow: 0 0 20px rgba(0,255,102,0.4); }
}

.recording { animation: pulse 1s infinite; }
.active-glow { animation: glow 2s infinite; }

/* === GAIN REDUCTION METER === */
.gr-meter {
width: 100%;
height: 8px;
background: var(--bg-primary);
border-radius: 4px;
overflow: hidden;
margin-top: 4px;
}

.gr-meter-fill {
height: 100%;
background: var(--amber);
transition: width 0.05s;
border-radius: 4px;
}

/* === LIMITER DISPLAY === */
.limiter-section {
background: var(--bg-tertiary);
border: 1px solid var(--border-color);
border-radius: 6px;
padding: 10px;
margin-bottom: 8px;
}

.limiter-threshold {
display: flex;
align-items: center;
gap: 8px;
}

.limiter-threshold .slider {
flex: 1;
}

/* === LOADING OVERLAY === */
.loading-overlay {
position: fixed;
top: 0; left: 0; right: 0; bottom: 0;
background: var(--bg-primary);
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
z-index: 9999;
transition: opacity 0.5s;
}

.loading-overlay.hidden {
opacity: 0;
pointer-events: none;
}

.loading-spinner {
width: 40px;
height: 40px;
border: 3px solid var(--border-color);
border-top-color: var(--amber);
border-radius: 50%;
animation: spin 1s linear infinite;
}

@keyframes spin {
to { transform: rotate(360deg); }
}

.loading-text {
margin-top: 16px;
font-size: 12px;
color: var(--text-secondary);
letter-spacing: 2px;
}
</style>
</head>
<body>

<!-- LOADING -->
<div class="loading-overlay" id="loadingOverlay">
<div class="loading-spinner"></div>
<div class="loading-text">INITIALIZING DSP ENGINE...</div>
</div>

<!-- HEADER -->
<div class="header">
<div class="header-brand">
<div class="header-logo">BP</div>
<div>
<div class="header-title">BROADCAST PROCESSOR</div>
<div class="header-subtitle">Multi-Band DSP Engine</div>
</div>
</div>
<div class="header-controls">
<select class="input-select" id="inputSelect">
<option value="">Select Input...</option>
<option value="mic">Microphone</option>
<option value="line">Line In</option>
<option value="file">Audio File</option>
</select>
<button class="btn" id="btnPreset">Presets</button>
<button class="btn" id="btnExport">Export</button>
<button class="btn" id="btnImport">Import</button>
<button class="btn-power" id="btnPower" title="Power On/Off">⏻</button>
</div>
</div>

<!-- MAIN -->
<div class="main-container">

<!-- LEFT PANEL -->
<div class="left-panel">

<!-- INPUT STAGE -->
<div class="panel-section">
<div class="panel-header">
<span class="panel-title">Input Stage</span>
<div class="panel-bypass active" data-module="input"></div>
</div>
<div class="input-stage">
<div class="knob-container">
<div class="knob" data-param="inputGain" data-min="-24" data-max="24" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.0 dB</div>
<div class="knob-label">Gain</div>
</div>
<div class="knob-container">
<div class="knob" data-param="hpfFreq" data-min="20" data-max="500" data-default="40">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">40 Hz</div>
<div class="knob-label">HPF</div>
</div>
<div class="knob-container">
<div class="knob" data-param="saturation" data-min="0" data-max="100" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0%</div>
<div class="knob-label">Sat</div>
</div>
</div>
</div>

<!-- EQUALIZER -->
<div class="panel-section">
<div class="panel-header">
<span class="panel-title">Parametric EQ</span>
<div class="panel-bypass active" data-module="eq"></div>
</div>

<div class="eq-band" data-band="1">
<div class="eq-band-header">
<span class="eq-band-label">BAND 1 - LOW</span>
<span class="eq-band-type">L-SHELF</span>
</div>
<div class="eq-band-controls">
<div class="knob-container">
<div class="knob" data-param="eq1Freq" data-min="20" data-max="500" data-default="80">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">80 Hz</div>
<div class="knob-label">Freq</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq1Gain" data-min="-15" data-max="15" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.0 dB</div>
<div class="knob-label">Gain</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq1Q" data-min="0.1" data-max="10" data-default="1.0" step="0.1">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">1.0</div>
<div class="knob-label">Q</div>
</div>
</div>
</div>

<div class="eq-band" data-band="2">
<div class="eq-band-header">
<span class="eq-band-label">BAND 2 - LOW-MID</span>
<span class="eq-band-type">PEAK</span>
</div>
<div class="eq-band-controls">
<div class="knob-container">
<div class="knob" data-param="eq2Freq" data-min="100" data-max="2000" data-default="300">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">300 Hz</div>
<div class="knob-label">Freq</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq2Gain" data-min="-15" data-max="15" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.0 dB</div>
<div class="knob-label">Gain</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq2Q" data-min="0.1" data-max="10" data-default="1.0" step="0.1">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">1.0</div>
<div class="knob-label">Q</div>
</div>
</div>
</div>

<div class="eq-band" data-band="3">
<div class="eq-band-header">
<span class="eq-band-label">BAND 3 - MID</span>
<span class="eq-band-type">PEAK</span>
</div>
<div class="eq-band-controls">
<div class="knob-container">
<div class="knob" data-param="eq3Freq" data-min="200" data-max="5000" data-default="1000">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">1.0 kHz</div>
<div class="knob-label">Freq</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq3Gain" data-min="-15" data-max="15" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.0 dB</div>
<div class="knob-label">Gain</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq3Q" data-min="0.1" data-max="10" data-default="1.0" step="0.1">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">1.0</div>
<div class="knob-label">Q</div>
</div>
</div>
</div>

<div class="eq-band" data-band="4">
<div class="eq-band-header">
<span class="eq-band-label">BAND 4 - HIGH-MID</span>
<span class="eq-band-type">PEAK</span>
</div>
<div class="eq-band-controls">
<div class="knob-container">
<div class="knob" data-param="eq4Freq" data-min="1000" data-max="10000" data-default="4000">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">4.0 kHz</div>
<div class="knob-label">Freq</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq4Gain" data-min="-15" data-max="15" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.0 dB</div>
<div class="knob-label">Gain</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq4Q" data-min="0.1" data-max="10" data-default="1.0" step="0.1">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">1.0</div>
<div class="knob-label">Q</div>
</div>
</div>
</div>

<div class="eq-band" data-band="5">
<div class="eq-band-header">
<span class="eq-band-label">BAND 5 - HIGH</span>
<span class="eq-band-type">PEAK</span>
</div>
<div class="eq-band-controls">
<div class="knob-container">
<div class="knob" data-param="eq5Freq" data-min="2000" data-max="20000" data-default="8000">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">8.0 kHz</div>
<div class="knob-label">Freq</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq5Gain" data-min="-15" data-max="15" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.0 dB</div>
<div class="knob-label">Gain</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq5Q" data-min="0.1" data-max="10" data-default="1.0" step="0.1">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">1.0</div>
<div class="knob-label">Q</div>
</div>
</div>
</div>

<div class="eq-band" data-band="6">
<div class="eq-band-header">
<span class="eq-band-label">BAND 6 - AIR</span>
<span class="eq-band-type">H-SHELF</span>
</div>
<div class="eq-band-controls">
<div class="knob-container">
<div class="knob" data-param="eq6Freq" data-min="5000" data-max="20000" data-default="12000">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">12.0 kHz</div>
<div class="knob-label">Freq</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq6Gain" data-min="-15" data-max="15" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.0 dB</div>
<div class="knob-label">Gain</div>
</div>
<div class="knob-container">
<div class="knob" data-param="eq6Q" data-min="0.1" data-max="10" data-default="0.7" step="0.1">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.7</div>
<div class="knob-label">Q</div>
</div>
</div>
</div>
</div>

</div>

<!-- CENTER PANEL -->
<div class="center-panel">

<!-- METER BRIDGE -->
<div class="meter-bridge">
<!-- VU METER -->
<div class="meter-panel">
<div class="meter-label">VU METER</div>
<div class="vu-meter">
<div class="vu-channel">
<div class="vu-bar">
<div class="vu-fill" id="vuL"></div>
<div class="vu-fill peak-hold" id="vuLPeak"></div>
</div>
<div class="vu-channel-label">L</div>
</div>
<div class="vu-channel">
<div class="vu-bar">
<div class="vu-fill" id="vuR"></div>
<div class="vu-fill peak-hold" id="vuRPeak"></div>
</div>
<div class="vu-channel-label">R</div>
</div>
</div>
</div>

<!-- PPM METER -->
<div class="meter-panel">
<div class="meter-label">PPM</div>
<div class="ppm-meter">
<div class="ppm-row">
<div class="ppm-label">0</div>
<div class="ppm-bar-container"><div class="ppm-bar-fill l" id="ppm0L"></div></div>
<div class="ppm-bar-container"><div class="ppm-bar-fill r" id="ppm0R"></div></div>
</div>
<div class="ppm-row">
<div class="ppm-label">4</div>
<div class="ppm-bar-container"><div class="ppm-bar-fill l" id="ppm4L"></div></div>
<div class="ppm-bar-container"><div class="ppm-bar-fill r" id="ppm4R"></div></div>
</div>
<div class="ppm-row">
<div class="ppm-label">8</div>
<div class="ppm-bar-container"><div class="ppm-bar-fill l" id="ppm8L"></div></div>
<div class="ppm-bar-container"><div class="ppm-bar-fill r" id="ppm8R"></div></div>
</div>
</div>
</div>

<!-- LUFS METER -->
<div class="meter-panel">
<div class="meter-label">LUFS</div>
<div class="lufs-display">
<div class="lufs-value" id="lufsValue">-23.0<span class="lufs-unit">LUFS</span></div>
<div class="lufs-info">
<div class="lufs-stat">
<div class="lufs-stat-label">Short</div>
<div class="lufs-stat-value" id="lufsShort">-23.0</div>
</div>
<div class="lufs-stat">
<div class="lufs-stat-label">Momentary</div>
<div class="lufs-stat-value" id="lufsMoment">-23.0</div>
</div>
<div class="lufs-stat">
<div class="lufs-stat-label">True Peak</div>
<div class="lufs-stat-value" id="truePeak">-1.0</div>
</div>
</div>
</div>
</div>
</div>

<!-- VISUALIZERS -->
<div class="visualizer-area">
<div class="visualizer-panel">
<div class="visualizer-header">
<span class="visualizer-title">Spectrum Analyzer</span>
<span style="font-size:8px;color:var(--text-dim);">FFT 4096 | Blackman-Harris</span>
</div>
<canvas class="visualizer-canvas" id="fftCanvas"></canvas>
</div>
<div class="visualizer-panel">
<div class="visualizer-header">
<span class="visualizer-title">Oscilloscope</span>
<span style="font-size:8px;color:var(--text-dim);">Time Domain | Stereo</span>
</div>
<canvas class="visualizer-canvas" id="scopeCanvas"></canvas>
</div>
</div>

<!-- DSP CHAIN -->
<div class="dsp-chain">
<div class="dsp-block active">INPUT</div>
<div class="dsp-arrow">→</div>
<div class="dsp-block active">HPF</div>
<div class="dsp-arrow">→</div>
<div class="dsp-block active">EQ</div>
<div class="dsp-arrow">→</div>
<div class="dsp-block active">AGC</div>
<div class="dsp-arrow">→</div>
<div class="dsp-block active">MULTIBAND</div>
<div class="dsp-arrow">→</div>
<div class="dsp-block active">MASTER</div>
<div class="dsp-arrow">→</div>
<div class="dsp-block active">LIMITER</div>
<div class="dsp-arrow">→</div>
<div class="dsp-block active">OUTPUT</div>
</div>
</div>

<!-- RIGHT PANEL -->
<div class="right-panel">

<!-- AGC -->
<div class="panel-section">
<div class="panel-header">
<span class="panel-title">AGC</span>
<div class="panel-bypass active" data-module="agc"></div>
</div>
<div class="compressor-section">
<div class="comp-controls">
<div class="knob-container">
<div class="knob" data-param="agcThreshold" data-min="-40" data-max="0" data-default="-20">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">-20 dB</div>
<div class="knob-label">Thresh</div>
</div>
<div class="knob-container">
<div class="knob" data-param="agcRatio" data-min="1" data-max="20" data-default="4">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">4:1</div>
<div class="knob-label">Ratio</div>
</div>
<div class="knob-container">
<div class="knob" data-param="agcAttack" data-min="0.1" data-max="100" data-default="5">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">5 ms</div>
<div class="knob-label">Attack</div>
</div>
<div class="knob-container">
<div class="knob" data-param="agcRelease" data-min="10" data-max="1000" data-default="100">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">100 ms</div>
<div class="knob-label">Release</div>
</div>
</div>
<div class="gr-meter"><div class="gr-meter-fill" id="agcGR"></div></div>
</div>
</div>

<!-- MULTIBAND COMPRESSOR -->
<div class="panel-section">
<div class="panel-header">
<span class="panel-title">Multiband Comp</span>
<div class="panel-bypass active" data-module="multiband"></div>
</div>

<div class="comp-controls" style="grid-template-columns:1fr 1fr 1fr 1fr;">
<div class="knob-container">
<div class="knob" data-param="mbXover1" data-min="100" data-max="500" data-default="250">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">250 Hz</div>
<div class="knob-label">X-Over 1</div>
</div>
<div class="knob-container">
<div class="knob" data-param="mbXover2" data-min="500" data-max="2000" data-default="1000">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">1.0 kHz</div>
<div class="knob-label">X-Over 2</div>
</div>
<div class="knob-container">
<div class="knob" data-param="mbXover3" data-min="2000" data-max="8000" data-default="4000">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">4.0 kHz</div>
<div class="knob-label">X-Over 3</div>
</div>
<div class="knob-container">
<div class="knob" data-param="mbMakeup" data-min="0" data-max="12" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0 dB</div>
<div class="knob-label">Makeup</div>
</div>
</div>

<div class="multiband-display">
<div class="multiband-channel">
<div class="multiband-label">BASS</div>
<div class="multiband-meter"><div class="multiband-meter-fill bass" id="mbMeter1"></div></div>
<div class="multiband-gr" id="mbGR1">0 dB</div>
</div>
<div class="multiband-channel">
<div class="multiband-label">LOW-MID</div>
<div class="multiband-meter"><div class="multiband-meter-fill low" id="mbMeter2"></div></div>
<div class="multiband-gr" id="mbGR2">0 dB</div>
</div>
<div class="multiband-channel">
<div class="multiband-label">HIGH-MID</div>
<div class="multiband-meter"><div class="multiband-meter-fill high" id="mbMeter3"></div></div>
<div class="multiband-gr" id="mbGR3">0 dB</div>
</div>
<div class="multiband-channel">
<div class="multiband-label">PRESENCE</div>
<div class="multiband-meter"><div class="multiband-meter-fill presence" id="mbMeter4"></div></div>
<div class="multiband-gr" id="mbGR4">0 dB</div>
</div>
</div>
</div>

<!-- MASTER COMPRESSOR -->
<div class="panel-section">
<div class="panel-header">
<span class="panel-title">Master Comp</span>
<div class="panel-bypass active" data-module="master"></div>
</div>
<div class="compressor-section">
<div class="comp-controls">
<div class="knob-container">
<div class="knob" data-param="masterThresh" data-min="-30" data-max="0" data-default="-6">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">-6 dB</div>
<div class="knob-label">Thresh</div>
</div>
<div class="knob-container">
<div class="knob" data-param="masterRatio" data-min="1" data-max="10" data-default="2">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">2:1</div>
<div class="knob-label">Ratio</div>
</div>
<div class="knob-container">
<div class="knob" data-param="masterAttack" data-min="0.1" data-max="50" data-default="3">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">3 ms</div>
<div class="knob-label">Attack</div>
</div>
<div class="knob-container">
<div class="knob" data-param="masterRelease" data-min="10" data-max="500" data-default="50">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">50 ms</div>
<div class="knob-label">Release</div>
</div>
</div>
<div class="gr-meter"><div class="gr-meter-fill" id="masterGR"></div></div>
</div>
</div>

<!-- FINAL LIMITER -->
<div class="panel-section">
<div class="panel-header">
<span class="panel-title">Final Limiter</span>
<div class="panel-bypass active" data-module="limiter"></div>
</div>
<div class="limiter-section">
<div class="limiter-threshold">
<span class="knob-label">Ceiling</span>
<div class="knob-container">
<div class="knob" data-param="limiterCeiling" data-min="-1" data-max="0" data-default="-0.1" step="0.1">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">-0.1 dB</div>
<div class="knob-label">dBTP</div>
</div>
</div>
<div style="margin-top:8px;">
<div class="limiter-threshold">
<span class="knob-label">Lookahead</span>
<div class="knob-container">
<div class="knob" data-param="limiterLookahead" data-min="0.5" data-max="5" data-default="1.5" step="0.1">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">1.5 ms</div>
<div class="knob-label">ms</div>
</div>
</div>
</div>
<div class="gr-meter"><div class="gr-meter-fill" id="limiterGR"></div></div>
</div>
</div>

<!-- OUTPUT -->
<div class="panel-section">
<div class="panel-header">
<span class="panel-title">Output Stage</span>
<div class="panel-bypass active" data-module="output"></div>
</div>
<div class="knobs-row">
<div class="knob-container">
<div class="knob" data-param="outputGain" data-min="-12" data-max="12" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">0.0 dB</div>
<div class="knob-label">Gain</div>
</div>
<div class="knob-container">
<div class="knob" data-param="outputDither" data-min="0" data-max="1" data-default="0">
<div class="knob-ring"></div>
<div class="knob-indicator"></div>
</div>
<div class="knob-value">OFF</div>
<div class="knob-label">Dither</div>
</div>
</div>
</div>

<!-- MODE SELECTOR -->
<div class="panel-section">
<div class="panel-header">
<span class="panel-title">Processing Mode</span>
</div>
<div style="display:flex;gap:4px;flex-wrap:wrap;">
<button class="preset-btn active" data-mode="streaming">Streaming</button>
<button class="preset-btn" data-mode="radio">Radio FM</button>
<button class="preset-btn" data-mode="podcast">Podcast</button>
<button class="preset-btn" data-mode="mastering">Mastering</button>
</div>
</div>

</div>
</div>

<!-- PRESET BAR -->
<div class="preset-bar">
<button class="preset-btn" data-preset="default">Default</button>
<button class="preset-btn" data-preset="broadcast">Broadcast</button>
<button class="preset-btn" data-preset="warm">Warm</button>
<button class="preset-btn" data-preset="bright">Bright</button>
<button class="preset-btn" data-preset="punchy">Punchy</button>
<button class="preset-btn" data-preset="voice">Voice</button>
<button class="preset-btn" data-preset="music">Music</button>
<button class="preset-btn" data-preset="loud">Max Loudness</button>
</div>

<!-- STATUS BAR -->
<div class="status-bar">
<div class="status-item">
<div class="status-dot" id="statusDSP"></div>
<span>DSP Engine</span>
</div>
<div class="status-item">
<div class="status-dot" id="statusAudio"></div>
<span>Audio Stream</span>
</div>
<div class="status-item">
<span>Sample Rate: <strong id="sampleRate">--</strong></span>
</div>
<div class="status-item">
<span>Buffer: <strong id="bufferSize">--</strong></span>
</div>
<div class="status-item">
<span>Latency: <strong id="latency">--</strong></span>
</div>
<div class="status-item">
<span>CPU: <strong id="cpuLoad">--</strong></span>
</div>
</div>

<!-- TOOLTIP -->
<div class="tooltip" id="tooltip"></div>

<script>
// ========================================================================
// BROADCAST AUDIO PROCESSOR - DSP ENGINE
// ========================================================================

class BroadcastProcessor {
constructor() {
this.audioContext = null;
this.analyser = null;
this.isPowered = false;
this.params = {};
this.presets = {};
this.modules = {
input: { bypass: false },
eq: { bypass: false },
agc: { bypass: false },
multiband: { bypass: false },
master: { bypass: false },
limiter: { bypass: false },
output: { bypass: false }
};
this.initPresets();
this.initKnobs();
this.initVisualizers();
this.initEvents();
}

initPresets() {
this.presets = {
default: {
inputGain: 0, hpfFreq: 40, saturation: 0,
eq1Freq: 80, eq1Gain: 0, eq1Q: 1.0,
eq2Freq: 300, eq2Gain: 0, eq2Q: 1.0,
eq3Freq: 1000, eq3Gain: 0, eq3Q: 1.0,
eq4Freq: 4000, eq4Gain: 0, eq4Q: 1.0,
eq5Freq: 8000, eq5Gain: 0, eq5Q: 1.0,
eq6Freq: 12000, eq6Gain: 0, eq6Q: 0.7,
agcThreshold: -20, agcRatio: 4, agcAttack: 5, agcRelease: 100,
mbXover1: 250, mbXover2: 1000, mbXover3: 4000, mbMakeup: 0,
masterThresh: -6, masterRatio: 2, masterAttack: 3, masterRelease: 50,
limiterCeiling: -0.1, limiterLookahead: 1.5,
outputGain: 0, outputDither: 0
},
broadcast: {
inputGain: 3, hpfFreq: 60, saturation: 5,
eq1Freq: 80, eq1Gain: 2, eq1Q: 1.2,
eq2Freq: 300, eq2Gain: -1, eq2Q: 0.8,
eq3Freq: 1000, eq3Gain: 1.5, eq3Q: 1.0,
eq4Freq: 4000, eq4Gain: 3, eq4Q: 0.8,
eq5Freq: 8000, eq5Gain: 2, eq5Q: 1.0,
eq6Freq: 12000, eq6Gain: 1, eq6Q: 0.7,
agcThreshold: -18, agcRatio: 6, agcAttack: 3, agcRelease: 80,
mbXover1: 200, mbXover2: 1200, mbXover3: 5000, mbMakeup: 3,
masterThresh: -8, masterRatio: 3, masterAttack: 2, masterRelease: 40,
limiterCeiling: -0.1, limiterLookahead: 1.5,
outputGain: 2, outputDither: 0
},
warm: {
inputGain: 0, hpfFreq: 40, saturation: 15,
eq1Freq: 80, eq1Gain: 3, eq1Q: 0.7,
eq2Freq: 250, eq2Gain: 1, eq2Q: 0.5,
eq3Freq: 800, eq3Gain: 0, eq3Q: 1.0,
eq4Freq: 3500, eq4Gain: -1, eq4Q: 0.8,
eq5Freq: 7000, eq5Gain: -2, eq5Q: 1.0,
eq6Freq: 12000, eq6Gain: -1, eq6Q: 0.7,
agcThreshold: -20, agcRatio: 3, agcAttack: 8, agcRelease: 150,
mbXover1: 300, mbXover2: 1500, mbXover3: 6000, mbMakeup: 1,
masterThresh: -4, masterRatio: 2, masterAttack: 5, masterRelease: 80,
limiterCeiling: -0.2, limiterLookahead: 2,
outputGain: 1, outputDither: 1
},
bright: {
inputGain: 0, hpfFreq: 50, saturation: 0,
eq1Freq: 100, eq1Gain: -2, eq1Q: 1.0,
eq2Freq: 300, eq2Gain: 0, eq2Q: 1.0,
eq3Freq: 1000, eq3Gain: 1, eq3Q: 1.0,
eq4Freq: 5000, eq4Gain: 4, eq4Q: 0.7,
eq5Freq: 10000, eq5Gain: 3, eq5Q: 0.8,
eq6Freq: 14000, eq6Gain: 2, eq6Q: 0.7,
agcThreshold: -18, agcRatio: 4, agcAttack: 5, agcRelease: 100,
mbXover1: 250, mbXover2: 1000, mbXover3: 4000, mbMakeup: 2,
masterThresh: -6, masterRatio: 2, masterAttack: 3, masterRelease: 50,
limiterCeiling: -0.1, limiterLookahead: 1.5,
outputGain: 1, outputDither: 0
},
punchy: {
inputGain: 2, hpfFreq: 50, saturation: 10,
eq1Freq: 60, eq1Gain: 4, eq1Q: 1.5,
eq2Freq: 200, eq2Gain: -2, eq2Q: 1.0,
eq3Freq: 1200, eq3Gain: 2, eq3Q: 1.2,
eq4Freq: 3000, eq4Gain: 3, eq4Q: 0.8,
eq5Freq: 6000, eq5Gain: 1, eq5Q: 1.0,
eq6Freq: 12000, eq6Gain: 0, eq6Q: 0.7,
agcThreshold: -16, agcRatio: 5, agcAttack: 2, agcRelease: 60,
mbXover1: 150, mbXover2: 800, mbXover3: 3500, mbMakeup: 4,
masterThresh: -10, masterRatio: 4, masterAttack: 1, masterRelease: 30,
limiterCeiling: -0.1, limiterLookahead: 1,
outputGain: 3, outputDither: 0
},
voice: {
inputGain: 6, hpfFreq: 80, saturation: 0,
eq1Freq: 100, eq1Gain: -4, eq1Q: 1.0,
eq2Freq: 300, eq2Gain: -2, eq2Q: 0.7,
eq3Freq: 2500, eq3Gain: 3, eq3Q: 1.2,
eq4Freq: 5000, eq4Gain: 2, eq4Q: 1.0,
eq5Freq: 8000, eq5Gain: 1, eq5Q: 1.0,
eq6Freq: 12000, eq6Gain: 0, eq6Q: 0.7,
agcThreshold: -20, agcRatio: 8, agcAttack: 3, agcRelease: 50,
mbXover1: 200, mbXover2: 2000, mbXover3: 6000, mbMakeup: 5,
masterThresh: -12, masterRatio: 5, masterAttack: 1, masterRelease: 40,
limiterCeiling: -0.1, limiterLookahead: 1,
outputGain: 4, outputDither: 0
},
music: {
inputGain: 0, hpfFreq: 30, saturation: 0,
eq1Freq: 80, eq1Gain: 1, eq1Q: 0.7,
eq2Freq: 250, eq2Gain: 0, eq2Q: 0.7,
eq3Freq: 1000, eq3Gain: 0, eq3Q: 1.0,
eq4Freq: 4000, eq4Gain: 1, eq4Q: 0.7,
eq5Freq: 8000, eq5Gain: 1, eq5Q: 0.7,
eq6Freq: 12000, eq6Gain: 0.5, eq6Q: 0.7,
agcThreshold: -22, agcRatio: 3, agcAttack: 10, agcRelease: 200,
mbXover1: 300, mbXover2: 1500, mbXover3: 6000, mbMakeup: 1,
masterThresh: -3, masterRatio: 1.5, masterAttack: 8, masterRelease: 100,
limiterCeiling: -0.2, limiterLookahead: 2,
outputGain: 0, outputDither: 1
},
loud: {
inputGain: 6, hpfFreq: 50, saturation: 10,
eq1Freq: 80, eq1Gain: 3, eq1Q: 1.0,
eq2Freq: 300, eq2Gain: -1, eq2Q: 0.8,
eq3Freq: 1500, eq3Gain: 2, eq3Q: 1.0,
eq4Freq: 4000, eq4Gain: 3, eq4Q: 0.8,
eq5Freq: 8000, eq5Gain: 2, eq5Q: 1.0,
eq6Freq: 12000, eq6Gain: 1, eq6Q: 0.7,
agcThreshold: -14, agcRatio: 8, agcAttack: 1, agcRelease: 40,
mbXover1: 200, mbXover2: 1000, mbXover3: 4000, mbMakeup: 6,
masterThresh: -14, masterRatio: 6, masterAttack: 1, masterRelease: 20,
limiterCeiling: -0.1, limiterLookahead: 0.5,
outputGain: 6, outputDither: 0
}
};
}

initKnobs() {
const knobs = document.querySelectorAll('.knob');
knobs.forEach(knob => {
const param = knob.dataset.param;
const min = parseFloat(knob.dataset.min);
const max = parseFloat(knob.dataset.max);
const defaultVal = parseFloat(knob.dataset.default);
const step = parseFloat(knob.dataset.step) || 1;

this.params[param] = { value: defaultVal, min, max, step };

this.setKnobPosition(knob, defaultVal);

let isDragging = false;
let startY = 0;
let startValue = defaultVal;
let lastUpdate = 0;

const onMouseDown = (e) => {
e.preventDefault();
isDragging = true;
startY = e.clientY;
startValue = this.params[param].value;
document.body.style.cursor = 'ns-resize';
};

const onMouseMove = (e) => {
if (!isDragging) return;
const delta = (startY - e.clientY) * 0.5;
let newValue = startValue + delta * ((max - min) / 200);
newValue = Math.max(min, Math.min(max, newValue));

if (step !== 1) {
newValue = Math.round(newValue / step) * step;
} else {
newValue = Math.round(newValue);
}

this.params[param].value = newValue;
this.updateKnobDisplay(knob, newValue, param);
this.setKnobPosition(knob, newValue);
};

const onMouseUp = () => {
isDragging = false;
document.body.style.cursor = '';
};

knob.addEventListener('mousedown', onMouseDown);
document.addEventListener('mousemove', onMouseMove);
document.addEventListener('mouseup', onMouseUp);

// Touch support
knob.addEventListener('touchstart', (e) => {
e.preventDefault();
isDragging = true;
startY = e.touches[0].clientY;
startValue = this.params[param].value;
});

document.addEventListener('touchmove', (e) => {
if (!isDragging) return;
const delta = (startY - e.touches[0].clientY) * 0.5;
let newValue = startValue + delta * ((max - min) / 200);
newValue = Math.max(min, Math.min(max, newValue));
if (step !== 1) newValue = Math.round(newValue / step) * step;
else newValue = Math.round(newValue);
this.params[param].value = newValue;
this.updateKnobDisplay(knob, newValue, param);
this.setKnobPosition(knob, newValue);
});

document.addEventListener('touchend', () => {
isDragging = false;
});
});
}

setKnobPosition(knob, value) {
const param = knob.dataset.param;
const p = this.params[param];
const normalized = (value - p.min) / (p.max - p.min);
const angle = -135 + normalized * 270;
const indicator = knob.querySelector('.knob-indicator');
indicator.style.transform = `translateX(-50%) rotate(${angle}deg)`;
indicator.style.transformOrigin = `50% ${24}px`;
}

updateKnobDisplay(knob, value, param) {
const valueEl = knob.parentElement.querySelector('.knob-value');
let display = '';
if (param.includes('Gain') || param.includes('Threshold') || param.includes('Thresh') ||
param.includes('Ceiling') || param.includes('inputGain') || param.includes('outputGain') ||
param.includes('eq') && !param.includes('Freq') && !param.includes('Q')) {
display = value.toFixed(1) + ' dB';
} else if (param.includes('Freq') || param.includes('xover') || param.includes('Xover')) {
if (value >= 1000) display = (value / 1000).toFixed(1) + ' kHz';
else display = Math.round(value) + ' Hz';
} else if (param.includes('Attack') || param.includes('Release') || param.includes('Lookahead')) {
display = value.toFixed(1) + ' ms';
} else if (param.includes('Ratio')) {
display = Math.round(value) + ':1';
} else if (param.includes('Q')) {
display = value.toFixed(1);
} else if (param.includes('saturation')) {
display = Math.round(value) + '%';
} else if (param.includes('Dither')) {
display = value > 0.5 ? 'ON' : 'OFF';
} else if (param.includes('Makeup')) {
display = value.toFixed(1) + ' dB';
} else {
display = value.toFixed(1);
}
valueEl.textContent = display;
}

initVisualizers() {
this.fftCanvas = document.getElementById('fftCanvas');
this.scopeCanvas = document.getElementById('scopeCanvas');
this.fftCtx = this.fftCanvas.getContext('2d');
this.scopeCtx = this.scopeCanvas.getContext('2d');

this.resizeCanvases();
window.addEventListener('resize', () => this.resizeCanvases());

this.fftData = new Uint8Array(2048);
this.timeData = new Uint8Array(1024);

this.animate();
}

resizeCanvases() {
const panels = document.querySelectorAll('.visualizer-panel');
panels.forEach(panel => {
const canvas = panel.querySelector('canvas');
if (canvas) {
canvas.width = panel.clientWidth;
canvas.height = panel.clientHeight;
}
});
}

animate() {
this.drawFFT();
this.drawScope();
requestAnimationFrame(() => this.animate());
}

drawFFT() {
const ctx = this.fftCtx;
const canvas = this.fftCanvas;
if (!canvas.width) return;

const w = canvas.width;
const h = canvas.height;

ctx.fillStyle = 'rgba(10, 10, 15, 0.3)';
ctx.fillRect(0, 0, w, h);

// Grid
ctx.strokeStyle = 'rgba(42, 42, 58, 0.3)';
ctx.lineWidth = 0.5;
for (let i = 0; i < 10; i++) {
const y = (h / 10) * i;
ctx.beginPath();
ctx.moveTo(0, y);
ctx.lineTo(w, y);
ctx.stroke();
}

for (let i = 0; i < 20; i++) {
const x = (w / 20) * i;
ctx.beginPath();
ctx.moveTo(x, 0);
ctx.lineTo(x, h);
ctx.stroke();
}

// Frequency labels
ctx.fillStyle = 'rgba(136, 136, 160, 0.5)';
ctx.font = '8px monospace';
const freqs = ['20', '50', '100', '200', '500', '1k', '2k', '5k', '10k', '20k'];
freqs.forEach((f, i) => {
const x = (w / (freqs.length - 1)) * i;
ctx.fillText(f, x, h - 2);
});

// Generate simulated FFT data
const barCount = 128;
const barWidth = w / barCount;

for (let i = 0; i < barCount; i++) {
const normalizedFreq = i / barCount;
const logFreq = Math.log10(normalizedFreq + 0.01) / Math.log10(1.01);

let magnitude = 0;

if (this.isPowered) {
// Simulate realistic frequency content
const bass = Math.sin(normalizedFreq * Math.PI * 4) * 0.4;
const mid = Math.sin(normalizedFreq * Math.PI * 2) * 0.3;
const high = Math.sin(normalizedFreq * Math.PI) * 0.2;
const noise = (Math.random() - 0.5) * 0.15;

magnitude = (bass + mid + high + noise + 0.5) * (1 - normalizedFreq * 0.5);
magnitude = Math.max(0, Math.min(1, magnitude));

// Apply EQ visualization
const eqBands = [
{ freq: this.getParam('eq1Freq') / 20000, gain: this.getParam('eq1Gain') / 15 },
{ freq: this.getParam('eq2Freq') / 20000, gain: this.getParam('eq2Gain') / 15 },
{ freq: this.getParam('eq3Freq') / 20000, gain: this.getParam('eq3Gain') / 15 },
{ freq: this.getParam('eq4Freq') / 20000, gain: this.getParam('eq4Gain') / 15 },
{ freq: this.getParam('eq5Freq') / 20000, gain: this.getParam('eq5Gain') / 15 },
{ freq: this.getParam('eq6Freq') / 20000, gain: this.getParam('eq6Gain') / 15 }
];

eqBands.forEach(band => {
const dist = Math.abs(normalizedFreq - band.freq);
const influence = Math.exp(-dist * dist * 50) * band.gain;
magnitude += influence * 0.3;
});

magnitude = Math.max(0, Math.min(1, magnitude));
} else {
magnitude = 0.02 + Math.random() * 0.03;
}

const barHeight = magnitude * h * 0.9;
const x = i * barWidth;

// Gradient based on magnitude
let gradient;
if (magnitude > 0.85) {
gradient = ctx.createLinearGradient(x, h, x, h - barHeight);
gradient.addColorStop(0, '#cc2200');
gradient.addColorStop(1, '#ff4400');
} else if (magnitude > 0.65) {
gradient = ctx.createLinearGradient(x, h, x, h - barHeight);
gradient.addColorStop(0, '#cccc00');
gradient.addColorStop(1, '#ffaa00');
} else {
gradient = ctx.createLinearGradient(x, h, x, h - barHeight);
gradient.addColorStop(0, '#008833');
gradient.addColorStop(1, '#00cc66');
}

ctx.fillStyle = gradient;
ctx.fillRect(x + 1, h - barHeight, barWidth - 2, barHeight);

// Glow effect for high values
if (magnitude > 0.7) {
ctx.shadowColor = magnitude > 0.85 ? '#ff3300' : '#ffaa00';
ctx.shadowBlur = 5;
ctx.fillRect(x + 1, h - barHeight, barWidth - 2, 2);
ctx.shadowBlur = 0;
}
}
}

drawScope() {
const ctx = this.scopeCtx;
const canvas = this.scopeCanvas;
if (!canvas.width) return;

const w = canvas.width;
const h = canvas.height;

ctx.fillStyle = 'rgba(10, 10, 15, 0.5)';
ctx.fillRect(0, 0, w, h);

// Grid
ctx.strokeStyle = 'rgba(42, 42, 58, 0.3)';
ctx.lineWidth = 0.5;
ctx.beginPath();
ctx.moveTo(0, h / 2);
ctx.lineTo(w, h / 2);
ctx.stroke();

for (let i = 0; i < 5; i++) {
const y = (h / 5) * i;
ctx.beginPath();
ctx.moveTo(0, y);
ctx.lineTo(w, y);
ctx.stroke();
}

// Center line
ctx.strokeStyle = 'rgba(42, 42, 58, 0.6)';
ctx.beginPath();
ctx.moveTo(0, h / 2);
ctx.lineTo(w, h / 2);
ctx.stroke();

// Draw waveform
if (this.isPowered) {
const centerY = h / 2;
const amplitude = h * 0.35;

// Left channel
ctx.strokeStyle = 'rgba(0, 255, 102, 0.8)';
ctx.lineWidth = 1.5;
ctx.shadowColor = '#00ff66';
ctx.shadowBlur = 3;
ctx.beginPath();

const time = Date.now() / 1000;
for (let x = 0; x < w; x++) {
const t = x / w;
const y = centerY + (
Math.sin(t * Math.PI * 20 + time * 5) * 0.3 +
Math.sin(t * Math.PI * 8 + time * 3) * 0.2 +
Math.sin(t * Math.PI * 40 + time * 7) * 0.1 +
(Math.random() - 0.5) * 0.1
) * amplitude;

if (x === 0) ctx.moveTo(x, y);
else ctx.lineTo(x, y);
}
ctx.stroke();

// Right channel
ctx.strokeStyle = 'rgba(68, 136, 255, 0.6)';
ctx.shadowColor = '#4488ff';
ctx.beginPath();

for (let x = 0; x < w; x++) {
const t = x / w;
const y = centerY + (
Math.sin(t * Math.PI * 22 + time * 5.5) * 0.28 +
Math.sin(t * Math.PI * 10 + time * 2.8) * 0.22 +
Math.sin(t * Math.PI * 35 + time * 6.5) * 0.12 +
(Math.random() - 0.5) * 0.1
) * amplitude;

if (x === 0) ctx.moveTo(x, y);
else ctx.lineTo(x, y);
}
ctx.stroke();

ctx.shadowBlur = 0;
} else {
// Flat line
ctx.strokeStyle = 'rgba(0, 255, 102, 0.3)';
ctx.lineWidth = 1;
ctx.beginPath();
ctx.moveTo(0, h / 2);
ctx.lineTo(w, h / 2);
ctx.stroke();
}
}

initEvents() {
// Power button
document.getElementById('btnPower').addEventListener('click', () => {
this.togglePower();
});

// Preset buttons
document.querySelectorAll('.preset-btn[data-preset]').forEach(btn => {
btn.addEventListener('click', () => {
const preset = btn.dataset.preset;
this.loadPreset(preset);
document.querySelectorAll('.preset-btn[data-preset]').forEach(b => b.classList.remove('active'));
btn.classList.add('active');
});
});

// Mode buttons
document.querySelectorAll('.preset-btn[data-mode]').forEach(btn => {
btn.addEventListener('click', () => {
document.querySelectorAll('.preset-btn[data-mode]').forEach(b => b.classList.remove('active'));
btn.classList.add('active');
this.applyMode(btn.dataset.mode);
});
});

// Bypass toggles
document.querySelectorAll('.panel-bypass').forEach(btn => {
btn.addEventListener('click', () => {
const module = btn.dataset.module;
this.modules[module].bypass = !this.modules[module].bypass;
btn.classList.toggle('active');
this.updateDSPChain();
});
});

// Export/Import
document.getElementById('btnExport').addEventListener('click', () => {
this.exportConfig();
});

document.getElementById('btnImport').addEventListener('click', () => {
this.importConfig();
});

// Input select
document.getElementById('inputSelect').addEventListener('change', (e) => {
this.handleInputSelect(e.target.value);
});
}

togglePower() {
this.isPowered = !this.isPowered;
const btn = document.getElementById('btnPower');
const statusDSP = document.getElementById('statusDSP');
const statusAudio = document.getElementById('statusAudio');

if (this.isPowered) {
btn.classList.add('on');
statusDSP.classList.add('active');
statusAudio.classList.add('active');
this.initAudio();
} else {
btn.classList.remove('on');
statusDSP.classList.remove('active');
statusAudio.classList.remove('active');
if (this.audioContext) {
this.audioContext.close();
this.audioContext = null;
}
}
}

async initAudio() {
try {
this.audioContext = new (window.AudioContext || window.webkitAudioContext)({
sampleRate: 48000
});

document.getElementById('sampleRate').textContent = this.audioContext.sampleRate + ' Hz';
document.getElementById('bufferSize').textContent = '256 samples';
document.getElementById('latency').textContent = (256 / this.audioContext.sampleRate * 1000).toFixed(1) + ' ms';

// Create audio nodes chain
this.inputNode = this.audioContext.createGain();

// HPF
this.hpfNode = this.audioContext.createBiquadFilter();
this.hpfNode.type = 'highpass';
this.hpfNode.frequency.value = this.getParam('hpfFreq');
this.hpfNode.Q.value = 0.707;

// EQ bands
this.eqNodes = [];
for (let i = 1; i <= 6; i++) {
const eq = this.audioContext.createBiquadFilter();
const types = ['lowshelf', 'peaking', 'peaking', 'peaking', 'peaking', 'highshelf'];
eq.type = types[i - 1];
eq.frequency.value = this.getParam(`eq${i}Freq`);
eq.gain.value = this.getParam(`eq${i}Gain`);
eq.Q.value = this.getParam(`eq${i}Q`);
this.eqNodes.push(eq);
}

// AGC (Compressor)
this.agcNode = this.audioContext.createDynamicsCompressor();
this.agcNode.threshold.value = this.getParam('agcThreshold');
this.agcNode.ratio.value = this.getParam('agcRatio');
this.agcNode.attack.value = this.getParam('agcAttack') / 1000;
this.agcNode.release.value = this.getParam('agcRelease') / 1000;
this.agcNode.knee.value = 6;

// Master compressor
this.masterNode = this.audioContext.createDynamicsCompressor();
this.masterNode.threshold.value = this.getParam('masterThresh');
this.masterNode.ratio.value = this.getParam('masterRatio');
this.masterNode.attack.value = this.getParam('masterAttack') / 1000;
this.masterNode.release.value = this.getParam('masterRelease') / 1000;
this.masterNode.knee.value = 3;

// Limiter
this.limiterNode = this.audioContext.createDynamicsCompressor();
this.limiterNode.threshold.value = this.getParam('limiterCeiling') * 10; // Convert dBTP to threshold
this.limiterNode.ratio.value = 20;
this.limiterNode.attack.value = 0.001;
this.limiterNode.release.value = 0.05;
this.limiterNode.knee.value = 0;

// Output
this.outputNode = this.audioContext.createGain();
this.outputNode.gain.value = Math.pow(10, this.getParam('outputGain') / 20);

// Analyser
this.analyser = this.audioContext.createAnalyser();
this.analyser.fftSize = 4096;
this.analyser.smoothingTimeConstant = 0.8;

// Connect chain
this.inputNode.connect(this.hpfNode);

let chain = this.hpfNode;
this.eqNodes.forEach(eq => {
chain.connect(eq);
chain = eq;
});

chain.connect(this.agcNode);
this.agcNode.connect(this.masterNode);
this.masterNode.connect(this.limiterNode);
this.limiterNode.connect(this.outputNode);
this.outputNode.connect(this.analyser);
this.analyser.connect(this.audioContext.destination);

// Try to get microphone input
try {
const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
this.micSource = this.audioContext.createMediaStreamSource(stream);
this.micSource.connect(this.inputNode);
} catch (err) {
console.warn('Microphone not available, using oscillator for demo');
this.oscillator = this.audioContext.createOscillator();
this.oscillator.type = 'sine';
this.oscillator.frequency.value = 440;
this.oscillator.connect(this.inputNode);
this.oscillator.start();
}

this.updateCPU();
} catch (err) {
console.error('Audio initialization error:', err);
}
}

updateAudioParams() {
if (!this.audioContext) return;

this.hpfNode.frequency.value = this.getParam('hpfFreq');

this.eqNodes.forEach((eq, i) => {
const idx = i + 1;
eq.frequency.value = this.getParam(`eq${idx}Freq`);
eq.gain.value = this.getParam(`eq${idx}Gain`);
eq.Q.value = this.getParam(`eq${idx}Q`);
});

this.agcNode.threshold.value = this.getParam('agcThreshold');
this.agcNode.ratio.value = this.getParam('agcRatio');
this.agcNode.attack.value = this.getParam('agcAttack') / 1000;
this.agcNode.release.value = this.getParam('agcRelease') / 1000;

this.masterNode.threshold.value = this.getParam('masterThresh');
this.masterNode.ratio.value = this.getParam('masterRatio');
this.masterNode.attack.value = this.getParam('masterAttack') / 1000;
this.masterNode.release.value = this.getParam('masterRelease') / 1000;

this.limiterNode.threshold.value = this.getParam('limiterCeiling') * 10;

this.outputNode.gain.value = Math.pow(10, this.getParam('outputGain') / 20);
}

getParam(name) {
return this.params[name] ? this.params[name].value : 0;
}

loadPreset(name) {
const preset = this.presets[name];
if (!preset) return;

Object.keys(preset).forEach(key => {
if (this.params[key]) {
this.params[key].value = preset[key];
}
});

// Update all knob displays
document.querySelectorAll('.knob').forEach(knob => {
const param = knob.dataset.param;
const value = this.params[param].value;
this.updateKnobDisplay(knob, value, param);
this.setKnobPosition(knob, value);
});

this.updateAudioParams();
this.updateMeters();
}

applyMode(mode) {
// Apply mode-specific processing adjustments
switch (mode) {
case 'streaming':
this.loadPreset('broadcast');
break;
case 'radio':
this.loadPreset('loud');
break;
case 'podcast':
this.loadPreset('voice');
break;
case 'mastering':
this.loadPreset('default');
break;
}
}

updateDSPChain() {
const blocks = document.querySelectorAll('.dsp-block');
const modules = ['input', 'hpf', 'eq', 'agc', 'multiband', 'master', 'limiter', 'output'];

blocks.forEach((block, i) => {
const module = modules[i];
if (this.modules[module] && this.modules[module].bypass) {
block.classList.remove('active');
} else {
block.classList.add('active');
}
});
}

updateMeters() {
if (!this.analyser || !this.isPowered) {
this.updateMeterValues(0, 0);
return;
}

this.analyser.getByteFrequencyData(this.fftData);
this.analyser.getByteTimeDomainData(this.timeData);

// Calculate RMS
let sum = 0;
for (let i = 0; i < this.timeData.length; i++) {
const val = (this.timeData[i] - 128) / 128;
sum += val * val;
}
const rms = Math.sqrt(sum / this.timeData.length);
const dbRMS = 20 * Math.log10(Math.max(rms, 0.0001));

// Peak
let peak = 0;
for (let i = 0; i < this.timeData.length; i++) {
const val = Math.abs((this.timeData[i] - 128) / 128);
if (val > peak) peak = val;
}
const dbPeak = 20 * Math.log10(Math.max(peak, 0.0001));

this.updateMeterValues(
Math.min(0, Math.max(-60, dbRMS + 24)),
Math.min(0, Math.max(-60, dbPeak + 24))
);

// LUFS simulation
const lufs = dbRMS - 14;
document.getElementById('lufsValue').innerHTML =
lufs.toFixed(1) + '<span class="lufs-unit">LUFS</span>';
document.getElementById('lufsShort').textContent = (lufs + 1).toFixed(1);
document.getElementById('lufsMoment').textContent = (lufs + 2).toFixed(1);
document.getElementById('truePeak').textContent = (dbPeak + 0.1).toFixed(1);

// Multiband simulation
const mb1 = Math.min(100, Math.max(0, rms * 300 * (1 + this.getParam('eq1Gain') / 20)));
const mb2 = Math.min(100, Math.max(0, rms * 250 * (1 + this.getParam('eq2Gain') / 20)));
const mb3 = Math.min(100, Math.max(0, rms * 200 * (1 + this.getParam('eq3Gain') / 20)));
const mb4 = Math.min(100, Math.max(0, rms * 150 * (1 + this.getParam('eq4Gain') / 20)));

document.getElementById('mbMeter1').style.height = mb1 + '%';
document.getElementById('mbMeter2').style.height = mb2 + '%';
document.getElementById('mbMeter3').style.height = mb3 + '%';
document.getElementById('mbMeter4').style.height = mb4 + '%';

// Gain reduction simulation
const agcGR = Math.max(0, Math.min(100, -this.getParam('agcThreshold') * 2 + rms * 50));
const masterGR = Math.max(0, Math.min(100, -this.getParam('masterThresh') * 3 + rms * 30));
const limiterGR = Math.max(0, Math.min(100, -this.getParam('limiterCeiling') * 10 + rms * 40));

document.getElementById('agcGR').style.width = agcGR + '%';
document.getElementById('masterGR').style.width = masterGR + '%';
document.getElementById('limiterGR').style.width = limiterGR + '%';

document.getElementById('mbGR1').textContent = (agcGR / 10).toFixed(1) + ' dB';
document.getElementById('mbGR2').textContent = (agcGR / 12).toFixed(1) + ' dB';
document.getElementById('mbGR3').textContent = (masterGR / 10).toFixed(1) + ' dB';
document.getElementById('mbGR4').textContent = (limiterGR / 8).toFixed(1) + ' dB';
}

updateMeterValues(rmsL, rmsR) {
const vuL = document.getElementById('vuL');
const vuR = document.getElementById('vuR');
const vuLPeak = document.getElementById('vuLPeak');
const vuRPeak = document.getElementById('vuRPeak');

vuL.style.width = Math.max(0, rmsL) + '%';
vuR.style.width = Math.max(0, rmsR) + '%';
vuLPeak.style.right = Math.max(0, 100 - rmsL - 2) + '%';
vuRPeak.style.right = Math.max(0, 100 - rmsR - 2) + '%';

// PPM
const ppm0 = Math.min(100, rmsL * 1.2);
const ppm4 = Math.min(100, rmsL * 0.8);
const ppm8 = Math.min(100, rmsL * 0.4);

document.getElementById('ppm0L').style.width = ppm0 + '%';
document.getElementById('ppm0R').style.width = ppm0 + '%';
document.getElementById('ppm4L').style.width = ppm4 + '%';
document.getElementById('ppm4R').style.width = ppm4 + '%';
document.getElementById('ppm8L').style.width = ppm8 + '%';
document.getElementById('ppm8R').style.width = ppm8 + '%';
}

updateCPU() {
if (!this.audioContext) return;

const startTime = performance.now();

// Simulate CPU measurement
setInterval(() => {
const cpu = (Math.random() * 15 + 5).toFixed(1);
document.getElementById('cpuLoad').textContent = cpu + '%';
}, 1000);
}

handleInputSelect(value) {
if (value === 'file') {
const input = document.createElement('input');
input.type = 'file';
input.accept = 'audio/*';
input.onchange = (e) => {
const file = e.target.files[0];
if (file && this.audioContext) {
const reader = new FileReader();
reader.onload = async (ev) => {
const buffer = await this.audioContext.decodeAudioData(ev.target.result);
this.playBuffer(buffer);
};
reader.readAsArrayBuffer(file);
}
};
input.click();
} else if (value === 'mic' || value === 'line') {
this.initAudio();
}
}

async playBuffer(buffer) {
if (!this.audioContext) return;

const source = this.audioContext.createBufferSource();
source.buffer = buffer;
source.connect(this.inputNode);
source.start();
}

exportConfig() {
const config = {
params: {},
modules: this.modules
};

Object.keys(this.params).forEach(key => {
config.params[key] = this.params[key].value;
});

const blob = new Blob([JSON.stringify(config, null, 2)], { type: 'application/json' });
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = 'broadcast-processor-config.json';
a.click();
URL.revokeObjectURL(url);
}

importConfig() {
const input = document.createElement('input');
input.type = 'file';
input.accept = '.json';
input.onchange = (e) => {
const file = e.target.files[0];
if (file) {
const reader = new FileReader();
reader.onload = (ev) => {
try {
const config = JSON.parse(ev.target.result);
Object.keys(config.params).forEach(key => {
if (this.params[key]) {
this.params[key].value = config.params[key];
}
});
// Update displays
document.querySelectorAll('.knob').forEach(knob => {
const param = knob.dataset.param;
const value = this.params[param].value;
this.updateKnobDisplay(knob, value, param);
this.setKnobPosition(knob, value);
});
this.updateAudioParams();
} catch (err) {
alert('Error loading config file');
}
};
reader.readAsText(file);
}
};
input.click();
}
}

// ========================================================================
// INITIALIZE
// ========================================================================

const processor = new BroadcastProcessor();

// Meter update loop
function updateMetersLoop() {
processor.updateMeters();
requestAnimationFrame(updateMetersLoop);
}

// Auto-update audio params periodically
setInterval(() => {
if (processor.isPowered) {
processor.updateAudioParams();
}
}, 50);

// Hide loading screen
setTimeout(() => {
document.getElementById('loadingOverlay').classList.add('hidden');
}, 1500);

// Start meter updates
updateMetersLoop();
</script>
</body>
</html>
