# trpstar.exe
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>sza.gurl Fixed Dashboard</title>
    <style>
        /* --- Root Variables & Colors --- */
        :root {
            --bg-deep: #050509;
            --bg-panel: #0d0d17;
            --neon-blue: #00f2ff;
            --neon-purple: #9d00ff;
            --neon-pink: #ff007f;
            --text-main: #e0e0e0;
            --text-muted: #888899;
            --panel-border: 1px solid rgba(255, 255, 255, 0.05);
            --glow-blue: 0 0 15px rgba(0, 242, 255, 0.6);
            --font-main: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            --font-mono: 'Courier New', Courier, monospace;
        }

        /* --- Global Styles --- */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            background-color: var(--bg-deep);
            color: var(--text-main);
            font-family: var(--font-main);
            overflow-x: hidden;
            display: flex;
        }

        /* --- Sidebar Navigation --- */
        .sidebar {
            width: 260px;
            height: 100vh;
            background-color: var(--bg-panel);
            border-right: var(--panel-border);
            display: flex;
            flex-direction: column;
            padding: 30px;
            position: fixed;
            z-index: 10;
        }

        /* Ginawang Neon Pink at Monospace para sa TRPSTAR.EXE branding */
        .brand-logo {
            font-size: 22px;
            font-weight: bold;
            color: var(--neon-pink);
            text-shadow: 0 0 15px rgba(255, 0, 127, 0.6);
            margin-bottom: 50px;
            font-family: var(--font-mono);
            letter-spacing: 1px;
        }

        .nav-links { list-style: none; flex-grow: 1; }
        .nav-links li { margin-bottom: 25px; }
        .nav-links button {
            background: none;
            border: none;
            color: var(--text-muted);
            font-size: 16px;
            font-family: var(--font-main);
            display: flex;
            align-items: center;
            width: 100%;
            text-align: left;
            cursor: pointer;
            padding: 10px;
            border-radius: 8px;
            transition: all 0.3s ease;
        }
        .nav-links button:hover, .nav-links button.active {
            color: var(--neon-blue);
            text-shadow: var(--glow-blue);
            background-color: rgba(0, 242, 255, 0.05);
        }

        /* --- Main Content Area --- */
        .main-content {
            margin-left: 260px;
            flex-grow: 1;
            padding: 40px;
            width: calc(100% - 260px);
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 40px;
        }

        /* --- PROFILE CARD --- */
        .profile-container {
            display: flex;
            justify-content: center;
            margin-bottom: 50px;
        }

        .profile-card {
            background-color: var(--bg-panel);
            border: var(--panel-border);
            border-radius: 15px;
            width: 400px;
            padding: 30px;
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        .avatar-wrapper {
            position: relative;
            width: 130px;
            height: 130px;
            margin-bottom: 20px;
        }

        .user-avatar {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            object-fit: cover;
            border: 4px solid var(--bg-panel);
            z-index: 2;
            position: relative;
            background-color: #151522;
        }

        .avatar-border {
            position: absolute;
            top: -4px; left: -4px; right: -4px; bottom: -4px;
            border-radius: 50%;
            background: linear-gradient(45deg, var(--neon-blue), var(--neon-purple), var(--neon-pink));
            z-index: 1;
            box-shadow: 0 0 20px rgba(0, 242, 255, 0.6), 0 0 40px rgba(157, 0, 255, 0.4);
            animation: spinGlow 3s linear infinite;
        }

        .status-badge {
            position: absolute;
            bottom: 6px;
            right: 6px;
            width: 24px;
            height: 24px;
            background-color: #23a55a;
            border-radius: 50%;
            border: 4px solid var(--bg-panel);
            z-index: 3;
        }

        .username {
            font-size: 24px;
            font-weight: bold;
            color: #ffffff;
            margin-bottom: 5px;
            font-family: var(--font-mono);
            cursor: pointer;
            user-select: none;
        }

        .user-role {
            color: var(--neon-pink);
            font-size: 12px;
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 25px;
            font-weight: bold;
        }

        /* --- Tab Panels --- */
        .tab-panel { display: none; }
        .tab-panel.active-panel {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 30px;
        }

        .info-card {
            background-color: var(--bg-panel);
            border: var(--panel-border);
            border-radius: 12px;
            padding: 25px;
        }

        .card-header {
            font-size: 13px;
            color: var(--text-muted);
            text-transform: uppercase;
            margin-bottom: 15px;
        }

        .card-value { font-size: 32px; font-weight: bold; }

        /* --- TRIPLE CLICK ADMIN PANEL MODAL --- */
        .admin-modal {
            display: none;
            position: fixed;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            background-color: #09090e;
            border: 2px solid var(--neon-pink);
            box-shadow: 0 0 30px rgba(255, 0, 127, 0.4);
            border-radius: 12px;
            padding: 25px;
            width: 360px;
            z-index: 100;
        }
        .admin-modal h3 { margin-bottom: 15px; color: var(--neon-pink); font-family: var(--font-mono); }
        .admin-field { margin-bottom: 15px; }
        .admin-field label { display: block; font-size: 11px; color: var(--text-muted); margin-bottom: 5px; text-transform: uppercase; }
        
        .admin-field input[type="text"], .admin-field input[type="file"] {
            width: 100%;
            background-color: #12121f;
            border: 1px solid #222235;
            padding: 8px 12px;
            color: #fff;
            border-radius: 6px;
        }

        .admin-buttons { display: flex; justify-content: flex-end; gap: 10px; margin-top: 20px; }
        .admin-btn { padding: 8px 16px; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; }
        .btn-save { background-color: var(--neon-blue); color: #000; }
        .btn-close { background-color: #222; color: #fff; }
        .modal-overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 99; }

        @keyframes spinGlow { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body>

    <audio id="bg-music" loop></audio>

    <nav class="sidebar">
        <div class="brand-logo">TRPSTAR.EXE</div>
        <ul class="nav-links">
            <li><button class="nav-btn active" onclick="switchTab('dashboard')">Dashboard</button></li>
            <li><button class="nav-btn" onclick="switchTab('network')">Network</button></li>
            <li><button class="nav-btn" onclick="switchTab('security')">Security</button></li>
            <li><button class="nav-btn" onclick="switchTab('settings')">Settings</button></li>
        </ul>
    </nav>

    <main class="main-content">
        <header>
            <h1 id="page-title">System Monitor</h1>
            <div style="color: var(--text-muted); font-size: 12px;">[Tip: Triple-click "sza.gurl" to configure]</div>
        </header>

        <section class="profile-container">
            <div class="profile-card">
                <div class="avatar-wrapper">
                    <img id="display-avatar" src="https://via.placeholder.com/150" alt="sza.gurl" class="user-avatar">
                    <div class="avatar-border"></div>
                    <div class="status-badge"></div>
                </div>
                
                <div class="username" id="secret-trigger">sza.gurl</div>
                <div class="user-role">Root Administrator</div>
            </div>
        </section>

        <div id="dashboard" class="tab-panel active-panel">
            <div class="info-card">
                <div class="card-header">Memory Allocation</div>
                <div class="card-value">16.4 GB</div>
            </div>
            <div class="info-card">
                <div class="card-header">Traffic Rate</div>
                <div class="card-value">412 Mb/s</div>
            </div>
            <div class="info-card">
                <div class="card-header">Threat Matrix</div>
                <div class="card-value" style="color: var(--neon-pink);">0 CLEAR</div>
            </div>
        </div>

        <div id="network" class="tab-panel">
            <div class="info-card">
                <div class="card-header">Server Node</div>
                <div class="card-value">Dagupan_Node</div>
            </div>
            <div class="info-card">
                <div class="card-header">Ping Latency</div>
                <div class="card-value" style="color: #23a55a;">12 ms</div>
            </div>
        </div>

        <div id="security" class="tab-panel">
            <div class="info-card">
                <div class="card-header">Firewall Status</div>
                <div class="card-value" style="color: #23a55a;">ACTIVE SHIELD</div>
            </div>
        </div>

        <div id="settings" class="tab-panel">
            <div class="info-card" style="grid-column: span 3;">
                <div class="card-header">Admin Logs</div>
                <div class="card-value" style="font-size: 20px;">Identity Verified: sza.gurl</div>
            </div>
        </div>
    </main>

    <div class="modal-overlay" id="overlay"></div>
    <div class="admin-modal" id="admin-panel">
        <h3>[CORE OVERRIDE PANEL]</h3>
        
        <div class="admin-field">
            <label>Upload Profile Picture (From PC)</label>
            <input type="file" id="upload-avatar-file" accept="image/*">
        </div>

        <div class="admin-field">
            <label>OR Use Image Web URL</label>
            <input type="text" id="input-avatar-url" placeholder="https://link-ng-pic.com/image.jpg">
        </div>

        <div class="admin-field">
            <label>Music Stream URL (Direct .mp3 link)</label>
            <input type="text" id="input-music-url" placeholder="https://server.com/song.mp3">
            <p style="font-size: 10px; color: var(--neon-pink); margin-top: 4px;">Dapat nagtatapos sa .mp3 ang link para tumugtog!</p>
        </div>

        <div class="admin-field">
            <label>Volume Control</label>
            <input type="range" id="input-volume" min="0" max="1" step="0.1" value="0.5" style="width: 100%;">
        </div>

        <div class="admin-buttons">
            <button class="admin-btn btn-close" onclick="closeAdmin()">Cancel</button>
            <button class="admin-btn btn-save" onclick="saveAdminChanges()">Apply & Play</button>
        </div>
    </div>

    <script>
        function switchTab(tabId) {
            document.querySelectorAll('.tab-panel').forEach(panel => panel.classList.remove('active-panel'));
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(tabId).classList.add('active-panel');
            event.currentTarget.classList.add('active');
            
            const pageTitle = document.getElementById('page-title');
            if(tabId === 'dashboard') pageTitle.innerText = "System Monitor";
            if(tabId === 'network') pageTitle.innerText = "Network Status";
            if(tabId === 'security') pageTitle.innerText = "Security Override";
            if(tabId === 'settings') pageTitle.innerText = "System Control Panel";
        }

        const trigger = document.getElementById('secret-trigger');
        trigger.addEventListener('click', function (e) {
            if (e.detail === 3) {
                document.getElementById('admin-panel').style.display = 'block';
                document.getElementById('overlay').style.display = 'block';
            }
        });

        function closeAdmin() {
            document.getElementById('admin-panel').style.display = 'none';
            document.getElementById('overlay').style.display = 'none';
        }

        function saveAdminChanges() {
            const fileInput = document.getElementById('upload-avatar-file');
            const urlInput = document.getElementById('input-avatar-url').value;
            const musicUrl = document.getElementById('input-music-url').value;
            const volumeVal = document.getElementById('input-volume').value;
            const audioPlayer = document.getElementById('bg-music');

            if (fileInput.files && fileInput.files[0]) {
                const reader = new FileReader();
                reader.onload = function (e) {
                    document.getElementById('display-avatar').src = e.target.result;
                    localStorage.setItem('savedAvatar', e.target.result);
                }
                reader.readAsDataURL(fileInput.files[0]);
            } 
            else if (urlInput.trim() !== "") {
                document.getElementById('display-avatar').src = urlInput;
                localStorage.setItem('savedAvatar', urlInput);
            }

            if (musicUrl.trim() !== "") {
                audioPlayer.src = musicUrl;
                audioPlayer.volume = volumeVal;
                audioPlayer.play()
                    .then(() => console.log("Music started successfully!"))
                    .catch(err => alert("Audio Error: Siguraduhing DIRECT .mp3 link ang nilagay mo pre."));
                
                localStorage.setItem('savedMusic', musicUrl);
                localStorage.setItem('savedVolume', volumeVal);
            } else {
                audioPlayer.volume = volumeVal;
                localStorage.setItem('savedVolume', volumeVal);
            }

            closeAdmin();
        }

        window.addEventListener('DOMContentLoaded', () => {
            const cachedAvatar = localStorage.getItem('savedAvatar');
            const cachedMusic = localStorage.getItem('savedMusic');
            const cachedVolume = localStorage.getItem('savedVolume') || 0.5;

            if (cachedAvatar) {
                document.getElementById('display-avatar').src = cachedAvatar;
            }
            if (cachedMusic) {
                const audioPlayer = document.getElementById('bg-music');
                audioPlayer.src = cachedMusic;
                audioPlayer.volume = cachedVolume;
                document.getElementById('input-music-url').value = cachedMusic;
                document.getElementById('input-volume').value = cachedVolume;

                document.body.addEventListener('click', () => {
                    if (audioPlayer.paused && audioPlayer.src) {
                        audioPlayer.play().catch(e => console.log("Autoplay block bypassed via click"));
                    }
                }, { once: true });
            }
        });
    </script>
</body>
</html>
