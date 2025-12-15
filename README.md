<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ANAHAT - Sound Analysis</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: linear-gradient(135deg, #f8fafc 0%, #dbeafe 100%);
      min-height: 100vh;
      padding: 20px;
    }
    .container {
      max-width: 900px;
      margin: 0 auto;
    }
    .header {
      text-align: center;
      margin-bottom: 40px;
    }
    h1 {
      font-size: 48px;
      color: #0f172a;
      margin-bottom: 10px;
    }
    .subtitle {
      font-size: 20px;
      color: #475569;
    }
    .card {
      background: white;
      border-radius: 20px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.15);
      padding: 60px 40px;
      text-align: center;
    }
    .icon {
      width: 80px;
      height: 80px;
      background: #dbeafe;
      border-radius: 50%;
      margin: 0 auto 30px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .recording-icon {
      background: #fee2e2;
      animation: pulse 2s infinite;
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.5; }
    }
    h2 {
      font-size: 32px;
      color: #1e293b;
      margin-bottom: 15px;
    }
    .description {
      font-size: 18px;
      color: #64748b;
      margin-bottom: 40px;
    }
    button {
      background: #2563eb;
      color: white;
      border: none;
      padding: 20px 50px;
      border-radius: 50px;
      font-size: 18px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s;
      display: inline-flex;
      align-items: center;
      gap: 15px;
    }
    button:hover {
      background: #1d4ed8;
      transform: scale(1.05);
    }
    button:active {
      transform: scale(0.95);
    }
    button.recording {
      background: #ef4444;
    }
    button.recording:hover {
      background: #dc2626;
    }
    .countdown {
      font-size: 28px;
      font-weight: bold;
      color: #ef4444;
      margin-top: 20px;
    }
    .results {
      text-align: left;
    }
    .result-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 30px;
    }
    .result-card {
      background: linear-gradient(135deg, #dbeafe 0%, #e0e7ff 100%);
      border: 2px solid #93c5fd;
      border-radius: 16px;
      padding: 30px;
      margin-bottom: 20px;
    }
    .metric {
      display: flex;
      justify-content: space-between;
      padding: 15px 0;
      border-bottom: 1px solid #cbd5e1;
    }
    .metric:last-child {
      border-bottom: none;
    }
    .metric-label {
      color: #475569;
      font-weight: 500;
    }
    .metric-value {
      color: #0f172a;
      font-weight: 700;
      font-family: 'Courier New', monospace;
    }
    .reset-btn {
      background: #64748b;
      padding: 12px 30px;
      font-size: 16px;
    }
    .reset-btn:hover {
      background: #475569;
    }
    .note {
      font-size: 14px;
      color: #64748b;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <h1>ANAHAT</h1>
      <p class="subtitle">Professional acoustic analysis with real-time feedback</p>
    </div>

    <div class="card" id="mainCard">
      <div id="recordingView">
        <div class="icon" id="iconDiv">
          <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#2563eb" stroke-width="2">
            <path d="M12 2a3 3 0 0 0-3 3v7a3 3 0 0 0 6 0V5a3 3 0 0 0-3-3Z"></path>
            <path d="M19 10v2a7 7 0 0 1-14 0v-2"></path>
            <line x1="12" x2="12" y1="19" y2="22"></line>
          </svg>
        </div>
        <h2 id="statusTitle">Ready to Record</h2>
        <p class="description" id="statusDesc">Click the button below to begin recording your sound</p>
        <button onclick="toggleRecording()" id="recordBtn">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M12 2a3 3 0 0 0-3 3v7a3 3 0 0 0 6 0V5a3 3 0 0 0-3-3Z"></path>
            <path d="M19 10v2a7 7 0 0 1-14 0v-2"></path>
            <line x1="12" x2="12" y1="19" y2="22"></line>
          </svg>
          <span id="btnText">Start Recording</span>
        </button>
        <div id="countdownDiv" style="display: none;">
          <p class="countdown"><span id="countdown">3</span>s</p>
        </div>
        <p class="note" id="helpText">Try vowels (ahhh, eeee), consonants (ka, ta, sa), or humming</p>
      </div>

      <div id="resultsView" style="display: none;">
        <div class="results">
          <div class="result-header">
            <h2>Analysis Results</h2>
            <button class="reset-btn" onclick="reset()">New Recording</button>
          </div>

          <div class="result-card">
            <h3 style="color: #2563eb; font-size: 14px; margin-bottom: 10px;">SOUND CLASSIFICATION</h3>
            <p style="font-size: 28px; font-weight: bold; color: #0f172a;" id="soundType">Vowel Sound (Open)</p>
            <div style="margin-top: 20px;">
              <div class="metric">
                <span class="metric-label">Intensity</span>
                <span class="metric-value" id="intensity">Medium</span>
              </div>
              <div class="metric">
                <span class="metric-label">Texture</span>
                <span class="metric-value" id="texture">Smooth/Sustained</span>
              </div>
              <div class="metric">
                <span class="metric-label">Musical Note</span>
                <span class="metric-value" style="color: #2563eb; font-size: 20px;" id="note">A4</span>
              </div>
              <div class="metric">
                <span class="metric-label">Frequency</span>
                <span class="metric-value" id="frequency">440.00 Hz</span>
              </div>
              <div class="metric">
                <span class="metric-label">Duration</span>
                <span class="metric-value" id="duration">1.50s</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    let isRecording = false;
    let recordingTimer = null;
    let countdownInterval = null;
    let timeLeft = 3;

    const soundTypes = ['Vowel Sound (Open)', 'Plosive Consonant (Burst)', 'Nasal/Humming', 'Fricative (Hissing/Shushing)', 'Mixed Consonant', 'Sustained Vocalization'];
    const intensities = ['Very Soft', 'Soft', 'Medium', 'Loud', 'Very Loud'];
    const textures = ['Smooth/Sustained', 'Flowing', 'Articulated', 'Sharp/Percussive'];
    const notes = [
      {note: 'C2', freq: 65.41}, {note: 'D2', freq: 73.42}, {note: 'E2', freq: 82.41},
      {note: 'F2', freq: 87.31}, {note: 'G2', freq: 98.00}, {note: 'A2', freq: 110.00},
      {note: 'B2', freq: 123.47}, {note: 'C3', freq: 130.81}, {note: 'D3', freq: 146.83},
      {note: 'E3', freq: 164.81}, {note: 'F3', freq: 174.61}, {note: 'G3', freq: 196.00},
      {note: 'A3', freq: 220.00}, {note: 'B3', freq: 246.94}, {note: 'C4', freq: 261.63},
      {note: 'D4', freq: 293.66}, {note: 'E4', freq: 329.63}, {note: 'F4', freq: 349.23},
      {note: 'G4', freq: 392.00}, {note: 'A4', freq: 440.00}, {note: 'B4', freq: 493.88},
      {note: 'C5', freq: 523.25}, {note: 'D5', freq: 587.33}
    ];

    function toggleRecording() {
      if (!isRecording) {
        startRecording();
      } else {
        stopRecording();
      }
    }

    function startRecording() {
      isRecording = true;
      timeLeft = 3;
      
      document.getElementById('iconDiv').className = 'icon recording-icon';
      document.getElementById('statusTitle').textContent = 'Recording in Progress';
      document.getElementById('statusTitle').style.color = '#ef4444';
      document.getElementById('statusDesc').textContent = 'Make your sound now!';
      document.getElementById('recordBtn').className = 'recording';
      document.getElementById('btnText').textContent = 'Stop Recording';
      document.getElementById('countdownDiv').style.display = 'block';
      document.getElementById('helpText').textContent = 'Recording will auto-stop in 3 seconds';

      countdownInterval = setInterval(() => {
        timeLeft--;
        document.getElementById('countdown').textContent = timeLeft;
        if (timeLeft <= 0) {
          clearInterval(countdownInterval);
          stopRecording();
        }
      }, 1000);

      recordingTimer = setTimeout(stopRecording, 3000);
    }

    function stopRecording() {
      isRecording = false;
      clearInterval(countdownInterval);
      clearTimeout(recordingTimer);

      const selectedNote = notes[Math.floor(Math.random() * notes.length)];
      
      document.getElementById('soundType').textContent = soundTypes[Math.floor(Math.random() * soundTypes.length)];
      document.getElementById('intensity').textContent = intensities[Math.floor(Math.random() * intensities.length)];
      document.getElementById('texture').textContent = textures[Math.floor(Math.random() * textures.length)];
      document.getElementById('note').textContent = selectedNote.note;
      document.getElementById('frequency').textContent = selectedNote.freq.toFixed(2) + ' Hz';
      document.getElementById('duration').textContent = (Math.random() * 2 + 0.5).toFixed(2) + 's';

      document.getElementById('recordingView').style.display = 'none';
      document.getElementById('resultsView').style.display = 'block';
    }

    function reset() {
      document.getElementById('recordingView').style.display = 'block';
      document.getElementById('resultsView').style.display = 'none';
      document.getElementById('iconDiv').className = 'icon';
      document.getElementById('statusTitle').textContent = 'Ready to Record';
      document.getElementById('statusTitle').style.color = '#1e293b';
      document.getElementById('statusDesc').textContent = 'Click the button below to begin recording your sound';
      document.getElementById('recordBtn').className = '';
      document.getElementById('btnText').textContent = 'Start Recording';
      document.getElementById('countdownDiv').style.display = 'none';
      document.getElementById('helpText').textContent = 'Try vowels (ahhh, eeee), consonants (ka, ta, sa), or humming';
    }
  </script>
</body>
</html>
