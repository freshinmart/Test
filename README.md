<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QRISku - Pembayaran QRIS</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* Reset dan Base Styles */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            -webkit-tap-highlight-color: transparent;
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
            touch-action: manipulation;
        }

        html, body {
            height: 100%;
            overflow: hidden;
            margin: 0;
            background-color: #f8fafc;
        }

        body {
            color: #1e293b;
            line-height: 1.5;
            position: relative;
            display: flex;
            flex-direction: column;
        }

        /* Container - Diperbaiki untuk mobile */
        .container {
            width: 100%;
            max-width: 100%;
            margin: 0 auto;
            min-height: 100vh;
            height: auto;
            display: flex;
            flex-direction: column;
            padding: 12px;
            padding-bottom: 80px; /* Space untuk bottom nav */
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
            background-color: #ffffff;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
            flex: 1;
        }

        /* Header - Diperbaiki ukuran untuk mobile */
        header {
            text-align: center;
            margin-bottom: 16px;
            padding: 12px 16px;
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            color: #ffffff;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
            flex-shrink: 0;
            position: relative;
            width: 100%;
        }

        header h1 {
            font-size: 1.3rem;
            font-weight: 600;
            margin-bottom: 4px;
            padding: 0 40px; /* Space untuk tombol */
        }

        header p {
            font-size: 0.8rem;
            opacity: 0.85;
            font-weight: 400;
            padding: 0 20px;
        }

        .profile-btn, .fullscreen-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(255, 255, 255, 0.15);
            border: none;
            color: #ffffff;
            font-size: 1.1rem;
            cursor: pointer;
            width: 36px;
            height: 36px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: background-color 0.2s ease, transform 0.2s ease;
        }

        .profile-btn {
            right: 12px;
        }

        .fullscreen-btn {
            left: 12px;
        }

        /* Input Section - Diperbaiki untuk mobile */
        .input-section {
            margin-bottom: 16px;
            background-color: #ffffff;
            padding: 12px;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
            border: 1px solid #e2e8f0;
            flex: 1;
            display: flex;
            flex-direction: column;
            min-height: 300px; /* Minimum height untuk konten */
        }

        .input-section h3 {
            margin-bottom: 12px;
            color: #1e293b;
            font-size: 1rem;
            font-weight: 500;
            text-align: center;
            flex-shrink: 0;
        }

        .amount-display, .phone-display, .calculator-display {
            text-align: right;
            font-size: 1.4rem;
            font-weight: 600;
            margin-bottom: 12px;
            padding: 12px;
            background-color: #f1f5f9;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
            color: #1e293b;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            word-break: break-all;
            flex-shrink: 0;
            width: 100%;
        }

        /* Keypads - Diperbaiki untuk mobile */
        .numeric-keypad-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            min-height: 200px; /* Minimum height untuk keypad */
        }

        .numeric-keypad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 8px;
            flex: 1;
        }

        .calculator-keypad {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
            flex: 1;
        }

        .key {
            padding: 12px 8px;
            background: linear-gradient(145deg, #ffffff, #f1f5f9);
            color: #1e293b;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            aspect-ratio: 1; /* Membuat tombol persegi */
        }

        .operator-key {
            background: linear-gradient(145deg, #e2e8f0, #cbd5e1);
            color: #1e40af;
        }

        .equals-key {
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            color: #ffffff;
        }

        .clear-key {
            background: linear-gradient(145deg, #fca5a5, #f87171);
            color: #ffffff;
        }

        .key:hover {
            background: linear-gradient(145deg, #f1f5f9, #e2e8f0);
            transform: translateY(-1px);
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
        }

        .key:active {
            transform: translateY(0);
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
        }

        /* Action Buttons - Diperbaiki ukuran */
        .action-buttons {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 8px;
            margin-top: 12px;
            flex-shrink: 0;
        }

        .convert-button, .clear-button, .save-button, .confirm-payment {
            padding: 12px;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            border: none;
            width: 100%;
            min-height: 44px;
        }

        .convert-button, .save-button {
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            color: #ffffff;
        }

        .confirm-payment {
            background: linear-gradient(135deg, #15803d, #22c55e);
            color: #ffffff;
            margin-top: 6px;
        }

        .clear-button {
            background: #f1f5f9;
            color: #1e293b;
            border: 1px solid #e2e8f0;
        }

        .convert-button:hover, .save-button:hover {
            background: linear-gradient(135deg, #1e40af, #60a5fa);
            transform: translateY(-1px);
        }

        .confirm-payment:hover {
            background: linear-gradient(135deg, #16a34a, #4ade80);
            transform: translateY(-1px);
        }

        .clear-button:hover {
            background: #e2e8f0;
            transform: translateY(-1px);
        }

        /* Navigation - Diperbaiki posisi */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background: #ffffff;
            display: flex;
            justify-content: space-around;
            padding: 8px 0;
            box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.1);
            z-index: 999;
            border-top: 1px solid #e2e8f0;
        }

        .nav-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            background: none;
            border: none;
            font-size: 0.75rem;
            font-weight: 500;
            color: #64748b;
            cursor: pointer;
            transition: color 0.2s ease, transform 0.2s ease;
            flex: 1;
            padding: 8px 4px;
            min-height: 60px;
        }

        .nav-btn.active {
            color: #1e3a8a;
        }

        .nav-btn i {
            font-size: 1.25rem;
            margin-bottom: 4px;
        }

        .nav-btn:hover {
            color: #1e3a8a;
            transform: translateY(-1px);
        }

        .inventory-btn {
            position: fixed;
            bottom: 70px;
            right: 16px;
            width: 50px;
            height: 50px;
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            color: #ffffff;
            border: none;
            border-radius: 50%;
            font-size: 1.25rem;
            cursor: pointer;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.15);
            z-index: 998;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
        }

        .inventory-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
        }

        /* Pages - Diperbaiki layout */
        .page {
            display: none;
            animation: fadeIn 0.3s ease;
            min-height: calc(100vh - 140px);
            flex-direction: column;
            width: 100%;
        }

        .page.active {
            display: flex;
        }

        /* Tabs */
        .finance-tabs, .settings-tabs {
            display: flex;
            margin-bottom: 16px;
            border-bottom: 1px solid #e2e8f0;
        }

        .finance-tab, .tab-btn {
            flex: 1;
            padding: 10px;
            background: none;
            border: none;
            border-bottom: 2px solid transparent;
            font-weight: 500;
            color: #64748b;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .finance-tab.active, .tab-btn.active {
            color: #1e3a8a;
            border-bottom-color: #1e3a8a;
        }

        .tab-content {
            display: none;
            flex: 1;
            overflow-y: auto;
        }

        .tab-content.active {
            display: block;
        }

        /* Modal - Diperbaiki untuk mobile */
        .settings-modal, .payment-modal, .pulsa-modal, .password-modal, .debt-edit-modal, .inventory-edit-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: flex-start; /* Diubah dari center ke flex-start */
            padding: 20px 16px;
            animation: fadeIn 0.3s ease;
            overflow-y: auto;
        }

        .settings-content, .payment-content, .pulsa-content, .password-content, .debt-edit-content, .inventory-edit-content {
            background: #ffffff;
            padding: 20px;
            border-radius: 12px;
            width: 100%;
            max-width: 100%;
            max-height: 90vh;
            overflow-y: auto;
            position: relative;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            animation: slideUp 0.3s ease;
            margin-top: 20px; /* Memberi jarak dari atas */
        }

        /* Modal Pembayaran - Diperbaiki untuk mobile */
        .payment-content {
            max-width: 100%;
            max-height: 95vh;
            padding: 15px;
            overflow-y: auto;
        }

        .payment-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .payment-time {
            font-size: 0.9rem;
            color: #64748b;
        }

        .payment-title {
            font-size: 1.2rem;
            font-weight: 600;
            text-align: center;
            margin-bottom: 5px;
        }

        .payment-subtitle {
            text-align: center;
            color: #64748b;
            font-size: 0.8rem;
            margin-bottom: 15px;
        }

        .payment-amount {
            font-size: 1.5rem;
            font-weight: 600;
            text-align: center;
            margin-bottom: 15px;
            color: #1e293b;
            background-color: #f1f5f9;
            padding: 10px;
            border-radius: 8px;
        }

        .payment-qr {
            display: flex;
            justify-content: center;
            margin-bottom: 15px;
        }

        .payment-qr img {
            width: 200px;
            height: 200px;
            max-width: 80%;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
        }

        .payment-info {
            margin-top: 10px;
            padding-top: 10px;
            border-top: 1px solid #e2e8f0;
        }

        .info-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 0.85rem;
        }

        .info-label {
            font-weight: 500;
            color: #64748b;
        }

        .info-value {
            font-weight: 500;
            color: #1e293b;
        }

        .close-btn {
            position: absolute;
            top: 12px;
            right: 12px;
            background: none;
            border: none;
            color: #64748b;
            font-size: 1.25rem;
            cursor: pointer;
            transition: color 0.2s ease;
        }

        .close-btn:hover {
            color: #1e293b;
        }

        /* Form Elements - Diperbaiki untuk mobile */
        .form-group {
            margin-bottom: 16px;
        }

        .form-group label {
            display: block;
            margin-bottom: 6px;
            font-weight: 500;
            color: #1e293b;
            font-size: 0.875rem;
        }

        .form-group input, .form-group textarea, .form-group select {
            width: 100%;
            padding: 12px;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            font-size: 1rem;
            color: #1e293b;
            background-color: #f8fafc;
            transition: border-color 0.2s ease, box-shadow 0.2s ease;
        }

        .form-group input:focus, .form-group textarea:focus, .form-group select:focus {
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
            outline: none;
        }

        /* Items */
        .transaction-item, .debt-item, .inventory-item, .calculator-history-item {
            background: #ffffff;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 8px;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
            border: 1px solid #e2e8f0;
            position: relative;
        }

        /* Kalkulator - Diperbaiki layout untuk mobile */
        .calculator-container {
            display: flex;
            flex-direction: column;
            height: auto;
            min-height: 400px;
        }

        .calculator-main {
            display: flex;
            flex-direction: column;
            gap: 15px;
            flex: 1;
        }

        .calculator-display-section {
            flex: 1;
            display: flex;
            flex-direction: column;
        }

        .calculator-history-section {
            flex: 1;
            display: flex;
            flex-direction: column;
            max-height: 200px;
        }

        .calculator-history {
            flex: 1;
            overflow-y: auto;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            padding: 10px;
            background-color: #f8fafc;
            min-height: 150px;
        }

        .calculator-history-title {
            font-size: 0.875rem;
            font-weight: 600;
            margin-bottom: 8px;
            color: #475569;
        }

        .calculator-history-item {
            font-size: 0.8rem;
            padding: 6px;
            margin-bottom: 4px;
            background-color: #ffffff;
            cursor: pointer;
        }

        .calculator-history-expression {
            color: #64748b;
        }

        .calculator-history-result {
            font-weight: 600;
            color: #1e293b;
            text-align: right;
        }

        /* Footer */
        footer {
            text-align: center;
            margin-top: 20px;
            padding: 12px;
            color: #64748b;
            font-size: 0.75rem;
            flex-shrink: 0;
            width: 100%;
        }

        /* Loading */
        .loading {
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .loading-spinner {
            width: 40px;
            height: 40px;
            border: 4px solid #f1f5f9;
            border-top: 4px solid #3b82f6;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 12px;
        }

        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes slideUp {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Responsive improvements untuk layar sangat kecil */
        @media (max-width: 360px) {
            .container {
                padding: 10px;
                padding-bottom: 70px;
            }
            
            header {
                padding: 10px 12px;
            }
            
            header h1 {
                font-size: 1.2rem;
                padding: 0 35px;
            }
            
            header p {
                font-size: 0.75rem;
                padding: 0 15px;
            }
            
            .profile-btn, .fullscreen-btn {
                width: 32px;
                height: 32px;
                font-size: 1rem;
            }
            
            .amount-display, .phone-display, .calculator-display {
                font-size: 1.2rem;
                min-height: 50px;
                padding: 10px;
            }
            
            .key {
                padding: 10px 6px;
                font-size: 1rem;
                min-height: 45px;
            }
            
            .numeric-keypad, .calculator-keypad {
                gap: 6px;
            }
            
            .input-section {
                padding: 10px;
            }
            
            .nav-btn {
                font-size: 0.7rem;
                padding: 6px 2px;
            }
            
            .nav-btn i {
                font-size: 1.1rem;
            }
            
            .inventory-btn {
                bottom: 65px;
                right: 12px;
                width: 45px;
                height: 45px;
                font-size: 1.1rem;
            }
            
            .payment-qr img {
                width: 180px;
                height: 180px;
            }
        }

        @media (max-height: 600px) {
            .input-section {
                min-height: 250px;
            }
            
            .numeric-keypad-container {
                min-height: 150px;
            }
            
            .key {
                min-height: 40px;
                padding: 8px 4px;
            }
            
            header h1 {
                font-size: 1.1rem;
            }
            
            header p {
                font-size: 0.7rem;
            }
        }

        /* Landscape mode improvements */
        @media (max-width: 768px) and (orientation: landscape) {
            .container {
                padding-bottom: 70px;
            }
            
            .page {
                min-height: auto;
            }
            
            .input-section {
                min-height: 200px;
            }
            
            .numeric-keypad-container {
                min-height: 120px;
            }
        }

        /* Fullscreen improvements */
        body:fullscreen {
            background-color: #f8fafc;
        }

        body:fullscreen .container {
            max-width: 100%;
            border-radius: 0;
            box-shadow: none;
            height: 100vh;
        }

        /* Pastikan konten tidak terpotong saat keyboard muncul */
        @media (max-height: 500px) {
            .container {
                padding-bottom: 60px;
            }
            
            .bottom-nav {
                padding: 6px 0;
            }
            
            .nav-btn {
                min-height: 50px;
                padding: 4px 2px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Home Page -->
        <div class="page active" id="homePage">
            <header>
                <button class="fullscreen-btn" id="fullscreenBtn"><i class="fas fa-expand"></i></button>
                <h1 id="headerTitle">QRISku</h1>
                <p id="headerSubtitle">Pembayaran QRIS Mudah dan Cepat</p>
                <button class="profile-btn" id="profileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="input-section">
                <h3>Tambah Nominal</h3>
                <div class="amount-display" id="amountDisplay">0</div>
                
                <div class="numeric-keypad-container">
                    <div class="numeric-keypad">
                        <button class="key" data-value="1">1</button>
                        <button class="key" data-value="2">2</button>
                        <button class="key" data-value="3">3</button>
                        <button class="key" data-value="4">4</button>
                        <button class="key" data-value="5">5</button>
                        <button class="key" data-value="6">6</button>
                        <button class="key" data-value="7">7</button>
                        <button class="key" data-value="8">8</button>
                        <button class="key" data-value="9">9</button>
                        <button class="key" data-value="000">000</button>
                        <button class="key" data-value="0">0</button>
                        <button class="key" id="backspaceBtn"><i class="fas fa-backspace"></i></button>
                    </div>
                </div>
                
                <div class="action-buttons">
                    <button class="convert-button" id="convertButton">Terapkan</button>
                    <button class="clear-button" id="clearButton">Clear</button>
                </div>
                
                <div class="loading" id="loading" style="display: none;">
                    <div class="loading-spinner"></div>
                    <p>Sedang memproses...</p>
                </div>
            </div>

            <footer>
                <p id="footerText">© 2025 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- Finance Page -->
        <div class="page" id="financePage">
            <header>
                <button class="fullscreen-btn"><i class="fas fa-expand"></i></button>
                <h1>Keuangan</h1>
                <p>Kelola transaksi dan hutang piutang</p>
                <button class="profile-btn" id="financeProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="finance-tabs">
                <button class="finance-tab active" data-tab="transactions-tab">Transaksi</button>
                <button class="finance-tab" data-tab="debts-tab">Hutang</button>
            </div>

            <div id="transactions-tab" class="tab-content active">
                <div class="transaction-list" id="transactionList"></div>
            </div>

            <div id="debts-tab" class="tab-content">
                <div class="debt-form">
                    <h3>Tambah Hutang Baru</h3>
                    <div class="form-group">
                        <label for="debtName">Nama</label>
                        <input type="text" id="debtName" placeholder="Nama orang yang berhutang" list="debtNameList">
                        <datalist id="debtNameList"></datalist>
                    </div>
                    <div class="form-group">
                        <label for="debtPhone">Nomor Telepon</label>
                        <input type="text" id="debtPhone" placeholder="Nomor telepon">
                    </div>
                    <div class="form-group">
                        <label for="debtItem">Nama Barang</label>
                        <input type="text" id="debtItem" placeholder="Nama barang yang dihutang">
                    </div>
                    <div class="form-group">
                        <label for="debtAmount">Jumlah Hutang</label>
                        <input type="number" id="debtAmount" placeholder="Jumlah hutang">
                    </div>
                    <button class="save-button" id="addDebtButton">Tambah Hutang</button>
                </div>

                <div class="debt-list" id="debtList">
                    <h3>Daftar Hutang</h3>
                </div>
            </div>

            <footer>
                <p>© 2025 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- QR Code Page -->
        <div class="page" id="qrcodePage">
            <header>
                <button class="fullscreen-btn"><i class="fas fa-expand"></i></button>
                <h1>Kode QRIS</h1>
                <p>Scan kode QRIS untuk pembayaran</p>
                <button class="profile-btn" id="qrcodeProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="qrcode-display" id="qrcodeDisplay">
                <p>Kode QRIS akan ditampilkan di sini</p>
            </div>

            <footer>
                <p>© 2025 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- Pulsa Page -->
        <div class="page" id="pulsaPage">
            <header>
                <button class="fullscreen-btn"><i class="fas fa-expand"></i></button>
                <h1>Isi Pulsa</h1>
                <p>Isi pulsa handphone dengan mudah</p>
                <button class="profile-btn" id="pulsaProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="phone-input-section">
                <h3>Masukkan nomor handphone</h3>
                <div class="phone-display" id="phoneDisplay">0</div>
                
                <div class="numeric-keypad-container">
                    <div class="numeric-keypad">
                        <button class="key" data-value="1">1</button>
                        <button class="key" data-value="2">2</button>
                        <button class="key" data-value="3">3</button>
                        <button class="key" data-value="4">4</button>
                        <button class="key" data-value="5">5</button>
                        <button class="key" data-value="6">6</button>
                        <button class="key" data-value="7">7</button>
                        <button class="key" data-value="8">8</button>
                        <button class="key" data-value="9">9</button>
                        <button class="key" data-value="+62">+62</button>
                        <button class="key" data-value="0">0</button>
                        <button class="key" id="pulsaBackspaceBtn"><i class="fas fa-backspace"></i></button>
                    </div>
                </div>
                
                <div class="action-buttons">
                    <button class="convert-button" id="savePhoneButton">Simpan</button>
                    <button class="clear-button" id="clearPhoneButton">Clear</button>
                </div>
            </div>

            <div class="phone-history" id="phoneHistory">
                <h3>Riwayat Nomor</h3>
            </div>

            <footer>
                <p>© 2025 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- Calculator Page - Diperbaiki dengan riwayat terpisah -->
        <div class="page" id="calculatorPage">
            <header>
                <button class="fullscreen-btn"><i class="fas fa-expand"></i></button>
                <h1>Kalkulator</h1>
                <p>Kalkulator lengkap dengan riwayat</p>
                <button class="profile-btn" id="calculatorProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="calculator-container">
                <div class="calculator-main">
                    <div class="calculator-display-section">
                        <div class="calculator-display" id="calculatorDisplay">0</div>
                        
                        <div class="numeric-keypad-container">
                            <div class="calculator-keypad">
                                <button class="key clear-key" id="calculatorClear">C</button>
                                <button class="key clear-key" id="calculatorClearEntry">CE</button>
                                <button class="key operator-key" id="calculatorBackspace"><i class="fas fa-backspace"></i></button>
                                <button class="key operator-key" data-operator="/">÷</button>
                                
                                <button class="key" data-value="7">7</button>
                                <button class="key" data-value="8">8</button>
                                <button class="key" data-value="9">9</button>
                                <button class="key operator-key" data-operator="*">×</button>
                                
                                <button class="key" data-value="4">4</button>
                                <button class="key" data-value="5">5</button>
                                <button class="key" data-value="6">6</button>
                                <button class="key operator-key" data-operator="-">-</button>
                                
                                <button class="key" data-value="1">1</button>
                                <button class="key" data-value="2">2</button>
                                <button class="key" data-value="3">3</button>
                                <button class="key operator-key" data-operator="+">+</button>
                                
                                <button class="key" data-value="0">0</button>
                                <button class="key" data-value=".">.</button>
                                <button class="key operator-key" data-operator="%">%</button>
                                <button class="key equals-key" id="calculatorEquals">=</button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="calculator-history-section">
                        <div class="calculator-history-title">Riwayat Perhitungan</div>
                        <div class="calculator-history" id="calculatorHistoryList"></div>
                    </div>
                </div>
            </div>

            <footer>
                <p>© 2025 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- Inventory Page -->
        <div class="page" id="inventoryPage">
            <header>
                <button class="fullscreen-btn"><i class="fas fa-expand"></i></button>
                <h1>Manajemen Inventori</h1>
                <p>Kelola stok barang dan daftar belanja</p>
                <button class="profile-btn" id="inventoryProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="inventory-form">
                <h3>Tambah Barang Baru</h3>
                <div class="form-group">
                    <label for="itemName">Nama Barang</label>
                    <input type="text" id="itemName" placeholder="Nama barang">
                </div>
                <div class="form-group">
                    <label for="itemPrice">Harga Jual</label>
                    <input type="number" id="itemPrice" placeholder="Harga Jual per item">
                </div>
                <div class="form-group">
                    <label for="itemCategory">Kategori</label>
                    <select id="itemCategory">
                        <option value="makanan">Makanan</option>
                        <option value="minuman">Minuman</option>
                        <option value="snack">Snack</option>
                        <option value="sembako">Sembako</option>
                        <option value="lainnya">Lainnya</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="itemStock">Stok Awal</label>
                    <input type="number" id="itemStock" placeholder="Jumlah stok">
                </div>
                <div class="form-group">
                    <label for="itemMinStock">Stok Minimum</label>
                    <input type="number" id="itemMinStock" placeholder="Batas stok minimum">
                </div>
                <button class="save-button" id="addItemButton">Tambah Barang</button>
            </div>
            
            <div class="inventory-management">
                <h3>Manajemen Data Inventori</h3>
                <div class="management-buttons">
                    <button id="downloadCsvButton">Download CSV</button>
                    <button id="backupButton">Backup JSON</button>
                    <label for="restoreInput">Restore JSON</label>
                    <input type="file" id="restoreInput" accept=".json" style="display:none;">
                    <label for="importCsvInput">Import CSV</label>
                    <input type="file" id="importCsvInput" accept=".csv" style="display:none;">
                </div>
                <p style="font-size: 0.75rem; text-align:center; color: #64748b;">Gunakan file .csv atau .json untuk menambah atau memulihkan data.</p>
            </div>

            <div class="inventory-list" id="inventoryList">
                <h3>Daftar Inventori</h3>
            </div>

            <div class="shopping-list" id="shoppingList">
                <h3>Daftar Belanja</h3>
            </div>

            <footer>
                <p>© 2025 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>
    </div>

    <!-- Bottom Navigation -->
    <div class="bottom-nav">
        <button class="nav-btn active" data-page="homePage"><i class="fas fa-home"></i><span>Beranda</span></button>
        <button class="nav-btn" data-page="financePage"><i class="fas fa-chart-line"></i><span>Keuangan</span></button>
        <button class="nav-btn" data-page="qrcodePage"><i class="fas fa-qrcode"></i><span>Kode QRIS</span></button>
        <button class="nav-btn" data-page="pulsaPage"><i class="fas fa-mobile-alt"></i><span>Pulsa</span></button>
        <button class="nav-btn" data-page="calculatorPage"><i class="fas fa-calculator"></i><span>Kalkulator</span></button>
    </div>

    <button class="inventory-btn" id="inventoryButton" data-page="inventoryPage">
        <i class="fas fa-box"></i>
    </button>

    <!-- Modals -->
    <div class="password-modal" id="passwordModal">
        <div class="password-content">
            <h2>Masukkan Sandi</h2>
            <input type="password" class="password-input" id="passwordInput" maxlength="6" placeholder="Masukkan 6 digit sandi">
            <div class="password-error" id="passwordError" style="display: none; color: #dc2626;">Sandi salah! Coba lagi.</div>
            <button class="save-button" id="submitPassword">Submit</button>
            <button class="close-btn" id="closePassword"><i class="fas fa-times"></i></button>
        </div>
    </div>

    <div class="settings-modal" id="settingsModal">
        <div class="settings-content">
            <button class="close-btn" id="closeSettings"><i class="fas fa-times"></i></button>
            <h2>Pengaturan QRIS</h2>
            
            <div class="settings-tabs">
                <button class="tab-btn active" data-tab="qris-settings">QRIS</button>
                <button class="tab-btn" data-tab="text-settings">Teks</button>
                <button class="tab-btn" data-tab="security-settings">Keamanan</button>
            </div>
            
            <div class="tab-content active" id="qris-settings">
                <div class="form-group">
                    <label for="merchantName">Nama Merchant</label>
                    <input type="text" id="merchantName" placeholder="Nama Merchant">
                </div>

                <div class="form-group">
                    <label for="qrisStaticCode">Kode QRIS Statis</label>
                    <textarea class="text-input" id="qrisStaticCode" placeholder="Tempel Kode QRIS statis di sini..." rows="4"></textarea>
                </div>
                
                <button class="clear-button" id="resetQrisButton">Reset QRIS</button>
            </div>
            
            <div class="tab-content" id="text-settings">
                <div class="form-group">
                    <label for="headerTitleInput">Judul Halaman</label>
                    <input type="text" id="headerTitleInput" placeholder="Judul Halaman">
                </div>
                
                <div class="form-group">
                    <label for="headerSubtitleInput">Subjudul Halaman</label>
                    <input type="text" id="headerSubtitleInput" placeholder="Subjudul Halaman">
                </div>
                
                <div class="form-group">
                    <label for="footerTextInput">Teks Footer</label>
                    <textarea id="footerTextInput" placeholder="Teks Footer" rows="2"></textarea>
                </div>
            </div>
            
            <div class="tab-content" id="security-settings">
                <div class="form-group">
                    <label for="currentPassword">Sandi Saat Ini</label>
                    <input type="password" id="currentPassword" placeholder="Masukkan sandi saat ini">
                </div>
                
                <div class="form-group">
                    <label for="newPassword">Sandi Baru</label>
                    <input type="password" id="newPassword" placeholder="Masukkan sandi baru (6 digit)">
                </div>
                
                <div class="form-group">
                    <label for="confirmPassword">Konfirmasi Sandi Baru</label>
                    <input type="password" id="confirmPassword" placeholder="Konfirmasi sandi baru">
                </div>
                
                <button class="save-button" id="changePasswordButton">Ubah Sandi</button>
            </div>
            
            <button class="save-button" id="saveButton">Simpan Pengaturan</button>
        </div>
    </div>

    <!-- Modal Pembayaran - Diperbaiki ukuran dan tata letak -->
    <div class="payment-modal" id="paymentModal">
        <div class="payment-content">
            <div class="payment-header">
                <div class="payment-time" id="paymentTime">17:05</div>
                <div class="payment-close" id="closePaymentModal"><i class="fas fa-times"></i></div>
            </div>
            
            <div class="payment-title" id="paymentTitle">QRISku</div>
            <div class="payment-subtitle">Scan QRIS berikut untuk melakukan pembayaran</div>
            
            <div class="payment-amount" id="paymentAmount">Rp 0</div>
            
            <div class="payment-qr">
                <img id="paymentQr" src="" alt="QR Code">
            </div>
            
            <div class="payment-info">
                <div class="info-item">
                    <span class="info-label">Merchant</span>
                    <span class="info-value" id="paymentMerchant">Merchant</span>
                </div>
                <div class="info-item">
                    <span class="info-label">Transaction ID</span>
                    <span class="info-value" id="transactionId">TRX-789012</span>
                </div>
            </div>
            
            <button class="confirm-payment" id="confirmPayment">Konfirmasi Pembayaran</button>
            <button class="clear-button" id="closePayment">Tutup</button>
        </div>
    </div>

    <div class="pulsa-modal" id="pulsaModal">
        <div class="pulsa-content">
            <button class="close-btn" id="closePulsaModal"><i class="fas fa-times"></i></button>
            <div class="pulsa-title">Pilih Nominal Pulsa</div>
            
            <div class="form-group">
                <label for="phoneName">Nama Kontak</label>
                <input type="text" id="phoneName" placeholder="Masukkan nama untuk nomor ini">
            </div>
            
            <div class="pulsa-options">
                <div class="pulsa-option" data-amount="5000">
                    <div class="pulsa-amount">5.000</div>
                    <div class="pulsa-price">Rp 5.000</div>
                </div>
                <div class="pulsa-option" data-amount="10000">
                    <div class="pulsa-amount">10.000</div>
                    <div class="pulsa-price">Rp 10.000</div>
                </div>
                <div class="pulsa-option" data-amount="20000">
                    <div class="pulsa-amount">20.000</div>
                    <div class="pulsa-price">Rp 20.000</div>
                </div>
                <div class="pulsa-option" data-amount="50000">
                    <div class="pulsa-amount">50.000</div>
                    <div class="pulsa-price">Rp 50.000</div>
                </div>
                <div class="pulsa-option" data-amount="100000">
                    <div class="pulsa-amount">100.000</div>
                    <div class="pulsa-price">Rp 100.000</div>
                </div>
                <div class="pulsa-option" data-amount="0">
                    <div class="pulsa-amount">Lainnya</div>
                    <div class="pulsa-price">Custom</div>
                </div>
            </div>
            
            <div class="form-group" id="customAmountGroup" style="display: none;">
                <label for="customAmount">Nominal Custom</label>
                <input type="number" id="customAmount" placeholder="Masukkan nominal pulsa">
            </div>
            
            <button class="save-button" id="confirmPulsaButton">Simpan</button>
        </div>
    </div>

    <div class="debt-edit-modal" id="debtEditModal">
        <div class="debt-edit-content">
            <h2>Edit Hutang</h2>
            <div class="form-group">
                <label for="editDebtName">Nama</label>
                <input type="text" id="editDebtName" placeholder="Nama orang yang berhutang">
            </div>
            <div class="form-group">
                <label for="editDebtPhone">Nomor Telepon</label>
                <input type="text" id="editDebtPhone" placeholder="Nomor telepon">
            </div>
            <div class="form-group">
                <label for="editDebtItem">Nama Barang</label>
                <input type="text" id="editDebtItem" placeholder="Nama barang yang dihutang">
            </div>
            <div class="form-group">
                <label for="editDebtAmount">Jumlah Hutang</label>
                <input type="number" id="editDebtAmount" placeholder="Jumlah hutang">
            </div>
            <button class="save-button" id="saveEditDebtButton">Simpan Perubahan</button>
            <button class="close-btn" id="closeEditDebtModal"><i class="fas fa-times"></i></button>
        </div>
    </div>

    <div class="inventory-edit-modal" id="inventoryEditModal">
        <div class="inventory-edit-content">
            <h2>Edit Barang</h2>
            <div class="form-group">
                <label for="editItemName">Nama Barang</label>
                <input type="text" id="editItemName" placeholder="Nama barang">
            </div>
            <div class="form-group">
                <label for="editItemPrice">Harga Jual</label>
                <input type="number" id="editItemPrice" placeholder="Harga Jual per item">
            </div>
            <div class="form-group">
                <label for="editItemCategory">Kategori</label>
                <select id="editItemCategory">
                    <option value="makanan">Makanan</option>
                    <option value="minuman">Minuman</option>
                    <option value="snack">Snack</option>
                    <option value="sembako">Sembako</option>
                    <option value="lainnya">Lainnya</option>
                </select>
            </div>
            <div class="form-group">
                <label for="editItemStock">Stok Saat Ini</label>
                <input type="number" id="editItemStock" placeholder="Jumlah stok">
            </div>
            <div class="form-group">
                <label for="editItemMinStock">Stok Minimum</label>
                <input type="number" id="editItemMinStock" placeholder="Batas stok minimum">
            </div>
            <button class="save-button" id="saveEditItemButton">Simpan Perubahan</button>
            <button class="close-btn" id="closeEditItemModal"><i class="fas fa-times"></i></button>
        </div>
    </div>

    <script>
        // Inisialisasi state aplikasi
        const state = {
            currentAmount: '0',
            currentPhone: '0',
            savedQrisImage: localStorage.getItem('qrisImage') || '',
            savedMerchant: localStorage.getItem('merchantName') || 'Merchant',
            savedHeaderTitle: localStorage.getItem('headerTitle') || 'QRISku',
            savedHeaderSubtitle: localStorage.getItem('headerSubtitle') || 'Pembayaran QRIS Mudah dan Cepat',
            savedFooterText: localStorage.getItem('footerText') || '© 2025 QRISku - Pembayaran QRIS',
            savedQrisStaticCode: localStorage.getItem('qrisStaticCode') || '',
            currentQrUrl: '',
            currentTransactionIdForConfirmation: null,
            phoneHistoryData: JSON.parse(localStorage.getItem('phoneHistory')) || [],
            transactions: JSON.parse(localStorage.getItem('transactions')) || [],
            selectedPulsaAmount: 0,
            selectedPhoneNumber: '',
            debts: JSON.parse(localStorage.getItem('debts')) || [],
            appPassword: localStorage.getItem('appPassword') || '131313',
            currentDebtId: null,
            passwordContext: null,
            inventory: JSON.parse(localStorage.getItem('inventory')) || [],
            currentItemId: null,
            // State kalkulator
            calculatorCurrentInput: '0',
            calculatorPreviousInput: '',
            calculatorOperator: '',
            calculatorShouldResetDisplay: false,
            calculatorHistory: JSON.parse(localStorage.getItem('calculatorHistory')) || []
        };

        // Helper functions
        const helpers = {
            formatNumber: (num) => {
                return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
            },
            
            updateAmountDisplay: () => {
                document.getElementById('amountDisplay').textContent = helpers.formatNumber(state.currentAmount);
            },
            
            updatePhoneDisplay: () => {
                document.getElementById('phoneDisplay').textContent = state.currentPhone;
            },
            
            updateCalculatorDisplay: () => {
                document.getElementById('calculatorDisplay').textContent = state.calculatorCurrentInput;
            },
            
            toggleFullscreen: () => {
                if (!document.fullscreenElement) {
                    document.documentElement.requestFullscreen().catch(err => {
                        console.log(`Error attempting to enable fullscreen: ${err.message}`);
                    });
                } else {
                    if (document.exitFullscreen) {
                        document.exitFullscreen();
                    }
                }
            },
            
            updateTime: () => {
                const now = new Date();
                const hours = String(now.getHours()).padStart(2, '0');
                const minutes = String(now.getMinutes()).padStart(2, '0');
                document.getElementById('paymentTime').textContent = `${hours}:${minutes}`;
            },
            
            generateTransactionId: () => {
                const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
                let result = 'TRX-';
                for (let i = 0; i < 6; i++) {
                    result += chars.charAt(Math.floor(Math.random() * chars.length));
                }
                return result;
            },
            
            getCategoryName: (category) => {
                const categories = {
                    'makanan': 'Makanan', 'minuman': 'Minuman', 'snack': 'Snack',
                    'sembako': 'Sembako', 'lainnya': 'Lainnya'
                };
                return categories[category] || 'Lainnya';
            }
        };

        // Fungsi kalkulator
        const calculator = {
            input: (value) => {
                if (state.calculatorShouldResetDisplay) {
                    state.calculatorCurrentInput = '';
                    state.calculatorShouldResetDisplay = false;
                }
                
                if (value === '.' && state.calculatorCurrentInput.includes('.')) {
                    return;
                }
                
                if (state.calculatorCurrentInput === '0' && value !== '.') {
                    state.calculatorCurrentInput = value;
                } else {
                    state.calculatorCurrentInput += value;
                }
                
                helpers.updateCalculatorDisplay();
            },
            
            setOperator: (operator) => {
                if (state.calculatorOperator && !state.calculatorShouldResetDisplay) {
                    calculator.calculate();
                }
                
                state.calculatorPreviousInput = state.calculatorCurrentInput;
                state.calculatorOperator = operator;
                state.calculatorShouldResetDisplay = true;
            },
            
            calculate: () => {
                let result;
                const prev = parseFloat(state.calculatorPreviousInput);
                const current = parseFloat(state.calculatorCurrentInput);
                
                if (isNaN(prev) || isNaN(current)) return;
                
                switch (state.calculatorOperator) {
                    case '+':
                        result = prev + current;
                        break;
                    case '-':
                        result = prev - current;
                        break;
                    case '*':
                        result = prev * current;
                        break;
                    case '/':
                        if (current === 0) {
                            alert('Tidak dapat membagi dengan nol');
                            return;
                        }
                        result = prev / current;
                        break;
                    case '%':
                        result = prev % current;
                        break;
                    default:
                        return;
                }
                
                const calculation = {
                    expression: `${state.calculatorPreviousInput} ${state.calculatorOperator} ${state.calculatorCurrentInput}`,
                    result: result.toString()
                };
                
                state.calculatorHistory.unshift(calculation);
                if (state.calculatorHistory.length > 10) {
                    state.calculatorHistory.pop();
                }
                
                localStorage.setItem('calculatorHistory', JSON.stringify(state.calculatorHistory));
                calculator.renderHistory();
                
                state.calculatorCurrentInput = result.toString();
                state.calculatorOperator = '';
                state.calculatorShouldResetDisplay = true;
                helpers.updateCalculatorDisplay();
            },
            
            clear: () => {
                state.calculatorCurrentInput = '0';
                state.calculatorPreviousInput = '';
                state.calculatorOperator = '';
                state.calculatorShouldResetDisplay = false;
                helpers.updateCalculatorDisplay();
            },
            
            clearEntry: () => {
                state.calculatorCurrentInput = '0';
                helpers.updateCalculatorDisplay();
            },
            
            backspace: () => {
                if (state.calculatorCurrentInput.length > 1) {
                    state.calculatorCurrentInput = state.calculatorCurrentInput.slice(0, -1);
                } else {
                    state.calculatorCurrentInput = '0';
                }
                helpers.updateCalculatorDisplay();
            },
            
            renderHistory: () => {
                const calculatorHistoryList = document.getElementById('calculatorHistoryList');
                calculatorHistoryList.innerHTML = '';
                
                if (state.calculatorHistory.length === 0) {
                    calculatorHistoryList.innerHTML = '<div style="text-align: center; color: #64748b; padding: 10px;">Belum ada riwayat perhitungan</div>';
                    return;
                }
                
                state.calculatorHistory.forEach(item => {
                    const historyItem = document.createElement('div');
                    historyItem.className = 'calculator-history-item';
                    historyItem.innerHTML = `
                        <div class="calculator-history-expression">${item.expression} =</div>
                        <div class="calculator-history-result">${item.result}</div>
                    `;
                    
                    historyItem.addEventListener('click', () => {
                        state.calculatorCurrentInput = item.result;
                        helpers.updateCalculatorDisplay();
                    });
                    
                    calculatorHistoryList.appendChild(historyItem);
                });
            }
        };

        // Fungsi render data
        const render = {
            phoneHistory: () => {
                const phoneHistory = document.getElementById('phoneHistory');
                phoneHistory.innerHTML = '<h3>Riwayat Nomor</h3>';
                
                if (state.phoneHistoryData.length === 0) {
                    phoneHistory.innerHTML += '<p style="text-align: center; color: #64748b; padding: 10px;">Belum ada riwayat nomor handphone</p>';
                    return;
                }
                
                state.phoneHistoryData.forEach(item => {
                    const historyItem = document.createElement('div');
                    historyItem.className = 'history-item';
                    historyItem.dataset.phone = item.phone;
                    
                    historyItem.innerHTML = `
                        <div class="history-phone">${item.phone}</div>
                        <div class="history-name">${item.name || 'Tidak ada nama'}</div>
                    `;
                    
                    historyItem.addEventListener('click', () => {
                        state.currentPhone = item.phone;
                        helpers.updatePhoneDisplay();
                    });
                    
                    phoneHistory.appendChild(historyItem);
                });
            },
            
            transactions: () => {
                const transactionList = document.getElementById('transactionList');
                transactionList.innerHTML = '';
                
                const confirmedTransactions = state.transactions.filter(t => t.status === 'success').sort((a, b) => new Date(b.rawDate) - new Date(a.rawDate));
                
                if (confirmedTransactions.length === 0) {
                    transactionList.innerHTML = '<p style="text-align: center; color: #64748b; padding: 20px;">Belum ada transaksi</p>';
                    return;
                }
                
                confirmedTransactions.forEach(transaction => {
                    const transactionItem = document.createElement('div');
                    transactionItem.className = 'transaction-item';
                    
                    transactionItem.innerHTML = `
                        <div class="transaction-header">
                            <div class="transaction-id">${transaction.id}</div>
                            <div class="transaction-date">${transaction.date}</div>
                        </div>
                        <div class="transaction-details">
                            <div class="transaction-amount">Rp ${helpers.formatNumber(transaction.amount)}</div>
                            <div class="transaction-status status-success">Berhasil</div>
                        </div>
                    `;
                    
                    transactionList.appendChild(transactionItem);
                });
            },
            
            debts: () => {
                const debtList = document.getElementById('debtList');
                debtList.innerHTML = '<h3>Daftar Hutang</h3>';
                
                if (state.debts.length === 0) {
                    debtList.innerHTML += '<p style="text-align: center; color: #64748b; padding: 20px;">Belum ada catatan hutang</p>';
                    return;
                }
                
                state.debts.forEach(debt => {
                    const debtItem = document.createElement('div');
                    debtItem.className = 'debt-item';
                    debtItem.dataset.id = debt.id;
                    
                    debtItem.innerHTML = `
                        <div class="debt-header">
                            <div>
                                <div class="debt-name">${debt.name}</div>
                                <div class="debt-phone">${debt.phone}</div>
                            </div>
                        </div>
                        <div class="debt-details">
                            <div>
                                <div class="debt-item-name">${debt.item}</div>
                                <div class="debt-amount">Rp ${helpers.formatNumber(debt.amount)}</div>
                            </div>
                            <div class="debt-status status-${debt.status}" data-id="${debt.id}">${debt.status === 'paid' ? 'Lunas' : 'Belum Lunas'}</div>
                        </div>
                    `;
                    
                    const statusElement = debtItem.querySelector('.debt-status');
                    statusElement.addEventListener('click', (e) => {
                        e.stopPropagation();
                        const debtId = e.target.getAttribute('data-id');
                        const debt = state.debts.find(d => d.id === debtId);
                        
                        if (debt.status === 'pending') {
                            modals.showPasswordPrompt('status', debtId);
                        }
                    });
                    
                    debtList.appendChild(debtItem);
                });
            },
            
            inventory: () => {
                const inventoryList = document.getElementById('inventoryList');
                inventoryList.innerHTML = '<h3>Daftar Inventori</h3>';
                
                if (state.inventory.length === 0) {
                    inventoryList.innerHTML += '<p style="text-align: center; color: #64748b; padding: 20px;">Belum ada data inventori</p>';
                    return;
                }
                
                state.inventory.forEach(item => {
                    const inventoryItem = document.createElement('div');
                    inventoryItem.className = 'inventory-item';
                    inventoryItem.dataset.id = item.id;
                    
                    const stockStatus = item.stock <= item.minStock ? 'stock-warning' : 'stock-adequate';
                    const stockText = item.stock <= item.minStock ? 'Stok Rendah!' : 'Stok Cukup';
                    
                    inventoryItem.innerHTML = `
                        <div class="inventory-header">
                            <div>
                                <div class="inventory-name">${item.name}</div>
                                <div class="inventory-category">${helpers.getCategoryName(item.category)}</div>
                            </div>
                            <div class="inventory-price">Rp ${helpers.formatNumber(item.price || 0)}</div>
                        </div>
                        <div class="inventory-details">
                            <div>
                                <div class="inventory-stock">Stok: ${item.stock}</div>
                                <div class="${stockStatus}">${stockText}</div>
                            </div>
                            <div class="inventory-action">
                                <button class="inventory-btn-small decrease-stock" data-id="${item.id}">-</button>
                                <button class="inventory-btn-small increase-stock" data-id="${item.id}">+</button>
                                <button class="inventory-btn-small sell sell-item" data-id="${item.id}">Jual</button>
                            </div>
                        </div>
                    `;
                    
                    // Event listeners untuk tombol stok
                    inventoryItem.querySelector('.decrease-stock').addEventListener('click', (e) => {
                        e.stopPropagation();
                        const itemId = e.target.getAttribute('data-id');
                        const itemIndex = state.inventory.findIndex(i => i.id === itemId);
                        
                        if (itemIndex >= 0 && state.inventory[itemIndex].stock > 0) {
                            state.inventory[itemIndex].stock--;
                            localStorage.setItem('inventory', JSON.stringify(state.inventory));
                            render.inventory();
                            render.shoppingList();
                        }
                    });
                    
                    inventoryItem.querySelector('.increase-stock').addEventListener('click', (e) => {
                        e.stopPropagation();
                        const itemId = e.target.getAttribute('data-id');
                        const itemIndex = state.inventory.findIndex(i => i.id === itemId);
                        
                        if (itemIndex >= 0) {
                            state.inventory[itemIndex].stock++;
                            localStorage.setItem('inventory', JSON.stringify(state.inventory));
                            render.inventory();
                            render.shoppingList();
                        }
                    });
                    
                    // Tombol jual
                    inventoryItem.querySelector('.sell-item').addEventListener('click', (e) => {
                        e.stopPropagation();
                        const itemId = e.target.getAttribute('data-id');
                        const itemIndex = state.inventory.findIndex(i => i.id === itemId);
                        const item = state.inventory[itemIndex];

                        const quantity = prompt(`Berapa banyak "${item.name}" yang ingin dijual?\nStok tersedia: ${item.stock}`, "1");

                        if (quantity === null || quantity.trim() === '') return;

                        const qty = parseInt(quantity);
                        if (isNaN(qty) || qty <= 0) {
                            alert("Jumlah tidak valid.");
                            return;
                        }
                        if (qty > item.stock) {
                            alert(`Stok tidak mencukupi. Stok tersisa hanya ${item.stock}.`);
                            return;
                        }

                        // Hitung total dan update stok
                        const total = qty * item.price;
                        state.inventory[itemIndex].stock -= qty;
                        localStorage.setItem('inventory', JSON.stringify(state.inventory));

                        // Pindah ke halaman beranda dan set amount
                        state.currentAmount = String(total);
                        helpers.updateAmountDisplay();
                        navigation.switchPage('homePage');
                        
                        // Render ulang inventori di background
                        render.inventory();
                        render.shoppingList();
                    });
                    
                    inventoryList.appendChild(inventoryItem);
                });
            },
            
            shoppingList: () => {
                const shoppingList = document.getElementById('shoppingList');
                shoppingList.innerHTML = '';
                
                const lowStockItems = state.inventory.filter(item => item.stock <= item.minStock);
                
                if (lowStockItems.length === 0) {
                    shoppingList.innerHTML = '<h3 style="margin-top:20px;">Daftar Belanja</h3><p style="text-align: center; color: #64748b; padding: 20px;">Tidak ada barang yang perlu dibeli</p>';
                    return;
                }
                
                shoppingList.innerHTML = '<h3 style="margin-top:20px;">Daftar Belanja (Stok Rendah)</h3>';
                
                lowStockItems.forEach(item => {
                    const shoppingItem = document.createElement('div');
                    shoppingItem.className = 'inventory-item';
                    
                    const needed = Math.max(0, item.minStock - item.stock + 1);

                    shoppingItem.innerHTML = `
                        <div class="inventory-header">
                            <div>
                                <div class="inventory-name">${item.name}</div>
                                <div class="inventory-category">${helpers.getCategoryName(item.category)}</div>
                            </div>
                        </div>
                        <div class="inventory-details">
                            <div>
                                <div class="stock-warning">Stok: ${item.stock} (Min: ${item.minStock})</div>
                                <div>Perlu dibeli: setidaknya ${needed}</div>
                            </div>
                        </div>
                    `;
                    
                    shoppingList.appendChild(shoppingItem);
                });
            },
            
            qrCodeDisplay: () => {
                const qrcodeDisplay = document.getElementById('qrcodeDisplay');
                qrcodeDisplay.innerHTML = '';
                
                if (state.savedQrisImage) {
                    qrcodeDisplay.innerHTML = `
                        <h3>${state.savedMerchant}</h3>
                        <p> </p>
                        <img src="${state.savedQrisImage}" alt="QRIS Code" style="max-width: 100%; height: auto; border: 1px solid #e2e8f0; border-radius: 8px;">
                        <p>Scan Kode QRIS di atas untuk pembayaran</p>
                    `;
                } else {
                    qrcodeDisplay.innerHTML = `
                        <h3>${state.savedMerchant}</h3>
                        <p> </p>
                        <div style="background-color: #f1f5f9; padding: 20px; display: inline-block; margin: 15px 0;">
                            <i class="fas fa-qrcode" style="font-size: 120px; color: #1e293b;"></i>
                        </div>
                        <p>Masukkan kode QRIS statis di pengaturan</p>
                    `;
                }
            }
        };

        // Fungsi navigasi
        const navigation = {
            switchPage: (pageId) => {
                document.querySelectorAll('.nav-btn').forEach(btn => {
                    if(btn.getAttribute('data-page') === pageId) {
                        btn.classList.add('active');
                    } else {
                        btn.classList.remove('active');
                    }
                });
                
                document.querySelectorAll('.page').forEach(page => {
                    if(page.id === pageId) {
                        page.classList.add('active');
                    } else {
                        page.classList.remove('active');
                    }
                });

                if (pageId === 'financePage') {
                    render.transactions();
                    render.debts();
                } else if (pageId === 'qrcodePage') {
                    render.qrCodeDisplay();
                } else if (pageId === 'inventoryPage') {
                    render.inventory();
                    render.shoppingList();
                } else if (pageId === 'calculatorPage') {
                    calculator.renderHistory();
                }
            }
        };

        // Fungsi modals
        const modals = {
            showPasswordPrompt: (context, debtId) => {
                state.passwordContext = context;
                state.currentDebtId = debtId;
                document.getElementById('passwordModal').style.display = 'flex';
                document.getElementById('passwordInput').value = '';
                document.getElementById('passwordError').style.display = 'none';
                document.getElementById('passwordInput').focus();
            },
            
            closeAllModals: () => {
                document.querySelectorAll('.settings-modal, .payment-modal, .pulsa-modal, .password-modal, .debt-edit-modal, .inventory-edit-modal').forEach(modal => {
                    modal.style.display = 'none';
                });
            }
        };

        // Inisialisasi aplikasi
        const initializeApp = () => {
            // Set nilai default
            document.getElementById('merchantName').value = state.savedMerchant;
            document.getElementById('headerTitleInput').value = state.savedHeaderTitle;
            document.getElementById('headerSubtitleInput').value = state.savedHeaderSubtitle;
            document.getElementById('footerTextInput').value = state.savedFooterText;
            document.getElementById('qrisStaticCode').value = state.savedQrisStaticCode;
            
            document.getElementById('paymentMerchant').textContent = state.savedMerchant;
            document.getElementById('headerTitle').textContent = state.savedHeaderTitle;
            document.getElementById('headerSubtitle').textContent = state.savedHeaderSubtitle;
            document.getElementById('footerText').textContent = state.savedFooterText;
            document.getElementById('paymentTitle').textContent = state.savedHeaderTitle;
            
            // Render data awal
            render.phoneHistory();
            render.transactions();
            render.qrCodeDisplay();
            render.debts();
            render.inventory();
            render.shoppingList();
            calculator.renderHistory();
            
            // Update waktu
            helpers.updateTime();
            
            // Event listener untuk fullscreen
            document.addEventListener('fullscreenchange', () => {
                const iconClass = document.fullscreenElement ? 'fa-compress' : 'fa-expand';
                document.querySelectorAll('.fullscreen-btn').forEach(btn => {
                    const icon = btn.querySelector('i');
                    icon.className = `fas ${iconClass}`;
                });
            });
        };

        // Event listeners
        document.addEventListener('DOMContentLoaded', () => {
            // Inisialisasi aplikasi
            initializeApp();
            
            // Navigation
            document.querySelectorAll('.nav-btn').forEach(navBtn => {
                navBtn.addEventListener('click', () => {
                    const pageId = navBtn.getAttribute('data-page');
                    navigation.switchPage(pageId);
                });
            });
            
            // Tombol inventori
            document.getElementById('inventoryButton').addEventListener('click', () => {
                navigation.switchPage('inventoryPage');
            });
            
            // Tombol fullscreen
            document.querySelectorAll('.fullscreen-btn').forEach(btn => {
                btn.addEventListener('click', helpers.toggleFullscreen);
            });
            
            // Tombol profile (buka modal password)
            document.querySelectorAll('.profile-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    modals.showPasswordPrompt('settings');
                });
            });
            
            // Modal password
            document.getElementById('closePassword').addEventListener('click', () => {
                document.getElementById('passwordModal').style.display = 'none';
            });
            
            document.getElementById('submitPassword').addEventListener('click', () => {
                if (document.getElementById('passwordInput').value === state.appPassword) {
                    document.getElementById('passwordModal').style.display = 'none';
                    
                    if (state.passwordContext === 'status') {
                        const debtIndex = state.debts.findIndex(d => d.id === state.currentDebtId);
                        if (debtIndex >= 0) {
                            state.debts[debtIndex].status = 'paid';
                            localStorage.setItem('debts', JSON.stringify(state.debts));
                            render.debts();
                        }
                    } else {
                        document.getElementById('settingsModal').style.display = 'flex';
                    }
                } else {
                    document.getElementById('passwordError').style.display = 'block';
                    document.getElementById('passwordInput').value = '';
                    setTimeout(() => {
                        document.getElementById('passwordError').style.display = 'none';
                    }, 2000);
                }
            });
            
            // Keypad beranda
            document.querySelectorAll('#homePage .key[data-value]').forEach(key => {
                key.addEventListener('click', () => {
                    const value = key.getAttribute('data-value');
                    state.currentAmount = (state.currentAmount === '0') ? value : state.currentAmount + value;
                    helpers.updateAmountDisplay();
                });
            });
            
            // Keypad pulsa
            document.querySelectorAll('#pulsaPage .key[data-value]').forEach(key => {
                key.addEventListener('click', () => {
                    const value = key.getAttribute('data-value');
                    state.currentPhone = (state.currentPhone === '0') ? value : state.currentPhone + value;
                    helpers.updatePhoneDisplay();
                });
            });
            
            // Keypad kalkulator
            document.querySelectorAll('#calculatorPage .key[data-value]').forEach(key => {
                key.addEventListener('click', () => {
                    const value = key.getAttribute('data-value');
                    calculator.input(value);
                });
            });
            
            document.querySelectorAll('#calculatorPage .key[data-operator]').forEach(key => {
                key.addEventListener('click', () => {
                    const operator = key.getAttribute('data-operator');
                    calculator.setOperator(operator);
                });
            });
            
            // Tombol kalkulator khusus
            document.getElementById('calculatorClear').addEventListener('click', calculator.clear);
            document.getElementById('calculatorClearEntry').addEventListener('click', calculator.clearEntry);
            document.getElementById('calculatorBackspace').addEventListener('click', calculator.backspace);
            document.getElementById('calculatorEquals').addEventListener('click', calculator.calculate);
            
            // Tombol backspace
            document.getElementById('backspaceBtn').addEventListener('click', () => {
                state.currentAmount = (state.currentAmount.length > 1) ? state.currentAmount.slice(0, -1) : '0';
                helpers.updateAmountDisplay();
            });
            
            document.getElementById('pulsaBackspaceBtn').addEventListener('click', () => {
                state.currentPhone = (state.currentPhone.length > 1) ? state.currentPhone.slice(0, -1) : '0';
                helpers.updatePhoneDisplay();
            });
            
            // Tombol clear
            document.getElementById('clearButton').addEventListener('click', () => {
                state.currentAmount = '0';
                helpers.updateAmountDisplay();
            });
            
            document.getElementById('clearPhoneButton').addEventListener('click', () => {
                state.currentPhone = '0';
                helpers.updatePhoneDisplay();
            });
            
            // Tombol konversi (generate QR)
            document.getElementById('convertButton').addEventListener('click', async () => {
                if (state.currentAmount === '0') {
                    alert('Harap masukkan nominal terlebih dahulu!');
                    return;
                }
                
                document.getElementById('loading').style.display = 'block';
                
                try {
                    const qrisData = state.savedQrisStaticCode;
                    if (!qrisData) {
                        alert('Harap masukkan Kode QRIS statis di pengaturan dahulu!');
                        document.getElementById('loading').style.display = 'none';
                        return;
                    }
                    
                    const encodedQris = encodeURIComponent(qrisData);
                    const response = await fetch(`https://cekid-ariepulsa.my.id/api/?qris_data=${encodedQris}&nominal=${state.currentAmount}`);
                    const data = await response.json();
                    
                    if (data.status === 'success') {
                        document.getElementById('paymentAmount').textContent = `Rp ${helpers.formatNumber(state.currentAmount)}`;
                        document.getElementById('paymentQr').src = data.link_qris;
                        document.getElementById('paymentMerchant').textContent = state.savedMerchant;
                        state.currentQrUrl = data.link_qris;
                        
                        helpers.updateTime();
                        const newId = helpers.generateTransactionId();
                        document.getElementById('transactionId').textContent = newId;
                        state.currentTransactionIdForConfirmation = newId;

                        const now = new Date();
                        const newTransaction = {
                            id: newId,
                            amount: parseInt(state.currentAmount),
                            date: now.toLocaleDateString('id-ID', { day: '2-digit', month: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit' }),
                            rawDate: now.toISOString(),
                            status: 'pending',
                            type: 'qris'
                        };
                        
                        state.transactions.push(newTransaction);
                        localStorage.setItem('transactions', JSON.stringify(state.transactions));
                        document.getElementById('paymentModal').style.display = 'flex';
                    } else {
                        alert('API mengembalikan status error: ' + (data.message || 'Unknown error'));
                    }
                } catch (error) {
                    alert('Terjadi kesalahan: ' + error.message);
                } finally {
                    document.getElementById('loading').style.display = 'none';
                }
            });
            
            // Konfirmasi pembayaran
            document.getElementById('confirmPayment').addEventListener('click', () => {
                if (state.currentTransactionIdForConfirmation) {
                    const transactionIndex = state.transactions.findIndex(t => t.id === state.currentTransactionIdForConfirmation);
                    if (transactionIndex >= 0) {
                        state.transactions[transactionIndex].status = 'success';
                        localStorage.setItem('transactions', JSON.stringify(state.transactions));
                        alert(`Transaksi ${state.currentTransactionIdForConfirmation} dikonfirmasi berhasil!`);
                        
                        document.getElementById('paymentModal').style.display = 'none';
                        state.currentTransactionIdForConfirmation = null;

                        if (document.getElementById('financePage').classList.contains('active')) {
                            render.transactions();
                        }
                    }
                }
            });
            
            // Tutup modal dengan klik di luar
            document.querySelectorAll('.settings-modal, .payment-modal, .pulsa-modal, .password-modal, .debt-edit-modal, .inventory-edit-modal').forEach(modal => {
                modal.addEventListener('click', (e) => {
                    if (e.target === modal) modal.style.display = 'none';
                });
            });
            
            // Tutup modal pembayaran dengan tombol tutup
            document.getElementById('closePaymentModal').addEventListener('click', () => {
                document.getElementById('paymentModal').style.display = 'none';
            });
            
            document.getElementById('closePayment').addEventListener('click', () => {
                document.getElementById('paymentModal').style.display = 'none';
            });
        });
    </script>
</body>
</html>
