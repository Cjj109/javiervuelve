# PROMPT PARA CLAUDE EN VS CODE
## Agregar dos nuevas secciones a javiervuelve/index.html

---

## CONTEXTO DEL PROYECTO

Este es un tributo web personal de Carlos y Anthonny para su amigo Javier, celebrando 20 años de amistad. Es un **archivo único** (`index.html`, ~1.5MB) con todo el CSS y JS inline. No usa frameworks — es vanilla HTML/CSS/JS.

### Stack
- HTML5 semántico
- CSS3 (grid, flexbox, transforms, animations, clip-path)
- JavaScript vanilla (DOM manipulation, Canvas API, IntersectionObserver, Web Audio)
- Fonts: Cinzel (títulos), Cormorant Garamond (subtítulos), Inter (body)

### Paleta
```css
--dark:  #010608;
--card:  #050d0a;
--gold:  #f0b000;
--green: #00c87a;
--text:  #dde8e2;
--muted: rgba(195,215,205,0.80);
```

### Patrón de secciones
Cada sección sigue este patrón HTML:
```html
<div class="[nombre]-section" id="[nombre]-section">
  <span class="s-label" style="text-align:center;display:block;">[Subtítulo pequeño]</span>
  <h2 class="s-title" style="text-align:center;">[Título con <em>énfasis dorado</em>]</h2>
  <!-- contenido -->
</div>
```

Las clases `s-label`, `s-title`, `.s`, `.rule`, `.body-text` ya existen.

### Sistema de color del canvas
Hay un canvas de fondo fijo (`#cvs`) que cambia de color por sección. Los colores se definen en `SECTION_COLORS`:
```js
const SECTION_COLORS = {
  hero:     [0, 230, 140],
  grupo:    [0, 210, 255],
  swatches: [0, 230, 140],
  gatsby:   [0, 255, 120],
  office:   [255, 180, 0],
  letters:  [60, 100, 255],
  habbo:    [0, 240, 180],
  vinyl:    [255, 40, 100],
  strange:  [255, 100, 30],
  kintsugi: [255, 190, 0]
};
```

Y hay un `sectionMap` que conecta selectores con las keys de color:
```js
const sectionMap = [
  { sel: '.hero',             key: 'hero' },
  { sel: '#grupo-section',    key: 'grupo' },
  { sel: '#swatches-section', key: 'swatches' },
  { sel: '#gatsby-section',   key: 'gatsby' },
  { sel: '#office-section',   key: 'office' },
  { sel: '#room-section',     key: 'habbo' },
  { sel: '#letters-section',  key: 'letters' },
  { sel: '#vinyl-section',    key: 'vinyl' },
  { sel: '#strange-section',  key: 'strange' },
  { sel: '#kintsugi',         key: 'kintsugi' }
];
```

### Patrón de audio existente
El Tesseract usa un MP3 externo con fade-in:
```js
var audio = document.getElementById('tesseract-audio');
audio.currentTime = 0;
audio.volume = 0;
audio.play().then(function() {
  var fi = setInterval(function() {
    if (audio.volume < 0.95) { audio.volume = Math.min(1, audio.volume + 0.03); }
    else { audio.volume = 1; clearInterval(fi); }
  }, 80);
});
```

El vinyl usa click-to-play:
```html
<audio id="vinyl-audio" src="data:audio/mpeg;base64,..."></audio>
```

---

## ORDEN DE LAS SECCIONES EN LA PÁGINA

El orden actual (del HTML) es:
1. Hero
2. Grupo
3. Swatches (cards/Kintsugi)
4. Gatsby
5. Office
6. Room (Habbo)
7. Letters (Tesseract)
8. **Vinyl (Canserbero)**
9. **Doctor Strange**
10. **Kintsugi (cierre)**

### Nuevo orden con las dos secciones:
1. Hero
2. Grupo
3. Swatches
4. Gatsby
5. Office
6. Room (Habbo)
7. Letters (Tesseract)
8. Vinyl (Canserbero)
9. **🆕 WHIPLASH** ← NUEVA (entre Vinyl y Strange)
10. Doctor Strange
11. **🆕 ADAGIO STICKMAN FLASHBACK** ← NUEVA (entre Strange y Kintsugi)
12. Kintsugi (cierre)

---

## ARCHIVOS DE AUDIO

Los audios estarán en `public/audio/`:
- `public/audio/Whiplash.mp3` — Clip del clímax de "Caravan" (el solo de batería final)
- `public/audio/Adagio.mp3` — Corte de Adagio for Strings de Barber (interpretación de Dudamel/Vienna Phil), desde ~min 3:15 hasta ~min 7:30 del video original (~4 min de audio, pero los timestamps exactos del corte los dará el usuario)

---

# SECCIÓN 1: WHIPLASH — "¿Fui suficiente?"

## Territorio emocional
Esta sección cubre la **autocrítica** — algo que ninguna otra sección toca. No es lo que Javier hizo o dejó de hacer. Es lo que Carlos no hizo. La culpa de no haber sido mejor amigo. Whiplash habla de la práctica brutal de intentar hasta que sangras. Reconectar con alguien después de años de silencio se siente igual.

## Diseño visual

### Color del canvas
```js
whiplash: [180, 40, 40]  // Rojo oscuro — tensión, esfuerzo, sangre del baterista
```

### Layout
- Fondo oscuro total. Sin adornos.
- Un **spotlight circular** desde arriba al centro de la sección (CSS radial-gradient o un pseudo-element). Como la luz que ilumina la silla del baterista en Shaffer Conservatory.
- El spotlight es rojo-ámbar tenue, no blanco.

