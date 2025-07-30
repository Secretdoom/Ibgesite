<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Análise Socioeconômica (IBGE)</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /*
         * Global Styles & Variables (Strict GitHub Inspired)
         * Color Palette:
         * - Backgrounds: #0d1117 (darkest), #161b22 (darker), #24292e (dark)
         * - Text: #c9d1d9 (light gray), #ffffff (white)
         * - Primary Accent (Blue): #58a6ff (GitHub blue for links, headings)
         * - Secondary Accents (Grays for UI, Curated Blues/Purples/Oranges for Charts)
         * - Borders: #30363d
         */
        :root {
            --bg-darkest: #0d1117;
            --bg-darker: #161b22;
            --bg-dark: #24292e;
            --text-light: #c9d1d9;
            --text-white: #ffffff;
            --accent-blue: #58a6ff;
            --accent-blue-hover: #4092ff;
            --border-color: #30363d;
        }

        body {
            font-family: 'Inter', 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
            background-color: var(--bg-darkest);
            color: var(--text-light);
            margin: 0;
            padding: 0;
            line-height: 1.6;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        /* --- Header & Navigation --- */
        header {
            background-color: var(--bg-darker);
            padding: 15px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border-color);
            position: sticky;
            top: 0;
            z-index: 1000;
        }

        header h1 {
            margin: 0;
            font-size: 1.8em;
            color: var(--text-white);
            display: flex;
            align-items: center;
            user-select: none;
        }

        header h1 i {
            margin-right: 10px;
            color: var(--accent-blue);
        }

        .nav-links {
            list-style: none;
            margin: 0;
            padding: 0;
            display: flex;
        }

        .nav-links li {
            margin-left: 30px;
        }

        .nav-links a {
            color: var(--text-light);
            text-decoration: none;
            font-weight: 500;
            transition: color 0.3s ease;
        }

        .nav-links a:hover {
            color: var(--accent-blue);
        }

        .hamburger-menu {
            display: none; /* Hidden on desktop */
            font-size: 1.8em;
            color: var(--text-light);
            cursor: pointer;
        }

        .sidebar {
            height: 100%;
            width: 0; /* Hidden by default */
            position: fixed;
            z-index: 1001;
            top: 0;
            right: 0;
            background-color: var(--bg-dark);
            overflow-x: hidden;
            transition: 0.5s;
            padding-top: 60px;
            box-shadow: -5px 0 15px rgba(0, 0, 0, 0.4);
        }

        .sidebar a {
            padding: 15px 25px 15px 35px;
            text-decoration: none;
            font-size: 1.5em;
            color: var(--text-light);
            display: block;
            transition: 0.3s;
        }

        .sidebar a:hover {
            color: var(--accent-blue);
            background-color: var(--border-color);
        }

        .sidebar .closebtn {
            position: absolute;
            top: 0;
            right: 25px;
            font-size: 3em;
            margin-left: 50px;
        }

        /* --- Main Content Layout --- */
        main {
            max-width: 1400px;
            margin: 20px auto;
            padding: 0 20px;
        }

        section {
            background-color: var(--bg-darker);
            border-radius: 8px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            border: 1px solid var(--border-color);
        }

        h2 {
            color: var(--accent-blue);
            border-bottom: 2px solid var(--border-color);
            padding-bottom: 15px;
            margin-bottom: 30px;
            font-size: 2em;
            display: flex;
            align-items: center;
        }

        h2 i {
            margin-right: 12px;
            font-size: 0.9em;
        }

        h3 {
            color: var(--text-white);
            margin-top: 0;
            margin-bottom: 15px;
            font-size: 1.4em;
        }

        .analysis-text {
            background-color: var(--bg-dark);
            border-left: 4px solid var(--accent-blue);
            padding: 15px 20px;
            margin-bottom: 25px;
            border-radius: 4px;
            font-size: 0.95em;
            color: var(--text-light);
        }
        .analysis-text strong {
            color: var(--text-white);
        }


        .chart-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
        }

        .chart-card {
            background-color: var(--bg-dark);
            padding: 20px;
            border-radius: 6px;
            border: 1px solid var(--border-color);
            position: relative;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .chart-canvas-container {
            position: relative;
            height: 300px; /* Fixed height for consistency */
            margin-bottom: 15px;
        }

        canvas {
            width: 100% !important;
            height: 100% !important;
        }

        .copy-graph-btn {
            background-color: var(--accent-blue);
            color: var(--text-white);
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9em;
            font-weight: 600;
            transition: background-color 0.3s ease, transform 0.2s ease;
            align-self: flex-end;
        }

        .copy-graph-btn:hover {
            background-color: var(--accent-blue-hover);
            transform: translateY(-2px);
        }

        /* New: Survey Answer List Styling */
        .survey-answers-list {
            list-style: none;
            padding: 0;
            margin-top: 15px;
            margin-bottom: 20px; /* Space between list and analysis text */
            font-size: 1em;
            color: var(--text-light);
        }
        .survey-answers-list li {
            background-color: var(--bg-darkest);
            padding: 8px 12px;
            margin-bottom: 5px;
            border-radius: 4px;
            border: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .survey-answers-list li strong {
            color: var(--text-white);
        }
        .survey-answers-list li .percentage {
            font-weight: bold;
            color: var(--accent-blue);
            font-size: 1.1em;
        }

        /* --- IBGE Insights Section --- */
        #ibge-insights .ibge-data-card {
            background-color: var(--bg-dark);
            padding: 25px;
            border-radius: 6px;
            border: 1px solid var(--border-color);
            margin-top: 20px;
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
        }

        #ibge-insights .ibge-data-card h3 {
            color: var(--accent-blue);
            margin-bottom: 15px;
            border-bottom: 1px dashed var(--border-color);
            padding-bottom: 10px;
        }

        #ibge-insights .ibge-data-card p {
            font-size: 1.1em;
            color: var(--text-light);
            margin-bottom: 10px;
        }
        #ibge-insights .ibge-data-card ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        #ibge-insights .ibge-data-card ul li {
            background-color: var(--bg-darkest);
            margin-bottom: 8px;
            padding: 10px 15px;
            border-radius: 4px;
            border: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        #ibge-insights .ibge-data-card ul li strong {
            color: var(--text-white);
        }

        #ibge-insights .loading-message,
        #ibge-insights .error-message {
            text-align: center;
            padding: 30px;
            font-style: italic;
            color: var(--text-light); /* Default color */
        }
        #ibge-insights .loading-message i {
            margin-right: 10px;
            color: var(--accent-blue);
        }
        #ibge-insights .error-message {
            color: #dc6060; /* A soft red for errors, distinct from main palette */
        }

        /* IBGE Data Table Styling */
        #ibge-insights table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            background-color: var(--bg-darkest);
            border-radius: 6px;
            overflow: hidden; /* For rounded corners */
        }
        #ibge-insights table th,
        #ibge-insights table td {
            padding: 12px 15px;
            border: 1px solid var(--border-color);
            text-align: left;
            color: var(--text-light);
        }
        #ibge-insights table th {
            background-color: var(--border-color);
            color: var(--text-white);
            font-weight: 600;
            text-align: center;
        }
        #ibge-insights table tbody tr:nth-child(even) {
            background-color: #1a1f26; /* Slightly lighter row for zebra striping */
        }
        #ibge-insights table tbody tr:hover {
            background-color: #2a313b; /* Hover effect */
        }
        #ibge-insights table td:last-child,
        #ibge-insights table th:last-child {
            text-align: right; /* Align numbers to the right */
        }

        /* --- Footer --- */
        footer {
            text-align: center;
            padding: 20px;
            margin-top: 40px;
            color: var(--text-light);
            font-size: 0.9em;
            border-top: 1px solid var(--border-color);
            background-color: var(--bg-darker);
        }

        footer a {
            color: var(--accent-blue);
            text-decoration: none;
        }

        /* --- Responsive Design (Hamburger Menu & Layout) --- */
        @media (max-width: 768px) {
            .nav-links {
                display: none;
            }

            .hamburger-menu {
                display: block;
            }

            .sidebar {
                width: 250px;
            }

            .sidebar.closed {
                width: 0;
            }

            header {
                padding: 15px 20px;
            }

            main {
                padding: 0 15px;
            }

            section {
                padding: 20px;
            }

            h2 {
                font-size: 1.8em;
            }

            h3 {
                font-size: 1.2em;
            }

            .chart-grid {
                grid-template-columns: 1fr;
            }

            .chart-canvas-container {
                height: 250px;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1><i class="fas fa-chart-line"></i> Análise Socioeconômica</h1>
        <nav>
            <ul class="nav-links">
                <li><a href="#sociodemograficas">Sociodemográficas</a></li>
                <li><a href="#trabalho-renda">Trabalho & Renda</a></li>
                <li><a href="#financas-credito">Finanças & Crédito</a></li>
                <li><a href="#ibge-insights">Insights IBGE</a></li>
            </ul>
            <div class="hamburger-menu" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </div>
        </nav>
    </header>

    <div id="mySidebar" class="sidebar closed">
        <a href="javascript:void(0)" class="closebtn" onclick="toggleSidebar()">&times;</a>
        <a href="#sociodemograficas" onclick="toggleSidebar()">Sociodemográficas</a>
        <a href="#trabalho-renda" onclick="toggleSidebar()">Trabalho & Renda</a>
        <a href="#financas-credito" onclick="toggleSidebar()">Finanças & Crédito</a>
        <a href="#ibge-insights" onclick="toggleSidebar()">Insights IBGE</a>
    </div>

    <main>
        <section id="sociodemograficas">
            <h2><i class="fas fa-users"></i> Informações Sociodemográficas (Dados de Pesquisa)</h2>
            <p class="analysis-text">
                Apresentamos os dados simulados de uma pesquisa para entender a composição da população. Essas informações, como gênero, etnia, idade e estrutura familiar, são a base para o desenvolvimento de políticas públicas e estratégias de inclusão mais eficazes.
            </p>
            <div class="chart-grid">
                <div class="chart-card">
                    <h3>Qual seu gênero?</h3>
                    <div class="chart-canvas-container">
                        <canvas id="genderChart"></canvas>
                    </div>
                    <ul class="survey-answers-list" id="gender-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="genderChart" data-chart-title="Gênero">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        A distribuição por gênero é um indicador básico da estrutura demográfica. Entender essa proporção pode influenciar a oferta de serviços e oportunidades, além de ser fundamental para análises de equidade.
                    </p>
                </div>
                <div class="chart-card">
                    <h3>Qual sua cor/raça/etnia?</h3>
                    <div class="chart-canvas-container">
                        <canvas id="ethnicityChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="ethnicity-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="ethnicityChart" data-chart-title="Cor/Raça/Etnia">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        A análise da composição étnica é crucial para identificar e combater desigualdades históricas e estruturais. Dados desagregados por cor/raça são essenciais para promover a equidade e representatividade em diversos setores da sociedade.
                    </p>
                </div>
                <div class="chart-card">
                    <h3>Faixa Etária</h3>
                    <div class="chart-canvas-container">
                        <canvas id="ageChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="age-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="ageChart" data-chart-title="Faixa Etária">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        A distribuição etária revela se a população é majoritariamente jovem, adulta ou idosa. Isso tem grandes implicações para o mercado de trabalho, sistemas de previdência, educação e saúde.
                    </p>
                </div>
                <div class="chart-card">
                    <h3>Com quantas pessoas você mora?</h3>
                    <div class="chart-canvas-container">
                        <canvas id="householdChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="household-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="householdChart" data-chart-title="Pessoas no Domicílio">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        O tamanho do domicílio pode indicar padrões de moradia, densidade populacional e a necessidade de infraestrutura habitacional. Famílias maiores podem ter desafios diferentes de famílias menores de recursos e espaço.
                    </p>
                </div>
            </div>
        </section>

        <section id="trabalho-renda">
            <h2><i class="fas fa-briefcase"></i> Situação de Trabalho e Renda (Dados de Pesquisa)</h2>
            <p class="analysis-text">
                Compreender a dinâmica do trabalho e da renda é vital para avaliar a saúde econômica de uma população. Esses dados indicam níveis de emprego, tipos de ocupação e as principais fontes de sustento.
            </p>
            <div class="chart-grid">
                <div class="chart-card">
                    <h3>Você está atualmente trabalhando?</h3>
                    <div class="chart-canvas-container">
                        <canvas id="workingChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="working-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="workingChart" data-chart-title="Situação de Trabalho">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        A taxa de ocupação é um dos principais indicadores econômicos. Uma alta taxa de pessoas trabalhando geralmente reflete um mercado aquecido e maior geração de renda.
                    </p>
                </div>
                <div class="chart-card">
                    <h3>Se sim, quantos trabalhos remunerados você tem?</h3>
                    <div class="chart-canvas-container">
                        <canvas id="jobsCountChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="jobsCount-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="jobsCountChart" data-chart-title="Quantidade de Trabalhos">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        A quantidade de trabalhos por pessoa pode indicar a necessidade de complementar a renda, a flexibilidade do mercado de trabalho ou até mesmo a precarização em alguns casos.
                    </p>
                </div>
                <div class="chart-card">
                    <h3>Seu(s) trabalho(s) são:</h3>
                    <div class="chart-canvas-container">
                        <canvas id="jobTypeChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="jobType-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="jobTypeChart" data-chart-title="Tipo de Trabalho">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        A proporção entre trabalhos formais, informais e mistos reflete a segurança empregatícia e a estrutura da economia. A informalidade, por exemplo, pode indicar menor proteção social.
                    </p>
                </div>
                <div class="chart-card">
                    <h3>Qual a sua principal fonte de renda?</h3>
                    <div class="chart-canvas-container">
                        <canvas id="incomeSourceChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="incomeSource-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="incomeSourceChart" data-chart-title="Fonte de Renda">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        A predominância de salários formais indica estabilidade, enquanto uma alta dependência de benefícios ou trabalhos informais pode apontar vulnerabilidades econômicas.
                    </p>
                </div>
            </div>
        </section>

        <section id="financas-credito">
            <h2><i class="fas fa-wallet"></i> Situação Financeira e Crédito (Dados de Pesquisa)</h2>
            <p class="analysis-text">
                A saúde financeira e o acesso ao crédito são indicadores importantes do bem-estar econômico individual e familiar, revelando padrões de consumo, endividamento e capacidade de investimento.
            </p>
            <div class="chart-grid">
                <div class="chart-card">
                    <h3>Você utiliza cartão de crédito?</h3>
                    <div class="chart-canvas-container">
                        <canvas id="creditCardChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="creditCard-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="creditCardChart" data-chart-title="Uso de Cartão de Crédito">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        O uso de cartão de crédito reflete tanto o acesso a serviços financeiros quanto o comportamento de consumo. Um uso consciente pode ser um facilitador, mas excessos podem levar ao endividamento.
                    </p>
                </div>
                <div class="chart-card">
                    <h3>Atualmente, você está com dívidas?</h3>
                    <div class="chart-canvas-container">
                        <canvas id="debtChart"></canvas>
                    </div>
                     <ul class="survey-answers-list" id="debt-list">
                        </ul>
                    <button class="copy-graph-btn" data-chart-id="debtChart" data-chart-title="Situação de Dívidas">Copiar gráfico</button>
                    <p class="analysis-text" style="margin-top: 15px;">
                        A proporção de pessoas endividadas é um alerta sobre a estabilidade financeira. Dívidas podem impactar o poder de compra e a qualidade de vida, sendo um fator de vulnerabilidade econômica.
                    </p>
                </div>
            </div>
        </section>

        <section id="ibge-insights">
            <h2><i class="fas fa-globe-americas"></i> Insights do IBGE (Dados Reais)</h2>
            <p class="analysis-text">
                Dados oficiais do Instituto Brasileiro de Geografia e Estatística (IBGE) fornecem um panorama robusto da realidade socioeconômica do Brasil. Estas informações são cruciais para análises mais aprofundadas e formulação de políticas.
            </p>
            <div class="ibge-data-card" id="ibgeUnemploymentData">
                <p class="loading-message"><i class="fas fa-spinner fa-spin"></i> Carregando dados de desocupação (PNADC)...</p>
            </div>
            <p class="analysis-text">
                A taxa de desocupação por nível de instrução do IBGE frequentemente revela uma correlação: **quanto maior o nível de escolaridade, menor a taxa de desocupação.** Isso sublinha a importância da educação como fator de empregabilidade e ascensão social no Brasil. As disparidades observadas refletem desafios estruturais no acesso e permanência na educação, bem como na absorção de mão de obra qualificada pelo mercado.
            </p>

            <div class="ibge-data-card" id="ibgeIncomeData">
                <p class="loading-message"><i class="fas fa-spinner fa-spin"></i> Carregando rendimento domiciliar per capita (PNADC)...</p>
            </div>
            <p class="analysis-text">
                O rendimento domiciliar per capita é um indicador chave da distribuição de renda e da desigualdade regional. As diferenças entre as Unidades da Federação destacam as **disparidades socioeconômicas existentes no país**, influenciadas por fatores históricos, econômicos e de desenvolvimento local. A compreensão dessas diferenças é vital para o planejamento de investimentos e distribuição de recursos.
            </p>

            <div class="ibge-data-card" id="ibgePopulationProjection">
                <p class="loading-message"><i class="fas fa-spinner fa-spin"></i> Carregando projeção populacional (IBGE)...</p>
            </div>
            <p class="analysis-text">
                A projeção populacional do IBGE é fundamental para o planejamento de longo prazo. Ela informa sobre o crescimento ou envelhecimento da população, impactando diretamente nas necessidades de infraestrutura (saúde, educação, moradia) e na dinâmica do mercado de trabalho. Entender a evolução demográfica permite antecipar demandas e otimizar recursos.
            </p>
        </section>
    </main>

    <footer>
        <p>&copy; 2025 Análise Socioeconômica. Dados do <a href="https://www.ibge.gov.br/" target="_blank">IBGE</a>.</p>
    </footer>

    <script>
        // Hamburger Menu Toggle
        function toggleSidebar() {
            const sidebar = document.getElementById("mySidebar");
            if (sidebar.classList.contains("closed")) {
                sidebar.classList.remove("closed");
                sidebar.style.width = "250px"; // Open sidebar
            } else {
                sidebar.classList.add("closed");
                sidebar.style.width = "0"; // Close sidebar
            }
        }

        // Smooth Scrolling for Navigation
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                document.querySelector(this.getAttribute('href')).scrollIntoView({
                    behavior: 'smooth'
                });
                if (window.innerWidth <= 768) {
                    toggleSidebar(); // Close sidebar after clicking a link on mobile
                }
            });
        });

        // Chart.js Global Configuration
        Chart.defaults.color = 'var(--text-light)'; // Default text color for charts
        Chart.defaults.font.family = 'Inter, "Segoe UI", Roboto, Helvetica, Arial, sans-serif';

        // Custom Color Palette for Charts (avoiding green/red, focusing on blues, grays, and some warm neutrals)
        const chartColorsPalette = [
            '#58a6ff', // Primary Blue (GitHub Accent)
            '#8b949e', // Gray
            '#2188ff', // Brighter Blue
            '#c9d1d9', // Light Gray
            '#a371f7', // Purple-ish
            '#ff8100', // Orange-ish
            '#4092ff', // Medium Blue
            '#6a6a6a', // Darker Gray
            '#0366d6', // Deeper Blue
            '#ddb75d', // Gold-ish
            '#1f6feb'  // Another Blue
        ];

        // Survey Data (based on your analysis of the images)
        // Data updated to reflect better estimations where images were unclear
        const surveyData = {
            gender: {
                type: 'pie',
                labels: ['Feminino', 'Masculino', 'Prefiro não responder'],
                data: [60, 40, 0],
                backgroundColor: [chartColorsPalette[0], chartColorsPalette[1], chartColorsPalette[3]]
            },
            ethnicity: {
                type: 'pie',
                labels: ['Branca', 'Parda', 'Preta', 'Amarela', 'Indígena', 'Prefiro não responder'],
                data: [55, 30, 15, 0, 0, 0], // Values based on visible image data
                backgroundColor: [chartColorsPalette[0], chartColorsPalette[1], chartColorsPalette[2], chartColorsPalette[3], chartColorsPalette[4], chartColorsPalette[5]]
            },
            age: {
                type: 'bar', // Bar chart for better comparison across categories
                labels: ['< 18', '18-24', '25-34', '35-44', '45-59', '60+'],
                data: [15, 15, 15, 25, 30, 0], // Values based on visible image data
                backgroundColor: chartColorsPalette[0] // Single color for bar chart
            },
            household: {
                type: 'pie',
                labels: ['0', '1-3', '4-6', '7 ou mais'],
                data: [5, 80, 15, 0], // Values based on visible image data
                backgroundColor: [chartColorsPalette[0], chartColorsPalette[1], chartColorsPalette[2], chartColorsPalette[3]]
            },
            working: {
                type: 'pie',
                labels: ['Sim', 'Não'],
                data: [80, 20],
                backgroundColor: [chartColorsPalette[0], chartColorsPalette[1]]
            },
            jobsCount: {
                type: 'pie',
                labels: ['1', '2', '3 ou mais'],
                data: [75, 18.8, 6.3],
                backgroundColor: [chartColorsPalette[0], chartColorsPalette[1], chartColorsPalette[2]]
            },
            jobType: {
                type: 'pie',
                labels: ['Todo formal', 'Todos informais', 'Misto'],
                data: [56.3, 31.3, 12.5],
                backgroundColor: [chartColorsPalette[0], chartColorsPalette[1], chartColorsPalette[2]]
            },
            incomeSource: {
                type: 'pie',
                labels: ['Salário formal', 'Trabalho informal/autônomo', 'Aposentadoria/Pensão', 'Benefício social', 'Outros'],
                data: [60, 20, 15, 5, 0], // 'Mãe' not clearly specified in percentage, assumed 'Outros'
                backgroundColor: [chartColorsPalette[0], chartColorsPalette[1], chartColorsPalette[2], chartColorsPalette[3], chartColorsPalette[4]]
            },
            creditCard: {
                type: 'pie',
                labels: ['Sim, frequentemente', 'Sim, às vezes', 'Não'],
                data: [40, 30, 30],
                backgroundColor: [chartColorsPalette[0], chartColorsPalette[1], chartColorsPalette[2]]
            },
            debt: {
                type: 'pie',
                labels: ['Sim', 'Não'],
                data: [50, 50], // Placeholder data as exact values were not visible
                backgroundColor: [chartColorsPalette[1], chartColorsPalette[0]]
            }
        };

        const charts = {}; // Store chart instances

        // Function to create a Pie Chart
        function createPieChart(chartId, labels, data, backgroundColor, title = '') {
            const ctx = document.getElementById(chartId).getContext('2d');
            if (charts[chartId]) { // Destroy existing chart if it exists
                charts[chartId].destroy();
            }
            charts[chartId] = new Chart(ctx, {
                type: 'pie',
                data: {
                    labels: labels,
                    datasets: [{
                        data: data,
                        backgroundColor: backgroundColor,
                        borderColor: 'var(--bg-darkest)',
                        borderWidth: 2,
                        hoverOffset: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'right',
                            labels: {
                                color: 'var(--text-light)',
                                font: { size: 14 }
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.label || '';
                                    if (label) { label += ': '; }
                                    if (context.parsed !== null) { label += context.parsed + '%'; }
                                    return label;
                                }
                            },
                            bodyColor: 'var(--text-white)',
                            titleColor: 'var(--accent-blue)',
                            backgroundColor: 'var(--bg-dark)',
                            borderColor: 'var(--border-color)',
                            borderWidth: 1
                        }
                    }
                }
            });
        }

        // Function to create a Bar Chart
        function createBarChart(chartId, labels, data, backgroundColor, title = '') {
            const ctx = document.getElementById(chartId).getContext('2d');
            if (charts[chartId]) { // Destroy existing chart if it exists
                charts[chartId].destroy();
            }
            charts[chartId] = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        data: data,
                        backgroundColor: backgroundColor,
                        borderColor: 'var(--accent-blue-hover)',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) { label += ': '; }
                                    if (context.parsed.y !== null) { label += context.parsed.y + '%'; }
                                    return label;
                                }
                            },
                            bodyColor: 'var(--text-white)',
                            titleColor: 'var(--accent-blue)',
                            backgroundColor: 'var(--bg-dark)',
                            borderColor: 'var(--border-color)',
                            borderWidth: 1
                        }
                    },
                    scales: {
                        x: {
                            ticks: { color: 'var(--text-light)' },
                            grid: { color: 'var(--border-color)' }
                        },
                        y: {
                            ticks: { color: 'var(--text-light)' },
                            grid: { color: 'var(--border-color)' }
                        }
                    }
                }
            });
        }

        // Function to populate survey answer lists with percentages
        function populateSurveyLists() {
            for (const key in surveyData) {
                const listElement = document.getElementById(`${key}-list`);
                if (listElement) {
                    listElement.innerHTML = ''; // Clear previous content
                    surveyData[key].labels.forEach((label, index) => {
                        const percentage = surveyData[key].data[index];
                        const listItem = document.createElement('li');
                        listItem.innerHTML = `<span>${label}:</span> <span class="percentage">${percentage}%</span>`;
                        listElement.appendChild(listItem);
                    });
                }
            }
        }

        // Initialize all survey charts
        function initializeSurveyChartsAndLists() {
            for (const key in surveyData) {
                const chartConfig = surveyData[key];
                if (chartConfig.type === 'pie') {
                    createPieChart(key + 'Chart', chartConfig.labels, chartConfig.data, chartConfig.backgroundColor, key);
                } else if (chartConfig.type === 'bar') {
                    createBarChart(key + 'Chart', chartConfig.labels, chartConfig.data, chartConfig.backgroundColor, key);
                }
            }
            populateSurveyLists(); // Populate the lists with percentages
        }


        // Implement "Copy Graph" functionality
        document.querySelectorAll('.copy-graph-btn').forEach(button => {
            button.addEventListener('click', (event) => {
                const chartId = event.target.dataset.chartId;
                const chartTitle = event.target.dataset.chartTitle;
                const chartInstance = charts[chartId];

                if (chartInstance) {
                    const chartData = {
                        title: chartTitle,
                        type: chartInstance.config.type,
                        labels: chartInstance.data.labels,
                        data: chartInstance.data.datasets[0].data
                    };
                    const dataToCopy = JSON.stringify(chartData, null, 2);

                    navigator.clipboard.writeText(dataToCopy)
                        .then(() => {
                            alert(`Dados do gráfico "${chartTitle}" copiados para a área de transferência!`);
                        })
                        .catch(err => {
                            console.error('Falha ao copiar dados do gráfico:', err);
                            alert('Erro ao copiar dados do gráfico. Por favor, tente novamente.');
                        });
                }
            });
        });

        // IBGE API Integration - Real Data Analysis
        // Function to handle API fetches and error display
        async function fetchIbgeData(elementId, url, title, parseFunction) {
            const element = document.getElementById(elementId);
            element.innerHTML = `<p class="loading-message"><i class="fas fa-spinner fa-spin"></i> Carregando ${title.toLowerCase()}...</p>`;
            try {
                const response = await fetch(url);
                if (!response.ok) {
                    throw new Error(`HTTP error! Status: ${response.status}`);
                }
                const data = await response.json();
                
                // Check if data structure is as expected before parsing
                if (data && data.length > 0 && data[0].resultados && data[0].resultados.length > 0) {
                    element.innerHTML = parseFunction(data, title);
                    element.classList.remove('loading-message');
                } else {
                    element.innerHTML = `<p class="error-message"><i class="fas fa-exclamation-triangle"></i> Não foram encontrados dados para ${title.toLowerCase()} neste período. (Resposta da API OK, mas sem dados)</p>`;
                    console.warn(`IBGE API: ${title} - Dados retornados vazios ou em formato inesperado. URL: ${url}`, data);
                }
            } catch (error) {
                console.error(`Erro ao buscar dados do IBGE para ${title}:`, error);
                element.innerHTML = `<p class="error-message"><i class="fas fa-exclamation-circle"></i> Erro ao carregar dados de ${title.toLowerCase()} do IBGE. Verifique a conexão ou tente novamente mais tarde. Detalhes: ${error.message}</p>`;
            }
        }

        // --- Parsing Functions for IBGE Data ---

        // 1. Unemployment Rate by Education Level (PNADC)
        function parseUnemploymentData(data, title) {
            let htmlContent = `<h3><i class="fas fa-percent"></i> ${title}</h3><ul>`;
            const periodName = data[0].resultados[0].periodos[0].nome;
            const seriesData = data[0].resultados[0].series[0].serie;

            const educationLevelsMap = {
                "1": "Sem instrução e F. incompleto",
                "2": "F. completo e M. incompleto",
                "3": "M. completo e S. incompleto",
                "4": "Superior completo",
                "5": "Mestrado",
                "6": "Doutorado"
            };

            for (const classificationId in seriesData) {
                const level = educationLevelsMap[classificationId.split('.')[1]];
                const rate = parseFloat(seriesData[classificationId]).toFixed(2);
                htmlContent += `<li><strong>${level}:</strong> ${rate}%</li>`;
            }
            htmlContent += `</ul><p><em>Período: ${periodName} (Fonte: IBGE - PNADC)</em></p>`;
            return htmlContent;
        }

        // 2. Monthly Nominal Per Capita Household Income (PNADC)
        function parseIncomeData(data, title) {
            let htmlContent = `<h3><i class="fas fa-money-bill-wave"></i> ${title}</h3><table><thead><tr><th>Unidade da Federação</th><th>Rendimento (R$)</th></tr></thead><tbody>`;
            const periodName = data[0].resultados[0].periodos[0].nome;
            const incomeData = [];

            for (const item of data[0].resultados) {
                const ufName = item.localidade.nome;
                // Safely extract the income value using Object.values to handle dynamic period keys
                const incomeValueRaw = Object.values(item.series[0].serie)[0]; // Gets the first (and usually only) value in the series object
                const incomeValue = parseFloat(incomeValueRaw);

                if (!isNaN(incomeValue)) {
                    incomeData.push({ uf: ufName, income: incomeValue });
                }
            }

            incomeData.sort((a, b) => b.income - a.income);

            incomeData.forEach(item => {
                htmlContent += `<tr>
                    <td>${item.uf}</td>
                    <td>R$ ${item.income.toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}</td>
                </tr>`;
            });
            htmlContent += `</tbody></table><p><em>Período: ${periodName} (Fonte: IBGE - PNADC)</em></p>`;
            return htmlContent;
        }

        // 3. Population Projection for Brazil
        function parsePopulationProjectionData(data, title) {
            const period = data[0].resultados[0].periodos[0].nome;
            const populationValue = parseInt(data[0].resultados[0].series[0].serie[period]).toLocaleString('pt-BR');
            return `<h3><i class="fas fa-users"></i> ${title}</h3>
                    <p>Em <strong>${period}</strong>, a população total estimada do Brasil é de aproximadamente <strong>${populationValue}</strong> habitantes.</p>
                    <p><em>Fonte: IBGE - Projeção da População</em></p>`;
        }

        // Execute all data fetches and chart initializations on page load
        document.addEventListener('DOMContentLoaded', () => {
            initializeSurveyChartsAndLists(); // Initialize survey charts and populate lists
            
            fetchIbgeData(
                'ibgeUnemploymentData',
                'https://servicodados.ibge.gov.br/api/v3/agregados/6389/periodos/ultimos/variaveis/4099?localidades=N1[BR]&classificacao=12739[1,2,3,4,5,6]',
                'Taxa de Desocupação por Nível de Instrução',
                parseUnemploymentData
            );

            fetchIbgeData(
                'ibgeIncomeData',
                'https://servicodados.ibge.gov.br/api/v3/agregados/2027/periodos/ultimos/variaveis/8032?localidades=N3[all]',
                'Rendimento Domiciliar Per Capita',
                parseIncomeData
            );

            fetchIbgeData(
                'ibgePopulationProjection',
                'https://servicodados.ibge.gov.br/api/v3/agregados/606/periodos/ultimos/variaveis/606?localidades=N1[BR]',
                'População Estimada do Brasil',
                parsePopulationProjectionData
            );
        });
    </script>
</body>
</html>
