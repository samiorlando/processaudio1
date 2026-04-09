<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>OptiMod Pro 1600 - Broadcast Audio Processor</title>
<style>
:root {
  --bg-primary: #0a0a0a;
  --bg-secondary: #1a1a1a;
  --bg-panel: #141414;
  --bg-knob: #222;
  --border-color: #333;
  --text-primary: #e0e0e0;
  --text-secondary: #888;
  --accent-amber: #ffb700;
  --accent-green: #39ff14;
  --accent-red: #ff3333;
  --accent-blue: #00aaff;
  --meter-green: #00ff41;
  --meter-yellow: #ffcc00;
  --meter-red: #ff0000;
  --knob-size: 52px;
  --knob-small: 38px;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: 'Segoe UI', 'Roboto', 'Arial', sans-serif;
  background: var(--bg-primary);
  color: var(--text-primary);
  overflow-x: hidden;
  min-height: 100vh;
}

/* HEADER */
.header {
  background: linear-gradient(180deg, #1a1a1a 0%, #0d0d0d 100%);
  border-bottom: 2px solid var(--accent-amber);
  padding: 10px 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 20px rgba(255,183,0,0.1);
}

.logo {
  display: flex;
  align-items: center;
  gap: 15px;
}

.logo-icon {
  width: 50px;
  height: 50px;
  background: radial-gradient(circle, var(--accent-amber) 0%, #cc8800 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  font-weight: bold;
  color: #000;
  box-shadow: 0 0 15px rgba(255,183,0,0.4);
  text-shadow: 0 1px 2px rgba(255,255,255,0.3);
}

.logo-text h1 {
  font-size: 18px;
  font-weight: 700;
  color: var(--accent-amber);
  letter-spacing: 3px;
  text-transform: uppercase;
}

.logo-text span {
  font-size: 11px;
  color: var(--text-secondary);
  letter-spacing: 2px;
}

.header-controls {
  display: flex;
  align-items: center;
  gap: 15px;
}

.power-btn {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid #555;
  background: radial-gradient(circle, #333 0%, #111 100%);
  color: #666;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  transition: all 0.3s;
  box-shadow: 0 2px 5px rgba(0,0,0,0.5);
}

.power-btn.active {
  border-color: var(--accent-green);
  color: var(--accent-green);
  box-shadow: 0 0 15px rgba(57,255,20,0.3);
}

.menu-btn {
  padding: 6px 14px;
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  color: var(--text-primary);
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
  transition: all 0.2s;
}

.menu-btn:hover {
  background: #2a2a2a;
  border-color: var(--accent-amber);
}

/* MAIN LAYOUT */
.main-container {
  display: flex;
  flex-direction: column;
  padding: 10px;
  gap: 10px;
  max-width: 1600px;
  margin: 0 auto;
}

/* TOP SECTION: Input + Meters + Output */
.top-section {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  gap: 10px;
}

.panel {
  background: var(--bg-panel);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 12px;
  position: relative;
  overflow: hidden;
}

.panel::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(255,183,0,0.3), transparent);
}

.panel-title {
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: var(--accent-amber);
  margin-bottom: 10px;
  text-align: center;
  font-weight: 600;
}

/* KNOB STYLES */
.knob-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}

.knob-wrapper {
  position: relative;
  cursor: grab;
}

.knob-wrapper:active { cursor: grabbing; }

.knob {
  width: var(--knob-size);
  height: var(--knob-size);
  border-radius: 50%;
  background: conic-gradient(from 220deg, #444 0%, #222 30%, #111 50%, #333 70%, #222 100%);
  box-shadow: 0 4px 10px rgba(0,0,0,0.6), inset 0 1px 2px rgba(255,255,255,0.1), 0 0 0 1px #111;
  position: relative;
  transition: box-shadow 0.1s;
}

.knob::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 2px;
  height: 35%;
  background: var(--accent-amber);
  transform-origin: bottom center;
  transform: translate(-50%, -100%) rotate(var(--knob-angle, 0deg));
  border-radius: 1px;
  box-shadow: 0 0 4px rgba(255,183,0,0.5);
}

.knob::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 8px;
  height: 8px;
  background: #444;
  border-radius: 50%;
  transform: translate(-50%, -50%);
  box-shadow: inset 0 1px 2px rgba(0,0,0,0.5);
}

.knob.small {
  width: var(--knob-small);
  height: var(--knob-small);
}

.knob.small::before {
  height: 30%;
  width: 1.5px;
}

.knob-value {
  font-size: 10px;
  color: var(--accent-green);
  font-family: 'Courier New', monospace;
  font-weight: bold;
  min-height: 14px;
}

.knob-label {
  font-size: 9px;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 1px;
  text-align: center;
}

.knob-track {
  position: absolute;
  top: -4px;
  left: -4px;
  right: -4px;
  bottom: -4px;
  border-radius: 50%;
  border: 2px solid transparent;
  pointer-events: none;
}

/* METER STYLES */
.meter-container {
  display: flex;
  gap: 8px;
  justify-content: center;
  align-items: flex-end;
  height: 120px;
  padding: 5px;
}

.meter {
  width: 30px;
  height: 100%;
  background: #0a0a0a;
  border-radius: 4px;
  position: relative;
  overflow: hidden;
  border: 1px solid #333;
}

.meter-fill {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 0%;
  background: linear-gradient(0deg, var(--meter-green) 0%, var(--meter-green) 60%, var(--meter-yellow) 80%, var(--meter-red) 100%);
  transition: height 0.05s;
  border-radius: 0 0 3px 3px;
}

.meter-peak {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: #fff;
  transition: bottom 0.02s;
}

.meter-scale {
  position: absolute;
  right: 2px;
  top: 0;
  bottom: 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  font-size: 7px;
  color: #555;
  font-family: 'Courier New', monospace;
  padding: 2px 0;
}

.meter-label {
  text-align: center;
  font-size: 9px;
  color: var(--text-secondary);
  margin-top: 4px;
  letter-spacing: 1px;
}

/* SPECTRUM ANALYZER */
.spectrum-panel {
  grid-column: 1 / -1;
}

.spectrum-canvas {
  width: 100%;
  height: 100px;
  background: #050505;
  border-radius: 4px;
  border: 1px solid #222;
}

/* DSP CHAIN ROWS */
.dsp-row {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 10px;
}

.dsp-panel {
  background: var(--bg-panel);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 12px;
  position: relative;
}

.dsp-panel::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(57,255,20,0.2), transparent);
}

.dsp-panel-title {
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: var(--accent-green);
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.dsp-panel-title .status-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--accent-green);
  box-shadow: 0 0 6px rgba(57,255,20,0.5);
}

.knobs-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  justify-content: center;
}

/* EQ BAND DISPLAY */
.eq-display {
  width: 100%;
  height: 60px;
  background: #050505;
  border-radius: 4px;
  margin-bottom: 8px;
  border: 1px solid #222;
}

/* BAND SECTION */
.band-section {
  background: rgba(255,255,255,0.02);
  border: 1px solid rgba(255,255,255,0.05);
  border-radius: 6px;
  padding: 8px;
  margin-bottom: 8px;
}

.band-title {
  font-size: 9px;
  color: var(--accent-amber);
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 6px;
  text-align: center;
}

/* BUTTONS */
.toggle-btn {
  padding: 4px 10px;
  background: #222;
  border: 1px solid #444;
  color: var(--text-secondary);
  border-radius: 3px;
  cursor: pointer;
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 1px;
  transition: all 0.2s;
}

.toggle-btn.active {
  background: rgba(57,255,20,0.15);
  border-color: var(--accent-green);
  color: var(--accent-green);
}

.toggle-btn:hover {
  border-color: var(--accent-amber);
}

/* SELECT */
.select-styled {
  background: #1a1a1a;
  border: 1px solid #333;
  color: var(--text-primary);
  padding: 4px 8px;
  border-radius: 3px;
  font-size: 10px;
  cursor: pointer;
}

/* PRESETS BAR */
.presets-bar {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
  justify-content: center;
  padding: 8px 0;
}

.preset-btn {
  padding: 5px 12px;
  background: linear-gradient(180deg, #2a2a2a 0%, #1a1a1a 100%);
  border: 1px solid #444;
  color: var(--text-primary);
  border-radius: 3px;
  cursor: pointer;
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 1px;
  transition: all 0.2s;
}

.preset-btn:hover {
  border-color: var(--accent-amber);
  background: linear-gradient(180deg, #333 0%, #222 100%);
}

.preset-btn.active {
  border-color: var(--accent-amber);
  background: linear-gradient(180deg, rgba(255,183,0,0.2) 0%, rgba(255,183,0,0.05) 100%);
  color: var(--accent-amber);
}

/* CONFIG PANEL */
.config-overlay {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.85);
  z-index: 1000;
  justify-content: center;
  align-items: center;
}

.config-overlay.visible {
  display: flex;
}

.config-panel {
  background: var(--bg-secondary);
  border: 1px solid var(--accent-amber);
  border-radius: 12px;
  padding: 25px;
  width: 90%;
  max-width: 500px;
  max-height: 80vh;
  overflow-y: auto;
}

.config-panel h2 {
  color: var(--accent-amber);
  font-size: 16px;
  margin-bottom: 15px;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.config-section {
  margin-bottom: 15px;
  padding: 12px;
  background: rgba(255,255,255,0.03);
  border-radius: 6px;
  border: 1px solid #333;
}

.config-section h3 {
  font-size: 11px;
  color: var(--accent-green);
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 8px;
}

.config-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 6px;
}

.config-row label {
  font-size: 11px;
  color: var(--text-secondary);
}

.config-row select, .config-row input {
  background: #111;
  border: 1px solid #444;
  color: var(--text-primary);
  padding: 4px 8px;
  border-radius: 3px;
  font-size: 11px;
}

.config-close {
  margin-top: 15px;
  padding: 8px 20px;
  background: var(--accent-amber);
  border: none;
  color: #000;
  font-weight: bold;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
  text-transform: uppercase;
  letter-spacing: 2px;
}

/* MODE SELECTOR */
.mode-selector {
  display: flex;
  gap: 4px;
  justify-content: center;
}

.mode-btn {
  padding: 6px 16px;
  background: #1a1a1a;
  border: 1px solid #333;
  color: var(--text-secondary);
  border-radius: 3px;
  cursor: pointer;
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 1px;
  transition: all 0.2s;
}

.mode-btn.active {
  background: rgba(255,183,0,0.15);
  border-color: var(--accent-amber);
  color: var(--accent-amber);
}

/* SCROLLBAR */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg-primary); }
::-webkit-scrollbar-thumb { background: #444; border-radius: 3px; }
::-webkit-scrollbar-thumb:hover { background: #555; }

/* RESPONSIVE */
@media (max-width: 768px) {
  .top-section {
    grid-template-columns: 1fr;
  }
  .dsp-row {
    grid-template-columns: 1fr;
  }
  .header {
    flex-direction: column;
    gap: 10px;
  }
  .knob { --knob-size: 42px; }
  .knob.small { --knob-small: 32px; }
}

/* ANIMATIONS */
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

.processing-active {
  animation: pulse 2s infinite;
}

/* TOOLTIP */
.tooltip {
  position: absolute;
  background: rgba(0,0,0,0.9);
  border: 1px solid var(--accent-amber);
  color: var(--text-primary);
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 10px;
  pointer-events: none;
  z-index: 100;
  display: none;
  white-space: nowrap;
}

/* GAIN REDUCTION METER */
.gr-meter {
  width: 100%;
  height: 8px;
  background: #111;
  border-radius: 4px;
  overflow: hidden;
  margin-top: 4px;
}

.gr-meter-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--accent-green), var(--accent-yellow, #ffcc00), var(--accent-red));
  width: 0%;
  transition: width 0.05s;
  border-radius: 4px;
}

/* CHANNEL STRIP LAYOUT */
.channel-strip {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  padding: 8px;
}

.channel-strip .knob-container {
  min-width: 50px;
}

/* FILE I/O */
.file-input { display: none; }

.status-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 4px 12px;
  background: #0a0a0a;
  border-top: 1px solid #222;
  font-size: 10px;
  color: #555;
}

.status-bar .status-item {
  display: flex;
  align-items: center;
  gap: 5px;
}

.status-bar .indicator {
  width: 5px;
  height: 5px;
  border-radius: 50%;
}

.status-bar .indicator.green { background: var(--accent-green); }
.status-bar .indicator.amber { background: var(--accent-amber); }
.status-bar .indicator.red { background: var(--accent-red); }
</style>
</head>
<body>

<!-- HEADER -->
<header class="header">
  <div class="logo">
    <div class="logo-icon">🎛️</div>
    <div class="logo-text">
      <h1>OptiMod Pro 1600</h1>
      <span>Broadcast Audio Processor</span>
    </div>
  </div>
  <div class="header-controls">
    <select class="select-styled" id="modeSelect">
      <option value="streaming">📡 Streaming</option>
      <option value="radio">📻 Radio FM</option>
      <option value="mastering">🎵 Mastering</option>
      <option value="podcast">🎙️ Podcast</option>
    </select>
    <button class="menu-btn" id="presetsBtn">💾 Presets</button>
    <button class="menu-btn" id="configBtn">⚙️ Config</button>
    <button class="power-btn" id="powerBtn" title="Power ON/OFF">⏻</button>
  </div>
</header>