### Contenido
El contenido son **frases de autocrítica** que aparecen y se desvanecen, una a la vez, al centro bajo el spotlight. Como golpes de tambor — secas, directas, sin adorno.

Las frases (en orden):
1. "¿Pude haber llamado más?"
2. "¿Debí haber insistido?"
3. "¿Cuántas veces elegí el silencio por cobardía?"

Cada frase:
- Aparece con un fade-in rápido (0.3s)
- Se queda visible 3-4 segundos
- Se desvanece con fade-out (0.5s)
- La siguiente aparece después de 0.5s de pausa

### Tipografía de las frases
- Font: `Cormorant Garamond`, italic
- Tamaño: `clamp(1.2rem, 3.5vw, 2rem)`
- Color: `var(--text)` con `text-shadow: 0 0 30px rgba(180,40,40,0.3)`
- Centrado

### Audio
Un `<audio>` element con src `public/audio/Whiplash.mp3`. El audio se dispara con un botón de play (similar al vinyl — el usuario controla cuándo empieza). Un botón minimalista debajo del título que diga algo como "Escuchar" o tenga un ícono de play.

Cuando el audio empieza, las frases comienzan su secuencia automáticamente. El audio tiene fade-in como el Tesseract.

### La frase final
Después de las 3 frases de autocrítica y una pausa de 1.5s, aparece la frase de cierre con estilo diferente:

> "No fui el amigo perfecto. Pero estoy aquí. Y eso también cuenta."

Esta frase:
- Font: `Cormorant Garamond`, no italic (a diferencia de las preguntas)
- Color: `var(--gold)`
- `text-shadow: 0 0 20px rgba(240,176,0,0.25)`
- Aparece letra por letra o palabra por palabra con un delay sutil
- Se queda visible permanentemente (no se desvanece)

### Título de la sección
```html
<span class="s-label">Shaffer Conservatory</span>
<h2 class="s-title">¿Fui <em>suficiente</em>?</h2>
```

### Estructura HTML sugerida
```html
<!-- ══════════════ WHIPLASH — ¿FUI SUFICIENTE? ══════════════ -->
<div class="whiplash-section" id="whiplash-section">
  <span class="s-label" style="text-align:center;display:block;">Shaffer Conservatory</span>
  <h2 class="s-title" style="text-align:center;">¿Fui <em>suficiente</em>?</h2>
  <div class="rule" style="margin:0 auto 2rem;"></div>

  <div class="whiplash-stage">
    <div class="whiplash-spotlight"></div>
    <p class="whiplash-phrase" id="whiplash-phrase"></p>
    <p class="whiplash-closing" id="whiplash-closing"></p>
  </div>

  <button class="whiplash-play" id="whiplash-play" aria-label="Reproducir audio de Whiplash">▶</button>
  <audio id="whiplash-audio" src="public/audio/Whiplash.mp3" preload="auto"></audio>
</div>
```

### CSS clave
```css
.whiplash-section {
  position: relative; z-index: 1;
  text-align: center;
  padding: 5rem 1.25rem;
  min-height: 80vh;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
}

.whiplash-stage {
  position: relative;
  min-height: 200px;
  display: flex; align-items: center; justify-content: center;
  flex-direction: column;
}

.whiplash-spotlight {
  position: absolute;
  top: -100px; left: 50%; transform: translateX(-50%);
  width: 300px; height: 400px;
  background: radial-gradient(ellipse at center top, rgba(180,40,40,0.12) 0%, transparent 70%);
  pointer-events: none;
}

.whiplash-phrase {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-style: italic;
  font-size: clamp(1.2rem, 3.5vw, 2rem);
  color: var(--text);
  text-shadow: 0 0 30px rgba(180,40,40,0.3);
  opacity: 0;
  transition: opacity 0.3s ease;
  line-height: 1.6;
}
.whiplash-phrase.visible { opacity: 1; }

.whiplash-closing {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(1.1rem, 3vw, 1.7rem);
  color: var(--gold);
  text-shadow: 0 0 20px rgba(240,176,0,0.25);
  opacity: 0;
  transition: opacity 0.8s ease;
  margin-top: 2rem;
}
.whiplash-closing.visible { opacity: 1; }

.whiplash-play {
  margin-top: 2rem;
  background: none; border: 1px solid rgba(180,40,40,0.4);
  color: var(--text); font-size: 1.2rem;
  width: 48px; height: 48px; border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}
.whiplash-play:hover {
  border-color: rgba(180,40,40,0.8);
  box-shadow: 0 0 20px rgba(180,40,40,0.2);
}
```

### JS — Lógica
```js
/* ─── Whiplash: autocrítica ─── */
(function() {
  var audio = document.getElementById('whiplash-audio');
  var playBtn = document.getElementById('whiplash-play');
  var phraseEl = document.getElementById('whiplash-phrase');
  var closingEl = document.getElementById('whiplash-closing');
  if (!playBtn || !phraseEl) return;

  var phrases = [
    '¿Pude haber llamado más?',
    '¿Debí haber insistido?',
    '¿Cuántas veces elegí el silencio por cobardía?'
  ];
  var closing = 'No fui el amigo perfecto. Pero estoy aquí. Y eso también cuenta.';
  var running = false;
  var timers = [];

  function schedule(fn, ms) {
    var id = setTimeout(fn, ms);
    timers.push(id);
    return id;
  }

  playBtn.addEventListener('click', function() {
    if (running) return;
    running = true;
    playBtn.style.display = 'none';

    // Audio fade-in
    if (audio) {
      audio.currentTime = 0;
      audio.volume = 0;
      audio.play().then(function() {
        var fi = setInterval(function() {
          if (audio.volume < 0.95) { audio.volume = Math.min(1, audio.volume + 0.03); }
          else { audio.volume = 1; clearInterval(fi); }
        }, 80);
      }).catch(function() {});
    }

    // Phrase sequence
    var delay = 2000; // initial wait
    phrases.forEach(function(text, i) {
      schedule(function() {
        phraseEl.textContent = text;
        phraseEl.classList.add('visible');
      }, delay);

      delay += 3500; // visible duration

      schedule(function() {
        phraseEl.classList.remove('visible');
      }, delay);

      delay += 800; // pause between phrases
    });

    // Closing phrase
    schedule(function() {
      phraseEl.style.display = 'none';
      closingEl.textContent = closing;
      closingEl.classList.add('visible');
    }, delay + 1500);
  });
})();
```

