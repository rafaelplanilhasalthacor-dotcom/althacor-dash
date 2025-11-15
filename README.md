<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ALTHACOR - Gestão de Custos e Margens</title>
    <!-- Chart.js CDN for interactive graphs -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Font Awesome CDN for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* Global Styles */
        :root {
            --primary-blue: #1e40af; /* Tailwind blue-700 */
            --secondary-gray: #4b5563; /* Tailwind gray-700 */
            --text-light: #f9fafb; /* Tailwind gray-50 */
            --text-dark: #1f2937; /* Tailwind gray-900 */
            --bg-light: #ffffff;
            --bg-dark: #1f2937;
            --card-bg-light: #f3f4f6; /* Tailwind gray-100 */
            --card-bg-dark: #374151; /* Tailwind gray-700 */
            --border-light: #e5e7eb; /* Tailwind gray-200 */
            --border-dark: #4b5563; /* Tailwind gray-600 */
            --success-green: #10b981; /* Tailwind green-500 */
            --warning-yellow: #f59e0b; /* Tailwind yellow-500 */
            --danger-red: #ef4444; /* Tailwind red-500 */
            --info-blue: #3b82f6; /* Tailwind blue-500 */
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            min-height: 100vh;
            transition: background-color 0.3s, color 0.3s;
        }

        /* Theme Switching */
        body.light-theme {
            background-color: var(--bg-light);
            color: var(--text-dark);
        }

        body.dark-theme {
            background-color: var(--bg-dark);
            color: var(--text-light);
        }

        /* App Container */
        #app-container {
            display: flex;
            width: 100%;
        }

        /* Sidebar */
        #sidebar {
            width: 250px;
            background-color: var(--primary-blue);
            color: var(--text-light);
            padding: 20px;
            box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
            display: flex;
            flex-direction: column;
            flex-shrink: 0;
        }

        #sidebar .logo {
            text-align: center;
            margin-bottom: 30px;
            font-size: 1.8em;
            font-weight: bold;
            color: var(--text-light);
        }

        #sidebar nav ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        #sidebar nav ul li {
            margin-bottom: 10px;
        }

        #sidebar nav ul li a {
            display: block;
            color: var(--text-light);
            text-decoration: none;
            padding: 12px 15px;
            border-radius: 8px;
            transition: background-color 0.3s, color 0.3s;
            display: flex;
            align-items: center;
        }

        #sidebar nav ul li a i {
            margin-right: 10px;
            font-size: 1.1em;
        }

        #sidebar nav ul li a:hover,
        #sidebar nav ul li a.active {
            background-color: rgba(255, 255, 255, 0.2);
        }

        /* Main Content */
        #content {
            flex-grow: 1;
            padding: 20px;
            overflow-y: auto;
        }

        /* Topbar */
        #topbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--border-light);
        }
        body.dark-theme #topbar {
            border-bottom-color: var(--border-dark);
        }

        #topbar h1 {
            margin: 0;
            font-size: 2em;
            color: var(--primary-blue);
        }
        body.dark-theme #topbar h1 {
            color: var(--text-light);
        }

        #theme-toggle {
            background: none;
            border: none;
            font-size: 1.5em;
            cursor: pointer;
            color: var(--secondary-gray);
            transition: color 0.3s;
        }
        body.dark-theme #theme-toggle {
            color: var(--text-light);
        }
        #theme-toggle:hover {
            color: var(--primary-blue);
        }
        body.dark-theme #theme-toggle:hover {
            color: var(--info-blue);
        }

        /* View Sections */
        .view {
            display: none;
        }

        .view.active {
            display: block;
        }

        /* Cards */
        .card-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .card {
            background-color: var(--card-bg-light);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            text-align: center;
            transition: background-color 0.3s, color 0.3s;
        }
        body.dark-theme .card {
            background-color: var(--card-bg-dark);
        }

        .card h3 {
            margin-top: 0;
            font-size: 1.1em;
            color: var(--secondary-gray);
        }
        body.dark-theme .card h3 {
            color: var(--text-light);
        }

        .card p {
            font-size: 2.2em;
            font-weight: bold;
            margin-bottom: 0;
            color: var(--primary-blue);
        }
        body.dark-theme .card p {
            color: var(--info-blue);
        }

        /* Charts */
        .chart-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .chart-container {
            background-color: var(--card-bg-light);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            position: relative;
            height: 350px; /* Fixed height for charts */
            transition: background-color 0.3s;
        }
        body.dark-theme .chart-container {
            background-color: var(--card-bg-dark);
        }

        /* Forms and Inputs */
        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        input[type="text"],
        input[type="number"],
        input[type="email"],
        select,
        textarea {
            width: calc(100% - 20px);
            padding: 10px;
            border: 1px solid var(--border-light);
            border-radius: 5px;
            font-size: 1em;
            background-color: var(--bg-light);
            color: var(--text-dark);
            transition: border-color 0.3s, background-color 0.3s, color 0.3s;
        }
        body.dark-theme input[type="text"],
        body.dark-theme input[type="number"],
        body.dark-theme input[type="email"],
        body.dark-theme select,
        body.dark-theme textarea {
            border-color: var(--border-dark);
            background-color: var(--card-bg-dark);
            color: var(--text-light);
        }

        input[type="text"]:focus,
        input[type="number"]:focus,
        input[type="email"]:focus,
        select:focus,
        textarea:focus {
            border-color: var(--primary-blue);
            outline: none;
        }

        /* Buttons */
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.3s, color 0.3s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn-primary {
            background-color: var(--primary-blue);
            color: var(--text-light);
        }

        .btn-primary:hover {
            background-color: #1a368b; /* Darker blue */
        }

        .btn-secondary {
            background-color: var(--secondary-gray);
            color: var(--text-light);
        }

        .btn-secondary:hover {
            background-color: #374151; /* Darker gray */
        }

        .btn-success {
            background-color: var(--success-green);
            color: var(--text-light);
        }

        .btn-success:hover {
            background-color: #0c8a62;
        }

        .btn-danger {
            background-color: var(--danger-red);
            color: var(--text-light);
        }

        .btn-danger:hover {
            background-color: #c22f2f;
        }

        .btn-info {
            background-color: var(--info-blue);
            color: var(--text-light);
        }

        .btn-info:hover {
            background-color: #2563eb;
        }

        .btn-outline {
            background-color: transparent;
            border: 1px solid var(--primary-blue);
            color: var(--primary-blue);
        }
        body.dark-theme .btn-outline {
            border-color: var(--info-blue);
            color: var(--info-blue);
        }
        .btn-outline:hover {
            background-color: var(--primary-blue);
            color: var(--text-light);
        }
        body.dark-theme .btn-outline:hover {
            background-color: var(--info-blue);
            color: var(--text-light);
        }

        .btn-group {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        /* Tables */
        .table-container {
            overflow-x: auto;
            margin-top: 20px;
        }

        .data-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            background-color: var(--bg-light);
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }
        body.dark-theme .data-table {
            background-color: var(--card-bg-dark);
        }

        .data-table th,
        .data-table td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid var(--border-light);
        }
        body.dark-theme .data-table th,
        body.dark-theme .data-table td {
            border-bottom-color: var(--border-dark);
        }

        .data-table th {
            background-color: var(--card-bg-light);
            font-weight: bold;
            color: var(--secondary-gray);
            text-transform: uppercase;
            font-size: 0.9em;
        }
        body.dark-theme .data-table th {
            background-color: var(--card-bg-dark);
            color: var(--text-light);
        }

        .data-table tbody tr:hover {
            background-color: var(--card-bg-light);
        }
        body.dark-theme .data-table tbody tr:hover {
            background-color: #4b5563;
        }

        .data-table .action-buttons {
            display: flex;
            gap: 5px;
        }

        .data-table .action-buttons .btn {
            padding: 6px 10px;
            font-size: 0.85em;
        }

        /* Margin Colors in Table */
        .margin-green { color: var(--success-green); font-weight: bold; }
        .margin-yellow { color: var(--warning-yellow); font-weight: bold; }
        .margin-red { color: var(--danger-red); font-weight: bold; }

        /* Calculator Specific Styles */
        #calculator-view .search-results {
            max-height: 200px;
            overflow-y: auto;
            border: 1px solid var(--border-light);
            border-radius: 5px;
            margin-top: 5px;
            background-color: var(--bg-light);
        }
        body.dark-theme #calculator-view .search-results {
            border-color: var(--border-dark);
            background-color: var(--card-bg-dark);
        }

        #calculator-view .search-results div {
            padding: 10px;
            cursor: pointer;
            border-bottom: 1px solid var(--border-light);
        }
        body.dark-theme #calculator-view .search-results div {
            border-bottom-color: var(--border-dark);
        }
        #calculator-view .search-results div:last-child {
            border-bottom: none;
        }
        #calculator-view .search-results div:hover {
            background-color: var(--card-bg-light);
        }
        body.dark-theme #calculator-view .search-results div:hover {
            background-color: #4b5563;
        }

        #calculator-view .product-details,
        #calculator-view .simulation-results {
            background-color: var(--card-bg-light);
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
            transition: background-color 0.3s;
        }
        body.dark-theme #calculator-view .product-details,
        body.dark-theme #calculator-view .simulation-results {
            background-color: var(--card-bg-dark);
        }

        #calculator-view .product-details h3,
        #calculator-view .simulation-results h3 {
            margin-top: 0;
            color: var(--primary-blue);
        }
        body.dark-theme #calculator-view .product-details h3,
        body.dark-theme #calculator-view .simulation-results h3 {
            color: var(--info-blue);
        }

        #calculator-view .result-item {
            display: flex;
            justify-content: space-between;
            padding: 5px 0;
            border-bottom: 1px dashed var(--border-light);
        }
        body.dark-theme #calculator-view .result-item {
            border-bottom-color: var(--border-dark);
        }
        #calculator-view .result-item:last-child {
            border-bottom: none;
        }
        #calculator-view .result-item span:first-child {
            font-weight: bold;
        }

        .status-badge {
            padding: 5px 10px;
            border-radius: 5px;
            font-weight: bold;
            display: inline-block;
            margin-top: 10px;
        }
        .status-healthy { background-color: var(--success-green); color: var(--text-light); }
        .status-low { background-color: var(--danger-red); color: var(--text-light); }

        /* Modal Styles */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.4);
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: var(--bg-light);
            margin: auto;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 600px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            position: relative;
            transition: background-color 0.3s;
        }
        body.dark-theme .modal-content {
            background-color: var(--card-bg-dark);
        }

        .close-button {
            color: var(--secondary-gray);
            position: absolute;
            top: 15px;
            right: 20px;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }
        .close-button:hover,
        .close-button:focus {
            color: var(--primary-blue);
            text-decoration: none;
        }

        /* Formula Builder */
        #formula-ingredients-list {
            border: 1px solid var(--border-light);
            border-radius: 5px;
            padding: 10px;
            margin-top: 10px;
            max-height: 200px;
            overflow-y: auto;
        }
        body.dark-theme #formula-ingredients-list {
            border-color: var(--border-dark);
        }

        .formula-ingredient-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 0;
            border-bottom: 1px dashed var(--border-light);
        }
        body.dark-theme .formula-ingredient-item {
            border-bottom-color: var(--border-dark);
        }
        .formula-ingredient-item:last-child {
            border-bottom: none;
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            #app-container {
                flex-direction: column;
            }

            #sidebar {
                width: 100%;
                height: auto;
                padding: 15px;
                box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            }

            #sidebar nav ul {
                display: flex;
                flex-wrap: wrap;
                justify-content: center;
            }

            #sidebar nav ul li {
                margin: 5px;
            }

            #sidebar nav ul li a {
                padding: 8px 12px;
                font-size: 0.9em;
            }
            #sidebar nav ul li a i {
                margin-right: 5px;
            }

            #content {
                padding: 15px;
            }

            #topbar {
                flex-direction: column;
                align-items: flex-start;
                margin-bottom: 20px;
            }

            #topbar h1 {
                font-size: 1.5em;
                margin-bottom: 10px;
            }

            .card-grid, .chart-grid {
                grid-template-columns: 1fr;
            }

            .data-table th, .data-table td {
                padding: 8px 10px;
                font-size: 0.85em;
            }
        }
    </style>