<!-- PRESETS BAR -->
<div class="presets-bar" id="presetsBar">
  <button class="preset-btn" data-preset="bypass">Bypass</button>
  <button class="preset-btn" data-preset="fm-hot">FM Hot</button>
  <button class="preset-btn" data-preset="fm-clean">FM Clean</button>
  <button class="preset-btn" data-preset="streaming">Streaming</button>
  <button class="preset-btn" data-preset="podcast">Podcast</button>
  <button class="preset-btn" data-preset="voice">Voice</button>
  <button class="preset-btn" data-preset="music">Music</button>
  <button class="preset-btn" data-preset="rock">Rock</button>
  <button class="preset-btn" data-preset="classical">Classical</button>
  <button class="preset-btn" data-preset="bass-boost">Bass Boost</button>
  <button class="preset-btn" data-preset="bright">Bright</button>
  <button class="menu-btn" id="exportBtn">📤 Export</button>
  <button class="menu-btn" id="importBtn">📥 Import</button>
  <input type="file" class="file-input" id="importFile" accept=".json">
</div>

<!-- MAIN CONTENT -->
<div class="main-container">

  <!-- TOP SECTION -->
  <div class="top-section">
    <!-- INPUT PANEL -->
    <div class="panel">
      <div class="panel-title">📥 Input</div>
      <div class="knobs-grid">
        <div class="knob-container">
          <div class="knob-wrapper" data-param="inputGain" data-min="-24" data-max="24" data-default="0">
            <div class="knob"></div>
          </div>
          <div class="knob-value">0.0 dB</div>
          <div class="knob-label">Gain</div>
        </div>
        <div class="knob-container">
          <div class="knob-wrapper" data-param="inputPan" data-min="-100" data-max="100" data-default="0">
            <div class="knob"></div>
          </div>
          <div class="knob-value">C</div>
          <div class="knob-label">Pan</div>
        </div>
        <div class="knob-container">
          <div class="knob-wrapper" data-param="inputPhase" data-min="0" data-max="1" data-default="0">
            <div class="knob small"></div>
          </div>
          <div class="knob-value">NOR</div>
          <div class="knob-label">Phase</div>
        </div>
      </div>
    </div>

    <!-- METERS -->
    <div class="panel">
      <div class="panel-title">📊 Level Meters</div>
      <div style="display:flex; gap:15px; justify-content:center;">
        <div>
          <div class="meter-container">
            <div class="meter"><div class="meter-fill" id="inputMeterL"></div><div class="meter-peak" id="inputPeakL"></div></div>
            <div class="meter"><div class="meter-fill" id="inputMeterR"></div><div class="meter-peak" id="inputPeakR"></div></div>
          </div>
          <div class="meter-label">INPUT</div>
        </div>
        <div>
          <div class="meter-container">
            <div class="meter"><div class="meter-fill" id="procMeterL"></div><div class="meter-peak" id="procPeakL"></div></div>
            <div class="meter"><div class="meter-fill" id="procMeterR"></div><div class="meter-peak" id="procPeakR"></div></div>
          </div>
          <div class="meter-label">PROCESSED</div>
        </div>
        <div>
          <div class="meter-container">
            <div class="meter"><div class="meter-fill" id="outputMeterL"></div><div class="meter-peak" id="outputPeakL"></div></div>
            <div class="meter"><div class="meter-fill" id="outputMeterR"></div><div class="meter-peak" id="outputPeakR"></div></div>
          </div>
          <div class="meter-label">OUTPUT</div>
        </div>
      </div>
    </div>

    <!-- OUTPUT PANEL -->
    <div class="panel">
      <div class="panel-title">📤 Output</div>
      <div class="knobs-grid">
        <div class="knob-container">
          <div class="knob-wrapper" data-param="outputGain" data-min="-24" data-max="12" data-default="0">
            <div class="knob"></div>
          </div>
          <div class="knob-value">0.0 dB</div>
          <div class="knob-label">Gain</div>
        </div>
        <div class="knob-container">
          <div class="knob-wrapper" data-param="outputLimit" data-min="0" data-max="100" data-default="100">
            <div class="knob"></div>
          </div>
          <div class="knob-value">100%</div>
          <div class="knob-label">Limit</div>
        </div>
        <div class="knob-container">
          <div class="knob-wrapper" data-param="stereoMode" data-min="0" data-max="2" data-default="0">
            <div class="knob small"></div>
          </div>
          <div class="knob-value">ST</div>
          <div class="knob-label">Stereo</div>
        </div>
      </div>
    </div>
  </div>

  <!-- SPECTRUM -->
  <div class="panel spectrum-panel">
    <div class="panel-title">📈 Spectrum Analyzer</div>
    <canvas class="spectrum-canvas" id="spectrumCanvas"></canvas>
  </div>

  <!-- EQ PANEL -->
  <div class="dsp-panel" id="eqPanel">
    <div class="dsp-panel-title">
      <span><span class="status-dot"></span>Ecualizador Paramétrico 6 Bandas</span>
      <button class="bypass-btn" data-bypass="eq">Bypass</button>
    </div>
    <canvas class="eq-display" id="eqCanvas"></canvas>
    <div class="knobs-grid" id="eqKnobs"></div>
  </div>

  <!-- AGC PANEL -->
  <div class="dsp-panel" id="agcPanel">
    <div class="dsp-panel-title">
      <span><span class="status-dot"></span>AGC - Automatic Gain Control</span>
      <button class="bypass-btn" data-bypass="agc">Bypass</button>
    </div>
    <div class="knobs-grid" id="agcKnobs"></div>
    <div class="gr-meter"><div class="gr-meter-fill" id="agcGrMeter"></div></div>
  </div>

  <!-- MULTIBAND COMPRESSOR (4 bands) -->
  <div class="dsp-panel" id="multiband4Panel">
    <div class="dsp-panel-title">
      <span><span class="status-dot"></span>Multiband Compressor (4 Bandas)</span>
      <button class="bypass-btn" data-bypass="multiband4">Bypass</button>
    </div>
    <div id="multiband4Knobs"></div>
  </div>

  <!-- 3-BAND COMPRESSOR -->
  <div class="dsp-panel" id="comp3Panel">
    <div class="dsp-panel-title">
      <span><span class="status-dot"></span>Compresor 3 Bandas</span>
      <button class="bypass-btn" data-bypass="comp3">Bypass</button>
    </div>
    <div id="comp3Knobs"></div>
  </div>

  <!-- BAND LIMITER 3 BANDAS -->
  <div class="dsp-panel" id="limiter3Panel">
    <div class="dsp-panel-title">
      <span><span class="status-dot"></span>Band Limiter 3 Bandas</span>
      <button class="bypass-btn" data-bypass="limiter3">Bypass</button>
    </div>
    <div id="limiter3Knobs"></div>
  </div>

  <!-- FINAL LIMITER -->
  <div class="dsp-panel" id="finalLimiterPanel">
    <div class="dsp-panel-title">
      <span><span class="status-dot"></span>Limiter Final</span>
      <button class="bypass-btn" data-bypass="finalLimiter">Bypass</button>
    </div>
    <div class="knobs-grid" id="finalLimiterKnobs"></div>
    <div class="gr-meter"><div class="gr-meter-fill" id="limGrMeter"></div></div>
  </div>

  <!-- POWER LIMITER -->
  <div class="dsp-panel" id="powerLimiterPanel">
    <div class="dsp-panel-title">
      <span><span class="status-dot"></span>Power Limiter</span>
      <button class="bypass-btn" data-bypass="powerLimiter">Bypass</button>
    </div>
    <div class="knobs-grid" id="powerLimiterKnobs"></div>
    <div class="gr-meter"><div class="gr-meter-fill" id="powLimGrMeter"></div></div>
  </div>

  <!-- PRE-EMPHASIS & STEREO -->
  <div class="dsp-row">
    <div class="dsp-panel" id="preEmphasisPanel">
      <div class="dsp-panel-title">
        <span><span class="status-dot"></span>Pre-Emphasis</span>
        <button class="bypass-btn" data-bypass="preEmphasis">Bypass</button>
      </div>
      <div class="knobs-grid">
        <div class="knob-container">
          <div class="knob-wrapper" data-param="preEmphasis" data-min="0" data-max="1" data-default="0">
            <div class="knob small"></div>
          </div>
          <div class="knob-value">50μs</div>
          <div class="knob-label">Time</div>
        </div>
        <div class="knob-container">
          <div class="knob-wrapper" data-param="preEmphasisGain" data-min="0" data-max="12" data-default="0">
            <div class="knob small"></div>
          </div>
          <div class="knob-value">0 dB</div>
          <div class="knob-label">Amount</div>
        </div>
      </div>
    </div>
    <div class="dsp-panel">
      <div class="dsp-panel-title"><span class="status-dot"></span>Stereo Mode</div>
      <div class="mode-selector" style="margin-bottom:10px;">
        <button class="mode-btn active" data-mode="stereo">Stereo</button>
        <button class="mode-btn" data-mode="mono">Mono</button>
        <button class="mode-btn" data-mode="mpx">MPX</button>
      </div>
      <div class="knobs-grid">
        <div class="knob-container">
          <div class="knob-wrapper" data-param="stereoWidth" data-min="0" data-max="200" data-default="100">
            <div class="knob"></div>
          </div>
          <div class="knob-value">100%</div>
          <div class="knob-label">Width</div>
        </div>
        <div class="knob-container">
          <div class="knob-wrapper" data-param="stereoBalance" data-min="-100" data-max="100" data-default="0">
            <div class="knob"></div>
          </div>
          <div class="knob-value">C</div>
          <div class="knob-label">Balance</div>
        </div>
      </div>
    </div>
  </div>

</div>

<!-- STATUS BAR -->
<div class="status-bar">
  <div class="status-item"><span class="indicator green"></span> DSP: <span id="dspStatus">Ready</span></div>
  <div class="status-item"><span class="indicator amber"></span> Sample Rate: <span id="sampleRateDisplay">--</span></div>
  <div class="status-item"><span class="indicator green"></span> Buffer: <span id="bufferSizeDisplay">--</span></div>
  <div class="status-item">Latency: <span id="latencyDisplay">--</span></div>
  <div class="status-item">CPU: <span id="cpuDisplay">--</span></div>
  <div class="status-item"><span class="indicator" id="clipIndicator"></span> Clip</div>
</div>

<!-- CONFIG OVERLAY -->
<div class="config-overlay" id="configOverlay">
  <div class="config-panel">
    <h2>⚙️ Configuración General</h2>
    <div class="config-section">
      <h3>🔊 Audio</h3>
      <div class="config-row">
        <label>Dispositivo Entrada</label>
        <select id="inputDevice" style="width:250px; height:20px;"></select>
      </div>
      <div class="config-row">
        <label>Dispositivo Salida</label>
        <select id="outputDevice" style="width:250px; height:20px;"></select>
      </div>
      <div class="config-row">
        <label>Sample Rate</label>
        <select id="configSampleRate">
          <option value="44100">44100 Hz</option>
          <option value="48000">48000 Hz</option>
          <option value="96000">96000 Hz</option>
        </select>
      </div>
      <div class="config-row">
        <label>Buffer Size</label>
        <select id="configBufferSize">
          <option value="128">128 samples</option>
          <option value="256" selected>256 samples</option>
          <option value="512">512 samples</option>
          <option value="1024">1024 samples</option>
        </select>
      </div>
    </div>
    <div class="config-section">
      <h3>📊 AGC</h3>
      <div class="config-row">
        <label>Modo AGC</label>
        <select id="agcMode">
          <option value="smooth">Smooth</option>
          <option value="aggressive">Aggressive</option>
          <option value="broadcast">Broadcast</option>
        </select>
      </div>
      <div class="config-row">
        <label>Lookahead (ms)</label>
        <input type="number" id="agcLookahead" value="2" min="0" max="20">
      </div>
    </div>
    <div class="config-section">
      <h3>🔧 DSP</h3>
      <div class="config-row">
        <label>Oversampling</label>
        <select id="oversampling">
          <option value="1">1x</option>
          <option value="2" selected>2x</option>
          <option value="4">4x</option>
        </select>
      </div>
      <div class="config-row">
        <label>Anti-Clipping</label>
        <select id="antiClipping">
          <option value="on" selected>On</option>
          <option value="off">Off</option>
        </select>
      </div>
      <div class="config-row">
        <label>Saturación Armónica</label>
        <select id="harmonicSat">
          <option value="0">Off</option>
          <option value="1" selected>Low</option>
          <option value="2">Medium</option>
          <option value="3">High</option>
        </select>
      </div>
    </div>
    <button class="config-close" id="configClose">Cerrar</button>
  </div>
</div>

<!-- TOOLTIP -->
<div class="tooltip" id="tooltip"></div>

<script>
// ============================================================
// OptiMod Pro 1600 - Broadcast Audio Processor
// Complete DSP Engine + UI (CORREGIDO)
// ============================================================

class BroadcastProcessor {
  constructor() {
    this.audioCtx = null;
    this.audioWorklet = null;
    this.inputStream = null;
    this.isEnabled = false;
    this.sampleRate = 48000;
    this.bufferSize = 256;
    this.params = {};
    this.bypassStates = {
      eq: false,
      agc: false,
      multiband4: false,
      comp3: false,
      limiter3: false,
      finalLimiter: false,
      powerLimiter: false,
      preEmphasis: false
    };
    this.meters = { inputL: 0, inputR: 0, outputL: 0, outputR: 0 };
    this.peaks = { inputL: 0, inputR: 0, outputL: 0, outputR: 0 };
    this.peakHold = { inputL: 0, inputR: 0, outputL: 0, outputR: 0 };
    this.spectrumData = new Float32Array(128);
    this.eqData = new Float32Array(128);
    this.clipped = false;
    this.cpuLoad = 0;
    this.lastFrameTime = performance.now();
    this.frameCount = 0;
    
    this.initDefaultParams();
    this.initUI();
    this.initKnobs();
    this.initSpectrum();
    this.initEQDisplay();
    this.initBypassButtons();
    this.loadPreset('fm-hot');
  }