### Integración con el sistema de color
Agregar al objeto `SECTION_COLORS`:
```js
whiplash: [180, 40, 40]   // rojo oscuro — tensión, autocrítica
```

Agregar al array `sectionMap` (entre vinyl y strange):
```js
{ sel: '#whiplash-section', key: 'whiplash' }
```

### Responsive
Seguir el patrón de las otras secciones. En mobile, reducir padding a `3rem 1rem` y asegurarse que el spotlight se vea bien.

---

# SECCIÓN 2: ADAGIO FOR STRINGS — STICKMAN FLASHBACK

## Territorio emocional
Esta es la sección más poderosa de toda la página. Javier se va. Carlos tiene un flashback de todos los recuerdos. La música escala. El silencio golpea. Y en la coda, Carlos mira al usuario (a Javier) y dice la frase final. Es el corazón de la página.

## Diseño visual general
- Un **canvas** ocupa toda la sección (como el Habbo room pero más grande, full-width del viewport)
- Fondo del canvas: oscuro, con un gradiente sutil púrpura-azul que va cambiando
- Los stickmen se dibujan con trazo ligeramente imperfecto (hand-drawn feel) — agregar un pequeño random offset (±1px) a cada punto del stickman en cada frame para que las líneas "tiemblen" suavemente
- Color de los stickmen: blanco (`#dde8e2`) con lineWidth de 2.5-3px
- Color de los fantasmas/recuerdos: dorado (`#f0b000`) con `globalAlpha: 0.35` y `shadowBlur: 15, shadowColor: 'rgba(240,176,0,0.5)'`

## Color del canvas de fondo (sistema Eternal Sunshine)
```js
adagio: [100, 50, 160]    // púrpura profundo — el flashback, la memoria
```

## Estructura HTML
```html
<!-- ══════════════ ADAGIO — STICKMAN FLASHBACK ══════════════ -->
<div class="adagio-section" id="adagio-section">
  <span class="s-label" style="text-align:center;display:block;">Adagio for Strings</span>
  <h2 class="s-title" style="text-align:center;">Lo que <em>fuiste</em></h2>
  <div class="rule" style="margin:0 auto 2rem;"></div>

  <div class="adagio-stage">
    <canvas id="adagio-cvs" aria-label="Animación de recuerdos — stickmen"></canvas>
    <p class="adagio-text" id="adagio-text"></p>
  </div>

  <button class="adagio-play" id="adagio-play" aria-label="Reproducir escena">Reproducir</button>
  <audio id="adagio-audio" src="public/audio/Adagio.mp3" preload="auto"></audio>
</div>
```

## CSS
```css
.adagio-section {
  position: relative; z-index: 1;
  text-align: center;
  padding: 5rem 1.25rem 3rem;
}

.adagio-stage {
  position: relative;
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  aspect-ratio: 16 / 9;
}

.adagio-stage canvas {
  width: 100%; height: 100%;
  border-radius: 8px;
  display: block;
}

.adagio-text {
  position: absolute;
  bottom: 15%;
  left: 50%; transform: translateX(-50%);
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(1.1rem, 3vw, 1.8rem);
  color: var(--gold);
  text-shadow: 0 0 20px rgba(240,176,0,0.3);
  opacity: 0;
  transition: opacity 1s ease;
  text-align: center;
  width: 80%;
  pointer-events: none;
}
.adagio-text.visible { opacity: 1; }

.adagio-play {
  margin-top: 1.5rem;
  background: none;
  border: 1px solid rgba(100,50,160,0.4);
  color: var(--text);
  font-family: 'Inter', sans-serif;
  font-size: 0.85rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  padding: 0.7rem 2rem;
  border-radius: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}
.adagio-play:hover {
  border-color: rgba(100,50,160,0.8);
  box-shadow: 0 0 20px rgba(100,50,160,0.2);
}
```

## JS — El motor de animación

### Estructura general
```
(function() {
  // SETUP: canvas, context, dimensions, DPR
  // STICKMAN DRAWING FUNCTIONS
  // SCENE DEFINITIONS (7 memories)
  // ANIMATION TIMELINE (synced to audio.currentTime)
  // PLAY BUTTON handler
})();
```

### Función para dibujar un stickman

El stickman se dibuja con líneas simples. Los parámetros controlan posición, escala (para niños vs adultos), pose de extremidades, y color/opacidad.

