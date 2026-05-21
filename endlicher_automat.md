# Endliche Automaten – Interaktiver Visualisierer

> Mit diesem Werkzeug könnt ihr eigene endliche Automaten (deterministisch, DFA) definieren, als Graphen anzeigen lassen und Eingabewörter Schritt für Schritt simulieren.

## Was ist ein endlicher Automat?

Ein **deterministischer endlicher Automat (DEA)** ist ein mathematisches Modell zur Beschreibung von Berechnungen. Er besteht aus:

| Symbol | Bedeutung |
|--------|-----------|
| **Q** | Endliche Menge von Zuständen |
| **Σ** | Eingabealphabet (erlaubte Zeichen) |
| **δ** | Übergangsfunktion: δ(Zustand, Zeichen) → nächster Zustand |
| **q₀** | Anfangszustand |
| **F** | Menge der Endzustände (akzeptierende Zustände) |

Ein Automat **akzeptiert** ein Wort, wenn er nach dem Lesen des gesamten Wortes in einem Endzustand ist.

## Interaktiver Automat

<script style="display:block; width:100%; font-family:inherit;">
"HTML"
</script>

``` html
<!DOCTYPE html>
<html>
<head>
<style>
  * { box-sizing: border-box; }
  body { font-family: system-ui, sans-serif; margin: 0; padding: 0; background: #fff; color: #222; font-size: 14px; }
  .layout { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 16px; }
  .panel { background: #f7f7f7; border: 1px solid #e0e0e0; border-radius: 10px; padding: 14px 16px; }
  .panel h3 { margin: 0 0 10px; font-size: 14px; color: #444; font-weight: 600; }
  textarea { width: 100%; border: 1px solid #d0d0d0; border-radius: 6px; padding: 8px; font-family: monospace; font-size: 12px; resize: vertical; background: #fff; }
  button { background: #222; color: #fff; border: none; border-radius: 6px; padding: 8px 18px; cursor: pointer; font-size: 13px; margin-right: 8px; }
  button:hover { background: #444; }
  button.sec { background: #fff; color: #333; border: 1px solid #ccc; }
  button.sec:hover { background: #f0f0f0; }
  input[type=text] { border: 1px solid #d0d0d0; border-radius: 6px; padding: 7px 10px; font-size: 13px; width: 100%; margin-bottom: 8px; }
  canvas { background: #fff; border: 1px solid #e0e0e0; border-radius: 10px; display: block; width: 100%; }
  .trace { display: flex; flex-wrap: wrap; gap: 6px; margin: 10px 0; min-height: 32px; }
  .trace .step { padding: 4px 12px; border-radius: 20px; font-size: 13px; font-weight: 600; background: #f0f0f0; color: #555; transition: background 0.2s; }
  .trace .step.active { background: #2a7ed3; color: #fff; }
  .trace .step.accept { background: #2e9e5b; color: #fff; }
  .trace .step.reject { background: #e05a2b; color: #fff; }
  .result { font-size: 14px; font-weight: 600; padding: 8px 14px; border-radius: 8px; display: inline-block; margin-top: 6px; }
  .result.accept { background: #dcfce7; color: #166534; }
  .result.reject { background: #fee2e2; color: #991b1b; }
  .result.running { background: #fef9c3; color: #854d0e; }
  .err { color: #c0392b; font-size: 12px; margin: 4px 0; }
  .info-row { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; margin-bottom: 8px; }
  .badge { font-size: 11px; background: #e8e8e8; color: #444; padding: 3px 9px; border-radius: 12px; }
  .step-controls { display: flex; gap: 8px; align-items: center; margin: 8px 0; }
  .step-controls button { padding: 6px 14px; font-size: 12px; }
</style>
</head>
<body>

<div class="layout">
  <div class="panel">
    <h3>Zustandsdefinition</h3>
    <label style="font-size:12px;color:#666;">Zustände (kommagetrennt, Endzustände mit * markieren, Anfangszustand mit &gt; markieren)</label>
    <textarea id="statesInput" rows="2">>q0, q1*, q2</textarea>
    <label style="font-size:12px;color:#666;margin-top:8px;display:block;">Alphabet (kommagetrennt)</label>
    <textarea id="alphaInput" rows="1">0, 1</textarea>
  </div>
  <div class="panel">
    <h3>Übergänge</h3>
    <label style="font-size:12px;color:#666;">Format: <code>Zustand, Zeichen → Zielzustand</code> (eine pro Zeile)</label>
    <textarea id="transInput" rows="5">q0, 0 → q0
q0, 1 → q1
q1, 0 → q2
q1, 1 → q0
q2, 0 → q1
q2, 1 → q2</textarea>
  </div>
</div>

<div style="margin-bottom:12px;">
  <button onclick="build()">▶ Automat aufbauen</button>
  <button class="sec" onclick="loadExample('even1')">Beispiel: gerade Anzahl von 1en</button>
  <button class="sec" onclick="loadExample('div3')">Beispiel: Binärzahl ÷ 3</button>
</div>
<div id="buildErr" class="err"></div>

<canvas id="canvas" height="300"></canvas>

<div class="panel" style="margin-top:16px;">
  <h3>Wort simulieren</h3>
  <div class="info-row">
    <input type="text" id="wordInput" placeholder="Eingabewort, z.B. 101" style="max-width:200px;margin:0;">
    <button onclick="startSim()">Simulieren</button>
    <button class="sec" onclick="resetSim()">Zurücksetzen</button>
  </div>
  <div class="step-controls" id="stepCtrl" style="display:none;">
    <button onclick="stepBack()">◀ Zurück</button>
    <button onclick="stepForward()">Vor ▶</button>
    <button onclick="runAll()">Alles ▶▶</button>
  </div>
  <div id="traceLabel" style="font-size:12px;color:#666;margin-bottom:4px;"></div>
  <div class="trace" id="trace"></div>
  <div id="result"></div>
</div>

<script>
let automaton = null;
let simSteps = [], simPos = -1;

const EXAMPLES = {
  even1: {
    states: '>q0*, q1',
    alpha: '0, 1',
    trans: 'q0, 0 → q0\nq0, 1 → q1\nq1, 0 → q1\nq1, 1 → q0'
  },
  div3: {
    states: '>r0*, r1, r2',
    alpha: '0, 1',
    trans: 'r0, 0 → r0\nr0, 1 → r1\nr1, 0 → r2\nr1, 1 → r0\nr2, 0 → r1\nr2, 1 → r2'
  }
};

function loadExample(key) {
  const ex = EXAMPLES[key];
  document.getElementById('statesInput').value = ex.states;
  document.getElementById('alphaInput').value = ex.alpha;
  document.getElementById('transInput').value = ex.trans;
  build();
}

function parseAutomaton() {
  const rawStates = document.getElementById('statesInput').value;
  const rawAlpha = document.getElementById('alphaInput').value;
  const rawTrans = document.getElementById('transInput').value;

  const stateTokens = rawStates.split(',').map(s => s.trim()).filter(Boolean);
  let startState = null;
  const acceptStates = new Set();
  const states = [];
  for (const tok of stateTokens) {
    let name = tok;
    let isStart = false, isAccept = false;
    if (name.startsWith('>')) { isStart = true; name = name.slice(1); }
    if (name.endsWith('*')) { isAccept = true; name = name.slice(0, -1); }
    name = name.trim();
    if (!name) continue;
    states.push(name);
    if (isStart) { if (startState) throw new Error('Mehr als ein Anfangszustand (>) gefunden.'); startState = name; }
    if (isAccept) acceptStates.add(name);
  }
  if (!startState) throw new Error('Kein Anfangszustand angegeben. Einen Zustand mit > markieren.');
  if (states.length === 0) throw new Error('Keine Zustände definiert.');

  const alphabet = rawAlpha.split(',').map(s => s.trim()).filter(Boolean);
  if (alphabet.length === 0) throw new Error('Kein Alphabet angegeben.');

  const delta = {};
  const lines = rawTrans.split('\n').map(l => l.trim()).filter(Boolean);
  for (const line of lines) {
    const m = line.match(/^(.+?)\s*,\s*(.+?)\s*[→>-]+\s*(.+)$/);
    if (!m) throw new Error(`Ungültige Zeile: "${line}"`);
    const [, from, sym, to] = m.map(s => s.trim());
    if (!states.includes(from)) throw new Error(`Unbekannter Zustand: "${from}"`);
    if (!states.includes(to)) throw new Error(`Unbekannter Zielzustand: "${to}"`);
    if (!alphabet.includes(sym)) throw new Error(`Unbekanntes Symbol: "${sym}" – nicht im Alphabet?`);
    if (!delta[from]) delta[from] = {};
    delta[from][sym] = to;
  }

  return { states, alphabet, delta, startState, acceptStates };
}

function build() {
  document.getElementById('buildErr').textContent = '';
  resetSim();
  try {
    automaton = parseAutomaton();
    draw();
  } catch (e) {
    document.getElementById('buildErr').textContent = '⚠ ' + e.message;
    automaton = null;
  }
}

function layout(states, delta) {
  const n = states.length;
  const cx = 340, cy = 150, r = 110;
  const pos = {};
  states.forEach((s, i) => {
    const angle = (2 * Math.PI * i / n) - Math.PI / 2;
    pos[s] = { x: cx + r * Math.cos(angle), y: cy + r * Math.sin(angle) };
  });
  return pos;
}

function draw(highlightState = null) {
  if (!automaton) return;
  const canvas = document.getElementById('canvas');
  canvas.width = canvas.offsetWidth || 680;
  const ctx = canvas.getContext('2d');
  const { states, delta, startState, acceptStates } = automaton;
  const pos = layout(states, delta);
  const W = canvas.width, H = canvas.height;
  ctx.clearRect(0, 0, W, H);
  ctx.fillStyle = '#fff';
  ctx.fillRect(0, 0, W, H);

  const NODE_R = 28;

  function arrowHead(ctx, x, y, angle) {
    ctx.save(); ctx.translate(x, y); ctx.rotate(angle);
    ctx.beginPath(); ctx.moveTo(0, 0); ctx.lineTo(-10, -5); ctx.lineTo(-10, 5); ctx.closePath();
    ctx.fillStyle = '#666'; ctx.fill(); ctx.restore();
  }

  for (const from of states) {
    if (!delta[from]) continue;
    const selfLabels = {};
    for (const sym of Object.keys(delta[from])) {
      const to = delta[from][sym];
      if (from === to) {
        selfLabels[sym] = true;
        continue;
      }
      const p1 = pos[from], p2 = pos[to];
      const dx = p2.x - p1.x, dy = p2.y - p1.y;
      const dist = Math.sqrt(dx * dx + dy * dy);
      const nx = dx / dist, ny = dy / dist;
      const sx = p1.x + nx * NODE_R, sy = p1.y + ny * NODE_R;
      const ex = p2.x - nx * NODE_R, ey = p2.y - ny * NODE_R;
      const reverseExists = delta[to] && delta[to][sym] === from;
      let cx2 = (sx + ex) / 2, cy2 = (sy + ey) / 2;
      if (reverseExists) {
        cx2 += (-ny) * 30; cy2 += nx * 30;
      }
      ctx.beginPath();
      ctx.moveTo(sx, sy);
      ctx.quadraticCurveTo(cx2, cy2, ex, ey);
      ctx.strokeStyle = '#999'; ctx.lineWidth = 1.5;
      ctx.stroke();
      const angle2 = Math.atan2(ey - cy2, ex - cx2);
      arrowHead(ctx, ex, ey, angle2);
      const lx = (sx + 2 * cx2 + ex) / 4, ly = (sy + 2 * cy2 + ey) / 4;
      ctx.fillStyle = '#555'; ctx.font = '12px system-ui'; ctx.textAlign = 'center';
      ctx.fillText(sym, lx, ly - 6);
    }
    if (Object.keys(selfLabels).length > 0) {
      const p = pos[from];
      ctx.beginPath();
      ctx.arc(p.x, p.y - NODE_R - 18, 18, 0.2 * Math.PI, 2.8 * Math.PI);
      ctx.strokeStyle = '#999'; ctx.lineWidth = 1.5; ctx.stroke();
      arrowHead(ctx, p.x + 10, p.y - NODE_R - 3, 0.7);
      ctx.fillStyle = '#555'; ctx.font = '12px system-ui'; ctx.textAlign = 'center';
      ctx.fillText(Object.keys(selfLabels).join(','), p.x, p.y - NODE_R - 38);
    }
  }

  const startP = pos[startState];
  ctx.beginPath();
  ctx.moveTo(startP.x - NODE_R - 30, startP.y);
  ctx.lineTo(startP.x - NODE_R, startP.y);
  ctx.strokeStyle = '#999'; ctx.lineWidth = 1.5; ctx.stroke();
  arrowHead(ctx, startP.x - NODE_R, startP.y, 0);

  for (const s of states) {
    const p = pos[s];
    const isHighlight = s === highlightState;
    ctx.beginPath();
    ctx.arc(p.x, p.y, NODE_R, 0, 2 * Math.PI);
    ctx.fillStyle = isHighlight ? '#2a7ed3' : (acceptStates.has(s) ? '#e8f5e9' : '#f5f5f5');
    ctx.fill();
    ctx.strokeStyle = acceptStates.has(s) ? '#2e9e5b' : '#999';
    ctx.lineWidth = acceptStates.has(s) ? 2.5 : 1.5;
    ctx.stroke();
    if (acceptStates.has(s)) {
      ctx.beginPath(); ctx.arc(p.x, p.y, NODE_R - 5, 0, 2 * Math.PI);
      ctx.strokeStyle = '#2e9e5b'; ctx.lineWidth = 1.5; ctx.stroke();
    }
    ctx.fillStyle = isHighlight ? '#fff' : '#222';
    ctx.font = 'bold 13px system-ui'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
    ctx.fillText(s, p.x, p.y);
  }
}

function startSim() {
  if (!automaton) { alert('Bitte zuerst den Automaten aufbauen.'); return; }
  const word = document.getElementById('wordInput').value.trim();
  const { delta, startState, alphabet } = automaton;

  simSteps = [{ state: startState, charIdx: -1 }];
  let cur = startState;
  for (let i = 0; i < word.length; i++) {
    const ch = word[i];
    if (!alphabet.includes(ch)) {
      document.getElementById('result').innerHTML = `<span class="result reject">⚠ Zeichen "${ch}" nicht im Alphabet!</span>`;
      return;
    }
    const next = delta[cur] && delta[cur][ch];
    if (!next) {
      simSteps.push({ state: null, charIdx: i });
      break;
    }
    cur = next;
    simSteps.push({ state: cur, charIdx: i });
  }
  simPos = 0;
  document.getElementById('stepCtrl').style.display = 'flex';
  renderStep(word);
}

function renderStep(word) {
  word = word || document.getElementById('wordInput').value.trim();
  const trace = document.getElementById('trace');
  trace.innerHTML = '';
  simSteps.forEach((step, idx) => {
    const el = document.createElement('div');
    const label = step.charIdx < 0
      ? `Start: ${step.state}`
      : step.state
        ? `"${word[step.charIdx]}" → ${step.state}`
        : `"${word[step.charIdx]}" → ✕ (kein Übergang)`;
    el.textContent = label;
    el.className = 'step';
    if (idx === simPos) {
      const isLast = idx === simSteps.length - 1;
      const isAccept = isLast && automaton.acceptStates.has(step.state);
      const isReject = isLast && (!step.state || !automaton.acceptStates.has(step.state));
      el.classList.add(isAccept ? 'accept' : isReject ? 'reject' : 'active');
    }
    trace.appendChild(el);
  });

  const cur = simSteps[simPos];
  document.getElementById('traceLabel').textContent =
    `Schritt ${simPos} / ${simSteps.length - 1}`;

  draw(cur.state);

  const isLast = simPos === simSteps.length - 1;
  const resEl = document.getElementById('result');
  if (isLast) {
    const accepted = cur.state && automaton.acceptStates.has(cur.state);
    resEl.innerHTML = accepted
      ? `<span class="result accept">✓ AKZEPTIERT – "${word}" gehört zur Sprache.</span>`
      : `<span class="result reject">✗ ABGELEHNT – "${word}" gehört nicht zur Sprache.</span>`;
  } else {
    resEl.innerHTML = `<span class="result running">⏳ Simulation läuft…</span>`;
  }
}

function stepForward() {
  if (simPos < simSteps.length - 1) { simPos++; renderStep(); }
}
function stepBack() {
  if (simPos > 0) { simPos--; renderStep(); }
}
function runAll() {
  simPos = simSteps.length - 1; renderStep();
}
function resetSim() {
  simSteps = []; simPos = -1;
  document.getElementById('trace').innerHTML = '';
  document.getElementById('result').innerHTML = '';
  document.getElementById('traceLabel').textContent = '';
  document.getElementById('stepCtrl').style.display = 'none';
  if (automaton) draw();
}

window.addEventListener('load', build);
window.addEventListener('resize', () => { if (automaton) draw(simSteps[simPos]?.state); });
</script>
</body>
</html>
```

