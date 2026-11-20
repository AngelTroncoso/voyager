# ✅ Voyager — QA Test Checklist

**Version:** 1.0.0  
**Last Updated:** 2026-11-17  
**QA Lead:** qa-tester agent  
**Project:** Voyager — Constellation Game  
**Scope:** `index.html` (single-file build) and all supporting modules

> [!IMPORTANT]
> Run this checklist against every release candidate before marking it as "SHIP". Each test case must be explicitly marked PASS ✅, FAIL ❌, or SKIP ⏭️ with a tester's note.

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Passed |
| ❌ | Failed — log a bug |
| ⏭️ | Skipped (reason required) |
| 🔄 | Re-test after fix |

---

## 1. 🎮 Core Functionality

| # | Test Case | Steps | Expected Result | Priority |
|---|-----------|-------|----------------|---------|
| CF-01 | Game loads without errors | Open `index.html` in browser | No console errors; intro screen visible | P0 |
| CF-02 | Intro screen renders correctly | Observe intro screen | Title "VOYAGER" visible; Start button present; animated starfield in background | P0 |
| CF-03 | Game starts on click | Click Start button | Transitions to first puzzle (Cassiopeia); HUD shows name, score=0, timer (if applicable) | P0 |
| CF-04 | Background star field renders | Enter any puzzle | 200+ small background stars visible; randomized positions | P1 |
| CF-05 | Puzzle stars render | Enter any puzzle | Constellation stars visible as larger, brighter points; clearly distinguishable from BG stars | P0 |
| CF-06 | Star hover tooltip | Hover mouse over a puzzle star | Star name appears as tooltip within 100ms | P1 |
| CF-07 | First star selection | Click any puzzle star | Star highlights with brighter glow and/or selection ring; no connection drawn yet | P0 |
| CF-08 | Star deselection | Click an already-selected star | Star returns to default state; nothing else changes | P1 |
| CF-09 | Valid connection drawn | Select Star A, then click Star B (where A-B is a valid edge) | A line is drawn between A and B; stars return to normal (not selected); score unchanged at this step | P0 |
| CF-10 | Invalid connection rejected | Select Star A, then click Star C (no valid edge A-C) | Shake animation plays; "−50" score feedback shown; both stars deselect | P0 |
| CF-11 | Duplicate connection ignored | Draw connection A-B; try A-B again | No duplicate line drawn; no penalty applied; silent ignore | P1 |
| CF-12 | Constellation completion | Draw all required connections for a constellation | Celebration VFX fires; score for that constellation displayed; Next button appears | P0 |
| CF-13 | Score deduction on error | Make 3 incorrect connections | Score decremented by 50 for each (−150 total) | P0 |
| CF-14 | Hint reveals connection | Press Hint button | One un-drawn valid connection flashes/highlights briefly | P1 |
| CF-15 | Hint penalty applied | Use 1 hint; complete the constellation | Final score is 200 pts less than it would be without hint | P1 |
| CF-16 | Hint counter decrements | Note hint count; use a hint | Hint count in HUD decrements by 1 | P1 |
| CF-17 | Timer counts down | Enter a difficulty ≥2 puzzle | Timer in HUD decrements in real time (each second) | P0 |
| CF-18 | Timer triggers Game Over | Let timer reach 0 | Game Over screen appears; no score for that constellation | P0 |
| CF-19 | No timer on difficulty 1 | Play Cassiopeia or Crux | No timer element visible; game does not auto-end | P1 |
| CF-20 | Puzzle advance | Complete constellation N | Puzzle N+1 starts correctly; HUD updates to new constellation name | P0 |
| CF-21 | Victory screen after all | Complete all 10 constellations | Victory screen appears with total session score | P0 |
| CF-22 | High score saved | Complete a session; refresh page; start new game | Previous high score loaded and displayed correctly on Victory/Intro screen | P1 |
| CF-23 | Score formula correctness | Perfect play on Cassiopeia (5 stars, 4 connections, diff 1, no timer) | Score = 5×4×100 × 1.0 × 1.0 × 1.0 − 0 = **2,000 pts** | P1 |
| CF-24 | Speed bonus applied | Complete Orion with ≥90s remaining | Speed bonus reflected in score (above base); verify formula | P1 |
| CF-25 | Retry after Game Over | Trigger Game Over; click Retry | Same constellation restarts; score for that round resets; session total preserved | P1 |

