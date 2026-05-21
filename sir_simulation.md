# Epidemiologisches SIR-Modell

> Diese interaktive Simulation veranschaulicht, wie sich eine Infektionskrankheit in einer Bevölkerung ausbreitet und wieder zurückgeht. Das **SIR-Modell** teilt die Bevölkerung in drei Gruppen auf.

## Grundkonzept

Das SIR-Modell wurde 1927 von Kermack und McKendrick entwickelt. Es beschreibt den Verlauf einer Infektionskrankheit mit drei Zuständen:

| Kürzel | Name | Bedeutung |
|--------|------|-----------|
| **S** | Susceptible (Anfällig) | Personen, die noch nicht infiziert wurden und angesteckt werden können |
| **I** | Infectious (Infektiös) | Personen, die aktuell erkrankt sind und andere anstecken können |
| **R** | Recovered (Erholt) | Personen, die genesen sind und nun immun sind |

Die Übergänge zwischen den Zuständen werden durch zwei Schlüsselparameter gesteuert:

- **Ansteckungsrate (β):** Wie leicht überträgt sich die Krankheit? Höhere Werte = schnellere Ausbreitung.
- **Genesungsrate (γ):** Wie schnell genesen Infizierte? Höhere Werte = kürzere Krankheitsdauer.
- **Basisreproduktionszahl R₀ = β / γ:** Gibt an, wie viele Personen eine infektiöse Person im Durchschnitt ansteckt. R₀ > 1 → Epidemie, R₀ < 1 → Aussterben.

## Interaktive Simulation

Stellt die Schieberegler ein und beobachtet, wie sich die Kurven verändern!

<script style="display:block; width:100%; font-family:inherit;">
"HTML"
</script>

