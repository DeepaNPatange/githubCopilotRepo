# PRD: Dark Theme — Memory Game

## Overview
Add a user-selectable dark theme to the Memory Game to improve usability in low-light conditions, match user OS preferences, and modernize the UI.

## Goals
- Provide a visually consistent dark theme that complements the existing light theme.
- Allow users to toggle theme manually and persist preference across sessions.
- Respect system-level dark mode preference by default.
- Maintain accessibility (contrast, keyboard navigation, screen reader labels).

## Success metrics
- 95% of color combinations meet WCAG AA contrast for text/interactive elements.
- No regressions in existing functionality or keyboard navigation.
- Persisted preference used on subsequent visits (localStorage key exists).

## User stories
- As a user, I want to toggle dark mode so the game is comfortable to play at night.
- As an accessibility-focused user, I want good contrast and keyboard support in dark mode.
- As a returning player, I want the game to remember my theme preference.

## UX & Interaction
- Add a theme toggle button in the header next to the title or stats area with a sun/moon icon.
- Default behavior:
  - If a saved preference exists in `localStorage`, use it.
  - Otherwise, follow the OS preference (`prefers-color-scheme`).
- Toggling the button flips themes immediately and stores the choice in `localStorage`.
- The DOM approach: add/remove a `.dark-theme` class on the `body` element. CSS variables control colors.

## Visual Design / Tokens
Introduce CSS custom properties in `:root` and `.dark-theme` overrides. Example tokens:

- `--background-color`
- `--text-color`
- `--primary-color`
- `--card-back`
- `--card-front`
- `--modal-background`
- `--surface` (panels/cards background)

Light theme (existing values) remain in `:root`; `.dark-theme` provides alternate values, e.g.: 
- `--background-color: #121212`
- `--text-color: #eaeaea`
- `--card-back: #1e88e5` (adjusted for contrast)
- `--card-front: #263238`

Keep emoji/card faces unchanged; ensure emoji render legibly on chosen backgrounds.

## Accessibility
- Ensure text and interactive controls meet WCAG AA contrast (4.5:1 for normal text). Use tools to validate.
- Toggle must be keyboard-focusable and have `aria-pressed` or `aria-checked` as appropriate and a visible focus outline.
- Maintain existing `aria-live` regions for `moves`, `timer`, and `rating`.
- Verify modal contrast and focus trap behavior remains correct in dark theme.

## Persistence
- LocalStorage key: `memoryGameTheme` with values `'dark' | 'light' | 'system'` (or simply `'dark'|'light'`).
- On load, script reads preference and applies `.dark-theme` if needed.

## Implementation details
1. Add CSS variable definitions to existing `<style>` in `memory-game.html`:
   - Move current color literals into `:root` variables if not already.
   - Add a `.dark-theme` selector with overrides.
2. Add a toggle button to the header area (HTML) with class `.theme-toggle` and an accessible label.
3. Add JS helper functions:
   - `applyTheme(theme)` — adds/removes `.dark-theme` and writes to `localStorage`.
   - `getPreferredTheme()` — resolves saved preference or `window.matchMedia('(prefers-color-scheme: dark)')`.
   - Initialize theme on page load before `initializeGrid()` to avoid a flash of incorrect theme.
4. Update any components that used hard-coded colors (e.g., inline styles) to use CSS variables.

## Backwards compatibility
- If localStorage is unavailable (private mode), default to system preference and fall back to light. Wrap storage access in try/catch.

## Testing
- Manual QA: verify toggle, persisted preference, initial system-respect, and keyboard navigation.
- Automated checks: run a contrast check (script or CI step) against token values.
- Test across 4x4 and 6x6 grids, modal visibility, and high-contrast browser settings.

## Rollout & Timeline
- Day 0: Add CSS variables, `.dark-theme`, and toggle button; unit manual testing.
- Day 1: Accessibility pass and fix contrast issues.
- Day 2: Merge and monitor metrics (errors, user feedback).

## Open questions
- Should we support an explicit `system` setting visible in UI, or implicit fallback only?
- Do we want to animate the theme transition (e.g., `transition` on color variables)?

---

This PRD is intentionally focused and implementation-ready; if you want, I can also create the initial CSS/JS changes and a PR branch implementing the feature.