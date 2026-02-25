# Smart-Automatic-Plant-Watering-System-using-Arduino
Arduino-powered automated plant watering system using a soil moisture sensor  and relay-controlled pump. When soil moisture drops below threshold, the pump  automatically activates, watering plants until optimal moisture is reached.  Ensures efficient hydration without manual intervention.
#code
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Plant Watering System - IoT Solution</title>
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 36 36'><text y='22' font-size='20'>🌱</text></svg>">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    :root {
      --primary: #667eea;
      --secondary: #764ba2;
      --accent: #4facfe;
      --success: #00c853;
      --danger: #ef4444;
      --warning: #f59e0b;
      --light: #f8f9fa;
      --border: #e0e0e0;
      --dark: #333;
      --gray: #666;
    }

    html, body {
      font-family: 'Segoe UI', Arial, sans-serif;
      scroll-behavior: smooth;
      background: #f5f5f5;
    }

    body {
      color: var(--dark);
      line-height: 1.6;
    }

    /* ============ NAVIGATION ============ */
    .navbar {
      background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
      padding: 1rem 2rem;
      position: sticky;
      top: 0;
      z-index: 100;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }

    .nav-container {
      max-width: 1200px;
      margin: 0 auto;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
    }

    .logo {
      font-size: 1.8em;
      font-weight: bold;
      color: white;
    }

    .nav-links {
      display: flex;
      list-style: none;
      gap: 2rem;
    }

    .nav-links a {
      color: white;
      text-decoration: none;
      font-weight: 500;
      transition: opacity 0.3s;
      cursor: pointer;
    }

    .nav-links a:hover,
    .nav-links a.active {
      opacity: 0.8;
      border-bottom: 2px solid white;
      padding-bottom: 5px;
    }

    /* ============ BUTTONS ============ */
    .btn {
      display: inline-block;
      padding: 12px 28px;
      border-radius: 8px;
      text-decoration: none;
      font-weight: bold;
      cursor: pointer;
      border: none;
      transition: all 0.3s ease;
      font-size: 1em;
    }

    .btn-primary {
      background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
      color: white;
    }

    .btn-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(102, 126, 234, 0.4);
    }

    .btn-secondary {
      background: white;
      color: var(--primary);
      border: 2px solid var(--primary);
    }

    .btn-secondary:hover {
      background: var(--primary);
      color: white;
    }

    button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }

    /* ============ SECTIONS ============ */
    section {
      display: none;
    }

    section.active {
      display: block;
    }

    .section-container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 40px 2rem;
    }

    .section-title {
      font-size: 2.5em;
      margin-bottom: 1rem;
      text-align: center;
      background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    /* ============ HOME PAGE ============ */
    .hero {
      background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
      color: white;
      padding: 100px 2rem;
      text-align: center;
    }

    .hero h1 {
      font-size: 3.5em;
      margin-bottom: 1rem;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
    }

    .hero p {
      font-size: 1.3em;
      margin-bottom: 2rem;
      opacity: 0.95;
    }

    .hero-buttons {
      display: flex;
      gap: 1rem;
      justify-content: center;
      flex-wrap: wrap;
    }

    .features-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 2rem;
      margin-top: 3rem;
    }

    .feature-card {
      background: white;
      padding: 2rem;
      border-radius: 12px;
      box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
      transition: transform 0.3s ease;
    }

    .feature-card:hover {
      transform: translateY(-5px);
    }

    .feature-icon {
      font-size: 3em;
      margin-bottom: 1rem;
    }

    .feature-card h3 {
      color: var(--primary);
      margin-bottom: 0.5rem;
    }

    .feature-card p {
      color: var(--gray);
    }

    /* ============ DASHBOARD ============ */
    .dashboard-controls {
      display: flex;
      gap: 1rem;
      justify-content: center;
      flex-wrap: wrap;
      margin-bottom: 2rem;
    }

    .status-indicator {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      padding: 0.75rem 1.5rem;
      background: white;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      font-weight: 500;
    }

    .status-indicator.connected {
      color: var(--success);
    }

    .status-indicator.disconnected {
      color: var(--danger);
    }

    .status-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: currentColor;
      animation: blink 2s infinite;
    }

    @keyframes blink {
      0%, 49%, 100% { opacity: 1; }
      50%, 99% { opacity: 0.5; }
    }

    .dashboard-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 2rem;
      margin-bottom: 2rem;
    }

    .card {
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
      overflow: hidden;
      transition: transform 0.3s ease;
    }

    .card:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
    }

    .card-header {
      background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
      color: white;
      padding: 1.5rem;
      font-size: 1.2em;
      font-weight: bold;
    }

    .card-body {
      padding: 2rem;
    }

    .big-value {
      font-size: 3em;
      font-weight: bold;
      text-align: center;
      background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      margin: 1rem 0;
    }

    .status-badge {
      display: inline-block;
      padding: 8px 16px;
      border-radius: 20px;
      font-weight: bold;
      margin-top: 1rem;
      text-align: center;
    }

    .badge-dry {
      background: #fee2e2;
      color: #dc2626;
    }

    .badge-ok {
      background: #fef3c7;
      color: #d97706;
    }

    .badge-wet {
      background: #dcfce7;
      color: #059669;
    }

    .progress-bar {
      width: 100%;
      height: 24px;
      background: #e0e0e0;
      border-radius: 12px;
      overflow: hidden;
      margin: 1rem 0;
    }

    .progress-fill {
      height: 100%;
      background: linear-gradient(90deg, #ef4444 0%, #f59e0b 50%, #10b981 100%);
      transition: width 0.3s ease;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-weight: bold;
      font-size: 0.8em;
    }

    .pump-indicator {
      font-size: 3.5em;
      text-align: center;
      margin: 1rem 0;
    }

    .pump-indicator.active {
      animation: pulse 1s infinite;
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.1); }
    }

    .info-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1rem;
      margin-top: 1rem;
    }

    .info-item {
      background: var(--light);
      padding: 1rem;
      border-radius: 8px;
      text-align: center;
    }

    .info-label {
      font-size: 0.85em;
      color: var(--gray);
      margin-bottom: 0.5rem;
    }

    .info-value {
      font-size: 1.3em;
      font-weight: bold;
      color: var(--primary);
    }

    .chart-container {
      position: relative;
      height: 300px;
      margin-top: 1rem;
    }

    .settings-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 2rem;
    }

    .setting-group {
      display: flex;
      flex-direction: column;
    }

    .setting-group label {
      font-weight: bold;
      margin-bottom: 0.5rem;
      color: var(--dark);
    }

    .setting-group input {
      padding: 10px;
      border: 2px solid var(--border);
      border-radius: 6px;
      font-size: 1em;
      transition: border-color 0.3s ease;
    }

    .setting-group input:focus {
      outline: none;
      border-color: var(--primary);
    }

    .setting-group small {
      font-size: 0.8em;
      color: var(--gray);
      margin-top: 0.3rem;
    }

    /* ============ SETUP PAGE ============ */
    .setup-section {
      background: white;
      padding: 2rem;
      border-radius: 12px;
      margin-bottom: 2rem;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
    }

    .setup-section h2 {
      color: var(--primary);
      margin-bottom: 1.5rem;
      font-size: 1.8em;
    }

    .components-list {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 1.5rem;
    }

    .component {
      background: var(--light);
      padding: 1.5rem;
      border-radius: 8px;
      border-left: 4px solid var(--primary);
    }

    .component h3 {
      color: var(--primary);
      margin-bottom: 1rem;
    }

    .component ul {
      list-style: none;
      padding: 0;
    }

    .component li {
      padding: 0.5rem 0;
      padding-left: 1.5rem;
      position: relative;
    }

    .component li:before {
      content: "✓";
      position: absolute;
      left: 0;
      color: var(--success);
      font-weight: bold;
    }

    .wiring-guide {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 1.5rem;
    }

    .wiring-item {
      background: var(--light);
      padding: 1.5rem;
      border-radius: 8px;
      border-left: 4px solid var(--primary);
    }

    .wiring-item h3 {
      color: var(--primary);
      margin-bottom: 1rem;
    }

    .wiring-item code {
      display: block;
      background: white;
      padding: 1rem;
      border-radius: 6px;
      font-family: 'Courier New', monospace;
      font-size: 0.85em;
      line-height: 1.8;
    }

    pre {
      background: var(--light);
      padding: 1.5rem;
      border-radius: 8px;
      overflow-x: auto;
      border-left: 4px solid var(--primary);
    }

    pre code {
      font-family: 'Courier New', monospace;
      font-size: 0.85em;
      line-height: 1.6;
    }

    .step-cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 1.5rem;
    }

    .step-card {
      background: var(--light);
      padding: 1.5rem;
      border-radius: 8px;
      text-align: center;
      border-top: 4px solid var(--primary);
    }

    .step-num {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 40px;
      height: 40px;
      background: var(--primary);
      color: white;
      border-radius: 50%;
      font-weight: bold;
      margin-bottom: 1rem;
    }

    .step-card h4 {
      color: var(--primary);
      margin-bottom: 0.5rem;
    }

    /* ============ ABOUT PAGE ============ */
    .about-cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 1.5rem;
      margin-top: 2rem;
    }

    .about-card {
      background: white;
      padding: 1.5rem;
      border-radius: 8px;
      text-align: center;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
    }

    .about-card h3 {
      color: var(--primary);
      margin-bottom: 0.5rem;
    }

    .tech-list {
      list-style: none;
      padding: 0;
      background: white;
      border-radius: 8px;
      padding: 2rem;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
    }

    .tech-list li {
      padding: 1rem;
      background: var(--light);
      margin-bottom: 1rem;
      border-radius: 8px;
      border-left: 4px solid var(--primary);
    }

    /* ============ FOOTER ============ */
    footer {
      background: #2c3e50;
      color: white;
      padding: 2rem;
      text-align: center;
      margin-top: 40px;
    }

    footer p {
      margin: 0;
    }

    footer a {
      color: #bbb;
      text-decoration: none;
    }

    footer a:hover {
      color: white;
    }

    /* ============ RESPONSIVE ============ */
    @media (max-width: 768px) {
      .nav-links {
        gap: 1rem;
        font-size: 0.9em;
      }

      .hero h1 {
        font-size: 2em;
      }

      .dashboard-grid {
        grid-template-columns: 1fr;
      }

      .settings-grid {
        grid-template-columns: 1fr;
      }

      .info-grid {
        grid-template-columns: 1fr;
      }

      .hero-buttons {
        flex-direction: column;
      }

      button {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <!-- NAVIGATION -->
  <nav class="navbar">
    <div class="nav-container">
      <div class="logo">🌱 SmartWater</div>
      <ul class="nav-links">
        <li><a onclick="showSection('home')" class="nav-link active">Home</a></li>
        <li><a onclick="showSection('dashboard')" class="nav-link">Dashboard</a></li>
        <li><a onclick="showSection('setup')" class="nav-link">Setup</a></li>
        <li><a onclick="showSection('about')" class="nav-link">About</a></li>
      </ul>
    </div>
  </nav>

  <!-- HOME SECTION -->
  <section id="home" class="active">
    <div class="hero">
      <h1>🌱 Smart Plant Watering System</h1>
      <p>Automate your plant care with intelligent IoT technology</p>
      <p>Monitor soil moisture in real-time and control your pump automatically</p>
      <div class="hero-buttons">
        <button class="btn btn-primary" onclick="showSection('dashboard')">🎯 Go to Dashboard</button>
        <button class="btn btn-secondary" onclick="showSection('setup')">📖 Setup Guide</button>
      </div>
    </div>
    <div class="section-container">
      <h2 class="section-title">Why Choose SmartWater?</h2>
      <div class="features-grid">
        <div class="feature-card">
          <div class="feature-icon">📊</div>
          <h3>Real-Time Monitoring</h3>
          <p>Track soil moisture levels 24/7 with live data visualization</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">🤖</div>
          <h3>Automatic Control</h3>
          <p>Smart pump activation based on custom thresholds</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">📱</div>
          <h3>Web Interface</h3>
          <p>Access your system from any device with a browser</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">⚙️</div>
          <h3>Fully Configurable</h3>
          <p>Adjust settings on-the-fly without code changes</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">💚</div>
          <h3>Energy Efficient</h3>
          <p>Arduino-based solution with minimal power consumption</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">🔧</div>
          <h3>Easy Setup</h3>
          <p>Simple wiring and plug-and-play configuration</p>
        </div>
      </div>
    </div>
  </section>

  <!-- DASHBOARD SECTION -->
  <section id="dashboard">
    <div class="section-container">
      <h2 class="section-title">🎛️ System Dashboard</h2>
      
      <div class="dashboard-controls">
        <button id="connectBtn" class="btn btn-primary" onclick="connectArduino()">🔌 Connect Arduino</button>
        <div id="status" class="status-indicator disconnected">
          <span class="status-dot"></span>
          <span id="statusText">Disconnected</span>
        </div>
      </div>

      <div class="dashboard-grid">
        <!-- Moisture Card -->
        <div class="card">
          <div class="card-header">💧 Soil Moisture Level</div>
          <div class="card-body">
            <div class="big-value" id="moistureValue">0</div>
            <div class="progress-bar">
              <div class="progress-fill" id="progressFill" style="width: 0%;">
                <span id="moisturePercent">0%</span>
              </div>
            </div>
            <span class="status-badge badge-ok" id="moistureStatus">Loading...</span>
            <div class="info-grid">
              <div class="info-item">
                <div class="info-label">Raw Value</div>
                <div class="info-value" id="rawValue">---</div>
              </div>
              <div class="info-item">
                <div class="info-label">Threshold</div>
                <div class="info-value" id="thresholdValue">350</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Pump Card -->
        <div class="card">
          <div class="card-header">🚰 Pump Status</div>
          <div class="card-body">
            <div class="pump-indicator" id="pumpIndicator">🔴</div>
            <div style="text-align: center; font-size: 1.5em; font-weight: bold; margin: 1rem 0;" id="pumpStatusText">OFF</div>
            <div class="info-grid">
              <div class="info-item">
                <div class="info-label">Mode</div>
                <div class="info-value" id="pumpMode">AUTO</div>
              </div>
              <div class="info-item">
                <div class="info-label">Cycles</div>
                <div class="info-value" id="pumpCycles">0</div>
              </div>
            </div>
            <button class="btn btn-secondary" onclick="togglePump()" id="pumpBtn" disabled style="width: 100%; margin-top: 1rem;">Manual Control</button>
          </div>
        </div>

        <!-- System Info Card -->
        <div class="card">
          <div class="card-header">⚙️ System Info</div>
          <div class="card-body">
            <div class="info-grid">
              <div class="info-item">
                <div class="info-label">Uptime</div>
                <div class="info-value" id="uptime">0s</div>
              </div>
              <div class="info-item">
                <div class="info-label">Readings</div>
                <div class="info-value" id="readingCount">0</div>
              </div>
              <div class="info-item">
                <div class="info-label">Last Update</div>
                <div class="info-value" id="lastUpdate">---</div>
              </div>
              <div class="info-item">
                <div class="info-label">Connection</div>
                <div class="info-value" id="connectionStatus">---</div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Chart -->
      <div class="card" style="grid-column: 1 / -1;">
        <div class="card-header">📈 Moisture History</div>
        <div class="card-body">
          <div class="chart-container">
            <canvas id="moistureChart"></canvas>
          </div>
        </div>
      </div>

      <!-- Settings -->
      <div class="card" style="grid-column: 1 / -1;">
        <div class="card-header">⚙️ System Settings</div>
        <div class="card-body">
          <div class="settings-grid">
            <div class="setting-group">
              <label for="dryThreshold">Dry Threshold (< this = Water)</label>
              <input type="number" id="dryThreshold" value="350" min="0" max="1023">
              <small>Trigger pump when below this value</small>
            </div>
            <div class="setting-group">
              <label for="wetThreshold">Wet Threshold (> this = Stop)</label>
              <input type="number" id="wetThreshold" value="600" min="0" max="1023">
              <small>Stop pump when above this value</small>
            </div>
            <div class="setting-group">
              <label for="updateInterval">Update Interval (ms)</label>
              <input type="number" id="updateInterval" value="2000" min="500" max="10000" step="500">
              <small>How often to read sensor</small>
            </div>
            <div class="setting-group">
              <label for="pumpDuration">Max Pump Duration (seconds)</label>
              <input type="number" id="pumpDuration" value="10" min="1" max="60">
              <small>Safety timeout for pump</small>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- SETUP SECTION -->
  <section id="setup">
    <div class="section-container">
      <h2 class="section-title">📖 Setup Guide</h2>

      <div class="setup-section">
        <h2>🔧 Hardware Requirements</h2>
        <div class="components-list">
          <div class="component">
            <h3>Microcontroller</h3>
            <ul>
              <li>Arduino UNO/Nano/Mega</li>
              <li>Compatible board with USB serial</li>
            </ul>
          </div>
          <div class="component">
            <h3>Sensors & Actuators</h3>
            <ul>
              <li>Capacitive Soil Moisture Sensor</li>
              <li>5V Relay Module</li>
              <li>5V/12V Water Pump (Submersible)</li>
            </ul>
          </div>
          <div class="component">
            <h3>Other Components</h3>
            <ul>
              <li>Jumper Wires</li>
              <li>USB Cable (Type A to B)</li>
              <li>Power Supply (5V/12V)</li>
              <li>Vinyl Tubing</li>
            </ul>
          </div>
        </div>
      </div>

      <div class="setup-section">
        <h2>🔌 Wiring Diagram</h2>
        <div class="wiring-guide">
          <div class="wiring-item">
            <h3>Soil Moisture Sensor</h3>
            <code>
              VCC → Arduino 5V<br>
              GND → Arduino GND<br>
              AOUT → Arduino A0
            </code>
          </div>
          <div class="wiring-item">
            <h3>Relay Module</h3>
            <code>
              VCC → Arduino 5V<br>
              GND → Arduino GND<br>
              IN → Arduino D7
            </code>
          </div>
          <div class="wiring-item">
            <h3>Water Pump</h3>
            <code>
              (+) → Relay NO pin<br>
              (-) → GND<br>
              Relay COM → Pump Power Supply
            </code>
          </div>
        </div>
      </div>

      <div class="setup-section">
        <h2>💻 Arduino Code</h2>
        <p>Upload this code to your Arduino:</p>
        <pre><code>#define RELAY_PIN 7
#define MOISTURE_PIN A0
#define DRY_THRESHOLD 350
#define WET_THRESHOLD 600

void setup() {
  Serial.begin(9600);
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, HIGH);
}

void loop() {
  int moisture = analogRead(MOISTURE_PIN);
  Serial.print("Soil Moisture: ");
  Serial.println(moisture);
  
  if(moisture < DRY_THRESHOLD) {
    digitalWrite(RELAY_PIN, LOW);
    Serial.println("Pump: ON");
  } else if(moisture > WET_THRESHOLD) {
    digitalWrite(RELAY_PIN, HIGH);
    Serial.println("Pump: OFF");
  }
  
  delay(2000);
}</code></pre>
      </div>

      <div class="setup-section">
        <h2>📊 Sensor Calibration</h2>
        <div class="step-cards">
          <div class="step-card">
            <div class="step-num">1</div>
            <h4>Prepare</h4>
            <p>Upload the Arduino code and open Serial Monitor (9600 baud)</p>
          </div>
          <div class="step-card">
            <div class="step-num">2</div>
            <h4>Air Reading</h4>
            <p>Hold sensor in air - note the value (~800-900). This is your WET value.</p>
          </div>
          <div class="step-card">
            <div class="step-num">3</div>
            <h4>Dry Soil</h4>
            <p>Place sensor in completely dry soil - note the value (~200-350). This is your DRY value.</p>
          </div>
          <div class="step-card">
            <div class="step-num">4</div>
            <h4>Set Thresholds</h4>
            <p>Update DRY_THRESHOLD and WET_THRESHOLD in code or use dashboard settings.</p>
          </div>
        </div>
      </div>

      <div class="setup-section">
        <h2>🌐 Using the Dashboard</h2>
        <ol>
          <li>Open this webpage in <strong>Chrome/Edge/Opera</strong></li>
          <li>Click <strong>"🔌 Connect Arduino"</strong> button</li>
          <li>Select your Arduino port from the dialog</li>
          <li>Monitor real-time data and adjust settings as needed</li>
        </ol>
      </div>
    </div>
  </section>

  <!-- ABOUT SECTION -->
  <section id="about">
    <div class="section-container">
      <h2 class="section-title">About SmartWater</h2>

      <div class="setup-section">
        <h2>Our Mission</h2>
        <p>We believe every plant deserves the perfect amount of water, at the perfect time. SmartWater brings smart home technology to plant care, making it easy for anyone to maintain a thriving garden without guesswork.</p>
      </div>

      <div class="setup-section">
        <h2>What We Offer</h2>
        <div class="about-cards">
          <div class="about-card">
            <h3>🌱 Smart Automation</h3>
            <p>IoT-based automatic watering system that learns your plant's needs and adapts accordingly.</p>
          </div>
          <div class="about-card">
            <h3>📊 Real-Time Monitoring</h3>
            <p>Track soil moisture, temperature, and system status from anywhere with an internet connection.</p>
          </div>
          <div class="about-card">
            <h3>🔧 Easy Integration</h3>
            <p>Works with Arduino, ESP32, and other microcontrollers. Open-source code for full customization.</p>
          </div>
          <div class="about-card">
            <h3>💚 Eco-Friendly</h3>
            <p>Save water, reduce waste, and grow healthy plants with precision watering.</p>
          </div>
        </div>
      </div>

      <div class="setup-section">
        <h2>Technology Stack</h2>
        <ul class="tech-list">
          <li><strong>Hardware:</strong> Arduino Microcontroller, Soil Moisture Sensors, Relay Module, Water Pump</li>
          <li><strong>Frontend:</strong> HTML5, CSS3, JavaScript (Web Serial API)</li>
          <li><strong>Visualization:</strong> Chart.js for real-time data graphing</li>
          <li><strong>Communication:</strong> Serial UART Protocol (9600 baud)</li>
        </ul>
      </div>

      <div class="setup-section">
        <h2>Key Features</h2>
        <div class="about-cards">
          <div class="about-card">
            <h3>✅ Automatic Watering</h3>
            <p>Intelligent pump control based on soil moisture levels</p>
          </div>
          <div class="about-card">
            <h3>📈 Data Visualization</h3>
            <p>Beautiful real-time charts and historical data tracking</p>
          </div>
          <div class="about-card">
            <h3>⚙️ Fully Customizable</h3>
            <p>Adjust thresholds and settings on-the-fly</p>
          </div>
          <div class="about-card">
            <h3>🔒 Safe & Reliable</h3>
            <p>Built-in safety timeouts and error handling</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- FOOTER -->
  <footer>
    <p>&copy; 2026 SmartWater - Smart Plant Watering System | Arduino + Web Serial API</p>
  </footer>

  <script>
    let port, reader, isConnected = false, moistureHistory = [], chart, pumpActive = false, cycles = 0, startTime = 0, readingCount = 0;

    // Initialize Chart
    function initChart() {
      const ctx = document.getElementById('moistureChart').getContext('2d');
      chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: [],
          datasets: [{
            label: 'Soil Moisture Level',
            data: [],
            borderColor: '#667eea',
            backgroundColor: 'rgba(102, 126, 234, 0.1)',
            borderWidth: 3,
            fill: true,
            tension: 0.3,
            pointRadius: 4,
            pointBackgroundColor: '#667eea',
            pointBorderColor: '#fff',
            pointBorderWidth: 2
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: { legend: { display: true, position: 'top' } },
          scales: { y: { beginAtZero: true, max: 1023 } }
        }
      });
    }

    // Show Section
    function showSection(sectionId) {
      document.querySelectorAll('section').forEach(s => s.classList.remove('active'));
      document.getElementById(sectionId).classList.add('active');
      document.querySelectorAll('.nav-link').forEach(link => link.classList.remove('active'));
      event.target.classList.add('active');
      window.scrollTo(0, 0);
    }

    // Connect to Arduino
    async function connectArduino() {
      if (isConnected) {
        port.close();
        isConnected = false;
        updateConnectionStatus('Disconnected', false);
        document.getElementById('connectBtn').textContent = '🔌 Connect Arduino';
        document.getElementById('pumpBtn').disabled = true;
        return;
      }
      try {
        port = await navigator.serial.requestPort();
        await port.open({ baudRate: 9600 });
        isConnected = true;
        startTime = Date.now();
        readingCount = 0;
        cycles = 0;
        updateConnectionStatus('Connected', true);
        document.getElementById('connectBtn').textContent = '❌ Disconnect';
        document.getElementById('pumpBtn').disabled = false;
        readArduinoData();
        updateSystemTime();
      } catch (err) {
        alert('Failed to connect: ' + err.message);
      }
    }

    // Read Arduino Data
    async function readArduinoData() {
      reader = port.readable.getReader();
      try {
        while (isConnected) {
          const { value, done } = await reader.read();
          if (done) break;
          const text = new TextDecoder().decode(value);
          const lines = text.split('\n');
          for (let line of lines) {
            line = line.trim();
            if (line.includes('Soil Moisture:')) {
              const match = line.match(/Soil Moisture:\s*(\d+)/);
              if (match) {
                updateMoistureDisplay(parseInt(match[1]));
                readingCount++;
                document.getElementById('readingCount').textContent = readingCount;
              }
            }
            if (line.includes('Pump:')) {
              pumpActive = line.includes('ON');
              updatePumpDisplay();
            }
          }
        }
      } finally {
        reader.releaseLock();
      }
    }

    // Update Moisture Display
    function updateMoistureDisplay(moisture) {
      document.getElementById('moistureValue').textContent = moisture;
      document.getElementById('rawValue').textContent = moisture;
      const dry = parseInt(document.getElementById('dryThreshold').value);
      const wet = parseInt(document.getElementById('wetThreshold').value);
      const percent = Math.max(0, Math.min(100, ((moisture - dry) / (wet - dry)) * 100));
      document.getElementById('progressFill').style.width = percent + '%';
      document.getElementById('moisturePercent').textContent = Math.round(percent) + '%';
      document.getElementById('thresholdValue').textContent = dry;

      let badge = document.getElementById('moistureStatus');
      if (moisture < dry) {
        badge.textContent = '🔴 DRY';
        badge.className = 'status-badge badge-dry';
      } else if (moisture < wet) {
        badge.textContent = '🟡 OK';
        badge.className = 'status-badge badge-ok';
      } else {
        badge.textContent = '🟢 WET';
        badge.className = 'status-badge badge-wet';
      }

      const now = new Date().toLocaleTimeString();
      moistureHistory.push({ t: now, y: moisture });
      if (moistureHistory.length > 20) moistureHistory.shift();
      if (chart) {
        chart.data.labels = moistureHistory.map(v => v.t);
        chart.data.datasets[0].data = moistureHistory.map(v => v.y);
        chart.update();
      }
      document.getElementById('lastUpdate').textContent = now;
    }

    // Update Pump Display
    function updatePumpDisplay() {
      const ind = document.getElementById('pumpIndicator');
      const text = document.getElementById('pumpStatusText');
      if (pumpActive) {
        ind.textContent = '🟢';
        ind.classList.add('active');
        text.textContent = 'ON';
        text.style.color = '#00c853';
      } else {
        ind.textContent = '🔴';
        ind.classList.remove('active');
        text.textContent = 'OFF';
        text.style.color = '#333';
      }
    }

    // Toggle Pump
    function togglePump() {
      if (!isConnected) {
        alert('Arduino not connected!');
        return;
      }
      pumpActive = !pumpActive;
      cycles++;
      document.getElementById('pumpCycles').textContent = cycles;
      updatePumpDisplay();
    }

    // Update Connection Status
    function updateConnectionStatus(status, connected) {
      const statusDiv = document.getElementById('status');
      document.getElementById('statusText').textContent = status;
      if (connected) {
        statusDiv.classList.remove('disconnected');
        statusDiv.classList.add('connected');
      } else {
        statusDiv.classList.remove('connected');
        statusDiv.classList.add('disconnected');
      }
    }

    // Update System Time
    function updateSystemTime() {
      if (!isConnected) return;
      const elapsed = Math.floor((Date.now() - startTime) / 1000);
      const hours = Math.floor(elapsed / 3600);
      const minutes = Math.floor((elapsed % 3600) / 60);
      const seconds = elapsed % 60;
      let timeString = '';
      if (hours > 0) timeString += hours + 'h ';
      if (minutes > 0) timeString += minutes + 'm ';
      timeString += seconds + 's';
      document.getElementById('uptime').textContent = timeString;
      setTimeout(updateSystemTime, 1000);
    }

    // Initialize
    window.addEventListener('load', () => {
      initChart();
      document.getElementById('dryThreshold').addEventListener('change', () => {
        document.getElementById('thresholdValue').textContent = document.getElementById('dryThreshold').value;
      });
    });
  </script>
</body>
</html>