```js
function drawStickman(ctx, x, y, scale, pose, color, alpha) {
  // pose = { headTilt, bodyLean, armL, armR, legL, legR }
  // Todos son ángulos en grados

  ctx.save();
  ctx.globalAlpha = alpha || 1;
  ctx.strokeStyle = color || '#dde8e2';
  ctx.lineWidth = 2.5;
  ctx.lineCap = 'round';
  ctx.lineJoin = 'round';

  // Wobble hand-drawn effect: pequeño offset random que cambia cada frame
  var w = function() { return (Math.random() - 0.5) * 1.5; };

  var s = scale || 1;
  var headR = 8 * s;       // radio cabeza
  var bodyLen = 30 * s;     // largo torso
  var armLen = 22 * s;      // largo brazo
  var legLen = 25 * s;      // largo pierna

  var rad = function(deg) { return deg * Math.PI / 180; };

  // Shoulder point (top of body, below head)
  var sx = x + w();
  var sy = y - bodyLen - headR * 2;

  // Head
  ctx.beginPath();
  ctx.arc(sx + w(), sy - headR + w(), headR, 0, Math.PI * 2);
  ctx.stroke();

  // Body
  var bx = sx + Math.sin(rad(pose.bodyLean || 0)) * bodyLen;
  var by = sy + bodyLen;
  ctx.beginPath();
  ctx.moveTo(sx + w(), sy + w());
  ctx.lineTo(bx + w(), by + w());
  ctx.stroke();

  // Hip point
  var hx = bx;
  var hy = by;

  // Arms (from shoulder point)
  var midX = (sx + bx) / 2;
  var midY = (sy + by) / 2;
  // Left arm
  ctx.beginPath();
  ctx.moveTo(midX + w(), midY + w());
  ctx.lineTo(midX + Math.sin(rad(pose.armL || -30)) * armLen + w(),
             midY + Math.cos(rad(pose.armL || -30)) * armLen + w());
  ctx.stroke();
  // Right arm
  ctx.beginPath();
  ctx.moveTo(midX + w(), midY + w());
  ctx.lineTo(midX + Math.sin(rad(pose.armR || 30)) * armLen + w(),
             midY + Math.cos(rad(pose.armR || 30)) * armLen + w());
  ctx.stroke();

  // Legs
  // Left leg
  ctx.beginPath();
  ctx.moveTo(hx + w(), hy + w());
  ctx.lineTo(hx + Math.sin(rad(pose.legL || -15)) * legLen + w(),
             hy + Math.cos(rad(pose.legL || -15)) * legLen + w());
  ctx.stroke();
  // Right leg
  ctx.beginPath();
  ctx.moveTo(hx + w(), hy + w());
  ctx.lineTo(hx + Math.sin(rad(pose.legR || 15)) * legLen + w(),
             hy + Math.cos(rad(pose.legR || 15)) * legLen + w());
  ctx.stroke();

  // Shadow glow for golden stickmen
  if (color === '#f0b000') {
    ctx.shadowBlur = 15;
    ctx.shadowColor = 'rgba(240,176,0,0.5)';
  }

  ctx.restore();
}
```

### Función para dibujar un ciclo de caminar

Interpolar ángulos de piernas/brazos en un ciclo sinusoidal basado en `time`:
```js
function walkPose(time) {
  var cycle = Math.sin(time * 5);
  return {
    armL: -30 + cycle * 25,
    armR: 30 - cycle * 25,
    legL: -10 + cycle * 20,
    legR: 10 - cycle * 20,
    bodyLean: 3
  };
}
```

### Los 7 recuerdos como escenas

Cada escena es un objeto que define qué dibujar y cómo:

