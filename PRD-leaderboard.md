## PRD: Leaderboard — Memory Game

## Overview
- Add a local, client-side leaderboard to the Memory Game that records the top 5 player results, ranked by fewest total moves. The leaderboard appears automatically when a game completes.
- Each leaderboard entry stores the player's display name, total moves, elapsed time (seconds), star rating, grid size (4x4 or 6x6), and ISO timestamp.

## Objective
- Provide persistent, easy-to-scan high-score visibility so players can compare best performances across sessions on the same device.
- Encourage replayability by surfacing the top 5 scores and prompting the player to enter their name when they finish a game.

## Scope
- Implement client-side storage using `localStorage` under a single key `memoryGameLeaderboard` that stores a JSON array of score objects.
- Add UI elements: a leaderboard panel (showing top 5), an editable name input in the end-of-game modal, and an optional "Clear leaderboard" button in the Best Scores section.
- Place the Best Scores panel in the top-right corner of the app on desktop view for immediate visibility; on small screens it remains inline below the grid.
- Provide `Clear leaderboard` and `Export` actions adjacent to the Best Scores panel for quick access.
- Add a `Reset Game` control in the main UI to allow users to restart the current game instantly; this control does not modify the leaderboard contents.

## Out of Scope
- Server-side persistence, cross-device sync, or account-based leaderboards (no network/API integration in this PR).
- Advanced ranking models (ELO, weighted scoring) and social features (comments, avatars, follows).

## Functional Requirements
- Save Score on Win: When a user completes a game, show the existing end-of-game modal extended with a name input field (max 20 characters) and a `Save Score` button. If the user submits a blank name, use `Anonymous`.
- Score Object Format: Each saved score must be an object containing: `{ name, moves, timeSeconds, rating, gridSize, dateISO }` and be appended to a locally stored array under `memoryGameLeaderboard`.
- Sorting & Truncation: After adding a new score, sort the leaderboard by ascending `moves`; for ties, sort by ascending `timeSeconds`. Persist only the top 5 entries (truncate extras before saving).
- Automatic Display: After saving, automatically display the updated leaderboard panel and update the Best Scores section in the main UI without requiring a page reload.
- Best Scores Panel: Add a `Best Scores` panel (or reuse the existing `.scores-list`) that shows ranked entries with format: `1. NAME — 12 moves — 01:32 — ★★★ — 4x4` and a small date (local). This list updates on load and after each completed game.
- Clear & Export: Provide a `Clear leaderboard` button that removes all entries from `localStorage` after a confirmation prompt; also provide a one-click `Export` link that downloads the current leaderboard as a JSON file named `memory-game-leaderboard.json`.
- UI Placement: The Best Scores panel and action buttons must be visible in the app header area (top-right on desktop) so users can quickly view and manage the leaderboard without scrolling.
- Button Resilience: Button click handlers must be robustly bound (defensive binding) so that UI layering or dynamic DOM changes do not prevent `Clear` and `Export` from functioning.
- Reset Behavior: Include a `Reset Game` button in the game UI that immediately resets the current board, clears current moves/timer/state, and preserves the leaderboard data. Clicking `Reset Game` also hides any end-of-game modal or prompts.
- Defensive Storage: Wrap all `localStorage` access in try/catch; if storage fails (private mode), fall back to an in-memory list for the session and show a non-intrusive warning in the UI.

## Acceptance Criteria
- Data Persistence: After completing and saving a score, reloading the page preserves the new entry in `memoryGameLeaderboard` (localStorage) and it appears in the Best Scores panel.
- Correct Ordering: Given a test dataset with scores having moves and times, the UI must display entries ordered by moves ascending, then time ascending. If 7 scores exist, only the best 5 persist.
- Name Handling: If a player saves a blank name, the stored name must be `Anonymous`. Names longer than 20 characters are truncated client-side before saving.
- Display & Modal Flow: The end-of-game modal must include a name input, `Save Score` and `Play Again` buttons. Clicking `Save Score` writes to storage, refreshes the scoreboard in the page, and displays the leaderboard panel.
- Storage Fallback: Simulate storage failure (e.g., throw from `localStorage.setItem`) — the app must not crash; instead, it retains scores only for the session and displays a small message "Leaderboard unavailable: persistent storage blocked." in the Best Scores panel.
- UI Placement Verified: On desktop viewport, the Best Scores panel appears fixed at the top-right and contains the `Clear leaderboard` and `Export` actions; on mobile viewport the panel is positioned inline under the grid.
- Button Robustness: If the `Clear leaderboard` or `Export` buttons are clicked, the expected action occurs even if the DOM is reflowed; handlers are registered defensively so the buttons do not silently fail.
- Reset Preserves Leaderboard: Clicking the `Reset Game` button resets the current game state (moves=0, timer reset, new shuffled board) but does not remove or alter any saved leaderboard entries. The leaderboard UI remains visible and unchanged after reset.
- Reset Hides Modal: If an end-of-game modal was open, clicking `Reset Game` must close the modal and return to the main board view.

---

## Implementation notes (developer-focused)
- Implement helper functions: `loadLeaderboard()`, `saveLeaderboard(array)`, `addScore(scoreObject)`, `formatTime(seconds)`, `exportLeaderboard()`.
- Reuse the existing `.scores-list` container to render the top-5 list and add `Clear leaderboard` / `Export` action buttons beneath it.
- Update the win modal (`.modal-content`) to include: name input (`maxlength=20`, `aria-label="Player name"`), `Save Score` button, and wire `Enter` key to submit.
- Ensure unit-friendly structure so these functions can be tested in isolation (pure functions for sorting/truncating).

If you'd like, I can also implement this feature directly in the repo (add UI and JS, tests, and commit), or create a follow-up PR draft that contains the exact code changes. Let me know which you'd prefer.
