<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Status Minecraft Serverů</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --online: #27ae60;
            --offline: #e74c3c;
            --maintenance: #f39c12;
            --unknown: #7f8c8d;
            --background: #f5f6fa;
            --card-bg: #ffffff;
            --text: #2c3e50;
            --border: #dfe6e9;
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            --hover-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Roboto', sans-serif;
            background-color: var(--background);
            color: var(--text);
            line-height: 1.6;
            padding: 0;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            text-align: center;
            margin-bottom: 40px;
            padding: 20px 0;
            border-bottom: 1px solid var(--border);
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            color: #2c3e50;
            font-weight: 700;
        }

        .subtitle {
            font-size: 1.1rem;
            color: #7f8c8d;
            font-weight: 300;
        }

        .status-overview {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 20px;
            margin-bottom: 40px;
        }

        .status-card {
            background: var(--card-bg);
            border-radius: 12px;
            box-shadow: var(--shadow);
            padding: 25px;
            flex: 1;
            min-width: 200px;
            text-align: center;
            transition: all 0.3s ease;
        }

        .status-card:hover {
            transform: translateY(-5px);
            box-shadow: var(--hover-shadow);
        }

        .status-card h3 {
            font-size: 1.1rem;
            margin-bottom: 15px;
            color: #7f8c8d;
            font-weight: 500;
        }

        .status-card p {
            font-size: 1.8rem;
            font-weight: 700;
        }

        .status-indicator {
            width: 16px;
            height: 16px;
            border-radius: 50%;
            display: inline-block;
            margin-right: 8px;
            vertical-align: middle;
        }

        .online {
            background-color: var(--online);
        }

        .offline {
            background-color: var(--offline);
        }

        .maintenance {
            background-color: var(--maintenance);
        }

        .unknown {
            background-color: var(--unknown);
        }

        .server-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
        }

        .server-item {
            background: var(--card-bg);
            border-radius: 12px;
            box-shadow: var(--shadow);
            padding: 25px;
            transition: all 0.3s ease;
            border-left: 6px solid var(--unknown);
            position: relative;
            overflow: hidden;
        }

        .server-item:hover {
            transform: translateY(-3px);
            box-shadow: var(--hover-shadow);
        }

        .server-item.online {
            border-left-color: var(--online);
        }

        .server-item.offline {
            border-left-color: var(--offline);
        }

        .server-item.maintenance {
            border-left-color: var(--maintenance);
        }

        .server-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .server-name {
            font-weight: 700;
            font-size: 1.3rem;
            color: var(--text);
        }

        .server-status {
            display: flex;
            align-items: center;
            font-weight: 500;
            font-size: 0.95rem;
        }

        .server-details {
            margin-top: 15px;
        }

        .detail-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 0.95rem;
        }

        .detail-label {
            color: #7f8c8d;
            font-weight: 400;
        }

        .detail-value {
            font-weight: 500;
            text-align: right;
        }

        .server-message {
            background: rgba(0, 0, 0, 0.03);
            padding: 12px;
            border-radius: 8px;
            margin-top: 15px;
            font-size: 0.9rem;
            border-left: 3px solid #bdc3c7;
        }

        .maintenance .server-message {
            border-left-color: var(--maintenance);
        }

        .offline .server-message {
            border-left-color: var(--offline);
        }

        .last-updated {
            text-align: center;
            padding: 20px;
            color: #7f8c8d;
            font-size: 0.9rem;
            border-top: 1px solid var(--border);
        }

        .loading {
            text-align: center;
            padding: 60px 20px;
            font-size: 1.1rem;
            color: #7f8c8d;
        }

        .spinner {
            border: 5px solid rgba(0, 0, 0, 0.1);
            width: 50px;
            height: 50px;
            border-radius: 50%;
            border-left-color: #3498db;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .status-badge {
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 500;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .status-badge.online {
            background-color: rgba(39, 174, 96, 0.1);
            color: var(--online);
        }

        .status-badge.offline {
            background-color: rgba(231, 76, 60, 0.1);
            color: var(--offline);
        }

        .status-badge.maintenance {
            background-color: rgba(243, 156, 18, 0.1);
            color: var(--maintenance);
        }

        .players-bar {
            height: 8px;
            background: #ecf0f1;
            border-radius: 4px;
            margin-top: 10px;
            overflow: hidden;
        }

        .players-bar-fill {
            height: 100%;
            background: var(--online);
            border-radius: 4px;
            transition: width 0.5s ease;
        }

        .refresh-btn {
            display: block;
            margin: 0 auto 30px;
            padding: 10px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 1rem;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .refresh-btn:hover {
            background-color: #2980b9;
        }

        @media (max-width: 768px) {
            .status-overview {
                flex-direction: column;
            }
            
            .status-card {
                min-width: 100%;
            }
            
            .server-list {
                grid-template-columns: 1fr;
            }
            
            h1 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Status Minecraft Serverů</h1>
            <p class="subtitle">Aktuální stav serverů</p>
        </header>

        <button id="refresh-btn" class="refresh-btn">Aktualizovat</button>

        <div class="status-overview">
            <div class="status-card">
                <h3>Celkový Stav</h3>
                <p id="all-systems-status">Načítání...</p>
            </div>
            <div class="status-card">
                <h3>Online</h3>
                <p id="online-count">0</p>
            </div>
            <div class="status-card">
                <h3>Offline</h3>
                <p id="offline-count">0</p>
            </div>
            <div class="status-card">
                <h3>Údržba</h3>
                <p id="maintenance-count">0</p>
            </div>
        </div>

        <div id="loading" class="loading">
            <div class="spinner"></div>
            <p>Načítám stav serverů...</p>
        </div>

        <div id="server-list" class="server-list" style="display: none;">
            <!-- Server items will be inserted here by JavaScript -->
        </div>

        <div id="last-updated" class="last-updated">
            Naposledy aktualizováno: <span id="update-time">nikdy</span>
        </div>
    </div>

    <script>
        // KONFIGURACE - ZDE NASTAVTE SVÉ SERVERY
        const CONFIG = {
            refreshInterval: 1 * 60 * 1000, // 3 minuty v milisekundách
            apiBaseUrl: 'https://api.mcsrvstat.us/2/', // Minecraft Server Status API
            servers: [
                {
                    name: "Proxy",
                    ip: "cod.hostify.cz:44550",
                    maintenance: false,
                    maintenanceMessage: ""
                },
                {
                    name: "Lobby",
                    ip: "axolotl.hostify.cz:43510",
                    maintenance: true,
                    maintenanceMessage: "opět online v 19:00"
                },
                {
                    name: "Oneblock",
                    ip: "armadillo.hostify.cz:46770",
                    maintenance: false,
                    maintenanceMessage: ""
                },
                {
                    name: "Event",
                    ip: "villager.hostify.cz:39740",
                    maintenance: false,
                    maintenanceMessage: ""
                }
            ]
        };

        // Formátování data a času
        function formatDateTime(isoString) {
            if (!isoString) return 'Neznámý čas';
            
            const date = new Date(isoString);
            return date.toLocaleString('cs-CZ', {
                day: '2-digit',
                month: '2-digit',
                year: 'numeric',
                hour: '2-digit',
                minute: '2-digit',
                second: '2-digit'
            });
        }

        // Získání dat o serveru z API
        async function fetchServerData(serverConfig) {
            if (serverConfig.maintenance) {
                return {
                    ...serverConfig,
                    online: false,
                    lastUpdated: new Date().toISOString()
                };
            }

            try {
                const response = await fetch(`${CONFIG.apiBaseUrl}${encodeURIComponent(serverConfig.ip)}`);
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                const data = await response.json();
                return {
                    ...serverConfig,
                    ...data,
                    lastUpdated: new Date().toISOString()
                };
            } catch (error) {
                console.error(`Chyba při načítání dat pro server ${serverConfig.name}:`, error);
                return {
                    ...serverConfig,
                    online: false,
                    error: error.message,
                    lastUpdated: new Date().toISOString()
                };
            }
        }

        // Získání dat o všech serverech
        async function fetchAllServers() {
            // Zobrazení načítání
            document.getElementById('loading').style.display = 'block';
            document.getElementById('server-list').style.display = 'none';
            
            // Načtení dat pro všechny servery
            const serverData = [];
            for (const server of CONFIG.servers) {
                const data = await fetchServerData(server);
                serverData.push(data);
            }
            
            return serverData;
        }

        // Aktualizace status page
        function updateStatusPage(servers) {
            if (!servers || servers.length === 0) return;
            
            // Spočítání stavů
            const onlineCount = servers.filter(s => s.online && !s.maintenance).length;
            const offlineCount = servers.filter(s => !s.online && !s.maintenance).length;
            const maintenanceCount = servers.filter(s => s.maintenance).length;
            
            // Aktualizace přehledových karet
            document.getElementById('all-systems-status').innerHTML = 
                `<span class="status-indicator ${
                    offlineCount === 0 && maintenanceCount === 0 ? 'online' : 
                    maintenanceCount > 0 ? 'maintenance' : 'offline'
                }"></span>
                ${
                    offlineCount === 0 && maintenanceCount === 0 ? 'Vše v pořádku' :
                    maintenanceCount > 0 ? 'Probíhá údržba' : 'Problémy'
                }`;
            
            document.getElementById('online-count').textContent = onlineCount;
            document.getElementById('offline-count').textContent = offlineCount;
            document.getElementById('maintenance-count').textContent = maintenanceCount;
            
            // Aktualizace seznamu serverů
            const serverList = document.getElementById('server-list');
            serverList.innerHTML = '';
            
            servers.forEach(server => {
                const playerPercentage = server.players && server.players.max ? 
                    Math.round((server.players.online / server.players.max) * 100) : 0;
                
                const serverItem = document.createElement('div');
                serverItem.className = `server-item ${
                    server.maintenance ? 'maintenance' : 
                    server.online ? 'online' : 'offline'
                }`;
                
                serverItem.innerHTML = `
                    <div class="server-header">
                        <div class="server-name">${server.name}</div>
                        <div class="server-status">
                            <span class="status-badge ${
                                server.maintenance ? 'maintenance' : 
                                server.online ? 'online' : 'offline'
                            }">
                                ${
                                    server.maintenance ? 'Údržba' : 
                                    server.online ? 'Online' : 'Offline'
                                }
                            </span>
                        </div>
                    </div>
                    
                    <div class="server-details">
                        ${server.online && !server.maintenance ? `
                        <div class="detail-row">
                            <span class="detail-label">Verze:</span>
                            <span class="detail-value">${server.version || 'Neznámá'}</span>
                        </div>
                        
                        <div class="detail-row">
                            <span class="detail-label">Hráči:</span>
                            <span class="detail-value">${server.players ? server.players.online : '?'}/${server.players ? server.players.max : '?'}</span>
                        </div>
                        
                        ${server.players && server.players.max ? `
                        <div class="players-bar">
                            <div class="players-bar-fill" style="width: ${playerPercentage}%"></div>
                        </div>
                        ` : ''}
                        ` : ''}
                        
                        <div class="detail-row">
                            <span class="detail-label">Aktualizováno:</span>
                            <span class="detail-value">${formatDateTime(server.lastUpdated)}</span>
                        </div>
                    </div>
                    
                    ${server.maintenance ? `
                    <div class="server-message">
                        <strong>Status:</strong> ${server.maintenanceMessage || 'Server je v údržbě'}
                    </div>
                    ` : !server.online ? `
                    <div class="server-message">
                        <strong>Status:</strong> Server je offline nebo nedostupný
                    </div>
                    ` : ''}
                `;
                
                serverList.appendChild(serverItem);
            });
            
            // Aktualizace času poslední aktualizace
            document.getElementById('update-time').textContent = formatDateTime(new Date().toISOString());
            
            // Skrytí načítání, zobrazení obsahu
            document.getElementById('loading').style.display = 'none';
            document.getElementById('server-list').style.display = 'grid';
        }

        // Načtení dat při kliknutí na tlačítko
        document.getElementById('refresh-btn').addEventListener('click', async () => {
            const servers = await fetchAllServers();
            if (servers) {
                updateStatusPage(servers);
            }
        });

        // Automatická aktualizace
        async function autoRefresh() {
            const servers = await fetchAllServers();
            if (servers) {
                updateStatusPage(servers);
            }
            
            // Nastavení periodické aktualizace
            window.refreshInterval = setInterval(async () => {
                const updatedServers = await fetchAllServers();
                if (updatedServers) {
                    updateStatusPage(updatedServers);
                }
            }, CONFIG.refreshInterval);
        }

        // Inicializace stránky
        document.addEventListener('DOMContentLoaded', () => {
            autoRefresh();
        });

        // Ukončení interval při opuštění stránky
        window.addEventListener('beforeunload', () => {
            if (window.refreshInterval) {
                clearInterval(window.refreshInterval);
            }
        });
    </script>
</body>
</html>