---

## 2. 🖥️ UI / UX

| # | Test Case | Steps | Expected Result | Priority |
|---|-----------|-------|----------------|---------|
| UX-01 | HUD always visible | Enter any puzzle | HUD (constellation name, score, timer, hints) is always visible and not overlapping stars | P0 |
| UX-02 | Font loads correctly | Open game with network; observe typography | Orbitron or monospace fallback renders; no default serif text visible in HUD | P1 |
| UX-03 | Color palette correct | Observe game visuals | Background: `#000011`; star glow: blue-white; score: `#ffd700`; connection lines: white semi-transparent | P2 |
| UX-04 | Intro animation | Observe intro screen | Starfield animated; no static/frozen background | P2 |
| UX-05 | Completion animation | Complete any constellation | Particle burst or glow animation plays; duration ≤3 seconds; does not block input indefinitely | P1 |
| UX-06 | Error feedback readability | Make an invalid connection | Shake animation visible; score penalty number displayed clearly (e.g., "−50") | P1 |
| UX-07 | Transition smoothness | Move from puzzle to puzzle | No flickering; transition between constellations is smooth (fade or instant-clear) | P2 |
| UX-08 | Button states | Observe Hint button when hints = 0 | Button visually disabled (grayed out); click has no effect | P1 |
| UX-09 | Star size proportional to magnitude | Compare Vega (mag 0.03) vs Delta Lyr (mag 4.22) | Vega renders noticeably larger | P2 |
| UX-10 | Lore / constellation name displayed | Enter any puzzle | Constellation name shown in HUD; lore blurb visible on completion screen | P2 |

---

## 3. ⚡ Performance

| # | Test Case | Steps | Expected Result | Priority |
|---|-----------|-------|----------------|---------|
| PF-01 | Steady 60 FPS (desktop) | Open DevTools Performance; play for 30s | Frame rate consistently ≥60 FPS; no drops below 30 | P1 |
| PF-02 | Load time (cold) | Disable cache; reload page; measure | First Contentful Paint ≤3 seconds on a standard connection | P1 |
| PF-03 | Load time (warm) | Enable cache; reload page; measure | Page fully interactive ≤1 second | P2 |
| PF-04 | No memory leak | Complete all 10 constellations; check DevTools Heap | Heap size does not grow unboundedly; garbage collected between puzzles | P1 |
| PF-05 | Animation frame cleanup | Complete celebration VFX; check for orphaned rAF loops | No lingering `requestAnimationFrame` callbacks after transition | P1 |
| PF-06 | Mobile performance | Open on iPhone 12 or equivalent | Gameplay at ≥30 FPS; no janky scrolling or input lag | P2 |
| PF-07 | Canvas resize performance | Resize browser window rapidly during gameplay | Canvas updates correctly; no lag or visual artifacts; FPS acceptable | P2 |

---

## 4. 🧩 Edge Cases