## Bedienung

1. **Zustände definieren:** Jeden Zustand kommagetrennt eingeben. Anfangszustand mit `>` markieren (z. B. `>q0`), Endzustände mit `*` (z. B. `q1*`).
2. **Übergänge eingeben:** Format: `Zustand, Zeichen → Zielzustand`, eine Zeile pro Übergang.
3. **Automat aufbauen:** Auf den Button klicken – der Graph wird gezeichnet.
4. **Wort simulieren:** Wort eingeben und Schritt-für-Schritt durchgehen oder alles auf einmal ausführen.

## Vorgeladene Beispiele

### Beispiel 1: Gerade Anzahl von 1en

Der Automat erkennt alle Binärwörter mit **gerader Anzahl von 1en** (0 gilt als gerade).

| Zustand | Bedeutung |
|---------|-----------|
| q0 | Bisher gerade viele 1en gelesen (Anfangs- und Endzustand) |
| q1 | Bisher ungerade viele 1en gelesen |

Testet: `1010` ✓, `101` ✗, `11` ✓, `0000` ✓

### Beispiel 2: Binärzahl durch 3 teilbar

Der Automat erkennt Binärdarstellungen von Zahlen, die durch 3 teilbar sind.

| Zustand | Bedeutung |
|---------|-----------|
| r0 | Rest 0 beim Teilen durch 3 (Anfangs- und Endzustand) |
| r1 | Rest 1 |
| r2 | Rest 2 |