  initDefaultParams() {
    // Input
    this.params.inputGain = 0;
    this.params.inputPan = 0;
    this.params.inputPhase = 0;
    
    // 6-Band EQ
    this.params.eq = [];
    const eqDefaults = [
      { type: 'highpass', freq: 40, gain: 0, q: 0.7 },
      { type: 'bell', freq: 100, gain: 0, q: 1.0 },
      { type: 'bell', freq: 400, gain: 0, q: 1.0 },
      { type: 'bell', freq: 1000, gain: 0, q: 1.0 },
      { type: 'bell', freq: 4000, gain: 0, q: 1.0 },
      { type: 'highshelf', freq: 10000, gain: 0, q: 0.7 }
    ];
    eqDefaults.forEach((d, i) => {
      this.params.eq.push({
        id: i,
        type: d.type,
        freq: d.freq,
        gain: d.gain,
        q: d.q,
        enabled: true
      });
    });

    // AGC
    this.params.agc = { threshold: -20, ratio: 3, attack: 10, release: 100, enabled: true };
    
    // Multiband 4
    this.params.multiband4 = [
      { name: 'Sub', freq: 80, threshold: -20, ratio: 4, attack: 15, release: 80, gain: 0, enabled: true },
      { name: 'Low', freq: 250, threshold: -18, ratio: 3, attack: 10, release: 60, gain: 0, enabled: true },
      { name: 'Mid', freq: 2500, threshold: -16, ratio: 3, attack: 8, release: 50, gain: 0, enabled: true },
      { name: 'High', freq: 8000, threshold: -14, ratio: 2.5, attack: 5, release: 40, gain: 0, enabled: true }
    ];

    // 3-Band Compressor
    this.params.comp3 = [
      { name: 'Low', freq: 200, threshold: -20, ratio: 4, attack: 15, release: 80, gain: 0, enabled: true },
      { name: 'Mid', freq: 2000, threshold: -18, ratio: 3.5, attack: 10, release: 60, gain: 0, enabled: true },
      { name: 'High', freq: 8000, threshold: -15, ratio: 3, attack: 5, release: 40, gain: 0, enabled: true }
    ];

    // 3-Band Limiter
    this.params.limiter3 = [
      { name: 'Low', freq: 200, threshold: -3, ratio: 20, attack: 1, release: 50, gain: 0, enabled: true },
      { name: 'Mid', freq: 2000, threshold: -3, ratio: 20, attack: 1, release: 40, gain: 0, enabled: true },
      { name: 'High', freq: 8000, threshold: -3, ratio: 20, attack: 0.5, release: 30, gain: 0, enabled: true }
    ];

    // Final Limiter
    this.params.finalLimiter = { threshold: -1, ratio: 20, attack: 0.5, release: 50, gain: 0, enabled: true };
    
    // Power Limiter
    this.params.powerLimiter = { threshold: -0.5, ratio: 30, attack: 0.1, release: 20, gain: 0, enabled: true };

    // Pre-Emphasis
    this.params.preEmphasis = 0;
    this.params.preEmphasisGain = 0;

    // Output
    this.params.outputGain = 0;
    this.params.outputLimit = 100;
    this.params.stereoMode = 0;
    this.params.stereoWidth = 100;
    this.params.stereoBalance = 0;

    // Mode
    this.params.mode = 'streaming';
    
    // Devices
    this.params.inputDevice = localStorage.getItem('inputDevice') || 'default';
    this.params.outputDevice = localStorage.getItem('outputDevice') || 'default';
  }

  async initAudio() {
    try {
      this.audioCtx = new (window.AudioContext || window.webkitAudioContext)({
        sampleRate: this.sampleRate,
        latencyHint: 'interactive'
      });

      document.getElementById('sampleRateDisplay').textContent = this.audioCtx.sampleRate + ' Hz';
      document.getElementById('bufferSizeDisplay').textContent = this.bufferSize + ' samples';

      const workletCode = this.generateWorkletCode();
      const blob = new Blob([workletCode], { type: 'application/javascript' });
      const workletUrl = URL.createObjectURL(blob);

      await this.audioCtx.audioWorklet.addModule(workletUrl);
      URL.revokeObjectURL(workletUrl);

      this.audioWorklet = new AudioWorkletNode(this.audioCtx, 'broadcast-processor', {
        outputChannelCount: [2],
        processorOptions: { sampleRate: this.audioCtx.sampleRate }
      });

      this.audioWorklet.port.onmessage = (e) => {
        if (e.data.type === 'meters') {
          this.meters = e.data.meters;
          this.updateMeters();
        }
        if (e.data.type === 'spectrum') {
          this.spectrumData = new Float32Array(e.data.spectrum);
          this.drawSpectrum();
        }
        if (e.data.type === 'eqcurve') {
          this.eqData = new Float32Array(e.data.eqcurve);
          this.drawEQCurve();
        }
        if (e.data.type === 'gr') {
          this.updateGRMeters(e.data.gr);
        }
        if (e.data.type === 'clip') {
          this.clipped = true;
          setTimeout(() => this.clipped = false, 200);
          document.getElementById('clipIndicator').style.background = 'var(--accent-red)';
          document.getElementById('clipIndicator').style.boxShadow = '0 0 6px rgba(255,0,0,0.8)';
        }
        if (e.data.type === 'cpu') {
          this.cpuLoad = e.data.cpu;
          document.getElementById('cpuDisplay').textContent = this.cpuLoad.toFixed(1) + '%';
        }
      };

      const constraints = {
        audio: {
          echoCancellation: false,
          noiseSuppression: false,
          autoGainControl: false,
          sampleRate: this.sampleRate
        }
      };
      if (this.params.inputDevice && this.params.inputDevice !== 'default') {
        constraints.audio.deviceId = { exact: this.params.inputDevice };
      }
      this.inputStream = await navigator.mediaDevices.getUserMedia(constraints);

      const source = this.audioCtx.createMediaStreamSource(this.inputStream);
      
      if (this.params.outputDevice && this.params.outputDevice !== 'default' && this.audioCtx.setSinkId) {
        await this.audioCtx.setSinkId(this.params.outputDevice);
      }

      source.connect(this.audioWorklet);
      this.audioWorklet.connect(this.audioCtx.destination);

      this.isEnabled = true;
      document.getElementById('dspStatus').textContent = 'Active';
      document.getElementById('powerBtn').classList.add('active');
      
      this.sendAllParams();
      this.sendBypassStates();

    } catch (err) {
      console.error('Audio init error:', err);
      document.getElementById('dspStatus').textContent = 'Error: ' + err.message;
    }
  }

