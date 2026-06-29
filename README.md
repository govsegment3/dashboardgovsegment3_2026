<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard KPI Contact Center - Gov & Energy Segment</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
  :root {
    --primary: #1e3a5f;
    --secondary: #2d5a87;
    --accent: #00b4d8;
    --success: #2ecc71;
    --warning: #f39c12;
    --danger: #e74c3c;
    --info: #3498db;
    --bg: #f0f2f5;
    --card-bg: #ffffff;
    --text: #1a1a2e;
    --text-muted: #6b7280;
    --border: #e5e7eb;
    --shadow: 0 4px 20px rgba(0,0,0,0.08);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    background: var(--bg);
    color: var(--text);
    line-height: 1.5;
  }
  .header {
    background: linear-gradient(135deg, var(--primary), var(--secondary));
    color: #fff;
    padding: 20px 24px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 12px;
  }
  .header h1 { font-size: 1.5rem; font-weight: 700; }
  .header .subtitle { font-size: 0.85rem; opacity: 0.85; }
  .header .badge-period {
    background: rgba(255,255,255,0.15);
    padding: 6px 14px;
    border-radius: 20px;
    font-size: 0.8rem;
  }
  .container { max-width: 1400px; margin: 0 auto; padding: 24px; }
  .kpi-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 16px;
    margin-bottom: 24px;
  }
  .kpi-card {
    background: var(--card-bg);
    border-radius: 12px;
    padding: 20px;
    box-shadow: var(--shadow);
    border-left: 4px solid var(--accent);
    transition: transform 0.2s;
  }
  .kpi-card:hover { transform: translateY(-3px); }
  .kpi-card.green { border-left-color: var(--success); }
  .kpi-card.orange { border-left-color: var(--warning); }
  .kpi-card.red { border-left-color: var(--danger); }
  .kpi-card.blue { border-left-color: var(--info); }
  .kpi-label { font-size: 0.75rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 6px; }
  .kpi-value { font-size: 1.8rem; font-weight: 700; color: var(--text); }
  .kpi-delta {
    font-size: 0.8rem;
    margin-top: 6px;
    display: flex;
    align-items: center;
    gap: 4px;
  }
  .kpi-delta.up { color: var(--success); }
  .kpi-delta.down { color: var(--danger); }
  .kpi-delta.neutral { color: var(--text-muted); }
  .section-title {
    font-size: 1.1rem;
    font-weight: 700;
    margin: 24px 0 14px 0;
    color: var(--primary);
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .chart-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
    margin-bottom: 24px;
  }
  .chart-card {
    background: var(--card-bg);
    border-radius: 12px;
    padding: 20px;
    box-shadow: var(--shadow);
  }
  .chart-card h3 { font-size: 0.9rem; color: var(--text-muted); margin-bottom: 12px; text-transform: uppercase; letter-spacing: 0.5px; }
  .chart-wrapper { position: relative; height: 260px; }
  .table-card {
    background: var(--card-bg);
    border-radius: 12px;
    padding: 20px;
    box-shadow: var(--shadow);
    overflow-x: auto;
  }
  .table-toolbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 12px;
    margin-bottom: 14px;
  }
  .table-toolbar h3 { font-size: 0.95rem; color: var(--primary); }
  .btn {
    border: none;
    padding: 8px 16px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 0.85rem;
    font-weight: 600;
    display: inline-flex;
    align-items: center;
    gap: 6px;
    transition: background 0.2s, transform 0.1s;
  }
  .btn:active { transform: scale(0.97); }
  .btn-export { background: var(--success); color: #fff; }
  .btn-export:hover { background: #27ae60; }
  .btn-filter { background: var(--info); color: #fff; }
  .btn-filter:hover { background: #2980b9; }
  table { width: 100%; border-collapse: collapse; font-size: 0.85rem; min-width: 800px; }
  thead th {
    background: var(--primary);
    color: #fff;
    padding: 10px 12px;
    text-align: left;
    font-weight: 600;
    position: sticky;
    top: 0;
  }
  tbody tr { border-bottom: 1px solid var(--border); transition: background 0.15s; }
  tbody tr:hover { background: #f8fafc; }
  tbody td { padding: 10px 12px; }
  .badge {
    display: inline-block;
    padding: 4px 10px;
    border-radius: 12px;
    font-size: 0.75rem;
    font-weight: 700;
    text-transform: uppercase;
  }
  .badge-success { background: #d4edda; color: #155724; }
  .badge-warning { background: #fff3cd; color: #856404; }
  .badge-danger { background: #f8d7da; color: #721c24; }
  .progress-bg {
    background: var(--border);
    border-radius: 6px;
    height: 10px;
    width: 100px;
    overflow: hidden;
  }
  .progress-fill {
    height: 100%;
    border-radius: 6px;
    transition: width 0.6s ease;
  }
  .progress-fill.green { background: var(--success); }
  .progress-fill.orange { background: var(--warning); }
  .progress-fill.red { background: var(--danger); }
  .progress-fill.blue { background: var(--info); }
  .filters {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 14px;
  }
  .filters select, .filters input {
    padding: 6px 10px;
    border-radius: 6px;
    border: 1px solid var(--border);
    font-size: 0.85rem;
  }
  .footer {
    text-align: center;
    padding: 20px;
    color: var(--text-muted);
    font-size: 0.8rem;
  }
  @media (max-width: 768px) {
    .header h1 { font-size: 1.2rem; }
    .kpi-grid { grid-template-columns: repeat
