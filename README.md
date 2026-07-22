# pharmastock-pk
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PharmaStock PK - Executive Management System</title>
    <!-- Tesseract.js for Browser OCR Scanning -->
    <script src="https://cdn.jsdelivr.net/npm/tesseract.js@5/dist/tesseract.min.js"></script>
    <style>
        :root {
            --primary: #0284c7;
            --primary-dark: #0369a1;
            --primary-light: #e0f2fe;
            --accent: #6366f1;
            --bg-main: #f1f5f9;
            --surface: #ffffff;
            --sidebar-bg: #0f172a;
            --sidebar-text: #94a3b8;
            --sidebar-active: #38bdf8;
            --text-dark: #0f172a;
            --text-muted: #64748b;
            --border: #e2e8f0;
            --danger: #ef4444;
            --danger-bg: #fef2f2;
            --warning: #f59e0b;
            --warning-bg: #fffbeb;
            --success: #10b981;
            --success-bg: #ecfdf5;
            --shadow-sm: 0 1px 3px rgba(0,0,0,0.05);
            --shadow-md: 0 4px 6px -1px rgba(0,0,0,0.1);
            --shadow-lg: 0 10px 15px -3px rgba(0,0,0,0.1);
            --radius-md: 10px;
            --radius-lg: 16px;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', system-ui, -apple-system, sans-serif; }
        body { background-color: var(--bg-main); color: var(--text-dark); line-height: 1.5; min-height: 100vh; }

        /* Auth Screen */
        .auth-container { display: flex; justify-content: center; align-items: center; min-height: 100vh; background: linear-gradient(135deg, #0f172a 0%, #1e293b 50%, #0284c7 100%); }
        .auth-card { background: var(--surface); padding: 3rem 2.5rem; border-radius: var(--radius-lg); box-shadow: var(--shadow-lg); width: 100%; max-width: 420px; text-align: center; }
        .auth-card .logo-icon { font-size: 3rem; margin-bottom: 0.5rem; display: inline-block; }
        .auth-card h2 { font-size: 1.6rem; color: var(--text-dark); margin-bottom: 0.25rem; font-weight: 700; }
        .auth-card p { color: var(--text-muted); font-size: 0.9rem; margin-bottom: 2rem; }
        
        /* Layout Structure */
        .app-container { display: none; min-height: 100vh; flex-direction: row; }
        
        /* Sidebar */
        aside { width: 285px; background: var(--sidebar-bg); color: var(--sidebar-text); display: flex; flex-direction: column; justify-content: space-between; flex-shrink: 0; }
        .brand-header { padding: 1.75rem 1.5rem; border-bottom: 1px solid rgba(255,255,255,0.08); display: flex; align-items: center; gap: 0.75rem; }
        .brand-title { color: #ffffff; font-size: 1.2rem; font-weight: 700; letter-spacing: -0.5px; }
        .brand-subtitle { font-size: 0.7rem; color: var(--sidebar-active); text-transform: uppercase; font-weight: 600; letter-spacing: 1px; }

        nav.nav-menu { padding: 1rem 0.75rem; display: flex; flex-direction: column; gap: 0.35rem; flex: 1; overflow-y: auto; }
        .nav-btn { display: flex; align-items: center; gap: 0.85rem; width: 100%; padding: 0.8rem 1rem; border: none; background: transparent; color: var(--sidebar-text); font-size: 0.88rem; font-weight: 600; border-radius: var(--radius-md); cursor: pointer; transition: all 0.2s ease; text-align: left; }
        .nav-btn:hover { background: rgba(255,255,255,0.06); color: #ffffff; }
        .nav-btn.active { background: linear-gradient(90deg, #0284c7 0%, #0369a1 100%); color: #ffffff; box-shadow: 0 4px 12px rgba(2, 132, 199, 0.3); }

        .sidebar-footer { padding: 1.25rem; border-top: 1px solid rgba(255,255,255,0.08); }
        .user-badge { display: flex; align-items: center; justify-content: space-between; background: rgba(255,255,255,0.05); padding: 0.75rem 1rem; border-radius: var(--radius-md); }
        .user-details { font-size: 0.85rem; }
        .user-name { color: #ffffff; font-weight: 600; display: block; }
        .user-role { color: var(--sidebar-active); font-size: 0.75rem; }

        /* Main Workspace */
        main { flex: 1; display: flex; flex-direction: column; overflow-x: hidden; }
        header.top-header { background: var(--surface); border-bottom: 1px solid var(--border); padding: 1.25rem 2.5rem; display: flex; justify-content: space-between; align-items: center; box-shadow: var(--shadow-sm); }
        .page-title-text { font-size: 1.35rem; font-weight: 700; color: var(--text-dark); }

        .content-area { padding: 2rem 2.5rem; overflow-y: auto; flex: 1; }
        .section { display: none; animation: fadeIn 0.3s ease-in-out; }
        .section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

        /* Cards */
        .card { background: var(--surface); border-radius: var(--radius-lg); border: 1px solid var(--border); padding: 1.75rem; margin-bottom: 1.75rem; box-shadow: var(--shadow-sm); transition: box-shadow 0.2s ease; }
        .card:hover { box-shadow: var(--shadow-md); }
        .card-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1.25rem; flex-wrap: wrap; gap: 1rem; }
        .card-title { font-size: 1.1rem; font-weight: 700; color: var(--text-dark); display: flex; align-items: center; gap: 0.5rem; }

        /* KPI Grid */
        .kpi-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(230px, 1fr)); gap: 1.25rem; margin-bottom: 1.75rem; }
        .kpi-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius-lg); padding: 1.5rem; display: flex; align-items: center; gap: 1.25rem; box-shadow: var(--shadow-sm); }
        .kpi-icon { width: 54px; height: 54px; border-radius: 14px; display: flex; align-items: center; justify-content: center; font-size: 1.6rem; flex-shrink: 0; }
        .kpi-icon.blue { background: var(--primary-light); color: var(--primary); }
        .kpi-icon.green { background: var(--success-bg); color: var(--success); }
        .kpi-icon.orange { background: var(--warning-bg); color: var(--warning); }
        .kpi-icon.purple { background: #f3e8ff; color: #9333ea; }
        .kpi-meta p { font-size: 0.82rem; font-weight: 600; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.5px; }
        .kpi-value { font-size: 1.5rem; font-weight: 800; color: var(--text-dark); margin-top: 0.2rem; }

        /* Forms & Inputs */
        .form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(210px, 1fr)); gap: 1.25rem; margin-bottom: 1.25rem; }
        .form-group { display: flex; flex-direction: column; gap: 0.45rem; }
        .form-group label { font-size: 0.85rem; font-weight: 600; color: var(--text-dark); }
        input, select, textarea { padding: 0.75rem 1rem; border: 1px solid var(--border); border-radius: var(--radius-md); font-size: 0.95rem; background: #f8fafc; transition: all 0.2s; outline: none; }
        input:focus, select:focus, textarea:focus { border-color: var(--primary); background: #ffffff; box-shadow: 0 0 0 3px rgba(2, 132, 199, 0.15); }

        /* Buttons */
        .btn { background: var(--primary); color: #ffffff; border: none; padding: 0.75rem 1.5rem; border-radius: var(--radius-md); font-weight: 600; font-size: 0.9rem; cursor: pointer; transition: all 0.2s ease; display: inline-flex; align-items: center; justify-content: center; gap: 0.5rem; }
        .btn:hover { background: var(--primary-dark); transform: translateY(-1px); box-shadow: var(--shadow-md); }
        .btn-danger { background: var(--danger); }
        .btn-danger:hover { background: #dc2626; }
        .btn-secondary { background: var(--text-muted); }
        .btn-secondary:hover { background: #475569; }
        .btn-success { background: var(--success); }
        .btn-success:hover { background: #059669; }
        .btn-warning { background: var(--warning); color: #fff; }
        .btn-warning:hover { background: #d97706; }

        /* Tables */
        .table-responsive { width: 100%; overflow-x: auto; border-radius: var(--radius-md); }
        table { width: 100%; border-collapse: collapse; text-align: left; }
        th { background: #f8fafc; padding: 0.9rem 1.2rem; font-size: 0.82rem; font-weight: 700; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.5px; border-bottom: 2px solid var(--border); }
        td { padding: 1rem 1.2rem; border-bottom: 1px solid var(--border); font-size: 0.92rem; color: var(--text-dark); vertical-align: middle; }
        tr:hover td { background-color: #f8fafc; }

        /* Badges */
        .badge { padding: 0.35rem 0.75rem; border-radius: 20px; font-size: 0.78rem; font-weight: 700; display: inline-block; }
        .badge-danger { background: var(--danger-bg); color: var(--danger); }
        .badge-warning { background: var(--warning-bg); color: var(--warning); }
        .badge-success { background: var(--success-bg); color: var(--success); }
        .badge-info { background: var(--primary-light); color: var(--primary); }

        /* Modal */
        .modal-overlay { display: none; position: fixed; top:0; left:0; width:100%; height:100%; background: rgba(15, 23, 42, 0.6); backdrop-filter: blur(4px); justify-content:center; align-items:center; z-index:1000; }
        .modal { background: var(--surface); padding: 2rem; border-radius: var(--radius-lg); width: 90%; max-width: 460px; box-shadow: var(--shadow-lg); text-align: center; }

        /* Printable Area */
        .printable-area { display: none; padding: 2rem; background: white; border: 1px solid var(--border); margin-top: 1rem; }
        #scanner-preview { max-width: 100%; max-height: 250px; border-radius: var(--radius-md); margin-top: 1rem; display: none; border: 1px solid var(--border); }
        #ocr-loading { display: none; color: var(--primary-dark); font-weight: 600; margin-top: 1rem; }

        @media print {
            body * { visibility: hidden; }
            #printable-receipt, #printable-receipt *, 
            #printable-report, #printable-report *, 
            #printable-po, #printable-po * { visibility: visible; }
            .printable-area { position: absolute; left: 0; top: 0; width: 100%; display: block !important; }
        }
    </style>
</head>
<body>

    <!-- Auth Screen -->
    <div id="auth-screen" class="auth-container">
        <div class="auth-card">
            <span class="logo-icon">💊</span>
            <h2>PharmaStock PK</h2>
            <p>Enter your User or Worker ID to continue</p>
            <form id="login-form">
                <div class="form-group" style="margin-bottom: 1rem; text-align: left;">
                    <label>User / Worker ID</label>
                    <input type="text" id="user-id" required placeholder="e.g., PHARM-9901 or WRK-1001">
                </div>
                <div class="form-group" style="margin-bottom: 1.75rem; text-align: left;">
                    <label>Password</label>
                    <input type="password" id="password" required placeholder="••••••••">
                </div>
                <button type="submit" class="btn" style="width: 100%;">Sign In</button>
            </form>
            <p id="auth-error" style="color: var(--danger); font-size:0.85rem; margin-top:1.25rem; display:none;"></p>
        </div>
    </div>

    <!-- Application View -->
    <div id="app-screen" class="app-container">
        <aside>
            <div>
                <div class="brand-header">
                    <span style="font-size: 1.6rem;">💊</span>
                    <div>
                        <div class="brand-title">PharmaStock PK</div>
                        <div class="brand-subtitle">Rupees (PKR) Edition</div>
                    </div>
                </div>
                <nav class="nav-menu">
                    <button class="nav-btn active" id="btn-nav-dashboard" onclick="switchSection('dashboard', 'btn-nav-dashboard')">📊 Dashboard</button>
                    <button class="nav-btn" id="btn-nav-inventory" onclick="switchSection('inventory', 'btn-nav-inventory')">📦 Inventory Stock</button>
                    <button class="nav-btn" id="btn-nav-pos" onclick="switchSection('pos', 'btn-nav-pos')">🧾 Billing & POS</button>
                    <button class="nav-btn" id="btn-nav-scanner" onclick="switchSection('scanner', 'btn-nav-scanner')">📷 Document Auto-Scan</button>
                    <button class="nav-btn" id="btn-nav-supply" onclick="switchSection('supply', 'btn-nav-supply')">🚚 Supply Orders</button>
                    <button class="nav-btn" id="btn-nav-orders" onclick="switchSection('orders', 'btn-nav-orders')">🔍 Order Search / Missing Stock</button>
                    <button class="nav-btn" id="btn-nav-reports" onclick="switchSection('reports', 'btn-nav-reports')">📈 Daily Sales Reports</button>
                    <!-- Dedicated Worker ID Tab ALWAYS VISIBLE -->
                    <button class="nav-btn" id="worker-nav-btn" onclick="switchSection('worker-gen', 'worker-nav-btn')">🆔 Worker ID Generation & Management</button>
                </nav>
            </div>
            <div class="sidebar-footer">
                <div class="user-badge">
                    <div class="user-details">
                        <span class="user-name" id="user-display-name">Owner Admin</span>
                        <span class="user-role" id="user-display-role">Parent Admin</span>
                    </div>
                    <button class="btn btn-secondary" id="logout-btn" style="padding: 0.4rem 0.75rem; font-size: 0.8rem;">Logout</button>
                </div>
            </div>
        </aside>

        <main>
            <header class="top-header">
                <div class="page-title-text" id="section-title">Dashboard Overview</div>
                <div style="font-size: 0.85rem; color: var(--text-muted); font-weight: 600;" id="current-date-display"></div>
            </header>

            <div class="content-area">

                <!-- SECTION 0: DASHBOARD -->
                <section id="dashboard" class="section active">
                    <div class="kpi-grid">
                        <div class="kpi-card">
                            <div class="kpi-icon blue">💰</div>
                            <div class="kpi-meta">
                                <p>Today's Sales</p>
                                <div class="kpi-value">Rs. <span id="dash-today-sales">0.00</span></div>
                            </div>
                        </div>
                        <div class="kpi-card">
                            <div class="kpi-icon green">🧾</div>
                            <div class="kpi-meta">
                                <p>Transactions Today</p>
                                <div class="kpi-value" id="dash-today-count">0</div>
                            </div>
                        </div>
                        <div class="kpi-card">
                            <div class="kpi-icon orange">⚠️</div>
                            <div class="kpi-meta">
                                <p>Low Stock Items</p>
                                <div class="kpi-value" id="dash-low-stock">0</div>
                            </div>
                        </div>
                        <div class="kpi-card">
                            <div class="kpi-icon purple">📦</div>
                            <div class="kpi-meta">
                                <p>Total Inventory Items</p>
                                <div class="kpi-value" id="dash-total-items">0</div>
                            </div>
                        </div>
                    </div>

                    <div style="display: grid; grid-template-columns: 2fr 1fr; gap: 1.5rem;">
                        <div class="card">
                            <div class="card-header">
                                <div class="card-title">Recent Transactions</div>
                                <button class="btn btn-secondary" style="padding: 0.4rem 0.8rem; font-size: 0.8rem;" onclick="switchSection('reports', 'btn-nav-reports')">View All</button>
                            </div>
                            <div class="table-responsive">
                                <table>
                                    <thead>
                                        <tr><th>Tx ID</th><th>Time</th><th>Processed By</th><th>Amount</th></tr>
                                    </thead>
                                    <tbody id="dash-recent-sales"></tbody>
                                </table>
                            </div>
                        </div>

                        <div class="card">
                            <div class="card-header">
                                <div class="card-title">⚡ Quick Shortcuts</div>
                            </div>
                            <div style="display: flex; flex-direction: column; gap: 0.8rem;">
                                <button class="btn btn-success" style="width: 100%; justify-content: flex-start;" onclick="switchSection('pos', 'btn-nav-pos')">🧾 Open Billing POS Terminal</button>
                                <button class="btn" style="width: 100%; justify-content: flex-start;" onclick="switchSection('inventory', 'btn-nav-inventory')">📦 Manage Medicine Inventory</button>
                                <button class="btn btn-secondary" id="dash-worker-btn" style="width: 100%; justify-content: flex-start;" onclick="switchSection('worker-gen', 'worker-nav-btn')">🆔 Worker ID Generation & Management</button>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- SECTION 1: INVENTORY MANAGEMENT & SUPPLIER TRACKING -->
                <section id="inventory" class="section">
                    <div class="card">
                        <div class="card-title">🏢 Add / Manage Registered Suppliers & Companies</div>
                        <p style="color:var(--text-muted); font-size:0.85rem; margin-bottom:1rem;">Add supplier or company names here to maintain supplier-wise inventory stock.</p>
                        <form id="add-supplier-form">
                            <div style="display:flex; gap:1rem; align-items:center;">
                                <input type="text" id="supplier-name-input" placeholder="e.g., GlaxoSmithKline, Getz Pharma, Pfizer, Local Vendor" required style="flex:1;">
                                <button type="submit" class="btn btn-success">Add Supplier</button>
                            </div>
                        </form>
                    </div>

                    <div class="card">
                        <div class="card-title" style="margin-bottom: 1.25rem;">➕ Add Medicine to Stock (Supplier-Wise)</div>
                        <form id="add-medicine-form">
                            <div class="form-grid">
                                <div class="form-group"><label>Brand Name</label><input type="text" id="med-name" required placeholder="e.g., Panadol Extra"></div>
                                <div class="form-group"><label>Formula / Generic</label><input type="text" id="med-formula" required placeholder="e.g., Paracetamol"></div>
                                <div class="form-group">
                                    <label>Supplier / Company</label>
                                    <select id="med-supplier" required></select>
                                </div>
                                <div class="form-group"><label>Batch Number</label><input type="text" id="med-batch" required placeholder="e.g., B-901"></div>
                                <div class="form-group"><label>Expiry Date</label><input type="date" id="med-expiry" required></div>
                                <div class="form-group"><label>Stock Quantity</label><input type="number" id="med-qty" min="1" required placeholder="100"></div>
                                <div class="form-group"><label>Unit Price (Rs.)</label><input type="number" id="med-price" step="0.01" min="0" required placeholder="35.00"></div>
                            </div>
                            <button type="submit" class="btn">Save Medicine to Stock</button>
                        </form>
                    </div>

                    <div class="card">
                        <div class="card-header">
                            <div class="card-title">📦 Inventory Stock Table</div>
                            <div style="display:flex; gap:0.75rem;">
                                <select id="filter-supplier-select" onchange="renderInventory()" style="width: 200px;">
                                    <option value="ALL">All Suppliers / Companies</option>
                                </select>
                                <input type="text" id="search-inventory" placeholder="🔍 Search brand or formula..." onkeyup="renderInventory()" style="width: 240px;">
                            </div>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr><th>Brand Name</th><th>Formula</th><th>Supplier/Company</th><th>Batch</th><th>Expiry</th><th>Stock</th><th>Unit Price</th><th>Status</th><th>Actions</th></tr>
                                </thead>
                                <tbody id="inventory-table-body"></tbody>
                            </table>
                        </div>
                    </div>
                </section>

                <!-- SECTION 2: POS & BILLING -->
                <section id="pos" class="section">
                    <div class="card">
                        <div class="card-title" style="margin-bottom: 1.25rem;">🧾 Process New Billing Invoice</div>
                        <div class="form-grid">
                            <div class="form-group">
                                <label>Select Available Medicine</label>
                                <select id="pos-select-med"></select>
                            </div>
                            <div class="form-group">
                                <label>Quantity to Sell</label>
                                <input type="number" id="pos-qty" min="1" value="1">
                            </div>
                        </div>
                        <button class="btn btn-success" onclick="addToCart()">➕ Add Item to Cart</button>
                    </div>

                    <div class="card">
                        <div class="card-title" style="margin-bottom: 1rem;">🛒 Active Checkout Cart</div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr><th>Item Name</th><th>Supplier</th><th>Qty</th><th>Unit Price</th><th>Total</th></tr>
                                </thead>
                                <tbody id="cart-table-body"></tbody>
                            </table>
                        </div>
                        <br>
                        <div style="display: flex; justify-content: space-between; align-items: center; border-top: 1px solid var(--border); padding-top: 1.25rem;">
                            <div>
                                <p style="color: var(--text-muted); font-weight: 600;">Subtotal: Rs. <span id="cart-subtotal">0.00</span></p>
                                <p style="color: var(--text-muted); font-weight: 600;">GST (17%): Rs. <span id="cart-tax">0.00</span></p>
                                <h3 style="color: var(--text-dark); margin-top: 0.2rem;">Grand Total: Rs. <span id="cart-total">0.00</span></h3>
                            </div>
                            <button class="btn" style="padding: 0.85rem 1.75rem;" onclick="generateInvoice()">Checkout, Subtract Stock & Print Receipt</button>
                        </div>
                    </div>

                    <div id="printable-receipt" class="printable-area">
                        <h2 style="text-align: center;">PHARMACY RECEIPT</h2>
                        <p style="text-align: center; margin-bottom: 1rem; color: var(--text-muted);">Date: <span id="bill-date"></span></p>
                        <hr><br>
                        <div id="bill-items-list"></div>
                        <br><hr><br>
                        <p style="display:flex; justify-content:space-between;"><span>Subtotal:</span> <span>Rs. <span id="bill-subtotal">0.00</span></span></p>
                        <p style="display:flex; justify-content:space-between;"><span>GST (17%):</span> <span>Rs. <span id="bill-tax">0.00</span></span></p>
                        <h3>Grand Total: Rs. <span id="bill-grand-total">0.00</span></h3>
                        <p style="text-align: center; margin-top: 2rem; font-size: 0.8rem;">Thank you for your visit!</p>
                    </div>
                </section>

                <!-- SECTION 3: DOCUMENT OCR AUTO-SCAN -->
                <section id="scanner" class="section">
                    <div class="card">
                        <div class="card-title">📷 Invoice & Document Scanner</div>
                        <p style="color: var(--text-muted); font-size: 0.88rem; margin-top: 0.25rem;">Upload an invoice or document image to automatically parse details using AI OCR technology.</p><br>
                        <input type="file" id="ocr-file-input" accept="image/*">
                        <div id="ocr-loading">⏳ AI OCR scanning document... Please wait.</div>
                        <img id="scanner-preview" alt="Document Preview"/>
                    </div>

                    <div class="card" id="ocr-result-card" style="display:none;">
                        <div class="card-title">Review Extracted Scanned Details</div><br>
                        <form id="ocr-edit-form">
                            <div class="form-grid">
                                <div class="form-group"><label>Brand Name</label><input type="text" id="ocr-med-name" required></div>
                                <div class="form-group"><label>Formula / Generic</label><input type="text" id="ocr-med-formula" required></div>
                                <div class="form-group"><label>Supplier / Company</label><select id="ocr-med-supplier" required></select></div>
                                <div class="form-group"><label>Batch Number</label><input type="text" id="ocr-med-batch" required></div>
                                <div class="form-group"><label>Expiry Date</label><input type="date" id="ocr-med-expiry" required></div>
                                <div class="form-group"><label>Stock Qty</label><input type="number" id="ocr-med-qty" min="1" required></div>
                                <div class="form-group"><label>Unit Price (Rs.)</label><input type="number" id="ocr-med-price" step="0.01" min="0" required></div>
                            </div>
                            <div class="form-group" style="margin-bottom:1.25rem;">
                                <label>Raw Extracted Text</label>
                                <textarea id="ocr-raw-text" rows="3" readonly style="background:#f1f5f9; color:var(--text-muted);"></textarea>
                            </div>
                            <button type="submit" class="btn">Confirm & Add Scanned Item to Inventory</button>
                        </form>
                    </div>
                </section>

                <!-- SECTION 4A: SUPPLY ORDERS -->
                <section id="supply" class="section">
                    <div class="card">
                        <div class="card-title">➕ Create New Vendor Supply Purchase Order</div><br>
                        <form id="create-supply-form">
                            <div class="form-grid">
                                <div class="form-group">
                                    <label>Vendor / Supplier Name</label>
                                    <select id="sup-vendor-select" required></select>
                                </div>
                                <div class="form-group">
                                    <label>Medicine Name</label>
                                    <input type="text" id="sup-med" required placeholder="e.g., Augmentin 625mg">
                                </div>
                                <div class="form-group">
                                    <label>Quantity Requested</label>
                                    <input type="number" id="sup-qty" min="1" required placeholder="200">
                                </div>
                                <div class="form-group">
                                    <label>Order Supply Category / Type</label>
                                    <select id="sup-type">
                                        <option value="Regular Reorder">Regular Reorder</option>
                                        <option value="Missing Stock Order">Missing Stock Order</option>
                                        <option value="Emergency Supply">Emergency Supply</option>
                                        <option value="Direct Purchase">Direct Purchase</option>
                                    </select>
                                </div>
                            </div>
                            <button type="submit" class="btn btn-success">Save Purchase Order Request</button>
                        </form>
                    </div>

                    <div class="card">
                        <div class="card-header">
                            <div class="card-title">🚚 Active Supply Purchase Orders</div>
                            <button class="btn btn-secondary" onclick="printAllOrders()">🖨️ Print Purchase Orders Slip</button>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr><th>Supplier / Ref</th><th>Medicine Name</th><th>Requested Qty</th><th>Type</th><th>Status</th><th>Actions</th></tr>
                                </thead>
                                <tbody id="supply-table-body"></tbody>
                            </table>
                        </div>
                    </div>
                </section>

                <!-- SECTION 4B: ORDER SEARCH / MISSING STOCK -->
                <section id="orders" class="section">
                    <div class="card">
                        <div class="card-title">🔍 Search Stock & Generate Purchase Orders</div>
                        <p style="color: var(--text-muted); font-size: 0.88rem; margin-top: 0.25rem;">Type any brand or formula. Search existing stock or generate purchase orders for out-of-stock and missing items.</p><br>
                        <div class="form-group">
                            <label>Type Medicine Name or Generic Formula</label>
                            <input type="text" id="smart-order-search" placeholder="e.g., Panadol, Amoxil, or any medicine..." onkeyup="handleSmartSearch()">
                        </div>
                        <div id="smart-search-results" style="margin-top:1.25rem;"></div>
                    </div>

                    <div class="card">
                        <div class="card-title">📋 Missing Stock & Special Purchase Requests Log</div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr><th>Supplier</th><th>Medicine Item</th><th>Requested Qty</th><th>Order Type</th><th>Status</th><th>Actions</th></tr>
                                </thead>
                                <tbody id="missing-orders-tbody"></tbody>
                            </table>
                        </div>
                    </div>
                </section>

                <!-- Shared Printable Purchase Order Slip -->
                <div id="printable-po" class="printable-area">
                    <h2 style="text-align: center;">PURCHASE ORDER SLIP</h2>
                    <p style="text-align: center; margin-bottom: 1rem; color: var(--text-muted);">Generated Date: <span id="po-print-date"></span></p>
                    <hr><br>
                    <div class="table-responsive">
                        <table style="width:100%; text-align:left;">
                            <thead>
                                <tr><th>Supplier</th><th>Item Name</th><th>Qty</th><th>Type</th><th>Status</th></tr>
                            </thead>
                            <tbody id="po-print-tbody"></tbody>
                        </table>
                    </div>
                    <br><hr><br>
                    <p style="text-align: center; font-size: 0.8rem; margin-top: 1.5rem;">Authorized Signature: _______________________</p>
                </div>

                <!-- SECTION 5: DAILY SALES REPORTS -->
                <section id="reports" class="section">
                    <div class="card">
                        <div class="card-header">
                            <div>
                                <h2 style="color: var(--text-dark);">Today's Revenue: Rs. <span id="report-today-total">0.00</span></h2>
                                <p style="color:var(--text-muted); font-size:0.85rem;">Total Sales Transactions Executed Today: <span id="report-today-count">0</span></p>
                            </div>
                            <button class="btn" onclick="printDailyReport()">🖨️ Save & Print Daily Summary</button>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr><th>Transaction ID</th><th>Timestamp</th><th>Cashier</th><th>Items Sold</th><th>Revenue</th></tr>
                                </thead>
                                <tbody id="sales-history-tbody"></tbody>
                            </table>
                        </div>
                    </div>

                    <div id="printable-report" class="printable-area">
                        <h2 style="text-align: center;">DAILY PHARMACY SALES REPORT</h2>
                        <p style="text-align: center; margin-bottom: 1rem;" id="report-print-date"></p>
                        <hr><br>
                        <h3>Summary Statistics:</h3>
                        <p>Total Revenue: Rs. <strong id="report-print-revenue">0.00</strong></p>
                        <p>Total Completed Sales: <strong id="report-print-orders">0</strong></p>
                        <br><hr><br>
                        <h3>Transaction History Log:</h3>
                        <div id="report-print-log"></div>
                    </div>
                </section>

                <!-- SECTION 6: DEDICATED WORKER ID GENERATION & MANAGEMENT TAB -->
                <section id="worker-gen" class="section">
                    <div class="card">
                        <div class="card-title">🆔 Register & Generate Worker Access ID</div>
                        <p style="color: var(--text-muted); font-size: 0.85rem; margin-bottom: 1rem;">Generate unique Worker IDs (e.g., <code>WRK-1001</code>) and set initial access credentials for store workers.</p>
                        <form id="create-worker-form">
                            <div class="form-grid">
                                <div class="form-group">
                                    <label>Worker Full Name</label>
                                    <input type="text" id="worker-name-input" required placeholder="e.g., Muhammad Ali">
                                </div>
                                <div class="form-group">
                                    <label>Worker Role</label>
                                    <select id="worker-role-input">
                                        <option value="Cashier">Cashier (Billing & Inventory View)</option>
                                        <option value="Pharmacist">Pharmacist (Full Stock & Billing)</option>
                                        <option value="Store Manager">Store Manager (Stock, POS & Supply)</option>
                                    </select>
                                </div>
                                <div class="form-group">
                                    <label>Initial Password</label>
                                    <input type="password" id="worker-pass-input" required placeholder="••••••••">
                                </div>
                            </div>
                            <button type="submit" class="btn btn-success">Generate & Register Worker ID</button>
                        </form>
                    </div>

                    <div class="card">
                        <div class="card-title">🔑 Change / Reset Worker Password</div>
                        <p style="color: var(--text-muted); font-size: 0.85rem; margin-bottom: 1rem;">Select any existing Worker ID to update or reset their system password.</p>
                        <form id="reset-worker-pass-form">
                            <div class="form-grid">
                                <div class="form-group">
                                    <label>Select Worker ID</label>
                                    <select id="reset-worker-select" required></select>
                                </div>
                                <div class="form-group">
                                    <label>New Password for Worker</label>
                                    <input type="password" id="reset-worker-newpass" required placeholder="Enter new password">
                                </div>
                            </div>
                            <button type="submit" class="btn btn-warning">Update Worker Password</button>
                        </form>
                    </div>

                    <div class="card">
                        <div class="card-title">🛡️ Change Parent Admin Master Password</div>
                        <p style="color: var(--text-muted); font-size: 0.85rem; margin-bottom: 1rem;">Change the main owner parent admin password used to access worker controls.</p>
                        <form id="change-parent-pass-form">
                            <div class="form-grid">
                                <div class="form-group">
                                    <label>New Master Password</label>
                                    <input type="password" id="parent-new-pass" required placeholder="Enter new master password">
                                </div>
                            </div>
                            <button type="submit" class="btn">Update Master Password</button>
                        </form>
                    </div>

                    <div class="card">
                        <div class="card-title">👥 Active Worker IDs & Privilege Oversight</div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr><th>Worker ID</th><th>Worker Name</th><th>Role</th><th>Active Password</th><th>Status</th><th>Access Privilege Control</th></tr>
                                </thead>
                                <tbody id="users-table-body"></tbody>
                            </table>
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-title">📜 Worker System Activity Logs</div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr><th>Timestamp</th><th>Worker ID</th><th>Module</th><th>Action Executed</th><th>Details</th></tr>
                                </thead>
                                <tbody id="activity-logs-tbody"></tbody>
                            </table>
                        </div>
                    </div>
                </section>

            </div>
        </main>
    </div>

    <!-- Missing Stock Modal -->
    <div id="missing-item-modal" class="modal-overlay">
        <div class="modal">
            <h3 style="color:var(--primary); margin-bottom:0.5rem;">Purchase Order Request</h3>
            <p id="modal-item-text" style="color: var(--text-muted); font-size: 0.95rem;">Add item to purchase orders?</p><br>
            <div class="form-group" style="margin-bottom:1rem; text-align:left;">
                <label>Requested Quantity</label>
                <input type="number" id="modal-order-qty" value="100" min="1">
            </div>
            <div style="display:flex; justify-content:center; gap:1rem;">
                <button class="btn btn-success" onclick="confirmAddMissingOrder()">Create Purchase Order</button>
                <button class="btn btn-secondary" onclick="closeModal()">Cancel</button>
            </div>
        </div>
    </div>

    <script>
        function sanitizeInput(str) {
            return String(str).replace(/[&<>"']/g, function(m) {
                return { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#039;' }[m];
            });
        }

        let failedLoginAttempts = 0;
        let activeUser = null;
        let pendingMissingItemName = "";

        let suppliers = JSON.parse(localStorage.getItem('pharma_suppliers')) || [
            "GlaxoSmithKline (GSK)", "Getz Pharma", "Pfizer", "Abbott Laboratories", "General Local Vendor"
        ];

        let users = JSON.parse(localStorage.getItem('pharma_users')) || [
            { id: "PHARM-9901", name: "Parent Owner", role: "Parent Admin", password: "admin", status: "Active" }
        ];

        let inventory = JSON.parse(localStorage.getItem('pharma_inventory')) || [
            { id: 1, name: "Paracetamol 500mg", formula: "Paracetamol", supplier: "GlaxoSmithKline (GSK)", batch: "B-101", expiry: "2027-12-31", qty: 100, price: 7.00 },
            { id: 2, name: "Amoxil 500mg", formula: "Amoxicillin", supplier: "Getz Pharma", batch: "B-204", expiry: "2026-10-15", qty: 8, price: 180.00 },
            { id: 3, name: "Brufen 400mg", formula: "Ibuprofen", supplier: "Abbott Laboratories", batch: "B-509", expiry: "2028-05-20", qty: 95, price: 45.00 }
        ];

        let salesHistory = JSON.parse(localStorage.getItem('pharma_sales')) || [];
        let orders = JSON.parse(localStorage.getItem('pharma_orders')) || [
            { supplier: "GlaxoSmithKline (GSK)", med: "Augmentin 625mg", qty: 200, type: "Regular Reorder", status: "Pending" },
            { supplier: "Getz Pharma", med: "Risek 20mg", qty: 100, type: "Missing Stock Order", status: "Pending" }
        ];
        let activityLogs = JSON.parse(localStorage.getItem('pharma_activity')) || [];
        let cart = [];

        function saveData() {
            localStorage.setItem('pharma_suppliers', JSON.stringify(suppliers));
            localStorage.setItem('pharma_inventory', JSON.stringify(inventory));
            localStorage.setItem('pharma_orders', JSON.stringify(orders));
            localStorage.setItem('pharma_users', JSON.stringify(users));
            localStorage.setItem('pharma_sales', JSON.stringify(salesHistory));
            localStorage.setItem('pharma_activity', JSON.stringify(activityLogs));
        }

        function logActivity(module, action, details) {
            const entry = {
                timestamp: new Date().toLocaleString(),
                userId: activeUser ? activeUser.id : "Anonymous",
                module: module,
                action: action,
                details: details
            };
            activityLogs.unshift(entry);
            saveData();
            if (activeUser) renderLogs();
        }

        // Authentication
        document.getElementById('login-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const errBox = document.getElementById('auth-error');
            
            if (failedLoginAttempts >= 5) {
                errBox.innerText = "Firewall Lockout: Security threshold reached. Refresh page.";
                errBox.style.display = 'block';
                return;
            }

            const inputId = document.getElementById('user-id').value.trim();
            const inputPass = document.getElementById('password').value.trim();

            const foundUser = users.find(u => u.id === inputId && u.password === inputPass);

            if (foundUser) {
                if (foundUser.status === "Revoked") {
                    errBox.innerText = "Access Denied: Worker ID revoked by Parent Admin.";
                    errBox.style.display = 'block';
                    return;
                }

                activeUser = foundUser;
                failedLoginAttempts = 0;
                document.getElementById('auth-screen').style.display = 'none';
                document.getElementById('app-screen').style.display = 'flex';
                document.getElementById('user-display-name').innerText = foundUser.name;
                document.getElementById('user-display-role').innerText = foundUser.role;

                document.getElementById('current-date-display').innerText = new Date().toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'short', day: 'numeric' });

                logActivity("Authentication", "LOGIN", `User ${foundUser.id} logged in`);
                populateSupplierDropdowns();
                renderDashboard();
                renderInventory();
                updatePOSSelect();
                renderSupplyOrders();
                renderUsers();
                renderLogs();
                renderSalesReport();
            } else {
                failedLoginAttempts++;
                errBox.innerText = `Invalid Credentials. Attempts left: ${5 - failedLoginAttempts}`;
                errBox.style.display = 'block';
            }
        });

        document.getElementById('logout-btn').addEventListener('click', () => {
            logActivity("Authentication", "LOGOUT", `User logged out`);
            activeUser = null;
            document.getElementById('app-screen').style.display = 'none';
            document.getElementById('auth-screen').style.display = 'flex';
        });

        function switchSection(sectionId, btnId) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            
            const targetSection = document.getElementById(sectionId);
            if(targetSection) targetSection.classList.add('active');

            const targetBtn = document.getElementById(btnId);
            if(targetBtn) targetBtn.classList.add('active');

            const titleMap = {
                'dashboard': 'Dashboard Overview',
                'inventory': 'Medicine Inventory Stock (Supplier-Wise)',
                'pos': 'Billing & Point of Sale',
                'scanner': 'Document Auto-Scan (OCR)',
                'supply': 'Vendor Supply Orders',
                'orders': 'Stock Search & Missing Item Orders',
                'reports': 'Daily Sales & Revenue Reports',
                'worker-gen': 'Worker ID Generation & Access Control'
            };
            document.getElementById('section-title').innerText = titleMap[sectionId] || 'PharmaStock PK';

            if(sectionId === 'dashboard') renderDashboard();
            if(sectionId === 'worker-gen') renderUsers();
            logActivity("Navigation", "VIEW_SWITCH", `Navigated to ${sectionId}`);
        }

        // Supplier Management
        function populateSupplierDropdowns() {
            const medSupplierSelect = document.getElementById('med-supplier');
            const ocrSupplierSelect = document.getElementById('ocr-med-supplier');
            const filterSupplierSelect = document.getElementById('filter-supplier-select');
            const supVendorSelect = document.getElementById('sup-vendor-select');

            if(medSupplierSelect) medSupplierSelect.innerHTML = '';
            if(ocrSupplierSelect) ocrSupplierSelect.innerHTML = '';
            if(supVendorSelect) supVendorSelect.innerHTML = '';
            if(filterSupplierSelect) filterSupplierSelect.innerHTML = '<option value="ALL">All Suppliers / Companies</option>';

            suppliers.forEach(sup => {
                if(medSupplierSelect) medSupplierSelect.innerHTML += `<option value="${sup}">${sup}</option>`;
                if(ocrSupplierSelect) ocrSupplierSelect.innerHTML += `<option value="${sup}">${sup}</option>`;
                if(supVendorSelect) supVendorSelect.innerHTML += `<option value="${sup}">${sup}</option>`;
                if(filterSupplierSelect) filterSupplierSelect.innerHTML += `<option value="${sup}">${sup}</option>`;
            });
        }

        document.getElementById('add-supplier-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const newSupName = sanitizeInput(document.getElementById('supplier-name-input').value.trim());
            if(newSupName && !suppliers.includes(newSupName)) {
                suppliers.push(newSupName);
                saveData();
                populateSupplierDropdowns();
                alert(`Supplier/Company "${newSupName}" added successfully!`);
                logActivity("Supplier", "ADD_SUPPLIER", `Added supplier: ${newSupName}`);
                document.getElementById('supplier-name-input').value = '';
            }
        });

        // Dashboard Overview
        function renderDashboard() {
            const todayStr = new Date().toISOString().split('T')[0];
            const todaysSales = salesHistory.filter(s => s.dateStr === todayStr);
            const totalRevenue = todaysSales.reduce((sum, s) => sum + s.totalAmount, 0);
            const lowStockCount = inventory.filter(i => i.qty <= 10).length;

            document.getElementById('dash-today-sales').innerText = totalRevenue.toFixed(2);
            document.getElementById('dash-today-count').innerText = todaysSales.length;
            document.getElementById('dash-low-stock').innerText = lowStockCount;
            document.getElementById('dash-total-items').innerText = inventory.length;

            const recentTbody = document.getElementById('dash-recent-sales');
            recentTbody.innerHTML = '';

            const recentList = [...salesHistory].reverse().slice(0, 5);
            if(recentList.length === 0) {
                recentTbody.innerHTML = '<tr><td colspan="4" style="text-align:center; color:var(--text-muted);">No sales recorded today yet.</td></tr>';
            } else {
                recentList.forEach(s => {
                    recentTbody.innerHTML += `
                        <tr>
                            <td><strong>${s.txId}</strong></td>
                            <td>${s.timestamp}</td>
                            <td>${s.cashier}</td>
                            <td><strong>Rs. ${s.totalAmount.toFixed(2)}</strong></td>
                        </tr>
                    `;
                });
            }
        }

        // Inventory Stock (Supplier-Wise)
        function renderInventory() {
            const tbody = document.getElementById('inventory-table-body');
            const searchQuery = document.getElementById('search-inventory').value.toLowerCase();
            const selectedSupplier = document.getElementById('filter-supplier-select').value;
            tbody.innerHTML = '';

            const today = new Date().toISOString().split('T')[0];
            inventory.forEach((item, index) => {
                const matchesSearch = item.name.toLowerCase().includes(searchQuery) || item.formula.toLowerCase().includes(searchQuery);
                const matchesSupplier = selectedSupplier === 'ALL' || item.supplier === selectedSupplier;

                if (matchesSearch && matchesSupplier) {
                    let statusBadge = '<span class="badge badge-success">In Stock</span>';
                    if (item.qty <= 10) statusBadge = '<span class="badge badge-warning">Low Stock</span>';
                    if (item.expiry <= today) statusBadge = '<span class="badge badge-danger">Expired</span>';

                    tbody.innerHTML += `
                        <tr>
                            <td><strong>${item.name}</strong></td>
                            <td>${item.formula}</td>
                            <td><span class="badge badge-info">${item.supplier || 'Unassigned'}</span></td>
                            <td>${item.batch}</td>
                            <td>${item.expiry}</td>
                            <td><strong>${item.qty}</strong></td>
                            <td>Rs. ${parseFloat(item.price).toFixed(2)}</td>
                            <td>${statusBadge}</td>
                            <td><button class="btn btn-danger" style="padding: 0.3rem 0.6rem; font-size: 0.8rem;" onclick="deleteItem(${index})">Remove</button></td>
                        </tr>
                    `;
                }
            });
        }

        document.getElementById('add-medicine-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const newItem = {
                id: Date.now(),
                name: sanitizeInput(document.getElementById('med-name').value),
                formula: sanitizeInput(document.getElementById('med-formula').value),
                supplier: document.getElementById('med-supplier').value,
                batch: sanitizeInput(document.getElementById('med-batch').value),
                expiry: document.getElementById('med-expiry').value,
                qty: parseInt(document.getElementById('med-qty').value),
                price: parseFloat(document.getElementById('med-price').value)
            };
            inventory.push(newItem);
            saveData();
            renderInventory();
            updatePOSSelect();
            logActivity("Inventory", "ADD_ITEM", `Added medicine: ${newItem.name} (Supplier: ${newItem.supplier})`);
            e.target.reset();
        });

        function deleteItem(index) {
            const deleted = inventory.splice(index, 1);
            saveData();
            renderInventory();
            updatePOSSelect();
            logActivity("Inventory", "DELETE_ITEM", `Removed item: ${deleted[0]?.name}`);
        }

        // Document Scanner (OCR)
        document.getElementById('ocr-file-input').addEventListener('change', async function(e) {
            const file = e.target.files[0];
            if (!file) return;

            const preview = document.getElementById('scanner-preview');
            const loading = document.getElementById('ocr-loading');
            const resultCard = document.getElementById('ocr-result-card');

            preview.src = URL.createObjectURL(file);
            preview.style.display = 'block';
            loading.style.display = 'block';
            resultCard.style.display = 'none';

            try {
                const worker = await Tesseract.createWorker('eng');
                const ret = await worker.recognize(file);
                await worker.terminate();

                const text = ret.data.text;
                document.getElementById('ocr-raw-text').value = text;

                const lines = text.split('\n').map(l => l.trim()).filter(l => l.length > 0);
                document.getElementById('ocr-med-name').value = lines[0] || "Scanned Product";
                document.getElementById('ocr-med-formula').value = lines[1] || "Active Ingredient";

                const batchMatch = text.match(/batch[:\s]*([a-z0-9\-]+)/i);
                document.getElementById('ocr-med-batch').value = batchMatch ? batchMatch[1] : "B-" + Math.floor(100 + Math.random() * 900);

                const expiryMatch = text.match(/\d{4}-\d{2}-\d{2}/);
                document.getElementById('ocr-med-expiry').value = expiryMatch ? expiryMatch[0] : new Date(Date.now() + 31536000000).toISOString().split('T')[0];

                const numMatches = text.match(/\d+/g);
                document.getElementById('ocr-med-qty').value = numMatches && numMatches[0] ? parseInt(numMatches[0]) : 50;
                document.getElementById('ocr-med-price').value = numMatches && numMatches[1] ? parseFloat(numMatches[1]) : 120.00;

                loading.style.display = 'none';
                resultCard.style.display = 'block';
                logActivity("OCR", "SCAN_SUCCESS", `Extracted text from document: ${file.name}`);
            } catch (err) {
                loading.style.display = 'none';
                alert("OCR processing error. Please use a clearer image document.");
            }
        });

        document.getElementById('ocr-edit-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const scannedItem = {
                id: Date.now(),
                name: sanitizeInput(document.getElementById('ocr-med-name').value),
                formula: sanitizeInput(document.getElementById('ocr-med-formula').value),
                supplier: document.getElementById('ocr-med-supplier').value,
                batch: sanitizeInput(document.getElementById('ocr-med-batch').value),
                expiry: document.getElementById('ocr-med-expiry').value,
                qty: parseInt(document.getElementById('ocr-med-qty').value),
                price: parseFloat(document.getElementById('ocr-med-price').value)
            };
            inventory.push(scannedItem);
            saveData();
            renderInventory();
            updatePOSSelect();
            alert("Auto-scanned medicine saved to stock!");
            logActivity("OCR", "SAVE_SCAN_ITEM", `Saved auto-scanned item: ${scannedItem.name}`);
            document.getElementById('ocr-result-card').style.display = 'none';
            document.getElementById('scanner-preview').style.display = 'none';
            document.getElementById('ocr-file-input').value = '';
        });

        // POS & Billing
        function updatePOSSelect() {
            const select = document.getElementById('pos-select-med');
            select.innerHTML = '';
            inventory.forEach(item => {
                select.innerHTML += `<option value="${item.id}">${item.name} (${item.formula}) - [${item.supplier || 'Vendor'}] - Stock: ${item.qty} | Rs. ${item.price}</option>`;
            });
        }

        function addToCart() {
            const id = parseInt(document.getElementById('pos-select-med').value);
            const qty = parseInt(document.getElementById('pos-qty').value);
            const med = inventory.find(i => i.id === id);

            if (!med || qty > med.qty) {
                alert("Insufficient stock available!");
                return;
            }

            cart.push({ ...med, cartQty: qty, total: med.price * qty });
            renderCart();
            logActivity("POS", "ADD_TO_CART", `Added ${qty} units of ${med.name} to billing cart`);
        }

        function renderCart() {
            const tbody = document.getElementById('cart-table-body');
            tbody.innerHTML = '';
            let subtotal = 0;
            cart.forEach(item => {
                subtotal += item.total;
                tbody.innerHTML += `
                    <tr>
                        <td><strong>${item.name}</strong></td>
                        <td>${item.supplier || 'General'}</td>
                        <td>${item.cartQty}</td>
                        <td>Rs. ${item.price.toFixed(2)}</td>
                        <td><strong>Rs. ${item.total.toFixed(2)}</strong></td>
                    </tr>
                `;
            });

            const tax = subtotal * 0.17;
            const grandTotal = subtotal + tax;

            document.getElementById('cart-subtotal').innerText = subtotal.toFixed(2);
            document.getElementById('cart-tax').innerText = tax.toFixed(2);
            document.getElementById('cart-total').innerText = grandTotal.toFixed(2);
        }

        function generateInvoice() {
            if (cart.length === 0) return alert("Billing cart is empty!");

            cart.forEach(cartItem => {
                const item = inventory.find(i => i.id === cartItem.id);
                if (item) {
                    item.qty -= cartItem.cartQty;
                }
            });

            const subtotal = cart.reduce((sum, item) => sum + item.total, 0);
            const tax = subtotal * 0.17;
            const grandTotal = subtotal + tax;

            const saleRecord = {
                txId: "TXN-" + Math.floor(100000 + Math.random() * 900000),
                timestamp: new Date().toLocaleString(),
                dateStr: new Date().toISOString().split('T')[0],
                cashier: activeUser ? activeUser.name : "Staff",
                itemsCount: cart.reduce((sum, item) => sum + item.cartQty, 0),
                summaryText: cart.map(i => `${i.name} (x${i.cartQty})`).join(', '),
                totalAmount: grandTotal
            };

            salesHistory.push(saleRecord);
            saveData();
            renderInventory();
            renderSalesReport();

            document.getElementById('bill-date').innerText = saleRecord.timestamp;
            const list = document.getElementById('bill-items-list');
            list.innerHTML = '';
            cart.forEach(item => {
                list.innerHTML += `<p style="display:flex; justify-content:space-between;"><span>${item.name} x${item.cartQty}</span> <span>Rs. ${item.total.toFixed(2)}</span></p>`;
            });

            document.getElementById('bill-subtotal').innerText = subtotal.toFixed(2);
            document.getElementById('bill-tax').innerText = tax.toFixed(2);
            document.getElementById('bill-grand-total').innerText = grandTotal.toFixed(2);

            logActivity("POS", "CHECKOUT_SALE", `Invoice #${saleRecord.txId} completed - Total: Rs. ${grandTotal.toFixed(2)}`);

            document.getElementById('printable-receipt').style.display = 'block';
            window.print();
            cart = [];
            renderCart();
            updatePOSSelect();
            document.getElementById('printable-receipt').style.display = 'none';
        }

        // Supply Orders Management
        document.getElementById('create-supply-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const newSupplyOrder = {
                supplier: document.getElementById('sup-vendor-select').value,
                med: sanitizeInput(document.getElementById('sup-med').value),
                qty: parseInt(document.getElementById('sup-qty').value),
                type: document.getElementById('sup-type').value,
                status: "Pending"
            };
            orders.push(newSupplyOrder);
            saveData();
            renderSupplyOrders();
            logActivity("Supply", "CREATE_ORDER", `Created supply order for ${newSupplyOrder.med} via ${newSupplyOrder.supplier}`);
            alert("Vendor Purchase Order Created Successfully!");
            e.target.reset();
        });

        function renderSupplyOrders() {
            const supplyTbody = document.getElementById('supply-table-body');
            const missingTbody = document.getElementById('missing-orders-tbody');
            
            if (supplyTbody) supplyTbody.innerHTML = '';
            if (missingTbody) missingTbody.innerHTML = '';

            if (orders.length === 0) {
                if (supplyTbody) supplyTbody.innerHTML = '<tr><td colspan="6" style="text-align:center; color:var(--text-muted);">No active supply purchase orders.</td></tr>';
                if (missingTbody) missingTbody.innerHTML = '<tr><td colspan="6" style="text-align:center; color:var(--text-muted);">No missing stock order requests logged.</td></tr>';
                return;
            }

            orders.forEach((o, index) => {
                const rowHtml = `
                    <tr>
                        <td>${o.supplier}</td>
                        <td><strong>${o.med}</strong></td>
                        <td>${o.qty}</td>
                        <td>${o.type || 'Regular'}</td>
                        <td><span class="badge badge-warning">${o.status}</span></td>
                        <td>
                            <button class="btn btn-secondary" style="padding: 0.3rem 0.6rem; font-size: 0.78rem;" onclick="printSingleOrder(${index})">🖨️ Print</button>
                            <button class="btn btn-danger" style="padding: 0.3rem 0.6rem; font-size: 0.78rem;" onclick="deleteOrder(${index})">Delete</button>
                        </td>
                    </tr>
                `;

                if (supplyTbody) supplyTbody.innerHTML += rowHtml;
                if (missingTbody && (o.type === 'Missing Stock Order' || o.type === 'Missing Stock Search')) {
                    missingTbody.innerHTML += rowHtml;
                }
            });
        }

        function deleteOrder(index) {
            orders.splice(index, 1);
            saveData();
            renderSupplyOrders();
        }

        // Smart Search & Missing Item Order Generation
        function handleSmartSearch() {
            const term = document.getElementById('smart-order-search').value.trim().toLowerCase();
            const resultsContainer = document.getElementById('smart-search-results');
            resultsContainer.innerHTML = '';

            if (!term) return;

            const matches = inventory.filter(i => i.name.toLowerCase().includes(term) || i.formula.toLowerCase().includes(term));

            let html = '';
            if (matches.length > 0) {
                matches.forEach(item => {
                    html += `
                        <div style="background:#e0f2fe; padding:0.85rem; border-radius:8px; margin-bottom:0.5rem; display:flex; justify-content:space-between; align-items:center; border: 1px solid #bae6fd;">
                            <div>
                                <strong>${item.name}</strong> (${item.formula}) - Supplier: <em>${item.supplier || 'N/A'}</em> - Stock: <strong>${item.qty}</strong> | Price: Rs. ${item.price}
                            </div>
                            <button class="btn" style="padding:0.35rem 0.75rem; font-size:0.8rem;" onclick="triggerMissingModal('${item.name}')">Reorder Stock</button>
                        </div>
                    `;
                });
            }

            html += `
                <div style="background:#fffbeb; padding:0.85rem; border-radius:8px; color:var(--text-dark); display:flex; justify-content:space-between; align-items:center; border: 1px solid #fde68a;">
                    <span>➕ Item missing or not found for <strong>"${document.getElementById('smart-order-search').value}"</strong>?</span>
                    <button class="btn btn-warning" style="padding:0.35rem 0.75rem; font-size:0.8rem;" onclick="triggerMissingModal('${document.getElementById('smart-order-search').value}')">Generate Missing Stock PO</button>
                </div>
            `;

            resultsContainer.innerHTML = html;
        }

        function triggerMissingModal(name) {
            pendingMissingItemName = name;
            document.getElementById('modal-item-text').innerText = `Generate Purchase Order request for missing item "${name}"?`;
            document.getElementById('missing-item-modal').style.display = 'flex';
        }

        function closeModal() {
            document.getElementById('missing-item-modal').style.display = 'none';
        }

        function confirmAddMissingOrder() {
            const qty = parseInt(document.getElementById('modal-order-qty').value) || 100;
            if (pendingMissingItemName) {
                const newOrder = {
                    supplier: suppliers[0] || "General Supplier",
                    med: pendingMissingItemName,
                    qty: qty,
                    type: "Missing Stock Order",
                    status: "Pending"
                };
                orders.push(newOrder);
                saveData();
                renderSupplyOrders();
                logActivity("Orders", "CREATE_MISSING_PO", `Created missing stock order for: ${pendingMissingItemName} (Qty: ${qty})`);
                closeModal();
                document.getElementById('smart-order-search').value = '';
                document.getElementById('smart-search-results').innerHTML = '';
            }
        }

        // Print Purchase Orders
        function printAllOrders() {
            if (orders.length === 0) return alert("No purchase orders available to print!");

            document.getElementById('po-print-date').innerText = new Date().toLocaleString();
            const poTbody = document.getElementById('po-print-tbody');
            poTbody.innerHTML = '';

            orders.forEach(o => {
                poTbody.innerHTML += `
                    <tr>
                        <td>${o.supplier}</td>
                        <td><strong>${o.med}</strong></td>
                        <td>${o.qty}</td>
                        <td>${o.type}</td>
                        <td>${o.status}</td>
                    </tr>
                `;
            });

            logActivity("Orders", "PRINT_ALL_PO", "Printed all supply purchase orders");
            document.getElementById('printable-po').style.display = 'block';
            window.print();
            document.getElementById('printable-po').style.display = 'none';
        }

        function printSingleOrder(index) {
            const o = orders[index];
            if (!o) return;

            document.getElementById('po-print-date').innerText = new Date().toLocaleString();
            const poTbody = document.getElementById('po-print-tbody');
            poTbody.innerHTML = `
                <tr>
                    <td>${o.supplier}</td>
                    <td><strong>${o.med}</strong></td>
                    <td>${o.qty}</td>
                    <td>${o.type}</td>
                    <td>${o.status}</td>
                </tr>
            `;

            logActivity("Orders", "PRINT_SINGLE_PO", `Printed PO slip for ${o.med}`);
            document.getElementById('printable-po').style.display = 'block';
            window.print();
            document.getElementById('printable-po').style.display = 'none';
        }

        // Daily Sales Reports
        function renderSalesReport() {
            const todayStr = new Date().toISOString().split('T')[0];
            const todaysSales = salesHistory.filter(s => s.dateStr === todayStr);
            let totalRev = 0;
            const tbody = document.getElementById('sales-history-tbody');
            tbody.innerHTML = '';

            todaysSales.forEach(s => {
                totalRev += s.totalAmount;
                tbody.innerHTML += `
                    <tr>
                        <td><strong>${s.txId}</strong></td>
                        <td>${s.timestamp}</td>
                        <td>${s.cashier}</td>
                        <td>${s.summaryText}</td>
                        <td><strong>Rs. ${s.totalAmount.toFixed(2)}</strong></td>
                    </tr>
                `;
            });

            document.getElementById('report-today-total').innerText = totalRev.toFixed(2);
            document.getElementById('report-today-count').innerText = todaysSales.length;
        }

        function printDailyReport() {
            const todayStr = new Date().toISOString().split('T')[0];
            const todaysSales = salesHistory.filter(s => s.dateStr === todayStr);
            const totalRev = todaysSales.reduce((sum, s) => sum + s.totalAmount, 0);

            document.getElementById('report-print-date').innerText = "Timestamp: " + new Date().toLocaleString();
            document.getElementById('report-print-revenue').innerText = totalRev.toFixed(2);
            document.getElementById('report-print-orders').innerText = todaysSales.length;

            const logDiv = document.getElementById('report-print-log');
            logDiv.innerHTML = '';
            todaysSales.forEach(s => {
                logDiv.innerHTML += `
                    <div style="border-bottom:1px solid #ddd; padding:0.5rem 0;">
                        <p><strong>${s.txId}</strong> - ${s.timestamp} (Cashier: ${s.cashier})</p>
                        <p style="font-size:0.85rem; color:#555;">Items: ${s.summaryText}</p>
                        <p>Total Revenue: Rs. ${s.totalAmount.toFixed(2)}</p>
                    </div>
                `;
            });

            logActivity("Reports", "PRINT_REPORT", `Printed daily sales report total: Rs. ${totalRev.toFixed(2)}`);
            document.getElementById('printable-report').style.display = 'block';
            window.print();
            document.getElementById('printable-report').style.display = 'none';
        }

        // Worker Management Section
        document.getElementById('create-worker-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const generatedID = "WRK-" + Math.floor(1000 + Math.random() * 9000);
            const newWorker = {
                id: generatedID,
                name: sanitizeInput(document.getElementById('worker-name-input').value),
                role: document.getElementById('worker-role-input').value,
                password: sanitizeInput(document.getElementById('worker-pass-input').value),
                status: "Active"
            };
            users.push(newWorker);
            saveData();
            renderUsers();
            logActivity("Worker Mgmt", "CREATE_WORKER_ID", `Generated Worker ID ${generatedID} for ${newWorker.name}`);
            alert(`Worker ID Registered Successfully!\n\nWorker ID: ${generatedID}\nPassword: ${newWorker.password}\nRole: ${newWorker.role}`);
            e.target.reset();
        });

        document.getElementById('reset-worker-pass-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const workerId = document.getElementById('reset-worker-select').value;
            const newPass = document.getElementById('reset-worker-newpass').value;

            const targetWorker = users.find(u => u.id === workerId);
            if (targetWorker) {
                targetWorker.password = newPass;
                saveData();
                renderUsers();
                logActivity("Worker Mgmt", "UPDATE_WORKER_PASS", `Updated password for Worker ID ${workerId}`);
                alert(`Password updated successfully for Worker ID: ${workerId}`);
                e.target.reset();
            }
        });

        document.getElementById('change-parent-pass-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const newPass = document.getElementById('parent-new-pass').value;
            const parentUser = users.find(u => u.role === "Parent Admin");
            if (parentUser) {
                parentUser.password = newPass;
                saveData();
                renderUsers();
                logActivity("Worker Mgmt", "UPDATE_MASTER_PASS", "Updated Parent Admin master password");
                alert("Parent Admin password updated successfully!");
                e.target.reset();
            }
        });

        function toggleUserAccess(userId) {
            const u = users.find(user => user.id === userId);
            if (u && u.role !== "Parent Admin") {
                u.status = (u.status === "Active") ? "Revoked" : "Active";
                saveData();
                renderUsers();
                logActivity("Worker Mgmt", "TOGGLE_WORKER_ACCESS", `Changed access for ${u.id} to ${u.status}`);
            }
        }

        function renderUsers() {
            const tbody = document.getElementById('users-table-body');
            const selectWorker = document.getElementById('reset-worker-select');

            if (tbody) tbody.innerHTML = '';
            if (selectWorker) selectWorker.innerHTML = '';

            users.forEach(u => {
                const statusBadge = u.status === "Active" 
                    ? '<span class="badge badge-success">Active Access</span>' 
                    : '<span class="badge badge-danger">Revoked</span>';

                const actionBtn = u.role === "Parent Admin" 
                    ? '<span style="color:var(--text-muted); font-size:0.8rem;">Master Owner</span>' 
                    : `<button class="btn ${u.status === 'Active' ? 'btn-danger' : 'btn-success'}" style="padding: 0.3rem 0.61rem; font-size:0.78rem;" onclick="toggleUserAccess('${u.id}')">${u.status === 'Active' ? 'Revoke Access' : 'Grant Access'}</button>`;

                if (tbody) {
                    tbody.innerHTML += `
                        <tr>
                            <td><strong>${u.id}</strong></td>
                            <td>${u.name}</td>
                            <td>${u.role}</td>
                            <td><code style="background:#f1f5f9; padding:0.25rem 0.5rem; border-radius:4px;">${u.password}</code></td>
                            <td>${statusBadge}</td>
                            <td>${actionBtn}</td>
                        </tr>
                    `;
                }

                if (selectWorker && u.role !== "Parent Admin") {
                    selectWorker.innerHTML += `<option value="${u.id}">${u.name} (${u.id})</option>`;
                }
            });
        }

        function renderLogs() {
            const tbody = document.getElementById('activity-logs-tbody');
            if (tbody) {
                tbody.innerHTML = '';
                activityLogs.forEach(log => {
                    tbody.innerHTML += `
                        <tr>
                            <td><small>${log.timestamp}</small></td>
                            <td><strong>${log.userId}</strong></td>
                            <td>${log.module}</td>
                            <td><strong>${log.action}</strong></td>
                            <td>${log.details}</td>
                        </tr>
                    `;
                });
            }
        }
    </script>
</body>
</html>