Testet: `110` (=6) ✓, `1001` (=9) ✓, `101` (=5) ✗, `0` (=0) ✓

## Aufgaben

**Aufgabe 1 – Analyse:** Ladet das erste Beispiel. Verfolgt Schritt für Schritt das Wort `1101`. In welchem Zustand landet der Automat? Wird es akzeptiert?

**Aufgabe 2 – Eigener Automat:** Konstruiert einen Automaten, der alle Wörter über `{a, b}` akzeptiert, die mit `ab` enden.

- Wie viele Zustände braucht ihr mindestens?
- Gebt die Übergangsfunktion als Tabelle an, bevor ihr sie in den Visualisierer eingebt.

**Aufgabe 3 – Ungerade Anzahl von 0en:** Modifiziert den Geradzahlen-Automaten so, dass er Wörter mit **ungerader** Anzahl von 0en akzeptiert.

**Aufgabe 4 – Grenzen des Modells:** Ein endlicher Automat kann die Sprache $ L = \{ a^n b^n \mid n \geq 1 \} $ (z. B. `ab`, `aabb`, `aaabbb`) **nicht** erkennen.

- Versucht, einen solchen Automaten zu bauen. Was passiert?
- Warum scheitert jeder endliche Automat an dieser Sprache? (Hinweis: Pumping-Lemma)

## Formale Definition

Ein DEA wird formal geschrieben als:

$$M = (Q, \Sigma, \delta, q_0, F)$$

Die vom Automaten erkannte Sprache ist:

$$L(M) = \{ w \in \Sigma^* \mid \hat{\delta}(q_0, w) \in F \}$$

wobei $\hat{\delta}$ die erweiterte Übergangsfunktion auf ganzen Wörtern ist (rekursiv definiert: $\hat{\delta}(q, \varepsilon) = q$ und $\hat{\delta}(q, xa) = \delta(\hat{\delta}(q, x), a)$).