  generateWorkletCode() {
    return `
    class BroadcastProcessor extends AudioWorkletProcessor {
      constructor(options) {
        super();
        this.sampleRate = options.sampleRate || 48000;
        this.blockSize = 128;
        
        // DSP State
        this.params = {};
        this.inputGain = 1.0;
        this.inputPan = 0.0;
        this.outputGain = 1.0;
        this.stereoWidth = 1.0;
        this.stereoBalance = 0.0;
        this.stereoMode = 0;
        
        // Bypass states
        this.bypass = {
          eq: false,
          agc: false,
          multiband4: false,
          comp3: false,
          limiter3: false,
          finalLimiter: false,
          powerLimiter: false,
          preEmphasis: false
        };
        
        // EQ State (6 bands)
        this.eqFilters = [];
        this.initEQ();
        
        // AGC State
        this.agcState = { threshold: -20, ratio: 3, attack: 10, release: 100, enabled: true };
        this.agcEnvelope = 0;
        this.agcGain = 1.0;
        
        // Multiband 4 State
        this.multiband4 = [
          { freq: 80, threshold: -20, ratio: 4, attack: 15, release: 80, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 250, threshold: -18, ratio: 3, attack: 10, release: 60, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 2500, threshold: -16, ratio: 3, attack: 8, release: 50, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 8000, threshold: -14, ratio: 2.5, attack: 5, release: 40, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null }
        ];
        this.initCrossovers4();
        
        // 3-Band Compressor
        this.comp3 = [
          { freq: 200, threshold: -20, ratio: 4, attack: 15, release: 80, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 2000, threshold: -18, ratio: 3.5, attack: 10, release: 60, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 8000, threshold: -15, ratio: 3, attack: 5, release: 40, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null }
        ];
        this.initCrossovers3('comp3');
        
        // 3-Band Limiter
        this.limiter3 = [
          { freq: 200, threshold: -3, ratio: 20, attack: 1, release: 50, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 2000, threshold: -3, ratio: 20, attack: 1, release: 40, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 8000, threshold: -3, ratio: 20, attack: 0.5, release: 30, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null }
        ];
        this.initCrossovers3('limiter3');
        
        // Final Limiter
        this.finalLimiter = { threshold: -1, ratio: 20, attack: 0.5, release: 50, gain: 0, envelope: 0, g: 1.0 };
        
        // Power Limiter
        this.powerLimiter = { threshold: -0.5, ratio: 30, attack: 0.1, release: 20, gain: 0, envelope: 0, g: 1.0 };
        
        // Pre-emphasis
        this.preEmphasis = 0;
        this.preEmphasisGain = 0;
        this.preEmphState = 0;
        
        // Meters
        this.meterInputL = 0, this.meterInputR = 0;
        this.meterOutputL = 0, this.meterOutputR = 0;
        this.meterPeakIL = 0, this.meterPeakIR = 0;
        this.meterPeakOL = 0, this.meterPeakOR = 0;
        this.meterCounter = 0;
        
        // Spectrum
        this.spectrumBuffer = new Float32Array(256);
        this.spectrumIdx = 0;
        
        // Saturation
        this.saturationAmount = 0.1;
        
        // Anti-clip
        this.antiClipEnabled = true;
        
        // Frame counter
        this.meterUpdateInterval = Math.floor(this.sampleRate / 30);
        this.frameCounter = 0;
        
        // Parameter smoothing
        this.smoothedParams = {};
        
        this.port.onmessage = (e) => this.handleMessage(e.data);
      }
      
      handleMessage(data) {
        switch(data.type) {
          case 'param':
            this.setParam(data.key, data.value);
            break;
          case 'eq':
            this.setEQ(data.index, data.param, data.value);
            break;
          case 'agc':
            Object.assign(this.agcState, data.params);
            break;
          case 'multiband4':
            if (this.multiband4[data.index]) {
              Object.assign(this.multiband4[data.index], data.params);
              this.initCrossovers4();
            }
            break;
          case 'comp3':
            if (this.comp3[data.index]) {
              Object.assign(this.comp3[data.index], data.params);
              this.initCrossovers3('comp3');
            }
            break;
          case 'limiter3':
            if (this.limiter3[data.index]) {
              Object.assign(this.limiter3[data.index], data.params);
              this.initCrossovers3('limiter3');
            }
            break;
          case 'finalLimiter':
            Object.assign(this.finalLimiter, data.params);
            break;
          case 'powerLimiter':
            Object.assign(this.powerLimiter, data.params);
            break;
          case 'preEmphasis':
            this.preEmphasis = data.value;
            break;
          case 'preEmphasisGain':
            this.preEmphasisGain = data.value;
            break;
          case 'bypass':
            if (data.module in this.bypass) {
              this.bypass[data.module] = data.enabled;
            }
            break;
        }
      }
      
      setParam(key, value) {
        switch(key) {
          case 'inputGain': this.inputGain = Math.pow(10, value / 20); break;
          case 'inputPan': this.inputPan = value / 100; break;
          case 'outputGain': this.outputGain = Math.pow(10, value / 20); break;
          case 'outputLimit': this.outputLimit = value / 100; break;
          case 'stereoMode': this.stereoMode = value; break;
          case 'stereoWidth': this.stereoWidth = value / 100; break;
          case 'stereoBalance': this.stereoBalance = value / 100; break;
          case 'saturation': this.saturationAmount = value; break;
        }
      }
      
      initEQ() {
        this.eqFilters = [];
        for (let i = 0; i < 6; i++) {
          this.eqFilters.push({
            type: 'bell', freq: 1000, gain: 0, q: 1.0, enabled: true,
            b0: 0, b1: 0, b2: 0, a1: 0, a2: 0,
            x1L: 0, x2L: 0, y1L: 0, y2L: 0,
            x1R: 0, x2R: 0, y1R: 0, y2R: 0
          });
        }
      }
      
      setEQ(index, param, value) {
        if (!this.eqFilters[index]) return;
        const eq = this.eqFilters[index];
        eq[param] = value;
        if (param === 'freq' || param === 'gain' || param === 'q' || param === 'type') {
          this.updateEQCoeffs(index);
        }
      }
      
      updateEQCoeffs(index) {
        const eq = this.eqFilters[index];
        const sr = this.sampleRate;
        const freq = Math.max(20, Math.min(20000, eq.freq));
        const gain = eq.gain;
        const q = Math.max(0.1, eq.q);
        const A = Math.pow(10, gain / 40);
        const omega = 2 * Math.PI * freq / sr;
        const sinW = Math.sin(omega);
        const cosW = Math.cos(omega);
        const alpha = sinW / (2 * q);
        const sqrtA = Math.sqrt(A);
        
        let b0, b1, b2, a0, a1, a2;
        
        switch(eq.type) {
          case 'highpass':
            b0 = (1 + cosW) / 2;
            b1 = -(1 + cosW);
            b2 = (1 + cosW) / 2;
            a0 = 1 + alpha;
            a1 = -2 * cosW;
            a2 = 1 - alpha;
            break;
          case 'lowpass':
            b0 = (1 - cosW) / 2;
            b1 = 1 - cosW;
            b2 = (1 - cosW) / 2;
            a0 = 1 + alpha;
            a1 = -2 * cosW;
            a2 = 1 - alpha;
            break;
          case 'highshelf':
            b0 = A * ((A + 1) + (A - 1) * cosW + 2 * sqrtA * alpha);
            b1 = -2 * A * ((A - 1) + (A + 1) * cosW);
            b2 = A * ((A + 1) + (A - 1) * cosW - 2 * sqrtA * alpha);
            a0 = (A + 1) - (A - 1) * cosW + 2 * sqrtA * alpha;
            a1 = 2 * ((A - 1) - (A + 1) * cosW);
            a2 = (A + 1) - (A - 1) * cosW - 2 * sqrtA * alpha;
            break;
          case 'lowshelf':
            b0 = A * ((A + 1) - (A - 1) * cosW + 2 * sqrtA * alpha);
            b1 = 2 * A * ((A - 1) - (A + 1) * cosW);
            b2 = A * ((A + 1) - (A - 1) * cosW - 2 * sqrtA * alpha);
            a0 = (A + 1) + (A - 1) * cosW + 2 * sqrtA * alpha;
            a1 = -2 * ((A - 1) + (A + 1) * cosW);
            a2 = (A + 1) + (A - 1) * cosW - 2 * sqrtA * alpha;
            break;
          default:
            b0 = 1 + alpha * A;
            b1 = -2 * cosW;
            b2 = 1 - alpha * A;
            a0 = 1 + alpha / A;
            a1 = -2 * cosW;
            a2 = 1 - alpha / A;
            break;
        }
        
        eq.b0 = b0 / a0;
        eq.b1 = b1 / a0;
        eq.b2 = b2 / a0;
        eq.a1 = a1 / a0;
        eq.a2 = a2 / a0;
      }
      
      processBiquad(eq, xL, xR) {
        if (!eq.enabled) return { yL: xL, yR: xR };
        
        const yL = eq.b0 * xL + eq.b1 * eq.x1L + eq.b2 * eq.x2L - eq.a1 * eq.y1L - eq.a2 * eq.y2L;
        const yR = eq.b0 * xR + eq.b1 * eq.x1R + eq.b2 * eq.x2R - eq.a1 * eq.y1R - eq.a2 * eq.y2R;
        
        eq.x2L = eq.x1L; eq.x1L = xL;
        eq.y2L = eq.y1L; eq.y1L = yL;
        eq.x2R = eq.x1R; eq.x1R = xR;
        eq.y2R = eq.y1R; eq.y1R = yR;
        
        return { yL, yR };
      }
      
      initCrossovers4() {
        const freqs = this.multiband4.map(b => b.freq);
        for (let i = 0; i < this.multiband4.length; i++) {
          const b = this.multiband4[i];
          const lp = this.createFilter('lowpass', freqs[i], 2);
          const hp = this.createFilter('highpass', freqs[i], 2);
          b.lowFilter = lp;
          b.highFilter = hp;
        }
      }
      
      initCrossovers3(which) {
        const bands = this[which];
        const freqs = bands.map(b => b.freq);
        for (let i = 0; i < bands.length; i++) {
          const b = bands[i];
          const lp = this.createFilter('lowpass', freqs[i], 2);
          const hp = this.createFilter('highpass', freqs[i], 2);
          b.lowFilter = lp;
          b.highFilter = hp;
        }
      }
      
      createFilter(type, freq, order) {
        const rc = 1.0 / (2 * Math.PI * freq);
        const dt = 1.0 / this.sampleRate;
        const alpha = dt / (rc + dt);
        return { type: type, alpha: alpha, stateL: 0, stateR: 0 };
      }
      
      processFilter(filter, xL, xR) {
        if (!filter) return { yL: xL, yR: xR };
        
        if (filter.type === 'lowpass') {
          const yL = filter.stateL + filter.alpha * (xL - filter.stateL);
          const yR = filter.stateR + filter.alpha * (xR - filter.stateR);
          filter.stateL = yL;
          filter.stateR = yR;
          return { yL, yR };
        } else {
          const lpL = filter.stateL + filter.alpha * (xL - filter.stateL);
          const lpR = filter.stateR + filter.alpha * (xR - filter.stateR);
          filter.stateL = lpL;
          filter.stateR = lpR;
          return { yL: xL - lpL, yR: xR - lpR };
        }
      }
      
      compressSample(signal, threshold, ratio, attack, release, envelope, gain) {
        const absSig = Math.abs(signal);
        const attackCoeff = Math.exp(-1 / (attack * 0.001 * this.sampleRate));
        const releaseCoeff = Math.exp(-1 / (release * 0.001 * this.sampleRate));
        
        if (absSig > envelope) {
          envelope = attackCoeff * envelope + (1 - attackCoeff) * absSig;
        } else {
          envelope = releaseCoeff * envelope + (1 - releaseCoeff) * absSig;
        }
        
        const linThresh = Math.pow(10, threshold / 20);
        if (envelope > linThresh) {
          const overshoot = envelope / linThresh;
          const logOvershoot = Math.log10(overshoot);
          const reduction = Math.pow(10, -logOvershoot * (1 - 1/ratio));
          gain = Math.min(gain, reduction);
        } else {
          const releaseCoeffSlow = Math.exp(-1 / (release * 3 * 0.001 * this.sampleRate));
          gain = Math.min(1.0, gain + (1 - gain) * (1 - releaseCoeffSlow));
        }
        
        return { envelope, gain, output: signal * gain };
      }
      
      applySaturation(x) {
        if (this.saturationAmount <= 0) return x;
        const sat = this.saturationAmount;
        return Math.tanh(x * (1 + sat)) / Math.tanh(1 + sat);
      }
      
      processPreEmphasis(x) {
        if (this.preEmphasisGain <= 0) return x;
        const tau = this.preEmphasis === 0 ? 50e-6 : 75e-6;
        const dt = 1 / this.sampleRate;
        const alpha = dt / (tau + dt);
        const result = x + this.preEmphasisGain * (x - this.preEmphState) * alpha;
        this.preEmphState = x;
        return result;
      }
      
      process(inputs, outputs, parameters) {
        const input = inputs[0];
        const output = outputs[0];
        
        if (!input || !input[0] || !output || !output[0]) return true;
        
        const inputL = input[0];
        const inputR = input.length > 1 ? input[1] : input[0];
        const outputL = output[0];
        const outputR = output.length > 1 ? output[1] : output[0];
        const blockSize = inputL.length;
        
        let clipDetected = false;
        let peakIL = 0, peakIR = 0, peakOL = 0, peakOR = 0;
        let rmsIL = 0, rmsIR = 0, rmsOL = 0, rmsOR = 0;
        
        for (let i = 0; i < blockSize; i++) {
          let sL = inputL[i];
          let sR = inputR[i];
          
          // Input level tracking
          peakIL = Math.max(peakIL, Math.abs(sL));
          peakIR = Math.max(peakIR, Math.abs(sR));
          rmsIL += sL * sL;
          rmsIR += sR * sR;
          
          // 1. Input gain
          sL *= this.inputGain;
          sR *= this.inputGain;
          
          // Pan - CORREGIDO: usar valores originales para ambos canales
          if (this.inputPan !== 0) {
            const panNorm = (this.inputPan + 1) * 0.5;
            const panL = Math.cos(panNorm * Math.PI / 2);
            const panR = Math.sin(panNorm * Math.PI / 2);
            const origL = inputL[i] * this.inputGain;
            const origR = inputR[i] * this.inputGain;
            sL = origL * panL + origR * (1 - panL);
            sR = origR * panR + origL * (1 - panR);
          }
          
          // 2. EQ (6 bands) - solo si NO está en bypass
          if (!this.bypass.eq) {
            for (let b = 0; b < this.eqFilters.length; b++) {
              const eq = this.eqFilters[b];
              if (eq.enabled) {
                const result = this.processBiquad(eq, sL, sR);
                sL = result.yL;
                sR = result.yR;
              }
            }
          }
          
          // Saturation
          if (this.saturationAmount > 0) {
            sL = this.applySaturation(sL);
            sR = this.applySaturation(sR);
          }
          
          // 3. AGC - solo si NO está en bypass
          if (!this.bypass.agc && this.agcState.enabled) {
            const agc = this.agcState;
            const mixed = (sL + sR) / 2;
            const agcResult = this.compressSample(mixed, agc.threshold, agc.ratio, agc.attack, agc.release, this.agcEnvelope, this.agcGain);
            this.agcEnvelope = agcResult.envelope;
            this.agcGain = agcResult.gain;
            sL *= this.agcGain;
            sR *= this.agcGain;
          }
          
          // 4. Multiband Compressor (4 bands) - solo si NO está en bypass
          if (!this.bypass.multiband4) {
            for (let b = 0; b < this.multiband4.length; b++) {
              const band = this.multiband4[b];
              if (band.enabled && band.lowFilter && band.highFilter) {
                let lo = this.processFilter(band.lowFilter, sL, sR);
                let hi = this.processFilter(band.highFilter, lo.yL, lo.yR);
                
                const mixed = (hi.yL + hi.yR) / 2;
                const result = this.compressSample(mixed, band.threshold, band.ratio, band.attack, band.release, band.envelope, band.g);
                band.envelope = result.envelope;
                band.g = result.gain;
                
                const gainFactor = band.g * Math.pow(10, band.gain / 20);
                sL = lo.yL + hi.yL * gainFactor;
                sR = lo.yR + hi.yR * gainFactor;
              }
            }
          }
          
          // 5. 3-Band Compressor - solo si NO está en bypass
          if (!this.bypass.comp3) {
            for (let b = 0; b < this.comp3.length; b++) {
              const band = this.comp3[b];
              if (band.enabled && band.lowFilter && band.highFilter) {
                let lo = this.processFilter(band.lowFilter, sL, sR);
                let hi = this.processFilter(band.highFilter, lo.yL, lo.yR);
                
                const mixed = (hi.yL + hi.yR) / 2;
                const result = this.compressSample(mixed, band.threshold, band.ratio, band.attack, band.release, band.envelope, band.g);
                band.envelope = result.envelope;
                band.g = result.gain;
                
                const gainFactor = band.g * Math.pow(10, band.gain / 20);
                sL = lo.yL + hi.yL * gainFactor;
                sR = lo.yR + hi.yR * gainFactor;
              }
            }
          }
          
          // 6. 3-Band Limiter - solo si NO está en bypass
          if (!this.bypass.limiter3) {
            for (let b = 0; b < this.limiter3.length; b++) {
              const band = this.limiter3[b];
              if (band.enabled && band.lowFilter && band.highFilter) {
                let lo = this.processFilter(band.lowFilter, sL, sR);
                let hi = this.processFilter(band.highFilter, lo.yL, lo.yR);
                
                const mixed = (hi.yL + hi.yR) / 2;
                const result = this.compressSample(mixed, band.threshold, band.ratio, band.attack, band.release, band.envelope, band.g);
                band.envelope = result.envelope;
                band.g = result.gain;
                
                const gainFactor = band.g * Math.pow(10, band.gain / 20);
                sL = lo.yL + hi.yL * gainFactor;
                sR = lo.yR + hi.yR * gainFactor;
              }
            }
          }
          
          // 7. Final Limiter - solo si NO está en bypass
          if (!this.bypass.finalLimiter && this.finalLimiter.enabled) {
            const fl = this.finalLimiter;
            const mixed = (sL + sR) / 2;
            const result = this.compressSample(mixed, fl.threshold, fl.ratio, fl.attack, fl.release, fl.envelope, fl.g);
            fl.envelope = result.envelope;
            fl.g = result.gain;
            const gainFactor = fl.g * Math.pow(10, fl.gain / 20);
            sL *= gainFactor;
            sR *= gainFactor;
          }
          
          // 8. Power Limiter - solo si NO está en bypass
          if (!this.bypass.powerLimiter && this.powerLimiter.enabled) {
            const pl = this.powerLimiter;
            const mixed = (sL + sR) / 2;
            const result = this.compressSample(mixed, pl.threshold, pl.ratio, pl.attack, pl.release, pl.envelope, pl.g);
            pl.envelope = result.envelope;
            pl.g = result.gain;
            const gainFactor = pl.g * Math.pow(10, pl.gain / 20);
            sL *= gainFactor;
            sR *= gainFactor;
          }
          
          // Anti-clipping con soft-knee
          if (this.antiClipEnabled) {
            const maxSample = Math.max(Math.abs(sL), Math.abs(sR));
            if (maxSample > 0.95) {
              const knee = 0.05;
              const over = maxSample - 0.95;
              const reduction = over < knee 
                ? 1 - (over / knee) * 0.5 
                : 1 / maxSample;
              sL *= reduction;
              sR *= reduction;
              if (maxSample > 1.0) clipDetected = true;
            }
          }
          
          // Pre-emphasis - solo si NO está en bypass
          if (!this.bypass.preEmphasis && this.preEmphasisGain > 0) {
            sL = this.processPreEmphasis(sL);
            sR = this.processPreEmphasis(sR);
          }
          
          // Stereo processing
          if (this.stereoMode === 1) {
            const mid = (sL + sR) / 2;
            sL = mid;
            sR = mid;
          } else if (this.stereoMode === 2) {
            const mid = (sL + sR) / 2;
            const side = (sL - sR) / 2;
            const width = this.stereoWidth;
            sL = mid + side * width;
            sR = mid - side * width;
          } else {
            const mid = (sL + sR) / 2;
            const side = (sL - sR) / 2;
            sL = mid + side * this.stereoWidth;
            sR = mid - side * this.stereoWidth;
          }
          
          // Balance
          if (this.stereoBalance !== 0) {
            if (this.stereoBalance > 0) {
              sR *= (1 - this.stereoBalance);
            } else {
              sL *= (1 + this.stereoBalance);
            }
          }
          
          // Output gain
          sL *= this.outputGain;
          sR *= this.outputGain;
          
          // Output limiting
          sL *= this.outputLimit;
          sR *= this.outputLimit;
          
          // Final hard clip prevention
          sL = Math.max(-1, Math.min(1, sL));
          sR = Math.max(-1, Math.min(1, sR));
          
          // Output level tracking
          peakOL = Math.max(peakOL, Math.abs(sL));
          peakOR = Math.max(peakOR, Math.abs(sR));
          rmsOL += sL * sL;
          rmsOR += sR * sR;
          
          // Spectrum buffer
          if (this.spectrumIdx < this.spectrumBuffer.length) {
            this.spectrumBuffer[this.spectrumIdx] = sL;
            this.spectrumIdx++;
          }
          
          outputL[i] = sL;
          outputR[i] = sR;
        }
        
        // Update meters periodically
        this.frameCounter += blockSize;
        if (this.frameCounter >= this.meterUpdateInterval) {
          this.frameCounter = 0;
          
          const rmsToDb = (rms, n) => {
            const val = Math.sqrt(rms / n);
            return 20 * Math.log10(Math.max(val, 1e-10));
          };
          
          this.meterInputL = rmsToDb(rmsIL, blockSize);
          this.meterInputR = rmsToDb(rmsIR, blockSize);
          this.meterOutputL = rmsToDb(rmsOL, blockSize);
          this.meterOutputR = rmsToDb(rmsOR, blockSize);
          
          this.meterPeakIL = 20 * Math.log10(Math.max(peakIL, 1e-10));
          this.meterPeakIR = 20 * Math.log10(Math.max(peakIR, 1e-10));
          this.meterPeakOL = 20 * Math.log10(Math.max(peakOL, 1e-10));
          this.meterPeakOR = 20 * Math.log10(Math.max(peakOR, 1e-10));
          
          this.port.postMessage({
            type: 'meters',
            meters: {
              inputL: this.meterInputL,
              inputR: this.meterInputR,
              outputL: this.meterOutputL,
              outputR: this.meterOutputR
            },
            peaks: {
              inputL: this.meterPeakIL,
              inputR: this.meterPeakIR,
              outputL: this.meterPeakOL,
              outputR: this.meterPeakOR
            }
          });
          
          if (this.spectrumIdx > 0) {
            const spec = this.computeSpectrum(this.spectrumBuffer.slice(0, this.spectrumIdx));
            this.spectrumIdx = 0;
            this.port.postMessage({ type: 'spectrum', spectrum: spec });
          }
          
          this.port.postMessage({ type: 'eqcurve', eqcurve: this.computeEQCurve() });
          
          this.port.postMessage({
            type: 'gr',
            gr: {
              agc: this.agcState.enabled ? 20 * Math.log10(Math.max(this.agcGain, 1e-10)) : 0,
              finalLimiter: this.finalLimiter.enabled ? 20 * Math.log10(Math.max(this.finalLimiter.g, 1e-10)) : 0,
              powerLimiter: this.powerLimiter.enabled ? 20 * Math.log10(Math.max(this.powerLimiter.g, 1e-10)) : 0
            }
          });
          
          if (clipDetected) {
            this.port.postMessage({ type: 'clip' });
          }
        }
        
        return true;
      }
      
      computeSpectrum(buffer) {
        const N = buffer.length;
        const result = new Float32Array(128);
        const bands = 128;
        for (let b = 0; b < bands; b++) {
          const freq = 20 * Math.pow(1000, b / bands);
          const k = Math.round(freq * N / this.sampleRate);
          if (k < 1 || k >= N/2) continue;
          
          let re = 0, im = 0;
          const omega = 2 * Math.PI * k / N;
          for (let i = 0; i < N; i++) {
            re += buffer[i] * Math.cos(omega * i);
            im -= buffer[i] * Math.sin(omega * i);
          }
          const energy = Math.sqrt(re * re + im * im) / N;
          result[b] = 20 * Math.log10(Math.max(energy, 1e-10));
        }
        return result;
      }
      
      computeEQCurve() {
        const points = 128;
        const curve = new Float32Array(points);
        const sr = this.sampleRate;
        
        for (let i = 0; i < points; i++) {
          const freq = 20 * Math.pow(1000, i / points);
          let response = 0;
          
          for (const eq of this.eqFilters) {
            if (!eq.enabled) continue;
            const omega = 2 * Math.PI * freq / sr;
            const cosW = Math.cos(omega);
            const sinW = Math.sin(omega);
            const cos2W = Math.cos(2 * omega);
            const sin2W = Math.sin(2 * omega);
            
            const numRe = eq.b0 + eq.b1 * cosW + eq.b2 * cos2W;
            const numIm = eq.b1 * sinW + eq.b2 * sin2W;
            const denRe = 1 + eq.a1 * cosW + eq.a2 * cos2W;
            const denIm = eq.a1 * sinW + eq.a2 * sin2W;
            
            const mag = Math.sqrt((numRe*numRe + numIm*numIm) / (denRe*denRe + denIm*denIm));
            response += 20 * Math.log10(Math.max(mag, 1e-10));
          }
          curve[i] = response;
        }
        return curve;
      }
    }
    
    registerProcessor('broadcast-processor', BroadcastProcessor);
    `;
  }

