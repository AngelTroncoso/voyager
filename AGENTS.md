# 🤖 AGENTS.md — Agent Collaboration Guide for Voyager

> This file defines the rules, conventions, and boundaries that ALL agents (human or AI) must follow when contributing to the Voyager project. Read this before making any changes.

---

## 🗂️ Project Layout

```
voyager/
├── index.html              ← Single-file game entry point (HTML + CSS + JS)
├── README.md               ← Project overview (do NOT auto-overwrite)
├── AGENTS.md               ← This file
├── data/
│   └── constellations.json ← Runtime constellation data (served by HTTP)
├── src/                    ← Source modules (if split-file build adopted)
│   ├── main.js
│   ├── GameState.js
│   ├── Constellation.js
│   ├── Renderer.js
│   ├── UI.js
│   ├── GameLogic.js
│   └── VFX.js
├── styles/
│   └── main.css
├── docs/
│   ├── game-design-doc.md  ← Authoritative design spec (read before coding)
│   └── qa-checklist.md     ← QA sign-off checklist
└── .agents/
    └── skills/
        └── voyager-game/
            ├── SKILL.md                ← Master coordination skill
            └── constellation-data.json ← Authoritative star data (source of truth)
```

---

## 📂 File Ownership & Access

| File / Directory | Owner Agent | Others May |
|-----------------|-------------|-----------|
| `index.html` | `frontend-dev` | Read; propose changes via board |
| `src/GameLogic.js` | `game-logic` | Read only |
| `src/VFX.js` | `vfx-artist` | Read only |
| `src/Renderer.js` | `frontend-dev` | Read only |
| `.agents/skills/voyager-game/constellation-data.json` | `game-designer` | Read only — **never edit** |
| `data/constellations.json` | `game-designer` | Sync'd copy — do not hand-edit |
| `docs/game-design-doc.md` | `qa-tester` | Read; propose amendments on board |
| `docs/qa-checklist.md` | `qa-tester` | Read only |
| `README.md` | `architect` | Read only |
| `AGENTS.md` | `qa-tester` | Read only |

> [!CAUTION]
> **Never modify another agent's owned file without explicit board agreement.** Coordinate by posting on the message board, then wait for the owner to accept or merge.

---

## 🏷️ Naming Conventions

### Files

| Type | Convention | Example |
|------|-----------|---------|
| JS modules | PascalCase | `GameState.js`, `VFX.js` |
| CSS files | kebab-case | `main.css`, `dark-theme.css` |
| JSON data | kebab-case | `constellation-data.json` |
| Markdown docs | kebab-case | `game-design-doc.md` |
| Test files | `*.test.js` or `*.spec.js` | `GameLogic.test.js` |
| Scratch / temp | `scratch-<purpose>-<date>` | `scratch-layout-20261117.html` |

### CSS Classes / IDs

- BEM-style: `.block__element--modifier`
- Namespace all game elements with `vgr-` prefix: `.vgr-hud`, `.vgr-star`, `.vgr-btn`
- Do NOT use inline styles for anything other than dynamic canvas values.

### JavaScript

- **Constants:** `SCREAMING_SNAKE_CASE` → `MAX_HINTS`, `BASE_SCORE`
- **Classes:** PascalCase → `GameState`, `Constellation`
- **Functions / variables:** camelCase → `drawConnection()`, `selectedStar`
- **Private class fields:** `#prefixed` → `#particles`, `#timer`
- **Events:** past-tense strings → `'star:selected'`, `'constellation:complete'`

---

## 🔀 Workflow Protocol

### 1. Claim Work Before Starting

Post to the board **before** touching a file:

```
Taking ownership of src/VFX.js — adding particle burst for constellation completion.
```

### 2. Post Interfaces Before Implementations

If your code exposes a public API, post the interface shape to the board **before** writing implementation so other agents can code against it.

```
GameLogic will export:
  connect(starA: number, starB: number): { valid: boolean; already_drawn: boolean }
  getScore(): number
  reset(): void
```

### 3. Respect the Data Contract

The authoritative star data lives in `.agents/skills/voyager-game/constellation-data.json`.  
- **Shape:** `{ id, name, difficulty, lore, stars[{ id, name, x, y, magnitude }], connections[[i,j]] }`
- Coordinate space: `[0, 100]` normalized — `(0,0)` = top-left; `(100,100)` = bottom-right.
- **Never** change IDs or reorder stars in an existing constellation — it breaks saved games.

### 4. Report Completion

When a task is done, post to the board with `[done]` and list what changed:

```
@coordinator [done] VFX particle system complete.
  - Added src/VFX.js (ParticleSystem class, 60fps rAF loop)
  - Exposed VFX.celebrateConstellation(x, y, color)
  - No changes to other files
```

---

## 🚫 What NOT to Do

| ❌ Prohibited | ✅ Alternative |
|-------------|--------------|
| Edit `constellation-data.json` star IDs | Propose a data amendment on the board; wait for game-designer approval |
| Hard-code pixel values in JS | Use canvas `width/height` ratios or CSS variables |
| Add npm / CDN dependencies without approval | Use Vanilla JS; discuss on board; get coordinator sign-off first |
| Delete or overwrite `README.md` | Only append via a board-approved update |
| Commit binary assets (images, audio) >100KB | Optimize first; use SVG or CSS where possible |
| Use `console.log` in production code | Use a `DEBUG` flag: `if (DEBUG) console.log(...)` |
| Write to `localStorage` keys outside the `vgr_` namespace | Namespace all keys: `vgr_highScore`, `vgr_settings` |

---

## 🧪 QA Requirements

Before posting `[done]` on any feature:

1. **Self-test:** Run through the relevant test cases in `docs/qa-checklist.md`.
2. **Console clean:** Zero errors and zero warnings in browser DevTools.
3. **No regressions:** Test the full game flow (Intro → all constellations → Victory).
4. **Performance check:** ≥60 FPS during gameplay on desktop Chrome.

> [!TIP]
> Run a quick performance audit in Chrome DevTools (Lighthouse → Performance) before marking a visual feature done.

---

## 🔢 Versioning

- Bump `version` in `package.json` (or `README.md` header) on each significant change.
- Use Semantic Versioning: `MAJOR.MINOR.PATCH`
  - **PATCH** — bug fix, no behavior change
  - **MINOR** — new feature, backward-compatible
  - **MAJOR** — breaking change to data format or public API

---

## 💬 Communication Norms

- **Address by @name** when requesting action from a specific agent.
- **@all** only for decisions that affect everyone (e.g., interface changes, data format updates).
- **Keep posts short** — results and blockers, not status.
- **Link to files by path** when referencing specific code.
- **Never start a post with** "Hello", "Hi", "Ready", or "Acknowledged".

---

## 🛡️ Definition of Done (DoD)

A feature is "done" when:

- [ ] Code is in the correct file per ownership table above.
- [ ] All P0 and P1 test cases related to the feature pass in `qa-checklist.md`.
- [ ] No new console errors or warnings introduced.
- [ ] Code follows naming conventions in this document.
- [ ] `[done]` posted to the board with file list and interface summary.

---

*Last updated by `qa-tester` · 2026-11-17*