```js
var SCENES = [
  {
    // ESCENA 1: Fútbol en La Merced (niños)
    name: 'futbol-ninos',
    draw: function(ctx, W, H, t) {
      // Piso
      ctx.strokeStyle = '#f0b000';
      ctx.globalAlpha = 0.3;
      ctx.beginPath();
      ctx.moveTo(W * 0.1, H * 0.75);
      ctx.lineTo(W * 0.9, H * 0.75);
      ctx.stroke();

      // Portería (rectángulo simple a la derecha)
      ctx.strokeRect(W * 0.75, H * 0.55, W * 0.12, H * 0.2);

      // Pelota (círculo pequeño)
      ctx.beginPath();
      ctx.arc(W * 0.52, H * 0.72, 5, 0, Math.PI * 2);
      ctx.stroke();

      // Stickman 1 (Carlos niño) — pateando
      drawStickman(ctx, W * 0.45, H * 0.75, 0.55,
        { armL: -40, armR: 50, legL: -60, legR: 30, bodyLean: 10 },
        '#f0b000', 0.4);

      // Stickman 2 (Javier niño) — corriendo
      drawStickman(ctx, W * 0.6, H * 0.75, 0.55,
        walkPose(t), '#f0b000', 0.4);
    }
  },
  {
    // ESCENA 2: Fútbol en La Merced (adultos)
    name: 'futbol-adultos',
    draw: function(ctx, W, H, t) {
      // Mismo setup pero stickmen con scale 1.0
      // Portería, piso, pelota iguales
      // Stickmen con scale: 1.0 en vez de 0.55
      // Mismas poses. El usuario ve que CRECIERON.
      // Piso
      ctx.strokeStyle = '#f0b000';
      ctx.globalAlpha = 0.3;
      ctx.beginPath();
      ctx.moveTo(W * 0.1, H * 0.85);
      ctx.lineTo(W * 0.9, H * 0.85);
      ctx.stroke();

      // Portería
      ctx.strokeRect(W * 0.75, H * 0.55, W * 0.12, H * 0.3);

      // Pelota
      ctx.beginPath();
      ctx.arc(W * 0.50, H * 0.82, 6, 0, Math.PI * 2);
      ctx.stroke();

      // Stickman 1 (Carlos adulto)
      drawStickman(ctx, W * 0.43, H * 0.85, 1.0,
        { armL: -40, armR: 50, legL: -60, legR: 30, bodyLean: 10 },
        '#f0b000', 0.4);

      // Stickman 2 (Javier adulto)
      drawStickman(ctx, W * 0.6, H * 0.85, 1.0,
        walkPose(t), '#f0b000', 0.4);
    }
  },
  {
    // ESCENA 3: Pasillo de la UCAB
    name: 'ucab-pasillo',
    draw: function(ctx, W, H, t) {
      // Piso
      ctx.strokeStyle = '#f0b000';
      ctx.globalAlpha = 0.3;
      ctx.beginPath();
      ctx.moveTo(W * 0.1, H * 0.80);
      ctx.lineTo(W * 0.9, H * 0.80);
      ctx.stroke();

      // Pared (línea horizontal más arriba)
      ctx.beginPath();
      ctx.moveTo(W * 0.1, H * 0.45);
      ctx.lineTo(W * 0.9, H * 0.45);
      ctx.stroke();

      // Carlos sentado (piernas estiradas)
      // Cuerpo reclinado, piernas hacia adelante
      drawStickman(ctx, W * 0.4, H * 0.70, 0.9,
        { armL: -10, armR: 10, legL: 50, legR: 55, bodyLean: -15 },
        '#f0b000', 0.4);

      // Javier sentado al lado
      drawStickman(ctx, W * 0.55, H * 0.70, 0.9,
        { armL: -10, armR: 15, legL: 50, legR: 55, bodyLean: -15 },
        '#f0b000', 0.4);
    }
  },
  {
    // ESCENA 4: Hércules en teatro
    name: 'hercules-teatro',
    draw: function(ctx, W, H, t) {
      ctx.strokeStyle = '#f0b000';
      ctx.globalAlpha = 0.3;

      // Tarima (rectángulo)
      ctx.strokeRect(W * 0.15, H * 0.65, W * 0.7, H * 0.05);

      // Cortinas (rectángulos a los lados)
      ctx.fillStyle = 'rgba(240,176,0,0.08)';
      ctx.fillRect(W * 0.08, H * 0.2, W * 0.08, H * 0.5);
      ctx.fillRect(W * 0.84, H * 0.2, W * 0.08, H * 0.5);

      // 4 stickmen en escena:
      // Pena (Javier) — pequeño, encorvado
      drawStickman(ctx, W * 0.30, H * 0.63, 0.6,
        { armL: -5, armR: 5, legL: -8, legR: 8, bodyLean: -10 },
        '#f0b000', 0.4);

      // Phil (Carlos) — bajito
      drawStickman(ctx, W * 0.42, H * 0.63, 0.65,
        { armL: -30, armR: 30, legL: -10, legR: 10, bodyLean: 0 },
        '#f0b000', 0.4);

      // Hércules (Arthur) — más grande, pose heroica
      drawStickman(ctx, W * 0.55, H * 0.63, 0.9,
        { armL: -60, armR: -60, legL: -15, legR: 15, bodyLean: 0 },
        '#f0b000', 0.4);

      // Hades (Anthonny) — brazos arriba tipo llamas
      drawStickman(ctx, W * 0.68, H * 0.63, 0.75,
        { armL: -80, armR: -80, legL: -12, legR: 12, bodyLean: 0 },
        '#f0b000', 0.4);
    }
  },
  {
    // ESCENA 5: FIFA en casa de Anthonny
    name: 'fifa-anthonny',
    draw: function(ctx, W, H, t) {
      ctx.strokeStyle = '#f0b000';
      ctx.globalAlpha = 0.3;

      // TV (rectángulo arriba)
      ctx.strokeRect(W * 0.35, H * 0.3, W * 0.3, H * 0.18);

      // Soporte TV (línea)
      ctx.beginPath();
      ctx.moveTo(W * 0.5, H * 0.48);
      ctx.lineTo(W * 0.5, H * 0.52);
      ctx.stroke();

      // Sofá (rectángulo bajo)
      ctx.strokeRect(W * 0.2, H * 0.68, W * 0.6, H * 0.06);
      // Respaldo
      ctx.strokeRect(W * 0.2, H * 0.60, W * 0.6, H * 0.08);

      // 3-4 stickmen sentados en el sofá
      var seats = [W * 0.30, W * 0.43, W * 0.56, W * 0.69];
      seats.forEach(function(sx) {
        drawStickman(ctx, sx, H * 0.66, 0.7,
          { armL: 30, armR: 30, legL: 30, legR: 35, bodyLean: -10 },
          '#f0b000', 0.4);
      });
    }
  },
  {
    // ESCENA 6: Optra camino a la UCAB
    name: 'optra-ucab',
    draw: function(ctx, W, H, t) {
      ctx.strokeStyle = '#f0b000';
      ctx.globalAlpha = 0.3;

      // Carrocería del carro (vista lateral simplificada)
      // Base
      ctx.beginPath();
      ctx.moveTo(W * 0.25, H * 0.6);
      ctx.lineTo(W * 0.75, H * 0.6);
      ctx.lineTo(W * 0.73, H * 0.7);
      ctx.lineTo(W * 0.27, H * 0.7);
      ctx.closePath();
      ctx.stroke();

      // Techo
      ctx.beginPath();
      ctx.moveTo(W * 0.35, H * 0.6);
      ctx.lineTo(W * 0.40, H * 0.47);
      ctx.lineTo(W * 0.63, H * 0.47);
      ctx.lineTo(W * 0.67, H * 0.6);
      ctx.stroke();

      // Ventanas (línea divisoria)
      ctx.beginPath();
      ctx.moveTo(W * 0.52, H * 0.47);
      ctx.lineTo(W * 0.51, H * 0.6);
      ctx.stroke();

      // Ruedas
      ctx.beginPath();
      ctx.arc(W * 0.35, H * 0.72, 8, 0, Math.PI * 2);
      ctx.stroke();
      ctx.beginPath();
      ctx.arc(W * 0.65, H * 0.72, 8, 0, Math.PI * 2);
      ctx.stroke();

      // Carlos (volante, izquierda) — cabeza simple dentro del carro
      ctx.beginPath();
      ctx.arc(W * 0.44, H * 0.53, 5, 0, Math.PI * 2);
      ctx.stroke();

      // Javier (copiloto, derecha)
      ctx.beginPath();
      ctx.arc(W * 0.58, H * 0.53, 5, 0, Math.PI * 2);
      ctx.stroke();

      // Notas musicales flotando arriba
      ctx.font = (14 * (W / 900)) + 'px serif';
      ctx.fillStyle = 'rgba(240,176,0,0.35)';
      ctx.fillText('♪', W * 0.48 + Math.sin(t * 2) * 5, H * 0.40 + Math.cos(t * 3) * 3);
      ctx.fillText('♫', W * 0.55 + Math.cos(t * 2) * 5, H * 0.38 + Math.sin(t * 2.5) * 3);
    }
  },
  {
    // ESCENA 7: Fortnite split screen
    name: 'fortnite-split',
    draw: function(ctx, W, H, t) {
      ctx.strokeStyle = '#f0b000';
      ctx.globalAlpha = 0.3;

      // Línea divisoria (split screen)
      ctx.lineWidth = 1;
      ctx.setLineDash([5, 5]);
      ctx.beginPath();
      ctx.moveTo(W * 0.5, H * 0.2);
      ctx.lineTo(W * 0.5, H * 0.85);
      ctx.stroke();
      ctx.setLineDash([]);
      ctx.lineWidth = 2.5;

      // LADO IZQUIERDO — Carlos
      // Monitor
      ctx.strokeRect(W * 0.15, H * 0.35, W * 0.15, H * 0.12);
      // Mesa
      ctx.beginPath();
      ctx.moveTo(W * 0.10, H * 0.55);
      ctx.lineTo(W * 0.35, H * 0.55);
      ctx.stroke();
      // Carlos sentado
      drawStickman(ctx, W * 0.23, H * 0.72, 0.75,
        { armL: 30, armR: 20, legL: 30, legR: 35, bodyLean: -15 },
        '#f0b000', 0.4);

      // LADO DERECHO — Javier
      // Monitor
      ctx.strokeRect(W * 0.6, H * 0.35, W * 0.15, H * 0.12);
      // Mesa
      ctx.beginPath();
      ctx.moveTo(W * 0.55, H * 0.55);
      ctx.lineTo(W * 0.80, H * 0.55);
      ctx.stroke();
      // Javier sentado
      drawStickman(ctx, W * 0.68, H * 0.72, 0.75,
        { armL: 30, armR: 20, legL: 30, legR: 35, bodyLean: -15 },
        '#f0b000', 0.4);
    }
  }
];
```

