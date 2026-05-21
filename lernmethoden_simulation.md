<!--
author:   Lehrbildung – Mediendidaktik
language: de
version:  1.0.0
-->

# Lernmethoden & Medienwirkung – Interaktive Simulation

> Diese Simulation veranschaulicht, wie sich verschiedene **Lehr-Lernmethoden und Medien** auf den Lernerfolg auswirken – basierend auf der **Lernpyramide** (NTL Institute, Dale's Cone of Experience), der **Dual-Coding-Theorie** (Paivio) und der **kognitiven Belastungstheorie** (Sweller).

## Theoretischer Hintergrund

| Konzept | Kernaussage |
|--------|-------------|
| **Lernpyramide (NTL)** | Passive Methoden (Lesen, Zuhören) führen zu ~5–30 % Behaltensrate; aktive Methoden (Erklären, Anwenden) zu ~50–90 % |
| **Dual-Coding (Paivio)** | Gleichzeitige verbale + visuelle Verarbeitung verbessert Enkodierung und Abruf |
| **Kognitive Belastung (Sweller)** | Das Arbeitsgedächtnis ist begrenzt; zu hohe extrinsische Belastung hemmt tiefes Lernen |
| **Elaborative Interrogation** | „Warum?"-Fragen & Selbsterklärung fördern Langzeitgedächtnis signifikant |
| **Erfahrungsbasiertes Lernen (Kolb)** | Konkretes Erleben + Reflexion = tiefere Konzeptbildung |

### Die Dimensionen des Lernerfolgs in dieser Simulation

- **Behaltenserfolg (kurzfristig):** Wie viel bleibt nach einer Lerneinheit direkt im Gedächtnis?
- **Langzeitbehalten:** Wie viel ist nach Wochen/Monaten noch abrufbar?
- **Transferleistung:** Kann das Gelernte auf neue Probleme angewendet werden?
- **Prozessverständnis:** Wird das „Warum" und „Wie" verstanden – nicht nur das „Was"?
- **Kognitive Belastung:** Wie stark wird das Arbeitsgedächtnis beansprucht? (niedrig = besser für tiefes Lernen)

## Interaktive Simulation

Wählt eine Lernmethode und passt die Kontextparameter an – beobachtet, wie sich die Lernerfolgsdimensionen verändern!

<div id="learn-app" style="font-family:system-ui,sans-serif;">

<!-- Methodenauswahl -->
<div style="margin-bottom:16px;">
  <div style="font-size:13px;font-weight:600;color:#444;margin-bottom:8px;">Lernmethode wählen:</div>
  <div id="method-buttons" style="display:flex;flex-wrap:wrap;gap:8px;"></div>
</div>

<!-- Kontextparameter -->
<div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;margin-bottom:14px;">
  <div style="background:#f7f7f7;border-radius:8px;padding:10px 12px;">
    <label style="font-size:11px;color:#666;display:block;margin-bottom:3px;">Wiederholungen / Übungszeit</label>
    <div style="font-size:17px;font-weight:700;" id="rep-val">1×</div>
    <input type="range" id="repetition" min="1" max="5" step="1" value="1" style="width:100%;margin-top:4px;">
  </div>
  <div style="background:#f7f7f7;border-radius:8px;padding:10px 12px;">
    <label style="font-size:11px;color:#666;display:block;margin-bottom:3px;">Vorwissen der Lernenden</label>
    <div style="font-size:17px;font-weight:700;" id="prior-val">Mittel</div>
    <input type="range" id="prior" min="0" max="2" step="1" value="1" style="width:100%;margin-top:4px;">
  </div>
  <div style="background:#f7f7f7;border-radius:8px;padding:10px 12px;">
    <label style="font-size:11px;color:#666;display:block;margin-bottom:3px;">Motivation / Engagement</label>
    <div style="font-size:17px;font-weight:700;" id="motiv-val">Mittel</div>
    <input type="range" id="motivation" min="0" max="2" step="1" value="1" style="width:100%;margin-top:4px;">
  </div>
</div>

<!-- R0-ähnliche Kennzahl -->
<div style="background:#f0f4ff;border:1px solid #c8d5f5;border-radius:8px;padding:10px 14px;display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;">
  <div>
    <div style="font-size:12px;color:#555;">Lernwirksamkeits-Index (LWI) – gewichtetes Gesamtmaß</div>
    <div style="font-size:22px;font-weight:800;" id="lwi-val">–</div>
  </div>
  <div id="lwi-badge" style="font-size:12px;padding:4px 12px;border-radius:20px;font-weight:600;">–</div>
</div>

<!-- Balkendiagramm -->
<canvas id="learnChart" height="260" style="width:100%;border-radius:8px;border:1px solid #e5e5e5;background:#fafafa;display:block;margin-bottom:10px;"></canvas>

<!-- Legende -->
<div style="display:flex;flex-wrap:wrap;gap:14px;font-size:12px;margin-bottom:10px;">
  <span style="display:flex;align-items:center;gap:5px;"><span style="width:12px;height:10px;border-radius:2px;display:inline-block;background:#2a7ed3;"></span>Behalten (kurzfristig)</span>
  <span style="display:flex;align-items:center;gap:5px;"><span style="width:12px;height:10px;border-radius:2px;display:inline-block;background:#7c3aed;"></span>Langzeitbehalten</span>
  <span style="display:flex;align-items:center;gap=5px;"><span style="width:12px;height:10px;border-radius:2px;display:inline-block;background:#2e9e5b;"></span>Transfer</span>
  <span style="display:flex;align-items:center;gap:5px;"><span style="width:12px;height:10px;border-radius:2px;display:inline-block;background:#e09a2b;"></span>Prozessverständnis</span>
  <span style="display:flex;align-items:center;gap:5px;"><span style="width:12px;height:10px;border-radius:2px;display:inline-block;background:#e05a2b;opacity:0.75"></span>Kognitive Belastung</span>
</div>

<!-- Methodeninformation -->
<div id="method-info" style="background:#f9f9f9;border-left:4px solid #2a7ed3;border-radius:0 8px 8px 0;padding:10px 14px;font-size:12px;color:#444;line-height:1.6;min-height:40px;"></div>

</div>

<script>
(function(){

  var METHODS = [
    {
      id:'vortrag',
      label:'📢 Vortrag / Frontalunterricht',
      color:'#e05a2b',
      base:{retention:0.25, longterm:0.12, transfer:0.15, process:0.18, cogload:0.30},
      info:'Passives Zuhören. Laut NTL Institute ca. 5–10 % Behaltensrate. Gering für Transfer und Prozessverständnis. Kognitive Belastung moderat, aber ohne aktive Verarbeitung kaum Tiefenlernen.',
      pyramid: 1
    },
    {
      id:'lesen',
      label:'📖 Lesen / Textstudium',
      color:'#e0832b',
      base:{retention:0.20, longterm:0.10, transfer:0.12, process:0.14, cogload:0.25},
      info:'Rein verbale Informationsverarbeitung. Schwächste Form nach Dual-Coding-Theorie, da kein bildliches Kodieren. ~5–10 % Behaltensrate. Abhängig stark von Vorwissen.',
      pyramid: 1
    },
    {
      id:'audio_visual',
      label:'🎬 Audio-visuelle Medien',
      color:'#e0c02b',
      base:{retention:0.40, longterm:0.22, transfer:0.25, process:0.30, cogload:0.40},
      info:'Video/Animation aktiviert verbales UND bildliches Kodieren (Dual Coding). Bis zu 20 % Behaltensrate laut NTL. Risiko: hohe extrinsische kognitive Belastung bei schlecht gestalteten Medien (Sweller).',
      pyramid: 2
    },
    {
      id:'demonstration',
      label:'🔬 Demonstration / Vorführung',
      color:'#a3a300',
      base:{retention:0.50, longterm:0.30, transfer:0.35, process:0.42, cogload:0.45},
      info:'Beobachten eines Experten in action. ~30 % Behaltensrate. Fördert Prozessverständnis durch Modell-Lernen (Bandura). Besser als reine Instruktion, aber noch passiv.',
      pyramid: 3
    },
    {
      id:'diskussion',
      label:'💬 Diskussion / Peer-Learning',
      color:'#2e9e5b',
      base:{retention:0.62, longterm:0.48, transfer:0.55, process:0.60, cogload:0.55},
      info:'Aktive Auseinandersetzung mit Inhalten. ~50 % Behaltensrate laut NTL. Elaborative Interrogation durch Fragen & Antworten verstärkt Enkodierung. Soziales Lernen fördert Transfer.',
      pyramid: 4
    },
    {
      id:'ueben',
      label:'🛠️ Üben / Anwenden',
      color:'#2a7ed3',
      base:{retention:0.72, longterm:0.60, transfer:0.70, process:0.68, cogload:0.60},
      info:'Aktives Tun. ~75 % Behaltensrate. Practice & Feedback ist einer der wirksamsten Lernmechanismen (Hattie, d=0.77). Direkte Verbindung von Handlung und Verständnis.',
      pyramid: 5
    },
    {
      id:'erklaeren',
      label:'🗣️ Anderen erklären / Lehren',
      color:'#7c3aed',
      base:{retention:0.82, longterm:0.74, transfer:0.80, process:0.85, cogload:0.70},
      info:'„Protégé effect": Wer andere lehrt, lernt selbst am tiefsten. ~90 % Behaltensrate laut NTL. Erzwingt Kohärenzprüfung des eigenen Wissens und schließt Lücken.',
      pyramid: 6
    },
    {
      id:'erleben',
      label:'🌍 Erleben / Reale Erfahrung',
      color:'#0e7490',
      base:{retention:0.88, longterm:0.82, transfer:0.90, process:0.92, cogload:0.65},
      info:'Höchste Stufe der Lernpyramide und Kolbs Erfahrungslernen. Kontextuell eingebettetes Lernen mit unmittelbarer Relevanz. Transfer und Prozessverständnis maximal – Grundlage für nachhaltiges Wissen.',
      pyramid: 7
    },
  ];

  var selectedMethod = METHODS[0];

  // Build method buttons
  var btns = document.getElementById('method-buttons');
  METHODS.forEach(function(m){
    var b = document.createElement('button');
    b.textContent = m.label;
    b.dataset.id = m.id;
    b.style.cssText='border:2px solid #ddd;background:#fff;border-radius:6px;padding:6px 12px;cursor:pointer;font-size:12px;font-weight:600;transition:all 0.15s;';
    b.addEventListener('click', function(){
      selectedMethod = m;
      document.querySelectorAll('#method-buttons button').forEach(function(x){
        x.style.background='#fff'; x.style.borderColor='#ddd'; x.style.color='#333';
      });
      b.style.background = m.color;
      b.style.borderColor = m.color;
      b.style.color = '#fff';
      update();
    });
    btns.appendChild(b);
  });
  // Activate first button
  btns.children[0].style.background = METHODS[0].color;
  btns.children[0].style.borderColor = METHODS[0].color;
  btns.children[0].style.color = '#fff';

  var priorLabels = ['Gering','Mittel','Hoch'];
  var motivLabels = ['Niedrig','Mittel','Hoch'];

  function getModifiers(){
    var rep = parseInt(document.getElementById('repetition').value);
    var prior = parseInt(document.getElementById('prior').value);
    var motiv = parseInt(document.getElementById('motivation').value);
    document.getElementById('rep-val').textContent = rep + '×';
    document.getElementById('prior-val').textContent = priorLabels[prior];
    document.getElementById('motiv-val').textContent = motivLabels[motiv];
    // Repetition: logarithmische Verbesserung
    var repFactor = 1 + (Math.log(rep) / Math.log(5)) * 0.28;
    // Prior knowledge boosts transfer and process, hurts retention slightly (less novelty)
    var priorFactor = [0.88, 1.0, 1.10][prior];
    // Motivation: linear boost
    var motivFactor = [0.78, 1.0, 1.15][motiv];
    return {rep: repFactor, prior: priorFactor, motiv: motivFactor};
  }

  function clamp(v){ return Math.max(0, Math.min(1, v)); }

  function computeScores(m, mods){
    var b = m.base;
    return {
      retention:  clamp(b.retention  * mods.rep * mods.motiv),
      longterm:   clamp(b.longterm   * mods.rep * mods.prior * mods.motiv),
      transfer:   clamp(b.transfer   * mods.rep * mods.prior * mods.motiv),
      process:    clamp(b.process    * mods.rep * mods.prior * mods.motiv),
      cogload:    clamp(b.cogload    * (2 - mods.motiv * 0.3))
    };
  }

  function computeLWI(s){
    // Weighted index: longterm & transfer weighted highest
    return (s.retention * 0.15 + s.longterm * 0.30 + s.transfer * 0.28 + s.process * 0.27) * 100;
  }

  function update(){
    var mods = getModifiers();
    var s = computeScores(selectedMethod, mods);
    var lwi = computeLWI(s);

    document.getElementById('lwi-val').textContent = lwi.toFixed(1) + ' / 100';
    var badge = document.getElementById('lwi-badge');
    if(lwi >= 70){ badge.textContent='Hoch wirksam'; badge.style.background='#dcfce7'; badge.style.color='#166534'; }
    else if(lwi >= 45){ badge.textContent='Mittel wirksam'; badge.style.background='#fef9c3'; badge.style.color='#854d0e'; }
    else{ badge.textContent='Gering wirksam'; badge.style.background='#fee2e2'; badge.style.color='#991b1b'; }

    document.getElementById('method-info').innerHTML =
      '<strong>' + selectedMethod.label + '</strong><br>' + selectedMethod.info;

    drawChart(s);
  }

  function drawChart(s){
    var canvas = document.getElementById('learnChart');
    canvas.width = canvas.offsetWidth || 680;
    var ctx = canvas.getContext('2d');
    var W = canvas.width, H = canvas.height;
    ctx.clearRect(0,0,W,H);
    ctx.fillStyle='#fafafa'; ctx.fillRect(0,0,W,H);

    var bars = [
      {label:'Behalten\n(kurzfristig)', value:s.retention, color:'#2a7ed3'},
      {label:'Langzeit-\nbehalten',     value:s.longterm,  color:'#7c3aed'},
      {label:'Transfer',               value:s.transfer,  color:'#2e9e5b'},
      {label:'Prozess-\nverständnis',  value:s.process,   color:'#e09a2b'},
      {label:'Kognitive\nBelastung',   value:s.cogload,   color:'#e05a2b', isLoad:true}
    ];

    var pad = {top:20, right:20, bottom:52, left:46};
    var pW = W - pad.left - pad.right;
    var pH = H - pad.top - pad.bottom;
    var bW = pW / bars.length * 0.55;
    var gap = pW / bars.length;

    // Grid lines
    ctx.strokeStyle='#e5e5e5'; ctx.lineWidth=1;
    [0,25,50,75,100].forEach(function(g){
      var y = pad.top + pH - (g/100)*pH;
      ctx.beginPath(); ctx.moveTo(pad.left, y); ctx.lineTo(pad.left+pW, y); ctx.stroke();
      ctx.fillStyle='#999'; ctx.font='11px system-ui'; ctx.textAlign='right';
      ctx.fillText(g+'%', pad.left-5, y+4);
    });

    bars.forEach(function(bar, i){
      var x = pad.left + gap*i + gap*0.5 - bW/2;
      var barH = bar.value * pH;
      var y = pad.top + pH - barH;

      // Shadow
      ctx.shadowColor='rgba(0,0,0,0.08)'; ctx.shadowBlur=4; ctx.shadowOffsetY=2;

      // Dashed outline for cognitive load (inverse: lower is better)
      if(bar.isLoad){
        ctx.setLineDash([4,3]);
        ctx.strokeStyle=bar.color; ctx.lineWidth=2;
        ctx.strokeRect(x, y, bW, barH);
        ctx.setLineDash([]);
        // Fill with pattern
        ctx.fillStyle=bar.color+'33';
        ctx.fillRect(x, y, bW, barH);
      } else {
        ctx.fillStyle=bar.color;
        ctx.beginPath();
        ctx.roundRect ? ctx.roundRect(x, y, bW, barH, [4,4,0,0]) : ctx.rect(x, y, bW, barH);
        ctx.fill();
      }
      ctx.shadowColor='transparent'; ctx.shadowBlur=0; ctx.shadowOffsetY=0;

      // Value label on top
      ctx.fillStyle='#333'; ctx.font='bold 12px system-ui'; ctx.textAlign='center';
      ctx.fillText(Math.round(bar.value*100)+'%', x+bW/2, y-6);

      // X axis labels (multi-line)
      ctx.fillStyle='#666'; ctx.font='11px system-ui';
      var lines = bar.label.split('\n');
      lines.forEach(function(line, li){
        ctx.fillText(line, x+bW/2, pad.top+pH+16+li*14);
      });
    });

    // Pyramid level indicator
    var m = selectedMethod;
    ctx.fillStyle='#aaa'; ctx.font='11px system-ui'; ctx.textAlign='left';
    ctx.fillText('Lernpyramide-Stufe: ' + m.pyramid + '/7', pad.left, pad.top+pH+48);

    ctx.strokeStyle='#e5e5e5'; ctx.lineWidth=1;
    ctx.strokeRect(pad.left, pad.top, pW, pH);
  }

  ['repetition','prior','motivation'].forEach(function(id){
    document.getElementById(id).addEventListener('input', update);
  });

  update();
  window.addEventListener('resize', update);

})();
</script>

## Methodenvergleich – Überblick

Die folgende Tabelle zeigt Richtwerte nach gängiger Forschungslage (NTL Institute, Hattie *Visible Learning*, Mayer *Multimedia Learning*):

| Methode | Behalten (kurz) | Langzeit | Transfer | Prozessverst. | Lernpyr. |
|--------|----------------|----------|----------|----------------|----------|
| Lesen | ~10 % | ~5 % | niedrig | niedrig | 1 |
| Vortrag | ~10 % | ~8 % | niedrig | niedrig | 1 |
| Audio-visuell | ~20 % | ~12 % | mittel | mittel | 2 |
| Demonstration | ~30 % | ~18 % | mittel | mittel-hoch | 3 |
| Diskussion | ~50 % | ~35 % | hoch | hoch | 4 |
| Üben / Anwenden | ~75 % | ~55 % | sehr hoch | hoch | 5 |
| Andere lehren | ~90 % | ~70 % | sehr hoch | sehr hoch | 6 |
| Reales Erleben | ~90 % | ~80 % | maximal | maximal | 7 |

> **Hinweis zur Interpretation:** Die Prozentwerte der Lernpyramide sind vereinfachte Richtwerte und keine exakten Messwerte einer einzelnen Studie. Sie beschreiben empirische Tendenzen aus Lehr-Lernforschung und Meta-Analysen.

## Aufgaben für angehende Lehrende

**Aufgabe 1 – Methodenwahl begründen:** Wählt eine Methode mit niedrigem LWI. Unter welchen Bedingungen (Vorwissen, Wiederholungen) nähert sie sich einer Methode mit hohem LWI an? Was sagt das über den Unterricht aus?

**Aufgabe 2 – Kognitive Belastung:** Warum steigt die kognitive Belastung bei audio-visuellen Medien stärker als beim Vortrag? Recherchiert Sweilers *split-attention effect* und den *redundancy effect*.

**Aufgabe 3 – Methodenmix:** Die Simulation zeigt Einzelmethoden. Wie würde eine Sequenz aus Demonstration → Üben → Erklären aussehen? Welche Parameter würdet ihr dafür im Unterricht optimieren?

**Aufgabe 4 – Transferdesign:** Transfer ist oft das schwächste Glied. Diskutiert: Welche Unterrichtsmethode maximiert Transfer? Was müsste im Unterrichtsdesign konkret passieren, damit Prozessverständnis ≠ „Stoff abfragen" bedeutet?

## Grenzen des Modells

- **Vereinfachte Richtwerte:** Die Lernpyramide-Prozentzahlen sind Tendenzen, keine exakten Messwerte aus einer einzelnen Studie.
- **Kontextabhängigkeit:** Alter, Fach, Klassengröße, Lehrpersönlichkeit und institutioneller Kontext sind nicht modelliert.
- **Interaktionseffekte:** Kombinationsmethoden (z. B. Flipped Classroom) bilden komplexere Wirkungen ab als hier dargestellt.
- **Affektive Dimension:** Emotionen, Klassenklima und Beziehung Lehrperson–Lernende fehlen im Modell – obwohl sie laut Hattie stark lernwirksam sind.
