<html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, maximum-scale=1.0">
    <title>QRISku - Pembayaran QRIS</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
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
        }

        .container {
            max-width: 480px;
            margin: 0 auto;
            height: 100vh;
            display: flex;
            flex-direction: column;
            padding: 16px;
            padding-bottom: 72px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
            background-color: #ffffff;
            border-radius: 16px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }

        /* Header - Satu Warna */
        header {
            text-align: center;
            margin-bottom: 16px;
            padding: 12px 16px;
            background: #3b82f6; /* Warna solid */
            color: #ffffff;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
            flex-shrink: 0;
            position: relative;
        }

        header h1 {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 4px;
        }

        header p {
            font-size: 0.875rem;
            opacity: 0.85;
            font-weight: 400;
        }

        .profile-btn {
            position: absolute;
            top: 12px;
            right: 12px;
            background: rgba(255, 255, 255, 0.15);
            border: none;
            color: #ffffff;
            font-size: 1.25rem;
            cursor: pointer;
            width: 36px;
            height: 36px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: background-color 0.2s ease, transform 0.2s ease;
        }

        .fullscreen-btn {
            position: absolute;
            top: 12px;
            left: 12px;
            background: rgba(255, 255, 255, 0.15);
            border: none;
            color: #ffffff;
            font-size: 1.25rem;
            cursor: pointer;
            width: 36px;
            height: 36px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: background-color 0.2s ease, transform 0.2s ease;
        }

        .profile-btn:hover, .fullscreen-btn:hover {
            background-color: rgba(255, 255, 255, 0.25);
            transform: scale(1.05);
        }

        /* Input Section */
        .input-section {
            margin-bottom: 16px;
            background-color: #ffffff;
            padding: 16px;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
            border: 1px solid #e2e8f0;
            flex: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
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
            font-size: 1.75rem;
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
        }

        /* Numeric Keypad - Tetap Kecil */
        .numeric-keypad-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .numeric-keypad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 6px; /* Gap lebih kecil */
            flex: 1;
            min-height: 0;
        }

        .calculator-keypad {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 6px; /* Gap lebih kecil */
            flex: 1;
            min-height: 0;
        }

        .key {
            padding: 10px; /* Padding lebih kecil */
            background: linear-gradient(145deg, #ffffff, #f1f5f9);
            color: #1e293b;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            font-size: 1.125rem; /* Font lebih kecil */
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
            min-height: 40px; /* Height lebih kecil */
            display: flex;
            align-items: center;
            justify-content: center;
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

        .action-buttons {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 8px;
            margin-top: 12px;
            flex-shrink: 0;
        }

        .convert-button, .clear-button, .save-button, .confirm-payment {
            padding: 10px;
            border-radius: 8px;
            font-size: 0.9375rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            border: none;
            width: 100%;
        }

        .convert-button, .save-button {
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            color: #ffffff;
        }

        .confirm-payment {
            background: linear-gradient(135deg, #15803d, #22c55e);
            color: #ffffff;
            margin-top: 8px;
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

        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background: #ffffff;
            display: flex;
            justify-content: space-around;
            padding: 8px 0;
            box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.05);
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
            padding: 8px;
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

        /* Inventory Button (FAB) */
        .inventory-btn {
            position: fixed;
            bottom: 80px;
            right: 16px;
            width: 48px;
            height: 48px;
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

        /* Modal */
        .settings-modal, .payment-modal, .pulsa-modal, .password-modal, .debt-edit-modal, .inventory-edit-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.4);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            padding: 16px;
            animation: fadeIn 0.3s ease;
        }

        .settings-content, .payment-content, .pulsa-content, .password-content, .debt-edit-content, .inventory-edit-content {
            background: #ffffff;
            padding: 20px;
            border-radius: 12px;
            width: 100%;
            max-width: 400px;
            max-height: 90vh;
            overflow-y: auto;
            position: relative;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            animation: slideUp 0.3s ease;
        }

        .payment-content {
            max-width: 360px;
        }

        .payment-qr img {
            width: 180px;
            height: 180px;
            max-width: 100%;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
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

        /* Form Elements */
        .form-group {
            margin-bottom: 12px;
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
            padding: 10px;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            font-size: 0.9375rem;
            color: #1e293b;
            background-color: #f8fafc;
            transition: border-color 0.2s ease, box-shadow 0.2s ease;
        }

        .form-group input:focus, .form-group textarea:focus, .form-group select:focus {
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
            outline: none;
        }

        /* Transaction, Debt, Inventory Items */
        .transaction-item, .debt-item, .inventory-item, .calculator-history-item {
            background: #ffffff;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 8px;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
            border: 1px solid #e2e8f0;
        }

        .transaction-amount, .debt-amount, .inventory-stock {
            font-size: 1rem;
            font-weight: 500;
        }

        /* Management Buttons */
        .management-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            margin-bottom: 12px;
        }

        .management-buttons button, .management-buttons label {
            padding: 10px;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
            background: #ffffff;
            color: #1e293b;
            font-weight: 500;
            font-size: 0.875rem;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: center;
        }

        .management-buttons button:hover, .management-buttons label:hover {
            background: #f1f5f9;
            transform: translateY(-1px);
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

        /* Page Styling */
        .page {
            display: none;
            animation: fadeIn 0.3s ease;
            height: 100%;
            flex-direction: column;
        }

        .page.active {
            display: flex;
        }

        /* QR Code Display */
        .qrcode-display img {
            max-width: 100%;
            height: auto;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
        }

        /* History Items */
        .history-item {
            padding: 12px;
            font-size: 0.875rem;
            background: #ffffff;
            border-radius: 8px;
            margin-bottom: 8px;
            border: 1px solid #e2e8f0;
        }

        /* Text Overflow Handling */
        .transaction-id, .debt-name, .inventory-name {
            word-break: break-word;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        /* Form Styling */
        .debt-form, .inventory-form {
            padding: 12px;
        }

        footer {
            text-align: center;
            margin-top: 24px;
            padding: 12px;
            color: #64748b;
            font-size: 0.75rem;
            flex-shrink: 0;
        }

        /* Finance Tabs */
        .finance-tabs {
            display: flex;
            margin-bottom: 16px;
            border-bottom: 1px solid #e2e8f0;
        }

        .finance-tab {
            flex: 1;
            padding: 10px;
            text-align: center;
            background: none;
            border: none;
            border-bottom: 3px solid transparent;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            color: #64748b;
        }

        .finance-tab.active {
            color: #3b82f6;
            border-bottom-color: #3b82f6;
        }

        .tab-content {
            display: none;
            flex: 1;
            overflow-y: auto;
        }

        .tab-content.active {
            display: block;
        }

        /* Calculator History */
        .calculator-history {
            margin-top: 16px;
            max-height: 200px;
            overflow-y: auto;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            padding: 8px;
            background-color: #f8fafc;
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

        /* Media Queries */
        @media (max-width: 360px) {
            .container {
                padding: 12px;
                padding-bottom: 64px;
            }
            header {
                padding: 10px;
            }
            header h1 {
                font-size: 1.25rem;
            }
            .amount-display, .phone-display, .calculator-display {
                font-size: 1.5rem;
                min-height: 56px;
                padding: 10px;
            }
            .key {
                padding: 8px;
                font-size: 1rem;
                min-height: 36px;
            }
            .numeric-keypad, .calculator-keypad {
                gap: 4px;
            }
            .input-section {
                padding: 12px;
            }
            .input-section h3 {
                font-size: 0.9375rem;
            }
            .nav-btn {
                font-size: 0.6875rem;
            }
            .nav-btn i {
                font-size: 1.125rem;
            }
            .inventory-btn {
                bottom: 72px;
                right: 12px;
                width: 44px;
                height: 44px;
                font-size: 1.125rem;
            }
        }

        @media (min-width: 768px) {
            .container {
                max-width: 640px;
                padding: 24px;
            }
            .numeric-keypad, .calculator-keypad {
                gap: 8px;
            }
            .key {
                font-size: 1.25rem;
                min-height: 48px;
            }
            .input-section {
                padding: 20px;
            }
        }

        @media (min-width: 1024px) {
            .container {
                max-width: 800px;
                padding: 32px;
            }
            .numeric-keypad, .calculator-keypad {
                gap: 10px;
            }
            .key {
                font-size: 1.375rem;
                min-height: 56px;
            }
            .input-section h3 {
                font-size: 1.125rem;
            }
        }

        /* Landscape Orientation */
        @media (max-width: 768px) and (orientation: landscape) {
            .container {
                padding-bottom: 64px;
            }
            .bottom-nav {
                padding: 6px 0;
            }
            .nav-btn {
                padding: 6px;
            }
            .nav-btn i {
                margin-bottom: 2px;
            }
        }

        /* Prevent hover effects on touch devices */
        @media (hover: none) {
            .key:hover, .convert-button:hover, .clear-button:hover, 
            .save-button:hover, .confirm-payment:hover, .profile-btn:hover,
            .nav-btn:hover, .inventory-btn:hover, .fullscreen-btn:hover {
                transform: none;
                background: initial;
            }
            .key:active, .convert-button:active, .clear-button:active, 
            .save-button:active, .confirm-payment:active, .profile-btn:active,
            .nav-btn:active, .inventory-btn:active, .fullscreen-btn:active {
                transform: scale(0.98);
                opacity: 0.9;
            }
        }

        /* Scrollbar Styling */
        ::-webkit-scrollbar {
            width: 6px;
        }

        ::-webkit-scrollbar-track {
            background: #f1f5f9;
            border-radius: 10px;
        }

        ::-webkit-scrollbar-thumb {
            background: #94a3b8;
            border-radius: 10px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: #64748b;
        }

        /* Fullscreen styles */
        body:fullscreen {
            background-color: #f8fafc;
        }
        
        body:fullscreen .container {
            max-width: 100%;
            border-radius: 0;
            box-shadow: none;
        }
    </style>
</head>
<body>
    <div class="container">
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

        <!-- Calculator Page -->
        <div class="page" id="calculatorPage">
            <header>
                <button class="fullscreen-btn"><i class="fas fa-expand"></i></button>
                <h1>Kalkulator</h1>
                <p>Kalkulator lengkap dengan riwayat</p>
                <button class="profile-btn" id="calculatorProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="input-section">
                <div class="calculator-display" id="calculatorDisplay">0</div>
                
                <div class="numeric-keypad-container">
                    <div class="calculator-keypad">
                        <button class="key clear-key" id="calculatorClear">C</button>
                        <button class="key clear-key" id="calculatorClearEntry">CE</button>
                        <button class="key operator-key" data-operator="backspace"><i class="fas fa-backspace"></i></button>
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
                
                <div class="calculator-history">
                    <div class="calculator-history-title">Riwayat Perhitungan</div>
                    <div id="calculatorHistoryList"></div>
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
                
                <div class="drop-area" id="qrisDropArea">
                    <i class="fas fa-cloud-upload-alt"></i>
                    <p>Drag & drop gambar QRIS di sini atau klik untuk memilih file</p>
                    <input type="file" id="qrisInput" accept="image/*" style="display: none;">
                </div>
                
                <div class="qris-preview" id="qrisPreview" style="display: none;">
                    <p>Preview QRIS:</p>
                    <img id="qrisPreviewImg" src="" alt="QRIS Preview">
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
        // Elements
        const profileBtn = document.getElementById('profileBtn');
        const financeProfileBtn = document.getElementById('financeProfileBtn');
        const qrcodeProfileBtn = document.getElementById('qrcodeProfileBtn');
        const pulsaProfileBtn = document.getElementById('pulsaProfileBtn');
        const inventoryProfileBtn = document.getElementById('inventoryProfileBtn');
        const calculatorProfileBtn = document.getElementById('calculatorProfileBtn');
        const inventoryButton = document.getElementById('inventoryButton');
        const passwordModal = document.getElementById('passwordModal');
        const passwordInput = document.getElementById('passwordInput');
        const passwordError = document.getElementById('passwordError');
        const closePassword = document.getElementById('closePassword');
        const submitPassword = document.getElementById('submitPassword');
        const settingsModal = document.getElementById('settingsModal');
        const closeSettings = document.getElementById('closeSettings');
        const amountDisplay = document.getElementById('amountDisplay');
        const convertButton = document.getElementById('convertButton');
        const clearButton = document.getElementById('clearButton');
        const paymentModal = document.getElementById('paymentModal');
        const paymentMerchant = document.getElementById('paymentMerchant');
        const paymentAmount = document.getElementById('paymentAmount');
        const paymentTitle = document.getElementById('paymentTitle');
        const paymentQr = document.getElementById('paymentQr');
        const closePayment = document.getElementById('closePayment');
        const confirmPayment = document.getElementById('confirmPayment');
        const closePaymentModal = document.getElementById('closePaymentModal');
        const loading = document.getElementById('loading');
        const qrisDropArea = document.getElementById('qrisDropArea');
        const qrisInput = document.getElementById('qrisInput');
        const saveButton = document.getElementById('saveButton');
        const backspaceBtn = document.getElementById('backspaceBtn');
        const merchantName = document.getElementById('merchantName');
        const paymentTime = document.getElementById('paymentTime');
        const transactionId = document.getElementById('transactionId');
        const headerTitle = document.getElementById('headerTitle');
        const headerSubtitle = document.getElementById('headerSubtitle');
        const footerText = document.getElementById('footerText');
        const headerTitleInput = document.getElementById('headerTitleInput');
        const headerSubtitleInput = document.getElementById('headerSubtitleInput');
        const footerTextInput = document.getElementById('footerTextInput');
        const tabBtns = document.querySelectorAll('.tab-btn');
        const tabContents = document.querySelectorAll('.tab-content');
        const navBtns = document.querySelectorAll('.nav-btn');
        const pages = document.querySelectorAll('.page');
        const phoneDisplay = document.getElementById('phoneDisplay');
        const savePhoneButton = document.getElementById('savePhoneButton');
        const clearPhoneButton = document.getElementById('clearPhoneButton');
        const pulsaBackspaceBtn = document.getElementById('pulsaBackspaceBtn');
        const phoneHistory = document.getElementById('phoneHistory');
        const transactionList = document.getElementById('transactionList');
        const qrcodeDisplay = document.getElementById('qrcodeDisplay');
        const pulsaModal = document.getElementById('pulsaModal');
        const closePulsaModal = document.getElementById('closePulsaModal');
        const pulsaOptions = document.querySelectorAll('.pulsa-option');
        const customAmountGroup = document.getElementById('customAmountGroup');
        const customAmount = document.getElementById('customAmount');
        const confirmPulsaButton = document.getElementById('confirmPulsaButton');
        const phoneName = document.getElementById('phoneName');
        const financeTabs = document.querySelectorAll('.finance-tab');
        const financeTabContents = document.querySelectorAll('#financePage .tab-content');
        const debtName = document.getElementById('debtName');
        const debtPhone = document.getElementById('debtPhone');
        const debtItem = document.getElementById('debtItem');
        const debtAmount = document.getElementById('debtAmount');
        const addDebtButton = document.getElementById('addDebtButton');
        const debtList = document.getElementById('debtList');
        const debtNameList = document.getElementById('debtNameList');
        const qrisPreview = document.getElementById('qrisPreview');
        const qrisPreviewImg = document.getElementById('qrisPreviewImg');
        const resetQrisButton = document.getElementById('resetQrisButton');
        const currentPassword = document.getElementById('currentPassword');
        const newPassword = document.getElementById('newPassword');
        const confirmPassword = document.getElementById('confirmPassword');
        const changePasswordButton = document.getElementById('changePasswordButton');
        const debtEditModal = document.getElementById('debtEditModal');
        const editDebtName = document.getElementById('editDebtName');
        const editDebtPhone = document.getElementById('editDebtPhone');
        const editDebtItem = document.getElementById('editDebtItem');
        const editDebtAmount = document.getElementById('editDebtAmount');
        const saveEditDebtButton = document.getElementById('saveEditDebtButton');
        const closeEditDebtModal = document.getElementById('closeEditDebtModal');
        const qrisStaticCode = document.getElementById('qrisStaticCode');
        
        // Inventory elements
        const itemName = document.getElementById('itemName');
        const itemPrice = document.getElementById('itemPrice');
        const itemCategory = document.getElementById('itemCategory');
        const itemStock = document.getElementById('itemStock');
        const itemMinStock = document.getElementById('itemMinStock');
        const addItemButton = document.getElementById('addItemButton');
        const inventoryList = document.getElementById('inventoryList');
        const shoppingList = document.getElementById('shoppingList');
        const inventoryEditModal = document.getElementById('inventoryEditModal');
        const editItemName = document.getElementById('editItemName');
        const editItemPrice = document.getElementById('editItemPrice');
        const editItemCategory = document.getElementById('editItemCategory');
        const editItemStock = document.getElementById('editItemStock');
        const editItemMinStock = document.getElementById('editItemMinStock');
        const saveEditItemButton = document.getElementById('saveEditItemButton');
        const closeEditItemModal = document.getElementById('closeEditItemModal');
        const downloadCsvButton = document.getElementById('downloadCsvButton');
        const backupButton = document.getElementById('backupButton');
        const restoreInput = document.getElementById('restoreInput');
        const importCsvInput = document.getElementById('importCsvInput');
        
        // Fullscreen button
        const fullscreenBtn = document.getElementById('fullscreenBtn');
        const fullscreenBtns = document.querySelectorAll('.fullscreen-btn');
        
        // Calculator elements
        const calculatorDisplay = document.getElementById('calculatorDisplay');
        const calculatorClear = document.getElementById('calculatorClear');
        const calculatorClearEntry = document.getElementById('calculatorClearEntry');
        const calculatorEquals = document.getElementById('calculatorEquals');
        const calculatorHistoryList = document.getElementById('calculatorHistoryList');
        const calculatorKeys = document.querySelectorAll('#calculatorPage .key[data-value], #calculatorPage .key[data-operator]');
        
        // State
        let currentAmount = '0';
        let currentPhone = '0';
        let savedQrisImage = localStorage.getItem('qrisImage') || '';
        let savedMerchant = localStorage.getItem('merchantName') || 'Merchant';
        let savedHeaderTitle = localStorage.getItem('headerTitle') || 'QRISku';
        let savedHeaderSubtitle = localStorage.getItem('headerSubtitle') || 'Pembayaran QRIS Mudah dan Cepat';
        let savedFooterText = localStorage.getItem('footerText') || '© 2025 QRISku - Pembayaran QRIS';
        let savedQrisStaticCode = localStorage.getItem('qrisStaticCode') || '';
        let currentQrUrl = '';
        let currentTransactionIdForConfirmation = null;
        let phoneHistoryData = JSON.parse(localStorage.getItem('phoneHistory')) || [];
        let transactions = JSON.parse(localStorage.getItem('transactions')) || [];
        let selectedPulsaAmount = 0;
        let selectedPhoneNumber = '';
        let debts = JSON.parse(localStorage.getItem('debts')) || [];
        let appPassword = localStorage.getItem('appPassword') || '131313';
        let currentDebtId = null;
        let passwordContext = null;
        let inventory = JSON.parse(localStorage.getItem('inventory')) || [];
        let currentItemId = null;
        
        // Calculator state
        let calculatorCurrentInput = '0';
        let calculatorPreviousInput = '';
        let calculatorOperator = '';
        let calculatorShouldResetDisplay = false;
        let calculatorHistory = JSON.parse(localStorage.getItem('calculatorHistory')) || [];
        
        // Helper Functions
        function formatNumber(num) {
            return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
        
        function updateAmountDisplay() {
            amountDisplay.textContent = formatNumber(currentAmount);
        }
        
        function updatePhoneDisplay() {
            phoneDisplay.textContent = currentPhone;
        }
        
        function updateCalculatorDisplay() {
            calculatorDisplay.textContent = calculatorCurrentInput;
        }
        
        // Fullscreen functionality
        function toggleFullscreen() {
            if (!document.fullscreenElement) {
                document.documentElement.requestFullscreen().catch(err => {
                    console.log(`Error attempting to enable fullscreen: ${err.message}`);
                });
            } else {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                }
            }
        }
        
        // Update fullscreen button icon based on fullscreen state
        document.addEventListener('fullscreenchange', () => {
            const iconClass = document.fullscreenElement ? 'fa-compress' : 'fa-expand';
            fullscreenBtns.forEach(btn => {
                const icon = btn.querySelector('i');
                icon.className = `fas ${iconClass}`;
            });
        });
        
        // Calculator functions
        function calculatorInput(value) {
            if (calculatorShouldResetDisplay) {
                calculatorCurrentInput = '';
                calculatorShouldResetDisplay = false;
            }
            
            if (value === '.' && calculatorCurrentInput.includes('.')) {
                return;
            }
            
            if (calculatorCurrentInput === '0' && value !== '.') {
                calculatorCurrentInput = value;
            } else {
                calculatorCurrentInput += value;
            }
            
            updateCalculatorDisplay();
        }
        
        function calculatorSetOperator(operator) {
            if (calculatorOperator && !calculatorShouldResetDisplay) {
                calculatorCalculate();
            }
            
            calculatorPreviousInput = calculatorCurrentInput;
            calculatorOperator = operator;
            calculatorShouldResetDisplay = true;
        }
        
        function calculatorCalculate() {
            let result;
            const prev = parseFloat(calculatorPreviousInput);
            const current = parseFloat(calculatorCurrentInput);
            
            if (isNaN(prev) || isNaN(current)) return;
            
            switch (calculatorOperator) {
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
                expression: `${calculatorPreviousInput} ${calculatorOperator} ${calculatorCurrentInput}`,
                result: result.toString()
            };
            
            calculatorHistory.unshift(calculation);
            if (calculatorHistory.length > 10) {
                calculatorHistory.pop();
            }
            
            localStorage.setItem('calculatorHistory', JSON.stringify(calculatorHistory));
            renderCalculatorHistory();
            
            calculatorCurrentInput = result.toString();
            calculatorOperator = '';
            calculatorShouldResetDisplay = true;
            updateCalculatorDisplay();
        }
        
        function calculatorClear() {
            calculatorCurrentInput = '0';
            calculatorPreviousInput = '';
            calculatorOperator = '';
            calculatorShouldResetDisplay = false;
            updateCalculatorDisplay();
        }
        
        function calculatorClearEntry() {
            calculatorCurrentInput = '0';
            updateCalculatorDisplay();
        }
        
        function calculatorBackspace() {
            if (calculatorCurrentInput.length > 1) {
                calculatorCurrentInput = calculatorCurrentInput.slice(0, -1);
            } else {
                calculatorCurrentInput = '0';
            }
            updateCalculatorDisplay();
        }
        
        function renderCalculatorHistory() {
            calculatorHistoryList.innerHTML = '';
            
            if (calculatorHistory.length === 0) {
                calculatorHistoryList.innerHTML = '<div style="text-align: center; color: #64748b; padding: 10px;">Belum ada riwayat perhitungan</div>';
                return;
            }
            
            calculatorHistory.forEach(item => {
                const historyItem = document.createElement('div');
                historyItem.className = 'calculator-history-item';
                historyItem.innerHTML = `
                    <div class="calculator-history-expression">${item.expression} =</div>
                    <div class="calculator-history-result">${item.result}</div>
                `;
                
                historyItem.addEventListener('click', () => {
                    calculatorCurrentInput = item.result;
                    updateCalculatorDisplay();
                });
                
                calculatorHistoryList.appendChild(historyItem);
            });
        }
        
        // Initialize
        function initializeApp() {
            merchantName.value = savedMerchant;
            headerTitleInput.value = savedHeaderTitle;
            headerSubtitleInput.value = savedHeaderSubtitle;
            footerTextInput.value = savedFooterText;
            qrisStaticCode.value = savedQrisStaticCode;
            paymentMerchant.textContent = savedMerchant;
            headerTitle.textContent = savedHeaderTitle;
            headerSubtitle.textContent = savedHeaderSubtitle;
            footerText.textContent = savedFooterText;
            paymentTitle.textContent = savedHeaderTitle;
            renderPhoneHistory();
            renderTransactions();
            updateQrCodeDisplay();
            renderDebts();
            updateDebtNameList();
            renderInventory();
            renderShoppingList();
            renderCalculatorHistory();
            
            if (savedQrisImage) {
                qrisPreview.style.display = 'block';
                qrisPreviewImg.src = savedQrisImage;
            }
        }
        
        // Finance tabs functionality
        financeTabs.forEach(tab => {
            tab.addEventListener('click', () => {
                const tabId = tab.getAttribute('data-tab');
                
                financeTabs.forEach(t => t.classList.remove('active'));
                financeTabContents.forEach(content => content.classList.remove('active'));
                
                tab.classList.add('active');
                document.getElementById(tabId).classList.add('active');
            });
        });
        
        // Settings tabs functionality
        tabBtns.forEach(tabBtn => {
            tabBtn.addEventListener('click', () => {
                const tabId = tabBtn.getAttribute('data-tab');
                
                tabBtns.forEach(btn => btn.classList.remove('active'));
                tabContents.forEach(content => content.classList.remove('active'));
                
                tabBtn.classList.add('active');
                document.getElementById(tabId).classList.add('active');
            });
        });
        
        // Navigation functionality
        function switchPage(pageId) {
            navBtns.forEach(btn => {
                if(btn.getAttribute('data-page') === pageId) {
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });
             pages.forEach(page => {
                if(page.id === pageId) {
                    page.classList.add('active');
                } else {
                    page.classList.remove('active');
                }
            });

             if (pageId === 'financePage') {
                 renderTransactions();
                 renderDebts();
             } else if (pageId === 'qrcodePage') {
                 updateQrCodeDisplay();
             } else if (pageId === 'inventoryPage') {
                 renderInventory();
                 renderShoppingList();
             } else if (pageId === 'calculatorPage') {
                 renderCalculatorHistory();
             }
        }

        navBtns.forEach(navBtn => {
            navBtn.addEventListener('click', () => {
                const pageId = navBtn.getAttribute('data-page');
                switchPage(pageId);
            });
        });
        
        // Inventory button functionality
        inventoryButton.addEventListener('click', () => {
            switchPage('inventoryPage');
        });
        
        // Update time
        function updateTime() {
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            paymentTime.textContent = `${hours}:${minutes}`;
        }
        
        // Generate random transaction ID
        function generateTransactionId() {
            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
            let result = 'TRX-';
            for (let i = 0; i < 6; i++) {
                result += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return result;
        }
        
        // Render phone history
        function renderPhoneHistory() {
            phoneHistory.innerHTML = '<h3>Riwayat Nomor</h3>';
            
            if (phoneHistoryData.length === 0) {
                phoneHistory.innerHTML += '<p style="text-align: center; color: #64748b; padding: 10px;">Belum ada riwayat nomor handphone</p>';
                return;
            }
            
            phoneHistoryData.forEach(item => {
                const historyItem = document.createElement('div');
                historyItem.className = 'history-item';
                historyItem.dataset.phone = item.phone;
                
                historyItem.innerHTML = `
                    <div class="history-phone">${item.phone}</div>
                    <div class="history-name">${item.name || 'Tidak ada nama'}</div>
                `;
                
                historyItem.addEventListener('click', () => {
                    currentPhone = item.phone;
                    updatePhoneDisplay();
                });
                
                phoneHistory.appendChild(historyItem);
            });
        }
        
        // Render transactions
        function renderTransactions() {
            transactionList.innerHTML = '';
            
            const confirmedTransactions = transactions.filter(t => t.status === 'success').sort((a, b) => new Date(b.rawDate) - new Date(a.rawDate));
            
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
                        <div class="transaction-amount">Rp ${formatNumber(transaction.amount)}</div>
                        <div class="transaction-status status-success">Berhasil</div>
                    </div>
                `;
                
                transactionList.appendChild(transactionItem);
            });
        }
        
        // Render debts
        function renderDebts() {
            debtList.innerHTML = '<h3>Daftar Hutang</h3>';
            
            if (debts.length === 0) {
                debtList.innerHTML += '<p style="text-align: center; color: #64748b; padding: 20px;">Belum ada catatan hutang</p>';
                return;
            }
            
            debts.forEach(debt => {
                const debtItem = document.createElement('div');
                debtItem.className = 'debt-item';
                debtItem.dataset.id = debt.id;
                
                debtItem.innerHTML = `
                    <button class="debt-item-menu"><i class="fas fa-ellipsis-v"></i></button>
                    <div class="debt-item-menu-content">
                        <button class="edit-debt" data-id="${debt.id}">Edit</button>
                        <button class="delete-debt" data-id="${debt.id}">Hapus</button>
                    </div>
                    <div class="debt-header">
                        <div>
                            <div class="debt-name">${debt.name}</div>
                            <div class="debt-phone">${debt.phone}</div>
                        </div>
                    </div>
                    <div class="debt-details">
                        <div>
                            <div class="debt-item-name">${debt.item}</div>
                            <div class="debt-amount">Rp ${formatNumber(debt.amount)}</div>
                        </div>
                        <div class="debt-status status-${debt.status}" data-id="${debt.id}">${debt.status === 'paid' ? 'Lunas' : 'Belum Lunas'}</div>
                    </div>
                `;
                
                const menuBtn = debtItem.querySelector('.debt-item-menu');
                const menuContent = debtItem.querySelector('.debt-item-menu-content');
                
                menuBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    document.querySelectorAll('.debt-item-menu-content').forEach(menu => {
                        if (menu !== menuContent) menu.style.display = 'none';
                    });
                    menuContent.style.display = menuContent.style.display === 'block' ? 'none' : 'block';
                });
                
                debtItem.querySelector('.edit-debt').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const debtId = e.target.getAttribute('data-id');
                    showPasswordPrompt('edit', debtId);
                    menuContent.style.display = 'none';
                });
                
                debtItem.querySelector('.delete-debt').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const debtId = e.target.getAttribute('data-id');
                    if (confirm('Apakah Anda yakin ingin menghapus hutang ini?')) {
                        debts = debts.filter(d => d.id !== debtId);
                        localStorage.setItem('debts', JSON.stringify(debts));
                        renderDebts();
                    }
                    menuContent.style.display = 'none';
                });
                
                const statusElement = debtItem.querySelector('.debt-status');
                statusElement.addEventListener('click', (e) => {
                    e.stopPropagation();
                    const debtId = e.target.getAttribute('data-id');
                    const debt = debts.find(d => d.id === debtId);
                    
                    if (debt.status === 'pending') {
                        showPasswordPrompt('status', debtId);
                    }
                });
                
                debtList.appendChild(debtItem);
            });
            
            document.addEventListener('click', () => {
                document.querySelectorAll('.debt-item-menu-content').forEach(menu => {
                    menu.style.display = 'none';
                });
            });
        }
        
        // Update debt name list for autocomplete
        function updateDebtNameList() {
            debtNameList.innerHTML = '';
            const uniqueNames = [...new Set(debts.map(debt => debt.name))];
            
            uniqueNames.forEach(name => {
                const option = document.createElement('option');
                option.value = name;
                debtNameList.appendChild(option);
            });
        }
        
        debtName.addEventListener('input', () => {
            const selectedName = debtName.value;
            const existingDebt = debts.find(debt => debt.name === selectedName);
            
            if (existingDebt) {
                debtPhone.value = existingDebt.phone;
            }
        });
        
        function showPasswordPrompt(context, debtId) {
            passwordContext = context;
            currentDebtId = debtId;
            passwordModal.style.display = 'flex';
            passwordInput.value = '';
            passwordError.style.display = 'none';
            passwordInput.focus();
        }
        
        function updateQrCodeDisplay() {
            qrcodeDisplay.innerHTML = '';
            
            if (savedQrisImage) {
                qrcodeDisplay.innerHTML = `
                    <h3>${savedMerchant}</h3>
                    <p> </p>
                    <img src="${savedQrisImage}" alt="QRIS Code" style="max-width: 100%; height: auto; border: 1px solid #e2e8f0; border-radius: 8px;">
                    <p>Scan Kode QRIS di atas untuk pembayaran</p>
                `;
            } else {
                qrcodeDisplay.innerHTML = `
                    <h3>${savedMerchant}</h3>
                    <p> </p>
                    <div style="background-color: #f1f5f9; padding: 20px; display: inline-block; margin: 15px 0;">
                        <i class="fas fa-qrcode" style="font-size: 120px; color: #1e293b;"></i>
                    </div>
                    <p>Upload gambar QRIS melalui pengaturan</p>
                `;
            }
        }
        
        // Render inventory
        function renderInventory() {
            inventoryList.innerHTML = '<h3>Daftar Inventori</h3>';
            
            if (inventory.length === 0) {
                inventoryList.innerHTML += '<p style="text-align: center; color: #64748b; padding: 20px;">Belum ada data inventori</p>';
                return;
            }
            
            inventory.forEach(item => {
                const inventoryItem = document.createElement('div');
                inventoryItem.className = 'inventory-item';
                inventoryItem.dataset.id = item.id;
                
                const stockStatus = item.stock <= item.minStock ? 'stock-warning' : 'stock-adequate';
                const stockText = item.stock <= item.minStock ? 'Stok Rendah!' : 'Stok Cukup';
                
                inventoryItem.innerHTML = `
                    <button class="inventory-item-menu"><i class="fas fa-ellipsis-v"></i></button>
                    <div class="inventory-item-menu-content">
                        <button class="edit-item" data-id="${item.id}">Edit</button>
                        <button class="delete-item" data-id="${item.id}">Hapus</button>
                    </div>
                    <div class="inventory-header">
                        <div>
                            <div class="inventory-name">${item.name}</div>
                            <div class="inventory-category">${getCategoryName(item.category)}</div>
                        </div>
                        <div class="inventory-price">Rp ${formatNumber(item.price || 0)}</div>
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
                
                const menuBtn = inventoryItem.querySelector('.inventory-item-menu');
                const menuContent = inventoryItem.querySelector('.inventory-item-menu-content');
                
                menuBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    document.querySelectorAll('.inventory-item-menu-content').forEach(menu => {
                        if (menu !== menuContent) menu.style.display = 'none';
                    });
                    menuContent.style.display = menuContent.style.display === 'block' ? 'none' : 'block';
                });
                
                inventoryItem.querySelector('.edit-item').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    const item = inventory.find(i => i.id === itemId);
                    
                    if (item) {
                        editItemName.value = item.name;
                        editItemPrice.value = item.price;
                        editItemCategory.value = item.category;
                        editItemStock.value = item.stock;
                        editItemMinStock.value = item.minStock;
                        currentItemId = itemId;
                        inventoryEditModal.style.display = 'flex';
                    }
                    menuContent.style.display = 'none';
                });
                
                inventoryItem.querySelector('.delete-item').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    if (confirm('Apakah Anda yakin ingin menghapus barang ini?')) {
                        inventory = inventory.filter(i => i.id !== itemId);
                        localStorage.setItem('inventory', JSON.stringify(inventory));
                        renderInventory();
                        renderShoppingList();
                    }
                    menuContent.style.display = 'none';
                });
                
                inventoryItem.querySelector('.decrease-stock').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    const itemIndex = inventory.findIndex(i => i.id === itemId);
                    
                    if (itemIndex >= 0 && inventory[itemIndex].stock > 0) {
                        inventory[itemIndex].stock--;
                        localStorage.setItem('inventory', JSON.stringify(inventory));
                        renderInventory();
                        renderShoppingList();
                    }
                });
                
                inventoryItem.querySelector('.increase-stock').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    const itemIndex = inventory.findIndex(i => i.id === itemId);
                    
                    if (itemIndex >= 0) {
                        inventory[itemIndex].stock++;
                        localStorage.setItem('inventory', JSON.stringify(inventory));
                        renderInventory();
                        renderShoppingList();
                    }
                });
                
                // POS Sell button
                inventoryItem.querySelector('.sell-item').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    const itemIndex = inventory.findIndex(i => i.id === itemId);
                    const item = inventory[itemIndex];

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

                    // Calculate total and update stock
                    const total = qty * item.price;
                    inventory[itemIndex].stock -= qty;
                    localStorage.setItem('inventory', JSON.stringify(inventory));

                    // Switch to home page and set amount
                    currentAmount = String(total);
                    updateAmountDisplay();
                    switchPage('homePage');
                    
                    // Re-render inventory in background
                    renderInventory();
                    renderShoppingList();
                });

                
                inventoryList.appendChild(inventoryItem);
            });
            
            document.addEventListener('click', () => {
                document.querySelectorAll('.inventory-item-menu-content').forEach(menu => {
                    menu.style.display = 'none';
                });
            });
        }
        
        // Render shopping list
        function renderShoppingList() {
            shoppingList.innerHTML = '';
            
            const lowStockItems = inventory.filter(item => item.stock <= item.minStock);
            
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
                            <div class="inventory-category">${getCategoryName(item.category)}</div>
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
        }
        
        function getCategoryName(category) {
            const categories = {
                'makanan': 'Makanan', 'minuman': 'Minuman', 'snack': 'Snack',
                'sembako': 'Sembako', 'lainnya': 'Lainnya'
            };
            return categories[category] || 'Lainnya';
        }
        
        // Event Listeners
        [profileBtn, financeProfileBtn, qrcodeProfileBtn, pulsaProfileBtn, inventoryProfileBtn, calculatorProfileBtn].forEach(btn => {
            btn.addEventListener('click', () => {
                passwordContext = 'settings';
                passwordModal.style.display = 'flex';
                passwordInput.value = '';
                passwordError.style.display = 'none';
                passwordInput.focus();
            });
        });

        // Fullscreen button event listeners
        fullscreenBtns.forEach(btn => {
            btn.addEventListener('click', toggleFullscreen);
        });

        closePassword.addEventListener('click', () => { passwordModal.style.display = 'none'; });
        
        submitPassword.addEventListener('click', () => {
            if (passwordInput.value === appPassword) {
                passwordModal.style.display = 'none';
                
                if (passwordContext === 'status') {
                    const debtIndex = debts.findIndex(d => d.id === currentDebtId);
                    if (debtIndex >= 0) {
                        debts[debtIndex].status = 'paid';
                        localStorage.setItem('debts', JSON.stringify(debts));
                        renderDebts();
                    }
                } else if (passwordContext === 'edit') {
                    const debt = debts.find(d => d.id === currentDebtId);
                    if (debt) {
                        editDebtName.value = debt.name;
                        editDebtPhone.value = debt.phone;
                        editDebtItem.value = debt.item;
                        editDebtAmount.value = debt.amount;
                        debtEditModal.style.display = 'flex';
                    }
                } else {
                    settingsModal.style.display = 'flex';
                }
            } else {
                passwordError.style.display = 'block';
                passwordInput.value = '';
                setTimeout(() => { passwordError.style.display = 'none'; }, 2000);
            }
        });
        
        closeSettings.addEventListener('click', () => { settingsModal.style.display = 'none'; });
        closeEditDebtModal.addEventListener('click', () => { debtEditModal.style.display = 'none'; });
        closeEditItemModal.addEventListener('click', () => { inventoryEditModal.style.display = 'none'; });
        closePaymentModal.addEventListener('click', () => { paymentModal.style.display = 'none'; });
        closePulsaModal.addEventListener('click', () => { pulsaModal.style.display = 'none'; });
        
        [passwordModal, settingsModal, debtEditModal, inventoryEditModal, paymentModal, pulsaModal].forEach(modal => {
            modal.addEventListener('click', (e) => {
                if (e.target === modal) modal.style.display = 'none';
            });
        });
        
        document.querySelectorAll('#homePage .key[data-value]').forEach(key => {
            key.addEventListener('click', () => {
                const value = key.getAttribute('data-value');
                currentAmount = (currentAmount === '0') ? value : currentAmount + value;
                updateAmountDisplay();
            });
        });
        
        document.querySelectorAll('#pulsaPage .key[data-value]').forEach(key => {
            key.addEventListener('click', () => {
                const value = key.getAttribute('data-value');
                currentPhone = (currentPhone === '0') ? value : currentPhone + value;
                updatePhoneDisplay();
            });
        });
        
        // Calculator key listeners
        calculatorKeys.forEach(key => {
            key.addEventListener('click', () => {
                const value = key.getAttribute('data-value');
                const operator = key.getAttribute('data-operator');
                
                if (value) {
                    calculatorInput(value);
                } else if (operator) {
                    if (operator === 'backspace') {
                        calculatorBackspace();
                    } else {
                        calculatorSetOperator(operator);
                    }
                }
            });
        });
        
        calculatorClear.addEventListener('click', calculatorClear);
        calculatorClearEntry.addEventListener('click', calculatorClearEntry);
        calculatorEquals.addEventListener('click', calculatorCalculate);
        
        backspaceBtn.addEventListener('click', () => {
            currentAmount = (currentAmount.length > 1) ? currentAmount.slice(0, -1) : '0';
            updateAmountDisplay();
        });
        
        pulsaBackspaceBtn.addEventListener('click', () => {
            currentPhone = (currentPhone.length > 1) ? currentPhone.slice(0, -1) : '0';
            updatePhoneDisplay();
        });
        
        clearButton.addEventListener('click', () => {
            currentAmount = '0';
            updateAmountDisplay();
        });
        
        clearPhoneButton.addEventListener('click', () => {
            currentPhone = '0';
            updatePhoneDisplay();
        });
        
        savePhoneButton.addEventListener('click', () => {
            if (currentPhone === '0' || currentPhone.length < 10) {
                alert('Harap masukkan nomor handphone yang valid!');
                return;
            }
            
            selectedPhoneNumber = currentPhone;
            phoneName.value = '';
            pulsaModal.style.display = 'flex';
            
            pulsaOptions.forEach(option => option.classList.remove('active'));
            customAmountGroup.style.display = 'none';
            selectedPulsaAmount = 0;
        });
        
        pulsaOptions.forEach(option => {
            option.addEventListener('click', () => {
                pulsaOptions.forEach(opt => opt.classList.remove('active'));
                option.classList.add('active');
                
                const amount = option.getAttribute('data-amount');
                selectedPulsaAmount = parseInt(amount);
                
                customAmountGroup.style.display = (amount === '0') ? 'block' : 'none';
            });
        });
        
        customAmount.addEventListener('input', () => { selectedPulsaAmount = parseInt(customAmount.value) || 0; });
        
        confirmPulsaButton.addEventListener('click', () => {
            if (selectedPulsaAmount <= 0) {
                alert('Harap pilih nominal pulsa!');
                return;
            }
            
            const name = phoneName.value.trim() || 'Tidak ada nama';
            const existingIndex = phoneHistoryData.findIndex(item => item.phone === selectedPhoneNumber);
            
            if (existingIndex >= 0) phoneHistoryData[existingIndex].name = name;
            else phoneHistoryData.push({ phone: selectedPhoneNumber, name: name });
            
            localStorage.setItem('phoneHistory', JSON.stringify(phoneHistoryData));
            
            const now = new Date();
            const newTransaction = {
                id: generateTransactionId(),
                amount: selectedPulsaAmount,
                date: now.toLocaleDateString('id-ID', { day: '2-digit', month: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit' }),
                rawDate: now.toISOString(),
                status: 'success', // Auto confirm for pulsa
                type: 'pulsa',
                phone: selectedPhoneNumber
            };
            
            transactions.push(newTransaction);
            localStorage.setItem('transactions', JSON.stringify(transactions));
            
            pulsaModal.style.display = 'none';
            alert('Pembelian pulsa berhasil!');
            
            renderPhoneHistory();
            if (document.getElementById('financePage').classList.contains('active')) {
                renderTransactions();
            }
        });
        
        addDebtButton.addEventListener('click', () => {
            const name = debtName.value.trim();
            const phone = debtPhone.value.trim();
            const itemName = debtItem.value.trim();
            const amount = parseInt(debtAmount.value);
            
            if (!name || !phone || !itemName || !amount) {
                alert('Harap isi semua field!');
                return;
            }
            
            debts.push({
                id: Date.now().toString(), name, phone, item: itemName, amount,
                status: 'pending', date: new Date().toLocaleDateString('id-ID')
            });
            localStorage.setItem('debts', JSON.stringify(debts));
            
            debtName.value = ''; debtPhone.value = ''; debtItem.value = ''; debtAmount.value = '';
            
            renderDebts();
            updateDebtNameList();
        });
        
        saveEditDebtButton.addEventListener('click', () => {
            const name = editDebtName.value.trim();
            const phone = editDebtPhone.value.trim();
            const itemName = editDebtItem.value.trim();
            const amount = parseInt(editDebtAmount.value);
            
            if (!name || !phone || !itemName || !amount) {
                alert('Harap isi semua field!');
                return;
            }
            
            const debtIndex = debts.findIndex(d => d.id === currentDebtId);
            if (debtIndex >= 0) {
                debts[debtIndex] = {...debts[debtIndex], name, phone, item: itemName, amount};
                localStorage.setItem('debts', JSON.stringify(debts));
                renderDebts();
                updateDebtNameList();
                debtEditModal.style.display = 'none';
            }
        });
        
        addItemButton.addEventListener('click', () => {
            const name = itemName.value.trim();
            const price = parseFloat(itemPrice.value) || 0;
            const category = itemCategory.value;
            const stock = parseInt(itemStock.value) || 0;
            const minStock = parseInt(itemMinStock.value) || 0;
            
            if (!name || !category) {
                alert('Harap isi nama dan kategori barang!');
                return;
            }
            
            inventory.push({ id: Date.now().toString(), name, price, category, stock, minStock });
            localStorage.setItem('inventory', JSON.stringify(inventory));
            
            itemName.value = ''; itemPrice.value = ''; itemCategory.value = 'makanan';
            itemStock.value = ''; itemMinStock.value = '';
            
            renderInventory();
            renderShoppingList();
        });
        
        saveEditItemButton.addEventListener('click', () => {
            const name = editItemName.value.trim();
            const price = parseFloat(editItemPrice.value) || 0;
            const category = editItemCategory.value;
            const stock = parseInt(editItemStock.value) || 0;
            const minStock = parseInt(editItemMinStock.value) || 0;
            
            if (!name || !category) {
                alert('Harap isi nama dan kategori barang!');
                return;
            }
            
            const itemIndex = inventory.findIndex(i => i.id === currentItemId);
            if (itemIndex >= 0) {
                inventory[itemIndex] = { ...inventory[itemIndex], name, price, category, stock, minStock };
                localStorage.setItem('inventory', JSON.stringify(inventory));
                renderInventory();
                renderShoppingList();
                inventoryEditModal.style.display = 'none';
            }
        });
        
        convertButton.addEventListener('click', async () => {
            if (currentAmount === '0') {
                alert('Harap masukkan nominal terlebih dahulu!');
                return;
            }
            
            loading.style.display = 'block';
            
            try {
                const qrisData = savedQrisStaticCode;
                if (!qrisData) {
                    alert('Harap masukkan Kode QRIS statis di pengaturan dahulu!');
                    loading.style.display = 'none';
                    return;
                }
                
                const encodedQris = encodeURIComponent(qrisData);
                const response = await fetch(`https://cekid-ariepulsa.my.id/api/?qris_data=${encodedQris}&nominal=${currentAmount}`);
                const data = await response.json();
                
                if (data.status === 'success') {
                    paymentAmount.textContent = `Rp ${formatNumber(currentAmount)}`;
                    paymentQr.src = data.link_qris;
                    paymentMerchant.textContent = savedMerchant;
                    currentQrUrl = data.link_qris;
                    
                    updateTime();
                    const newId = generateTransactionId();
                    transactionId.textContent = newId;
                    currentTransactionIdForConfirmation = newId;

                    const now = new Date();
                    const newTransaction = {
                        id: newId,
                        amount: parseInt(currentAmount),
                        date: now.toLocaleDateString('id-ID', { day: '2-digit', month: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit' }),
                        rawDate: now.toISOString(),
                        status: 'pending',
                        type: 'qris'
                    };
                    
                    transactions.push(newTransaction);
                    localStorage.setItem('transactions', JSON.stringify(transactions));
                    paymentModal.style.display = 'flex';
                } else {
                    alert('API mengembalikan status error: ' + (data.message || 'Unknown error'));
                }
            } catch (error) {
                alert('Terjadi kesalahan: ' + error.message);
            } finally {
                loading.style.display = 'none';
            }
        });

        confirmPayment.addEventListener('click', () => {
             if (currentTransactionIdForConfirmation) {
                const transactionIndex = transactions.findIndex(t => t.id === currentTransactionIdForConfirmation);
                if (transactionIndex >= 0) {
                    transactions[transactionIndex].status = 'success';
                    localStorage.setItem('transactions', JSON.stringify(transactions));
                    alert(`Transaksi ${currentTransactionIdForConfirmation} dikonfirmasi berhasil!`);
                    
                    paymentModal.style.display = 'none';
                    currentTransactionIdForConfirmation = null;

                    if (document.getElementById('financePage').classList.contains('active')) {
                        renderTransactions();
                    }
                }
             }
        });
        
        closePayment.addEventListener('click', () => { paymentModal.style.display = 'none'; });
        
        qrisDropArea.addEventListener('click', () => qrisInput.click());
        
        qrisInput.addEventListener('change', (e) => {
            if (e.target.files.length) {
                const reader = new FileReader();
                reader.onload = function(event) {
                    savedQrisImage = event.target.result;
                    localStorage.setItem('qrisImage', savedQrisImage);
                    qrisPreview.style.display = 'block';
                    qrisPreviewImg.src = savedQrisImage;
                    if (document.getElementById('qrcodePage').classList.contains('active')) {
                        updateQrCodeDisplay();
                    }
                };
                reader.readAsDataURL(e.target.files[0]);
            }
        });
        
        resetQrisButton.addEventListener('click', () => {
            if (confirm('Apakah Anda yakin ingin menghapus gambar QRIS?')) {
                savedQrisImage = '';
                localStorage.removeItem('qrisImage');
                qrisPreview.style.display = 'none';
                if (document.getElementById('qrcodePage').classList.contains('active')) {
                    updateQrCodeDisplay();
                }
            }
        });
        
        changePasswordButton.addEventListener('click', () => {
            const current = currentPassword.value;
            const newPass = newPassword.value;
            const confirmPass = confirmPassword.value;
            
            if (current !== appPassword) return alert('Sandi saat ini tidak benar!');
            if (newPass.length !== 6) return alert('Sandi baru harus 6 digit!');
            if (newPass !== confirmPass) return alert('Konfirmasi sandi tidak sesuai!');
            
            appPassword = newPass;
            localStorage.setItem('appPassword', appPassword);
            alert('Sandi berhasil diubah!');
            currentPassword.value = ''; newPassword.value = ''; confirmPassword.value = '';
        });
        
        saveButton.addEventListener('click', () => {
            const merchant = merchantName.value.trim();
            const headerTitleText = headerTitleInput.value.trim();
            const headerSubtitleText = headerSubtitleInput.value.trim();
            const footerTextContent = footerTextInput.value.trim();
            const qrisCode = qrisStaticCode.value.trim();
            
            if (!merchant || !headerTitleText || !headerSubtitleText || !footerTextContent || !qrisCode) {
                return alert('Harap isi semua field!');
            }
            
            savedMerchant = merchant;
            savedHeaderTitle = headerTitleText;
            savedHeaderSubtitle = headerSubtitleText;
            savedFooterText = footerTextContent;
            savedQrisStaticCode = qrisCode;
            
            localStorage.setItem('merchantName', merchant);
            localStorage.setItem('headerTitle', headerTitleText);
            localStorage.setItem('headerSubtitle', headerSubtitleText);
            localStorage.setItem('footerText', footerTextContent);
            localStorage.setItem('qrisStaticCode', qrisCode);
            
            paymentMerchant.textContent = merchant;
            headerTitle.textContent = headerTitleText;
            headerSubtitle.textContent = headerSubtitleText;
            footerText.textContent = footerTextContent;
            paymentTitle.textContent = headerTitleText;
            
            settingsModal.style.display = 'none';
        });
        
        // Inventory Data Management
        downloadCsvButton.addEventListener('click', () => {
            if (inventory.length === 0) return alert("Inventori kosong.");
            
            const headers = "ID,Nama Barang,Harga,Kategori,Stok,Stok Minimum";
            const csvContent = inventory.map(item => 
                [item.id, `"${item.name}"`, item.price, item.category, item.stock, item.minStock].join(',')
            ).join('\n');
            
            const blob = new Blob([headers + '\n' + csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            const url = URL.createObjectURL(blob);
            link.setAttribute("href", url);
            link.setAttribute("download", "inventory.csv");
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        });
        
        backupButton.addEventListener('click', () => {
            const dataStr = JSON.stringify(inventory, null, 2);
            const blob = new Blob([dataStr], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.href = url;
            link.download = 'inventory-backup.json';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        });
        
        restoreInput.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = function(event) {
                try {
                    const data = JSON.parse(event.target.result);
                    if (Array.isArray(data)) {
                        inventory = data;
                        localStorage.setItem('inventory', JSON.stringify(inventory));
                        renderInventory();
                        renderShoppingList();
                        alert('Data inventori berhasil dipulihkan!');
                    } else {
                        alert('Format file tidak valid.');
                    }
                } catch (error) {
                    alert('Terjadi kesalahan saat membaca file: ' + error.message);
                }
            };
            reader.readAsText(file);
            e.target.value = ''; // Reset input
        });
        
        importCsvInput.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = function(event) {
                try {
                    const csvText = event.target.result;
                    const lines = csvText.split('\n');
                    const headers = lines[0].split(',');
                    
                    // Skip header row and process data
                    for (let i = 1; i < lines.length; i++) {
                        if (lines[i].trim() === '') continue;
                        
                        const values = lines[i].split(',');
                        if (values.length >= 6) {
                            const item = {
                                id: values[0].replace(/"/g, ''),
                                name: values[1].replace(/"/g, ''),
                                price: parseFloat(values[2]) || 0,
                                category: values[3],
                                stock: parseInt(values[4]) || 0,
                                minStock: parseInt(values[5]) || 0
                            };
                            
                            // Check if item already exists
                            const existingIndex = inventory.findIndex(i => i.id === item.id);
                            if (existingIndex >= 0) {
                                inventory[existingIndex] = item;
                            } else {
                                inventory.push(item);
                            }
                        }
                    }
                    
                    localStorage.setItem('inventory', JSON.stringify(inventory));
                    renderInventory();
                    renderShoppingList();
                    alert('Data CSV berhasil diimpor!');
                } catch (error) {
                    alert('Terjadi kesalahan saat membaca file CSV: ' + error.message);
                }
            };
            reader.readAsText(file);
            e.target.value = ''; // Reset input
        });

        // Initialize the app
        initializeApp();
    </script>
</body>
</html>
