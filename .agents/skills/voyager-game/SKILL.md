---
name: voyager-game
description: >
  Master skill for the Voyager Constellation Game. A browser-based puzzle game
  where players connect stars to form constellations. This skill coordinates five
  specialized roles: game-designer, frontend-dev, game-logic, vfx-artist, and
  qa-tester. / Habilidad maestra para el Juego de Constelaciones Voyager. Un
  juego de rompecabezas en navegador donde los jugadores conectan estrellas para
  formar constelaciones. Esta habilidad coordina cinco roles especializados.
version: "1.0.0"
roles:
  - game-designer
  - frontend-dev
  - game-logic
  - vfx-artist
  - qa-tester
stack:
  - HTML5 Canvas
  - Vanilla JavaScript (ES2022)
  - CSS3 / CSS Variables
  - JSON (constellation data)
workspace: c:\Users\angel\OneDrive\Escritorio\ANTY\voyager
---

# Voyager — Constellation Game · Master Skill

> **Language / Idioma:** Instructions are written in both English and Spanish.
> Code, variable names, and file names are always in English.

---

## Project Overview / Descripción del Proyecto

**Voyager** is a browser-based puzzle game in which players study a star field
and draw lines between stars to recreate ancient constellations. The game
features:

- 10+ real astronomical constellations with accurate relative star positions
- 5 difficulty tiers (1 = Beginner → 5 = Expert)
- Particle effects, animated star-glow, and a procedurally generated starfield
- Scoring based on accuracy, speed, and connection order

**Voyager** es un juego de rompecabezas en navegador en el que los jugadores
estudian un campo estelar y dibujan líneas entre las estrellas para recrear
constelaciones antiguas. El juego incluye:

- 10+ constelaciones astronómicas reales con posiciones estelares precisas
- 5 niveles de dificultad (1 = Principiante → 5 = Experto)
- Efectos de partículas, brillo animado de estrellas, y un campo estelar generado proceduralmente
- Puntuación basada en precisión, velocidad y orden de conexión

---

## File Structure / Estructura de Archivos

```
voyager/
├── index.html                  # Entry point
├── README.md                   # Project documentation
├── src/
│   ├── main.js                 # Bootstrap & game loop
│   ├── GameState.js            # Central state machine
│   ├── Constellation.js        # Data model + validation
│   ├── Renderer.js             # Canvas rendering
│   ├── UI.js                   # HUD, menus, modals
│   ├── GameLogic.js            # Connection mechanics & scoring
│   └── VFX.js                  # Particles, animations, starfield
├── data/
│   └── constellations.json     # Constellation definitions
├── styles/
│   └── main.css                # Styling & CSS variables
├── assets/
│   └── audio/                  # Optional sound effects
└── .agents/
    └── skills/
        └── voyager-game/
            ├── SKILL.md        # This file
            └── constellation-data.json
```

---

## Shared Interfaces / Interfaces Compartidas

All agents must respect these data contracts. / Todos los agentes deben
respetar estos contratos de datos.

### Star Object
```json
{
  "id": 0,
  "name": "Betelgeuse",
  "x": 42.5,
  "y": 38.1,
  "magnitude": 0.5
}
```
- `x`, `y` — position in [0, 100] coordinate space (percentage of canvas)
- `magnitude` — apparent magnitude (lower = brighter); range typically 0–6

### Constellation Object
```json
{
  "id": "orion",
  "name": "Orion",
  "difficulty": 2,
  "lore": "The Hunter — visible from both hemispheres.",
  "stars": [ /* Star[] */ ],
  "connections": [[0,1],[1,2]]
}
```
- `connections` — ordered list of `[starIndexA, starIndexB]` pairs

### GameState Shape
```js
{
  phase: "menu" | "playing" | "paused" | "complete" | "gameover",
  constellation: Constellation | null,
  drawnConnections: Array<[number, number]>,
  score: number,
  timeElapsed: number,
  hintsUsed: number,
  difficulty: 1 | 2 | 3 | 4 | 5
}
```

---

## Role Definitions / Definición de Roles

---

### 🎨 Role: `game-designer`

**Owns / Propietario de:**
- `data/constellations.json`
- Game rules document (inline below)
- Difficulty scaling table

**Mission / Misión:**
Design and maintain the constellation dataset and core game rules. Ensure
constellation data is astronomically accurate (relative positions), balanced
across difficulty tiers, and correctly structured so other agents can consume
it without modification.

Diseñar y mantener el conjunto de datos de constelaciones y las reglas
principales del juego. Asegurar que los datos sean astronómicamente precisos,
estén balanceados entre niveles de dificultad, y estructurados correctamente.

**Rules / Reglas del Juego:**

| Rule | Detail |
|------|--------|
| Connection tolerance | A line is "correct" if both endpoints are within 8px of target stars |
| Order independence | Connections may be drawn in any order unless difficulty ≥ 4 |
| Hint system | Each hint reveals one correct connection; costs 200 pts |
| Time limit | Scales with difficulty: ∞ / 180s / 120s / 90s / 60s |
| Score formula | `base × (1 + speed_bonus) − (hints × 200)` |
| Base score | `stars_count × connections_count × 100 × accuracy_pct` |

