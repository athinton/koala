# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Koala Apple Game - A browser-based 2D game where a koala collects apples while avoiding a kangaroo. Single-file HTML/CSS/JavaScript application with zero external dependencies.

## Running the Game

Open `koala-game.html` directly in a browser. No build step, server, or installation required.

## Architecture

### Single File Structure (`koala-game.html`)
- Lines 1-170: HTML structure + embedded CSS
- Lines 215-868: Embedded JavaScript game logic

### Core Pattern: Centralized Game State
All game data lives in a single `gameState` object:
```javascript
gameState = {
  score, timeLeft, gameActive, kangarooEnabled,
  koala: { x, y, size, speed, dx, dy, rotation, isSpinning, spinStartTime },
  kangaroo: { x, y, size, speed },
  apples: [],
  keys: {},
  isMoving
}
```

### Game Loop
Standard `requestAnimationFrame` loop: `gameLoop()` → `update()` → `draw()`

### Three UI Screens
State transitions managed by toggling `.hidden` class:
1. Menu Screen (`#menuScreen`) - Game options and start
2. Game Screen (`#gameScreen`) - Active gameplay canvas
3. Game Over Screen (`#gameOverScreen`) - Score and replay

### Rendering
HTML5 Canvas with draw order: apples → kangaroo → koala (back-to-front layering)

### Sound System
Web Audio API with procedurally generated sounds (no audio files):
- `playCrunchSound()` - Apple collection
- `playBonkSound()` - Kangaroo collision
- `startScurryingSound()`/`stopScurryingSound()` - Movement audio

### Collision Detection
Distance-based formula: `distance < (size1 + size2) / 2`

### Apple Types
- Regular: 1 point, always present
- Golden: 5 points, 6% spawn chance, 10 second lifetime
- Diamond: 10 points, 2% spawn chance, 5 second lifetime

### Input Handling
Unified abstraction - keyboard events and mobile touch buttons both set `gameState.keys` flags, which `update()` reads.