</head>
<body class="light-theme">
    <div id="app-container">
        <aside id="sidebar">
            <div class="logo">ALTHACOR</div>
            <nav>
                <ul>
                    <li><a href="#" data-view="dashboard-view" class="active"><i class="fas fa-home"></i> Dashboard</a></li>
                    <li><a href="#" data-view="calculator-view"><i class="fas fa-calculator"></i> Calculadora de Margem</a></li>
                    <li><a href="#" data-view="products-view"><i class="fas fa-box"></i> Gestão de Produtos</a></li>
                    <li><a href="#" data-view="raw-materials-view"><i class="fas fa-flask"></i> Matérias-Primas</a></li>
                    <li><a href="#" data-view="formulas-view"><i class="fas fa-receipt"></i> Gestão de Fórmulas</a></li>
                    <li><a href="#" data-view="reports-view"><i class="fas fa-chart-line"></i> Relatórios</a></li>
                    <li><a href="#" data-view="settings-view"><i class="fas fa-cog"></i> Configurações</a></li>
                </ul>
            </nav>
        </aside>

        <main id="content">
            <header id="topbar">
                <h1 id="current-view-title">Dashboard</h1>
                <button id="theme-toggle"><i class="fas fa-moon"></i></button>
            </header>

            <!-- Dashboard View -->
            <section id="dashboard-view" class="view active">
                <div class="card-grid">
                    <div class="card">
                        <h3>Margem Média</h3>
                        <p id="kpi-avg-margin">0%</p>
                    </div>
                    <div class="card">
                        <h3>Produto Mais Lucrativo</h3>
                        <p id="kpi-most-profitable">N/A</p>
                    </div>
                    <div class="card">
                        <h3>Menor Margem</h3>
                        <p id="kpi-least-margin">N/A</p>
                    </div>
                    <div class="card">
                        <h3>Total de Produtos</h3>
                        <p id="kpi-total-products">0</p>
                    </div>
                </div>
                <div class="chart-grid">
                    <div class="chart-container">
                        <h3>Margem por Produto</h3>
                        <canvas id="marginChart"></canvas>
                    </div>
                    <div class="chart-container">
                        <h3>Distribuição por Categoria</h3>
                        <canvas id="categoryChart"></canvas>
                    </div>
                    <div class="chart-container">
                        <h3>Lucro vs Preço</h3>
                        <canvas id="profitPriceChart"></canvas>
                    </div>
                </div>
            </section>

            <!-- Calculator View -->
            <section id="calculator-view" class="view">
                <h2>Calculadora de Margem</h2>
                <div class="form-group">
                    <label for="calc-product-search">Buscar Produto:</label>
                    <input type="text" id="calc-product-search" placeholder="Digite o nome do produto...">
                    <div id="calc-search-results" class="search-results" style="display: none;"></div>
                </div>

                <div id="calc-product-details" class="product-details" style="display: none;">
                    <h3>Detalhes do Produto Selecionado</h3>
                    <div class="result-item"><span>Produto:</span> <span id="calc-product-name"></span></div>
                    <div class="result-item"><span>Custo Atual:</span> <span id="calc-product-cost"></span></div>
                    <div class="result-item"><span>Preço Atual:</span> <span id="calc-product-price"></span></div>
                    <div class="result-item"><span>Margem Atual:</span> <span id="calc-product-margin"></span></div>
                    <div class="result-item"><span>Lucro Unitário Atual:</span> <span id="calc-product-profit"></span></div>
                </div>

                <div id="calc-simulation" class="simulation-results" style="display: none;">
                    <h3>Simulação de Preço</h3>
                    <div class="form-group">
                        <label for="calc-simulated-price">Preço Simulado (R$):</label>
                        <input type="number" id="calc-simulated-price" step="0.01" min="0">
                    </div>

                    <div class="result-item"><span>Nova Margem:</span> <span id="calc-new-margin"></span></div>
                    <div class="result-item"><span>Novo Lucro Unitário:</span> <span id="calc-new-profit"></span></div>
                    <div class="result-item"><span>Diferença vs Preço Atual:</span> <span id="calc-profit-diff"></span></div>
                    <div class="result-item"><span>Status:</span> <span id="calc-status" class="status-badge"></span></div>

                    <h4 style="margin-top: 20px;">Impacto Financeiro (Lucro)</h4>
                    <div class="result-item"><span>10 unidades:</span> <span id="calc-impact-10"></span></div>
                    <div class="result-item"><span>50 unidades:</span> <span id="calc-impact-50"></span></div>
                    <div class="result-item"><span>100 unidades:</span> <span id="calc-impact-100"></span></div>
                </div>
            </section>

            <!-- Products View -->
            <section id="products-view" class="view">
                <h2>Gestão de Produtos</h2>
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                    <input type="text" id="product-search" placeholder="Buscar produto..." style="width: 40%;">
                    <select id="product-category-filter" style="width: 20%;">
                        <option value="">Todas as Categorias</option>
                    </select>
                    <select id="product-margin-filter" style="width: 20%;">
                        <option value="">Todas as Margens</option>
                        <option value="low">Margem Baixa (<30%)</option>
                        <option value="medium">Margem Média (30-49%)</option>
                        <option value="high">Margem Alta (>=50%)</option>
                    </select>
                    <button class="btn btn-primary" id="add-product-btn"><i class="fas fa-plus"></i> Adicionar Produto</button>
                    <button class="btn btn-info" id="export-products-csv-btn"><i class="fas fa-file-csv"></i> Exportar CSV</button>
                </div>
                <div class="table-container">
                    <table class="data-table">
                        <thead>
                            <tr>
                                <th>Produto</th>
                                <th>Custo</th>
                                <th>Preço</th>
                                <th>Margem %</th>
                                <th>Lucro/Unit</th>
                                <th>Categoria</th>
                                <th>Atualizado em</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody id="products-table-body">
                            <!-- Product rows will be inserted here -->
                        </tbody>
                    </table>
                </div>
            </section>

            <!-- Raw Materials View -->
            <section id="raw-materials-view" class="view">
                <h2>Gestão de Matérias-Primas</h2>
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                    <input type="text" id="raw-material-search" placeholder="Buscar matéria-prima..." style="width: 60%;">
                    <button class="btn btn-primary" id="add-raw-material-btn"><i class="fas fa-plus"></i> Adicionar Matéria-Prima</button>
                </div>
                <div class="table-container">
                    <table class="data-table">
                        <thead>
                            <tr>
                                <th>Nome</th>
                                <th>Custo Unitário</th>
                                <th>Unidade</th>
                                <th>Fornecedor</th>
                                <th>Contato</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody id="raw-materials-table-body">
                            <!-- Raw material rows will be inserted here -->
                        </tbody>
                    </table>
                </div>
            </section>

            <!-- Formulas View -->
            <section id="formulas-view" class="view">
                <h2>Gestão de Fórmulas (PI/PA)</h2>
                <div class="form-group">
                    <label for="formula-product-select">Produto:</label>
                    <select id="formula-product-select">
                        <option value="">Selecione um produto</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="formula-raw-material-select">Adicionar Matéria-Prima:</label>
                    <select id="formula-raw-material-select">
                        <option value="">Selecione uma matéria-prima</option>
                    </select>
                    <input type="number" id="formula-quantity" placeholder="Quantidade" step="0.01" min="0" style="width: calc(50% - 10px); margin-left: 10px;">
                    <button class="btn btn-secondary" id="add-formula-ingredient-btn" style="margin-top: 10px;"><i class="fas fa-plus"></i> Adicionar Ingrediente</button>
                </div>

                <h3>Ingredientes da Fórmula:</h3>
                <div id="formula-ingredients-list">
                    <!-- Ingredients will be listed here -->
                </div>

                <div class="form-group" style="margin-top: 20px;">
                    <label>Custo Total da Fórmula:</label>
                    <p id="formula-total-cost" style="font-size: 1.2em; font-weight: bold;">R$ 0,00</p>
                </div>
                <div class="form-group">
                    <label for="formula-desired-margin">Margem Desejada (%):</label>
                    <input type="number" id="formula-desired-margin" step="0.1" min="0" max="100" value="30">
                </div>
                <div class="form-group">
                    <label>Preço Sugerido:</label>
                    <p id="formula-suggested-price" style="font-size: 1.2em; font-weight: bold;">R$ 0,00</p>
                </div>
                <button class="btn btn-success" id="save-formula-btn"><i class="fas fa-save"></i> Salvar Fórmula e Atualizar Produto</button>
            </section>

            <!-- Reports View -->
            <section id="reports-view" class="view">
                <h2>Relatórios</h2>
                <div class="btn-group">
                    <button class="btn btn-primary" id="generate-general-report-btn"><i class="fas fa-list"></i> Relatório Geral de Produtos</button>
                    <button class="btn btn-info" id="generate-abc-report-btn"><i class="fas fa-chart-pie"></i> Análise ABC (Pareto)</button>
                    <button class="btn btn-danger" id="generate-low-margin-report-btn"><i class="fas fa-exclamation-triangle"></i> Produtos com Margem Baixa</button>
                </div>

                <div id="report-output" class="table-container" style="margin-top: 30px;">
                    <!-- Report content will be displayed here -->
                </div>
                <button class="btn btn-info" id="export-report-csv-btn" style="display: none; margin-top: 20px;"><i class="fas fa-file-csv"></i> Exportar Relatório CSV</button>
            </section>

            <!-- Settings View -->
            <section id="settings-view" class="view">
                <h2>Configurações</h2>
                <div class="form-group">
                    <label>Tema:</label>
                    <div style="display: flex; gap: 10px;">
                        <button class="btn btn-outline" id="set-light-theme"><i class="fas fa-sun"></i> Claro</button>
                        <button class="btn btn-outline" id="set-dark-theme"><i class="fas fa-moon"></i> Escuro</button>
                    </div>
                </div>
                <div class="form-group">
                    <label>Backup e Restauração:</label>
                    <button class="btn btn-secondary" id="backup-data-btn"><i class="fas fa-download"></i> Fazer Backup (JSON)</button>
                    <input type="file" id="restore-data-input" accept=".json" style="display: none;">
                    <button class="btn btn-secondary" id="restore-data-btn" style="margin-left: 10px;"><i class="fas fa-upload"></i> Restaurar Backup</button>
                </div>
                <div class="form-group">
                    <label>Exportar Todos os Dados:</label>
                    <button class="btn btn-info" id="export-all-data-btn"><i class="fas fa-file-export"></i> Exportar Tudo (JSON)</button>
                </div>
            </section>
        </main>
    </div>

    <!-- Modals -->
    <div id="product-modal" class="modal">
        <div class="modal-content">
            <span class="close-button">&times;</span>
            <h2><span id="product-modal-title">Adicionar Produto</span></h2>
            <form id="product-form">
                <input type="hidden" id="product-id">
                <div class="form-group">
                    <label for="product-name">Nome do Produto:</label>
                    <input type="text" id="product-name" required>
                </div>
                <div class="form-group">
                    <label for="product-cost">Custo (R$):</label>
                    <input type="number" id="product-cost" step="0.01" min="0" required>
                </div>
                <div class="form-group">
                    <label for="product-price">Preço de Venda (R$):</label>
                    <input type="number" id="product-price" step="0.01" min="0" required>
                </div>
                <div class="form-group">
                    <label for="product-category">Categoria:</label>
                    <input type="text" id="product-category" required>
                </div>
                <div class="btn-group">
                    <button type="submit" class="btn btn-success"><i class="fas fa-save"></i> Salvar</button>
                    <button type="button" class="btn btn-secondary" onclick="closeModal('product-modal')"><i class="fas fa-times"></i> Cancelar</button>
                </div>
            </form>
        </div>
    </div>

    <div id="raw-material-modal" class="modal">
        <div class="modal-content">
            <span class="close-button">&times;</span>
            <h2><span id="raw-material-modal-title">Adicionar Matéria-Prima</span></h2>
            <form id="raw-material-form">
                <input type="hidden" id="raw-material-id">
                <div class="form-group">
                    <label for="raw-material-name">Nome da Matéria-Prima:</label>
                    <input type="text" id="raw-material-name" required>
                </div>
                <div class="form-group">
                    <label for="raw-material-unit-cost">Custo Unitário (R$):</label>
                    <input type="number" id="raw-material-unit-cost" step="0.01" min="0" required>
                </div>
                <div class="form-group">
                    <label for="raw-material-unit-measure">Unidade de Medida:</label>
                    <input type="text" id="raw-material-unit-measure" placeholder="Ex: kg, L, m" required>
                </div>
                <div class="form-group">
                    <label for="raw-material-supplier">Fornecedor:</label>
                    <input type="text" id="raw-material-supplier">
                </div>
                <div class="form-group">
                    <label for="raw-material-contact">Contato do Fornecedor:</label>
                    <input type="text" id="raw-material-contact">
                </div>
                <div class="btn-group">
                    <button type="submit" class="btn btn-success"><i class="fas fa-save"></i> Salvar</button>
                    <button type="button" class="btn btn-secondary" onclick="closeModal('raw-material-modal')"><i class="fas fa-times"></i> Cancelar</button>
                </div>
            </form>
        </div>
    </div>

    <script>
        // --- Data Structures ---
        let products = [];
        let rawMaterials = [];
        let formulas = []; // { productId: 'p1', ingredients: [{ rawMaterialId: 'rm1', quantity: 0.5 }] }

        // --- Chart Instances ---
        let marginChartInstance, categoryChartInstance, profitPriceChartInstance;

        // --- Utility Functions ---
        function generateId() {
            return '_' + Math.random().toString(36).substr(2, 9);
        }

        function formatCurrency(value) {
            return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(value);
        }

        function formatDate(dateString) {
            const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
            return new Date(dateString).toLocaleDateString('pt-BR', options);
        }

        function calculateProductMetrics(product) {
            if (product.cost > 0 && product.price > 0) {
                product.margin = ((product.price - product.cost) / product.price) * 100;
                product.profitPerUnit = product.price - product.cost;
            } else {
                product.margin = 0;
                product.profitPerUnit = 0;
            }
            return product;
        }

        function getMarginClass(margin) {
            if (margin >= 50) return 'margin-green';
            if (margin >= 30) return 'margin-yellow';
            return 'margin-red';
        }

        function getMarginStatus(margin) {
            if (margin >= 50) return { text: 'Margem Saudável', class: 'status-healthy' };
            if (margin >= 30) return { text: 'Margem Boa', class: 'status-healthy' };
            return { text: 'Margem Baixa', class: 'status-low' };
        }

        // --- Local Storage Management ---
        function saveData() {
            localStorage.setItem('althacor_products', JSON.stringify(products));
            localStorage.setItem('althacor_rawMaterials', JSON.stringify(rawMaterials));
            localStorage.setItem('althacor_formulas', JSON.stringify(formulas));
        }

        function loadData() {

            // Recalculate metrics on load to ensure consistency
            products = products.map(p => calculateProductMetrics(p));
        }

        function initializeExampleData() {
            if (products.length === 0 && rawMaterials.length === 0) {
                rawMaterials = [
                    { id: generateId(), name: 'Resina Acrílica', unitCost: 20.00, unitMeasure: 'kg', supplier: 'TechQuímica', contact: 'contato@techquimica.com' },
                    { id: generateId(), name: 'Pigmento Vermelho', unitCost: 50.00, unitMeasure: 'kg', supplier: 'ColorTex', contact: 'vendas@colortex.com' },
                    { id: generateId(), name: 'Pigmento Azul', unitCost: 45.00, unitMeasure: 'kg', supplier: 'ColorTex', contact: 'vendas@colortex.com' },
                    { id: generateId(), name: 'Etileno Glicol', unitCost: 15.00, unitMeasure: 'L', supplier: 'PoliTech', contact: 'comercial@politech.com' },
                    { id: generateId(), name: 'Diluente Industrial', unitCost: 5.00, unitMeasure: 'L', supplier: 'PoliTech', contact: 'comercial@politech.com' },
                    { id: generateId(), name: 'Verniz Base', unitCost: 10.00, unitMeasure: 'L', supplier: 'BrilhoMax', contact: 'vendas@brilhomax.com' },
                ];

                products = [
                    { id: generateId(), name: 'Tinta Vermelha 1L', cost: 12.50, price: 35.00, category: 'Tintas', lastUpdated: new Date().toISOString() },
                    { id: generateId(), name: 'Resina Acrílica (Produto)', cost: 20.00, price: 50.00, category: 'Resinas', lastUpdated: new Date().toISOString() },
                    { id: generateId(), name: 'Pigmento Azul (Produto)', cost: 45.00, price: 120.00, category: 'Pigmentos', lastUpdated: new Date().toISOString() },
                    { id: generateId(), name: 'Verniz Fosco 500ml', cost: 8.00, price: 25.00, category: 'Vernizes', lastUpdated: new Date().toISOString() },
                    { id: generateId(), name: 'Diluente Industrial 5L', cost: 5.00, price: 18.00, category: 'Diluentes', lastUpdated: new Date().toISOString() },
                ];

                // Example formula for 'Tinta Vermelha 1L'
                const tintaVermelha = products.find(p => p.name === 'Tinta Vermelha 1L');
                if (tintaVermelha) {
                    const resina = rawMaterials.find(rm => rm.name === 'Resina Acrílica');
                    const pigmentoVermelho = rawMaterials.find(rm => rm.name === 'Pigmento Vermelho');
                    const etilenoGlicol = rawMaterials.find(rm => rm.name === 'Etileno Glicol');

                    if (resina && pigmentoVermelho && etilenoGlicol) {
                        formulas.push({
                            productId: tintaVermelha.id,
                            ingredients: [
                                { rawMaterialId: resina.id, quantity: 0.3 },
                                { rawMaterialId: pigmentoVermelho.id, quantity: 0.05 },
                                { rawMaterialId: etilenoGlicol.id, quantity: 0.1 }
                            ]
                        });
                        // Update product cost based on formula
                        const formulaCost = (resina.unitCost * 0.3) + (pigmentoVermelho.unitCost * 0.05) + (etilenoGlicol.unitCost * 0.1);
                        tintaVermelha.cost = formulaCost;
                        tintaVermelha.price = formulaCost * 2.8; // Example price
                    }
                }

                saveData();
            }
        }

        // --- Theme Management ---
        function applyTheme(theme) {
            document.body.className = theme + '-theme';
            localStorage.setItem('althacor_theme', theme);
            const icon = document.getElementById('theme-toggle').querySelector('i');
            if (theme === 'dark') {
                icon.classList.remove('fa-moon');
                icon.classList.add('fa-sun');
            } else {
                icon.classList.remove('fa-sun');
                icon.classList.add('fa-moon');
            }
            // Re-render charts to apply new theme colors
            renderDashboard();
        }

        // --- Navigation ---
        function navigateTo(viewId) {
            document.querySelectorAll('.view').forEach(view => view.classList.remove('active'));
            document.getElementById(viewId).classList.add('active');

            document.querySelectorAll('#sidebar nav ul li a').forEach(link => link.classList.remove('active'));
            document.querySelector(`#sidebar nav ul li a[data-view="${viewId}"]`).classList.add('active');

            const viewTitle = document.querySelector(`#sidebar nav ul li a[data-view="${viewId}"]`).textContent.trim();
            document.getElementById('current-view-title').textContent = viewTitle;

            // Render specific view content
            if (viewId === 'dashboard-view') renderDashboard();
            else if (viewId === 'calculator-view') renderCalculator();
            else if (viewId === 'products-view') renderProducts();
            else if (viewId === 'raw-materials-view') renderRawMaterials();
            else if (viewId === 'formulas-view') renderFormulas();
            else if (viewId === 'reports-view') renderReports();
        }

        // --- Modals ---
        function openModal(modalId) {
            document.getElementById(modalId).style.display = 'flex';
        }

        function closeModal(modalId) {
            document.getElementById(modalId).style.display = 'none';
        }

        // --- Dashboard View ---
        function renderDashboard() {
            // Calculate KPIs
            let totalProducts = products.length;
            let totalMargin = products.reduce((sum, p) => sum + p.margin, 0);
            let avgMargin = totalProducts > 0 ? totalMargin / totalProducts : 0;

            let mostProfitable = products.reduce((max, p) => (p.profitPerUnit > max.profitPerUnit ? p : max), { profitPerUnit: -Infinity });
            let leastMargin = products.reduce((min, p) => (p.margin < min.margin ? p : min), { margin: Infinity });

            document.getElementById('kpi-avg-margin').textContent = `${avgMargin.toFixed(2)}%`;
            document.getElementById('kpi-most-profitable').textContent = mostProfitable.name && mostProfitable.profitPerUnit !== -Infinity ? `${mostProfitable.name} (${formatCurrency(mostProfitable.profitPerUnit)})` : 'N/A';
            document.getElementById('kpi-least-margin').textContent = leastMargin.name && leastMargin.margin !== Infinity ? `${leastMargin.name} (${leastMargin.margin.toFixed(2)}%)` : 'N/A';
            document.getElementById('kpi-total-products').textContent = totalProducts;

            // Chart Data
            const productNames = products.map(p => p.name);
            const productMargins = products.map(p => p.margin);
            const productCategories = [...new Set(products.map(p => p.category))];
            const categoryCounts = productCategories.map(cat => products.filter(p => p.category === cat).length);
            const productProfits = products.map(p => p.profitPerUnit);
            const productPrices = products.map(p => p.price);

            const getChartColors = () => {
                const isDark = document.body.classList.contains('dark-theme');
                return {
                    fontColor: isDark ? 'var(--text-light)' : 'var(--text-dark)',
                    gridColor: isDark ? 'rgba(255,255,255,0.1)' : 'rgba(0,0,0,0.1)',
                    tooltipBg: isDark ? 'var(--card-bg-dark)' : 'var(--card-bg-light)',
                    tooltipBorder: isDark ? 'var(--border-dark)' : 'var(--border-light)',
                    chartBg: [
                        'rgba(30, 64, 175, 0.7)', 'rgba(59, 130, 246, 0.7)', 'rgba(96, 165, 250, 0.7)',
                        'rgba(16, 185, 129, 0.7)', 'rgba(52, 211, 153, 0.7)', 'rgba(245, 158, 11, 0.7)',
                        'rgba(251, 191, 36, 0.7)', 'rgba(239, 68, 68, 0.7)', 'rgba(252, 165, 165, 0.7)'
                    ],
                    chartBorder: [
                        'rgba(30, 64, 175, 1)', 'rgba(59, 130, 246, 1)', 'rgba(96, 165, 250, 1)',
                        'rgba(16, 185, 129, 1)', 'rgba(52, 211, 153, 1)', 'rgba(245, 158, 11, 1)',
                        'rgba(251, 191, 36, 1)', 'rgba(239, 68, 68, 1)', 'rgba(252, 165, 165, 1)'
                    ]
                };
            };
            const chartColors = getChartColors();

            // Destroy existing charts before re-rendering
            if (marginChartInstance) marginChartInstance.destroy();
            if (categoryChartInstance) categoryChartInstance.destroy();
            if (profitPriceChartInstance) profitPriceChartInstance.destroy();

            // Margin Chart
            const marginCtx = document.getElementById('marginChart').getContext('2d');
            marginChartInstance = new Chart(marginCtx, {
                type: 'bar',
                data: {
                    labels: productNames,
                    datasets: [{
                        label: 'Margem (%)',
                        data: productMargins,
                        backgroundColor: productMargins.map(m => m >= 50 ? chartColors.chartBg[3] : (m >= 30 ? chartColors.chartBg[5] : chartColors.chartBg[7])),
                        borderColor: productMargins.map(m => m >= 50 ? chartColors.chartBorder[3] : (m >= 30 ? chartColors.chartBorder[5] : chartColors.chartBorder[7])),
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: true,
                            title: { display: true, text: 'Margem (%)', color: chartColors.fontColor },
                            ticks: { color: chartColors.fontColor },
                            grid: { color: chartColors.gridColor }
                        },
                        x: {
                            ticks: { color: chartColors.fontColor },
                            grid: { color: chartColors.gridColor }
                        }
                    },
                    plugins: {
                        legend: { labels: { color: chartColors.fontColor } },
                        tooltip: {
                            backgroundColor: chartColors.tooltipBg,
                            borderColor: chartColors.tooltipBorder,
                            borderWidth: 1,
                            titleColor: chartColors.fontColor,
                            bodyColor: chartColors.fontColor
                        }
                    }
                }
            });

            // Category Chart
            const categoryCtx = document.getElementById('categoryChart').getContext('2d');
            categoryChartInstance = new Chart(categoryCtx, {
                type: 'pie',
                data: {
                    labels: productCategories,
                    datasets: [{
                        data: categoryCounts,
                        backgroundColor: chartColors.chartBg,
                        borderColor: chartColors.chartBorder,
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { position: 'top', labels: { color: chartColors.fontColor } },
                        tooltip: {
                            backgroundColor: chartColors.tooltipBg,
                            borderColor: chartColors.tooltipBorder,
                            borderWidth: 1,
                            titleColor: chartColors.fontColor,
                            bodyColor: chartColors.fontColor
                        }
                    }
                }
            });

            // Profit vs Price Chart
            const profitPriceCtx = document.getElementById('profitPriceChart').getContext('2d');
            profitPriceChartInstance = new Chart(profitPriceCtx, {
                type: 'scatter',
                data: {
                    datasets: [{
                        label: 'Lucro Unitário vs Preço',
                        data: products.map(p => ({ x: p.price, y: p.profitPerUnit, name: p.name })),
                        backgroundColor: chartColors.chartBg[0],
                        borderColor: chartColors.chartBorder[0],
                        borderWidth: 1,
                        pointRadius: 5
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            type: 'linear',
                            position: 'bottom',
                            title: { display: true, text: 'Preço (R$)', color: chartColors.fontColor },
                            ticks: { color: chartColors.fontColor },
                            grid: { color: chartColors.gridColor }
                        },
                        y: {
                            title: { display: true, text: 'Lucro Unitário (R$)', color: chartColors.fontColor },
                            ticks: { color: chartColors.fontColor },
                            grid: { color: chartColors.gridColor }
                        }
                    },
                    plugins: {
                        legend: { labels: { color: chartColors.fontColor } },
                        tooltip: {
                            backgroundColor: chartColors.tooltipBg,
                            borderColor: chartColors.tooltipBorder,
                            borderWidth: 1,
                            titleColor: chartColors.fontColor,
                            bodyColor: chartColors.fontColor,
                            callbacks: {
                                label: function(context) {
                                    const item = context.raw;
                                    return `${item.name}: Preço ${formatCurrency(item.x)}, Lucro ${formatCurrency(item.y)}`;
                                }
                            }
                        }
                    }
                }
            });
        }

        // --- Calculator View ---
        let selectedProductForCalc = null;

        function renderCalculator() {
            document.getElementById('calc-product-search').value = '';
            document.getElementById('calc-search-results').innerHTML = '';
            document.getElementById('calc-search-results').style.display = 'none';
            document.getElementById('calc-product-details').style.display = 'none';
            document.getElementById('calc-simulation').style.display = 'none';
            selectedProductForCalc = null;
        }

        document.getElementById('calc-product-search').addEventListener('input', function() {
            const searchTerm = this.value.toLowerCase();
            const resultsDiv = document.getElementById('calc-search-results');
            resultsDiv.innerHTML = '';

            if (searchTerm.length > 1) {
                const filteredProducts = products.filter(p => p.name.toLowerCase().includes(searchTerm));
                if (filteredProducts.length > 0) {
                    filteredProducts.forEach(p => {
                        const div = document.createElement('div');
                        div.textContent = p.name;
                        div.dataset.productId = p.id;
                        div.addEventListener('click', () => selectProductForCalc(p.id));
                        resultsDiv.appendChild(div);
                    });
                    resultsDiv.style.display = 'block';
                } else {
                    resultsDiv.style.display = 'none';
                }
            } else {
                resultsDiv.style.display = 'none';
            }
        });

        function selectProductForCalc(productId) {
            selectedProductForCalc = products.find(p => p.id === productId);
            if (selectedProductForCalc) {
                document.getElementById('calc-product-search').value = selectedProductForCalc.name;
                document.getElementById('calc-search-results').style.display = 'none';

                document.getElementById('calc-product-name').textContent = selectedProductForCalc.name;
                document.getElementById('calc-product-cost').textContent = formatCurrency(selectedProductForCalc.cost);
                document.getElementById('calc-product-price').textContent = formatCurrency(selectedProductForCalc.price);
                document.getElementById('calc-product-margin').textContent = `${selectedProductForCalc.margin.toFixed(2)}%`;
                document.getElementById('calc-product-profit').textContent = formatCurrency(selectedProductForCalc.profitPerUnit);
                document.getElementById('calc-product-details').style.display = 'block';

                document.getElementById('calc-simulated-price').value = selectedProductForCalc.price.toFixed(2);
                document.getElementById('calc-simulation').style.display = 'block';
                simulatePriceChange(); // Initial simulation
            }
        }

        document.getElementById('calc-simulated-price').addEventListener('input', simulatePriceChange);

        function simulatePriceChange() {
            if (!selectedProductForCalc) return;

            const simulatedPrice = parseFloat(document.getElementById('calc-simulated-price').value);
            const cost = selectedProductForCalc.cost;

                document.getElementById('calc-new-margin').textContent = 'N/A';
                document.getElementById('calc-new-profit').textContent = 'N/A';
                document.getElementById('calc-profit-diff').textContent = 'N/A';
                document.getElementById('calc-status').textContent = '';
                document.getElementById('calc-status').className = 'status-badge';
                document.getElementById('calc-impact-10').textContent = 'N/A';
                document.getElementById('calc-impact-50').textContent = 'N/A';
                document.getElementById('calc-impact-100').textContent = 'N/A';
                return;
            }

            const newProfitPerUnit = simulatedPrice - cost;
            const newMargin = (newProfitPerUnit / simulatedPrice) * 100;
            const profitDiff = newProfitPerUnit - selectedProductForCalc.profitPerUnit;

            const status = getMarginStatus(newMargin);

            document.getElementById('calc-new-margin').textContent = `${newMargin.toFixed(2)}%`;
            document.getElementById('calc-new-profit').textContent = formatCurrency(newProfitPerUnit);
            document.getElementById('calc-profit-diff').textContent = formatCurrency(profitDiff);
            document.getElementById('calc-status').textContent = status.text;
            document.getElementById('calc-status').className = `status-badge ${status.class}`;

            document.getElementById('calc-impact-10').textContent = formatCurrency(newProfitPerUnit * 10);
            document.getElementById('calc-impact-50').textContent = formatCurrency(newProfitPerUnit * 50);
            document.getElementById('calc-impact-100').textContent = formatCurrency(newProfitPerUnit * 100);
        }

        // --- Products View ---
        function renderProducts() {
            const tableBody = document.getElementById('products-table-body');
            tableBody.innerHTML = '';

            const searchTerm = document.getElementById('product-search').value.toLowerCase();
            const categoryFilter = document.getElementById('product-category-filter').value;
            const marginFilter = document.getElementById('product-margin-filter').value;

            let filteredProducts = products.filter(p => p.name.toLowerCase().includes(searchTerm));

            if (categoryFilter) {
                filteredProducts = filteredProducts.filter(p => p.category === categoryFilter);
            }

            if (marginFilter) {
                filteredProducts = filteredProducts.filter(p => {
                    if (marginFilter === 'low') return p.margin < 30;
                    if (marginFilter === 'medium') return p.margin >= 30 && p.margin < 50;
                    if (marginFilter === 'high') return p.margin >= 50;
                    return true;
                });
            }

            filteredProducts.forEach(product => {
                const row = tableBody.insertRow();
                row.innerHTML = `
                    <td>${product.name}</td>
                    <td>${formatCurrency(product.cost)}</td>
                    <td>${formatCurrency(product.price)}</td>
                    <td class="${getMarginClass(product.margin)}">${product.margin.toFixed(2)}%</td>
                    <td>${formatCurrency(product.profitPerUnit)}</td>
                    <td>${product.category}</td>
                    <td>${formatDate(product.lastUpdated)}</td>
                    <td class="action-buttons">
                        <button class="btn btn-info" onclick="editProduct('${product.id}')"><i class="fas fa-edit"></i></button>
                        <button class="btn btn-danger" onclick="deleteProduct('${product.id}')"><i class="fas fa-trash"></i></button>
                    </td>
                `;
            });

            // Populate category filter dropdown
            const categoryFilterSelect = document.getElementById('product-category-filter');
            const currentCategories = [...new Set(products.map(p => p.category))];
            const selectedCategory = categoryFilterSelect.value; // Preserve selected value
            categoryFilterSelect.innerHTML = '<option value="">Todas as Categorias</option>';
            currentCategories.forEach(cat => {
                const option = document.createElement('option');
                option.value = cat;
                option.textContent = cat;
                if (cat === selectedCategory) option.selected = true;
                categoryFilterSelect.appendChild(option);
            });
        }

        function addProduct() {
            document.getElementById('product-modal-title').textContent = 'Adicionar Produto';
            document.getElementById('product-form').reset();
            document.getElementById('product-id').value = '';
            openModal('product-modal');
        }

        function editProduct(id) {
            const product = products.find(p => p.id === id);
            if (product) {
                document.getElementById('product-modal-title').textContent = 'Editar Produto';
                document.getElementById('product-id').value = product.id;
                document.getElementById('product-name').value = product.name;
                document.getElementById('product-cost').value = product.cost;
                document.getElementById('product-price').value = product.price;
                document.getElementById('product-category').value = product.category;
                openModal('product-modal');
            }
        }

        document.getElementById('product-form').addEventListener('submit', function(event) {
            event.preventDefault();
            const id = document.getElementById('product-id').value;
            const name = document.getElementById('product-name').value;
            const cost = parseFloat(document.getElementById('product-cost').value);
            const price = parseFloat(document.getElementById('product-price').value);
            const category = document.getElementById('product-category').value;

            if (id) {
                // Edit existing product
                const index = products.findIndex(p => p.id === id);
                if (index !== -1) {
                    products[index].name = name;
                    products[index].cost = cost;
                    products[index].price = price;
                    products[index].category = category;
                    products[index].lastUpdated = new Date().toISOString();
                    products[index] = calculateProductMetrics(products[index]);
                }
            } else {
                // Add new product
                const newProduct = calculateProductMetrics({
                    id: generateId(),
                    name,
                    cost,
                    price,
                    category,
                    lastUpdated: new Date().toISOString()
                });
                products.push(newProduct);
            }
            saveData();
            renderProducts();
            closeModal('product-modal');
        });

        function deleteProduct(id) {
            if (confirm('Tem certeza que deseja excluir este produto?')) {
                products = products.filter(p => p.id !== id);
                // Also remove any formulas associated with this product
                formulas = formulas.filter(f => f.productId !== id);
                saveData();
                renderProducts();
            }
        }

        document.getElementById('product-search').addEventListener('input', renderProducts);
        document.getElementById('product-category-filter').addEventListener('change', renderProducts);
        document.getElementById('product-margin-filter').addEventListener('change', renderProducts);
        document.getElementById('add-product-btn').addEventListener('click', addProduct);
        document.getElementById('export-products-csv-btn').addEventListener('click', () => exportTableToCSV('products-table-body', 'relatorio_produtos.csv'));


        // --- Raw Materials View ---
        function renderRawMaterials() {
            const tableBody = document.getElementById('raw-materials-table-body');
            tableBody.innerHTML = '';

            const searchTerm = document.getElementById('raw-material-search').value.toLowerCase();
            const filteredRawMaterials = rawMaterials.filter(rm => rm.name.toLowerCase().includes(searchTerm));

            filteredRawMaterials.forEach(rm => {
                const row = tableBody.insertRow();
                row.innerHTML = `
                    <td>${rm.name}</td>
                    <td>${formatCurrency(rm.unitCost)}</td>
                    <td>${rm.unitMeasure}</td>
                    <td class="action-buttons">
                        <button class="btn btn-info" onclick="editRawMaterial('${rm.id}')"><i class="fas fa-edit"></i></button>
                        <button class="btn btn-danger" onclick="deleteRawMaterial('${rm.id}')"><i class="fas fa-trash"></i></button>
                    </td>
                `;
            });
        }

        function addRawMaterial() {
            document.getElementById('raw-material-modal-title').textContent = 'Adicionar Matéria-Prima';
            document.getElementById('raw-material-form').reset();
            document.getElementById('raw-material-id').value = '';
            openModal('raw-material-modal');
        }

        function editRawMaterial(id) {
            const rm = rawMaterials.find(r => r.id === id);
            if (rm) {
                document.getElementById('raw-material-modal-title').textContent = 'Editar Matéria-Prima';
                document.getElementById('raw-material-id').value = rm.id;
                document.getElementById('raw-material-name').value = rm.name;
                document.getElementById('raw-material-unit-cost').value = rm.unitCost;
                document.getElementById('raw-material-unit-measure').value = rm.unitMeasure;
                document.getElementById('raw-material-supplier').value = rm.supplier;
                document.getElementById('raw-material-contact').value = rm.contact;
                openModal('raw-material-modal');
            }
        }

        document.getElementById('raw-material-form').addEventListener('submit', function(event) {
            event.preventDefault();
            const id = document.getElementById('raw-material-id').value;
            const name = document.getElementById('raw-material-name').value;
            const unitCost = parseFloat(document.getElementById('raw-material-unit-cost').value);
            const unitMeasure = document.getElementById('raw-material-unit-measure').value;
            const supplier = document.getElementById('raw-material-supplier').value;
            const contact = document.getElementById('raw-material-contact').value;

            if (id) {
                const index = rawMaterials.findIndex(r => r.id === id);
                if (index !== -1) {
                    rawMaterials[index] = { id, name, unitCost, unitMeasure, supplier, contact };
                }
            } else {
                rawMaterials.push({ id: generateId(), name, unitCost, unitMeasure, supplier, contact });
            }
            saveData();
            renderRawMaterials();
            closeModal('raw-material-modal');
            // Re-render formulas view to update dropdowns
            renderFormulas();
        });

        function deleteRawMaterial(id) {
            if (confirm('Tem certeza que deseja excluir esta matéria-prima? Isso pode afetar fórmulas existentes.')) {
                rawMaterials = rawMaterials.filter(rm => rm.id !== id);
                // Remove raw material from any formulas
                formulas.forEach(f => {
                    f.ingredients = f.ingredients.filter(ing => ing.rawMaterialId !== id);
                });
                saveData();
                renderRawMaterials();
                renderFormulas(); // Update formulas view
            }
        }

        document.getElementById('raw-material-search').addEventListener('input', renderRawMaterials);
        document.getElementById('add-raw-material-btn').addEventListener('click', addRawMaterial);

        // --- Formulas View ---
        let currentFormulaIngredients = []; // Temporary storage for ingredients being added to a formula

        function renderFormulas() {
            const productSelect = document.getElementById('formula-product-select');
            productSelect.innerHTML = '<option value="">Selecione um produto</option>';
            products.forEach(p => {
                const option = document.createElement('option');
                option.value = p.id;
                option.textContent = p.name;
                productSelect.appendChild(option);
            });

            const rawMaterialSelect = document.getElementById('formula-raw-material-select');
            rawMaterialSelect.innerHTML = '<option value="">Selecione uma matéria-prima</option>';
            rawMaterials.forEach(rm => {
                const option = document.createElement('option');
                option.value = rm.id;
                option.textContent = `${rm.name} (${formatCurrency(rm.unitCost)}/${rm.unitMeasure})`;
                rawMaterialSelect.appendChild(option);
            });

            // Reset formula builder
            currentFormulaIngredients = [];
            document.getElementById('formula-quantity').value = '';
            document.getElementById('formula-desired-margin').value = 30;
            updateFormulaDisplay();
        }

        document.getElementById('formula-product-select').addEventListener('change', function() {
            const productId = this.value;
            currentFormulaIngredients = [];
            if (productId) {
                const existingFormula = formulas.find(f => f.productId === productId);
                if (existingFormula) {
                    currentFormulaIngredients = JSON.parse(JSON.stringify(existingFormula.ingredients)); // Deep copy
                }
            }
            updateFormulaDisplay();
        });

        document.getElementById('add-formula-ingredient-btn').addEventListener('click', function() {
            const rawMaterialId = document.getElementById('formula-raw-material-select').value;
            const quantity = parseFloat(document.getElementById('formula-quantity').value);

                alert('Por favor, selecione uma matéria-prima e insira uma quantidade válida.');
                return;
            }

            const rawMaterial = rawMaterials.find(rm => rm.id === rawMaterialId);
            if (!rawMaterial) {
                alert('Matéria-prima não encontrada.');
                return;
            }

            const existingIngredientIndex = currentFormulaIngredients.findIndex(ing => ing.rawMaterialId === rawMaterialId);
            if (existingIngredientIndex !== -1) {
                currentFormulaIngredients[existingIngredientIndex].quantity += quantity;
            } else {
                currentFormulaIngredients.push({ rawMaterialId, quantity });
            }

            document.getElementById('formula-raw-material-select').value = '';
            document.getElementById('formula-quantity').value = '';
            updateFormulaDisplay();
        });

        function removeFormulaIngredient(index) {
            currentFormulaIngredients.splice(index, 1);
            updateFormulaDisplay();
        }

        function updateFormulaDisplay() {
            const ingredientsListDiv = document.getElementById('formula-ingredients-list');
            ingredientsListDiv.innerHTML = '';
            let totalCost = 0;

            if (currentFormulaIngredients.length === 0) {
                ingredientsListDiv.innerHTML = '<p style="text-align: center; color: var(--secondary-gray);">Nenhum ingrediente adicionado.</p>';
            } else {
                currentFormulaIngredients.forEach((ing, index) => {
                    const rm = rawMaterials.find(r => r.id === ing.rawMaterialId);
                    if (rm) {
                        const ingredientCost = rm.unitCost * ing.quantity;
                        totalCost += ingredientCost;
                        const itemDiv = document.createElement('div');
                        itemDiv.className = 'formula-ingredient-item';
                        itemDiv.innerHTML = `
                            <span>${rm.name} (${ing.quantity} ${rm.unitMeasure})</span>
                            <span>${formatCurrency(ingredientCost)} <button class="btn btn-danger" style="margin-left: 10px; padding: 5px 8px;" onclick="removeFormulaIngredient(${index})"><i class="fas fa-trash"></i></button></span>
                        `;
                        ingredientsListDiv.appendChild(itemDiv);
                    }
                });
            }

            document.getElementById('formula-total-cost').textContent = formatCurrency(totalCost);
            calculateSuggestedPrice(totalCost);
        }

        document.getElementById('formula-desired-margin').addEventListener('input', function() {
            const totalCost = parseFloat(document.getElementById('formula-total-cost').textContent.replace('R$', '').replace(',', '.'));
            calculateSuggestedPrice(totalCost);
        });

        function calculateSuggestedPrice(cost) {
            const desiredMargin = parseFloat(document.getElementById('formula-desired-margin').value);
            let suggestedPrice = 0;
            if (cost > 0 && desiredMargin >= 0 && desiredMargin < 100) {
                suggestedPrice = cost / (1 - (desiredMargin / 100));
            } else if (desiredMargin === 100) {
                suggestedPrice = cost * 1000; // Effectively infinite margin, set a very high price
            } else {
                suggestedPrice = cost; // If margin is 0 or invalid, price is cost
            }
            document.getElementById('formula-suggested-price').textContent = formatCurrency(suggestedPrice);
        }

        document.getElementById('save-formula-btn').addEventListener('click', function() {
            const productId = document.getElementById('formula-product-select').value;
            if (!productId) {
                alert('Por favor, selecione um produto para salvar a fórmula.');
                return;
            }

            const product = products.find(p => p.id === productId);
            if (!product) {
                alert('Produto não encontrado.');
                return;
            }

            const totalCost = parseFloat(document.getElementById('formula-total-cost').textContent.replace('R$', '').replace(',', '.'));
            const suggestedPrice = parseFloat(document.getElementById('formula-suggested-price').textContent.replace('R$', '').replace(',', '.'));

            // Update product cost and price
            product.cost = totalCost;
            product.price = suggestedPrice;
            product.lastUpdated = new Date().toISOString();
            calculateProductMetrics(product);

            // Save/update formula
            const existingFormulaIndex = formulas.findIndex(f => f.productId === productId);
            if (existingFormulaIndex !== -1) {
                formulas[existingFormulaIndex].ingredients = currentFormulaIngredients;
            } else {
                formulas.push({ productId, ingredients: currentFormulaIngredients });
            }

            saveData();
            alert('Fórmula salva e produto atualizado com sucesso!');
            renderProducts(); // Refresh products view
            renderFormulas(); // Reset formula builder
        });

        // --- Reports View ---
        let currentReportData = []; // To store data for CSV export

        function renderReports() {
            document.getElementById('report-output').innerHTML = '';
            document.getElementById('export-report-csv-btn').style.display = 'none';
            currentReportData = [];
        }

        document.getElementById('generate-general-report-btn').addEventListener('click', function() {
            const reportOutput = document.getElementById('report-output');
            reportOutput.innerHTML = `
                <h3>Relatório Geral de Produtos</h3>
                <table class="data-table">
                    <thead>
                        <tr>
                            <th>Produto</th>
                            <th>Custo</th>
                            <th>Preço</th>
                            <th>Margem %</th>
                            <th>Lucro/Unit</th>
                            <th>Categoria</th>
                            <th>Atualizado em</th>
                        </tr>
                    </thead>
                    <tbody id="general-report-table-body"></tbody>
                </table>
            `;
            const tableBody = document.getElementById('general-report-table-body');
            currentReportData = [];

            products.forEach(p => {
                const row = tableBody.insertRow();
                row.innerHTML = `
                    <td>${p.name}</td>
                    <td>${formatCurrency(p.cost)}</td>
                    <td>${formatCurrency(p.price)}</td>
                    <td class="${getMarginClass(p.margin)}">${p.margin.toFixed(2)}%</td>
                    <td>${formatCurrency(p.profitPerUnit)}</td>
                    <td>${p.category}</td>
                    <td>${formatDate(p.lastUpdated)}</td>
                `;
                currentReportData.push([p.name, p.cost, p.price, p.margin.toFixed(2), p.profitPerUnit, p.category, formatDate(p.lastUpdated)]);
            });
            document.getElementById('export-report-csv-btn').style.display = 'inline-flex';
        });

        document.getElementById('generate-abc-report-btn').addEventListener('click', function() {
            const reportOutput = document.getElementById('report-output');
            reportOutput.innerHTML = `
                <h3>Análise ABC (Pareto) - Por Lucro Unitário</h3>
                <p>Classifica produtos pela contribuição de lucro. A (maior lucro), B (médio), C (menor).</p>
                <table class="data-table">
                    <thead>
                        <tr>
                            <th>Produto</th>
                            <th>Lucro/Unit</th>
                            <th>Margem %</th>
                            <th>Categoria</th>
                            <th>Classe ABC</th>
                        </tr>
                    </thead>
                    <tbody id="abc-report-table-body"></tbody>
                </table>
            `;
            const tableBody = document.getElementById('abc-report-table-body');
            currentReportData = [];

            const sortedProducts = [...products].sort((a, b) => b.profitPerUnit - a.profitPerUnit);
            const totalProfit = sortedProducts.reduce((sum, p) => sum + p.profitPerUnit, 0);

            let cumulativeProfit = 0;
            sortedProducts.forEach(p => {
                cumulativeProfit += p.profitPerUnit;
                const cumulativePercentage = (cumulativeProfit / totalProfit) * 100;
                let abcClass = 'C';
                if (cumulativePercentage <= 80) abcClass = 'A';
                else if (cumulativePercentage <= 95) abcClass = 'B';

                const row = tableBody.insertRow();
                row.innerHTML = `
                    <td>${p.name}</td>
                    <td>${formatCurrency(p.profitPerUnit)}</td>
                    <td class="${getMarginClass(p.margin)}">${p.margin.toFixed(2)}%</td>
                    <td>${p.category}</td>
                    <td>${abcClass}</td>
                `;
                currentReportData.push([p.name, p.profitPerUnit, p.margin.toFixed(2), p.category, abcClass]);
            });
            document.getElementById('export-report-csv-btn').style.display = 'inline-flex';
        });

        document.getElementById('generate-low-margin-report-btn').addEventListener('click', function() {
            const reportOutput = document.getElementById('report-output');
            reportOutput.innerHTML = `
                <h3>Produtos com Margem Baixa (< 30%)</h3>
                <p>Estes produtos podem precisar de revisão de custo ou preço.</p>
                <table class="data-table">
                    <thead>
                        <tr>
                            <th>Produto</th>
                            <th>Custo</th>
                            <th>Preço</th>
                            <th>Margem %</th>
                            <th>Lucro/Unit</th>
                            <th>Categoria</th>
                        </tr>
                    </thead>
                    <tbody id="low-margin-report-table-body"></tbody>
                </table>
            `;
            const tableBody = document.getElementById('low-margin-report-table-body');
            currentReportData = [];

            const lowMarginProducts = products.filter(p => p.margin < 30).sort((a, b) => a.margin - b.margin);

            if (lowMarginProducts.length === 0) {
                tableBody.innerHTML = '<tr><td colspan="6" style="text-align: center;">Nenhum produto com margem baixa encontrado.</td></tr>';
            } else {
                lowMarginProducts.forEach(p => {
                    const row = tableBody.insertRow();
                    row.innerHTML = `
                        <td>${p.name}</td>
                        <td>${formatCurrency(p.cost)}</td>
                        <td>${formatCurrency(p.price)}</td>
                        <td class="${getMarginClass(p.margin)}">${p.margin.toFixed(2)}%</td>
                        <td>${formatCurrency(p.profitPerUnit)}</td>
                        <td>${p.category}</td>
                    `;
                    currentReportData.push([p.name, p.cost, p.price, p.margin.toFixed(2), p.profitPerUnit, p.category]);
                });
            }
            document.getElementById('export-report-csv-btn').style.display = 'inline-flex';
        });

        document.getElementById('export-report-csv-btn').addEventListener('click', function() {
            if (currentReportData.length === 0) {
                alert('Nenhum dado de relatório para exportar.');
                return;
            }
            const headers = Array.from(document.querySelector('#report-output .data-table thead th')).map(th => th.textContent);
            exportToCSV(headers, currentReportData, 'relatorio_althacor.csv');
        });

        function exportToCSV(headers, data, filename) {
            let csvContent = headers.join(';') + '\n';
            data.forEach(row => {
                csvContent += row.map(item => {
                    if (typeof item === 'string' && item.includes(';')) {
                        return `"${item.replace(/"/g, '""')}"`; // Escape quotes and wrap in quotes
                    }
                    return item;
                }).join(';') + '\n';
            });

            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            if (link.download !== undefined) {
                const url = URL.createObjectURL(blob);
                link.setAttribute('href', url);
                link.setAttribute('download', filename);
                link.style.visibility = 'hidden';
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            }
        }

        // --- Settings View ---
        document.getElementById('set-light-theme').addEventListener('click', () => applyTheme('light'));
        document.getElementById('set-dark-theme').addEventListener('click', () => applyTheme('dark'));

        document.getElementById('backup-data-btn').addEventListener('click', function() {
            const data = { products, rawMaterials, formulas };
            const jsonString = JSON.stringify(data, null, 2);
            const blob = new Blob([jsonString], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `althacor_backup_${new Date().toISOString().slice(0, 10)}.json`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            alert('Backup dos dados salvo com sucesso!');
        });

        document.getElementById('restore-data-btn').addEventListener('click', function() {
            document.getElementById('restore-data-input').click();
        });

        document.getElementById('restore-data-input').addEventListener('change', function(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        const restoredData = JSON.parse(e.target.result);
                        if (restoredData.products && restoredData.rawMaterials && restoredData.formulas) {
                            products = restoredData.products.map(p => calculateProductMetrics(p));
                            rawMaterials = restoredData.rawMaterials;
                            formulas = restoredData.formulas;
                            saveData();
                            alert('Dados restaurados com sucesso! Recarregando a aplicação...');
                            location.reload(); // Reload to apply all changes
                        } else {
                            alert('Arquivo JSON inválido. Certifique-se de que contém "products", "rawMaterials" e "formulas".');
                        }
                    } catch (error) {
                        alert('Erro ao ler o arquivo JSON: ' + error.message);
                    }
                };
                reader.readAsText(file);
            }
        });

        document.getElementById('export-all-data-btn').addEventListener('click', function() {
            const data = { products, rawMaterials, formulas };
            const jsonString = JSON.stringify(data, null, 2);
            const blob = new Blob([jsonString], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `althacor_all_data_${new Date().toISOString().slice(0, 10)}.json`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            alert('Todos os dados exportados para JSON!');
        });

        // --- Initialization ---
        document.addEventListener('DOMContentLoaded', () => {
            loadData();
            initializeExampleData(); // Only runs if no data is loaded
            
            // Apply saved theme or default to light
            applyTheme(savedTheme);

            // Setup navigation
            document.querySelectorAll('#sidebar nav ul li a').forEach(link => {
                link.addEventListener('click', function(event) {
                    event.preventDefault();
                    navigateTo(this.dataset.view);
                });
            });

            // Setup modal close buttons
            document.querySelectorAll('.close-button').forEach(button => {
                button.addEventListener('click', function() {
                    this.closest('.modal').style.display = 'none';
                });
            });
            window.addEventListener('click', function(event) {
                if (event.target.classList.contains('modal')) {
                    event.target.style.display = 'none';
                }
            });

            // Initial view render
            navigateTo('dashboard-view');
        });
    </script>
</body>
</html>