### Timeline de la animación

La animación está controlada por `audio.currentTime`. El loop de `requestAnimationFrame` verifica el tiempo actual del audio y decide qué dibujar.

**IMPORTANTE:** Los timestamps exactos dependerán del corte de audio que el usuario haga. Los tiempos de abajo son RELATIVOS al inicio del clip de audio (0 = inicio del MP3 cortado). El usuario debe ajustar estos valores según su corte específico.

**Timestamps estimados (asumiendo corte desde ~3:15 del original, ~4 min total de audio):**

```js
var TIMELINE = {
  // ACTO 1: La partida (0s - 45s)
  ACT1_START: 0,         // Dos stickmen juntos
  ACT1_WALK: 5,          // Javier empieza a caminar
  ACT1_GONE: 35,         // Javier desaparece del canvas
  ACT1_HEAD: 38,         // Carlos baja la cabeza

  // ACTO 2: Recuerdos lentos (45s - 90s)
  MEM1_IN: 45,           // Fútbol niños — fade in
  MEM1_OUT: 53,          // Fútbol niños — fade out
  MEM2_IN: 56,           // Fútbol adultos — fade in
  MEM2_OUT: 64,          // Fútbol adultos — fade out
  MEM3_IN: 67,           // Pasillo UCAB — fade in
  MEM3_OUT: 75,          // Pasillo UCAB — fade out

  // ACTO 3: Recuerdos rápidos (90s - 130s)
  MEM4_IN: 80,           // Hércules — 4s
  MEM4_OUT: 84,
  MEM5_IN: 86,           // FIFA — 3.5s
  MEM5_OUT: 89.5,
  MEM6_IN: 91,           // Optra — 3s
  MEM6_OUT: 94,
  MEM7_IN: 95.5,         // Fortnite — 2.5s
  MEM7_OUT: 98,

  // ACTO 3b: Residuos convergiendo (98s - 115s)
  CONVERGE_START: 98,    // Todos los residuos se mueven al centro
  CONVERGE_PEAK: 112,    // Brillo máximo

  // ACTO 4: EL SILENCIO (el usuario DEBE anotar el segundo exacto del silencio en su corte)
  SILENCE: 115,          // AJUSTAR al segundo exacto del silencio en el MP3
  SILENCE_END: 120,      // ~5 segundos de negro total

  // ACTO 5: La coda (empezar cuando entran las cuerdas graves)
  CODA_START: 120,       // AJUSTAR al segundo de las cuerdas graves en el MP3
  CODA_CARLOS: 122,      // Carlos aparece mirando al frente
  CODA_TEXT1: 130,       // "Todo eso fuiste."
  CODA_TEXT2: 136,       // "Todo eso eres."
  CODA_TEXT3: 142,       // "Y por eso estoy aquí."
  END: 160               // Se queda estático para siempre
};
```

