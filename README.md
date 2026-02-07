# 3D PacMinion

## Overview

3D PacMinion is a browser-based 3D arcade game built using Three.js. It combines classic Pacman-style gameplay with modern features like procedural map generation, 3rd-person immersive cameras, and Generative AI integration.

The player controls a Minion (or Pacman) to collect bananas while avoiding ghosts. The game features multiple power-ups, a life system, and warp tunnels.

## Features

### Gameplay

Objective: Collect all bananas/items to win.

Enemies: Four ghosts with different colors (Blinky, Pinky, Inky, Clyde) that chase the player.

Lives: Player starts with 3 lives. Getting hit respawns the player at the start with temporary invincibility.

Warp Tunnels: Moving off the left/right edge of the map teleports the player to the opposite side.

### Power-Ups (Fruits)

🍌 Banana: Basic score item (+10 points).

🍍 Pineapple (Giant Mode): Makes the player huge. Ghosts turn blue and flee. Player can eat ghosts for points.

🥭 Mango (Freeze): Freezes all ghosts in place for 10 seconds.

🍓 Strawberry (Invincibility): Player becomes transparent and cannot be killed by ghosts.

🍎 Apple (Extra Life): Grants +1 Life.

### Controls & Camera

- Movement: Arrow Keys or W/A/S/D.

- Tactical Mode: Absolute direction (Up moves North).

- Immersive Mode: Relative direction (Up moves Forward).

- Camera Toggle ('C'): Switches between Top-Down (Tactical) and Over-the-Shoulder (Immersive).

- Map Generation ('M'): Switches between the classic pre-made map and a new procedurally generated maze.

- Character Switch ('P'): Toggles the player model between a 3D Minion and a 3D Pacman.

- Mobile: Virtual Joystick for touch devices.

### AI & Audio

Gemini AI: Uses Google's Gemini API to generate dynamic taunts from ghosts during chases and sarcastic performance reviews at the Game Over screen.

Synthesized Audio: Uses the Web Audio API to generate sound effects (waka-waka, chimes, power-ups) without external assets.

## Technical Architecture

### Technologies

HTML5/CSS3: Structure and UI overlay.

JavaScript (ES6+): Game logic.

Three.js (r128): 3D Rendering engine.

Google Gemini API: Generative text for dynamic commentary.

### Code Structure (index.html)

The code is contained within a single HTML file for portability.

#### Global Variables & Configuration

Constants define game balance (Speed, Tile Size, Colors) and map layout.

const TILE_SIZE = 10;
const SPEED = 0.06; 
const MAP_DEFAULT = [...]; // 2D Array representing the grid


#### Audio System (initAudio, playSound)

Instead of loading MP3 files, the game synthesizes sounds in real-time using AudioContext oscillators. This reduces load times and file size.

Types: 'waka', 'coin', 'powerup', 'splat', 'eatghost', 'warp'.

#### AI Integration (callGemini)

Handles HTTP requests to the Gemini API.

Input: System prompt (context) and User prompt.

Output: Text string displayed in the UI.

Triggers:

triggerGhostTaunt: Called when a ghost starts a chase.

triggerGameReview: Called on Win/Loss.

#### Procedural Generation (generateMaze)

Uses a Depth-First Search (DFS) Recursive Backtracker algorithm to carve a perfect maze from a grid of walls.

Starts with a full grid of walls.

Carves paths randomly.

Post-processing adds a "Ghost House" in the center, clears warp tunnel paths, and punches random holes (loops) to make the maze imperfect and more playable.

#### Three.js Scene Setup (init, buildLevel)

Scene/Camera/Renderer: Standard boilerplate.

Lighting: Ambient + Directional + colored PointLights for "Neon" atmosphere.

Geometry:

Map: Created by iterating the MAP_LAYOUT array. 1 = BoxGeometry (Wall), 0 = Empty/Item.

Player: Group of primitives (Cylinder, Sphere, Torus) constructed to look like a Minion.

Items: TubeGeometry for Bananas, primitives for other fruits.

#### Game Loop (animate)

Uses requestAnimationFrame for 60fps rendering.

updatePlayer(): Handles movement, collision with walls (grid-based), and collision with items.

updateGhosts(): Handles AI pathfinding (simple distance checks + random wandering), collision with player, and visual state (scared/frozen).

updatePowerUps(): Manages timers for active effects.

updateCamera(): Smoothly lerps the camera position based on the selected mode.

#### Key Functions Explained

spawnEntity(obj, typeCode): Finds a random tile in the grid matching the typeCode (e.g., 9 for ghost house, 0 for empty path) and places the object there.

getGridPos(vector): Converts 3D world coordinates (x, z) into 2D grid integer coordinates (col, row) for collision detection.

isWalkable(x, z): Returns true if a grid cell is a path or a warp tunnel; false if it is a wall.

checkWarp(entity): Detects if an entity is at the map edge on the specific Warp Row and transports them to the other side.

#### Setup & Configuration

##### Prerequisites

A modern web browser (Chrome, Firefox, Edge).

Gemini API Key: To enable the AI text features, you must insert a valid API key into the apiKey constant at the top of the script.

#### Customization

You can tweak the game balance by modifying the CONFIGURATION section at the top of the script:

SPEED: Increase to make the player faster.

GHOST_SPEED: Adjust difficulty.

CHASE_RADIUS_TILES: Change how close you must be for a ghost to start chasing.

#### Running the Game

Simply open index.html in your browser. No server or build process is required (unless using modules, but this script uses the CDN version of Three.js).