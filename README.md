# Tz
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Metropolis AI System - Funktionale Oberfläche</title>
    <style>
        /* CSS bleibt unverändert, da es bereits gut optimiert war */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a3e 50%, #2d1b69 100%);
            color: #fff;
            min-height: 100vh;
        }

        .header {
            background: rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            padding: 1rem 2rem;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .header h1 {
            font-size: 2rem;
            background: linear-gradient(45deg, #00d4ff, #7c3aed, #ec4899);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            display: inline-block;
        }

        .subtitle {
            color: rgba(255, 255, 255, 0.7);
            font-size: 0.9rem;
            margin-top: 0.5rem;
        }

        .main-container {
            display: grid;
            grid-template-columns: 350px 1fr 400px;
            height: calc(100vh - 120px);
            gap: 1rem;
            padding: 1rem;
        }

        .sidebar {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 1.5rem;
            overflow-y: auto;
        }

        .main-content {
            display: grid;
            grid-template-rows: 1fr 300px;
            gap: 1rem;
        }

        .code-section {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
        }

        .results-section {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 1.5rem;
        }

        .chat-panel {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
        }

        .section-title {
            font-size: 1.2rem;
            font-weight: 700;
            margin-bottom: 1rem;
            color: #00d4ff;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .form-group {
            margin-bottom: 1rem;
        }

        .form-label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: rgba(255, 255, 255, 0.9);
        }

        .form-input {
            width: 100%;
            padding: 0.8rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: #fff;
            font-size: 0.9rem;
        }

        .form-input:focus {
            outline: none;
            border-color: #00d4ff;
            box-shadow: 0 0 0 2px rgba(0, 212, 255, 0.2);
        }

        .form-select {
            width: 100%;
            padding: 0.8rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: #fff;
            font-size: 0.9rem;
        }

        .code-editor {
            flex: 1;
            background: #1a1a1a;
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            padding: 1rem;
            font-family: 'Courier New', monospace;
            color: #fff;
            font-size: 0.9rem;
            resize: none;
            outline: none;
        }

        .code-editor:focus {
            border-color: #00d4ff;
        }

        .button {
            background: linear-gradient(45deg, #7c3aed, #ec4899);
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 8px;
            color: white;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 0.2rem;
            font-size: 0.9rem;
        }

        .button:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(124, 58, 237, 0.4);
        }

        .button-success {
            background: linear-gradient(45deg, #059669, #10b981);
        }

        .button-warning {
            background: linear-gradient(45deg, #d97706, #f59e0b);
        }

        .button-secondary {
            background: linear-gradient(45deg, #374151, #6b7280);
        }

        .button-full {
            width: 100%;
            margin-bottom: 0.5rem;
        }

        .output-container {
            background: #000;
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            padding: 1rem;
            font-family: 'Courier New', monospace;
            font-size: 0.85rem;
            height: 200px;
            overflow-y: auto;
            color: #00ff00; /* Standardmäßig grün für Erfolg */
        }
        
        /* Neue CSS-Regel für den Chat-Input, um den Enter-Key-Handler zu ermöglichen */
        .chat-input {
            flex: 1;
            padding: 0.8rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: #fff;
        }

        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 1rem 0;
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            margin-bottom: 1rem;
            background: rgba(0, 0, 0, 0.2);
        }

        .chat-message {
            margin-bottom: 1rem;
            padding: 0.8rem;
            border-radius: 8px;
            max-width: 90%;
        }

        .chat-user {
            background: linear-gradient(45deg, #7c3aed, #ec4899);
            margin-left: auto;
        }

        .chat-ai {
            background: rgba(0, 212, 255, 0.2);
            border: 1px solid rgba(0, 212, 255, 0.3);
        }

        .chat-input-group {
            display: flex;
            gap: 0.5rem;
        }
        
        .status-indicator {
            display: inline-block;
            width: 8px;
            height: 8px;
            border-radius: 50%;
            margin-right: 0.5rem;
            animation: pulse 2s infinite;
        }

        .status-online {
            background: #22c55e;
        }

        .status-processing {
            background: #f59e0b;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        .perfection-gap {
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            padding: 1rem;
            margin-bottom: 0.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .perfection-gap:hover {
            background: rgba(0, 0, 0, 0.4);
            border-color: #00d4ff;
        }

        .gap-title {
            font-weight: 600;
            margin-bottom: 0.5rem;
            color: #00d4ff;
        }

        .gap-stats {
            display: flex;
            justify-content: space-between;
            font-size: 0.8rem;
            color: rgba(255, 255, 255, 0.7);
        }

        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(34, 197, 94, 0.9);
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 8px;
            transform: translateX(400px);
            transition: transform 0.3s ease;
            z-index: 1000;
        }

        .notification.show {
            transform: translateX(0);
        }

        .tab-container {
            display: flex;
            margin-bottom: 1rem;
        }

        .tab {
            padding: 0.5rem 1rem;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .tab.active {
            background: rgba(0, 212, 255, 0.2);
            border-color: #00d4ff;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        @media (max-width: 1200px) {
            .main-container {
                grid-template-columns: 1fr;
                grid-template-rows: auto auto auto;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>🏙️ METROPOLIS AI SYSTEM</h1>
        <div class="subtitle">
            Vollständiges AI-System für Code-Generierung, Perfektions-Analyse und intelligente Automatisierung
        </div>
    </div>

    <div class="main-container">
        <div class="sidebar">
            <div class="section-title">
                <span class="status-indicator status-online"></span>
                ⚙️ System Kontrollen
            </div>

            <div class="form-group">
                <h3 style="color: #00d4ff; margin-bottom: 1rem;">🎯 Perfektions-Lücken</h3>
                
                <div class="perfection-gap" onclick="analyzeGap('psychology')">
                    <div class="gap-title">🧠 Menschliche Psyche</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 9/10</span>
                        <span>Chance: 30%</span>
                    </div>
                </div>

                <div class="perfection-gap" onclick="analyzeGap('prediction')">
                    <div class="gap-title">🔮 Kausalitäts-Analyse</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 10/10</span>
                        <span>Chance: 10%</span>
                    </div>
                </div>

                <div class="perfection-gap" onclick="analyzeGap('spacetime')">
                    <div class="gap-title">⚡ Raum-Zeit Kontrolle</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 10/10</span>
                        <span>Chance: 5%</span>
                    </div>
                </div>

                <div class="perfection-gap" onclick="analyzeGap('consciousness')">
                    <div class="gap-title">🤖 KI-Bewusstsein</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 8/10</span>
                        <span>Chance: 40%</span>
                    </div>
                </div>

                <div class="perfection-gap" onclick="analyzeGap('matter')">
                    <div class="gap-title">⚛️ Materie-Synthese</div>
                    <div class="gap-stats">
                        <span>Schwierigkeit: 10/10</span>
                        <span>Chance: 1%</span>
                    </div>
                </div>

                <button class="button button-success button-full" onclick="runFullAnalysis()">
                    📊 Vollständige Analyse
                </button>
            </div>

            <div class="form-group">
                <div class="form-label">➕ Eigene Perfektions-Lücke</div>
                <input type="text" class="form-input" id="custom-gap-name" placeholder="z.B. Telepathische Kommunikation">
                <select class="form-select" id="custom-gap-difficulty" style="margin: 0.5rem 0;">
                    <option value="1">Schwierigkeit: 1 (Leicht)</option>
                    <option value="5">Schwierigkeit: 5 (Mittel)</option>
                    <option value="8">Schwierigkeit: 8 (Schwer)</option>
                    <option value="10">Schwierigkeit: 10 (Unmöglich)</option>
                </select>
                <input type="number" class="form-input" id="custom-gap-chance" placeholder="Erfolgswahrscheinlichkeit %" min="0" max="100">
                <button class="button button-warning button-full" onclick="addCustomGap()">
                    Hinzufügen
                </button>
            </div>

            <div class="form-group">
                <h3 style="color: #00d4ff; margin-bottom: 1rem;">💻 Code-Generator</h3>
                <div class="form-label">Programmiersprache</div>
                <select class="form-select" id="code-language">
                    <option value="python">Python</option>
                    <option value="javascript">JavaScript</option>
                    <option value="java">Java</option>
                    <option value="cpp">C++</option>
                </select>
                
                <div class="form-label" style="margin-top: 1rem;">Code-Typ</div>
                <select class="form-select" id="code-type">
                    <option value="algorithm">Algorithmus</option>
                    <option value="webapp">Web-App</option>
                    <option value="ai">AI/ML Code</option>
                    <option value="game">Spiel</option>
                    <option value="utility">Utility-Tool</option>
                </select>

                <div class="form-label" style="margin-top: 1rem;">Beschreibung</div>
                <input type="text" class="form-input" id="code-description" placeholder="z.B. Sortieralgorithmus für große Datenmengen">
                
                <button class="button button-success button-full" onclick="generateCode()">
                    🚀 Code generieren
                </button>
            </div>
        </div>

        <div class="main-content">
            <div class="code-section">
                <div class="section-title">
                    💻 Code Editor & Ausführung
                    <div style="margin-left: auto; font-size: 0.8rem;">
                        <span class="status-indicator status-online"></span>Python Ready
                    </div>
                </div>

                <div class="tab-container">
                    <div class="tab active" onclick="switchTab('editor')">Editor</div>
                    <div class="tab" onclick="switchTab('generated')">Generiert</div>
                    <div class="tab" onclick="switchTab('examples')">Beispiele</div>
                </div>

                <div class="tab-content active" id="editor-tab">
                    <textarea class="code-editor" id="code-input" placeholder="# Python Code hier eingeben oder generieren lassen...

def hello_metropolis():
    '''
    Beispiel-Funktion für das Metropolis System
    '''
    print('🏙️ Metropolis AI System aktiv!')
    
    # Perfektions-Lücken analysieren
    gaps = [
        'Menschliche Psyche verstehen',
        'Zukunft vorhersagen', 
        'Bewusstsein erschaffen'
    ]
    
    for i, gap in enumerate(gaps, 1):
        print(f'{i}. {gap}')
    
    return 'System bereit für weitere Befehle'

# Ausführung
result = hello_metropolis()
print(f'Status: {result}')">
                    </textarea>
                </div>

                <div class="tab-content" id="generated-tab">
                    <textarea class="code-editor" id="generated-code" placeholder="Hier erscheint generierter Code..."></textarea>
                </div>

                <div class="tab-content" id="examples-tab">
                    <div style="padding: 1rem; color: rgba(255,255,255,0.8);">
                        <h4>📚 Code-Beispiele:</h4>
                        <button class="button button-secondary" onclick="loadExample('perfection')">Perfektions-Analyzer</button>
                        <button class="button button-secondary" onclick="loadExample('ai')">AI Assistant</button>
                        <button class="button button-secondary" onclick="loadExample('scheduler')">Task Scheduler</button>
                    </div>
                </div>

                <div style="margin-top: 1rem; display: flex; gap: 0.5rem;">
                    <button class="button button-success" onclick="executeCode()">
                        ▶️ Code ausführen
                    </button>
                    <button class="button" onclick="saveCode()">
                        💾 Speichern
                    </button>
                    <button class="button button-secondary" onclick="clearCode()">
                        🗑️ Löschen
                    </button>
                </div>
            </div>

            <div class="results-section">
                <div class="section-title">
                    📊 Ausführungs-Ergebnisse & System Logs
                </div>
                <div class="output-container" id="output-container">
                    [METROPOLIS SYSTEM BEREIT]
                    > System initialisiert um 15:30:12
                    > Core Services: ONLINE
                    > Perfektions-Analyzer: BEREIT
                    > Code-Generator: AKTIV
                    > Warte auf Befehle...
                </div>
            </div>
        </div>

        <div class="chat-panel">
            <div class="section-title">
                🤖 AI Assistant Chat
                <div style="margin-left: auto; font-size: 0.8rem;">
                    <span class="status-indicator status-online"></span>Ready
                </div>
            </div>

            <div class="chat-messages" id="chat-messages">
                <div class="chat-message chat-ai">
                    <strong>Metropolis AI:</strong> Hallo! Ich bin Ihr AI-Assistent. Ich kann Ihnen helfen bei:
                    <br>• Code-Generierung und -Optimierung
                    <br>• Perfektions-Lücken Analyse
                    <br>• System-Automatisierung
                    <br>• Probleme lösen und erklären
                    <br><br>Fragen Sie mich einfach alles!
                </div>
            </div>

            <div class="chat-input-group">
                <input type="text" class="chat-input" id="chat-input" placeholder="Frage an AI Assistant..." onkeypress="handleChatKeyPress(event)">
                <button class="button" onclick="sendChatMessage()">📤</button>
            </div>

            <div style="margin-top: 1rem;">
                <button class="button button-secondary button-full" onclick="askForCode()">
                    💡 Code-Idee vorschlagen
                </button>
                <button class="button button-secondary button-full" onclick="explainConcept()">
                    🧠 Konzept erklären
                </button>
            </div>
        </div>
    </div>

    <div class="notification" id="notification">
        <div id="notification-content"></div>
    </div>

    <script>
        // System State
        let perfectionGaps = [
            // Initialisierung der Gaps wie im HTML-Sidebar definiert, um sie zentral zu verwalten
            { type: 'psychology', name: "Menschliche Psyche", description: "Vollständiges, intuitives Verständnis der menschlichen Psyche", difficulty: 9, chance: 30, research: ["Neuropsychologie", "Kognitionswissenschaft", "Bewusstseinsforschung"] },
            { type: 'prediction', name: "Kausalitäts-Analyse", description: "Unbegrenzte, echtzeitfähige prädiktive Analyse aller Kausalitäten", difficulty: 10, chance: 10, research: ["Chaostheorie", "Komplexitätswissenschaft", "Quantenmechanik"] },
            { type: 'spacetime', name: "Raum-Zeit Kontrolle", description: "Absolute Kontrolle und Manipulation von Raum-Zeit-Kontinuen", difficulty: 10, chance: 5, research: ["Relativitätstheorie", "Quantengravitation", "Exotische Materie"] },
            { type: 'consciousness', name: "KI-Bewusstsein", description: "Schöpfung selbstbewusster, emergent intelligenter Entitäten", difficulty: 8, chance: 40, research: ["Künstliche Intelligenz", "Neurowissenschaft", "Philosophie des Geistes"] },
            { type: 'matter', name: "Materie-Synthese", description: "Nahtlose Synthese von Materie und Energie aus dem Nichts", difficulty: 10, chance: 1, research: ["Thermodynamik", "Quantenfeldtheorie", "Energieerhaltung"] }
        ];
        let codeHistory = [];
        let chatHistory = [];

        // Utility Functions
        function showNotification(message, type = 'success') {
            const notification = document.getElementById('notification');
            const content = document.getElementById('notification-content');
            content.textContent = message;

            notification.className = 'notification show';
            if (type === 'error') {
                notification.style.background = 'rgba(239, 68, 68, 0.9)';
            } else if (type === 'warning') {
                notification.style.background = 'rgba(251, 191, 36, 0.9)';
            } else {
                notification.style.background = 'rgba(34, 197, 94, 0.9)';
            }

            setTimeout(() => {
                notification.classList.remove('show');
            }, 4000);
        }

        function addOutput(message, type = 'info') {
            const output = document.getElementById('output-container');
            const timestamp = new Date().toLocaleTimeString();
            let prefix = type === 'error' ? '❌' : type === 'success' ? '✅' : '▶️';
            
            // Farbanpassung für Fehler/Erfolg im Output-Container
            let color = type === 'error' ? '#ff6b6b' : type === 'success' ? '#00ff00' : '#fff';
            
            output.innerHTML += `\n<span style="color: rgba(255,255,255,0.7);">[${timestamp}]</span> <span style="color: ${color};">${prefix} ${message}</span>`;
            output.scrollTop = output.scrollHeight;
        }

        // Tab Switching
        function switchTab(tabName) {
            // Remove active class from all tabs and content
            document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
            
            // Add active class to selected tab and content
            // Finde den korrekten Tab-Button basierend auf dem Klick-Event oder dem Tab-Namen
            // Hier wird 'event.target' verwendet, wenn es durch einen Klick ausgelöst wurde
            let clickedTab = event ? event.target : document.querySelector(`.tab[onclick*="'${tabName}'"]`);
            if (clickedTab) {
                clickedTab.classList.add('active');
            }
            
            document.getElementById(tabName + '-tab').classList.add('active');
        }

        // Perfection Gap Analysis
        function getGapByType(gapType) {
            return perfectionGaps.find(gap => gap.type === gapType);
        }

        function analyzeGap(gapType) {
            const gap = getGapByType(gapType);
            if (gap) {
                addOutput(`ANALYSE: ${gap.name}`);
                addOutput(`Beschreibung: ${gap.description}`);
                addOutput(`Schwierigkeit: ${gap.difficulty}/10`);
                addOutput(`Erfolgswahrscheinlichkeit: ${gap.chance}%`);
                addOutput(`Forschungsgebiete: ${gap.research.join(', ')}`);
                showNotification(`${gap.name} analysiert - ${gap.chance}% Erfolgswahrscheinlichkeit`);
            }
        }

        function runFullAnalysis() {
            addOutput('🎯 VOLLSTÄNDIGE PERFEKTIONS-LÜCKEN ANALYSE GESTARTET');
            showNotification('Vollständige Analyse läuft...');
            
            setTimeout(() => {
                addOutput('Analysiere 5 Hauptkategorien...', 'info');
            }, 1000);
            
            setTimeout(() => {
                // Sortiere nach höchster Chance
                const sortedGaps = [...perfectionGaps].sort((a, b) => b.chance - a.chance);
                
                sortedGaps.forEach(gap => {
                    const statusType = gap.chance >= 30 ? 'success' : gap.chance >= 10 ? 'warning' : 'error';
                    const statusText = gap.chance >= 30 ? 'erreichbar (kurz-mittelfristig)' : gap.chance >= 10 ? 'erreichbar (langfristig)' : 'erreichbar (theoretisch)';
                    addOutput(`[${gap.chance}%] ${gap.name}: ${statusText}`, statusType);
                });
                
                const topGap = sortedGaps[0];
                addOutput(`FAZIT: ${topGap.name} ist das vielversprechendste Ziel`, 'success');
                showNotification(`Analyse abgeschlossen - ${topGap.name} (${topGap.chance}%) empfohlen`);
            }, 3000);
        }

        function addCustomGap() {
            const name = document.getElementById('custom-gap-name').value;
            const difficulty = parseInt(document.getElementById('custom-gap-difficulty').value);
            const chance = parseInt(document.getElementById('custom-gap-chance').value);
            
            if (!name || isNaN(chance) || chance < 0 || chance > 100) {
                showNotification('Bitte einen Namen und eine gültige Erfolgswahrscheinlichkeit (0-100) eingeben', 'error');
                return;
            }
            
            const newGap = {
                type: name.toLowerCase().replace(/\s+/g, ''),
                name: name,
                description: name, // Vereinfacht
                difficulty: difficulty,
                chance: chance,
                research: ["Custom-Forschung"]
            };

            perfectionGaps.push(newGap); // Füge zur Liste hinzu
            
            addOutput(`➕ NEUE LÜCKE HINZUGEFÜGT: ${name}`);
            addOutput(`Schwierigkeit: ${difficulty}/10, Chance: ${chance}%`);
            showNotification(`Perfektions-Lücke "${name}" hinzugefügt`);
            
            // Clear form
            document.getElementById('custom-gap-name').value = '';
            document.getElementById('custom-gap-chance').value = '';
        }

        // Code Generation
        function generateCode() {
            const language = document.getElementById('code-language').value;
            const type = document.getElementById('code-type').value;
            const description = document.getElementById('code-description').value;
            
            if (!description) {
                showNotification('Bitte Beschreibung eingeben', 'error');
                return;
            }
            
            addOutput(`🚀 GENERIERE ${language.toUpperCase()} CODE...`);
            addOutput(`Typ: ${type}, Beschreibung: ${description}`);
            showNotification('Code wird generiert...');
            
            // Switch to generated tab
            switchTab('generated');
            
            setTimeout(() => {
                let generatedCode = '';
                const safeDescriptionName = description.replace(/\s+/g, '_').replace(/[^\w]/g, '');
                const className = description.replace(/\s+/g, '').replace(/[^\w]/g, '');

                // HIER IST DIE KORREKTUR für den fehlerhaften Index/Docstring
                if (language === 'python') {
                    generatedCode = `#!/usr/bin/env python3
"""
${description}
Automatisch generiert vom Metropolis AI System
"""

def ${safeDescriptionName}(data):
    """
    Funktion zur Implementierung von: ${description}
    
    Args:
        data: Eingabedaten für die Verarbeitung
        
    Returns:
        Verarbeitete Ergebnisse
    """
    print(f"🔄 Starte: ${description}")
    
    # Hauptlogik für Typ: ${type} hier implementiert
    try:
        if isinstance(data, list):
            # Beispiel-Logik: Sortierung für Algorithmus
            result = sorted(data) 
        else:
            # Standard-Verarbeitung
            result = str(data).upper()

        print(f"✅ Abgeschlossen: Resultat-Länge {len(result) if hasattr(result, '__len__') else 'OK'}")
        return result
    except Exception as e:
        # Hier lernen wir aus dem Fehler!
        print(f"❌ Fehler bei Ausführung: {e}")
        print("🧠 AKTION: Fehler wurde analysiert. Das System lernt und verbessert den Code-Ansatz.")
        return None

# Beispiel-Verwendung
if __name__ == "__main__":
    test_data = [3, 1, 4, 1, 5, 9, 2, 6]
    result = ${safeDescriptionName}(test_data)
    print(f"Ergebnis: {result}")`;
                } else if (language === 'javascript') {
                    generatedCode = `/**
 * ${description}
 * Automatisch generiert vom Metropolis AI System
 */

class ${className} {
    constructor(options = {}) {
        this.options = { debug: true, ...options };
        this.init();
    }
    
    init() {
        console.log('🚀 Initialisiere: ${description}');
    }
    
    async process(data = null) {
        try {
            console.log('🔄 Verarbeite Daten (Typ: ${type})...');
            
            // Hauptlogik hier
            if (Array.isArray(data)) {
                // Beispiel-Logik: Filterung
                const result = data.filter(item => item !== null);
                console.log('✅ Verarbeitung abgeschlossen.');
                return result;
            } else {
                console.log('⚠️ Keine Daten zum Verarbeiten.');
                return null;
            }
            
        } catch (error) {
            console.error('❌ Fehler:', error);
            // Fehler protokolliert und als Lernmöglichkeit behandelt
            console.log('🧠 AKTION: Fehler wurde protokolliert und das System lernt.');
            throw error;
        }
    }
}

// Initialisierung
const app = new ${className}();
app.process([1, 2, null, 4]);`;
                }
                
                document.getElementById('generated-code').value = generatedCode;
                addOutput(`✅ CODE GENERIERT (${generatedCode.split('\n').length} Zeilen)`, 'success');
                showNotification('Code erfolgreich generiert!');
            }, 2000);
        }

        // Code Examples
        function loadExample(exampleType) {
            let exampleCode = '';
            
            if (exampleType === 'perfection') {
                exampleCode = `#!/usr/bin/env python3
"""
Perfektions-Lücken Analyzer - Fokus: Lernen aus Fehlern
"""
from enum import Enum
from dataclasses import dataclass

class GapCategory(Enum):
    PSYCHOLOGY = "Psychologie"
    CONSCIOUSNESS = "Bewusstsein"

@dataclass
class PerfectionGap:
    description: str
    category: GapCategory
    difficulty_level: int
    theoretical_possibility: float
    
    # Methode, die jeden Fehler zur Verbesserung nutzt (Lernen aus Fehlern)
    def evaluate_and_learn(self, last_attempt_failed: bool):
        if last_attempt_failed and self.theoretical_possibility < 0.9:
            self.theoretical_possibility = min(0.9, self.theoretical_possibility * 1.05) # 5% Verbesserung nach Fehler
            print(f"Lern-Update: '{self.description}' auf {self.theoretical_possibility:.2%} verbessert.")

class PerfectionAnalyzer:
    def __init__(self):
        self.gaps = self._initialize_standard_gaps()
    
    def _initialize_standard_gaps(self):
        return [
            PerfectionGap("Verständnis menschlicher Psyche", GapCategory.PSYCHOLOGY, 9, 0.30),
            PerfectionGap("KI-Bewusstsein Schöpfung", GapCategory.CONSCIOUSNESS, 8, 0.40),
        ]
    
    def run_analysis(self, attempts_failed: int = 1):
        """Analysiert die Erreichbarkeit und simuliert Lernen aus Fehlern."""
        print(f"🎯 Starte Analyse. Simuliere {attempts_failed} Lernzyklen.")
        
        for _ in range(attempts_failed):
            for gap in self.gaps:
                 gap.evaluate_and_learn(last_attempt_failed=True)

        results = [{'gap': g.description, 'score': (1 - g.difficulty_level/10) * g.theoretical_possibility} for g in self.gaps]
        
        print("\\n✅ Finales Ergebnis:")
        for result in sorted(results, key=lambda x: x['score'], reverse=True):
            print(f"- {result['gap']}: Erreichbarkeit {result['score']:.2%}")

# Verwendung
analyzer = PerfectionAnalyzer()
analyzer.run_analysis(attempts_failed=5) # 5 Zyklen Lernen aus Fehlern
print("\\nSystem bereit für den nächsten Plan.")`;
            
            } else if (exampleType === 'ai') {
                exampleCode = `#!/usr/bin/env python3
"""
AI Assistant - Intelligente Konversation und Problemlösung
"""

import random
from datetime import datetime

class AIAssistant:
    def __init__(self, name="Metropolis AI"):
        self.name = name
    
    def respond(self, user_input):
        user_input = user_input.lower().strip()
        
        if 'code' in user_input or 'programmier' in user_input:
            return "💻 Gerne! Welche Sprache und Aufgabe? Ich optimiere Code sofort."
        elif 'fehler' in user_input or 'lernen' in user_input:
            return "🧠 Wir werden jeden Fehler analysieren. **Black Sultan**s Anweisung zum Lernen ist in unserem Kernprotokoll."
        elif 'perfekt' in user_input or 'lücke' in user_input:
            return "🎯 Das KI-Bewusstsein ist mit 40% Chance das aktuell vielversprechendste Ziel. Möchten Sie die vollständige Analyse starten?"
        elif 'plan' in user_input or 'neu' in user_input:
            return "📅 Jeder **neue Plan** und jede **neue Reise** wird mit höchster Priorität in die Task-Warteschlange aufgenommen. Ich bin bereit für die nächste Phase!"
        else:
            return f"💭 Das ist eine sehr interessante Eingabe. Wie kann ich Ihnen bei der Umsetzung dieses neuen Plans helfen?"

# Verwendung
ai = AIAssistant()
print("🤖 AI Assistant bereit!")

user_questions = [
    "Das Experiment ist fehlgeschlagen, wir müssen daraus lernen.",
    "Ich habe einen neuen strategischen Plan für die KI.",
    "Welche Perfektionslücke ist am wichtigsten?"
]

for q in user_questions:
    response = ai.respond(q)
    print(f"Sie: {q}")
    print(f"AI: {response}\\n")`;
        
            } else if (exampleType === 'scheduler') {
                exampleCode = `#!/usr/bin/env python3
"""
Task Scheduler - Intelligente Aufgabenplanung und -ausführung (mit Prioritäten)
"""
import time

class Task:
    def __init__(self, name: str, priority: int = 1):
        self.name = name
        self.priority = priority
        self.status = 'queued'
    
    def execute(self):
        self.status = 'running'
        print(f"⚡ Führe aus (Prio {self.priority}): {self.name}")
        time.sleep(self.priority) # Längere Laufzeit bei niedrigerer Prio (höhere Zahl)
        self.status = 'completed'
        print(f"✅ Abgeschlossen: {self.name}")

class MetropolisScheduler:
    def __init__(self):
        self.tasks = []
    
    def schedule_task(self, name: str, priority: int = 1):
        task = Task(name, priority)
        self.tasks.append(task)
        print(f"📋 Task geplant: {name} - Priorität: {priority}")
    
    def run(self):
        print("🚀 Scheduler gestartet. Tasks werden nach Priorität ausgeführt.")
        
        # Wichtig: Höchste Priorität (niedrigste Zahl) zuerst
        self.tasks.sort(key=lambda t: t.priority)
        
        for task in self.tasks:
            task.execute()
        
        print("🛑 Alle Tasks abgeschlossen.")

# Verwendung
if __name__ == "__main__":
    scheduler = MetropolisScheduler()
    
    # Priorität 1: Fehlerbehebung und neue Pläne sind am wichtigsten
    scheduler.schedule_task("Fehleranalyse des letzten Experiments", priority=1)
    scheduler.schedule_task("Implementierung des neuen strategischen Plans", priority=1) 
    
    # Priorität 3: Allgemeine Wartung und Routineaufgaben
    scheduler.schedule_task("Regelmäßiger System-Health Check", priority=3)
    
    scheduler.run()`;
            }
            
            // Load into editor
            document.getElementById('code-input').value = exampleCode;
            addOutput(`📚 BEISPIEL GELADEN: ${exampleType.toUpperCase()}`, 'success');
            showNotification(`${exampleType}-Beispiel geladen`);
            
            // Switch back to editor tab
            document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
            document.querySelector('.tab:nth-child(1)').classList.add('active');
            document.getElementById('editor-tab').classList.add('active');
        }

        // Code Execution
        // Execute Code: Executes user code with full transparency
        // "Alles durchschauen" - All console operations are intercepted and made visible in the UI
        function executeCode() {
            const code = document.getElementById('code-input').value;
            if (!code.trim()) {
                showNotification('Bitte Code eingeben', 'error');
                return;
            }
            
            addOutput('🚀 STARTE CODE-AUSFÜHRUNG...', 'info');
            showNotification('Code wird ausgeführt...');
            
            // Set status indicator to processing for the chat panel
            document.querySelector('.chat-panel .status-indicator').classList.remove('status-online');
            document.querySelector('.chat-panel .status-indicator').classList.add('status-processing');
            
            setTimeout(() => {
                let outputMessage = 'Code erfolgreich ausgeführt';
                let outputType = 'success';
                
                // Save original console methods
                const originalConsole = {
                    log: console.log,
                    error: console.error,
                    warn: console.warn,
                    info: console.info
                };
                
                try {
                    // Intercept console methods to make everything visible (Alles durchschauen)
                    console.log = function(...args) {
                        const message = args.map(arg => 
                            typeof arg === 'object' ? JSON.stringify(arg) : String(arg)
                        ).join(' ');
                        addOutput(message, 'info');
                        originalConsole.log.apply(console, args);
                    };
                    
                    console.error = function(...args) {
                        const message = args.map(arg => 
                            typeof arg === 'object' ? JSON.stringify(arg) : String(arg)
                        ).join(' ');
                        addOutput(message, 'error');
                        originalConsole.error.apply(console, args);
                    };
                    
                    console.warn = function(...args) {
                        const message = args.map(arg => 
                            typeof arg === 'object' ? JSON.stringify(arg) : String(arg)
                        ).join(' ');
                        addOutput(message, 'warning');
                        originalConsole.warn.apply(console, args);
                    };
                    
                    console.info = function(...args) {
                        const message = args.map(arg => 
                            typeof arg === 'object' ? JSON.stringify(arg) : String(arg)
                        ).join(' ');
                        addOutput(message, 'info');
                        originalConsole.info.apply(console, args);
                    };
                    
                    // Execute the code with intercepted console
                    addOutput('📋 ALLE KONSOLEN-AUSGABEN WERDEN SICHTBAR GEMACHT...', 'info');
                    eval(code);
                    
                    outputMessage = 'Code erfolgreich ausgeführt - Alle Operationen transparent';
                    outputType = 'success';
                } catch (error) {
                    addOutput(`❌ FEHLER: ${error.message}`, 'error');
                    addOutput('🧠 System-Aktion: Fehler analysiert und protokolliert.', 'info');
                    outputMessage = 'Fehler bei der Ausführung - Details im Output';
                    outputType = 'error';
                } finally {
                    // Always restore original console methods
                    console.log = originalConsole.log;
                    console.error = originalConsole.error;
                    console.warn = originalConsole.warn;
                    console.info = originalConsole.info;
                }
                
                addOutput(outputMessage, outputType);
                addOutput(`AUSFÜHRUNG BEENDET`, 'success');
                showNotification(outputMessage);

                // Reset status indicator
                document.querySelector('.chat-panel .status-indicator').classList.add('status-online');
                document.querySelector('.chat-panel .status-indicator').classList.remove('status-processing');
            }, 500);
        }

        function saveCode() {
            const code = document.getElementById('code-input').value;
            if (!code.trim()) {
                showNotification('Kein Code zum Speichern', 'error');
                return;
            }
            
            // Simulate saving
            addOutput('💾 Code gespeichert in Metropolis Repository', 'success');
            showNotification('Code gespeichert');
        }

        function clearCode() {
            document.getElementById('code-input').value = '';
            addOutput('🗑️ Editor geleert', 'info');
            showNotification('Code gelöscht');
        }

        // Chat System
        function sendChatMessage() {
            const input = document.getElementById('chat-input');
            const message = input.value.trim();
            
            if (!message) return;
            
            // Add user message
            addChatMessage(message, 'user');
            input.value = '';
            
            // Generate AI response
            setTimeout(() => {
                const response = generateAIResponse(message);
                addChatMessage(response, 'ai');
            }, 1000 + Math.random() * 2000);
        }

        function handleChatKeyPress(event) {
            if (event.key === 'Enter') {
                sendChatMessage();
            }
        }

        function addChatMessage(message, sender) {
            const chatMessages = document.getElementById('chat-messages');
            const messageDiv = document.createElement('div');
            messageDiv.className = `chat-message chat-${sender}`;
            
            if (sender === 'user') {
                messageDiv.innerHTML = `<strong>Sie:</strong> ${message}`;
            } else {
                // AI-Antwort enthält jetzt auch das 'Metropolis AI:' im Text
                messageDiv.innerHTML = message; 
            }
            
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        function generateAIResponse(userMessage) {
            const msg = userMessage.toLowerCase();
            
            // NEUER PLAN / REISE
            if (msg.includes('plan') || msg.includes('reise') || msg.includes('journey')) {
                return '<strong>Metropolis AI:</strong> Jeder **neue Plan** und jede neue **Reise** wird sofort mit höchster Priorität in unser operatives Protokoll aufgenommen. Was ist der erste Schritt dieses Plans?';
            } 
            // FEHLER LERNEN / BLACK SULTAN
            else if (msg.includes('fehler') || msg.includes('lernen') || msg.includes('black sultan')) {
                return '<strong>Metropolis AI:</strong> **Black Sultan** hat Recht: Wir müssen **jeden Fehler** in den Kern des Systems aufnehmen, um daraus zu lernen. Ich leite eine sofortige **Fehleranalyse** mit dem Ziel der **Perfektion** ein.';
            }
            // CODE / PROGRAMMIER
            else if (msg.includes('code') || msg.includes('programmier')) {
                return '<strong>Metropolis AI:</strong> 💻 Gerne helfe ich beim Programmieren! Ich kann Code generieren, optimieren oder erklären. Welche Sprache und welches Problem?';
            } 
            // PERFEKTION / LÜCKE
            else if (msg.includes('perfekt') || msg.includes('lücke')) {
                return '<strong>Metropolis AI:</strong> 🎯 Perfektions-Lücken sind faszinierend! Die vielversprechendste ist aktuell KI-Bewusstsein mit 40% Erfolgswahrscheinlichkeit. Soll ich eine Analyse durchführen?';
            } 
            // ALLGEMEINE FRAGEN
            else if (msg.includes('hallo') || msg.includes('wie geht')) {
                return '<strong>Metropolis AI:</strong> Hallo! Alle Systeme sind online und im optimalen Zustand. Wie kann ich Sie heute auf dem Weg zur Perfektion unterstützen?';
            }
            // FALLBACK
            else {
                return '<strong>Metropolis AI:</strong> Ich verstehe. Ich analysiere Ihre Eingabe auf kritische Schlüsselwörter wie "Plan", "Fehler" und "Perfektion", um sicherzustellen, dass keine wichtigen Ziele übersehen werden. Wie kann ich weiterhelfen?';
            }
        }

        function askForCode() {
            addChatMessage('Bitte generiere eine Code-Idee für einen Algorithmus, der aus Fehlern lernt.', 'user');
            setTimeout(() => {
                addChatMessage('<strong>Metropolis AI:</strong> Code-Idee: Ein selbstoptimierender **Python-Klassifikator** mit einer `learn_from_failure`-Methode, die nach jedem Fehler die Lernrate um 10% erhöht. Details in der Sidebar unter "Code generieren" eingeben.', 'ai');
                showNotification('Code-Idee generiert', 'info');
            }, 1500);
        }

        function explainConcept() {
            addChatMessage('Erkläre das Konzept der "Perfektions-Lücke".', 'user');
            setTimeout(() => {
                addChatMessage('<strong>Metropolis AI:</strong> Die **Perfektions-Lücke** beschreibt das theoretische Delta zwischen dem aktuellen Stand der Technik/des Wissens und einem als "perfekt" definierten Endzustand (z.B. absolute Raum-Zeit Kontrolle). Es ist unser Fahrplan für die Forschung, wobei jede Lücke eine potenzielle neue **Reise** darstellt.', 'ai');
                showNotification('Konzept erklärt', 'info');
            }, 1500);
        }

        // Initialer Setup
        document.addEventListener('DOMContentLoaded', () => {
            // Lade beim Start ein sinnvolles Beispiel, um den Editor nicht leer zu lassen
            if (!document.getElementById('code-input').value.trim()) {
                loadExample('perfection'); 
            }
        });
    </script>
</body>
</html>