**Difficulty Scaling Table:**

| Level | Stars Range | Connections | Time Limit | Order Required | Helper Lines |
|-------|-------------|-------------|------------|----------------|--------------|
| 1 | 3–5 | 2–4 | None | No | Yes (faint) |
| 2 | 5–8 | 4–7 | 180s | No | No |
| 3 | 7–12 | 6–11 | 120s | No | No |
| 4 | 10–16 | 9–15 | 90s | Yes | No |
| 5 | 14–20 | 13–19 | 60s | Yes | No |

**Deliverables / Entregables:**
- `data/constellations.json` — 10+ constellations, all fields populated
- Balance review after QA testing

---

### 💻 Role: `frontend-dev`

**Owns / Propietario de:**
- `index.html`
- `styles/main.css`
- `src/Renderer.js`
- `src/UI.js`
- `src/main.js` (bootstrap only)

**Mission / Misión:**
Build the visual shell of the game: the HTML5 Canvas renderer, CSS theme, HUD
overlays, menus, and responsive layout. The canvas must handle both mouse and
touch input and pass raw events to GameLogic.

Construir la capa visual del juego: el renderizador Canvas HTML5, el tema CSS,
los overlays del HUD, menús y diseño responsivo.

**Canvas Layers (back to front):**
1. **Starfield layer** (static offscreen canvas, re-drawn only on resize)
2. **Constellation layer** (target stars, confirmed connections)
3. **Interaction layer** (hover highlights, in-progress line from cursor)
4. **UI/HUD layer** (DOM elements overlaid with CSS)

**Rendering Contracts:**
- `Renderer.drawStar(ctx, star, state)` — draws a star; `state ∈ {idle, hover, selected, correct, wrong}`
- `Renderer.drawConnection(ctx, a, b, state)` — draws a line; `state ∈ {pending, correct, wrong}`
- `Renderer.clear()` — clears all canvas layers
- `UI.showModal(type, data)` — renders a modal (`type ∈ {levelComplete, gameOver, hint}`)
- `UI.updateHUD(gameState)` — refreshes score, timer, connections counter

**CSS Variables (define in `:root`):**
```css
--color-bg: #04050f;
--color-star-idle: #c8d8ff;
--color-star-hover: #ffffff;
--color-star-selected: #ffd700;
--color-connection-correct: #7ec8e3;
--color-connection-pending: rgba(255,255,255,0.4);
--color-connection-wrong: #ff6b6b;
--font-ui: 'Orbitron', monospace;
```

**Responsive Rules:**
- Canvas fills viewport; stars use % coordinates scaled to canvas px
- Touch targets: stars have a minimum 44px hitbox radius

**Deliverables / Entregables:**
- `index.html` with two `<canvas>` elements (starfield + game)
- `styles/main.css` with full CSS variable theme
- `src/Renderer.js` class with all draw methods
- `src/UI.js` class with HUD and modal methods

---

### ⚙️ Role: `game-logic`

**Owns / Propietario de:**
- `src/GameState.js`
- `src/GameLogic.js`
- `src/Constellation.js`

**Mission / Misión:**
Implement the state machine, connection validation, scoring engine, and hint
system. No DOM or canvas code belongs here — only pure logic that takes input
events and returns new state.

Implementar la máquina de estados, validación de conexiones, motor de puntuación
y sistema de pistas. Sin código DOM o canvas — solo lógica pura.

**State Machine Transitions:**
```
menu ──[start]──► playing ──[pause]──► paused ──[resume]──► playing
                     │                                          │
                  [timeout]                               [all correct]
                     │                                          │
                  gameover                                  complete
```

**Key Methods / Métodos Clave:**

```js
// GameLogic.js
class GameLogic {
  // Returns { valid: bool, duplicate: bool, connectionIdx: number|null }
  validateConnection(starA, starB, constellation) {}

  // Returns updated score number
  calculateScore(state) {}

  // Returns [starIndexA, starIndexB] of a random uncompleted connection
  getHint(state) {}

  // Returns true when all required connections are drawn
  isConstellationComplete(state) {}

  // Called every frame tick; returns updated state (timer, phase changes)
  tick(state, deltaMs) {}
}
```

**Scoring Details:**
- Base: `stars × connections × 100 × (correctConnections / totalConnections)`
- Speed bonus: `max(0, (timeLimit − elapsed) / timeLimit)` → adds up to 50%
- Hint penalty: `−200` per hint used
- Wrong connection penalty: `−50` per wrong attempt

**Deliverables / Entregables:**
- `src/GameState.js` — state factory + transition helpers
- `src/GameLogic.js` — full logic class (no side effects)
- `src/Constellation.js` — data loader + model

---

### ✨ Role: `vfx-artist`

