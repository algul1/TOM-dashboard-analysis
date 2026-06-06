<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TOMRA Systems ASA — Financial Analysis</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;1,400&family=Source+Sans+3:wght@400;600&family=Source+Code+Pro:wght@400;600&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Source Sans 3', serif;
    background: #faf9f7;
    color: #1a1a18;
    font-size: 15px;
    line-height: 1.65;
  }

  .page {
    max-width: 860px;
    margin: 0 auto;
    padding: 4rem 2rem 5rem;
  }

  /* Title block */
  .title-block {
    border-top: 2px solid #1a1a18;
    border-bottom: 1px solid #c8c6c0;
    padding: 1.5rem 0 1.2rem;
    margin-bottom: 2.5rem;
  }
  .title-course {
    font-family: 'Source Sans 3', sans-serif;
    font-size: 11px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: #888;
    margin-bottom: 8px;
  }
  .title-main {
    font-family: 'Lora', serif;
    font-size: 26px;
    font-weight: 600;
    letter-spacing: -0.01em;
    margin-bottom: 4px;
  }
  .title-sub {
    font-family: 'Lora', serif;
    font-size: 15px;
    font-style: italic;
    color: #555;
    margin-bottom: 14px;
  }
  .title-meta {
    font-size: 12px;
    color: #888;
    display: flex;
    gap: 2rem;
    flex-wrap: wrap;
  }

  /* Section headings */
  h2 {
    font-family: 'Lora', serif;
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    color: #1a1a18;
    border-bottom: 1px solid #d4d2cc;
    padding-bottom: 6px;
    margin: 2.5rem 0 1.2rem;
  }
  h2:first-of-type { margin-top: 0; }

  /* KPI table */
  .kpi-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13.5px;
    margin-bottom: 1rem;
  }
  .kpi-table th {
    font-family: 'Source Sans 3', sans-serif;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    color: #888;
    text-align: left;
    padding: 6px 12px 6px 0;
    border-bottom: 1px solid #d4d2cc;
  }
  .kpi-table th:not(:first-child) { text-align: right; }
  .kpi-table td {
    padding: 7px 12px 7px 0;
    border-bottom: 1px solid #eceae5;
    vertical-align: middle;
  }
  .kpi-table td:not(:first-child) { text-align: right; }
  .kpi-table tr:last-child td { border-bottom: none; }
  .kpi-table .label { color: #444; }
  .kpi-table .val {
    font-family: 'Source Code Pro', monospace;
    font-size: 13px;
    font-weight: 600;
  }
  .kpi-table .note { font-size: 12px; color: #888; font-style: italic; }
  .pos { color: #2a6e3f; }
  .neg { color: #922020; }
  .neu { color: #666; }

  /* Charts */
  .chart-section {
    margin-bottom: 2rem;
  }
  .chart-caption {
    font-size: 12px;
    color: #888;
    font-style: italic;
    margin-top: 8px;
    text-align: center;
  }
  .chart-wrap { position: relative; width: 100%; }
  .chart-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    margin-bottom: 2rem;
  }

  /* Legend */
  .legend {
    display: flex; gap: 16px; margin-bottom: 8px;
    font-size: 12px; color: #666;
  }
  .legend span { display: flex; align-items: center; gap: 5px; }
  .legend-dot { width: 10px; height: 3px; border-radius: 1px; flex-shrink: 0; }

  /* Segment table */
  .seg-table { width: 100%; border-collapse: collapse; font-size: 13.5px; margin-bottom: 1rem; }
  .seg-table th {
    font-size: 11px; font-weight: 600; letter-spacing: 0.06em;
    text-transform: uppercase; color: #888; text-align: left;
    padding: 6px 0; border-bottom: 1px solid #d4d2cc;
  }
  .seg-table th:not(:first-child) { text-align: right; }
  .seg-table td { padding: 8px 0; border-bottom: 1px solid #eceae5; vertical-align: middle; }
  .seg-table td:not(:first-child) { text-align: right; }
  .seg-table tr:last-child td { border-bottom: none; }
  .bar-inline { display: inline-block; height: 4px; vertical-align: middle; margin-left: 8px; border-radius: 2px; }

  /* Valuation */
  .val-table { width: 100%; border-collapse: collapse; font-size: 13.5px; margin-bottom: 1rem; }
  .val-table th {
    font-size: 11px; font-weight: 600; letter-spacing: 0.06em;
    text-transform: uppercase; color: #888; text-align: left;
    padding: 6px 0; border-bottom: 1px solid #d4d2cc;
  }
  .val-table th:not(:first-child) { text-align: right; }
  .val-table td { padding: 8px 0; border-bottom: 1px solid #eceae5; }
  .val-table td:not(:first-child) { text-align: right; }
  .val-table tr:last-child td { border-bottom: 2px solid #1a1a18; font-weight: 600; padding-top: 10px; }
  .val-table .mono { font-family: 'Source Code Pro', monospace; font-size: 13px; }

  /* Verdict box */
  .verdict-box {
    border: 1px solid #c8c6c0;
    padding: 1rem 1.25rem;
    margin-top: 1.5rem;
    background: #f3f1ec;
  }
  .verdict-box p { font-size: 13.5px; color: #333; line-height: 1.7; }
  .verdict-box strong { font-weight: 600; }

  /* Footer */
  .footer {
    margin-top: 3.5rem;
    padding-top: 1rem;
    border-top: 1px solid #c8c6c0;
    font-size: 11px;
    color: #aaa;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 4px;
  }

  @media (max-width: 640px) {
    .chart-grid { grid-template-columns: 1fr; }
  }
</style>
</head>
<body>
<div class="page">

  <div class="title-block">
    <div class="title-course">Financial Analysis · Oslo, 2025</div>
    <div class="title-main">Financial Analysis and Valuation of TOMRA Systems ASA</div>
    <div class="title-sub">Reverse vending, recycling and food sorting &nbsp;·&nbsp; Oslo Stock Exchange: TOM</div>
    <div class="title-meta">
      <span>Completed: May 2025</span>
      <span>Reference date: 1 January 2025</span>
      <span>Market price: NOK 146.5</span>
    </div>
  </div>

  <!-- Key figures -->
  <h2>Key figures — FY 2024</h2>
  <table class="kpi-table">
    <thead>
      <tr>
        <th style="width:42%">Metric</th>
        <th>Value</th>
        <th>YoY</th>
        <th>Note</th>
      </tr>
    </thead>
    <tbody>
      <tr><td class="label">Revenue</td><td class="val">€1,348M</td><td class="pos">+5.0%</td><td class="note">Record high</td></tr>
      <tr><td class="label">NOPAT</td><td class="val">€122M</td><td class="pos">+52.5%</td><td class="note">Recovery from 2023 low</td></tr>
      <tr><td class="label">Operating cash flow</td><td class="val">€235M</td><td class="pos">+71.5%</td><td class="note">vs. net income €99M</td></tr>
      <tr><td class="label">EPS</td><td class="val">€0.27</td><td class="pos">+51.9%</td><td class="note"></td></tr>
      <tr><td class="label">ROIC</td><td class="val">9.4%</td><td class="pos">+3.2pp</td><td class="note">Above WACC of 7.1%</td></tr>
      <tr><td class="label">EVA</td><td class="val">€49.8M</td><td class="pos">+226%</td><td class="note">Weakest 2023: €15.3M</td></tr>
      <tr><td class="label">NIBD / EBITDA</td><td class="val">1.72x</td><td class="neg">↑ from 0.71x</td><td class="note">Rising leverage</td></tr>
      <tr><td class="label">P/E ratio</td><td class="val">38.8x</td><td class="neg">Premium</td><td class="note">Peer avg: 23.9x</td></tr>
    </tbody>
  </table>

  <!-- Profitability charts -->
  <h2>Profitability — 2021 to 2024</h2>
  <div class="chart-grid">
    <div class="chart-section">
      <div class="legend">
        <span><span class="legend-dot" style="background:#2a4a7f;height:10px;border-radius:2px"></span>Revenue (€M)</span>
        <span><span class="legend-dot" style="background:#2a6e3f;height:10px;border-radius:2px"></span>NOPAT (€M)</span>
      </div>
      <div class="chart-wrap" style="height:200px">
        <canvas id="revenueChart" role="img" aria-label="Revenue and NOPAT 2021-2024">Revenue grew from €924M to €1,348M. NOPAT recovered to €122M in 2024 after declining 2021-2023.</canvas>
      </div>
      <div class="chart-caption">Figure 1. Revenue and NOPAT development</div>
    </div>
    <div class="chart-section">
      <div class="legend">
        <span><span class="legend-dot" style="background:#2a4a7f;height:3px"></span>ROIC (%)</span>
        <span><span class="legend-dot" style="background:#922020;height:3px;border: 1px dashed #922020;background:transparent"></span>WACC (%)</span>
      </div>
      <div class="chart-wrap" style="height:200px">
        <canvas id="roicChart" role="img" aria-label="ROIC vs WACC 2021-2024">ROIC declined from 13.5% to 6.2% before recovering. WACC rose from 5.8% to 7.5%.</canvas>
      </div>
      <div class="chart-caption">Figure 2. ROIC vs WACC — value creation spread</div>
    </div>
  </div>

  <div class="chart-grid">
    <div class="chart-section">
      <div class="legend">
        <span><span class="legend-dot" style="background:#2a4a7f;height:10px;border-radius:2px"></span>EBIT margin</span>
        <span><span class="legend-dot" style="background:#2a6e3f;height:10px;border-radius:2px"></span>Net margin</span>
      </div>
      <div class="chart-wrap" style="height:190px">
        <canvas id="marginChart" role="img" aria-label="Margin development 2021-2024">Margins compressed 2021-2023 due to cost growth, partially recovered in 2024.</canvas>
      </div>
      <div class="chart-caption">Figure 3. Margin development (%)</div>
    </div>
    <div class="chart-section">
      <div class="legend">
        <span><span class="legend-dot" style="background:#2a6e3f;height:10px;border-radius:2px"></span>Positive</span>
        <span><span class="legend-dot" style="background:#922020;height:10px;border-radius:2px"></span>Near-zero</span>
      </div>
      <div class="chart-wrap" style="height:190px">
        <canvas id="evaChart" role="img" aria-label="EVA 2021-2024">EVA remained positive throughout. Lowest in 2023 at €15.3M.</canvas>
      </div>
      <div class="chart-caption">Figure 4. Economic Value Added (€M)</div>
    </div>
  </div>

  <!-- Segments -->
  <h2>Segment breakdown — FY 2024</h2>
  <table class="seg-table">
    <thead>
      <tr>
        <th style="width:30%">Segment</th>
        <th>Revenue share</th>
        <th>YoY growth</th>
        <th>Comment</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Collection</td>
        <td>57% <span class="bar-inline" style="width:57px;background:#2a4a7f"></span></td>
        <td class="pos">+11%</td>
        <td style="font-size:12.5px;color:#555">Dominant market share; 113,700 RVMs installed; high service revenue recurring stream</td>
      </tr>
      <tr>
        <td>Food</td>
        <td>23% <span class="bar-inline" style="width:23px;background:#888"></span></td>
        <td class="neg">-3%</td>
        <td style="font-size:12.5px;color:#555">Weakest segment; competitive pressure from Bühler and peers</td>
      </tr>
      <tr>
        <td>Recycling</td>
        <td>20% <span class="bar-inline" style="width:20px;background:#aaa"></span></td>
        <td class="neg">-2%</td>
        <td style="font-size:12.5px;color:#555">Sensitive to industrial capex cycles; high interest rates delayed investments in 2024</td>
      </tr>
    </tbody>
  </table>

  <!-- Valuation -->
  <h2>Valuation summary</h2>
  <table class="val-table">
    <thead>
      <tr>
        <th style="width:42%">Method</th>
        <th>Key assumption</th>
        <th>Est. price</th>
        <th>vs market</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>DCF (primary)</td>
        <td style="font-size:12.5px;color:#666">WACC 7.1%, g 2.5%</td>
        <td class="mono">NOK 114.2</td>
        <td class="neg">-22.1%</td>
      </tr>
      <tr>
        <td>P/E multiple</td>
        <td style="font-size:12.5px;color:#666">Peer avg 23.9x</td>
        <td class="mono">NOK 94.0</td>
        <td class="neg">-35.8%</td>
      </tr>
      <tr>
        <td>P/B multiple</td>
        <td style="font-size:12.5px;color:#666">Peer avg 3.24x</td>
        <td class="mono">NOK 81.7</td>
        <td class="neg">-44.2%</td>
      </tr>
      <tr>
        <td>DDM</td>
        <td style="font-size:12.5px;color:#666">K<sub>e</sub> 7.75%</td>
        <td class="mono">NOK 40.0</td>
        <td class="neg">-72.7%</td>
      </tr>
      <tr>
        <td>Market price (1 Jan 2025)</td>
        <td></td>
        <td class="mono">NOK 146.5</td>
        <td>—</td>
      </tr>
    </tbody>
  </table>

  <div class="verdict-box">
    <p><strong>Trade recommendation: Sell.</strong> All four valuation methods indicate the stock is trading at a significant premium to intrinsic value. While TOMRA's long-term strategic position is strong — driven by regulatory tailwinds, dominant market share in collection, and a growing service revenue base — the current market price does not appear justified by near-term fundamentals. ROIC exceeds WACC in every year analysed, and EVA remained positive throughout, confirming the company creates value. However, at 38.8x P/E versus a peer average of 23.9x, the implied growth expectations appear overstated given the weaker performance in Food and Recycling. We recommend reducing exposure at current levels while maintaining a positive long-term view.</p>
  </div>

  <div class="footer">
    <span>TOMRA Systems ASA (OSE: TOM) &nbsp;·&nbsp; Financial Analysis of TOMRA Systems ASA · Oslo, 2025</span>
    <span>For educational purposes only. Not investment advice.</span>
  </div>

</div>

<script>
const years = ['2021','2022','2023','2024'];
const gridColor = 'rgba(0,0,0,0.05)';
const tickColor = '#999';
const font = { family: "'Source Sans 3', sans-serif", size: 11 };

const base = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: { legend: { display: false }, tooltip: { mode: 'index', intersect: false } },
  scales: {
    x: { grid: { color: gridColor }, ticks: { color: tickColor, font }, border: { color: '#ccc' } },
    y: { grid: { color: gridColor }, ticks: { color: tickColor, font }, border: { color: '#ccc' } }
  }
};

new Chart(document.getElementById('revenueChart'), {
  type: 'bar',
  data: {
    labels: years,
    datasets: [
      { label: 'Revenue (€M)', data: [924,1103,1278,1348], backgroundColor: '#2a4a7f', borderRadius: 3 },
      { label: 'NOPAT (€M)', data: [128,100,80,122], backgroundColor: '#2a6e3f', borderRadius: 3 }
    ]
  },
  options: { ...base, scales: { ...base.scales, y: { ...base.scales.y, ticks: { ...base.scales.y.ticks, callback: v => '€'+v } } } }
});

new Chart(document.getElementById('roicChart'), {
  type: 'line',
  data: {
    labels: years,
    datasets: [
      { label: 'ROIC (%)', data: [13.5,8.8,6.2,9.4], borderColor: '#2a4a7f', backgroundColor: 'rgba(42,74,127,0.07)', tension: 0.3, fill: true, pointRadius: 4, pointBackgroundColor: '#2a4a7f', borderWidth: 2 },
      { label: 'WACC (%)', data: [5.8,6.6,7.5,7.1], borderColor: '#922020', backgroundColor: 'transparent', tension: 0.3, pointRadius: 4, pointBackgroundColor: '#922020', borderDash: [4,3], borderWidth: 1.5 }
    ]
  },
  options: { ...base, scales: { ...base.scales, y: { ...base.scales.y, min: 0, max: 18, ticks: { ...base.scales.y.ticks, callback: v => v+'%' } } } }
});

new Chart(document.getElementById('marginChart'), {
  type: 'line',
  data: {
    labels: years,
    datasets: [
      { label: 'EBIT margin', data: [16.2,10.1,7.9,11.7], borderColor: '#2a4a7f', backgroundColor: 'rgba(42,74,127,0.06)', tension: 0.3, fill: true, pointRadius: 4, pointBackgroundColor: '#2a4a7f', borderWidth: 2 },
      { label: 'Net margin', data: [11.4,7.2,5.5,7.4], borderColor: '#2a6e3f', backgroundColor: 'rgba(42,110,63,0.06)', tension: 0.3, fill: true, pointRadius: 4, pointBackgroundColor: '#2a6e3f', borderDash: [4,3], borderWidth: 1.5 }
    ]
  },
  options: { ...base, scales: { ...base.scales, y: { ...base.scales.y, min: 0, ticks: { ...base.scales.y.ticks, callback: v => v+'%' } } } }
});

new Chart(document.getElementById('evaChart'), {
  type: 'bar',
  data: {
    labels: years,
    datasets: [{
      label: 'EVA (€M)',
      data: [90,38,15,50],
      backgroundColor: ['#2a6e3f','#2a6e3f','#922020','#2a6e3f'],
      borderRadius: 3
    }]
  },
  options: { ...base, scales: { ...base.scales, y: { ...base.scales.y, ticks: { ...base.scales.y.ticks, callback: v => '€'+v+'M' } } } }
});
</script>
</body>
</html>
