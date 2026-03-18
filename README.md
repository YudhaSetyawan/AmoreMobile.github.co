<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>BPR BKK JATENG - Mobile Reporting</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700&display=swap" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css">
    
    <style>
        :root { 
            --bkk-blue: #003366; 
            --bkk-blue-light: #0056b3;
            --bkk-gold: #FFD700; 
            --bkk-green: #198754;
            --bg-light: #F4F7FA; 
        }
        
        body { 
            background-color: var(--bg-light); 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            padding-bottom: 80px; 
            color: #1E293B;
        }

        /* Header Style */
        .header-app { 
            background: linear-gradient(135deg, var(--bkk-blue) 0%, #001a33 100%);
            color: white; padding: 25px 20px; 
            border-bottom: 4px solid var(--bkk-gold);
            border-bottom-left-radius: 25px;
            border-bottom-right-radius: 25px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        /* Dashboard Stats */
        .stats-container {
            margin-top: -30px;
            padding: 0 20px;
        }
        .stats-card {
            background: white; border-radius: 20px; padding: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.05);
            display: flex; justify-content: space-around; text-align: center;
        }
        .stat-item h4 { margin: 0; font-weight: 700; color: var(--bkk-blue); }
        .stat-item span { font-size: 0.7rem; color: #64748B; font-weight: 600; text-uppercase: uppercase; }

        /* Menu Grid */
        .menu-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 20px; }
        .menu-item {
            background: white; border-radius: 22px; padding: 25px 15px;
            display: flex; flex-direction: column; align-items: center;
            text-decoration: none; color: inherit; border: 1px solid #E2E8F0;
            transition: all 0.2s ease; cursor: pointer;
        }
        .menu-item:active { transform: scale(0.95); background: #F8FAFC; }
        .menu-item i { font-size: 2.5rem; margin-bottom: 10px; }
        
        /* Form Styling */
        .form-section { display: none; padding: 20px; animation: slideUp 0.4s ease; }
        .form-section.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        
        .card-form { 
            background: white; border-radius: 25px; padding: 25px; 
            box-shadow: 0 4px 20px rgba(0,0,0,0.03); 
        }
        .form-label { font-weight: 700; font-size: 0.85rem; color: #334155; margin-bottom: 8px; }
        .form-control, .form-select { 
            border-radius: 12px; padding: 12px; border: 1px solid #CBD5E1; background-color: #F8FAFC;
        }
        .form-control:focus { border-color: var(--bkk-blue); box-shadow: none; background-color: white; }

        /* Bottom Nav */
        .bottom-nav { 
            position: fixed; bottom: 0; left: 0; right: 0; background: white; 
            display: flex; box-shadow: 0 -5px 20px rgba(0,0,0,0.05); z-index: 1000;
            padding: 12px 0; border-top-left-radius: 20px; border-top-right-radius: 20px;
        }
        .nav-link { flex: 1; text-align: center; color: #94A3B8; text-decoration: none; font-size: 0.75rem; font-weight: 600; }
        .nav-link i { font-size: 1.5rem; display: block; }
        .nav-link.active { color: var(--bkk-blue); }

        .btn-submit { padding: 15px; border-radius: 15px; font-weight: 700; letter-spacing: 1px; }
        .preview-img-box { 
            width: 100%; height: 150px; background: #EDF2F7; border-radius: 12px; 
            display: flex; align-items: center; justify-content: center; overflow: hidden;
            border: 2px dashed #CBD5E1;
        }
        .preview-img-box img { width: 100%; height: 100%; object-fit: cover; }
    </style>
</head>
<body>

<div class="header-app d-flex justify-content-between align-items-start">
    <div>
        <h4 class="fw-bold mb-0">BPR BKK JATENG</h4>
        <small class="opacity-75">Sistem Laporan Remedial & Marketing</small>
    </div>
    <div class="bg-white rounded-circle p-2 shadow-sm">
        <img src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png" width="30">
    </div>
</div>

<div id="pageHome" class="form-section active">
    <div class="stats-container">
        <div class="stats-card">
            <div class="stat-item">
                <span id="dateNow">10 Mar 2026</span>
                <h4 id="dashSetoran">Rp 0</h4>
                <span>Total Koleksi</span>
            </div>
            <div class="stat-item">
                <br>
                <h4 id="dashMkt">0</h4>
                <span>Prospek Baru</span>
            </div>
        </div>
    </div>

    <div class="menu-grid">
        <div class="menu-item" onclick="showPage('pageColl')">
            <i class="bi bi-wallet2 text-primary"></i>
            <span class="fw-bold">COLLECTION</span>
        </div>
        <div class="menu-item" onclick="showPage('pageMkt')">
            <i class="bi bi-piggy-bank text-success"></i>
            <span class="fw-bold">MARKETING</span>
        </div>
        <div class="menu-item" onclick="showPage('pageHistory')">
            <i class="bi bi-clock-history text-warning"></i>
            <span class="fw-bold">RIWAYAT</span>
        </div>
        <div class="menu-item" onclick="location.reload()">
            <i class="bi bi-arrow-repeat text-secondary"></i>
            <span class="fw-bold">REFRESH</span>
        </div>
    </div>
</div>

<div id="pageColl" class="form-section">
    <div class="container">
        <div class="d-flex align-items-center mb-3" onclick="showPage('pageHome')">
            <i class="bi bi-chevron-left fs-4 me-2"></i>
            <h5 class="fw-bold m-0">Laporan Penagihan</h5>
        </div>
        <div class="card-form">
            <form id="formCollection">
                <div class="mb-3">
                    <label class="form-label">Nama Petugas</label>
                    <select class="form-select select-petugas" id="p_coll" required></select>
                </div>
                <div class="mb-3">
                    <label class="form-label">Nama Nasabah</label>
                    <input type="text" class="form-control" id="n_coll" placeholder="Nama lengkap nasabah" required>
                </div>
                <div class="row g-2 mb-3">
                    <div class="col-6">
                        <label class="form-label">Status</label>
                        <select class="form-select" id="s_coll">
                            <option value="Titip Angsuran">Titip Angsuran</option>
                            <option value="Janji Bayar">Janji Bayar</option>
                            <option value="Rumah Kosong">Rumah Kosong</option>
                        </select>
                    </div>
                    <div class="col-6">
                        <label class="form-label">Nominal (Rp)</label>
                        <input type="text" class="form-control fw-bold" id="v_coll" placeholder="0" onkeyup="formatRupiah(this)">
                    </div>
                </div>
                <div class="mb-4">
                    <label class="form-label">Bukti Foto Kunjungan</label>
                    <div class="preview-img-box mb-2" id="box_coll">
                        <i class="bi bi-camera fs-1 text-muted"></i>
                    </div>
                    <input type="file" class="form-control" id="f_coll" accept="image/*" capture="camera" required onchange="handleFile(this, 'box_coll', 'photoColl')">
                </div>
                <button type="submit" class="btn btn-primary w-100 btn-submit shadow">KIRIM LAPORAN KOLEKSI</button>
            </form>
        </div>
    </div>
</div>

<div id="pageMkt" class="form-section">
    <div class="container">
        <div class="d-flex align-items-center mb-3" onclick="showPage('pageHome')">
            <i class="bi bi-chevron-left fs-4 me-2"></i>
            <h5 class="fw-bold m-0">Input Prospek Tabungan</h5>
        </div>
        <div class="card-form">
            <form id="formMarketing">
                <div class="mb-3">
                    <label class="form-label">Nama Petugas Marketing</label>
                    <select class="form-select select-petugas" id="p_mkt" required></select>
                </div>
                <div class="mb-3">
                    <label class="form-label">Calon Nasabah</label>
                    <input type="text" class="form-control" id="n_mkt" placeholder="Nama calon nasabah" required>
                </div>
                <div class="row g-2 mb-3">
                    <div class="col-6">
                        <label class="form-label">No. WA</label>
                        <input type="tel" class="form-control" id="wa_mkt" placeholder="08xxx" required>
                    </div>
                    <div class="col-6">
                        <label class="form-label">Produk</label>
                        <select class="form-select" id="prod_mkt">
                            <option value="Tabungan Utama">Tabungan Utama</option>
                            <option value="Simpel">Simpel</option>
                            <option value="Deposito">Deposito</option>
                        </select>
                    </div>
                </div>
                <div class="mb-4">
                    <label class="form-label">Foto Lokasi / Nasabah</label>
                    <div class="preview-img-box mb-2" id="box_mkt">
                        <i class="bi bi-camera-fill fs-1 text-muted"></i>
                    </div>
                    <input type="file" class="form-control" id="f_mkt" accept="image/*" capture="camera" required onchange="handleFile(this, 'box_mkt', 'photoMkt')">
                </div>
                <button type="submit" class="btn btn-success w-100 btn-submit shadow">SIMPAN DATA PROSPEK</button>
            </form>
        </div>
    </div>
</div>

<div id="pageHistory" class="form-section">
    <div class="container">
        <h6 class="fw-bold mb-3">Aktivitas Terbaru Anda</h6>
        <div id="logList"></div>
    </div>
</div>

<div class="bottom-nav">
    <a href="#" class="nav-link active" id="navHome" onclick="showPage('pageHome')">
        <i class="bi bi-grid-fill"></i>Beranda
    </a>
    <a href="#" class="nav-link" onclick="showPage('pageColl')">
        <i class="bi bi-currency-dollar"></i>Collection
    </a>
    <a href="#" class="nav-link" onclick="showPage('pageMkt')">
        <i class="bi bi-person-plus"></i>Marketing
    </a>
    <a href="#" class="nav-link" onclick="showPage('pageHistory')">
        <i class="bi bi-clipboard-data"></i>Riwayat
    </a>
</div>

<script>
    // GANTI DENGAN URL WEB APP GOOGLE SCRIPT ANDA
    const WEB_APP_URL = "URL_GOOGLE_SCRIPT_ANDA";
    
    let photoColl = "";
    let photoMkt = "";

    window.onload = () => {
        document.getElementById('dateNow').innerText = new Date().toLocaleDateString('id-ID', {day:'numeric', month:'short', year:'numeric'});
        loadInitialData();
    };

    function showPage(pageId) {
        document.querySelectorAll('.form-section').forEach(p => p.classList.remove('active'));
        document.getElementById(pageId).classList.add('active');
        window.scrollTo(0,0);
    }

    function formatRupiah(el) {
        let val = el.value.replace(/[^0-9]/g, '');
        el.value = val ? 'Rp ' + parseInt(val).toLocaleString('id-ID') : '';
    }

    // LOAD NAMA PETUGAS & RIWAYAT AWAL
    async function loadInitialData() {
        try {
            const res = await fetch(WEB_APP_URL + "?action=init").then(r => r.json());
            
            // Isi Dropdown Petugas
            const selects = document.querySelectorAll('.select-petugas');
            selects.forEach(s => {
                s.innerHTML = '<option value="" disabled selected>-- Pilih Nama --</option>';
                res.petugas.forEach(p => s.innerHTML += `<option value="${p.nama}">${p.nama}</option>`);
            });

            // Update Dashboard
            document.getElementById('dashSetoran').innerText = 'Rp ' + res.totalSetoran.toLocaleString('id-ID');
            document.getElementById('dashMkt').innerText = res.totalMkt;

            // Render Riwayat
            const log = document.getElementById('logList');
            log.innerHTML = "";
            res.riwayat.forEach(h => {
                const color = h.type === 'COLLECTION' ? 'primary' : 'success';
                log.innerHTML += `
                    <div class="card border-0 shadow-sm rounded-4 mb-2 p-3 border-start border-4 border-${color}">
                        <div class="d-flex justify-content-between">
                            <small class="fw-bold text-${color}">${h.type}</small>
                            <small class="text-muted" style="font-size:0.6rem">${h.waktu}</small>
                        </div>
                        <div class="fw-bold">${h.nama}</div>
                        <small class="text-muted">${h.info}</small>
                    </div>`;
            });

        } catch(e) { console.error("Sync gagal."); }
    }

    // HANDLE KAMERA & RESIZE
    function handleFile(input, boxId, varName) {
        const reader = new FileReader();
        reader.onload = function(e) {
            const img = new Image();
            img.src = e.target.result;
            img.onload = function() {
                const canvas = document.createElement('canvas');
                const ctx = canvas.getContext('2d');
                const maxW = 600;
                const scale = maxW / img.width;
                canvas.width = maxW; canvas.height = img.height * scale;
                ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                
                const base64 = canvas.toDataURL('image/jpeg', 0.7);
                if(varName === 'photoColl') photoColl = base64;
                else photoMkt = base64;

                document.getElementById(boxId).innerHTML = `<img src="${base64}">`;
            }
        };
        reader.readAsDataURL(input.files[0]);
    }

    // GET GPS COORDINATES
    function getGPS() {
        return new Promise((resolve) => {
            navigator.geolocation.getCurrentPosition(
                (pos) => resolve(`${pos.coords.latitude},${pos.coords.longitude}`),
                () => resolve("0,0"),
                { timeout: 5000 }
            );
        });
    }

    // SUBMIT COLLECTION
    document.getElementById('formCollection').onsubmit = async function(e) {
        e.preventDefault();
        const btn = e.submitter; btn.disabled = true; btn.innerText = "Mengirim...";
        
        const loc = await getGPS();
        const payload = {
            action: 'SAVE_COLLECTION',
            petugas: document.getElementById('p_coll').value,
            nasabah: document.getElementById('n_coll').value,
            status: document.getElementById('s_coll').value,
            nominal: document.getElementById('v_coll').value.replace(/[^0-9]/g, ''),
            foto: photoColl,
            gps: loc
        };

        await sendData(payload);
    };

    // SUBMIT MARKETING
    document.getElementById('formMarketing').onsubmit = async function(e) {
        e.preventDefault();
        const btn = e.submitter; btn.disabled = true; btn.innerText = "Menyimpan...";
        
        const loc = await getGPS();
        const payload = {
            action: 'SAVE_MARKETING',
            petugas: document.getElementById('p_mkt').value,
            nasabah: document.getElementById('n_mkt').value,
            wa: document.getElementById('wa_mkt').value,
            produk: document.getElementById('prod_mkt').value,
            foto: photoMkt,
            gps: loc
        };

        await sendData(payload);
    };

    async function sendData(payload) {
        try {
            await fetch(WEB_APP_URL, { 
                method: 'POST', 
                mode: 'no-cors', 
                body: JSON.stringify(payload) 
            });
            alert("Berhasil! Data telah terkirim ke sistem.");
            location.reload();
        } catch(e) {
            alert("Terjadi kesalahan koneksi.");
            location.reload();
        }
    }
</script>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
