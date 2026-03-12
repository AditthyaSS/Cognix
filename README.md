<div align="center">

# LLM Minecraft

**A real-time strategy game for building and connecting LLM agent pipelines in a 3D isometric world.**

[![React](https://img.shields.io/badge/React_18-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://react.dev)
[![Three.js](https://img.shields.io/badge/Three.js-000000?style=for-the-badge&logo=threedotjs&logoColor=white)](https://threejs.org)
[![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev)
[![Zustand](https://img.shields.io/badge/Zustand-443E38?style=for-the-badge&logo=npm&logoColor=white)](https://zustand-demo.pmnd.rs/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)

[Getting Started](#getting-started) · [Features](#features) · [Architecture](#architecture) · [Roadmap](#roadmap)

</div>

---

## Overview

LLM Minecraft reimagines AI pipeline orchestration as a city-building game. Each **building** in the town represents a project. Each **agent** inside represents an LLM node. Players place buildings, configure agents, wire them together through visual connections, and watch AI pipelines execute in real time.

The entire 3D world is built from code-generated geometry — no external 3D model files. Every mesh is a primitive `boxGeometry`, `coneGeometry`, or `sphereGeometry`, assembled into rich low-poly scenes.

> **Phase 1** delivers the complete visual shell with mocked AI activity. All agent output is simulated with character-by-character token streaming.

---

## Getting Started

```bash
# Clone
git clone https://github.com/AditthyaSS/llm-Minecraft.git
cd llm-Minecraft

# Install dependencies
npm install

# Start development server
npm run dev
```

Open `http://localhost:5173` in your browser.

### Controls

| Action | Input |
|---|---|
| Rotate camera | Left-click drag |
| Pan camera | Right-click drag |
| Zoom | Scroll wheel |
| Enter building | Click a building |
| Select agent | Click a desk |
| Return to town | Click **Back** button |
| Run pipeline | Click **Run All** |

---

## Features

### Town View

The overworld is a 20×20 tiled isometric scene featuring:

- **Buildings** — Minecraft-style grass base blocks, tan multi-story walls with window shutters, three-layered dark red roofs, chimneys with smoke particles, and pulsing glow rings on active projects.
- **Billboards** — Tall wooden pole structures with diagonal support struts and lit purple panels displaying project names and agent counts.
- **Environment** — Low-poly layered trees, wooden perimeter fences, stone paths connecting buildings, and street lamps with animated glow.
- **Animals** — Pigs, sheep, and cats that wander the ground with randomized movement AI, idle timers, and rotation toward their travel direction.
- **UI** — Project sidebar with status indicators, top toolbar with mode buttons, gold counter, session/today progress dots, zoom indicator, and a canvas-based overhead minimap.

### Office Interior

Clicking a building transitions the camera into a detailed office room:

- **Room** — Green carpet floor with grid lines, light-colored walls with baseboards, ceiling with fluorescent panel lights.
- **Wall Props** — Digital clock display, metrics screens with bar chart visualizations, analog clock with animated hour and minute hands.
- **Decorations** — Blue balloon clusters, potted plants, filing cabinets with drawer handles, rotating purple crystal, pulsing status dome, starburst ornament.
- **Agent Desks** — White desks with monitor and bezel, keyboard with key rows, mouse, coffee mug, pencil holder, and office chair with star-base casters.
- **Robot Agents** — Five color-coded character types with body detail stripes, chest screen, visor face plate, antenna, and animated arm swing during thinking state.
- **Connections** — CatmullRomCurve3 tube wires between desks with animated traveling dots when the pipeline is active.
- **Agent Panel** — Chat-style sidebar showing agent role, model selector, streaming output with typing cursor, and action buttons.

### Agent Types

| Role | Color | Default Model | Description |
|---|---|---|---|
| Lead | Gold | claude-3-5-sonnet | Coordinates and reviews work |
| Researcher | Blue | claude-3-5-sonnet | Analyzes data and provides insights |
| Worker | Green | gpt-4o | Executes tasks efficiently |
| Critic | Pink | claude-3-5-sonnet | Reviews and finds issues |
| Writer | Purple | gpt-4o | Creates documentation |

### Status Indicators

Each agent has a floating status orb that changes based on state:

| Status | Color | Behavior |
|---|---|---|
| Idle | Grey | Static |
| Thinking | Blue | Robot bobs and swings arms |
| Done | Green | Static after completion |
| Error | Red | Static |

---

## Architecture

### Tech Stack

| Layer | Technology |
|---|---|
| Framework | React 18 + Vite |
| 3D Engine | Three.js with @react-three/fiber |
| 3D Utilities | @react-three/drei (Text, Html, OrbitControls) |
| Post-processing | @react-three/postprocessing (Bloom) |
| State Management | Zustand |
| Styling | Tailwind CSS |
| Camera Animation | GSAP |
| Fonts | Press Start 2P (game text), Inter (UI panels) |

### Project Structure

```
src/
├── App.jsx                         # Root — switches Town / Office view
├── main.jsx                        # Entry point
├── index.css                       # Tailwind + global styles
│
├── game/
│   ├── core/
│   │   ├── Engine.jsx              # R3F Canvas, shadows, postprocessing
│   │   ├── CameraRig.jsx           # OrbitControls + GSAP transitions
│   │   └── Lighting.jsx            # Directional, ambient, hemisphere
│   │
│   ├── world/
│   │   ├── Ground.jsx              # 20x20 tiled floor with path
│   │   ├── GrassTile.jsx           # Minecraft-style raised block
│   │   ├── Building.jsx            # Walls + roof + chimney + smoke
│   │   ├── Billboard.jsx           # Pole + lit sign panel
│   │   ├── Tree.jsx                # Layered cone foliage
│   │   ├── Road.jsx                # Stone path tiles
│   │   ├── Fence.jsx               # Posts with dual rails
│   │   └── Lamp.jsx                # Street lamp with glow
│   │
│   ├── animals/
│   │   ├── Pig.jsx                 # Box-geometry pig
│   │   ├── Sheep.jsx               # Fluffy sheep
│   │   ├── Cat.jsx                 # Slim cat with tail
│   │   └── AnimalController.jsx    # Wandering behavior system
│   │
│   ├── office/
│   │   ├── OfficeRoom.jsx          # Room, walls, props, decorations
│   │   ├── AgentDesk.jsx           # Desk + monitor + chair + items
│   │   ├── RobotCharacter.jsx      # Color-coded robot agent
│   │   └── Wire.jsx                # Curved tube with traveling dot
│   │
│   ├── scenes/
│   │   ├── TownScene.jsx           # Assembles town world
│   │   └── OfficeScene.jsx         # Assembles office interior
│   │
│   └── utils/
│       ├── colors.js               # Central color palette
│       ├── geometry.js             # Geometry + path helpers
│       └── mockStream.js           # Fake token streaming
│
├── ui/
│   ├── HUD.jsx                     # Top toolbar + stats
│   ├── Sidebar.jsx                 # Project list panel
│   ├── AgentPanel.jsx              # Agent config + chat output
│   ├── Minimap.jsx                 # Canvas-based overhead map
│   ├── BuildMenu.jsx               # Agent type picker
│   ├── BackButton.jsx              # Office exit button
│   └── Toast.jsx                   # Notifications
│
└── store/
    └── gameStore.js                # Zustand state management
```

### State Management

The Zustand store manages all game state including:

- **View state** — Current view (town/office), active project, tool mode
- **Projects** — Array of project objects with positions, agent IDs, and running status
- **Agents** — Map of agent objects with name, model, system prompt, status, and output
- **Connections** — Array of wire connections between agents
- **Animals** — Array of animal objects with positions, targets, speeds, and idle timers
- **Session data** — Cost tracking, API call counters, zoom level

---

## Roadmap

| Phase | Description | Status |
|---|---|---|
| Phase 1 | Visual shell with mocked AI activity | Complete |
| Phase 2 | Real API integration (Anthropic, OpenAI, Gemini) | Planned |
| Phase 3 | Drag-to-connect wire creation | Planned |
| Phase 4 | Persistence and user accounts | Planned |
| Phase 5 | Multi-user collaboration | Planned |

---

## License

MIT

---

<div align="center">
<sub>Every mesh is a primitive. No external 3D models. Pure code-generated geometry.</sub>
</div>