``` html
<!DOCTYPE html>
<html>
<head>
<style>
  body { font-family: system-ui, sans-serif; margin: 0; padding: 0; background: #fff; color: #222; }
  .controls { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 24px; }
  .ctrl-group { background: #f5f5f5; border-radius: 10px; padding: 14px 16px; }
  .ctrl-group label { font-size: 13px; color: #555; display: block; margin-bottom: 6px; }
  .ctrl-group .val { font-size: 20px; font-weight: 600; color: #222; }
  input[type=range] { width: 100%; margin: 6px 0 0; accent-color: #333; }
  .r0box { grid-column: span 2; background: #f0f4ff; border: 1px solid #c8d5f5; border-radius: 10px; padding: 12px 16px; display: flex; align-items: center; justify-content: space-between; }
  .r0box .label { font-size: 13px; color: #555; }
  .r0box .r0val { font-size: 22px; font-weight: 700; }
  .r0box .status { font-size: 13px; padding: 4px 12px; border-radius: 20px; font-weight: 600; }
  canvas { width: 100%; border-radius: 8px; background: #fafafa; border: 1px solid #e5e5e5; }
  .legend { display: flex; gap: 24px; margin: 10px 0 0; font-size: 13px; }
  .legend span { display: flex; align-items: center; gap: 6px; }
  .dot { width: 12px; height: 12px; border-radius: 3px; display: inline-block; }
  .peak-info { margin-top: 10px; font-size: 13px; color: #555; background: #f9f9f9; border-radius: 8px; padding: 10px 14px; }
  .peak-info strong { color: #222; }
</style>
</head>
<body>

<div class="controls">
  <div class="ctrl-group">
    <label>Ansteckungsrate β</label>
    <div class="val"><span id="bval">0.30</span></div>
    <input type="range" id="beta" min="0.05" max="0.80" step="0.01" value="0.30">
  </div>
  <div class="ctrl-group">
    <label>Genesungsrate γ</label>
    <div class="val"><span id="gval">0.10</span></div>
    <input type="range" id="gamma" min="0.01" max="0.50" step="0.01" value="0.10">
  </div>
  <div class="ctrl-group">
    <label>Bevölkerungsgröße N</label>
    <div class="val"><span id="nval">10 000</span></div>
    <input type="range" id="npop" min="1000" max="100000" step="1000" value="10000">
  </div>
  <div class="ctrl-group">
    <label>Anfangs-Infizierte I₀</label>
    <div class="val"><span id="i0val">10</span></div>
    <input type="range" id="i0" min="1" max="500" step="1" value="10">
  </div>
  <div class="r0box">
    <div>
      <div class="label">Basisreproduktionszahl R₀ = β / γ</div>
      <div class="r0val" id="r0val">—</div>
    </div>
    <div class="status" id="r0status">—</div>
  </div>
</div>

<canvas id="chart" height="300"></canvas>

<div class="legend">
  <span><span class="dot" style="background:#2a7ed3"></span> S – Anfällig</span>
  <span><span class="dot" style="background:#e05a2b"></span> I – Infektiös</span>
  <span><span class="dot" style="background:#2e9e5b"></span> R – Erholt/Immun</span>
</div>
<div class="peak-info" id="peakinfo"></div>

<script>
const canvas = document.getElementById('chart');
const ctx = canvas.getContext('2d');

function simulate(beta, gamma, N, I0) {
  let S = N - I0, I = I0, R = 0;
  const dt = 0.5;
  const maxDays = 365;
  const days = [], sArr = [], iArr = [], rArr = [];
  for (let t = 0; t <= maxDays; t += dt) {
    days.push(t);
    sArr.push(S / N * 100);
    iArr.push(I / N * 100);
    rArr.push(R / N * 100);
    const dS = -beta * S * I / N * dt;
    const dI = (beta * S * I / N - gamma * I) * dt;
    const dR = gamma * I * dt;
    S += dS; I += dI; R += dR;
    if (I < 0.001 && t > 5) break;
  }
  return { days, sArr, iArr, rArr };
}

function getPeak(iArr, days) {
  let maxI = 0, peakDay = 0;
  for (let k = 0; k < iArr.length; k++) {
    if (iArr[k] > maxI) { maxI = iArr[k]; peakDay = days[k]; }
  }
  return { maxI, peakDay };
}

function draw() {
  const beta = parseFloat(document.getElementById('beta').value);
  const gamma = parseFloat(document.getElementById('gamma').value);
  const N = parseInt(document.getElementById('npop').value);
  const I0 = parseInt(document.getElementById('i0').value);

  document.getElementById('bval').textContent = beta.toFixed(2);
  document.getElementById('gval').textContent = gamma.toFixed(2);
  document.getElementById('nval').textContent = N.toLocaleString('de-DE');
  document.getElementById('i0val').textContent = I0;

  const r0 = beta / gamma;
  document.getElementById('r0val').textContent = r0.toFixed(2);
  const r0el = document.getElementById('r0status');
  if (r0 > 1) {
    r0el.textContent = 'Epidemie möglich';
    r0el.style.background = '#fee2e2'; r0el.style.color = '#991b1b';
  } else {
    r0el.textContent = 'Kein Ausbruch';
    r0el.style.background = '#dcfce7'; r0el.style.color = '#166534';
  }

  const { days, sArr, iArr, rArr } = simulate(beta, gamma, N, I0);
  const { maxI, peakDay } = getPeak(iArr, days);
  const finalR = rArr[rArr.length - 1];

  document.getElementById('peakinfo').innerHTML =
    `<strong>Infektionsgipfel:</strong> ${maxI.toFixed(1)} % der Bevölkerung erkrankt gleichzeitig (Tag ${Math.round(peakDay)}) &nbsp;|&nbsp; ` +
    `<strong>Gesamtinfizierte am Ende:</strong> ca. ${finalR.toFixed(1)} % (${Math.round(finalR/100*N).toLocaleString('de-DE')} Personen)`;

  const W = canvas.width, H = canvas.height;
  ctx.clearRect(0, 0, W, H);

  const pad = { top: 20, right: 20, bottom: 40, left: 50 };
  const plotW = W - pad.left - pad.right;
  const plotH = H - pad.top - pad.bottom;

  ctx.fillStyle = '#fafafa';
  ctx.fillRect(0, 0, W, H);

  const maxDay = days[days.length - 1];
  const xOf = d => pad.left + (d / maxDay) * plotW;
  const yOf = v => pad.top + plotH - (v / 100) * plotH;

  ctx.strokeStyle = '#e5e5e5'; ctx.lineWidth = 1;
  for (let g = 0; g <= 100; g += 25) {
    const y = yOf(g);
    ctx.beginPath(); ctx.moveTo(pad.left, y); ctx.lineTo(pad.left + plotW, y); ctx.stroke();
    ctx.fillStyle = '#999'; ctx.font = '11px system-ui';
    ctx.textAlign = 'right'; ctx.fillText(g + '%', pad.left - 6, y + 4);
  }
  const tickDays = [0, 30, 60, 90, 120, 150, 180, 210, 240, 270, 300, 330, 365].filter(d => d <= maxDay);
  tickDays.forEach(d => {
    const x = xOf(d);
    ctx.beginPath(); ctx.moveTo(x, pad.top + plotH); ctx.lineTo(x, pad.top + plotH + 5); ctx.stroke();
    ctx.fillStyle = '#999'; ctx.textAlign = 'center';
    ctx.fillText('Tag ' + d, x, pad.top + plotH + 18);
  });

  const series = [
    { arr: sArr, color: '#2a7ed3' },
    { arr: rArr, color: '#2e9e5b' },
    { arr: iArr, color: '#e05a2b' }
  ];
  series.forEach(({ arr, color }) => {
    ctx.beginPath();
    ctx.strokeStyle = color; ctx.lineWidth = 2.5;
    arr.forEach((v, k) => {
      const x = xOf(days[k]), y = yOf(v);
      k === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
    });
    ctx.stroke();
  });

  ctx.strokeStyle = '#bbb'; ctx.lineWidth = 1;
  ctx.strokeRect(pad.left, pad.top, plotW, plotH);
}

canvas.width = canvas.offsetWidth || 680;
canvas.height = 300;

['beta','gamma','npop','i0'].forEach(id =>
  document.getElementById(id).addEventListener('input', draw)
);
draw();

window.addEventListener('resize', () => {
  canvas.width = canvas.offsetWidth;
  draw();
});
</script>
</body>
</html>
```

