<!DOCTYPE html>
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

        /* Header */
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

        .profile-btn, .fullscreen-btn {
            position: absolute;
            top: 12px;
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

        .profile-btn {
            right: 12px;
        }

        .fullscreen-btn {
            left: 12px;
        }

        .profile-btn:hover, .fullscreen-btn:hover {
            background-color: rgba(255, 255, 255, 0.25);
            transform: scale(1.05);
        }

        /* Beranda - Tampilan Baru dengan Riwayat Transaksi dan Carousel */
        .payment-section {
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

        .payment-header {
            text-align: center;
            margin-bottom: 16px;
        }

        .payment-header h2 {
            font-size: 1.25rem;
            font-weight: 600;
            color: #1e293b;
            margin-bottom: 8px;
        }

        /* Carousel Styles */
        .carousel-container {
            position: relative;
            width: 100%;
            margin-bottom: 16px;
            overflow: hidden;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }

        .carousel {
            display: flex;
            transition: transform 0.3s ease;
            height: 150px;
        }

        .carousel-slide {
            min-width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .carousel-image {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .carousel-overlay {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(transparent, rgba(0, 0, 0, 0.7));
            color: white;
            padding: 10px;
            text-align: center;
        }

        .carousel-indicators {
            display: flex;
            justify-content: center;
            margin-top: 10px;
        }

        .carousel-indicator {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background-color: #cbd5e1;
            margin: 0 4px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .carousel-indicator.active {
            background-color: #3b82f6;
        }

        .carousel-nav {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(0, 0, 0, 0.5);
            color: white;
            border: none;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            z-index: 10;
        }

        .carousel-prev {
            left: 10px;
        }

        .carousel-next {
            right: 10px;
        }

        /* Riwayat Transaksi Terbaru */
        .recent-transactions {
            margin-top: 16px;
        }

        .recent-transactions h3 {
            font-size: 1rem;
            font-weight: 600;
            margin-bottom: 10px;
            color: #1e293b;
        }

        .transaction-card {
            background: #f8fafc;
            border-radius: 8px;
            padding: 12px;
            margin-bottom: 10px;
            border-left: 4px solid #10b981;
        }

        .transaction-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }

        .transaction-id {
            font-size: 0.75rem;
            color: #64748b;
            font-weight: 500;
        }

        .transaction-date {
            font-size: 0.7rem;
            color: #94a3b8;
        }

        .transaction-amount {
            font-size: 1.1rem;
            font-weight: 600;
            color: #059669;
        }

        .no-transactions {
            text-align: center;
            padding: 20px;
            color: #64748b;
            font-style: italic;
        }

        /* Input Section untuk nominal pembayaran */
        .input-section {
            margin-bottom: 16px;
            background-color: #ffffff;
            padding: 12px;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
            border: 1px solid #e2e8f0;
        }

        .input-section h3 {
            margin-bottom: 10px;
            color: #1e293b;
            font-size: 0.9rem;
            font-weight: 500;
            text-align: center;
        }

        .amount-display, .phone-display, .calculator-display {
            text-align: right;
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 10px;
            padding: 10px;
            background-color: #f1f5f9;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
            color: #1e293b;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            word-break: break-all;
        }

        /* Keypads */
        .numeric-keypad-container {
            display: flex;
            flex-direction: column;
        }

        .numeric-keypad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 6px;
        }

        .calculator-keypad {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
        }

        .key {
            padding: 8px;
            background: linear-gradient(145deg, #ffffff, #f1f5f9);
            color: #1e293b;
            border: 1px solid #e2e8f0;
            border-radius: 6px;
            font-size: 1rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
            min-height: 40px;
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

        /* Action Buttons */
        .action-buttons {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 6px;
            margin-top: 10px;
        }

        .convert-button, .clear-button, .save-button, .confirm-payment {
            padding: 8px;
            border-radius: 6px;
            font-size: 0.875rem;
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

        /* Navigation */
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

        /* Pages */
        .page {
            display: none;
            animation: fadeIn 0.3s ease;
            height: 100%;
            flex-direction: column;
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

        /* Modal - Diperbaiki dengan pengaturan yang lebih baik */
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

        /* Modal Pembayaran - Diperbaiki */
        .payment-content {
            max-width: 320px;
            max-height: 85vh;
            padding: 15px;
            overflow: hidden;
            display: flex;
            flex-direction: column;
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
            width: 180px;
            height: 180px;
            max-width: 100%;
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

        /* Drop Area untuk upload QRIS dan Carousel */
        .drop-area {
            border: 2px dashed #cbd5e1;
            border-radius: 8px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-bottom: 16px;
        }

        .drop-area:hover, .drop-area.dragover {
            border-color: #3b82f6;
            background-color: #f0f9ff;
        }

        .drop-area i {
            font-size: 2rem;
            color: #94a3b8;
            margin-bottom: 10px;
        }

        .qris-preview, .carousel-preview {
            text-align: center;
            margin: 16px 0;
        }

        .qris-preview img, .carousel-preview img {
            max-width: 200px;
            height: auto;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
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

        /* Kalkulator */
        .calculator-container {
            display: flex;
            flex-direction: column;
            height: 100%;
        }

        .calculator-main {
            display: flex;
            flex: 1;
            gap: 10px;
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
            max-height: 300px;
        }

        .calculator-history {
            flex: 1;
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

        /* Footer */
        footer {
            text-align: center;
            margin-top: 24px;
            padding: 12px;
            color: #64748b;
            font-size: 0.75rem;
            flex-shrink: 0;
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

        /* Password Strength Indicator */
        .password-strength {
            height: 4px;
            background: #e2e8f0;
            border-radius: 2px;
            margin-top: 8px;
            overflow: hidden;
        }

        .password-strength-bar {
            height: 100%;
            width: 0;
            transition: width 0.3s ease, background-color 0.3s ease;
        }

        .password-strength.weak .password-strength-bar {
            width: 33%;
            background-color: #dc2626;
        }

        .password-strength.medium .password-strength-bar {
            width: 66%;
            background-color: #d97706;
        }

        .password-strength.strong .password-strength-bar {
            width: 100%;
            background-color: #16a34a;
        }

        /* Error and Success Messages */
        .error-message {
            color: #dc2626;
            font-size: 0.75rem;
            margin-top: 4px;
            display: none;
        }

        .success-message {
            color: #16a34a;
            font-size: 0.75rem;
            margin-top: 4px;
            display: none;
        }

        /* Button Group */
        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        .save-button, .reset-button, .clear-button {
            padding: 12px 20px;
            border-radius: 8px;
            font-size: 0.9375rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            border: none;
            flex: 1;
        }

        .save-button {
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            color: #ffffff;
        }

        .reset-button {
            background: #f1f5f9;
            color: #1e293b;
            border: 1px solid #e2e8f0;
        }

        .clear-button {
            background: #fef2f2;
            color: #dc2626;
            border: 1px solid #fecaca;
        }

        .save-button:hover {
            background: linear-gradient(135deg, #1e40af, #60a5fa);
        }

        .reset-button:hover {
            background: #e2e8f0;
        }

        .clear-button:hover {
            background: #fee2e2;
        }

        /* Responsive */
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
                font-size: 1.3rem;
                min-height: 45px;
                padding: 8px;
            }
            .key {
                padding: 6px;
                font-size: 0.9rem;
                min-height: 35px;
            }
            .numeric-keypad, .calculator-keypad {
                gap: 4px;
            }
            .input-section, .payment-section {
                padding: 10px;
            }
            .payment-header h2 {
                font-size: 1.1rem;
            }
            .carousel {
                height: 120px;
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
            .payment-content {
                max-width: 300px;
                padding: 12px;
            }
            .payment-qr img {
                width: 160px;
                height: 160px;
            }
            .calculator-main {
                flex-direction: column;
            }
            .calculator-history-section {
                max-height: 200px;
            }
        }

        /* Fullscreen */
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
        <!-- Home Page - Diperbarui dengan Riwayat Transaksi dan Carousel -->
        <div class="page active" id="homePage">
            <header>
                <button class="fullscreen-btn" id="fullscreenBtn"><i class="fas fa-expand"></i></button>
                <h1 id="headerTitle">QRISku</h1>
                <p id="headerSubtitle">Pembayaran QRIS Mudah dan Cepat</p>
                <button class="profile-btn" id="profileBtn"><i class="fas fa-user"></i></button>
            </header>

            <!-- Section Pembayaran dengan Carousel dan Riwayat Transaksi -->
            <div class="payment-section">
                <div class="payment-header">
                    <h2>Riwayat Transaksi Terbaru</h2>
                </div>

                <!-- Carousel untuk gambar yang dapat diatur di pengaturan -->
                <div class="carousel-container">
                    <div class="carousel" id="carousel">
                        <!-- Slide akan diisi oleh JavaScript -->
                    </div>
                    <button class="carousel-nav carousel-prev" id="carouselPrev">
                        <i class="fas fa-chevron-left"></i>
                    </button>
                    <button class="carousel-nav carousel-next" id="carouselNext">
                        <i class="fas fa-chevron-right"></i>
                    </button>
                    <div class="carousel-indicators" id="carouselIndicators">
                        <!-- Indicator akan diisi oleh JavaScript -->
                    </div>
                </div>

                <!-- Riwayat Transaksi Terbaru -->
                <div class="recent-transactions">
                    <h3>Transaksi Berhasil Terbaru</h3>
                    <div id="recentTransactionsList">
                        <!-- Daftar transaksi akan diisi oleh JavaScript -->
                    </div>
                </div>
            </div>

            <!-- Input Section untuk nominal pembayaran -->
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
                    <button class="convert-button" id="convertButton">Buat QR Pembayaran</button>
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

    <!-- Modal Pengaturan yang Diperbaiki dengan Tab Carousel -->
    <div class="settings-modal" id="settingsModal">
        <div class="settings-content">
            <button class="close-btn" id="closeSettings"><i class="fas fa-times"></i></button>
            <h2>Pengaturan QRIS</h2>
            
            <div class="settings-tabs">
                <button class="tab-btn active" data-tab="qris-settings">QRIS</button>
                <button class="tab-btn" data-tab="text-settings">Teks</button>
                <button class="tab-btn" data-tab="carousel-settings">Carousel</button>
                <button class="tab-btn" data-tab="security-settings">Keamanan</button>
                <button class="tab-btn" data-tab="backup-settings">Backup & Reset</button>
            </div>
            
            <div class="tab-content active" id="qris-settings">
                <div class="form-group">
                    <label for="merchantName">Nama Merchant *</label>
                    <input type="text" id="merchantName" placeholder="Nama Merchant" required>
                    <div class="error-message" id="merchantNameError">Nama merchant harus diisi</div>
                </div>

                <div class="form-group">
                    <label for="qrisStaticCode">Kode QRIS Statis *</label>
                    <textarea class="text-input" id="qrisStaticCode" placeholder="Tempel Kode QRIS statis di sini..." rows="4" required></textarea>
                    <div class="error-message" id="qrisStaticCodeError">Kode QRIS statis harus diisi</div>
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
                
                <div class="button-group">
                    <button class="clear-button" id="resetQrisButton">Reset QRIS</button>
                    <button class="save-button" id="saveQrisButton">Simpan Pengaturan QRIS</button>
                </div>
            </div>
            
            <div class="tab-content" id="text-settings">
                <div class="form-group">
                    <label for="headerTitleInput">Judul Halaman *</label>
                    <input type="text" id="headerTitleInput" placeholder="Judul Halaman" required>
                    <div class="error-message" id="headerTitleError">Judul halaman harus diisi</div>
                </div>
                
                <div class="form-group">
                    <label for="headerSubtitleInput">Subjudul Halaman *</label>
                    <input type="text" id="headerSubtitleInput" placeholder="Subjudul Halaman" required>
                    <div class="error-message" id="headerSubtitleError">Subjudul halaman harus diisi</div>
                </div>
                
                <div class="form-group">
                    <label for="footerTextInput">Teks Footer *</label>
                    <textarea id="footerTextInput" placeholder="Teks Footer" rows="2" required></textarea>
                    <div class="error-message" id="footerTextError">Teks footer harus diisi</div>
                </div>
                
                <div class="button-group">
                    <button class="reset-button" id="resetTextButton">Reset ke Default</button>
                    <button class="save-button" id="saveTextButton">Simpan Pengaturan Teks</button>
                </div>
            </div>
            
            <div class="tab-content" id="carousel-settings">
                <div class="form-group">
                    <label>Gambar Carousel</label>
                    <p style="font-size: 0.875rem; color: #64748b; margin-bottom: 10px;">Upload gambar untuk ditampilkan di carousel beranda</p>
                    <div class="drop-area" id="carouselDropArea">
                        <i class="fas fa-cloud-upload-alt"></i>
                        <p>Drag & drop gambar carousel di sini atau klik untuk memilih file</p>
                        <input type="file" id="carouselInput" accept="image/*" style="display: none;" multiple>
                    </div>
                </div>
                
                <div class="carousel-preview" id="carouselPreview" style="display: none;">
                    <p>Preview Carousel:</p>
                    <div id="carouselPreviewImages"></div>
                </div>
                
                <div class="button-group">
                    <button class="clear-button" id="resetCarouselButton">Reset Carousel</button>
                    <button class="save-button" id="saveCarouselButton">Simpan Pengaturan Carousel</button>
                </div>
            </div>
            
            <div class="tab-content" id="security-settings">
                <div class="form-group">
                    <label for="currentPassword">Sandi Saat Ini *</label>
                    <input type="password" id="currentPassword" placeholder="Masukkan sandi saat ini" required>
                    <div class="error-message" id="currentPasswordError">Sandi saat ini harus diisi</div>
                </div>
                
                <div class="form-group">
                    <label for="newPassword">Sandi Baru (6 digit) *</label>
                    <input type="password" id="newPassword" placeholder="Masukkan sandi baru (6 digit)" maxlength="6" required>
                    <div class="error-message" id="newPasswordError">Sandi baru harus 6 digit</div>
                    <div class="password-strength" id="passwordStrength">
                        <div class="password-strength-bar"></div>
                    </div>
                </div>
                
                <div class="form-group">
                    <label for="confirmPassword">Konfirmasi Sandi Baru *</label>
                    <input type="password" id="confirmPassword" placeholder="Konfirmasi sandi baru" maxlength="6" required>
                    <div class="error-message" id="confirmPasswordError">Konfirmasi sandi tidak sesuai</div>
                </div>
                
                <div class="button-group">
                    <button class="reset-button" id="resetPasswordButton">Reset Sandi ke Default</button>
                    <button class="save-button" id="changePasswordButton">Ubah Sandi</button>
                </div>
                <div class="success-message" id="passwordSuccess">Sandi berhasil diubah!</div>
            </div>
            
            <div class="tab-content" id="backup-settings">
                <div class="form-group">
                    <label>Backup Data</label>
                    <p style="font-size: 0.875rem; color: #64748b; margin-bottom: 10px;">Buat cadangan data aplikasi Anda</p>
                    <div class="button-group">
                        <button class="save-button" id="backupDataButton">Backup Data ke JSON</button>
                    </div>
                </div>
                
                <div class="form-group">
                    <label>Restore Data</label>
                    <p style="font-size: 0.875rem; color: #64748b; margin-bottom: 10px;">Pulihkan data dari file backup</p>
                    <div class="drop-area" id="restoreDropArea">
                        <i class="fas fa-file-import"></i>
                        <p>Drag & drop file backup JSON di sini atau klik untuk memilih file</p>
                        <input type="file" id="restoreInput" accept=".json" style="display: none;">
                    </div>
                </div>
                
                <div class="form-group">
                    <label>Reset Aplikasi</label>
                    <p style="font-size: 0.875rem; color: #64748b; margin-bottom: 10px;">Hapus semua data dan reset ke pengaturan default</p>
                    <div class="button-group">
                        <button class="clear-button" id="resetAllButton">Reset Semua Data</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Pembayaran -->
    <div class="payment-modal" id="paymentModal">
        <div class="payment-content">
            <div class="payment-header">
                <div class="payment-time" id="paymentTime">17:05</div>
                <div class="payment-close" id="closePaymentModal"><i class="fas fa-times"></i></button>
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
            carouselImages: JSON.parse(localStorage.getItem('carouselImages')) || [],
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
            calculatorHistory: JSON.parse(localStorage.getItem('calculatorHistory')) || [],
            // State carousel
            currentCarouselIndex: 0,
            carouselInterval: null
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
            },
            
            showError: (elementId, message) => {
                const errorElement = document.getElementById(elementId);
                errorElement.textContent = message;
                errorElement.style.display = 'block';
            },
            
            hideError: (elementId) => {
                const errorElement = document.getElementById(elementId);
                errorElement.style.display = 'none';
            },
            
            validateRequiredFields: (fields) => {
                let isValid = true;
                
                fields.forEach(field => {
                    if (!field.input.value.trim()) {
                        helpers.showError(field.errorId, field.message);
                        isValid = false;
                    } else {
                        helpers.hideError(field.errorId);
                    }
                });
                
                return isValid;
            },
            
            checkPasswordStrength: (password) => {
                let strength = 0;
                
                if (password.length >= 6) strength++;
                if (password.match(/[a-z]/) && password.match(/[A-Z]/)) strength++;
                if (password.match(/\d/)) strength++;
                if (password.match(/[^a-zA-Z\d]/)) strength++;
                
                const passwordStrength = document.getElementById('passwordStrength');
                passwordStrength.className = 'password-strength';
                if (strength > 0) {
                    if (strength < 2) passwordStrength.classList.add('weak');
                    else if (strength < 4) passwordStrength.classList.add('medium');
                    else passwordStrength.classList.add('strong');
                }
            }
        };

        // Fungsi Carousel
        const carousel = {
            init: () => {
                carousel.render();
                carousel.startAutoSlide();
                
                // Event listeners untuk navigasi carousel
                document.getElementById('carouselPrev').addEventListener('click', carousel.prevSlide);
                document.getElementById('carouselNext').addEventListener('click', carousel.nextSlide);
                
                // Touch events untuk swipe
                const carouselElement = document.getElementById('carousel');
                let startX = 0;
                let endX = 0;
                
                carouselElement.addEventListener('touchstart', (e) => {
                    startX = e.touches[0].clientX;
                });
                
                carouselElement.addEventListener('touchmove', (e) => {
                    endX = e.touches[0].clientX;
                });
                
                carouselElement.addEventListener('touchend', () => {
                    const diff = startX - endX;
                    if (Math.abs(diff) > 50) { // Minimum swipe distance
                        if (diff > 0) {
                            carousel.nextSlide();
                        } else {
                            carousel.prevSlide();
                        }
                    }
                });
            },
            
            render: () => {
                const carouselElement = document.getElementById('carousel');
                const indicatorsElement = document.getElementById('carouselIndicators');
                
                carouselElement.innerHTML = '';
                indicatorsElement.innerHTML = '';
                
                if (state.carouselImages.length === 0) {
                    // Default slide jika tidak ada gambar
                    const slide = document.createElement('div');
                    slide.className = 'carousel-slide active';
                    slide.innerHTML = `
                        <div style="width: 100%; height: 100%; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, #1e3a8a, #3b82f6); color: white;">
                            <div style="text-align: center;">
                                <i class="fas fa-images" style="font-size: 2rem; margin-bottom: 10px;"></i>
                                <p>Upload gambar carousel di pengaturan</p>
                            </div>
                        </div>
                    `;
                    carouselElement.appendChild(slide);
                    
                    const indicator = document.createElement('span');
                    indicator.className = 'carousel-indicator active';
                    indicatorsElement.appendChild(indicator);
                } else {
                    state.carouselImages.forEach((image, index) => {
                        const slide = document.createElement('div');
                        slide.className = `carousel-slide ${index === state.currentCarouselIndex ? 'active' : ''}`;
                        slide.innerHTML = `
                            <img src="${image.url}" alt="Carousel Image ${index + 1}" class="carousel-image">
                            <div class="carousel-overlay">
                                <p>${image.title || 'Gambar Promosi'}</p>
                            </div>
                        `;
                        carouselElement.appendChild(slide);
                        
                        const indicator = document.createElement('span');
                        indicator.className = `carousel-indicator ${index === state.currentCarouselIndex ? 'active' : ''}`;
                        indicator.addEventListener('click', () => carousel.goToSlide(index));
                        indicatorsElement.appendChild(indicator);
                    });
                    
                    carousel.updateCarouselPosition();
                }
            },
            
            nextSlide: () => {
                state.currentCarouselIndex = (state.currentCarouselIndex + 1) % state.carouselImages.length;
                carousel.render();
            },
            
            prevSlide: () => {
                state.currentCarouselIndex = state.currentCarouselIndex === 0 ? state.carouselImages.length - 1 : state.currentCarouselIndex - 1;
                carousel.render();
            },
            
            goToSlide: (index) => {
                state.currentCarouselIndex = index;
                carousel.render();
            },
            
            updateCarouselPosition: () => {
                const carouselElement = document.getElementById('carousel');
                carouselElement.style.transform = `translateX(-${state.currentCarouselIndex * 100}%)`;
            },
            
            startAutoSlide: () => {
                // Hentikan interval sebelumnya jika ada
                if (state.carouselInterval) {
                    clearInterval(state.carouselInterval);
                }
                
                // Mulai interval baru hanya jika ada lebih dari 1 gambar
                if (state.carouselImages.length > 1) {
                    state.carouselInterval = setInterval(() => {
                        carousel.nextSlide();
                    }, 5000); // Ganti slide setiap 5 detik
                }
            },
            
            stopAutoSlide: () => {
                if (state.carouselInterval) {
                    clearInterval(state.carouselInterval);
                    state.carouselInterval = null;
                }
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
                
                // Juga update recent transactions di beranda
                render.recentTransactions();
            },
            
            recentTransactions: () => {
                const recentTransactionsList = document.getElementById('recentTransactionsList');
                recentTransactionsList.innerHTML = '';
                
                const confirmedTransactions = state.transactions
                    .filter(t => t.status === 'success')
                    .sort((a, b) => new Date(b.rawDate) - new Date(a.rawDate))
                    .slice(0, 3); // Hanya ambil 3 transaksi terbaru
                
                if (confirmedTransactions.length === 0) {
                    recentTransactionsList.innerHTML = '<div class="no-transactions">Belum ada transaksi yang berhasil</div>';
                    return;
                }
                
                confirmedTransactions.forEach(transaction => {
                    const transactionCard = document.createElement('div');
                    transactionCard.className = 'transaction-card';
                    
                    transactionCard.innerHTML = `
                        <div class="transaction-header">
                            <div class="transaction-id">${transaction.id}</div>
                            <div class="transaction-date">${transaction.date}</div>
                        </div>
                        <div class="transaction-amount">Rp ${helpers.formatNumber(transaction.amount)}</div>
                    `;
                    
                    recentTransactionsList.appendChild(transactionCard);
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
            },
            
            carouselPreview: () => {
                const carouselPreview = document.getElementById('carouselPreview');
                const carouselPreviewImages = document.getElementById('carouselPreviewImages');
                
                carouselPreviewImages.innerHTML = '';
                
                if (state.carouselImages.length === 0) {
                    carouselPreview.style.display = 'none';
                    return;
                }
                
                carouselPreview.style.display = 'block';
                
                state.carouselImages.forEach((image, index) => {
                    const imgContainer = document.createElement('div');
                    imgContainer.style.marginBottom = '10px';
                    imgContainer.innerHTML = `
                        <img src="${image.url}" alt="Carousel Preview ${index + 1}" style="max-width: 100px; height: auto; border-radius: 4px; margin-right: 10px;">
                        <span>Gambar ${index + 1}</span>
                        <button class="clear-button" data-index="${index}" style="padding: 4px 8px; margin-left: 10px;">Hapus</button>
                    `;
                    
                    imgContainer.querySelector('button').addEventListener('click', (e) => {
                        const indexToRemove = parseInt(e.target.getAttribute('data-index'));
                        state.carouselImages.splice(indexToRemove, 1);
                        settings.saveCarouselSettings();
                    });
                    
                    carouselPreviewImages.appendChild(imgContainer);
                });
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
                } else if (pageId === 'homePage') {
                    render.recentTransactions();
                    carousel.init();
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

        // Fungsi pengaturan
        const settings = {
            initializeSettings: () => {
                // Set nilai default
                document.getElementById('merchantName').value = state.savedMerchant;
                document.getElementById('headerTitleInput').value = state.savedHeaderTitle;
                document.getElementById('headerSubtitleInput').value = state.savedHeaderSubtitle;
                document.getElementById('footerTextInput').value = state.savedFooterText;
                document.getElementById('qrisStaticCode').value = state.savedQrisStaticCode;
                
                if (state.savedQrisImage) {
                    document.getElementById('qrisPreview').style.display = 'block';
                    document.getElementById('qrisPreviewImg').src = state.savedQrisImage;
                }
                
                render.carouselPreview();
            },
            
            saveQrisSettings: () => {
                const fields = [
                    { input: document.getElementById('merchantName'), errorId: 'merchantNameError', message: 'Nama merchant harus diisi' },
                    { input: document.getElementById('qrisStaticCode'), errorId: 'qrisStaticCodeError', message: 'Kode QRIS statis harus diisi' }
                ];
                
                if (!helpers.validateRequiredFields(fields)) return;
                
                state.savedMerchant = document.getElementById('merchantName').value.trim();
                state.savedQrisStaticCode = document.getElementById('qrisStaticCode').value.trim();
                
                localStorage.setItem('merchantName', state.savedMerchant);
                localStorage.setItem('qrisStaticCode', state.savedQrisStaticCode);
                
                if (state.savedQrisImage) {
                    localStorage.setItem('qrisImage', state.savedQrisImage);
                }
                
                // Update tampilan
                document.getElementById('paymentMerchant').textContent = state.savedMerchant;
                
                alert('Pengaturan QRIS berhasil disimpan!');
            },
            
            saveTextSettings: () => {
                const fields = [
                    { input: document.getElementById('headerTitleInput'), errorId: 'headerTitleError', message: 'Judul halaman harus diisi' },
                    { input: document.getElementById('headerSubtitleInput'), errorId: 'headerSubtitleError', message: 'Subjudul halaman harus diisi' },
                    { input: document.getElementById('footerTextInput'), errorId: 'footerTextError', message: 'Teks footer harus diisi' }
                ];
                
                if (!helpers.validateRequiredFields(fields)) return;
                
                state.savedHeaderTitle = document.getElementById('headerTitleInput').value.trim();
                state.savedHeaderSubtitle = document.getElementById('headerSubtitleInput').value.trim();
                state.savedFooterText = document.getElementById('footerTextInput').value.trim();
                
                localStorage.setItem('headerTitle', state.savedHeaderTitle);
                localStorage.setItem('headerSubtitle', state.savedHeaderSubtitle);
                localStorage.setItem('footerText', state.savedFooterText);
                
                // Update tampilan
                document.getElementById('headerTitle').textContent = state.savedHeaderTitle;
                document.getElementById('headerSubtitle').textContent = state.savedHeaderSubtitle;
                document.getElementById('footerText').textContent = state.savedFooterText;
                document.getElementById('paymentTitle').textContent = state.savedHeaderTitle;
                
                alert('Pengaturan teks berhasil disimpan!');
            },
            
            saveCarouselSettings: () => {
                localStorage.setItem('carouselImages', JSON.stringify(state.carouselImages));
                render.carouselPreview();
                carousel.init();
                alert('Pengaturan carousel berhasil disimpan!');
            },
            
            changePassword: () => {
                const fields = [
                    { input: document.getElementById('currentPassword'), errorId: 'currentPasswordError', message: 'Sandi saat ini harus diisi' },
                    { input: document.getElementById('newPassword'), errorId: 'newPasswordError', message: 'Sandi baru harus 6 digit' },
                    { input: document.getElementById('confirmPassword'), errorId: 'confirmPasswordError', message: 'Konfirmasi sandi harus diisi' }
                ];
                
                if (!helpers.validateRequiredFields(fields)) return;
                
                if (document.getElementById('currentPassword').value !== state.appPassword) {
                    helpers.showError('currentPasswordError', 'Sandi saat ini tidak benar');
                    return;
                }
                
                if (document.getElementById('newPassword').value.length !== 6) {
                    helpers.showError('newPasswordError', 'Sandi baru harus 6 digit');
                    return;
                }
                
                if (document.getElementById('newPassword').value !== document.getElementById('confirmPassword').value) {
                    helpers.showError('confirmPasswordError', 'Konfirmasi sandi tidak sesuai');
                    return;
                }
                
                state.appPassword = document.getElementById('newPassword').value;
                localStorage.setItem('appPassword', state.appPassword);
                
                document.getElementById('passwordSuccess').style.display = 'block';
                setTimeout(() => {
                    document.getElementById('passwordSuccess').style.display = 'none';
                }, 3000);
                
                document.getElementById('currentPassword').value = '';
                document.getElementById('newPassword').value = '';
                document.getElementById('confirmPassword').value = '';
                document.getElementById('passwordStrength').className = 'password-strength';
            },
            
            backupData: () => {
                const data = {
                    merchantName: state.savedMerchant,
                    headerTitle: state.savedHeaderTitle,
                    headerSubtitle: state.savedHeaderSubtitle,
                    footerText: state.savedFooterText,
                    qrisStaticCode: state.savedQrisStaticCode,
                    qrisImage: state.savedQrisImage,
                    carouselImages: state.carouselImages,
                    appPassword: state.appPassword,
                    phoneHistory: state.phoneHistoryData,
                    transactions: state.transactions,
                    debts: state.debts,
                    inventory: state.inventory,
                    calculatorHistory: state.calculatorHistory,
                    backupDate: new Date().toISOString()
                };
                
                const dataStr = JSON.stringify(data, null, 2);
                const blob = new Blob([dataStr], { type: 'application/json' });
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                link.href = url;
                link.download = `qrisku-backup-${new Date().toISOString().split('T')[0]}.json`;
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                
                alert('Backup data berhasil dibuat!');
            },
            
            restoreData: (file) => {
                const reader = new FileReader();
                reader.onload = function(event) {
                    try {
                        const data = JSON.parse(event.target.result);
                        
                        if (confirm('Apakah Anda yakin ingin memulihkan data dari backup? Data saat ini akan diganti.')) {
                            if (data.merchantName) {
                                state.savedMerchant = data.merchantName;
                                localStorage.setItem('merchantName', data.merchantName);
                            }
                            
                            if (data.headerTitle) {
                                state.savedHeaderTitle = data.headerTitle;
                                localStorage.setItem('headerTitle', data.headerTitle);
                            }
                            
                            if (data.headerSubtitle) {
                                state.savedHeaderSubtitle = data.headerSubtitle;
                                localStorage.setItem('headerSubtitle', data.headerSubtitle);
                            }
                            
                            if (data.footerText) {
                                state.savedFooterText = data.footerText;
                                localStorage.setItem('footerText', data.footerText);
                            }
                            
                            if (data.qrisStaticCode) {
                                state.savedQrisStaticCode = data.qrisStaticCode;
                                localStorage.setItem('qrisStaticCode', data.qrisStaticCode);
                            }
                            
                            if (data.qrisImage) {
                                state.savedQrisImage = data.qrisImage;
                                localStorage.setItem('qrisImage', data.qrisImage);
                            }
                            
                            if (data.carouselImages) {
                                state.carouselImages = data.carouselImages;
                                localStorage.setItem('carouselImages', JSON.stringify(data.carouselImages));
                            }
                            
                            if (data.appPassword) {
                                state.appPassword = data.appPassword;
                                localStorage.setItem('appPassword', data.appPassword);
                            }
                            
                            if (data.phoneHistory) {
                                state.phoneHistoryData = data.phoneHistory;
                                localStorage.setItem('phoneHistory', JSON.stringify(data.phoneHistory));
                            }
                            
                            if (data.transactions) {
                                state.transactions = data.transactions;
                                localStorage.setItem('transactions', JSON.stringify(data.transactions));
                            }
                            
                            if (data.debts) {
                                state.debts = data.debts;
                                localStorage.setItem('debts', JSON.stringify(data.debts));
                            }
                            
                            if (data.inventory) {
                                state.inventory = data.inventory;
                                localStorage.setItem('inventory', JSON.stringify(data.inventory));
                            }
                            
                            if (data.calculatorHistory) {
                                state.calculatorHistory = data.calculatorHistory;
                                localStorage.setItem('calculatorHistory', JSON.stringify(data.calculatorHistory));
                            }
                            
                            // Refresh tampilan
                            settings.initializeSettings();
                            render.phoneHistory();
                            render.transactions();
                            render.debts();
                            render.inventory();
                            render.shoppingList();
                            calculator.renderHistory();
                            carousel.init();
                            
                            alert('Data berhasil dipulihkan dari backup!');
                        }
                    } catch (error) {
                        alert('Terjadi kesalahan saat membaca file backup: ' + error.message);
                    }
                };
                reader.readAsText(file);
            },
            
            resetAllData: () => {
                if (confirm('PERINGATAN: Apakah Anda yakin ingin mereset semua data? Tindakan ini tidak dapat dibatalkan!')) {
                    if (confirm('Semua data akan dihapus dan aplikasi akan dikembalikan ke pengaturan default. Lanjutkan?')) {
                        localStorage.clear();
                        alert('Semua data telah direset. Halaman akan dimuat ulang.');
                        location.reload();
                    }
                }
            }
        };

        // Inisialisasi aplikasi
        const initializeApp = () => {
            // Set nilai default
            settings.initializeSettings();
            
            // Update tampilan
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
            carousel.init();
            
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
            
            // Pengaturan tabs
            document.querySelectorAll('.settings-tabs .tab-btn').forEach(tabBtn => {
                tabBtn.addEventListener('click', () => {
                    const tabId = tabBtn.getAttribute('data-tab');
                    
                    document.querySelectorAll('.settings-tabs .tab-btn').forEach(btn => btn.classList.remove('active'));
                    document.querySelectorAll('#settingsModal .tab-content').forEach(content => content.classList.remove('active'));
                    
                    tabBtn.classList.add('active');
                    document.getElementById(tabId).classList.add('active');
                });
            });
            
            // Pengaturan QRIS
            document.getElementById('qrisDropArea').addEventListener('click', () => {
                document.getElementById('qrisInput').click();
            });
            
            document.getElementById('qrisInput').addEventListener('change', (e) => {
                if (e.target.files.length) {
                    const reader = new FileReader();
                    reader.onload = function(event) {
                        state.savedQrisImage = event.target.result;
                        document.getElementById('qrisPreview').style.display = 'block';
                        document.getElementById('qrisPreviewImg').src = state.savedQrisImage;
                    };
                    reader.readAsDataURL(e.target.files[0]);
                }
            });
            
            document.getElementById('resetQrisButton').addEventListener('click', () => {
                if (confirm('Apakah Anda yakin ingin menghapus gambar QRIS?')) {
                    state.savedQrisImage = '';
                    document.getElementById('qrisPreview').style.display = 'none';
                }
            });
            
            document.getElementById('saveQrisButton').addEventListener('click', settings.saveQrisSettings);
            
            // Pengaturan teks
            document.getElementById('resetTextButton').addEventListener('click', () => {
                if (confirm('Apakah Anda yakin ingin mengembalikan teks ke default?')) {
                    document.getElementById('headerTitleInput').value = 'QRISku';
                    document.getElementById('headerSubtitleInput').value = 'Pembayaran QRIS Mudah dan Cepat';
                    document.getElementById('footerTextInput').value = '© 2025 QRISku - Pembayaran QRIS';
                }
            });
            
            document.getElementById('saveTextButton').addEventListener('click', settings.saveTextSettings);
            
            // Pengaturan carousel
            document.getElementById('carouselDropArea').addEventListener('click', () => {
                document.getElementById('carouselInput').click();
            });
            
            document.getElementById('carouselInput').addEventListener('change', (e) => {
                if (e.target.files.length) {
                    const files = Array.from(e.target.files);
                    files.forEach(file => {
                        const reader = new FileReader();
                        reader.onload = function(event) {
                            state.carouselImages.push({
                                url: event.target.result,
                                title: file.name.replace(/\.[^/.]+$/, "") // Hapus ekstensi file
                            });
                            render.carouselPreview();
                        };
                        reader.readAsDataURL(file);
                    });
                }
            });
            
            document.getElementById('resetCarouselButton').addEventListener('click', () => {
                if (confirm('Apakah Anda yakin ingin menghapus semua gambar carousel?')) {
                    state.carouselImages = [];
                    settings.saveCarouselSettings();
                }
            });
            
            document.getElementById('saveCarouselButton').addEventListener('click', settings.saveCarouselSettings);
            
            // Pengaturan keamanan
            document.getElementById('newPassword').addEventListener('input', () => {
                helpers.checkPasswordStrength(document.getElementById('newPassword').value);
            });
            
            document.getElementById('resetPasswordButton').addEventListener('click', () => {
                if (confirm('Apakah Anda yakin ingin mengembalikan sandi ke default (131313)?')) {
                    state.appPassword = '131313';
                    localStorage.setItem('appPassword', state.appPassword);
                    alert('Sandi telah direset ke default: 131313');
                }
            });
            
            document.getElementById('changePasswordButton').addEventListener('click', settings.changePassword);
            
            // Backup & restore
            document.getElementById('backupDataButton').addEventListener('click', settings.backupData);
            
            document.getElementById('restoreDropArea').addEventListener('click', () => {
                document.getElementById('restoreInput').click();
            });
            
            document.getElementById('restoreInput').addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (file) {
                    settings.restoreData(file);
                }
            });
            
            document.getElementById('resetAllButton').addEventListener('click', settings.resetAllData);
            
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

                        // Update tampilan transaksi
                        render.transactions();
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
            
            // Tutup modal pengaturan
            document.getElementById('closeSettings').addEventListener('click', () => {
                document.getElementById('settingsModal').style.display = 'none';
            });
        });
    </script>
</body>
</html>