| # | Test Case | Steps | Expected Result | Priority |
|---|-----------|-------|----------------|---------|
| EC-01 | Rapid clicking | Click rapidly (≥10 clicks/sec) on stars | No crash; no duplicate connections; score stays consistent | P0 |
| EC-02 | Click outside stars | Click empty canvas area | No star selected; no error; existing selection preserved or cleared gracefully | P1 |
| EC-03 | Very small viewport | Resize browser to 320×480 | Game still renders; stars clickable; no UI overflow clipping critical elements | P1 |
| EC-04 | Very large viewport | Open on 4K monitor (3840×2160) | Canvas scales correctly; stars well-distributed; not clustered in a corner | P2 |
| EC-05 | localStorage unavailable | Block localStorage in browser settings; play game | Game functions normally; no crash; high score feature silently degrades | P1 |
| EC-06 | Keyboard navigation attempt | Tab through the page; press Enter/Space | No JS errors; at minimum, keyboard does not break the game | P2 |
| EC-07 | Multiple rapid hint presses | Press Hint button multiple times quickly | Hint fires once; hint count correct; no duplicate connections revealed | P1 |
| EC-08 | Complete Scorpius (hardest) | Draw all 14 connections for Scorpius within 90s | Completion fires correctly; score reflects difficulty multiplier (×1.75) | P0 |
| EC-09 | Session with all errors maxed | Make 100 wrong connections across session | Score can go negative; game does not crash; negative score displayed | P2 |
| EC-10 | Offline play | Disconnect network; open `index.html` | Game works fully offline (unless Google Fonts CDN fails; font should fallback gracefully) | P1 |

---

## 5. ♿ Accessibility

| # | Test Case | Steps | Expected Result | Priority |
|---|-----------|-------|----------------|---------|
| AC-01 | Touch target size (mobile) | Inspect star hit areas on 375px viewport | Each star's clickable radius ≥44px (WCAG 2.5.5) | P1 |
| AC-02 | Contrast ratio — HUD text | Use a contrast checker on score/timer text | Text-to-background contrast ≥4.5:1 (WCAG AA) | P1 |
| AC-03 | Contrast ratio — Buttons | Check Start, Hint, Next button text contrast | Button label contrast ≥4.5:1 (WCAG AA) | P1 |
| AC-04 | No seizure-risk flashing | Observe celebration VFX | No rapid flashing >3 times per second (WCAG 2.3.1) | P0 |
| AC-05 | Reduced motion respect | Enable OS "Reduce Motion"; play game | Celebration animations simplified or skipped; no fullscreen flashes | P2 |
| AC-06 | Alt text / ARIA labels | Inspect DOM with screen reader | Canvas has `aria-label` or adjacent description; buttons have accessible names | P2 |
| AC-07 | Zoom compatibility | Browser zoom at 150% and 200% | Game still playable; text readable; buttons accessible | P1 |
| AC-08 | Color-only information | Identify connections using color alone | At least one non-color indicator (shape, label, or animation) distinguishes valid vs. invalid connections | P1 |

---

## 6. 🌐 Cross-Browser Compatibility

| # | Test Case | Browser | Expected Result | Priority |
|---|-----------|---------|----------------|---------|
| CB-01 | Full game play | Chrome 90+ | All features function correctly | P0 |
| CB-02 | Full game play | Firefox 88+ | All features function correctly | P0 |
| CB-03 | Full game play | Safari 14+ | All features function correctly; no canvas rendering differences | P1 |
| CB-04 | Full game play | Edge 90+ | All features function correctly | P1 |
| CB-05 | Mobile Safari | iOS 14+ Safari | Touch interactions work; canvas renders correctly | P1 |

---

## Bug Report Template

When a test fails, file a bug using this format:

```markdown
### BUG-<number>: <Short Title>

- **Test Case:** <CF-XX / UX-XX / etc.>
- **Severity:** P0 / P1 / P2
- **Environment:** Browser + version, OS, viewport size
- **Steps to Reproduce:**
  1. ...
  2. ...
- **Expected:** ...
- **Actual:** ...
- **Screenshot / Console log:** (attach)
- **Status:** Open / In Progress / Fixed / Won't Fix
```

---

## Sign-off

| Release | QA Lead | Date | Verdict |
|---------|---------|------|---------|
| v1.0.0 | qa-tester | — | Pending |

> [!NOTE]
> A release requires **all P0 tests PASS** and **no open P1 bugs** before shipping.