## Aufgaben

**Aufgabe 1 – Basiswissen:** Setzt β = 0,30 und γ = 0,10.

- Wie groß ist R₀?
- Wie viel Prozent der Bevölkerung sind am Peak gleichzeitig krank?
- Was passiert, wenn ihr γ auf 0,30 erhöht (schnellere Genesung)?

**Aufgabe 2 – Herdenimmunität:** Die Herdenimmunitätsschwelle liegt bei $ 1 - \frac{1}{R_0} $.

- Berechnet die Schwelle für R₀ = 3 (vergleichbar mit Masern: R₀ ≈ 12–18, Influenza: R₀ ≈ 2–3).
- Welcher Anteil der Bevölkerung müsste geimpft sein, damit die Epidemie nicht ausbricht?

**Aufgabe 3 – Soziale Distanzierung:** Reduziert β stark (z. B. von 0,50 auf 0,15) – das simuliert Maßnahmen wie Maskenpflicht oder Lockdown.

- Beschreibt, wie sich die I-Kurve verändert. Was versteht man unter dem Begriff **„die Kurve flachen"**?
- Welchen Einfluss hat eine frühe vs. späte Intervention (verschiedene I₀)?

**Aufgabe 4 – Vergleich realer Krankheiten:**

| Krankheit | β (typisch) | γ (typisch) | R₀ |
|-----------|-------------|-------------|-----|
| Influenza | 0,25 | 0,17 | ~1,5 |
| COVID-19 (original) | 0,35 | 0,10 | ~3,5 |
| Masern | 0,90 | 0,07 | ~13 |

Simuliert jede Krankheit und vergleicht den Kurvenverlauf. Was fällt euch auf?

## Grenzen des Modells

Das einfache SIR-Modell hat einige Vereinfachungen:

- **Homogene Durchmischung:** Alle Personen begegnen sich gleich häufig – Netzwerkstrukturen (Familie, Schule, Arbeit) werden ignoriert.
- **Keine Geburten/Todesfälle:** Die Bevölkerungsgröße ist konstant.
- **Keine Inkubationszeit:** Infizierte sind sofort ansteckend (realistischer: SEIR-Modell mit *Exposed*-Phase).
- **Lebenslange Immunität:** Jeder Genesene ist dauerhaft immun (bei manchen Krankheiten nicht der Fall).
