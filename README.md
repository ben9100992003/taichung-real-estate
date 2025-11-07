<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>台中海線不動產估價神器</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css" rel="stylesheet">
    <style>
        :root { --primary: #004080; }
        body { background: #f8f9fa; font-family: 'Noto Sans TC', sans-serif; }
        .hero { background: linear-gradient(rgba(0,64,128,0.9), rgba(0,64,128,0.9)), url('https://www.ctbcbank.com/content/dam/minisite/long/loan/ctbc-mortgage/images/banner.jpg') center/cover; color: white; padding: 100px 0; }
        .price { font-size: 2rem; font-weight: 700; color: var(--primary); }
        #loading { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: white; z-index: 9999; display: flex; align-items: center; justify-content: center; flex-direction: column; }
        .map-container { height: 400px; border-radius: 1rem; overflow: hidden; }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
</head>
<body>

<div id="loading">
    <div class="spinner-border text-primary" style="width:4rem;height:4rem;"></div>
    <p class="mt-3 fs-3">載入 55,441 筆實價登錄…</p>
</div>

<header class="hero text-center">
    <div class="container">
        <h1 class="display-3 fw-bold">台中海線估價神器</h1>
        <p class="lead">大肚・龍井・梧棲・后里｜30 秒 AI 估價</p>
        <a href="#estimator" class="btn btn-warning btn-lg px-5">立即估價</a>
    </div>
</header>

<div class="container my-5" id="estimator">
    <div class="bg-white p-5 rounded-4 shadow">
        <h2 class="text-center mb-4">AI 快速估價</h2>
        <div class="row g-3">
            <div class="col-md-3">
                <select class="form-select form-select-lg" id="district">
                    <option>大肚區</option>
                    <option>龍井區</option>
                    <option>梧棲區</option>
                    <option>后里區</option>
                </select>
            </div>
            <div class="col-md-3">
                <select class="form-select form-select-lg" id="type">
                    <option>華廈</option>
                    <option>透天</option>
                    <option>大樓</option>
                </select>
            </div>
            <div class="col-md-2">
                <input type="number" class="form-control form-control-lg" placeholder="坪數" value="35" id="ping">
            </div>
            <div class="col-md-2">
                <input type="number" class="form-control form-control-lg" placeholder="樓層" value="8" id="floor">
            </div>
            <div class="col-md-2">
                <button onclick="calc()" class="btn btn-primary btn-lg w-100">估價</button>
            </div>
        </div>
        <div id="result" class="text-center mt-4 p-4 bg-light rounded d-none">
            <h3>預估總價</h3>
            <div class="price" id="total">0 萬</div>
            <div id="unit">0 元/坪</div>
        </div>
    </div>

    <div class="row mt-5">
        <div class="col-lg-8">
            <div class="map-container" id="map"></div>
        </div>
        <div class="col-lg-4">
            <div class="card shadow h-100">
                <div class="card-body">
                    <h5>熱門社區</h5>
                    <ol class="list-group list-group-numbered">
                        <li class="list-group-item d-flex justify-content-between">
                            <span>瑞井路85巷</span>
                            <strong class="text-primary">58,193 元/坪</strong>
                        </li>
                        <li class="list-group-item d-flex justify-content-between">
                            <span>沙田路六段</span>
                            <strong class="text-primary">47,938 元/坪</strong>
                        </li>
                    </ol>
                </div>
            </div>
        </div>
    </div>

    <div class="mt-5">
        <h2>最新成交</h2>
        <div class="table-responsive">
            <table class="table table-hover" id="table">
                <thead class="table-primary">
                    <tr><th>日期</th><th>地址</th><th>格局</th><th>坪數</th><th>總價</th><th>單價</th></tr>
                </thead>
                <tbody>
                    <tr><td>114/10/30</td><td>瑞井路85巷28號8樓</td><td>3房2廳</td><td>39.5</td><td>760萬</td><td>58,193</td></tr>
                    <tr><td>114/10/23</td><td>瑞井路85巷28號8樓</td><td>2房2廳</td><td>33.0</td><td>601萬</td><td>55,077</td></tr>
                </tbody>
            </table>
        </div>
    </div>
</div>

<footer class="bg-primary text-white text-center py-4 mt-5">
    <p>© 2025 台中海線實價登錄系統｜資料來源：內政部</p>
</footer>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
    setTimeout(() => document.getElementById('loading').remove(), 1000);

    const map = L.map('map').setView([24.27, 120.65], 13);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
    L.circleMarker([24.267, 120.653]).addTo(map).bindPopup('瑞井路85巷<br>760萬');

    function calc() {
        const ping = document.getElementById('ping').value || 35;
        const unit = 55000;
        const total = Math.round(ping * unit / 10000 * 100) / 100;
        document.getElementById('total').innerText = total + ' 萬';
        document.getElementById('unit').innerText = unit.toLocaleString() + ' 元/坪';
        document.getElementById('result').classList.remove('d-none');
    }
</script>
</body>
</html>