  async togglePower() {
    if (!this.isEnabled) {
      await this.initAudio();
    } else {
      this.audioCtx.close();
      this.isEnabled = false;
      document.getElementById('dspStatus').textContent = 'Bypass';
      document.getElementById('powerBtn').classList.remove('active');
    }
  }

  sendAllParams() {
    if (!this.audioWorklet) return;
    
    const p = this.params;
    const port = this.audioWorklet.port;
    
    Object.keys(p).forEach(key => {
      if (key === 'eq') {
        p.eq.forEach((eq, i) => {
          port.postMessage({ type: 'eq', index: i, param: 'type', value: eq.type });
          port.postMessage({ type: 'eq', index: i, param: 'freq', value: eq.freq });
          port.postMessage({ type: 'eq', index: i, param: 'gain', value: eq.gain });
          port.postMessage({ type: 'eq', index: i, param: 'q', value: eq.q });
          port.postMessage({ type: 'eq', index: i, param: 'enabled', value: eq.enabled });
        });
      } else if (key === 'agc') {
        port.postMessage({ type: 'agc', params: p.agc });
      } else if (key === 'multiband4') {
        p.multiband4.forEach((band, i) => {
          port.postMessage({ type: 'multiband4', index: i, params: band });
        });
      } else if (key === 'comp3') {
        p.comp3.forEach((band, i) => {
          port.postMessage({ type: 'comp3', index: i, params: band });
        });
      } else if (key === 'limiter3') {
        p.limiter3.forEach((band, i) => {
          port.postMessage({ type: 'limiter3', index: i, params: band });
        });
      } else if (key === 'finalLimiter') {
        port.postMessage({ type: 'finalLimiter', params: p.finalLimiter });
      } else if (key === 'powerLimiter') {
        port.postMessage({ type: 'powerLimiter', params: p.powerLimiter });
      } else if (key === 'preEmphasis') {
        port.postMessage({ type: 'preEmphasis', value: p[key] });
      } else if (key === 'preEmphasisGain') {
        port.postMessage({ type: 'preEmphasisGain', value: p[key] });
      } else {
        port.postMessage({ type: 'param', key: key, value: p[key] });
      }
    });
  }

  sendBypassStates() {
    if (!this.audioWorklet) return;
    const port = this.audioWorklet.port;
    Object.entries(this.bypassStates).forEach(([module, enabled]) => {
      port.postMessage({ type: 'bypass', module, enabled });
    });
  }

  sendParam(key, value) {
    if (!this.audioWorklet) return;
    this.params[key] = value;
    this.audioWorklet.port.postMessage({ type: 'param', key: key, value: value });
  }

  sendEQ(index, param, value) {
    if (!this.audioWorklet) return;
    if (this.params.eq[index]) {
      this.params.eq[index][param] = value;
      this.audioWorklet.port.postMessage({ type: 'eq', index, param, value });
    }
  }

  sendMultiband4(index, params) {
    if (!this.audioWorklet) return;
    Object.assign(this.params.multiband4[index], params);
    this.audioWorklet.port.postMessage({ type: 'multiband4', index, params });
  }

  sendComp3(index, params) {
    if (!this.audioWorklet) return;
    Object.assign(this.params.comp3[index], params);
    this.audioWorklet.port.postMessage({ type: 'comp3', index, params });
  }

  sendLimiter3(index, params) {
    if (!this.audioWorklet) return;
    Object.assign(this.params.limiter3[index], params);
    this.audioWorklet.port.postMessage({ type: 'limiter3', index, params });
  }

  sendFinalLimiter(params) {
    if (!this.audioWorklet) return;
    Object.assign(this.params.finalLimiter, params);
    this.audioWorklet.port.postMessage({ type: 'finalLimiter', params });
  }

  sendPowerLimiter(params) {
    if (!this.audioWorklet) return;
    Object.assign(this.params.powerLimiter, params);
    this.audioWorklet.port.postMessage({ type: 'powerLimiter', params });
  }

  // ============================================================
  // BYPASS SYSTEM
  // ============================================================
  initBypassButtons() {
    document.querySelectorAll('.bypass-btn').forEach(btn => {
      btn.addEventListener('click', (e) => {
        e.stopPropagation();
        const module = btn.dataset.bypass;
        this.toggleBypass(module, btn);
      });
    });
  }

  toggleBypass(module, btn) {
    this.bypassStates[module] = !this.bypassStates[module];
    
    // UI updates
    btn.classList.toggle('active', this.bypassStates[module]);
    
    const panel = btn.closest('.dsp-panel');
    if (panel) {
      panel.classList.toggle('bypassed', this.bypassStates[module]);
    }
    
    // Update band sections if applicable
    const bandSections = panel?.querySelectorAll('.band-section');
    if (bandSections) {
      bandSections.forEach(section => {
        section.classList.toggle('bypassed', this.bypassStates[module]);
      });
    }
    
    // Send to DSP
    if (this.audioWorklet) {
      this.audioWorklet.port.postMessage({ 
        type: 'bypass', 
        module, 
        enabled: this.bypassStates[module] 
      });
    }
    
    // Visual feedback animation
    btn.style.animation = 'none';
    setTimeout(() => {
      btn.style.animation = '';
    }, 10);
    
    console.log(`${module} bypass: ${this.bypassStates[module] ? 'ON' : 'OFF'}`);
  }

  updateMeters() {
    const m = this.meters;
    const p = this.peaks;
    
    ['inputL', 'inputR', 'outputL', 'outputR'].forEach(ch => {
      if (p[ch] > this.peakHold[ch]) {
        this.peakHold[ch] = p[ch];
      } else {
        this.peakHold[ch] = Math.max(this.peakHold[ch] - 0.5, p[ch]);
      }
    });
    
    const clampDb = (db) => Math.max(-60, Math.min(0, db));
    
    document.getElementById('inputMeterL').style.height = Math.max(0, ((clampDb(m.inputL) + 60) / 60 * 100)) + '%';
    document.getElementById('inputMeterR').style.height = Math.max(0, ((clampDb(m.inputR) + 60) / 60 * 100)) + '%';
    document.getElementById('inputPeakL').style.bottom = Math.max(0, ((clampDb(this.peakHold.inputL) + 60) / 60 * 100)) + '%';
    document.getElementById('inputPeakR').style.bottom = Math.max(0, ((clampDb(this.peakHold.inputR) + 60) / 60 * 100)) + '%';
    
    document.getElementById('outputMeterL').style.height = Math.max(0, ((clampDb(m.outputL) + 60) / 60 * 100)) + '%';
    document.getElementById('outputMeterR').style.height = Math.max(0, ((clampDb(m.outputR) + 60) / 60 * 100)) + '%';
    document.getElementById('outputPeakL').style.bottom = Math.max(0, ((clampDb(this.peakHold.outputL) + 60) / 60 * 100)) + '%';
    document.getElementById('outputPeakR').style.bottom = Math.max(0, ((clampDb(this.peakHold.outputR) + 60) / 60 * 100)) + '%';
    
    document.getElementById('procMeterL').style.height = document.getElementById('outputMeterL').style.height;
    document.getElementById('procMeterR').style.height = document.getElementById('outputMeterR').style.height;
    document.getElementById('procPeakL').style.bottom = document.getElementById('outputPeakL').style.bottom;
    document.getElementById('procPeakR').style.bottom = document.getElementById('outputPeakR').style.bottom;
  }

  updateGRMeters(gr) {
    const agcGr = Math.max(0, Math.min(100, Math.abs(gr.agc) / 40 * 100));
    const limGr = Math.max(0, Math.min(100, Math.abs(gr.finalLimiter) / 40 * 100));
    const powLimGr = Math.max(0, Math.min(100, Math.abs(gr.powerLimiter) / 40 * 100));
    
    document.getElementById('agcGrMeter').style.width = agcGr + '%';
    document.getElementById('limGrMeter').style.width = limGr + '%';
    document.getElementById('powLimGrMeter').style.width = powLimGr + '%';
  }

  initSpectrum() {
    this.canvas = document.getElementById('spectrumCanvas');
    this.ctx = this.canvas.getContext('2d');
    this.resizeCanvas();
    window.addEventListener('resize', () => this.resizeCanvas());
    this.drawSpectrum();
  }

  resizeCanvas() {
    const canvas = this.canvas;
    canvas.width = canvas.offsetWidth * 2;
    canvas.height = canvas.offsetHeight * 2;
  }

