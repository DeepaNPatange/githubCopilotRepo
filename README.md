# Memory Game Collection

A collection of single-file HTML games built with pure HTML, CSS, and JavaScript.

## Games Included

- `memory-game.html` — a responsive Memory (Concentration) card game with 4x4 and 6x6 grids, star rating, timer, and local high scores.
- `constellation-connect.html` — a star-connecting puzzle game where you recreate hidden constellations without crossing lines.

## Features

- 4x4 and 6x6 Memory game grid options
- Smooth card flip animations
- Timer and moves counter
- Star rating system
- Local storage for best scores
- Compact single-file game design
- Keyboard controls for accessibility
- Responsive design for all devices
- No external dependencies
- New Constellation Connect puzzle game with demo playback

## How to Play

### Memory Game
1. Open `memory-game.html` in a web browser
2. Click or use keyboard (Tab + Enter/Space) to flip cards
3. Find matching pairs to win
4. Try to complete the game with as few moves as possible
5. Switch between 4x4 and 6x6 grids using the buttons

### Constellation Connect
1. Open `constellation-connect.html` in a web browser
2. Select a level from the dropdown
3. Click a star to start a segment, then click another star to connect
4. Avoid crossing lines and use the least number of links
5. Use the `Hint` button to reveal a missing connection
6. Use the `Demo` button to watch the constellation build step-by-step

## Technical Details

- Pure HTML, CSS, and JavaScript implementation
- No external libraries or dependencies
- Fully responsive design using CSS Grid
- Accessible with ARIA attributes and keyboard controls
- Local storage for persistent high scores

## Development

1. Clone the repository
2. Open `memory-game.html` in a web browser
3. Or serve using a local HTTP server:
   ```bash
   python3 -m http.server 8000
   ```
4. Visit `http://localhost:8000/memory-game.html`