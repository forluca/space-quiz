# Copilot instructions

## Project shape

The entire app is a single self-contained `index.html` (inline `<style>` and `<script>`). Keep it that way: no build step, package manager, server, external fonts, CDN scripts, or image files. It must work when opened directly via `file:///…/index.html`.

There are no build, lint, or test commands. To verify changes, open `index.html` in the integrated browser and drive it with `evaluate_javascript` (click `.answer` buttons and `#next`, then assert on `#score`, `#progress`, `#feedback`, and `#results`). The integrated browser can't send real key presses; when the Playwright MCP server is available, use it for genuine Tab/Enter keyboard checks against the `file://` URL.

## Architecture

- **Quiz data**: the `questions` array in the script. Each entry is `{ category, question, answers[4], correct, fact }`; `correct` is a zero-based index into `answers`, and `fact` is shown as feedback after answering. The UI assumes exactly 10 questions (hard-coded `max="10"` on `#progress`, `/ 10` labels, `index === 9` for the final "See results" button), so update those together if the count changes.
- **State**: module-level `index`, `score`, `answered`, `history` (array of booleans used to render the result dots).
- **Flow**: `renderQuestion()` rebuilds the answer buttons → `selectAnswer(i)` locks all answers, applies `.correct`/`.wrong`/`.dim`, updates score/progress, and fills `#feedback` → `#next` advances or calls `showResults()` → `#restart` resets state.
- **Result tiers** in `showResults()`: ≥8 🚀, ≥5 🧑‍🚀, otherwise 🔭.

## Conventions

- **Theming**: all colors come from CSS custom properties on `:root` (`--paper`, `--ink`, `--muted`, `--line`, `--accent`, `--soft`, `--green`/`--green-bg`, `--red`/`--red-bg`). Dark mode is a single `@media (prefers-color-scheme: dark)` block that overrides those variables. Use the variables in new styles rather than literal colors so both themes keep working.
- **Feedback**: correct = green + `pop` animation + ✓; wrong = red + `shake` animation + ×. Never rely on color alone. The `prefers-reduced-motion` block disables all animations and transitions globally.
- **Accessibility contracts to preserve**:
  - Answers are native `<button>`s; after answering, focus moves to `#next`, then to `#question` (`tabindex="-1"`) on the next question and to `#result-title` on the results screen.
  - `#feedback` has `role="status"` and `#score` has `aria-live="polite"`.
  - `#answers` is `aria-labelledby="question"`.
  - The emoji reaction has `role="img"` with an `aria-label`.
- **DOM safety**: question/answer/fact text is inserted with `textContent`, not `innerHTML`; keep it that way.
- **Layout/typography**: narrow centered column (`.shell`, max 560px), system sans-serif stack, 14–16px body text (root 15px). `#question` uses a monospaced "mission-control readout" style (grid background, accent left rail, corner brackets).
- CSS is written compactly (one rule per line, minimal whitespace); match that style.
