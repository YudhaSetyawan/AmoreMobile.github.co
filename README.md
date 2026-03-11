<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>BPR BKK Jateng - Enterprise Reporting</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css">
    <style>
        :root { --bkk-blue: #003366; --bkk-gold: #FFD700; --bkk-green: #198754; }
        body { background: #f8f9fa; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; padding-bottom: 90px; }
        
        /* Header & Dash */
        .header-app { background: linear-gradient(135deg, var(--bkk-blue) 0%, #004d99 100%); color: white; padding: 25px 20px 55px; border-bottom: 4px solid var(--bkk-gold); border-radius: 0 0 25px 25px; }
        .stats-card { background: white; border-radius: 15px; padding: 18px; margin: -35px 15px 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        
        /* Navigation & Sections */
        #pageLogin { position: fixed; inset: 0; background: white; z-index: 10000; display: flex; align-items: center; }
        .section { display: none; animation: fadeIn 0.4s ease-out; }
        .section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Floating Nav */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; padding: 10px 0; box-shadow: 0 -3px 15px rgba(0,0,0,0.05); border-radius: 20px 20px 0 0; border-top: 1px solid #eee; z-index: 1000; }
        .nav-link { flex: 1; text-align: center; color: #64748b; text-decoration: none; font-size: 0.7rem; transition: 0.3s; }
        .nav-link.active { color: var(--bkk-blue); transform: translateY(-3px); font-weight: bold; }
        .nav-link i { font-size: 1.4rem; display: block; margin-bottom: 2px; }

        /* Form Components */
        .preview-box { width: 100%; height: 180px; background: #f1f5f9; border-radius: 12px; border: 2px dashed #cbd5e1; display: flex; align-items: center; justify-content: center; overflow: hidden; position: relative; }
        .preview-box img { width: 100%; height: 100%; object-fit: cover; }
        .btn-submit { border-radius: 10px; padding: 14px; font-weight: bold; text-transform: uppercase; letter-spacing: 0.5px; transition: 0.3s; }
        
        /* Loading Overlay */
        #loader { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.6); z-index: 11000; align-items: center; justify-content: center; color: white; flex-direction: column; }
    </style>
</head>
<body>

<div id="loader">
    <div class="spinner-border text-light mb-2"></div>
    <span id="loaderText">Memproses...</span>
</div>

<div id="pageLogin">
    <div class="container p-4">
        <div class="text-center mb-5">
            <h2 class="fw-bold" style="color: var(--bkk-blue);">BPR BKK JATENG</h2>
            <p class="text-muted">Digital Reporting System v2.0</p>
        </div>
        <form id="formLogin" class="bg-white p-4 rounded-4 shadow-sm border">
            <div class="mb-3">
                <label class="form-label small fw-bold">User ID</label>
                <input type="text" id="user" class="form-control form-control-lg fs-6" placeholder="Masukkan Username" required>
            </div>
            <div class="mb-4">
                <label class="form-label small fw-bold">Password</label>
                <input type="password" id="pass" class="form-control form-control-lg fs-6" placeholder="••••••••" required>
            </div>
            <button type="submit" id="btnLogin" class="btn btn-primary w-100 py-3 fw-bold shadow-sm">LOG IN</button>
        </form>
    </div>
</div>

<div class="header-app d-flex justify-content-between align-items-center">
    <div>
        <h5 class="fw-bold mb-0">PT BPR BKK JATENG</h5>
        <small id="userGreet" class="opacity-75">Selamat bertugas...</small>
    </div>
    <div class="bg-white rounded-circle p-2 shadow-sm cursor-pointer" onclick="logout()">
        <i class="bi bi-box-arrow-right text-danger fs-5"></i>
    </div>
</div>

<div id="pageHome" class="section active">
    <div class="stats-card">
        <div class="d-flex justify-content-between align-items-center mb-2">
            <div>
                <small class="text-muted d-block">Realisasi Koleksi Hari Ini</small>
                <h4 id="dashSetoran" class="fw-bold text-primary mb-0">Rp 0</h4>
            </div>
            <div class="text-end">
                <span id="txtPersen" class="badge bg-primary rounded-pill">0%</span>
            </div>
        </div>
        <div class="progress" style="height: 8px; border-radius: 10px; background: #e9ecef;">
            <div id="barTarget" class="progress-bar progress-bar-striped progress-bar-animated" style="width: 0%; background: var(--bkk-blue);"></div>
        </div>
    </div>

    <div class="container px-3">
        <h6 class="fw-bold text-muted mb-3 mt-4 px-1">MENU UTAMA</h6>
        <div class="row g-3">
            <div class="col-6" onclick="showPage('pageColl')">
                <div class="card border-0 shadow-sm text-center p-4 rounded-4 h-100 border-bottom border-4 border-primary">
                    <i class="bi bi-currency-dollar fs-1 text-primary mb-2"></i>
                    <span class="small fw-bold">LAPORAN KOLEKSI</span>
                </div>
            </div>
            <div class="col-6" onclick="showPage('pageMkt')">
                <div class="card border-0 shadow-sm text-center p-4 rounded-4 h-100 border-bottom border-4 border-success">
                    <i class="bi bi-person-plus fs-1 text-success mb-2"></i>
                    <span class="small fw-bold">MARKETING</span>
                </div>
            </div>
        </div>
    </div>
</div>

<div id="pageColl" class="section p-3">
    <div class="card border-0 shadow-sm rounded-4 p-4">
        <div class="d-flex align-items-center mb-4 text-primary" onclick="showPage('pageHome')">
            <i class="bi bi-chevron-left fs-5 me-2"></i><h5 class="fw-bold mb-0">Input Koleksi</h5>
        </div>
        <form id="formCollection">
            <div class="mb-3">
                <label class="form-label small fw-bold">Petugas</label>
                <input type="text" id="p_coll" class="form-control bg-light" readonly>
            </div>
            <div class="mb-3">
                <label class="form-label small fw-bold">Nama Nasabah</label>
                <input type="text" id="n_coll" class="form-control border-primary" placeholder="Cth: Budiman" required>
            </div>
            <div class="row g-2 mb-3">
                <div class="col-6">
                    <label class="form-label small fw-bold">Status</label>
                    <select id="s_coll" class="form-select">
                        <option value="Titip Angsuran">Titip Angsuran</option>
                        <option value="Janji Bayar">Janji Bayar</option>
                        <option value="Rumah Kosong">Rumah Kosong</option>
                    </select>
                </div>
                <div class="col-6">
                    <label class="form-label small fw-bold">Nominal (Rp)</label>
                    <input type="number" id="v_coll" class="form-control fw-bold text-primary" placeholder="0">
                </div>
            </div>
            <div class="mb-4">
                <label class="form-label small fw-bold d-block">Foto Bukti & Lokasi</label>
                <div class="preview-box" id="box_coll" onclick="document.getElementById('f_coll').click()">
                    <i class="bi bi-camera-fill fs-1 text-muted"></i>
                </div>
                <input type="file" id="f_coll" class="d-none" accept="image/*" capture="camera" onchange="handleFile(this, 'box_coll')">
            </div>
            <button type="submit" class="btn btn-primary w-100 btn-submit shadow">SUBMIT DATA KOLEKSI</button>
        </form>
    </div>
</div>

<div id="pageMkt" class="section p-3">
    <div class="card border-0 shadow-sm rounded-4 p-4">
        <div class="d-flex align-items-center mb-4 text-success" onclick="showPage('pageHome')">
            <i class="bi bi-chevron-left fs-5 me-2"></i><h5 class="fw-bold mb-0">Input Prospek</h5>
        </div>
        <form id="formMarketing">
            <div class="mb-3">
                <label class="form-label small fw-bold">Calon Nasabah</label>
                <input type="text" id="n_mkt" class="form-control border-success" placeholder="Nama Lengkap" required>
            </div>
            <div class="mb-3">
                <label class="form-label small fw-bold">No. WhatsApp</label>
                <input type="tel" id="wa_mkt" class="form-control" placeholder="0812xxx" required>
            </div>
            <div class="mb-3">
                <label class="form-label small fw-bold">Kategori Produk</label>
                <select id="prod_mkt" class="form-select mb-2">
                    <option value="Kredit">Kredit Mikro/Pegawai</option>
                    <option value="Tabungan">Tabungan Utama</option>
                    <option value="Deposito">Deposito</option>
                </select>
                <select id="stat_mkt" class="form-select bg-light">
                    <option value="Hot Prospect">🔥 Hot Prospect</option>
                    <option value="Warm Prospect">⚡ Warm Prospect</option>
                </select>
            </div>
            <div class="mb-4">
                <div class="preview-box" id="box_mkt" onclick="document.getElementById('f_mkt').click()">
                    <i class="bi bi-camera-fill fs-1 text-success"></i>
                </div>
                <input type="file" id="f_mkt" class="d-none" accept="image/*" capture="camera" onchange="handleFile(this, 'box_mkt')">
            </div>
            <button type="submit" class="btn btn-success w-100 btn-submit shadow">SIMPAN DATA PROSPEK</button>
        </form>
    </div>
</div>

<div id="pageHistory" class="section p-3">
    <div class="d-flex justify-content-between align-items-center mb-3">
        <h6 class="fw-bold mb-0 text-muted">RIWAYAT AKTIVITAS</h6>
        <button class="btn btn-sm btn-outline-primary rounded-pill px-3" onclick="syncAll()"><i class="bi bi-arrow-repeat"></i> Sync</button>
    </div>
    <div id="logList">
        <div class="text-center p-5 text-muted"><div class="spinner-border spinner-border-sm mb-2"></div><br>Menghubungkan ke Server...</div>
    </div>
</div>

<div class="bottom-nav">
    <a href="javascript:void(0)" class="nav-link active" id="nav_pageHome" onclick="showPage('pageHome')">
        <i class="bi bi-grid-1x2"></i>Home
    </a>
    <a href="javascript:void(0)" class="nav-link" id="nav_pageColl" onclick="showPage('pageColl')">
        <i class="bi bi-wallet2"></i>Koleksi
    </a>
    <a href="javascript:void(0)" class="nav-link" id="nav_pageHistory" onclick="showPage('pageHistory')">
        <i class="bi bi-clock-history"></i>Riwayat
    </a>
</div>

<script>
    // --- KONFIGURASI ---
    const WEB_APP_URL = "https://script.google.com/macros/s/AKfycby3K3umrm4Xf3xD3U1dmsewzkmuZ-AYOVCfeFCR0Ep4iIfSwqYCfesmqobucYrKx-Aaeg/exec"; 
    let currentPhoto = "";

    // --- INITIALIZATION ---
    window.onload = () => {
        if (localStorage.getItem('isLoggedIn') === 'true') {
            document.getElementById('pageLogin').style.display = 'none';
            applySession();
            syncAll();
        }
    };

    function applySession() {
        const u = JSON.parse(localStorage.getItem('userData'));
        document.getElementById('userGreet').innerText = `Halo, ${u.nama}`;
        document.getElementById('p_coll').value = u.nama;
    }

    // --- CORE FUNCTIONS ---
    async function syncAll() {
        try {
            const res = await fetch(`${WEB_APP_URL}?action=init`).then(r => r.json());
            
            // Update Dashboard
            document.getElementById('dashSetoran').innerText = `Rp ${Number(res.totalSetoran).toLocaleString('id-ID')}`;
            document.getElementById('barTarget').style.width = `${res.persenTarget}%`;
            document.getElementById('txtPersen').innerText = `${res.persenTarget}%`;

            // Update Riwayat List
            const list = document.getElementById('logList');
            list.innerHTML = res.riwayat.length ? "" : '<div class="text-center p-5 text-muted">Belum ada aktivitas hari ini</div>';
            
            res.riwayat.forEach(h => {
                const color = h.type === 'COLLECTION' ? 'primary' : 'success';
                list.innerHTML += `
                    <div class="card border-0 shadow-sm rounded-4 mb-2 overflow-hidden border-start border-4 border-${color}">
                        <div class="card-body p-3">
                            <div class="d-flex justify-content-between">
                                <span class="badge bg-${color} mb-1">${h.type}</span>
                                <small class="text-muted fw-bold">${h.waktu}</small>
                            </div>
                            <div class="fw-bold">${h.nama}</div>
                            <div class="text-muted small">${h.info}</div>
                        </div>
                    </div>`;
            });
        } catch (e) {
            console.error("Sync Gagal:", e);
            document.getElementById('logList').innerHTML = '<div class="alert alert-danger mx-2">Koneksi Gagal. Cek internet/URL Apps Script.</div>';
        }
    }

    async function submitForm(e, actionType) {
        e.preventDefault();
        if (!currentPhoto) return alert("Wajib lampirkan foto bukti!");

        setLoading(true, "Mendapatkan Lokasi GPS...");

        navigator.geolocation.getCurrentPosition(async (pos) => {
            const payload = {
                action: actionType,
                petugas: JSON.parse(localStorage.getItem('userData')).nama,
                gps: `${pos.coords.latitude},${pos.coords.longitude}`,
                foto: currentPhoto
            };

            if (actionType === 'SAVE_COLLECTION') {
                payload.nasabah = document.getElementById('n_coll').value;
                payload.status = document.getElementById('s_coll').value;
                payload.nominal = document.getElementById('v_coll').value;
            } else {
                payload.nasabah = document.getElementById('n_mkt').value;
                payload.wa = document.getElementById('wa_mkt').value;
                payload.produk = `${document.getElementById('prod_mkt').value} (${document.getElementById('stat_mkt').value})`;
            }

            setLoading(true, "Mengirim Laporan ke Server...");

            try {
                const response = await fetch(WEB_APP_URL, { 
                    method: 'POST', 
                    body: JSON.stringify(payload) 
                });
                const res = await response.json();

                if (res.status === "SUCCESS") {
                    alert("Berhasil! Laporan telah tersimpan.");
                    location.reload();
                } else {
                    alert("GAGAL: " + res.message);
                }
            } catch (err) {
                alert("Kesalahan Sistem: " + err.message);
            } finally {
                setLoading(false);
            }
        }, (err) => {
            setLoading(false);
            alert("Gagal mendapatkan GPS. Mohon aktifkan GPS HP Anda.");
        }, { enableHighAccuracy: true });
    }

    // --- UI HELPERS ---
    function showPage(id) {
        document.querySelectorAll('.section').forEach(p => p.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
        const navEl = document.getElementById('nav_' + id);
        if(navEl) navEl.classList.add('active');
    }

    function handleFile(input, boxId) {
        const reader = new FileReader();
        reader.onload = (e) => {
            currentPhoto = e.target.result;
            document.getElementById(boxId).innerHTML = `<img src="${currentPhoto}">`;
        };
        reader.readAsDataURL(input.files[0]);
    }

    function setLoading(show, text = "") {
        const el = document.getElementById('loader');
        document.getElementById('loaderText').innerText = text;
        el.style.display = show ? 'flex' : 'none';
    }

    // --- AUTH ---
    document.getElementById('formLogin').onsubmit = async (e) => {
        e.preventDefault();
        setLoading(true, "Verifikasi Identitas...");
        try {
            const res = await fetch(WEB_APP_URL, { 
                method: 'POST', 
                body: JSON.stringify({ action: 'LOGIN', username: document.getElementById('user').value, password: document.getElementById('pass').value })
            }).then(r => r.json());

            if (res.status === "SUCCESS") {
                localStorage.setItem('isLoggedIn', 'true');
                localStorage.setItem('userData', JSON.stringify(res.user));
                location.reload();
            } else {
                alert("Username atau Password Salah!");
            }
        } catch(e) { alert("Error Server!"); }
        finally { setLoading(false); }
    };

    document.getElementById('formCollection').onsubmit = (e) => submitForm(e, 'SAVE_COLLECTION');
    document.getElementById('formMarketing').onsubmit = (e) => submitForm(e, 'SAVE_MARKETING');

    function logout() { localStorage.clear(); location.reload(); }
</script>
</body>
</html>
