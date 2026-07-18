<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Brecha digital en México — Historia de datos interactiva</title>
<!-- Librería Chart.js cargada desde un servidor alternativo más rápido -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
  :root{
    --navy:#1B2A4A;
    --navy-2:#26365c;
    --teal:#1E7F7A;
    --teal-light:#DCEFEE;
    --gold:#C9A24B;
    --grey:#6b7280;
    --bg:#F7F8FA;
    --card:#FFFFFF;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    font-family:'Segoe UI', Calibri, Arial, sans-serif;
    background:var(--bg);
    color:#222;
  }
  header{
    background:linear-gradient(135deg, var(--navy) 0%, var(--navy-2) 100%);
    color:#fff;
    padding:48px 24px 40px;
    text-align:center;
  }
  header .eyebrow{
    color:var(--gold);
    letter-spacing:.14em;
    text-transform:uppercase;
    font-size:.78rem;
    font-weight:700;
    margin-bottom:10px;
  }
  header h1{
    margin:0 auto 12px;
    max-width:820px;
    font-size:clamp(1.5rem, 3.2vw, 2.3rem);
    line-height:1.25;
    font-weight:700;
  }
  header p{
    max-width:680px;
    margin:0 auto;
    color:#cfd6e6;
    font-size:.98rem;
    line-height:1.55;
  }
  .wrap{
    max-width:960px;
    margin:0 auto;
    padding:32px 20px 60px;
  }
  .stat-row{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:14px;
    margin:-30px 0 34px;
  }
  .stat{
    background:var(--card);
    border-radius:12px;
    padding:18px 16px;
    box-shadow:0 6px 18px rgba(27,42,74,.08);
    text-align:center;
    border-top:4px solid var(--teal);
  }
  .stat b{
    display:block;
    font-size:1.65rem;
    color:var(--navy);
  }
  .stat span{
    font-size:.78rem;
    color:var(--grey);
  }
  .card{
    background:var(--card);
    border-radius:14px;
    padding:26px 26px 18px;
    box-shadow:0 6px 22px rgba(27,42,74,.07);
    margin-bottom:32px;
  }
  .card h2{
    color:var(--navy);
    font-size:1.15rem;
    margin:0 0 4px;
  }
  .card .sub{
    color:var(--grey);
    font-size:.88rem;
    margin-bottom:18px;
  }
  .chart-box{ position:relative; height:360px; }
  .source{
    font-size:.74rem;
    color:var(--grey);
    margin-top:10px;
    font-style:italic;
  }
  .toggle-row{
    display:flex;
    gap:8px;
    margin-bottom:18px;
    flex-wrap:wrap;
  }
  .toggle-row button{
    border:1px solid #d7dbe4;
    background:#fff;
    color:var(--navy);
    padding:7px 14px;
    border-radius:999px;
    font-size:.82rem;
    font-weight:600;
    cursor:pointer;
    transition:.15s;
  }
  .toggle-row button.active{
    background:var(--navy);
    border-color:var(--navy);
    color:#fff;
  }
  .toggle-row button:hover{ border-color:var(--teal); }
  .legend-note{
    display:flex; gap:18px; flex-wrap:wrap; margin-top:14px; font-size:.82rem; color:var(--grey);
  }
  .legend-note span{ display:inline-flex; align-items:center; gap:6px; }
  .dot{ width:10px; height:10px; border-radius:3px; display:inline-block; }
  .instructivo{
    background:var(--teal-light);
    border-left:4px solid var(--teal);
    border-radius:8px;
    padding:18px 20px;
    font-size:.9rem;
    line-height:1.6;
    color:#0f3d3a;
  }
  .instructivo h3{ margin:0 0 8px; color:var(--navy); font-size:1rem; }
  .instructivo ol{ margin:0; padding-left:20px; }
  .instructivo li{ margin-bottom:6px; }
  footer{
    text-align:center;
    color:var(--grey);
    font-size:.78rem;
    padding:20px;
  }
  @media (max-width:640px){
    .stat-row{ grid-template-columns:1fr; margin-top:16px; }
  }
</style>
</head>
<body>

<header>
  <div class="eyebrow">Visualización de datos · Proyecto integrador</div>
  <h1>La brecha digital en México: crece el país, pero no todos parejo</h1>
  <p>Historia de datos interactiva construida con cifras públicas de la Encuesta Nacional sobre Disponibilidad y Uso de Tecnologías de la Información en los Hogares (ENDUTIH), INEGI 2021–2024. Pasa el cursor sobre las barras y los puntos para ver el detalle.</p>
</header>

