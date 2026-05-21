<!--
author:   Mediendidaktik – Kapitel 8
language: de
version:  1.0.0
-->

# Unterrichtsmedien im Vergleich

> Diese interaktive Übersicht zeigt **Vorteile, Nachteile und Einsatzbereiche** der klassischen Unterrichtsmedien – basierend auf mediendidaktischen Grundlagen (Dale's Cone of Experience, Funktionen von Medien im Lernprozess).

## Grundbegriff: Medien im Unterricht

> Medien sind **Vermittlungshilfen oder Mittler im Lernprozess** – sie sollen das Lernen erleichtern, intensivieren und die Motivation erhöhen.

Sie erfüllen drei Kernfunktionen:

| Funktion | Bedeutung |
|----------|-----------|
| **Demonstrationsmittel** | Sachverhalte sichtbar und erfahrbar machen |
| **Arbeitsmittel** | Aktive Auseinandersetzung der Lernenden ermöglichen |
| **Motivationsmittel** | Interesse wecken, Aufmerksamkeit binden |

## Interaktiver Medienvergleich

Wählt ein Medium – und erkundet Vorteile, Nachteile, Eignung und Lernwirksamkeit im Detail!

<div id="media-app" style="font-family:system-ui,sans-serif;font-size:14px;">

<!-- Medium-Auswahl -->
<div style="margin-bottom:16px;">
  <div style="font-size:13px;font-weight:600;color:#444;margin-bottom:8px;">Medium auswählen:</div>
  <div id="media-buttons" style="display:flex;flex-wrap:wrap;gap:8px;"></div>
</div>

<!-- Radar-ähnliche Dimensionsanzeige -->
<div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:14px;">

  <div style="background:#f7f7f7;border-radius:10px;padding:14px;">
    <div style="font-size:12px;font-weight:700;color:#555;margin-bottom:10px;text-transform:uppercase;letter-spacing:0.05em;">Didaktische Wirksamkeit</div>
    <div id="dim-bars"></div>
  </div>

  <div style="background:#f7f7f7;border-radius:10px;padding:14px;">
    <div style="font-size:12px;font-weight:700;color:#555;margin-bottom:10px;text-transform:uppercase;letter-spacing:0.05em;">Überblick</div>
    <div id="overview-box" style="font-size:12px;line-height:1.65;color:#333;"></div>
  </div>

</div>

<!-- Vorteile / Nachteile -->
<div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:14px;">
  <div style="background:#f0fdf4;border:1px solid #bbf7d0;border-radius:10px;padding:12px 14px;">
    <div style="font-size:12px;font-weight:700;color:#166534;margin-bottom:8px;">✅ Vorteile</div>
    <ul id="pros-list" style="margin:0;padding-left:18px;font-size:12px;color:#1a3a22;line-height:1.75;"></ul>
  </div>
  <div style="background:#fef2f2;border:1px solid #fecaca;border-radius:10px;padding:12px 14px;">
    <div style="font-size:12px;font-weight:700;color:#991b1b;margin-bottom:8px;">⚠️ Nachteile</div>
    <ul id="cons-list" style="margin:0;padding-left:18px;font-size:12px;color:#3a1a1a;line-height:1.75;"></ul>
  </div>
</div>

<!-- Einsatz -->
<div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:14px;">
  <div style="background:#fffbeb;border:1px solid #fde68a;border-radius:10px;padding:12px 14px;">
    <div style="font-size:12px;font-weight:700;color:#92400e;margin-bottom:8px;">👍 Geeignet für …</div>
    <ul id="good-for-list" style="margin:0;padding-left:18px;font-size:12px;color:#3a2a00;line-height:1.75;"></ul>
  </div>
  <div style="background:#f5f3ff;border:1px solid #ddd6fe;border-radius:10px;padding:12px 14px;">
    <div style="font-size:12px;font-weight:700;color:#5b21b6;margin-bottom:8px;">👎 Weniger geeignet für …</div>
    <ul id="bad-for-list" style="margin:0;padding-left:18px;font-size:12px;color:#2a1a5a;line-height:1.75;"></ul>
  </div>
</div>

<!-- Vergleichsgrafik alle Medien -->
<div style="margin-bottom:8px;">
  <div style="font-size:12px;font-weight:700;color:#555;margin-bottom:8px;text-transform:uppercase;letter-spacing:0.05em;">Gesamtvergleich aller Medien</div>
  <canvas id="compareChart" height="220" style="width:100%;border-radius:8px;border:1px solid #e5e5e5;background:#fafafa;display:block;"></canvas>
</div>

<!-- Legende Dimensionen -->
<div style="display:flex;flex-wrap:wrap;gap:12px;font-size:11px;color:#666;margin-top:6px;">
  <span>📊 Balken = ausgewählte Dimension</span>
  <span style="display:flex;align-items:center;gap:4px;"><span style="width:12px;height:3px;background:#2a7ed3;display:inline-block;border-radius:2px;"></span>Visualisierungskraft</span>
  <span style="display:flex;align-items:center;gap:4px;"><span style="width:12px;height:3px;background:#2e9e5b;display:inline-block;border-radius:2px;"></span>Blickkontakt</span>
  <span style="display:flex;align-items:center;gap:4px;"><span style="width:12px;height:3px;background:#7c3aed;display:inline-block;border-radius:2px;"></span>Flexibilität</span>
  <span style="display:flex;align-items:center;gap:4px;"><span style="width:12px;height:3px;background:#e09a2b;display:inline-block;border-radius:2px;"></span>Vorbereitungsaufwand (⬇ besser)</span>
  <span style="display:flex;align-items:center;gap:4px;"><span style="width:12px;height:3px;background:#e05a2b;display:inline-block;border-radius:2px;"></span>Motivationspotenzial</span>
</div>

</div>

<script>
(function(){

  var DIMS = [
    {key:'visual',   label:'Visualisierungskraft', color:'#2a7ed3'},
    {key:'contact',  label:'Blickkontakt möglich',  color:'#2e9e5b'},
    {key:'flex',     label:'Flexibilität / Spontanität', color:'#7c3aed'},
    {key:'prep',     label:'Vorbereitungsaufwand (⬇)',  color:'#e09a2b', inverse:true},
    {key:'motiv',    label:'Motivationspotenzial',  color:'#e05a2b'},
  ];

  var MEDIA = [
    {
      id:'sprache',
      label:'🗣️ Sprache / Vortrag',
      color:'#64748b',
      dims:{visual:0.15, contact:0.95, flex:0.90, prep:0.15, motiv:0.45},
      overview:'Reiner Lehrervortrag ohne visuelle Unterstützung. Stärkster Wirklichkeitsgrad: direkte Kommunikation. Aber: Informationen flüchtig, keine bleibende Spur.',
      pros:['Direkter, ungefilterter Blickkontakt jederzeit möglich','Maximale Flexibilität – kein Aufbau, keine Technik nötig','Spontane Anpassung an die Klasse jederzeit möglich','Persönlichste Kommunikationsform'],
      cons:['Keinerlei visuelle Unterstützung – rein verbal','Informationen verflüchtigen sofort (keine Dokumentation)','Sehr hohe Anforderung an Gedächtnis und Konzentration der Lernenden','Kein Rückgriff auf bereits Gesagtes möglich'],
      goodFor:['Einleitung und Motivierung','Mündliche Erklärungen und Diskussion','Überleitung zwischen Medien und Phasen'],
      badFor:['Komplexe Sachverhalte mit vielen Details','Längere Lehrphasen ohne Methodenwechsel','Lernende mit geringer Sprachkompetenz'],
    },
    {
      id:'tafel',
      label:'🖊️ Tafel / Whiteboard',
      color:'#1d4ed8',
      dims:{visual:0.65, contact:0.35, flex:0.80, prep:0.25, motiv:0.55},
      overview:'Klassischstes Unterrichtsmedium. Ideal für schrittweise Entwicklung von Inhalten. Kein Blickkontakt beim Schreiben – dafür maximale Korrektur- und Ergänzungsfreiheit.',
      pros:['Einfache, zuverlässige Handhabung – keine Technik','Große Schreibfläche für gleichzeitig mehrere Inhalte','Jederzeit lösch- und korrigierbar','Schrittweises Entwickeln von Sachverhalten möglich','Sammeln und Strukturieren (z. B. Brainstorming)'],
      cons:['Kein Augenkontakt beim Anschreiben','An- und Abschreiben ist zeitaufwendig','Hohe Fehlergefahr beim Abschreiben für Lernende','Inhalte gehen nach dem Unterricht verloren'],
      goodFor:['Schrittweise Entwicklung (math. Ableitungen, Argumentationsketten)','Erklärungen neuer Begriffe, Fachterminologie, Zahlen','Sammeln von Ideen und Vorschlägen','Spontane Ergänzungen im Unterrichtsgespräch'],
      badFor:['Darstellungen, die dauerhaft sichtbar bleiben sollen','Zeitaufwendige Schreib- und Zeichenarbeiten','Große Gruppen mit schlechter Sicht auf die Tafel'],
    },
    {
      id:'ohp',
      label:'📽️ Overhead-Projektor',
      color:'#0f766e',
      dims:{visual:0.75, contact:0.80, flex:0.70, prep:0.55, motiv:0.60},
      overview:'Projizierte Folie + Blickkontakt gleichzeitig möglich. Folien vorbereiten, aufheben und wiederverwenden. Schrittweise Offenbarungstechnik steuerbar.',
      pros:['Blickkontakt zur Klasse bleibt erhalten','Raum muss nicht verdunkelt werden','Folien vorbereiten, aufbewahren und wiederverwenden','Beliebig viele Kopien der Folien möglich','Schrittweise Offenbarung durch Abdecktechnik','Dokumentiert Gruppenarbeitsergebnisse für Plenum'],
      cons:['Technisches Gerät nötig (Aufwand, Störungsrisiko)','Folienerstellung erfordert Vorbereitung','Bei schlechten Folien: Überladung, unlesbare Schrift','Weniger flexibel für spontane Änderungen als Tafel'],
      goodFor:['Strukturierte Präsentation vorbereiteter Inhalte','Schrittweise Entwicklung (Abdecktechnik)','Visualisierung komplexer Sachverhalte','Dokumentation von Ergebnissen (Gruppenarbeiten)'],
      badFor:['Spontane, ungeplante Unterrichtsphasen','Sehr kleine Gruppen (Overshooting)','Wenn kein Gerät vorhanden ist'],
    },
    {
      id:'ppt',
      label:'💻 PowerPoint',
      color:'#b45309',
      dims:{visual:0.90, contact:0.70, flex:0.30, prep:0.80, motiv:0.70},
      overview:'Höchste Visualisierungskraft aller klassischen Medien. Unterstützt Lehrervortrag – ersetzt ihn aber nicht. Achtung: Verdunklung, Ablenkung durch Technik, Aufmerksamkeitsverlust.',
      pros:['Blickkontakt möglich (Lehrkraft muss nicht zur Fläche schauen)','Visualisierung erleichtert Zugang zur Thematik erheblich','Effektive Ergänzung und Unterstützung des Lehrervortrags','Erleichtert Lernenden das Mitschreiben','Medienreich: Bilder, Grafiken, Video, Animationen kombinierbar'],
      cons:['Raum muss häufig verdunkelt werden','Aufmerksamkeit lässt schnell nach (passive Rezeption)','Technik lenkt vom Präsentierenden ab','Kein Ersatz für aktiven Vortrag und Interaktion','Vorbereitung zeitaufwendig','Gefahr: zu viel Text / Informationsüberladung (Folie als Skript)'],
      goodFor:['Strukturierte, medienreiche Präsentationen','Visualisierung abstrakter Sachverhalte','Begleitung komplexerer Lehrvorträge','Unterrichtseinheiten mit Vorbereitung'],
      badFor:['Spontane oder interaktive Unterrichtsphasen','Kleine Gruppen ohne Beamer','Wenn Raum nicht verdunkelbar','Als Ersatz für Lehrervortrag / Interaktion'],
    },
    {
      id:'flipchart',
      label:'📋 Flipchart',
      color:'#7c3aed',
      dims:{visual:0.60, contact:0.60, flex:0.65, prep:0.35, motiv:0.65},
      overview:'Papierbasiert, dauerhaft sichtbar und einfach transportierbar. Ideal für kleine Gruppen und Workshopsettings. Korrekturen sind schwierig – Disziplin beim Schreiben nötig.',
      pros:['Einfache, unkomplizierte Handhabung','Informationen während des Vortrags dauerhaft sichtbar','Charts lassen sich teilweise wiederverwenden','Locker-kreative Atmosphäre (Workshop-Charakter)','Kein Strom, keine Technik nötig'],
      cons:['Korrekturen sind schwierig – Disziplin nötig','Handouts nur durch Abschreiben oder Fotografieren','Eher für kleine Gruppen geeignet','Begrenzte Schreibfläche pro Seite','Schrift muss groß und leserlich sein – zeitintensiv'],
      goodFor:['Seminare und Workshops mit kleinen Gruppen','Ergebnissicherung von Gruppenarbeiten','Moderierte Diskussionen und Brainstormings','Visualisierung von Prozessen und Strukturen'],
      badFor:['Große Klassen (>25 Personen)','Komplexe Grafiken und Diagramme','Wenn Korrekturen häufig nötig sind'],
    },
  ];

  var selected = MEDIA[0];

  function makeList(ulId, items){
    var ul = document.getElementById(ulId);
    ul.innerHTML = '';
    items.forEach(function(item){
      var li = document.createElement('li');
      li.textContent = item;
      ul.appendChild(li);
    });
  }

  function renderDimBars(m){
    var box = document.getElementById('dim-bars');
    box.innerHTML = '';
    DIMS.forEach(function(d){
      var val = m.dims[d.key];
      var pct = Math.round(val*100);
      var row = document.createElement('div');
      row.style.cssText = 'margin-bottom:8px;';
      var labelRow = document.createElement('div');
      labelRow.style.cssText = 'display:flex;justify-content:space-between;font-size:11px;color:#555;margin-bottom:3px;';
      labelRow.innerHTML = '<span>'+d.label+'</span><span style="font-weight:700;color:'+d.color+'">'+pct+'%</span>';
      var track = document.createElement('div');
      track.style.cssText = 'background:#e5e5e5;border-radius:20px;height:8px;width:100%;overflow:hidden;';
      var fill = document.createElement('div');
      fill.style.cssText = 'background:'+d.color+';height:100%;border-radius:20px;width:'+pct+'%;transition:width 0.4s ease;';
      track.appendChild(fill);
      row.appendChild(labelRow);
      row.appendChild(track);
      box.appendChild(row);
    });
  }

  function renderOverview(m){
    var box = document.getElementById('overview-box');
    box.innerHTML =
      '<div style="font-size:20px;margin-bottom:6px;">'+m.label+'</div>'+
      '<div style="color:#555;line-height:1.65;">'+m.overview+'</div>'+
      '<div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;">' +
      DIMS.map(function(d){
        var v = Math.round(m.dims[d.key]*100);
        return '<span style="background:'+d.color+'22;color:'+d.color+';border:1px solid '+d.color+'44;border-radius:20px;padding:2px 9px;font-size:11px;font-weight:600;">'+d.label.split('/')[0].trim()+': '+v+'%</span>';
      }).join('') + '</div>';
  }

  function drawCompare(){
    var canvas = document.getElementById('compareChart');
    canvas.width = canvas.offsetWidth || 680;
    var ctx = canvas.getContext('2d');
    var W = canvas.width, H = canvas.height;
    ctx.clearRect(0,0,W,H);
    ctx.fillStyle='#fafafa'; ctx.fillRect(0,0,W,H);

    var pad = {top:14, right:16, bottom:44, left:14};
    var nMedia = MEDIA.length;
    var nDims  = DIMS.length;
    var groupW = (W - pad.left - pad.right) / nMedia;
    var barW   = groupW / (nDims + 1);
    var pH     = H - pad.top - pad.bottom;

    // Grid
    ctx.strokeStyle='#e5e5e5'; ctx.lineWidth=1;
    [0,50,100].forEach(function(g){
      var y = pad.top + pH - (g/100)*pH;
      ctx.beginPath(); ctx.moveTo(pad.left, y); ctx.lineTo(W-pad.right, y); ctx.stroke();
    });

    MEDIA.forEach(function(m, mi){
      var gx = pad.left + mi*groupW;
      DIMS.forEach(function(d, di){
        var val = m.dims[d.key];
        var x = gx + (di+0.5)*barW;
        var barH = val * pH;
        var y = pad.top + pH - barH;
        var isSelected = (m.id === selected.id);

        ctx.globalAlpha = isSelected ? 1 : 0.35;
        ctx.fillStyle = d.color;
        if(ctx.roundRect){
          ctx.beginPath(); ctx.roundRect(x, y, barW*0.82, barH, [3,3,0,0]); ctx.fill();
        } else {
          ctx.fillRect(x, y, barW*0.82, barH);
        }
        ctx.globalAlpha = 1;
      });

      // Medium label
      ctx.fillStyle = (m.id===selected.id) ? m.color : '#999';
      ctx.font = (m.id===selected.id) ? 'bold 11px system-ui' : '10px system-ui';
      ctx.textAlign='center';
      var lx = gx + groupW/2 - barW*0.2;
      // Split label after emoji
      var parts = m.label.split(' ');
      var emoji = parts[0];
      var text  = parts.slice(1).join(' ');
      ctx.fillText(emoji+' '+text.substring(0,10), lx, H-pad.bottom+13);
      if(text.length>10) ctx.fillText(text.substring(10), lx, H-pad.bottom+25);

      // Highlight selected group
      if(m.id === selected.id){
        ctx.strokeStyle = m.color;
        ctx.lineWidth = 2;
        ctx.setLineDash([3,3]);
        ctx.strokeRect(gx+2, pad.top, groupW - barW*0.5, pH);
        ctx.setLineDash([]);
      }
    });

    ctx.strokeStyle='#e5e5e5'; ctx.lineWidth=1;
    ctx.strokeRect(pad.left, pad.top, W-pad.left-pad.right, pH);
  }

  function update(){
    renderDimBars(selected);
    renderOverview(selected);
    makeList('pros-list', selected.pros);
    makeList('cons-list', selected.cons);
    makeList('good-for-list', selected.goodFor);
    makeList('bad-for-list', selected.badFor);
    drawCompare();
  }

  // Build buttons
  var btns = document.getElementById('media-buttons');
  MEDIA.forEach(function(m){
    var b = document.createElement('button');
    b.textContent = m.label;
    b.dataset.id = m.id;
    b.style.cssText = 'border:2px solid #ddd;background:#fff;color:#333;border-radius:7px;padding:7px 13px;cursor:pointer;font-size:12px;font-weight:600;transition:all 0.15s;';
    b.addEventListener('click', function(){
      selected = m;
      document.querySelectorAll('#media-buttons button').forEach(function(x){
        x.style.background='#fff'; x.style.borderColor='#ddd'; x.style.color='#333';
      });
      b.style.background = m.color; b.style.borderColor = m.color; b.style.color = '#fff';
      update();
    });
    btns.appendChild(b);
  });
  // Activate first
  btns.children[0].style.background = MEDIA[0].color;
  btns.children[0].style.borderColor = MEDIA[0].color;
  btns.children[0].style.color = '#fff';

  update();
  window.addEventListener('resize', update);

})();
</script>

## Synoptische Übersicht

| Medium | Visualisierung | Blickkontakt | Flexibilität | Vorbereitung | Motivation |
|--------|:--------------:|:------------:|:------------:|:------------:|:----------:|
| 🗣️ Sprache | ★☆☆☆ | ★★★★ | ★★★★ | ★☆☆☆ | ★★☆☆ |
| 🖊️ Tafel / WB | ★★★☆ | ★★☆☆ | ★★★★ | ★☆☆☆ | ★★★☆ |
| 📽️ Overhead | ★★★★ | ★★★★ | ★★★☆ | ★★★☆ | ★★★☆ |
| 💻 PowerPoint | ★★★★ | ★★★☆ | ★★☆☆ | ★★★★ | ★★★★ |
| 📋 Flipchart | ★★★☆ | ★★★☆ | ★★★☆ | ★★☆☆ | ★★★☆ |

> ★ = niedrig / schwach &nbsp;|&nbsp; ★★★★ = hoch / stark &nbsp;(bei Vorbereitungsaufwand gilt: ★★★★ = hoher Aufwand)

## Aufgaben

**Aufgabe 1 – Medienwahl begründen:** Eine neue Lehrkraft plant eine 45-minütige Einheit zur Einführung in ein neues Thema. Welches Medium oder welche Medienkombination würdet ihr empfehlen? Begründet anhand der didaktischen Dimensionen.

**Aufgabe 2 – Nachteile ausgleichen:** Wie kann die Lehrkraft den fehlenden Blickkontakt beim Tafeleinsatz kompensieren? Welche Methoden oder Rituale helfen?

**Aufgabe 3 – PowerPoint-Kritik:** Die Folie wird oft als „Skript" missbraucht. Formuliert drei konkrete Qualitätskriterien für gute PowerPoint-Folien (vgl. auch die Hinweise aus der Präsentation: Textgröße, Farben, Informationsdichte).

**Aufgabe 4 – Medienpyramide:** Ordnet die fünf Medien auf Dales „Cone of Experience" ein. Welches Medium liegt am nächsten an der „direkten Erfahrung"? Was fehlt allen fünf Medien gemeinsam?

**Aufgabe 5 – Medienplanung:** Entwerft eine 20-minütige Unterrichtssequenz, in der mindestens drei verschiedene Medien sinnvoll kombiniert werden. Skizziert: Was wird mit welchem Medium vermittelt – und warum?

## Qualitätskriterien für gute Folien (OHP & PowerPoint)

Gute Folien …

- erzeugen **Aufmerksamkeit** und regen zum Nachdenken an
- geben **Impulse** und motivieren für die Sache
- haben einen **hohen Behaltenseffekt**
- **verdeutlichen und visualisieren** Sachverhalte
- lassen durch Aufbau-, Unterleg- und Abdecktechnik eine **schrittweise Gedankenentwicklung** zu
- dokumentieren Arbeitsergebnisse und erlauben ihre **plenare Diskussion**

### Achtung bei PowerPoint besonders:

- ❌ Nicht zu viel Text auf einer Folie
- ❌ Kein unruhiger Hintergrund
- ✅ Textgröße mindestens **18 pt**
- ✅ Nicht mehr als **3–4 Farben**
- ✅ Die Folie ergänzt den Vortrag – sie **ersetzt ihn nicht**
