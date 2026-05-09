# Financial-Analyst-
Customer Churn Analytics Of European Bank 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>European Bank – Customer Churn Analytics</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root {
    --bg: #0f1117;
    --surface: #1a1d27;
    --card: #21263a;
    --border: #2e3350;
    --accent: #4f6ef7;
    --accent2: #f75f4f;
    --accent3: #4fd6b0;
    --accent4: #f7c34f;
    --text: #e8eaf6;
    --muted: #8891b4;
    --red: #ef5350;
    --green: #4caf50;
    --amber: #ffa726;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: 'Segoe UI', system-ui, sans-serif; min-height: 100vh; }
 
  /* HEADER */
  .header { background: linear-gradient(135deg, #1a1d27 0%, #12162b 100%); border-bottom: 1px solid var(--border); padding: 20px 32px; display: flex; justify-content: space-between; align-items: center; }
  .header h1 { font-size: 22px; font-weight: 700; letter-spacing: -0.3px; }
  .header h1 span { color: var(--accent); }
  .badge { background: #1e2d5a; color: #7b9cf7; padding: 4px 12px; border-radius: 20px; font-size: 12px; font-weight: 600; border: 1px solid #2e4080; }
 
  /* TABS */
  .tabs { display: flex; gap: 4px; padding: 16px 32px 0; background: var(--surface); border-bottom: 1px solid var(--border); }
  .tab { padding: 10px 20px; border-radius: 8px 8px 0 0; cursor: pointer; font-size: 13px; font-weight: 600; color: var(--muted); border: 1px solid transparent; border-bottom: none; transition: all .2s; }
  .tab:hover { color: var(--text); background: var(--card); }
  .tab.active { color: var(--accent); background: var(--bg); border-color: var(--border); }
 
  /* CONTENT */
  .content { padding: 28px 32px; }
  .section { display: none; }
  .section.active { display: block; }
 
  /* KPI CARDS */
  .kpi-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; margin-bottom: 28px; }
  .kpi { background: var(--card); border: 1px solid var(--border); border-radius: 12px; padding: 20px; position: relative; overflow: hidden; }
  .kpi::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 3px; background: var(--accent-color, var(--accent)); border-radius: 12px 12px 0 0; }
  .kpi .label { font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: var(--muted); margin-bottom: 8px; }
  .kpi .value { font-size: 32px; font-weight: 800; letter-spacing: -1px; }
  .kpi .sub { font-size: 12px; color: var(--muted); margin-top: 4px; }
  .kpi .icon { position: absolute; right: 16px; top: 16px; font-size: 28px; opacity: 0.15; }
 
  /* CHARTS GRID */
  .charts-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px; }
  .charts-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px; margin-bottom: 20px; }
  .chart-card { background: var(--card); border: 1px solid var(--border); border-radius: 12px; padding: 20px; }
  .chart-card.full { grid-column: 1 / -1; }
  .chart-card h3 { font-size: 13px; font-weight: 700; text-transform: uppercase; letter-spacing: .8px; color: var(--muted); margin-bottom: 16px; }
  .chart-card canvas { max-height: 260px; }
 
  /* TABLE */
  .data-table { width: 100%; border-collapse: collapse; font-size: 13px; }
  .data-table th { background: var(--surface); color: var(--muted); font-weight: 700; font-size: 11px; text-transform: uppercase; letter-spacing: .8px; padding: 10px 14px; text-align: left; border-bottom: 1px solid var(--border); }
  .data-table td { padding: 11px 14px; border-bottom: 1px solid #1e2235; }
  .data-table tr:hover td { background: #1e2235; }
  .pill { display: inline-block; padding: 3px 10px; border-radius: 20px; font-size: 11px; font-weight: 700; }
  .pill-red { background: #3d1515; color: #ef9090; }
  .pill-amber { background: #3d2d0a; color: #ffc87a; }
  .pill-green { background: #0d2e15; color: #6fcf7a; }
 
  /* PROGRESS BAR */
  .bar-wrap { background: #1e2235; border-radius: 4px; height: 8px; overflow: hidden; flex: 1; }
  .bar-fill { height: 100%; border-radius: 4px; transition: width .5s; }
  .bar-row { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
  .bar-label { width: 90px; font-size: 12px; color: var(--muted); flex-shrink: 0; }
  .bar-val { width: 46px; text-align: right; font-size: 12px; font-weight: 700; flex-shrink: 0; }
 
  /* INSIGHT BOX */
  .insights { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-top: 20px; }
  .insight { background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 16px; border-left: 4px solid var(--accent-c, var(--accent)); }
  .insight .i-title { font-size: 12px; font-weight: 700; color: var(--muted); text-transform: uppercase; letter-spacing: .8px; margin-bottom: 6px; }
  .insight p { font-size: 13px; line-height: 1.6; color: var(--text); }
 
  /* COMPARISON TABLE */
  .compare-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 0; border: 1px solid var(--border); border-radius: 10px; overflow: hidden; }
  .compare-col { }
  .compare-header { padding: 14px 16px; font-weight: 700; font-size: 13px; text-align: center; }
  .compare-cell { padding: 12px 16px; border-top: 1px solid var(--border); font-size: 13px; text-align: center; }
  .compare-col:nth-child(1) .compare-header { background: var(--surface); color: var(--muted); }
  .compare-col:nth-child(2) .compare-header { background: #2d1515; color: var(--red); }
  .compare-col:nth-child(3) .compare-header { background: #0d2810; color: var(--green); }
  .compare-col:nth-child(2) .compare-cell { background: #1e1515; }
  .compare-col:nth-child(3) .compare-cell { background: #111e14; }
  .compare-col:nth-child(1) .compare-cell { background: var(--card); color: var(--muted); font-size: 12px; font-weight: 600; text-transform: uppercase; letter-spacing: .5px; }
 
  @media (max-width: 900px) {
    .kpi-row { grid-template-columns: 1fr 1fr; }
    .charts-2, .charts-3 { grid-template-columns: 1fr; }
    .insights { grid-template-columns: 1fr; }
  }
</style>
</head>
<body>
 
<div class="header">
  <div>
    <div style="font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:6px;">European Central Bank · Customer Analytics</div>
    <h1>Customer <span>Churn</span> Pattern Analytics</h1>
  </div>
  <div style="display:flex;gap:10px;align-items:center;">
    <span class="badge">📊 10,000 Customers</span>
    <span class="badge">🌍 France · Germany · Spain</span>
    <span class="badge">📅 2025</span>
  </div>
</div>
 
<div class="tabs">
  <div class="tab active" onclick="showTab('overview')">📈 Overview</div>
  <div class="tab" onclick="showTab('geography')">🌍 Geography</div>
  <div class="tab" onclick="showTab('demographics')">👥 Demographics</div>
  <div class="tab" onclick="showTab('financial')">💰 Financial Profile</div>
  <div class="tab" onclick="showTab('highvalue')">⭐ High-Value Customers</div>
  <div class="tab" onclick="showTab('insights')">💡 Insights & KPIs</div>
</div>
 
<!-- OVERVIEW -->
<div id="overview" class="content section active">
  <div class="kpi-row">
    <div class="kpi" style="--accent-color:#ef5350">
      <div class="icon">📉</div>
      <div class="label">Overall Churn Rate</div>
      <div class="value" style="color:#ef5350">20.37%</div>
      <div class="sub">2,037 customers exited</div>
    </div>
    <div class="kpi" style="--accent-color:#4caf50">
      <div class="icon">✅</div>
      <div class="label">Retention Rate</div>
      <div class="value" style="color:#4caf50">79.63%</div>
      <div class="sub">7,963 customers retained</div>
    </div>
    <div class="kpi" style="--accent-color:#4f6ef7">
      <div class="icon">🏦</div>
      <div class="label">Total Customers</div>
      <div class="value" style="color:#4f6ef7">10,000</div>
      <div class="sub">Across 3 countries</div>
    </div>
    <div class="kpi" style="--accent-color:#ffa726">
      <div class="icon">⚠️</div>
      <div class="label">Highest Risk Segment</div>
      <div class="value" style="color:#ffa726">46-60</div>
      <div class="sub">51.12% churn rate</div>
    </div>
  </div>
 
  <div class="charts-2">
    <div class="chart-card">
      <h3>Churn vs Retained Distribution</h3>
      <canvas id="donutChart"></canvas>
    </div>
    <div class="chart-card">
      <h3>Churn Rate by Number of Products</h3>
      <canvas id="productsChart"></canvas>
    </div>
  </div>
 
  <div class="charts-2">
    <div class="chart-card">
      <h3>Active vs Inactive Member Churn</h3>
      <canvas id="activeChart"></canvas>
    </div>
    <div class="chart-card">
      <h3>Churn by Credit Card Ownership</h3>
      <canvas id="overviewBarChart"></canvas>
    </div>
  </div>
</div>
 
<!-- GEOGRAPHY -->
<div id="geography" class="content section">
  <div class="kpi-row">
    <div class="kpi" style="--accent-color:#4f6ef7">
      <div class="label">France Churn Rate</div>
      <div class="value" style="color:#4f6ef7">16.15%</div>
      <div class="sub">810 of 5,014 customers</div>
    </div>
    <div class="kpi" style="--accent-color:#ef5350">
      <div class="label">Germany Churn Rate</div>
      <div class="value" style="color:#ef5350">32.44%</div>
      <div class="sub">814 of 2,509 customers · Highest Risk</div>
    </div>
    <div class="kpi" style="--accent-color:#4caf50">
      <div class="label">Spain Churn Rate</div>
      <div class="value" style="color:#4caf50">16.67%</div>
      <div class="sub">413 of 2,477 customers</div>
    </div>
    <div class="kpi" style="--accent-color:#ffa726">
      <div class="label">Geographic Risk Index</div>
      <div class="value" style="color:#ffa726">2.01x</div>
      <div class="sub">Germany vs France/Spain ratio</div>
    </div>
  </div>
 
  <div class="charts-2">
    <div class="chart-card">
      <h3>Churn Rate by Country</h3>
      <canvas id="geoChurnRate"></canvas>
    </div>
    <div class="chart-card">
      <h3>Total Churned Customers by Country</h3>
      <canvas id="geoVolume"></canvas>
    </div>
  </div>
 
  <div class="charts-2">
    <div class="chart-card">
      <h3>Geography × Gender Churn Matrix</h3>
      <canvas id="geoGenderChart"></canvas>
    </div>
    <div class="chart-card">
      <h3>Country Customer Distribution</h3>
      <canvas id="geoDistChart"></canvas>
    </div>
  </div>
</div>
 
<!-- DEMOGRAPHICS -->
<div id="demographics" class="content section">
  <div class="kpi-row">
    <div class="kpi" style="--accent-color:#f75f4f">
      <div class="label">Female Churn Rate</div>
      <div class="value" style="color:#f75f4f">25.07%</div>
      <div class="sub">1,139 of 4,543 female customers</div>
    </div>
    <div class="kpi" style="--accent-color:#4f6ef7">
      <div class="label">Male Churn Rate</div>
      <div class="value" style="color:#4f6ef7">16.46%</div>
      <div class="sub">898 of 5,457 male customers</div>
    </div>
    <div class="kpi" style="--accent-color:#ffa726">
      <div class="label">Peak Age Churn</div>
      <div class="value" style="color:#ffa726">46–60</div>
      <div class="sub">51.12% churn — critical segment</div>
    </div>
    <div class="kpi" style="--accent-color:#4caf50">
      <div class="label">Lowest Age Churn</div>
      <div class="value" style="color:#4caf50">&lt;30</div>
      <div class="sub">7.56% — most loyal young segment</div>
    </div>
  </div>
 
  <div class="charts-2">
    <div class="chart-card">
      <h3>Churn Rate by Age Group</h3>
      <canvas id="ageChurnChart"></canvas>
    </div>
    <div class="chart-card">
      <h3>Gender Churn Comparison</h3>
      <canvas id="genderChart"></canvas>
    </div>
  </div>
 
  <div class="charts-2">
    <div class="chart-card">
      <h3>Churn Rate by Tenure Group</h3>
      <canvas id="tenureChart"></canvas>
    </div>
    <div class="chart-card">
      <h3>Age Group Customer Volume vs Churn</h3>
      <canvas id="ageVolumeChart"></canvas>
    </div>
  </div>
</div>
 
<!-- FINANCIAL PROFILE -->
<div id="financial" class="content section">
  <div class="kpi-row">
    <div class="kpi" style="--accent-color:#ef5350">
      <div class="label">High-Balance Churn</div>
      <div class="value" style="color:#ef5350">25.23%</div>
      <div class="sub">Balance ≥ €100,000</div>
    </div>
    <div class="kpi" style="--accent-color:#4caf50">
      <div class="label">Zero-Balance Churn</div>
      <div class="value" style="color:#4caf50">13.82%</div>
      <div class="sub">Lowest churn — no assets at risk</div>
    </div>
    <div class="kpi" style="--accent-color:#ffa726">
      <div class="label">Avg Age — Churned</div>
      <div class="value" style="color:#ffa726">44.8</div>
      <div class="sub">vs 37.4 retained customers</div>
    </div>
    <div class="kpi" style="--accent-color:#4f6ef7">
      <div class="label">Avg Balance — Churned</div>
      <div class="value" style="color:#4f6ef7">€91K</div>
      <div class="sub">vs €72.7K for retained</div>
    </div>
  </div>
 
  <div class="charts-2">
    <div class="chart-card">
      <h3>Churn Rate by Balance Segment</h3>
      <canvas id="balanceChart"></canvas>
    </div>
    <div class="chart-card">
      <h3>Churn Rate by Credit Score Band</h3>
      <canvas id="creditChart"></canvas>
    </div>
  </div>
 
  <div class="chart-card full" style="margin-bottom:20px;">
    <h3>Churned vs Retained — Customer Profile Comparison</h3>
    <div class="compare-grid" style="margin-top:12px;">
      <div class="compare-col">
        <div class="compare-header">Metric</div>
        <div class="compare-cell">Avg Age</div>
        <div class="compare-cell">Avg Balance (€)</div>
        <div class="compare-cell">Avg Credit Score</div>
        <div class="compare-cell">Avg Salary (€)</div>
        <div class="compare-cell">Avg Tenure (yrs)</div>
        <div class="compare-cell">Avg Products</div>
      </div>
      <div class="compare-col">
        <div class="compare-header">🔴 Churned</div>
        <div class="compare-cell">44.8</div>
        <div class="compare-cell">91,108</div>
        <div class="compare-cell">645.4</div>
        <div class="compare-cell">101,466</div>
        <div class="compare-cell">4.9</div>
        <div class="compare-cell">1.48</div>
      </div>
      <div class="compare-col">
        <div class="compare-header">🟢 Retained</div>
        <div class="compare-cell">37.4</div>
        <div class="compare-cell">72,745</div>
        <div class="compare-cell">651.9</div>
        <div class="compare-cell">99,738</div>
        <div class="compare-cell">5.0</div>
        <div class="compare-cell">1.54</div>
      </div>
    </div>
  </div>
</div>
 
<!-- HIGH VALUE -->
<div id="highvalue" class="content section">
  <div class="kpi-row">
    <div class="kpi" style="--accent-color:#ef5350">
      <div class="label">High-Value Churn Rate</div>
      <div class="value" style="color:#ef5350">25.23%</div>
      <div class="sub">Balance ≥ €100,000</div>
    </div>
    <div class="kpi" style="--accent-color:#ffa726">
      <div class="label">High-Value Customers</div>
      <div class="value" style="color:#ffa726">4,799</div>
      <div class="sub">48% of total base</div>
    </div>
    <div class="kpi" style="--accent-color:#ef5350">
      <div class="label">High-Value Churned</div>
      <div class="value" style="color:#ef5350">1,211</div>
      <div class="sub">Lost premium accounts</div>
    </div>
    <div class="kpi" style="--accent-color:#4f6ef7">
      <div class="label">Avg Balance (Churned HV)</div>
      <div class="value" style="color:#4f6ef7">€131.7K</div>
      <div class="sub">vs €132.6K retained</div>
    </div>
  </div>
 
  <div class="charts-2">
    <div class="chart-card">
      <h3>High-Value vs Standard Customer Churn</h3>
      <canvas id="hvCompChart"></canvas>
    </div>
    <div class="chart-card">
      <h3>High-Value Churn Breakdown</h3>
      <canvas id="hvDonutChart"></canvas>
    </div>
  </div>
 
  <div class="chart-card full">
    <h3>Revenue Risk Exposure by Segment</h3>
    <div style="margin-top:8px;">
      <div class="bar-row">
        <div class="bar-label">High Balance</div>
        <div class="bar-wrap"><div class="bar-fill" style="width:100%;background:#ef5350"></div></div>
        <div class="bar-val" style="color:#ef5350">25.23%</div>
      </div>
      <div class="bar-row">
        <div class="bar-label">Low Balance</div>
        <div class="bar-wrap"><div class="bar-fill" style="width:82%;background:#ffa726"></div></div>
        <div class="bar-val" style="color:#ffa726">20.58%</div>
      </div>
      <div class="bar-row">
        <div class="bar-label">Zero Balance</div>
        <div class="bar-wrap"><div class="bar-fill" style="width:55%;background:#4caf50"></div></div>
        <div class="bar-val" style="color:#4caf50">13.82%</div>
      </div>
      <div class="bar-row">
        <div class="bar-label">Inactive</div>
        <div class="bar-wrap"><div class="bar-fill" style="width:100%;background:#ef5350"></div></div>
        <div class="bar-val" style="color:#ef5350">26.85%</div>
      </div>
      <div class="bar-row">
        <div class="bar-label">Germany</div>
        <div class="bar-wrap"><div class="bar-fill" style="width:100%;background:#ef5350"></div></div>
        <div class="bar-val" style="color:#ef5350">32.44%</div>
      </div>
      <div class="bar-row">
        <div class="bar-label">Age 46-60</div>
        <div class="bar-wrap"><div class="bar-fill" style="width:100%;background:#9c27b0"></div></div>
        <div class="bar-val" style="color:#ce93d8">51.12%</div>
      </div>
    </div>
  </div>
</div>
 
<!-- INSIGHTS -->
<div id="insights" class="content section">
  <div class="kpi-row">
    <div class="kpi" style="--accent-color:#4f6ef7">
      <div class="label">Overall Churn Rate KPI</div>
      <div class="value" style="color:#4f6ef7">20.37%</div>
      <div class="sub">Industry avg ~15–20%</div>
    </div>
    <div class="kpi" style="--accent-color:#ef5350">
      <div class="label">Geographic Risk Index</div>
      <div class="value" style="color:#ef5350">2.01×</div>
      <div class="sub">Germany vs France/Spain</div>
    </div>
    <div class="kpi" style="--accent-color:#ffa726">
      <div class="label">Engagement Drop Indicator</div>
      <div class="value" style="color:#ffa726">+88%</div>
      <div class="sub">Inactive vs active churn gap</div>
    </div>
    <div class="kpi" style="--accent-color:#4fd6b0">
      <div class="label">High-Value Churn Ratio</div>
      <div class="value" style="color:#4fd6b0">25.23%</div>
      <div class="sub">Premium customer loss rate</div>
    </div>
  </div>
 
  <div class="chart-card full" style="margin-bottom:20px;">
    <h3>KPI Summary Dashboard</h3>
    <canvas id="kpiRadar" style="max-height:320px;"></canvas>
  </div>
 
  <div class="insights">
    <div class="insight" style="--accent-c:#ef5350">
      <div class="i-title">🔴 Critical Finding: Germany</div>
      <p>Germany has a 32.44% churn rate — nearly double France (16.15%) and Spain (16.67%). German female customers exhibit 37.55% churn, the highest of all geographic-gender combinations. Immediate retention campaigns are needed in Germany.</p>
    </div>
    <div class="insight" style="--accent-c:#ffa726">
      <div class="i-title">⚠️ Age 46–60 Segment Crisis</div>
      <p>Customers aged 46–60 churn at 51.12% — more than half this segment leaves. This is 2.5× the overall rate. This likely reflects life-stage transitions (retirement planning, wealth reallocation). Targeted wealth management products could reduce attrition.</p>
    </div>
    <div class="insight" style="--accent-c:#9c27b0">
      <div class="i-title">📦 Product Trap: 3–4 Products</div>
      <p>Customers with 3 products churn at 82.71%, and those with 4 products at 100%. This counter-intuitive pattern suggests product bundling may be creating friction or dissatisfaction rather than loyalty. Review multi-product pricing models.</p>
    </div>
    <div class="insight" style="--accent-c:#4f6ef7">
      <div class="i-title">🎯 Inactivity = Churn Signal</div>
      <p>Inactive members churn at 26.85% vs 14.27% for active members — an 88% relative increase in churn risk. Proactive re-engagement programs (notifications, offers, relationship manager outreach) can significantly reduce this gap.</p>
    </div>
    <div class="insight" style="--accent-c:#ef5350">
      <div class="i-title">💰 High-Value Revenue Risk</div>
      <p>High-balance customers (≥€100K) churn at 25.23%, losing 1,211 premium accounts. Churned high-value customers hold an average of €131,700 — representing significant AUM outflow. Dedicated retention for premium segments is critical.</p>
    </div>
    <div class="insight" style="--accent-c:#4fd6b0">
      <div class="i-title">♀️ Gender Disparity</div>
      <p>Female customers churn at 25.07% vs 16.46% for males — a 52% relative difference. This gap is consistent across all geographies, most severely in Germany where female churn reaches 37.55%. Female-focused engagement and product design can close this gap.</p>
    </div>
  </div>
 
  <div class="chart-card" style="margin-top:20px;">
    <h3>Strategic Recommendations — Priority Matrix</h3>
    <table class="data-table" style="margin-top:4px;">
      <thead>
        <tr><th>Priority</th><th>Segment</th><th>Churn Rate</th><th>Action</th><th>Impact</th></tr>
      </thead>
      <tbody>
        <tr><td><span class="pill pill-red">P1 Critical</span></td><td>Germany · All Customers</td><td style="color:#ef5350;font-weight:700;">32.44%</td><td>Dedicated Germany retention campaign</td><td>814 customers at risk</td></tr>
        <tr><td><span class="pill pill-red">P1 Critical</span></td><td>Age 46–60</td><td style="color:#ef5350;font-weight:700;">51.12%</td><td>Retirement & wealth advisory products</td><td>842 customers at risk</td></tr>
        <tr><td><span class="pill pill-red">P1 Critical</span></td><td>3–4 Products</td><td style="color:#ef5350;font-weight:700;">82–100%</td><td>Review multi-product bundling</td><td>280 customers at risk</td></tr>
        <tr><td><span class="pill pill-amber">P2 High</span></td><td>Inactive Members</td><td style="color:#ffa726;font-weight:700;">26.85%</td><td>Re-engagement & notification programs</td><td>1,302 customers at risk</td></tr>
        <tr><td><span class="pill pill-amber">P2 High</span></td><td>High-Balance Customers</td><td style="color:#ffa726;font-weight:700;">25.23%</td><td>Premium relationship manager assignment</td><td>1,211 high-value at risk</td></tr>
        <tr><td><span class="pill pill-amber">P2 High</span></td><td>Female Customers</td><td style="color:#ffa726;font-weight:700;">25.07%</td><td>Gender-focused product design</td><td>1,139 customers at risk</td></tr>
        <tr><td><span class="pill pill-green">P3 Medium</span></td><td>Single-Product Users</td><td style="color:#4caf50;font-weight:700;">27.71%</td><td>Upsell to 2-product bundles (7.58% churn)</td><td>1,409 customers at risk</td></tr>
        <tr><td><span class="pill pill-green">P3 Medium</span></td><td>New Customers (≤2yr)</td><td style="color:#4caf50;font-weight:700;">21.15%</td><td>Early engagement onboarding program</td><td>528 customers at risk</td></tr>
      </tbody>
    </table>
  </div>
</div>
 
<script>
const C = {
  accent: '#4f6ef7', red: '#ef5350', green: '#4caf50', amber: '#ffa726',
  purple: '#ab47bc', teal: '#4fd6b0', pink: '#f75f4f',
  gridColor: '#2e3350', tickColor: '#8891b4', textColor: '#e8eaf6'
};
 
const defaultOpts = {
  responsive: true, maintainAspectRatio: true,
  plugins: { legend: { labels: { color: C.tickColor, font: { size: 11 } } } },
  scales: {
    x: { grid: { color: C.gridColor }, ticks: { color: C.tickColor } },
    y: { grid: { color: C.gridColor }, ticks: { color: C.tickColor } }
  }
};
 
function mkBar(id, labels, data, colors, label='Churn Rate %') {
  const ctx = document.getElementById(id).getContext('2d');
  new Chart(ctx, {
    type: 'bar',
    data: { labels, datasets: [{ label, data, backgroundColor: colors, borderRadius: 6 }] },
    options: { ...defaultOpts,
      plugins: { legend: { display: false } },
      scales: {
        x: { grid: { color: C.gridColor }, ticks: { color: C.tickColor } },
        y: { grid: { color: C.gridColor }, ticks: { color: C.tickColor, callback: v => v + '%' }, beginAtZero: true }
      }
    }
  });
}
 
// OVERVIEW
new Chart(document.getElementById('donutChart').getContext('2d'), {
  type: 'doughnut',
  data: { labels: ['Retained (79.63%)', 'Churned (20.37%)'],
          datasets: [{ data: [7963, 2037], backgroundColor: [C.green, C.red], borderWidth: 0, hoverOffset: 8 }] },
  options: { responsive: true, maintainAspectRatio: true, cutout: '65%',
             plugins: { legend: { position: 'bottom', labels: { color: C.tickColor, padding: 16 } } } }
});
 
mkBar('productsChart', ['1 Product','2 Products','3 Products','4 Products'],
  [27.71, 7.58, 82.71, 100.0], [C.amber, C.green, C.red, '#b71c1c']);
 
new Chart(document.getElementById('activeChart').getContext('2d'), {
  type: 'bar',
  data: { labels: ['Inactive Members','Active Members'],
          datasets: [{ label: 'Churn Rate %', data: [26.85, 14.27],
                       backgroundColor: [C.red, C.green], borderRadius: 8 }] },
  options: { ...defaultOpts, indexAxis: 'y',
             plugins: { legend: { display: false } },
             scales: { x: { grid: { color: C.gridColor }, ticks: { color: C.tickColor, callback: v => v+'%' } },
                       y: { grid: { color: C.gridColor }, ticks: { color: C.tickColor } } } }
});
 
mkBar('overviewBarChart', ['Has Credit Card','No Credit Card'], [20.85, 19.38],
  [C.accent, C.teal]);
 
// GEOGRAPHY
mkBar('geoChurnRate', ['France','Germany','Spain'], [16.15, 32.44, 16.67],
  [C.accent, C.red, C.green]);
 
new Chart(document.getElementById('geoVolume').getContext('2d'), {
  type: 'bar',
  data: { labels: ['France','Germany','Spain'],
          datasets: [
            { label: 'Retained', data: [4204, 1695, 2064], backgroundColor: C.green, borderRadius: 6 },
            { label: 'Churned', data: [810, 814, 413], backgroundColor: C.red, borderRadius: 6 }
          ] },
  options: { ...defaultOpts,
             plugins: { legend: { labels: { color: C.tickColor } } },
             scales: { x: { stacked: true, grid: { color: C.gridColor }, ticks: { color: C.tickColor } },
                       y: { stacked: true, grid: { color: C.gridColor }, ticks: { color: C.tickColor } } } }
});
 
new Chart(document.getElementById('geoGenderChart').getContext('2d'), {
  type: 'bar',
  data: { labels: ['France','Germany','Spain'],
          datasets: [
            { label: 'Female', data: [20.34, 37.55, 21.21], backgroundColor: C.pink, borderRadius: 5 },
            { label: 'Male', data: [12.71, 27.81, 13.11], backgroundColor: C.accent, borderRadius: 5 }
          ] },
  options: { ...defaultOpts,
             scales: { x: { grid: { color: C.gridColor }, ticks: { color: C.tickColor } },
                       y: { grid: { color: C.gridColor }, ticks: { color: C.tickColor, callback: v=>v+'%' }, beginAtZero:true } } }
});
 
new Chart(document.getElementById('geoDistChart').getContext('2d'), {
  type: 'doughnut',
  data: { labels: ['France (50.1%)','Germany (25.1%)','Spain (24.8%)'],
          datasets: [{ data: [5014,2509,2477], backgroundColor: [C.accent, C.red, C.green], borderWidth:0 }] },
  options: { responsive:true, maintainAspectRatio:true, cutout:'60%',
             plugins: { legend: { position:'bottom', labels:{ color:C.tickColor, padding:12 } } } }
});
 
// DEMOGRAPHICS
mkBar('ageChurnChart', ['<30','30-45','46-60','60+'],
  [7.56, 15.30, 51.12, 24.78], [C.green, C.accent, C.red, C.amber]);
 
new Chart(document.getElementById('genderChart').getContext('2d'), {
  type: 'bar',
  data: { labels: ['Female','Male'],
          datasets: [
            { label: 'Churned', data: [1139,898], backgroundColor: [C.pink, C.accent], borderRadius: 8, yAxisID:'y1' },
            { label: 'Churn %', data: [25.07, 16.46], backgroundColor: 'transparent',
              borderColor: [C.red, C.teal], borderWidth: 2, type:'line', yAxisID:'y2', pointBackgroundColor: [C.red, C.teal], pointRadius: 6 }
          ] },
  options: { ...defaultOpts,
             scales: { x: { grid:{color:C.gridColor}, ticks:{color:C.tickColor} },
                       y1: { position:'left', grid:{color:C.gridColor}, ticks:{color:C.tickColor} },
                       y2: { position:'right', grid:{display:false}, ticks:{color:C.tickColor, callback:v=>v+'%'} } } }
});
 
mkBar('tenureChart', ['New (≤2yr)','Mid-term (3-6yr)','Long-term (7+yr)'],
  [21.15, 20.64, 19.51], [C.red, C.amber, C.green]);
 
new Chart(document.getElementById('ageVolumeChart').getContext('2d'), {
  type: 'bar',
  data: { labels: ['<30','30-45','46-60','60+'],
          datasets: [
            { label: 'Retained', data: [1517, 5292, 805, 349], backgroundColor: C.green+'99', borderRadius: 4 },
            { label: 'Churned', data: [124, 956, 842, 115], backgroundColor: C.red+'99', borderRadius: 4 }
          ] },
  options: { ...defaultOpts,
             scales: { x: { stacked:true, grid:{color:C.gridColor}, ticks:{color:C.tickColor} },
                       y: { stacked:true, grid:{color:C.gridColor}, ticks:{color:C.tickColor} } } }
});
 
// FINANCIAL
mkBar('balanceChart', ['Zero Balance','Low Balance','High Balance'],
  [13.82, 20.58, 25.23], [C.green, C.amber, C.red]);
 
mkBar('creditChart', ['Medium (580-719)','High (720+)','Low (<580)'],
  [19.65, 20.32, 22.02], [C.accent, C.teal, C.red]);
 
// HIGH VALUE
new Chart(document.getElementById('hvCompChart').getContext('2d'), {
  type: 'bar',
  data: { labels: ['Standard Customers','High-Value (≥€100K)'],
          datasets: [
            { label: 'Churn Rate %', data: [16.0, 25.23],
              backgroundColor: [C.accent, C.red], borderRadius: 10 }
          ] },
  options: { ...defaultOpts,
             plugins:{legend:{display:false}},
             scales:{ x:{grid:{color:C.gridColor},ticks:{color:C.tickColor}},
                      y:{grid:{color:C.gridColor},ticks:{color:C.tickColor,callback:v=>v+'%'},beginAtZero:true} } }
});
 
new Chart(document.getElementById('hvDonutChart').getContext('2d'), {
  type: 'doughnut',
  data: { labels: ['Retained High-Value (74.77%)','Churned High-Value (25.23%)'],
          datasets: [{ data:[3588,1211], backgroundColor:[C.green, C.red], borderWidth:0 }] },
  options: { responsive:true, maintainAspectRatio:true, cutout:'65%',
             plugins:{ legend:{position:'bottom', labels:{color:C.tickColor,padding:14}} } }
});
 
// INSIGHTS RADAR
new Chart(document.getElementById('kpiRadar').getContext('2d'), {
  type: 'radar',
  data: { labels: ['Overall Churn','Germany Risk','Age 46-60 Risk','Inactivity Risk','HV Churn','Female Churn','Product 3+ Risk'],
          datasets: [
            { label: 'Churn Risk Score (normalized)', data: [20.37, 32.44, 51.12, 26.85, 25.23, 25.07, 82.71],
              backgroundColor: 'rgba(79,110,247,0.15)', borderColor: C.accent, pointBackgroundColor: C.accent, pointRadius: 5 },
            { label: 'Bank Average (20.37%)', data: [20.37,20.37,20.37,20.37,20.37,20.37,20.37],
              backgroundColor: 'rgba(239,83,80,0.05)', borderColor: C.red+'88', borderDash:[5,5], pointRadius:3, pointBackgroundColor:C.red }
          ] },
  options: { responsive:true, maintainAspectRatio:true,
             plugins:{ legend:{ labels:{ color:C.tickColor } } },
             scales:{ r:{ grid:{color:C.gridColor}, angleLines:{color:C.gridColor},
                          ticks:{color:C.tickColor, backdropColor:'transparent', font:{size:10}},
                          pointLabels:{color:C.textColor, font:{size:11}} } } }
});
 
function showTab(id) {
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  event.target.classList.add('active');
}
</script>
</body>
</html>