<div class="wrap">

  <div class="stat-row">
    <div class="stat"><b>83.1%</b><span>Personas usuarias de internet, 2024</span></div>
    <div class="stat"><b>45.2 pts</b><span>Diferencia CDMX vs. Chiapas, 2023</span></div>
    <div class="stat"><b>+7.5 pts</b><span>Crecimiento nacional 2021→2024</span></div>
  </div>

  <!-- GRÁFICO 1 -->
  <div class="card">
    <h2>Gráfico 1 — Hogares con acceso a internet por entidad federativa, 2023</h2>
    <div class="sub">Las tres entidades con mayor y menor cobertura, comparadas contra la media nacional</div>
    <div class="chart-box"><canvas id="chartBarras"></canvas></div>
    <div class="legend-note">
      <span><i class="dot" style="background:#1E7F7A"></i> Mayor cobertura</span>
      <span><i class="dot" style="background:#C9A24B"></i> Menor cobertura</span>
      <span>┄ Media nacional: 71.7%</span>
    </div>
    <div class="source">Fuente: INEGI, Encuesta Nacional sobre Disponibilidad y Uso de TIC en los Hogares (ENDUTIH), 2023.</div>
  </div>

  <!-- GRÁFICO 2 -->
  <div class="card">
    <h2>Gráfico 2 — Evolución de la población usuaria de internet en México</h2>
    <div class="sub">Selecciona el rango de años para explorar la tendencia nacional</div>
    <div class="toggle-row" id="rangeToggle">
      <button data-range="all" class="active">2021–2024</button>
      <button data-range="recent">2023–2024</button>
    </div>
    <div class="chart-box"><canvas id="chartLinea"></canvas></div>
    <div class="source">Fuente: INEGI, ENDUTIH 2021, 2023 y 2024 (comunicados 372/24 y ENDUTIH_24_RR).</div>
  </div>

  <div class="instructivo">
    <h3>Instructivo para el usuario</h3>
    <ol>
      <li>En el gráfico 1, compara cada barra contra la línea punteada (media nacional) para saber si un estado está arriba o abajo del promedio del país.</li>
      <li>Pasa el cursor (o toca, en móvil) sobre cualquier barra o punto para ver el valor exacto.</li>
      <li>En el gráfico 2 puedes cambiar el rango de años con los botones para enfocarte en el periodo más reciente.</li>
      <li>Todos los porcentajes están normalizados respecto al total de población u hogares de cada entidad, por lo que sí son comparables entre estados de distinto tamaño.</li>
    </ol>
  </div>

</div>

<footer>
  Autoría: [Nombre del alumno] · [Universidad] · Elaborado en 2026 a partir de datos abiertos del INEGI (ENDUTIH).
</footer>

<script>
// Configurar la tipografía global para Chart.js
Chart.defaults.font.family = "'Segoe UI', Calibri, Arial, sans-serif";

// ---------- Inicialización del Gráfico 1: Barras ----------
const estados = ["CDMX","Baja California","Quintana Roo","Guerrero","Oaxaca","Chiapas"];
const valores = [89.5, 86.4, 83.6, 53.9, 53.0, 44.3];
const colores = ["#1E7F7A","#1E7F7A","#1E7F7A","#C9A24B","#C9A24B","#C9A24B"];
const MEDIA_NACIONAL = 71.7;

const ctxBar = document.getElementById('chartBarras').getContext('2d');
new Chart(ctxBar, {
  type: 'bar',
  data: {
    labels: estados,
    datasets: [{
      label: 'Hogares con acceso a internet (%)',
      data: valores,
      backgroundColor: colores,
      borderRadius: 6,
      maxBarThickness: 60
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: { display: false },
      tooltip: {
        backgroundColor: '#1B2A4A',
        padding: 10,
        titleFont: { weight: 'bold' },
        callbacks: {
          label: (ctx) => `${ctx.parsed.y}% de los hogares · ${ (ctx.parsed.y - MEDIA_NACIONAL >= 0 ? '+' : '') + (ctx.parsed.y - MEDIA_NACIONAL).toFixed(1) } pts vs. media nacional`
        }
      }
    },
    scales: {
      y: {
        min: 0, max: 100,
        grid: { color: '#EDEFF3' },
        ticks: { callback: v => v + '%' }
      },
      x: { grid: { display: false } }
    }
  },
  plugins: [{
    id: 'nationalAvgLine',
    afterDraw(chart){
      const {ctx, chartArea, scales} = chart;
      const y = scales.y.getPixelForValue(MEDIA_NACIONAL);
      ctx.save();
      ctx.strokeStyle = '#1B2A4A';
      ctx.setLineDash([6,4]);
      ctx.lineWidth = 1.5;
      ctx.beginPath();
      ctx.moveTo(chartArea.left, y);
      ctx.lineTo(chartArea.right, y);
      ctx.stroke();
      ctx.setLineDash([]);
      ctx.fillStyle = '#1B2A4A';
      ctx.font = 'italic 11px Segoe UI';
      ctx.textAlign = 'right';
      ctx.fillText('Media nacional: 71.7%', chartArea.right, y - 6);
      ctx.restore();
    }
  }]
});

// ---------- Inicialización del Gráfico 2: Línea ----------
const dataAll = { labels:['2021','2023','2024'], values:[75.6, 81.2, 83.1] };
const dataRecent = { labels:['2023','2024'], values:[81.2, 83.1] };

const ctxLine = document.getElementById('chartLinea').getContext('2d');
let lineChart = new Chart(ctxLine, {
  type: 'line',
  data: {
    labels: dataAll.labels,
    datasets: [{
      label: 'Población usuaria de internet (%)',
      data: dataAll.values,
      borderColor: '#1E7F7A',
      backgroundColor: 'rgba(30,127,122,0.14)',
      borderWidth: 3,
      pointBackgroundColor: '#1B2A4A',
      pointBorderColor: '#fff',
      pointBorderWidth: 2,
      pointRadius: 6,
      pointHoverRadius: 8,
      fill: true,
      tension: 0.25
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: { display: false },
      tooltip: {
        backgroundColor: '#1B2A4A',
        padding: 10,
        callbacks: {
          label: (ctx) => `${ctx.parsed.y}% de la población usuaria`
        }
      }
    },
    scales: {
      y: { min: 70, max: 90, grid: { color:'#EDEFF3' }, ticks:{ callback: v => v + '%' } },
      x: { grid: { display:false } }
    }
  }
});

// Lógica para alternar el rango de años en el Gráfico 2
document.querySelectorAll('#rangeToggle button').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('#rangeToggle button').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    
    const d = btn.dataset.range === 'all' ? dataAll : dataRecent;
    lineChart.data.labels = d.labels;
    lineChart.data.datasets[0].data = d.values;
    lineChart.update();
  });
});
</script>
</body>
</html>
