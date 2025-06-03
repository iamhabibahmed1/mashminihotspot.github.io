<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MashMini Hotspot Zone - Server Monitoring</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Tiro+Bangla&family=Poppins:wght@600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #4361ee;
            --secondary-color: #3f37c9;
            --success-color: #4cc9f0;
            --danger-color: #f72585;
            --warning-color: #f8961e;
            --info-color: #4895ef;
            --dark-color: #212529;
            --light-color: #f8f9fa;
            --card-bg: #ffffff;
            --body-bg: #f5f7fa; /* বডির রঙ আগের অবস্থায় ফিরিয়ে আনা হলো */
            --text-color: #2b2d42;
            --text-muted: #6c757d;
            --hotspot-header-bg: #4361ee;
            --secondary-header-bg: #43ee57; /* দ্বিতীয় হেডার এর রঙ অপরিবর্তিত আছে */
        }

        /* General Body Styles */
        body {
            background-color: var(--body-bg);
            font-family: 'Tiro Bangla', serif;
            color: var(--text-color);
            padding: 20px;
        }

        .dashboard-container {
            max-width: 1400px;
            margin: 0 auto;
        }

        /* MashMini Hotspot Zone Header Styles (প্রথম হেডার) */
        .hotspot-header {
            background-color: var(--hotspot-header-bg);
            color: white;
            padding: 25px 0;
            margin-bottom: 20px;
            border-radius: 12px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
            text-align: center;
            position: relative;
            overflow: hidden;
            z-index: 1;
        }

        .hotspot-title {
            font-family: 'Poppins', sans-serif;
            font-weight: 700;
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
            letter-spacing: 1px;
        }

        .hotspot-subtitle {
            font-family: 'Tiro Bangla', serif;
            font-size: 1.1rem;
            opacity: 0.9;
            margin-bottom: 0;
            letter-spacing: 0.5px;
        }

        .hotspot-icon {
            font-size: 2rem;
            margin-right: 15px;
            vertical-align: middle;
        }

        /* New Secondary Dashboard Header Styles (দ্বিতীয় হেডার) */
        .secondary-dashboard-header {
            background-color: var(--secondary-header-bg);
            color: white;
            padding: 15px 0;
            margin-bottom: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            text-align: center;
        }

        .secondary-dashboard-header h1 {
            font-weight: 700;
            font-size: 2rem;
            margin-bottom: 5px;
        }

        .secondary-dashboard-header p {
            font-size: 0.95rem;
            opacity: 0.9;
            margin-bottom: 0;
        }


        /* Dashboard Specific Styles */
        .card {
            border: none;
            border-radius: 12px;
            overflow: hidden;
            margin-bottom: 20px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
            background-color: var(--card-bg);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.12);
        }

        .card-header {
            font-weight: 600;
            padding: 15px 20px;
            font-size: 1rem;
            background-color: var(--primary-color);
            color: white;
            border-bottom: none;
        }

        .status-badge {
            min-width: 80px;
            display: inline-block;
            text-align: center;
            font-size: 0.85rem;
            padding: 6px 12px;
            border-radius: 20px;
            font-weight: 500;
            letter-spacing: 0.5px;
        }

        .status-up {
            background-color: rgba(40, 167, 69, 0.15);
            color: #28a745;
            border: 1px solid #28a745;
        }

        .status-down {
            background-color: rgba(220, 53, 69, 0.15);
            color: #dc3545;
            border: 1px solid #dc3545;
        }

        .status-checking {
            background-color: rgba(255, 193, 7, 0.15);
            color: #d39e00;
            border: 1px solid #ffc107;
        }

        #serverTable {
            font-size: 0.92rem;
        }

        #serverTable th {
            padding: 12px 15px;
            background-color: #f8f9fa;
            font-weight: 600;
            text-transform: uppercase;
            font-size: 0.8rem;
            letter-spacing: 0.5px;
            color: var(--text-muted);
            border-bottom: 2px solid #e9ecef;
        }

        #serverTable td {
            padding: 12px 15px;
            vertical-align: middle;
            border-bottom: 1px solid #e9ecef;
        }

        #serverTable tr:last-child td {
            border-bottom: none;
        }

        #serverTable tr:hover td {
            background-color: rgba(67, 97, 238, 0.05);
        }

        .login-btn {
            padding: 6px 12px;
            font-size: 0.85rem;
            border-radius: 6px;
            font-weight: 500;
            transition: all 0.3s ease;
        }

        .login-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .refresh-btn {
            padding: 10px 20px;
            font-size: 0.95rem;
            border-radius: 8px;
            font-weight: 600;
            letter-spacing: 0.5px;
            transition: all 0.3s ease;
            border: none;
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            color: white;
        }

        .refresh-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(67, 97, 238, 0.25);
        }

        .compact-summary {
            position: sticky;
            top: 20px;
            z-index: 100;
        }

        .server-table-container {
            max-height: 70vh;
            overflow-y: auto;
            border-radius: 12px;
        }

        .server-table-container::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }

        .server-table-container::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }

        .server-table-container::-webkit-scrollbar-thumb {
            background: #c1c1c1;
            border-radius: 10px;
        }

        .server-table-container::-webkit-scrollbar-thumb:hover {
            background: #a8a8a8;
        }

        .summary-stats {
            font-size: 0.95rem;
        }

        .summary-stats h4 {
            font-size: 1.5rem;
            margin-bottom: 5px;
            font-weight: 700;
        }

        .summary-stats small {
            font-size: 0.8rem;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        #statusChart {
            max-height: 180px;
            margin: 0 auto;
        }

        .history-badge {
            display: inline-block;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            margin-right: 3px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .history-up { background-color: #28a745; }
        .history-down { background-color: #dc3545; }
        .history-checking { background-color: #ffc107; }
        .history-unknown { background-color: #adb5bd; }

        .history-container {
            margin-top: 5px;
            display: flex;
            flex-wrap: wrap;
            gap: 3px;
        }

        .router-number {
            font-weight: 600;
            color: var(--primary-color);
            white-space: nowrap;
        }

        .serial-number {
            font-weight: 700;
            color: var(--text-color);
            text-align: center;
            width: 50px;
        }

        .location-name {
            min-width: 180px;
            font-weight: 500;
        }

        .last-update {
            background-color: rgba(255, 255, 255, 0.2);
            padding: 3px 10px;
            border-radius: 20px;
            font-weight: 500;
        }

        /* stat-card স্টাইল বাদ দেওয়া হলো */
        /* .stat-card { ... } */

        /* স্ট্যাটাস সামারি কার্ডের ভেতরে নতুন স্টাইল যোগ করা হয়েছে */
        .summary-details {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
            padding-top: 15px;
            border-top: 1px solid #eee;
        }

        .summary-item {
            text-align: center;
        }

        .summary-item h4 {
            font-size: 1.5rem; /* আগের h3 এর সাইজ */
            font-weight: 700;
            margin-bottom: 5px;
        }

        .summary-item p {
            font-size: 0.85rem; /* আগের p এর সাইজ */
            color: var(--text-muted);
            margin-bottom: 0;
        }

        .summary-item.up h4 { color: #28a745; }
        .summary-item.down h4 { color: #dc3545; }
        .summary-item.checking h4 { color: #ffc107; }


        .footer {
            text-align: center;
            margin-top: 30px;
            padding: 20px 0;
            color: var(--text-muted);
            font-size: 0.85rem;
        }

        @media (max-width: 768px) {
            .hotspot-title {
                font-size: 1.8rem;
            }
            .hotspot-subtitle {
                font-size: 1rem;
            }
            .secondary-dashboard-header h1 {
                font-size: 1.5rem;
            }

            #serverTable th, #serverTable td {
                padding: 8px 10px;
                font-size: 0.85rem;
            }

            .location-name {
                min-width: 120px;
            }

            .summary-details {
                flex-direction: column; /* ছোট স্ক্রিনে উল্লম্বভাবে দেখাবে */
                align-items: center;
            }
            .summary-item {
                margin-bottom: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="dashboard-container">
        <header class="hotspot-header">
            <h1 class="hotspot-title">
                <i class="fas fa-wifi hotspot-icon"></i>MashMini Hotspot Zone
            </h1>
            <p class="hotspot-subtitle">MMH</p>
        </header>

        <header class="secondary-dashboard-header">
            <h1><i class="fas fa-server me-2"></i>সার্ভার মনিটরিং ড্যাশবোর্ড</h1>
            <p>Last Update: <span class="last-update" id="lastUpdate"></span></p>
        </header>

        <div class="row">
            <div class="col-lg-3">
                <div class="card shadow compact-summary">
                    <div class="card-header">
                        <h5><i class="fas fa-chart-pie me-1"></i>স্ট্যাটাস সামারি</h5>
                    </div>
                    <div class="card-body p-3 text-center">
                        <canvas id="statusChart" height="200"></canvas>
                        <div class="summary-details">
                            <div class="summary-item up">
                                <h4 id="upCount">0</h4>
                                <p>চালু</p>
                            </div>
                            <div class="summary-item down">
                                <h4 id="downCount">0</h4>
                                <p>বন্ধ</p>
                            </div>
                            <div class="summary-item checking">
                                <h4 id="checkingCount">0</h4>
                                <p>চেক চলছে</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="col-lg-9">
                <div class="card shadow">
                    <div class="card-header d-flex justify-content-between align-items-center">
                        <h5 class="mb-0"><i class="fas fa-list me-1"></i>সার্ভারের তালিকা</h5>
                        <div class="input-group" style="width: 250px;">
                            <input type="text" class="form-control form-control-sm" placeholder="সার্চ করুন..." id="searchInput">
                            <button class="btn btn-sm btn-outline-light" type="button"><i class="fas fa-search"></i></button>
                        </div>
                    </div>
                    <div class="card-body p-0">
                        <div class="table-responsive server-table-container">
                            <table class="table table-hover mb-0" id="serverTable">
                                <thead>
                                    <tr>
                                        <th class="serial-number">#</th>
                                        <th class="location-name">লোকেশন</th>
                                        <th>রাউটার</th>
                                        <th>স্ট্যাটাস</th>
                                        <th>শেষ চেক</th>
                                        <th>অ্যাকশন</th>
                                    </tr>
                                </thead>
                                <tbody id="serverListBody">
                                    </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="text-center mt-4">
            <button class="btn refresh-btn" id="refreshButton">
                <i class="fas fa-sync-alt me-2"></i>এখনই আপডেট করুন
            </button>
        </div>

        <div class="footer">
            <p>© 2023-2025 MashMini Hotspot Zone | সার্ভার মনিটরিং সিস্টেম | প্রতি ১০ সেকেন্ডে স্বয়ংক্রিয় আপডেট ! Admin -HABIB AHMED- </p>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // লোকেশন নামের লিস্ট
        const locationNames = [
            "মালোপাড়া পলাশে 2",
            "XNot ActiveX",
            "গাজীরহাট স্কুল",
            "মদনগাতী স্কুল",
            "মালোপাড়া পলাশের বাড়ি",
            "মালোপাড়া তাপশ কাকা",
            "XNot ActiveX",
            "কালার শপ",
            "কালার Home",
            "সিলিমপুর স্কুল শেষ",
            "পার মচন্দপুর বাড়ির ভিতর",
            "নজরুল গেট",
            "মান্না",
            "রফিক",
            "ফুরকান",
            "রাহাদ গাজীরহাট",
            "মচন্দপুর মন্দির",
            "আমবাড়িয়া",
            "তমাল Shop",
            "শিপন ভাই বোর্ডঘর",
            "ডুমরা",
            "আবালগাতী",
            "তমাল বাড়ীর ভিতর",
            "XNot ActiveX",
            "শিপন ভাই",
            "Ruijie"
        ];

        // রাউটার নাম্বারের লিস্ট (আপনার দেওয়া তালিকা)
        const routerNumbers = [
            "Haat 04",
            "Haat 07",
            "Haat 09",
            "Haat 10",
            "Haat 16",
            "Haat 23",
            "Haat 25",
            "Haat 31",
            "Haat 32",
            "Haat 33",
            "Haat 34",
            "Haat 37",
            "Haat 38",
            "Haat 39",
            "Haat 40",
            "Haat 47",
            "Haat 50",
            "Haat 63",
            "Haat 65",
            "Haat 67",
            "Haat 68",
            "Haat 69",
            "Haat 75",
            "Haat 76",
            "Ruijie"
        ];

        // সার্ভার ডেটা প্রস্তুত করা
        const servers = locationNames.map((location, index) => {
            const port = 6001 + index;
            return {
                port: port,
                name: location,
                url: `http://103.177.124.159:${port}/`,
                status: "unknown",
                lastChecked: "চেক করা হয়নি",
                elementId: `server-row-${port}`,
                history: [],
                routerNumber: routerNumbers[index] || "",
                serialNumber: index + 1
            };
        });

        const checkInterval = 10000; // 10 সেকেন্ড
        let statusChart = null;
        let monitoringInterval;

        // DOM লোড হওয়ার পর
        document.addEventListener('DOMContentLoaded', function() {
            initializeDashboard();
            startMonitoring();

            // রিফ্রেশ বাটন ইভেন্ট
            document.getElementById('refreshButton').addEventListener('click', function() {
                this.innerHTML = '<i class="fas fa-spinner fa-spin me-2"></i>আপডেট হচ্ছে...';
                clearInterval(monitoringInterval);
                checkAllServers();
                monitoringInterval = setInterval(checkAllServers, checkInterval);

                setTimeout(() => {
                    this.innerHTML = '<i class="fas fa-sync-alt me-2"></i>এখনই আপডেট করুন';
                }, 2000);
            });

            // সার্চ ফাংশনালিটি
            document.getElementById('searchInput').addEventListener('input', function() {
                const searchTerm = this.value.toLowerCase();
                const rows = document.querySelectorAll('#serverTable tbody tr');

                rows.forEach(row => {
                    const location = row.querySelector('.location-name').textContent.toLowerCase();
                    const router = row.querySelector('.router-number').textContent.toLowerCase();

                    if (location.includes(searchTerm) || router.includes(searchTerm)) {
                        row.style.display = '';
                    } else {
                        row.style.display = 'none';
                    }
                });
            });
        });

        // ড্যাশবোর্ড ইনিশিয়ালাইজেশন
        function initializeDashboard() {
            renderServerTable();
            initializeChart();
            updateLastCheckedTime();
            updateCountCards();
        }

        // মনিটরিং শুরু করবে
        function startMonitoring() {
            checkAllServers();
            monitoringInterval = setInterval(checkAllServers, checkInterval);
        }

        // সব সার্ভার চেক করবে
        function checkAllServers() {
            servers.forEach(server => {
                checkServerStatus(server);
            });
        }

        // একটি সার্ভারের স্ট্যাটাস চেক করবে
        function updateServerStatus(server, status) {
            const checkTime = new Date().toLocaleTimeString();

            // হিস্ট্রি আপডেট করুন
            server.history.unshift({
                status: status,
                time: checkTime
            });

            // সর্বাধিক ১০টি এন্ট্রি রাখুন
            server.history = server.history.slice(0, 10);

            server.status = status;
            server.lastChecked = checkTime;

            updateServerStatusUI(server);
            updateCountCards();
            updateChart();
            updateLastCheckedTime();
        }

        // সার্ভার স্ট্যাটাস চেক করবে
        function checkServerStatus(server) {
            server.status = "checking";
            server.lastChecked = new Date().toLocaleTimeString();
            updateServerStatusUI(server);
            updateCountCards();

            const controller = new AbortController();
            const timeoutId = setTimeout(() => controller.abort(), 5000); // 5 সেকেন্ডের টাইমআউট

            fetch(server.url, {
                method: 'HEAD',
                mode: 'no-cors',
                cache: 'no-store',
                signal: controller.signal
            })
            .then(() => {
                clearTimeout(timeoutId);
                updateServerStatus(server, "up");
            })
            .catch(error => {
                clearTimeout(timeoutId);
                console.error(`Error checking ${server.name} (${server.url}):`, error);
                updateServerStatus(server, "down");
            });
        }


        // সার্ভার টেবিল রেন্ডার করবে
        function renderServerTable() {
            const tbody = document.getElementById('serverListBody');
            tbody.innerHTML = '';

            servers.forEach(server => {
                const row = document.createElement('tr');
                row.id = server.elementId;

                row.innerHTML = `
                    <td class="serial-number">${server.serialNumber}</td>
                    <td class="location-name">${server.name}</td>
                    <td class="router-number">${server.routerNumber}</td>
                    <td class="status-cell">
                        <span class="badge rounded-pill status-badge ${getStatusClass(server.status)}">
                            <i class="fas ${getStatusIcon(server.status)} me-1"></i>
                            ${getStatusText(server.status)}
                        </span>
                        <div class="history-container">
                            ${renderHistoryBadges(server.history)}
                        </div>
                    </td>
                    <td class="last-checked">${server.lastChecked}</td>
                    <td>
                        <button class="btn btn-sm btn-outline-primary login-btn" onclick="window.open('${server.url}', '_blank')">
                            <i class="fas fa-sign-in-alt me-1"></i> লগইন
                        </button>
                    </td>
                `;

                tbody.appendChild(row);
            });
        }

        // হিস্ট্রি ব্যাজ রেন্ডার করবে
        function renderHistoryBadges(history) {
            let badges = '';
            for (let i = 0; i < 10; i++) {
                const entry = history[i] || { status: 'unknown' };
                badges += `<span class="history-badge ${getHistoryClass(entry.status)}" title="${entry.time || 'N/A'}"></span>`;
            }
            return badges;
        }

        // চার্ট ইনিশিয়ালাইজেশন
        function initializeChart() {
            const ctx = document.getElementById('statusChart').getContext('2d');
            statusChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['চালু', 'বন্ধ', 'চেকিং'],
                    datasets: [{
                        data: [0, 0, servers.length],
                        backgroundColor: ['#28a745', '#dc3545', '#ffc107'],
                        borderWidth: 0,
                        hoverOffset: 10
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '70%',
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                font: {
                                    family: 'Tiro Bangla',
                                    size: 12
                                },
                                padding: 20,
                                usePointStyle: true,
                                pointStyle: 'circle'
                            }
                        },
                        tooltip: {
                            bodyFont: {
                                family: 'Tiro Bangla'
                            },
                            titleFont: {
                                family: 'Tiro Bangla'
                            }
                        }
                    }
                }
            });
        }

        // চার্ট আপডেট করবে
        function updateChart() {
            if (statusChart) {
                const upCount = servers.filter(s => s.status === "up").length;
                const downCount = servers.filter(s => s.status === "down").length;
                const checkingCount = servers.filter(s => s.status === "checking").length;

                statusChart.data.datasets[0].data = [upCount, downCount, checkingCount];
                statusChart.update();
            }
        }

        // কাউন্ট কার্ড আপডেট করবে
        function updateCountCards() {
            const upCount = servers.filter(s => s.status === "up").length;
            const downCount = servers.filter(s => s.status === "down").length;
            const checkingCount = servers.filter(s => s.status === "checking").length;

            document.getElementById('upCount').textContent = upCount;
            document.getElementById('downCount').textContent = downCount;
            document.getElementById('checkingCount').textContent = checkingCount;
        }

        // স্ট্যাটাস UI আপডেট করবে
        function updateServerStatusUI(server) {
            const row = document.getElementById(server.elementId);
            if (row) {
                const statusCell = row.querySelector('.status-cell');
                const lastCheckedCell = row.querySelector('.last-checked');

                if (statusCell) {
                    statusCell.innerHTML = `
                        <span class="badge rounded-pill status-badge ${getStatusClass(server.status)}">
                            <i class="fas ${getStatusIcon(server.status)} me-1"></i>
                            ${getStatusText(server.status)}
                        </span>
                        <div class="history-container">
                            ${renderHistoryBadges(server.history)}
                        </div>
                    `;
                }

                if (lastCheckedCell) {
                    lastCheckedCell.textContent = server.lastChecked;
                }
            }
        }

        // শেষ চেকের সময় আপডেট করবে
        function updateLastCheckedTime() {
            const now = new Date();
            const timeString = now.toLocaleTimeString('bn-BD'); // বাংলা ফরম্যাটে সময়
            const dateString = now.toLocaleDateString('bn-BD'); // বাংলা ফরম্যাটে তারিখ
            document.getElementById('lastUpdate').textContent = `${timeString}, ${dateString}`;
        }

        // স্ট্যাটাস অনুযায়ী ক্লাস রিটার্ন করবে
        function getStatusClass(status) {
            return {
                'up': 'status-up',
                'down': 'status-down',
                'checking': 'status-checking'
            }[status] || 'bg-secondary';
        }

        // হিস্ট্রি ব্যাজের জন্য ক্লাস রিটার্ন করবে
        function getHistoryClass(status) {
            return {
                'up': 'history-up',
                'down': 'history-down',
                'checking': 'history-checking'
            }[status] || 'history-unknown';
        }

        // স্ট্যাটাস অনুযায়ী আইকন রিটার্ন করবে
        function getStatusIcon(status) {
            return {
                'up': 'fa-check-circle',
                'down': 'fa-times-circle',
                'checking': 'fa-sync-alt fa-spin'
            }[status] || 'fa-question-circle';
        }

        // স্ট্যাটাস অনুযায়ী টেক্সট রিটার্ন করবে
        function getStatusText(status) {
            return {
                'up': 'চালু',
                'down': 'বন্ধ',
                'checking': 'চেকিং...'
            }[status] || 'অজানা';
        }
    </script>
</body>
</html>