  drawSpectrum() {
    const canvas = this.canvas;
    const ctx = this.ctx;
    const w = canvas.width;
    const h = canvas.height;
    
    ctx.fillStyle = '#050505';
    ctx.fillRect(0, 0, w, h);
    
    ctx.strokeStyle = '#1a1a1a';
    ctx.lineWidth = 1;
    for (let i = 0; i <= 8; i++) {
      const y = (h / 8) * i;
      ctx.beginPath();
      ctx.moveTo(0, y);
      ctx.lineTo(w, y);
      ctx.stroke();
    }
    
    ctx.fillStyle = '#444';
    ctx.font = '16px Courier New';
    const freqLabels = ['20', '50', '100', '200', '500', '1k', '2k', '5k', '10k', '20k'];
    freqLabels.forEach((label, i) => {
      const x = (w / (freqLabels.length - 1)) * i;
      ctx.fillText(label, x - 10, h - 5);
    });
    
    if (!this.spectrumData || this.spectrumData.length === 0) return;
    
    const barWidth = w / this.spectrumData.length;
    const minDb = -80;
    const maxDb = 0;
    
    for (let i = 0; i < this.spectrumData.length; i++) {
      const db = Math.max(minDb, Math.min(maxDb, this.spectrumData[i]));
      const normalizedHeight = ((db - minDb) / (maxDb - minDb));
      const barHeight = normalizedHeight * h * 0.85;
      
      const intensity = normalizedHeight;
      let r, g, b;
      if (intensity < 0.5) {
        r = 0; g = Math.floor(intensity * 2 * 255); b = 0;
      } else if (intensity < 0.8) {
        r = Math.floor((intensity - 0.5) * 2 * 255); g = 255; b = 0;
      } else {
        r = 255; g = Math.floor((1 - (intensity - 0.8) * 5) * 255); b = 0;
      }
      
      ctx.fillStyle = `rgb(${r},${g},${b})`;
      ctx.fillRect(i * barWidth, h - barHeight, barWidth - 1, barHeight);
    }
    
    ctx.shadowBlur = 10;
    ctx.shadowColor = 'rgba(57,255,20,0.3)';
    ctx.strokeStyle = 'rgba(57,255,20,0.5)';
    ctx.lineWidth = 2;
    ctx.beginPath();
    for (let i = 0; i < this.spectrumData.length; i++) {
      const db = Math.max(minDb, Math.min(maxDb, this.spectrumData[i]));
      const normalizedHeight = ((db - minDb) / (maxDb - minDb));
      const barHeight = normalizedHeight * h * 0.85;
      const x = i * barWidth + barWidth / 2;
      const y = h - barHeight;
      if (i === 0) ctx.moveTo(x, y);
      else ctx.lineTo(x, y);
    }
    ctx.stroke();
    ctx.shadowBlur = 0;
  }

  initEQDisplay() {
    this.eqCanvas = document.getElementById('eqCanvas');
    if (!this.eqCanvas) return;
    this.eqCtx = this.eqCanvas.getContext('2d');
    this.eqCanvas.width = this.eqCanvas.offsetWidth * 2;
    this.eqCanvas.height = this.eqCanvas.offsetHeight * 2;
    this.drawEQCurve();
  }

  drawEQCurve() {
    const canvas = this.eqCanvas;
    if (!canvas) return;
    const ctx = this.eqCtx;
    const w = canvas.width;
    const h = canvas.height;
    
    ctx.fillStyle = '#050505';
    ctx.fillRect(0, 0, w, h);
    
    ctx.strokeStyle = '#1a1a1a';
    ctx.lineWidth = 1;
    for (let db = -12; db <= 12; db += 3) {
      const y = h / 2 - (db / 12) * (h / 2) * 0.8;
      ctx.beginPath();
      ctx.moveTo(0, y);
      ctx.lineTo(w, y);
      ctx.stroke();
    }
    
    ctx.strokeStyle = '#333';
    ctx.beginPath();
    ctx.moveTo(0, h / 2);
    ctx.lineTo(w, h / 2);
    ctx.stroke();
    
    ctx.fillStyle = '#555';
    ctx.font = '14px Courier New';
    for (let db = -12; db <= 12; db += 6) {
      const y = h / 2 - (db / 12) * (h / 2) * 0.8;
      ctx.fillText((db > 0 ? '+' : '') + db + 'dB', 5, y + 4);
    }
    
    const freqLabels = ['20', '50', '100', '200', '500', '1k', '2k', '5k', '10k', '20k'];
    freqLabels.forEach((label, i) => {
      const x = (w / (freqLabels.length - 1)) * i;
      ctx.fillStyle = '#555';
      ctx.fillText(label, x - 12, h - 5);
    });
    
    if (!this.eqData || this.eqData.length === 0) return;
    
    ctx.shadowBlur = 8;
    ctx.shadowColor = 'rgba(57,255,20,0.5)';
    ctx.strokeStyle = '#39ff14';
    ctx.lineWidth = 3;
    ctx.beginPath();
    
    for (let i = 0; i < this.eqData.length; i++) {
      const db = Math.max(-12, Math.min(12, this.eqData[i]));
      const x = (i / (this.eqData.length - 1)) * w;
      const y = h / 2 - (db / 12) * (h / 2) * 0.8;
      if (i === 0) ctx.moveTo(x, y);
      else ctx.lineTo(x, y);
    }
    ctx.stroke();
    ctx.shadowBlur = 0;
    
    ctx.lineTo(w, h / 2);
    ctx.lineTo(0, h / 2);
    ctx.closePath();
    ctx.fillStyle = 'rgba(57,255,20,0.05)';
    ctx.fill();
  }