### El loop principal de animación

```js
var animId = null;
var startTime = 0;
var residues = []; // Array de {x, y, sceneIndex, alpha} para los residuos dorados

function animate() {
  animId = requestAnimationFrame(animate);

  var currentTime = audio ? audio.currentTime : 0;
  var t = currentTime; // shorthand

  // Clear canvas
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Fondo oscuro base
  ctx.fillStyle = 'rgba(1,6,8,0.95)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  var W = canvas.width / dpr;
  var H = canvas.height / dpr;
  ctx.save();
  ctx.scale(dpr, dpr);

  var T = TIMELINE;

  // ═══ ACTO 1: La partida ═══
  if (t >= T.ACT1_START && t < T.ACT1_HEAD + 7) {
    // Carlos — siempre al centro-izquierda, quieto
    var carlosX = W * 0.45;
    var carlosY = H * 0.75;
    var idle = { armL: -20, armR: 20, legL: -8, legR: 8, bodyLean: 0 };

    // Si Carlos baja la cabeza
    if (t >= T.ACT1_HEAD) {
      idle.headTilt = 20;
      idle.bodyLean = -8;
    }

    drawStickman(ctx, carlosX, carlosY, 1.0, idle, '#dde8e2', 1);

    // Javier camina hacia la derecha
    if (t < T.ACT1_GONE) {
      var progress = Math.max(0, (t - T.ACT1_WALK) / (T.ACT1_GONE - T.ACT1_WALK));
      var javierX = W * 0.55 + progress * W * 0.5;
      var javierScale = 1.0 - progress * 0.4; // Se hace más pequeño
      var javierAlpha = 1.0 - progress * 0.8;

      // Estela dorada que deja atrás
      if (progress > 0 && progress < 1) {
        ctx.save();
        var grad = ctx.createLinearGradient(W * 0.55, 0, javierX, 0);
        grad.addColorStop(0, 'rgba(240,176,0,0)');
        grad.addColorStop(1, 'rgba(240,176,0,' + (0.15 * (1 - progress)) + ')');
        ctx.strokeStyle = grad;
        ctx.lineWidth = 1;
        ctx.beginPath();
        ctx.moveTo(W * 0.55, carlosY);
        ctx.lineTo(javierX, carlosY);
        ctx.stroke();
        ctx.restore();
      }

      drawStickman(ctx, javierX, carlosY, javierScale,
        walkPose(t), '#dde8e2', javierAlpha);
    }
  }

  // ═══ ACTO 2 y 3: Flashbacks de escenas completas ═══
  // Recorrer las escenas y comprobar si t está en su rango
  var sceneTimings = [
    { scene: 0, inTime: T.MEM1_IN, outTime: T.MEM1_OUT },
    { scene: 1, inTime: T.MEM2_IN, outTime: T.MEM2_OUT },
    { scene: 2, inTime: T.MEM3_IN, outTime: T.MEM3_OUT },
    { scene: 3, inTime: T.MEM4_IN, outTime: T.MEM4_OUT },
    { scene: 4, inTime: T.MEM5_IN, outTime: T.MEM5_OUT },
    { scene: 5, inTime: T.MEM6_IN, outTime: T.MEM6_OUT },
    { scene: 6, inTime: T.MEM7_IN, outTime: T.MEM7_OUT }
  ];

  sceneTimings.forEach(function(st) {
    if (t >= st.inTime && t < st.outTime) {
      // Fade in/out
      var fadeIn = Math.min(1, (t - st.inTime) / 0.5);
      var fadeOut = Math.min(1, (st.outTime - t) / 0.5);
      var alpha = Math.min(fadeIn, fadeOut);

      ctx.save();
      ctx.globalAlpha = alpha;
      SCENES[st.scene].draw(ctx, W, H, t);
      ctx.restore();

      // Al salir, agregar residuo dorado
      if (t >= st.outTime - 0.1 && !st.residueAdded) {
        st.residueAdded = true;
        residues.push({ sceneIndex: st.scene, alpha: 0.12, x: 0, y: 0 });
      }
    }
  });

  // ═══ ACTO 3b: Residuos convergiendo ═══
  if (t >= T.CONVERGE_START && t < T.SILENCE) {
    var convProgress = (t - T.CONVERGE_START) / (T.SILENCE - T.CONVERGE_START);
    residues.forEach(function(r, i) {
      ctx.save();
      // Los residuos se mueven hacia el centro y brillan más
      r.alpha = 0.12 + convProgress * 0.25;
      ctx.globalAlpha = r.alpha;
      // Glow creciente
      ctx.shadowBlur = convProgress * 30;
      ctx.shadowColor = 'rgba(240,176,0,0.6)';
      SCENES[r.sceneIndex].draw(ctx, W, H, t);
      ctx.restore();
    });

    // Overlay dorado que crece con el crescendo
    ctx.save();
    ctx.fillStyle = 'rgba(240,176,0,' + (convProgress * 0.15) + ')';
    ctx.fillRect(0, 0, W, H);
    ctx.restore();
  }

  // ═══ ACTO 4: EL SILENCIO — negro total ═══
  if (t >= T.SILENCE && t < T.SILENCE_END) {
    // Canvas ya está limpio. No dibujar NADA.
    // El clearRect del inicio + el fondo oscuro = negro.
    // Nada. Silencio visual absoluto.
  }

  // ═══ ACTO 5: LA CODA — Carlos mirando al frente ═══
  if (t >= T.CODA_START) {
    // Carlos aparece con fade-in
    var codaFade = Math.min(1, (t - T.CODA_CARLOS) / 2);

    if (t >= T.CODA_CARLOS) {
      // Carlos DORADO, mirando AL FRENTE (al usuario)
      // "Mirando al frente" = pose simétrica, de frente
      drawStickman(ctx, W * 0.5, H * 0.65, 1.0,
        { armL: -15, armR: 15, legL: -10, legR: 10, bodyLean: 0 },
        '#f0b000', codaFade);
    }

    // Textos
    var textEl = document.getElementById('adagio-text');
    if (textEl) {
      if (t >= T.CODA_TEXT3) {
        textEl.innerHTML = 'Todo eso fuiste. Todo eso eres.<br>Y por eso estoy aquí.';
        textEl.classList.add('visible');
      } else if (t >= T.CODA_TEXT2) {
        textEl.textContent = 'Todo eso fuiste. Todo eso eres.';
        textEl.classList.add('visible');
      } else if (t >= T.CODA_TEXT1) {
        textEl.textContent = 'Todo eso fuiste.';
        textEl.classList.add('visible');
      }
    }
  }

  ctx.restore();

  // Detener animación al final del audio
  if (audio && audio.ended && t >= T.END) {
    // Mantener el último frame — no cancelar animación
    // Solo dejar de actualizar
  }
}
```