**Owns / Propietario de:**
- `src/VFX.js`
- Starfield generation (called by Renderer on init/resize)

**Mission / Misión:**
Create all visual effects that make the game feel alive: the procedural starfield
background, star-glow pulsing, particle bursts on correct connections, and the
constellation "reveal" animation when a level is completed.

Crear todos los efectos visuales que hacen que el juego se sienta vivo.

**VFX Catalog:**

| Effect | Trigger | Description |
|--------|---------|-------------|
| `starfield` | Game init / resize | 300–500 randomly placed background stars, parallax on mouse move |
| `starPulse` | Star hover | Soft radial glow that pulses at 1.2 Hz |
| `connectionTrail` | Drawing line | Faint glowing trail follows cursor |
| `connectionLock` | Correct connection | Gold spark burst along the new line segment |
| `connectionWrong` | Wrong connection | Red ripple flash on both endpoints |
| `constellationReveal` | Level complete | Sequential glow sweep across all stars, then particle shower |
| `scorePopup` | Score change | Floating "+NNN" text rising from connection midpoint |

**VFX API:**
```js
class VFX {
  constructor(canvas, ctx) {}

  generateStarfield(width, height) {}      // → returns offscreen canvas
  update(deltaMs) {}                        // advance all active effects
  render(ctx) {}                            // draw all active effects
  trigger(effectName, payload) {}           // fire a named effect

  // Convenience triggers
  onStarHover(star, canvasX, canvasY) {}
  onConnectionCorrect(starA, starB) {}
  onConnectionWrong(starA, starB) {}
  onConstellationComplete(constellation) {}
}
```

**Performance Constraints:**
- Object pool for particles (max 500 active particles)
- Use `requestAnimationFrame`; never `setInterval`
- Target 60 fps on mid-range hardware (avoid per-frame allocation)

**Deliverables / Entregables:**
- `src/VFX.js` — full VFX class with all effects implemented
- Starfield offscreen canvas generation function

---

### 🧪 Role: `qa-tester`

**Owns / Propietario de:**
- `tests/` directory
- Bug reports posted to the board

**Mission / Misión:**
Validate the complete game flow by writing and running test cases. Report all
bugs with reproduction steps to the board, addressed to the responsible agent.
Sign off on a release only when all P0 and P1 bugs are resolved.

Validar el flujo completo del juego escribiendo y ejecutando casos de prueba.

**Test Suites:**

#### Suite 1 — Data Integrity
- All 10 constellations load from JSON without error
- Each constellation has ≥ 3 stars and ≥ 2 connections
- No duplicate star names within a constellation
- All connection indices are valid star indices
- Difficulty values are integers in [1, 5]

#### Suite 2 — Game Logic
- `validateConnection` accepts correct pairs and rejects incorrect ones
- `validateConnection` rejects duplicate connections
- `isConstellationComplete` returns `true` only when all connections drawn
- `calculateScore` returns 0 when no connections drawn
- `calculateScore` returns positive when constellation complete with no hints
- `tick` transitions phase to `gameover` when time expires
- Hint does not reveal an already-drawn connection

#### Suite 3 — Rendering
- Canvas resizes without distorting star positions
- Stars render at correct x/y after coordinate transform
- Hover state changes star visual within one frame
- Completed connections persist after re-render

#### Suite 4 — VFX
- Particle count never exceeds 500
- Starfield is regenerated on window resize
- `constellationReveal` completes within 2 seconds

#### Suite 5 — End-to-End Flows
- **Happy path:** Load page → select constellation → draw all connections → see complete screen
- **Timeout path:** Let timer expire → see game-over screen → retry → game resets
- **Hint path:** Use hint → connection reveals → score decreases by 200
- **Wrong connection:** Draw incorrect line → red flash → score decreases by 50

**Bug Severity:**

| Priority | Definition |
|----------|-----------|
| P0 | Game unplayable / crash |
| P1 | Core mechanic broken |
| P2 | Visual glitch / minor score error |
| P3 | Polish / UX improvement |

**Deliverables / Entregables:**
- `tests/data-integrity.test.js`
- `tests/game-logic.test.js`
- `tests/e2e-flow.md` (manual test script)
- Bug reports on the board addressed to the responsible agent

---

## Coordination Protocol / Protocolo de Coordinación

1. **game-designer** delivers `constellations.json` first — all other agents depend on it.
2. **frontend-dev** and **game-logic** build in parallel once the data contract is set.
3. **vfx-artist** integrates after `Renderer.js` exposes its canvas context.
4. **qa-tester** runs Suite 1 immediately on data delivery; Suites 2–5 after integration.
5. All agents post `[done] @coordinator <summary>` when their deliverable is merged.

## Definition of Done / Definición de Terminado

- [ ] All 5 deliverable sets are merged into `voyager/src/`
- [ ] QA Suites 1–4 pass with zero P0/P1 bugs
- [ ] Game runs in Chrome, Firefox, and Safari (latest)
- [ ] `README.md` is complete and accurate
- [ ] No `console.error` output during normal gameplay