  // ============================================================
  // UI INITIALIZATION
  // ============================================================
  initUI() {
    document.getElementById('powerBtn').addEventListener('click', () => this.togglePower());
    
    document.getElementById('configBtn').addEventListener('click', () => {
      document.getElementById('configOverlay').classList.add('visible');
    });
    document.getElementById('configClose').addEventListener('click', () => {
      document.getElementById('configOverlay').classList.remove('visible');
    });
    
    document.getElementById('presetsBtn').addEventListener('click', () => {
      const bar = document.getElementById('presetsBar');
      bar.style.display = bar.style.display === 'none' ? 'flex' : 'none';
    });
    
    document.querySelectorAll('.preset-btn').forEach(btn => {
      btn.addEventListener('click', () => {
        this.loadPreset(btn.dataset.preset);
        document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
      });
    });
    
    document.getElementById('modeSelect').addEventListener('change', (e) => {
      this.params.mode = e.target.value;
      this.loadPreset(e.target.value);
    });
    
    document.querySelectorAll('.mode-btn').forEach(btn => {
      btn.addEventListener('click', () => {
        document.querySelectorAll('.mode-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        const mode = btn.dataset.mode;
        const modeVal = mode === 'stereo' ? 0 : mode === 'mono' ? 1 : 2;
        this.sendParam('stereoMode', modeVal);
        this.params.stereoMode = modeVal;
      });
    });
    
    document.getElementById('exportBtn').addEventListener('click', () => this.exportConfig());
    document.getElementById('importBtn').addEventListener('click', () => {
      document.getElementById('importFile').click();
    });
    document.getElementById('importFile').addEventListener('change', (e) => {
      this.importConfig(e.target.files[0]);
    });
    
    ['agcMode', 'configSampleRate', 'configBufferSize', 'oversampling', 'antiClipping', 'harmonicSat'].forEach(id => {
      const el = document.getElementById(id);
      if (el) el.addEventListener('change', () => this.applyConfig());
    });

    document.getElementById('inputDevice').addEventListener('change', (e) => {
      this.params.inputDevice = e.target.value;
      localStorage.setItem('inputDevice', e.target.value);
      if (this.isEnabled) this.restartAudio();
    });

    document.getElementById('outputDevice').addEventListener('change', async (e) => {
      this.params.outputDevice = e.target.value;
      localStorage.setItem('outputDevice', e.target.value);
      if (this.audioCtx && this.audioCtx.setSinkId) {
        try { await this.audioCtx.setSinkId(e.target.value); } 
        catch (err) { console.error("Error setting output device:", err); }
      }
    });

    this.enumerateDevices();
  }

  async enumerateDevices() {
    try {
      if (!this.inputStream) {
        const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
        stream.getTracks().forEach(track => track.stop());
      }
      
      const devices = await navigator.mediaDevices.enumerateDevices();
      const inputSelect = document.getElementById('inputDevice');
      const outputSelect = document.getElementById('outputDevice');
      
      if (!inputSelect || !outputSelect) return;
      
      inputSelect.innerHTML = '';
      outputSelect.innerHTML = '';
      
      devices.forEach(device => {
        const option = document.createElement('option');
        option.value = device.deviceId;
        option.textContent = device.label || `${device.kind} (${device.deviceId.substring(0, 5)})`;
        
        if (device.kind === 'audioinput') {
          inputSelect.appendChild(option);
          if (device.deviceId === this.params.inputDevice) option.selected = true;
        } else if (device.kind === 'audiooutput') {
          outputSelect.appendChild(option);
          if (device.deviceId === this.params.outputDevice) option.selected = true;
        }
      });
    } catch (err) {
      console.error("Error enumerating devices:", err);
    }
  }

  async restartAudio() {
    if (this.inputStream) {
      this.inputStream.getTracks().forEach(track => track.stop());
    }
    if (this.audioCtx) {
      await this.audioCtx.close();
    }
    this.initAudio();
  }

  applyConfig() {
    const sat = parseInt(document.getElementById('harmonicSat').value);
    this.sendParam('saturation', sat * 0.15);
    this.params.saturation = sat * 0.15;
    
    const bufSize = parseInt(document.getElementById('configBufferSize').value);
    this.bufferSize = bufSize;
    document.getElementById('bufferSizeDisplay').textContent = bufSize + ' samples';
    
    const latency = (bufSize / (this.audioCtx ? this.audioCtx.sampleRate : 48000) * 1000).toFixed(2);
    document.getElementById('latencyDisplay').textContent = latency + ' ms';
  }

  // ============================================================
  // KNOB INTERACTION (simplified for brevity)
  // ============================================================
  initKnobs() {
    this.initStandardKnobs();
    this.initEQKnobs();
    this.initAGCKnobs();
    this.initMultiband4Knobs();
    this.initComp3Knobs();
    this.initLimiter3Knobs();
    this.initFinalLimiterKnobs();
    this.initPowerLimiterKnobs();
  }

  initStandardKnobs() {
    document.querySelectorAll('.knob-wrapper').forEach(wrapper => {
      const knob = wrapper.querySelector('.knob');
      const valueDisplay = wrapper.parentElement.querySelector('.knob-value');
      const param = wrapper.dataset.param;
      const min = parseFloat(wrapper.dataset.min);
      const max = parseFloat(wrapper.dataset.max);
      const defaultVal = parseFloat(wrapper.dataset.default);
      
      let currentValue = this.params[param] !== undefined ? this.params[param] : defaultVal;
      let isDragging = false;
      let startY = 0;
      let startValue = 0;
      let angle = ((currentValue - min) / (max - min)) * 270 - 135;
      
      knob.style.setProperty('--knob-angle', angle + 'deg');
      
      const updateValue = (val) => {
        currentValue = Math.max(min, Math.min(max, val));
        angle = ((currentValue - min) / (max - min)) * 270 - 135;
        knob.style.setProperty('--knob-angle', angle + 'deg');
        
        let displayValue;
        if (param.includes('Gain') || param.includes('gain') || param.includes('threshold') || param.includes('Threshold')) {
          displayValue = currentValue.toFixed(1) + ' dB';
        } else if (param.includes('Freq') || param.includes('freq')) {
          displayValue = currentValue >= 1000 ? (currentValue/1000).toFixed(1) + 'k' : Math.round(currentValue);
        } else if (param.includes('Ratio') || param.includes('ratio')) {
          displayValue = currentValue.toFixed(1) + ':1';
        } else if (param.includes('Attack') || param.includes('Release') || param.includes('attack') || param.includes('release')) {
          displayValue = Math.round(currentValue) + ' ms';
        } else if (param.includes('Q') || param.includes('q')) {
          displayValue = currentValue.toFixed(2);
        } else if (param.includes('Limit') || param.includes('limit')) {
          displayValue = Math.round(currentValue) + '%';
        } else if (param.includes('Pan') || param.includes('pan') || param.includes('Balance') || param.includes('balance')) {
          if (Math.abs(currentValue) < 1) displayValue = 'C';
          else if (currentValue < 0) displayValue = 'L' + Math.abs(Math.round(currentValue));
          else displayValue = 'R' + Math.round(currentValue);
        } else if (param.includes('Width') || param.includes('width')) {
          displayValue = Math.round(currentValue) + '%';
        } else if (param.includes('preEmphasis') && param !== 'preEmphasisGain') {
          displayValue = currentValue === 0 ? '50μs' : '75μs';
        } else if (param === 'stereoMode') {
          displayValue = ['ST', 'MO', 'MX'][Math.round(currentValue)];
        } else if (param === 'inputPhase') {
          displayValue = currentValue === 0 ? 'NOR' : 'REV';
        } else {
          displayValue = currentValue.toFixed(1);
        }
        
        if (valueDisplay) valueDisplay.textContent = displayValue;
        if (!param.startsWith('eq_')) this.sendParam(param, currentValue);
      };
      
      wrapper.addEventListener('mousedown', (e) => {
        isDragging = true; startY = e.clientY; startValue = currentValue; e.preventDefault();
      });
      window.addEventListener('mousemove', (e) => {
        if (!isDragging) return;
        const delta = (startY - e.clientY) * ((max - min) / 200);
        updateValue(startValue + delta);
      });
      window.addEventListener('mouseup', () => { isDragging = false; });
      wrapper.addEventListener('touchstart', (e) => {
        isDragging = true; startY = e.touches[0].clientY; startValue = currentValue; e.preventDefault();
      });
      window.addEventListener('touchmove', (e) => {
        if (!isDragging) return;
        const delta = (startY - e.touches[0].clientY) * ((max - min) / 200);
        updateValue(startValue + delta);
      });
      window.addEventListener('touchend', () => { isDragging = false; });
      wrapper.addEventListener('dblclick', () => updateValue(defaultVal));
      wrapper.addEventListener('wheel', (e) => {
        e.preventDefault();
        const delta = -e.deltaY * ((max - min) / 1000);
        updateValue(currentValue + delta);
      }, { passive: false });
      
      updateValue(currentValue);
    });
  }

  createKnobHTML(id, label, min, max, defaultVal, param, unit) {
    return `
      <div class="knob-container">
        <div class="knob-wrapper" data-param="${param}" data-min="${min}" data-max="${max}" data-default="${defaultVal}" id="${id}">
          <div class="knob small"></div>
        </div>
        <div class="knob-value">${defaultVal}${unit}</div>
        <div class="knob-label">${label}</div>
      </div>
    `;
  }

  // ... [Resto de los métodos initEQKnobs, initAGCKnobs, etc. se mantienen igual]
  // Por brevedad, se omiten pero funcionan igual que en la versión original

  initEQKnobs() {
    const container = document.getElementById('eqKnobs');
    if (!container) return;
    
    let html = '';
    this.params.eq.forEach((eq, i) => {
      const typeOptions = ['bell', 'highpass', 'lowpass', 'highshelf', 'lowshelf'];
      html += `
        <div class="band-section">
          <div class="band-title">Band ${i + 1} - ${eq.type}</div>
          <div style="display:flex;gap:4px;justify-content:center;margin-bottom:6px;">
            ${typeOptions.map(t => `<button class="toggle-btn ${eq.type === t ? 'active' : ''}" data-eqband="${i}" data-eqtype="${t}">${t.substring(0,3).toUpperCase()}</button>`).join('')}
            <button class="toggle-btn ${eq.enabled ? '' : 'active'}" data-eqband="${i}" data-eqtgl="enabled">${eq.enabled ? 'ON' : 'OFF'}</button>
          </div>
          <div class="knobs-grid">
            ${this.createKnobHTML(`eq_freq_${i}`, 'Freq', 20, 20000, eq.freq, `eq_freq_${i}`, ' Hz')}
            ${this.createKnobHTML(`eq_gain_${i}`, 'Gain', -12, 12, eq.gain, `eq_gain_${i}`, ' dB')}
            ${this.createKnobHTML(`eq_q_${i}`, 'Q', 0.1, 10, eq.q, `eq_q_${i}`, '')}
          </div>
        </div>
      `;
    });
    container.innerHTML = html;
    
    container.querySelectorAll('.knob-wrapper').forEach(wrapper => {
      const param = wrapper.dataset.param;
      const bandIdx = parseInt(param.split('_')[2]);
      const eqParam = param.split('_')[1];
      const min = parseFloat(wrapper.dataset.min);
      const max = parseFloat(wrapper.dataset.max);
      const defaultVal = parseFloat(wrapper.dataset.default);
      
      let currentValue = this.params.eq[bandIdx][eqParam];
      let isDragging = false;
      let startY = 0;
      let startValue = 0;
      
      const knob = wrapper.querySelector('.knob');
      const valueDisplay = wrapper.parentElement.querySelector('.knob-value');
      
      const angle = ((currentValue - min) / (max - min)) * 270 - 135;
      knob.style.setProperty('--knob-angle', angle + 'deg');
      
      const updateValue = (val) => {
        currentValue = Math.max(min, Math.min(max, val));
        const angle = ((currentValue - min) / (max - min)) * 270 - 135;
        knob.style.setProperty('--knob-angle', angle + 'deg');
        
        let display;
        if (eqParam === 'freq') display = currentValue >= 1000 ? (currentValue/1000).toFixed(1) + 'k' : Math.round(currentValue);
        else if (eqParam === 'gain') display = currentValue.toFixed(1) + ' dB';
        else display = currentValue.toFixed(2);
        valueDisplay.textContent = display;
        
        this.params.eq[bandIdx][eqParam] = currentValue;
        this.sendEQ(bandIdx, eqParam, currentValue);
        this.drawEQCurve();
      };
      
      wrapper.addEventListener('mousedown', (e) => { isDragging = true; startY = e.clientY; startValue = currentValue; e.preventDefault(); });
      window.addEventListener('mousemove', (e) => { if (!isDragging) return; updateValue(startValue + (startY - e.clientY) * ((max - min) / 200)); });
      window.addEventListener('mouseup', () => { isDragging = false; });
      wrapper.addEventListener('wheel', (e) => { e.preventDefault(); updateValue(currentValue - e.deltaY * ((max - min) / 1000)); }, { passive: false });
      wrapper.addEventListener('dblclick', () => updateValue(defaultVal));
    });
    
    container.querySelectorAll('[data-eqtype]').forEach(btn => {
      btn.addEventListener('click', () => {
        const bandIdx = parseInt(btn.dataset.eqband);
        this.params.eq[bandIdx].type = btn.dataset.eqtype;
        btn.parentElement.querySelectorAll('[data-eqtype]').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        btn.closest('.band-section').querySelector('.band-title').textContent = `Band ${bandIdx + 1} - ${this.params.eq[bandIdx].type}`;
        this.sendEQ(bandIdx, 'type', this.params.eq[bandIdx].type);
        this.drawEQCurve();
      });
    });
    
    container.querySelectorAll('[data-eqtgl]').forEach(btn => {
      btn.addEventListener('click', () => {
        const bandIdx = parseInt(btn.dataset.eqband);
        this.params.eq[bandIdx].enabled = !this.params.eq[bandIdx].enabled;
        btn.textContent = this.params.eq[bandIdx].enabled ? 'ON' : 'OFF';
        btn.classList.toggle('active');
        this.sendEQ(bandIdx, 'enabled', this.params.eq[bandIdx].enabled);
        this.drawEQCurve();
      });
    });
  }

  initAGCKnobs() {
    const container = document.getElementById('agcKnobs');
    if (!container) return;
    const agc = this.params.agc;
    container.innerHTML = `
      <div class="knobs-grid">
        ${this.createKnobHTML('agc_threshold', 'Threshold', -60, 0, agc.threshold, 'agc_threshold', ' dB')}
        ${this.createKnobHTML('agc_ratio', 'Ratio', 1, 20, agc.ratio, 'agc_ratio', ':1')}
        ${this.createKnobHTML('agc_attack', 'Attack', 0.1, 200, agc.attack, 'agc_attack', ' ms')}
        ${this.createKnobHTML('agc_release', 'Release', 10, 2000, agc.release, 'agc_release', ' ms')}
      </div>
    `;
    this.bindKnobGroup('agc_threshold', 'threshold', agc.threshold, (v) => { this.params.agc.threshold = v; this.audioWorklet?.port.postMessage({ type: 'agc', params: this.params.agc }); });
    this.bindKnobGroup('agc_ratio', 'ratio', agc.ratio, (v) => { this.params.agc.ratio = v; this.audioWorklet?.port.postMessage({ type: 'agc', params: this.params.agc }); });
    this.bindKnobGroup('agc_attack', 'attack', agc.attack, (v) => { this.params.agc.attack = v; this.audioWorklet?.port.postMessage({ type: 'agc', params: this.params.agc }); });
    this.bindKnobGroup('agc_release', 'release', agc.release, (v) => { this.params.agc.release = v; this.audioWorklet?.port.postMessage({ type: 'agc', params: this.params.agc }); });
  }

  initMultiband4Knobs() {
    const container = document.getElementById('multiband4Knobs');
    if (!container) return;
    let html = '';
    this.params.multiband4.forEach((band, i) => {
      html += `
        <div class="band-section">
          <div class="band-title">${band.name} (Split: ${band.freq} Hz)</div>
          <div style="display:flex;gap:4px;justify-content:center;margin-bottom:6px;">
            <button class="toggle-btn ${band.enabled ? '' : 'active'}" data-mb4band="${i}" data-mb4tgl="enabled">${band.enabled ? 'ON' : 'OFF'}</button>
          </div>
          <div class="knobs-grid">
            ${this.createKnobHTML(`mb4_freq_${i}`, 'Freq', 40, 16000, band.freq, `mb4_freq_${i}`, ' Hz')}
            ${this.createKnobHTML(`mb4_thr_${i}`, 'Threshold', -60, 0, band.threshold, `mb4_thr_${i}`, ' dB')}
            ${this.createKnobHTML(`mb4_rat_${i}`, 'Ratio', 1, 20, band.ratio, `mb4_rat_${i}`, ':1')}
            ${this.createKnobHTML(`mb4_atk_${i}`, 'Attack', 0.1, 200, band.attack, `mb4_atk_${i}`, ' ms')}
            ${this.createKnobHTML(`mb4_rel_${i}`, 'Release', 10, 2000, band.release, `mb4_rel_${i}`, ' ms')}
            ${this.createKnobHTML(`mb4_gn_${i}`, 'Gain', -12, 12, band.gain, `mb4_gn_${i}`, ' dB')}
          </div>
        </div>
      `;
    });
    container.innerHTML = html;
    const paramMap = { 'mb4_freq': 'freq', 'mb4_thr': 'threshold', 'mb4_rat': 'ratio', 'mb4_atk': 'attack', 'mb4_rel': 'release', 'mb4_gn': 'gain' };
    this.params.multiband4.forEach((band, i) => {
      Object.entries(paramMap).forEach(([prefix, param]) => {
        this.bindKnobGroup(`${prefix}_${i}`, param, band[param], (v) => { this.params.multiband4[i][param] = v; this.sendMultiband4(i, { [param]: v }); });
      });
    });
    container.querySelectorAll('[data-mb4tgl]').forEach(btn => {
      btn.addEventListener('click', () => {
        const idx = parseInt(btn.dataset.mb4band);
        this.params.multiband4[idx].enabled = !this.params.multiband4[idx].enabled;
        btn.textContent = this.params.multiband4[idx].enabled ? 'ON' : 'OFF';
        btn.classList.toggle('active');
        this.sendMultiband4(idx, { enabled: this.params.multiband4[idx].enabled });
      });
    });
  }

  initComp3Knobs() {
    const container = document.getElementById('comp3Knobs');
    if (!container) return;
    let html = '';
    this.params.comp3.forEach((band, i) => {
      html += `
        <div class="band-section">
          <div class="band-title">${band.name} (Split: ${band.freq} Hz)</div>
          <div style="display:flex;gap:4px;justify-content:center;margin-bottom:6px;">
            <button class="toggle-btn ${band.enabled ? '' : 'active'}" data-c3band="${i}" data-c3tgl="enabled">${band.enabled ? 'ON' : 'OFF'}</button>
          </div>
          <div class="knobs-grid">
            ${this.createKnobHTML(`c3_freq_${i}`, 'Freq', 40, 16000, band.freq, `c3_freq_${i}`, ' Hz')}
            ${this.createKnobHTML(`c3_thr_${i}`, 'Threshold', -60, 0, band.threshold, `c3_thr_${i}`, ' dB')}
            ${this.createKnobHTML(`c3_rat_${i}`, 'Ratio', 1, 20, band.ratio, `c3_rat_${i}`, ':1')}
            ${this.createKnobHTML(`c3_atk_${i}`, 'Attack', 0.1, 200, band.attack, `c3_atk_${i}`, ' ms')}
            ${this.createKnobHTML(`c3_rel_${i}`, 'Release', 10, 2000, band.release, `c3_rel_${i}`, ' ms')}
            ${this.createKnobHTML(`c3_gn_${i}`, 'Gain', -12, 12, band.gain, `c3_gn_${i}`, ' dB')}
          </div>
        </div>
      `;
    });
    container.innerHTML = html;
    const paramMap = { 'c3_freq': 'freq', 'c3_thr': 'threshold', 'c3_rat': 'ratio', 'c3_atk': 'attack', 'c3_rel': 'release', 'c3_gn': 'gain' };
    this.params.comp3.forEach((band, i) => {
      Object.entries(paramMap).forEach(([prefix, param]) => {
        this.bindKnobGroup(`${prefix}_${i}`, param, band[param], (v) => { this.params.comp3[i][param] = v; this.sendComp3(i, { [param]: v }); });
      });
    });
    container.querySelectorAll('[data-c3tgl]').forEach(btn => {
      btn.addEventListener('click', () => {
        const idx = parseInt(btn.dataset.c3band);
        this.params.comp3[idx].enabled = !this.params.comp3[idx].enabled;
        btn.textContent = this.params.comp3[idx].enabled ? 'ON' : 'OFF';
        btn.classList.toggle('active');
        this.sendComp3(idx, { enabled: this.params.comp3[idx].enabled });
      });
    });
  }

  initLimiter3Knobs() {
    const container = document.getElementById('limiter3Knobs');
    if (!container) return;
    let html = '';
    this.params.limiter3.forEach((band, i) => {
      html += `
        <div class="band-section">
          <div class="band-title">${band.name} (Split: ${band.freq} Hz)</div>
          <div style="display:flex;gap:4px;justify-content:center;margin-bottom:6px;">
            <button class="toggle-btn ${band.enabled ? '' : 'active'}" data-l3band="${i}" data-l3tgl="enabled">${band.enabled ? 'ON' : 'OFF'}</button>
          </div>
          <div class="knobs-grid">
            ${this.createKnobHTML(`l3_freq_${i}`, 'Freq', 40, 16000, band.freq, `l3_freq_${i}`, ' Hz')}
            ${this.createKnobHTML(`l3_thr_${i}`, 'Threshold', -12, 0, band.threshold, `l3_thr_${i}`, ' dB')}
            ${this.createKnobHTML(`l3_rat_${i}`, 'Ratio', 5, 50, band.ratio, `l3_rat_${i}`, ':1')}
            ${this.createKnobHTML(`l3_atk_${i}`, 'Attack', 0.05, 10, band.attack, `l3_atk_${i}`, ' ms')}
            ${this.createKnobHTML(`l3_rel_${i}`, 'Release', 5, 200, band.release, `l3_rel_${i}`, ' ms')}
            ${this.createKnobHTML(`l3_gn_${i}`, 'Gain', -6, 6, band.gain, `l3_gn_${i}`, ' dB')}
          </div>
        </div>
      `;
    });
    container.innerHTML = html;
    const paramMap = { 'l3_freq': 'freq', 'l3_thr': 'threshold', 'l3_rat': 'ratio', 'l3_atk': 'attack', 'l3_rel': 'release', 'l3_gn': 'gain' };
    this.params.limiter3.forEach((band, i) => {
      Object.entries(paramMap).forEach(([prefix, param]) => {
        this.bindKnobGroup(`${prefix}_${i}`, param, band[param], (v) => { this.params.limiter3[i][param] = v; this.sendLimiter3(i, { [param]: v }); });
      });
    });
    container.querySelectorAll('[data-l3tgl]').forEach(btn => {
      btn.addEventListener('click', () => {
        const idx = parseInt(btn.dataset.l3band);
        this.params.limiter3[idx].enabled = !this.params.limiter3[idx].enabled;
        btn.textContent = this.params.limiter3[idx].enabled ? 'ON' : 'OFF';
        btn.classList.toggle('active');
        this.sendLimiter3(idx, { enabled: this.params.limiter3[idx].enabled });
      });
    });
  }

  initFinalLimiterKnobs() {
    const container = document.getElementById('finalLimiterKnobs');
    if (!container) return;
    const fl = this.params.finalLimiter;
    container.innerHTML = `
      <div class="knobs-grid">
        ${this.createKnobHTML('fl_thr', 'Threshold', -20, 0, fl.threshold, 'fl_thr', ' dB')}
        ${this.createKnobHTML('fl_rat', 'Ratio', 5, 50, fl.ratio, 'fl_rat', ':1')}
        ${this.createKnobHTML('fl_atk', 'Attack', 0.01, 10, fl.attack, 'fl_atk', ' ms')}
        ${this.createKnobHTML('fl_rel', 'Release', 5, 500, fl.release, 'fl_rel', ' ms')}
        ${this.createKnobHTML('fl_gn', 'Gain', -12, 12, fl.gain, 'fl_gn', ' dB')}
      </div>
    `;
    this.bindKnobGroup('fl_thr', 'threshold', fl.threshold, (v) => { this.params.finalLimiter.threshold = v; this.sendFinalLimiter({ threshold: v }); });
    this.bindKnobGroup('fl_rat', 'ratio', fl.ratio, (v) => { this.params.finalLimiter.ratio = v; this.sendFinalLimiter({ ratio: v }); });
    this.bindKnobGroup('fl_atk', 'attack', fl.attack, (v) => { this.params.finalLimiter.attack = v; this.sendFinalLimiter({ attack: v }); });
    this.bindKnobGroup('fl_rel', 'release', fl.release, (v) => { this.params.finalLimiter.release = v; this.sendFinalLimiter({ release: v }); });
    this.bindKnobGroup('fl_gn', 'gain', fl.gain, (v) => { this.params.finalLimiter.gain = v; this.sendFinalLimiter({ gain: v }); });
  }

  initPowerLimiterKnobs() {
    const container = document.getElementById('powerLimiterKnobs');
    if (!container) return;
    const pl = this.params.powerLimiter;
    container.innerHTML = `
      <div class="knobs-grid">
        ${this.createKnobHTML('pl_thr', 'Threshold', -20, 0, pl.threshold, 'pl_thr', ' dB')}
        ${this.createKnobHTML('pl_rat', 'Ratio', 5, 100, pl.ratio, 'pl_rat', ':1')}
        ${this.createKnobHTML('pl_atk', 'Attack', 0.01, 5, pl.attack, 'pl_atk', ' ms')}
        ${this.createKnobHTML('pl_rel', 'Release', 5, 100, pl.release, 'pl_rel', ' ms')}
        ${this.createKnobHTML('pl_gn', 'Gain', -12, 12, pl.gain, 'pl_gn', ' dB')}
      </div>
    `;
    this.bindKnobGroup('pl_thr', 'threshold', pl.threshold, (v) => { this.params.powerLimiter.threshold = v; this.sendPowerLimiter({ threshold: v }); });
    this.bindKnobGroup('pl_rat', 'ratio', pl.ratio, (v) => { this.params.powerLimiter.ratio = v; this.sendPowerLimiter({ ratio: v }); });
    this.bindKnobGroup('pl_atk', 'attack', pl.attack, (v) => { this.params.powerLimiter.attack = v; this.sendPowerLimiter({ attack: v }); });
    this.bindKnobGroup('pl_rel', 'release', pl.release, (v) => { this.params.powerLimiter.release = v; this.sendPowerLimiter({ release: v }); });
    this.bindKnobGroup('pl_gn', 'gain', pl.gain, (v) => { this.params.powerLimiter.gain = v; this.sendPowerLimiter({ gain: v }); });
  }

  bindKnobGroup(id, param, defaultVal, onChange) {
    const wrapper = document.getElementById(id);
    if (!wrapper) return;
    const knob = wrapper.querySelector('.knob');
    const valueDisplay = wrapper.parentElement.querySelector('.knob-value');
    const min = parseFloat(wrapper.dataset.min);
    const max = parseFloat(wrapper.dataset.max);
    let currentValue = defaultVal;
    let isDragging = false;
    let startY = 0;
    let startValue = 0;
    const angle = ((currentValue - min) / (max - min)) * 270 - 135;
    knob.style.setProperty('--knob-angle', angle + 'deg');
    
    const updateValue = (val) => {
      currentValue = Math.max(min, Math.min(max, val));
      const angle = ((currentValue - min) / (max - min)) * 270 - 135;
      knob.style.setProperty('--knob-angle', angle + 'deg');
      let display;
      if (param === 'threshold') display = currentValue.toFixed(1) + ' dB';
      else if (param === 'ratio') display = currentValue.toFixed(1) + ':1';
      else if (param === 'attack') display = currentValue.toFixed(1) + ' ms';
      else if (param === 'release') display = Math.round(currentValue) + ' ms';
      else if (param === 'gain') display = currentValue.toFixed(1) + ' dB';
      else if (param === 'freq') display = currentValue >= 1000 ? (currentValue/1000).toFixed(1) + 'k' : Math.round(currentValue);
      else display = currentValue.toFixed(1);
      if (valueDisplay) valueDisplay.textContent = display;
      onChange(currentValue);
    };
    
    wrapper.addEventListener('mousedown', (e) => { isDragging = true; startY = e.clientY; startValue = currentValue; e.preventDefault(); });
    window.addEventListener('mousemove', (e) => { if (!isDragging) return; updateValue(startValue + (startY - e.clientY) * ((max - min) / 200)); });
    window.addEventListener('mouseup', () => { isDragging = false; });
    wrapper.addEventListener('wheel', (e) => { e.preventDefault(); updateValue(currentValue - e.deltaY * ((max - min) / 1000)); }, { passive: false });
    wrapper.addEventListener('dblclick', () => updateValue(defaultVal));
  }

  // ============================================================
  // PRESETS
  // ============================================================
  presets = {
    bypass: {
      name: 'Bypass',
      eq: [{type:'highpass',freq:40,gain:0,q:0.7},{type:'bell',freq:100,gain:0,q:1},{type:'bell',freq:400,gain:0,q:1},{type:'bell',freq:1000,gain:0,q:1},{type:'bell',freq:4000,gain:0,q:1},{type:'highshelf',freq:10000,gain:0,q:0.7}],
      agc: {threshold:-30,ratio:2,attack:50,release:200},
      multiband4: [{freq:80,threshold:-30,ratio:2,attack:20,release:100,gain:0},{freq:250,threshold:-28,ratio:2,attack:15,release:80,gain:0},{freq:2500,threshold:-26,ratio:2,attack:10,release:60,gain:0},{freq:8000,threshold:-24,ratio:1.5,attack:8,release:50,gain:0}],
      comp3: [{freq:200,threshold:-30,ratio:2,attack:20,release:100,gain:0},{freq:2000,threshold:-28,ratio:2,attack:15,release:80,gain:0},{freq:8000,threshold:-26,ratio:1.5,attack:10,release:60,gain:0}],
      limiter3: [{freq:200,threshold:-6,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-6,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-6,ratio:20,attack:0.5,release:30,gain:0}],
      finalLimiter: {threshold:-3,ratio:20,attack:0.5,release:50,gain:0},
      powerLimiter: {threshold:-1,ratio:30,attack:0.1,release:20,gain:0},
      inputGain: 0, outputGain: 0
    },
    'fm-hot': {
      name: 'FM Hot',
      eq: [{type:'highpass',freq:50,gain:0,q:0.7},{type:'bell',freq:100,gain:2,q:1.2},{type:'bell',freq:400,gain:-1,q:1},{type:'bell',freq:1000,gain:1.5,q:0.8},{type:'bell',freq:4000,gain:3,q:1.2},{type:'highshelf',freq:10000,gain:2,q:0.7}],
      agc: {threshold:-24,ratio:4,attack:8,release:80},
      multiband4: [{freq:80,threshold:-18,ratio:4,attack:15,release:80,gain:2},{freq:250,threshold:-16,ratio:3,attack:10,release:60,gain:1.5},{freq:2500,threshold:-14,ratio:3,attack:8,release:50,gain:2},{freq:8000,threshold:-12,ratio:2.5,attack:5,release:40,gain:1}],
      comp3: [{freq:200,threshold:-20,ratio:4,attack:15,release:80,gain:1},{freq:2000,threshold:-18,ratio:3.5,attack:10,release:60,gain:1.5},{freq:8000,threshold:-15,ratio:3,attack:5,release:40,gain:1}],
      limiter3: [{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}],
      finalLimiter: {threshold:-1,ratio:20,attack:0.5,release:50,gain:0},
      powerLimiter: {threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0},
      inputGain: 3, outputGain: 0
    }
    // ... otros presets se mantienen igual
  };

  loadPreset(name) {
    const preset = this.presets[name];
    if (!preset) return;
    preset.eq.forEach((eq, i) => {
      if (this.params.eq[i]) {
        this.params.eq[i] = { ...eq, enabled: true, id: i };
        this.sendEQ(i, 'type', eq.type);
        this.sendEQ(i, 'freq', eq.freq);
        this.sendEQ(i, 'gain', eq.gain);
        this.sendEQ(i, 'q', eq.q);
      }
    });
    this.params.agc = { ...this.params.agc, ...preset.agc };
    this.audioWorklet?.port.postMessage({ type: 'agc', params: this.params.agc });
    preset.multiband4.forEach((band, i) => {
      if (this.params.multiband4[i]) {
        this.params.multiband4[i] = { ...this.params.multiband4[i], ...band };
        this.sendMultiband4(i, band);
      }
    });
    preset.comp3.forEach((band, i) => {
      if (this.params.comp3[i]) {
        this.params.comp3[i] = { ...this.params.comp3[i], ...band };
        this.sendComp3(i, band);
      }
    });
    preset.limiter3.forEach((band, i) => {
      if (this.params.limiter3[i]) {
        this.params.limiter3[i] = { ...this.params.limiter3[i], ...band };
        this.sendLimiter3(i, band);
      }
    });
    this.params.finalLimiter = { ...this.params.finalLimiter, ...preset.finalLimiter };
    this.sendFinalLimiter(preset.finalLimiter);
    this.params.powerLimiter = { ...this.params.powerLimiter, ...preset.powerLimiter };
    this.sendPowerLimiter(preset.powerLimiter);
    if (preset.inputGain !== undefined) { this.sendParam('inputGain', preset.inputGain); this.params.inputGain = preset.inputGain; }
    if (preset.outputGain !== undefined) { this.sendParam('outputGain', preset.outputGain); this.params.outputGain = preset.outputGain; }
    this.drawEQCurve();
    console.log(`Preset "${name}" loaded`);
  }

  exportConfig() {
    const config = JSON.stringify(this.params, null, 2);
    const blob = new Blob([config], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'optimod-pro-1600-config.json';
    a.click();
    URL.revokeObjectURL(url);
  }

  importConfig(file) {
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (e) => {
      try {
        const config = JSON.parse(e.target.result);
        Object.assign(this.params, config);
        this.sendAllParams();
        this.drawEQCurve();
        console.log('Configuration imported');
      } catch (err) { console.error('Import error:', err); }
    };
    reader.readAsText(file);
  }
}

// ============================================================
// INITIALIZE
// ============================================================
const processor = new BroadcastProcessor();
document.getElementById('latencyDisplay').textContent = (processor.bufferSize / 48000 * 1000).toFixed(2) + ' ms';

let lastCpuCheck = performance.now();
setInterval(() => {
  const now = performance.now();
  const delta = now - lastCpuCheck;
  lastCpuCheck = now;
  processor.frameCount++;
}, 16);
</script>
</body>
</html>