### Inicialización y Play Button

```js
var canvas = document.getElementById('adagio-cvs');
var ctx = canvas.getContext('2d');
var audio = document.getElementById('adagio-audio');
var playBtn = document.getElementById('adagio-play');
var dpr = window.devicePixelRatio || 1;

function resizeCanvas() {
  var rect = canvas.parentElement.getBoundingClientRect();
  canvas.width = rect.width * dpr;
  canvas.height = rect.height * dpr;
}
resizeCanvas();
window.addEventListener('resize', resizeCanvas);

if (playBtn) {
  playBtn.addEventListener('click', function() {
    playBtn.style.display = 'none';
    resizeCanvas();

    // Audio fade-in
    if (audio) {
      audio.currentTime = 0;
      audio.volume = 0;
      audio.play().then(function() {
        var fi = setInterval(function() {
          if (audio.volume < 0.95) { audio.volume = Math.min(1, audio.volume + 0.03); }
          else { audio.volume = 1; clearInterval(fi); }
        }, 80);
      }).catch(function() {});
    }

    animate();
  });
}
```

### Integración con el sistema de color
Agregar al objeto `SECTION_COLORS`:
```js
adagio: [100, 50, 160]    // púrpura profundo — el flashback
```

Agregar al array `sectionMap` (entre strange y kintsugi):
```js
{ sel: '#adagio-section', key: 'adagio' }
```

### Responsive
- En mobile, el aspect-ratio del canvas puede cambiar a `4/3` para que sea más alto
- Los stickmen escalan automáticamente porque usan proporciones de `W` y `H`
- El texto de la coda debe ser legible en pantallas pequeñas: `clamp(0.95rem, 3vw, 1.8rem)`

### Consideraciones importantes
1. **Los timestamps del TIMELINE deben ser ajustados** por el usuario según el corte exacto de audio que haga. Lo más importante es anotar en qué segundo del MP3 cortado ocurre: (a) el silencio, y (b) la entrada de las cuerdas graves de la coda.
2. **prefers-reduced-motion**: Si el usuario tiene esta preferencia, mostrar una versión estática con Carlos y la frase final, sin animación.
3. **El stickman "mirando al frente"** en la coda es simplemente un stickman con pose simétrica — los brazos ligeramente abiertos, piernas paralelas. La diferencia es que durante toda la animación los stickmen son de PERFIL (caminan horizontalmente), y en la coda Carlos aparece de FRENTE. Ese cambio de perspectiva es lo que genera el impacto.
4. **Pausa de otros audios**: Cuando la sección de Adagio empieza, pausar cualquier otro audio que esté sonando (vinyl, tesseract). Mismo patrón que ya usa el Tesseract para pausar el vinyl.

---

## RESUMEN DE CAMBIOS EN EL ARCHIVO

### En el CSS (dentro de `<style>`):
1. Agregar estilos de `.whiplash-section` y componentes
2. Agregar estilos de `.adagio-section` y componentes
3. Agregar responsive en los media queries existentes

### En el HTML (dentro de `<body>`):
1. Agregar la sección Whiplash DESPUÉS de `<!-- ══════════════ VINILO CANSERBERO ══════════════ -->` y ANTES de `<!-- ══════════════ DOCTOR STRANGE — EL HECHIZO ══════════════ -->`
2. Agregar la sección Adagio DESPUÉS de `<!-- ══════════════ DOCTOR STRANGE — EL HECHIZO ══════════════ -->` (después de su `</div>` de cierre) y ANTES de `<!-- ══════════════ KINTSUGI ══════════════ -->`

### En el JS (dentro de `<script>`):
1. Agregar `whiplash: [180, 40, 40]` y `adagio: [100, 50, 160]` al objeto `SECTION_COLORS`
2. Agregar `{ sel: '#whiplash-section', key: 'whiplash' }` al array `sectionMap` (entre vinyl y strange)
3. Agregar `{ sel: '#adagio-section', key: 'adagio' }` al array `sectionMap` (entre strange y kintsugi)
4. Agregar el bloque JS de Whiplash (IIFE con audio + frases)
5. Agregar el bloque JS de Adagio (IIFE con canvas animation + audio sync + timeline)

### Archivos necesarios del usuario:
- `public/audio/Whiplash.mp3` — Clip del solo de Caravan
- `public/audio/Adagio.mp3` — Corte de Adagio for Strings (~3:15 a ~7:30 del video de Dudamel)
