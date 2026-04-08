# Stellar Guardian | 2D Space Shooter

A premium, web-native 2D space shooter designed with an emphasis on performance, aesthetics, and core Computer Graphics (CG) principles. This project demonstrates a production-level approach to game development using Vanilla JavaScript and HTML5 Canvas.

## 🚀 Key Features

- **Dynamic Leveling System**: Structured progression where difficulty (speed and spawn rates) scales across discrete levels.
- **Multi-Axis Combat**: Complete 2D navigation (W/A/S/D) within tactical vertical constraints.
- **Premium HUD**: High-performance, Glassmorphism-style Head-Up Display tracking Score, Level, and Lives.
- **Quantum Discharge (Combat)**: Fast-paced shooting mechanics with high-fidelity collision detection.
- **Cross-Platform Support**: Unified input handling for Desktop (Keyboard) and Mobile (On-screen Joystick + Fire button).
- **Rich Visuals**: Neon glow aesthetics, particle explosions, and multi-layered starfield parallax.

## 🛠 Tech Stack

- **Core Logic**: Vanilla JavaScript (ES6 Modules)
- **Rendering**: HTML5 Canvas API
- **Styling**: CSS3 with Glassmorphism and Neon design tokens
- **Architecture**: Object-Oriented Entity Management (Mark-and-Sweep pattern for robust cleanup)

## 🎓 Computer Graphics Concept Mapping

This project serves as a practical implementation of fundamental CG algorithms:

| Feature | CG Concept | Implementation Detail |
| :--- | :--- | :--- |
| **Bullet Rendering** | **Bresenham's Algorithm** | Manual implementation of integer-based line drawing for pixel-perfect projectiles. |
| **Ship Dynamics** | **2D Transformations** | Explicit matrix-based translation and rotation for ship "banking" and starfield movement. |
| **Collision Logic** | **AABB Detection** | Axis-Aligned Bounding Box checks for efficient real-time hit detection. |
| **Rasterization** | **Game Loop Pipeline** | Clear -> Update -> Render -> RequestAnimationFrame cycle. |

## 🕹 Controls

### Desktop
- **W / A / S / D** or **Arrow Keys**: Navigate Ship
- **SPACEBAR**: Quantum Discharge (Fire)

### Mobile
- **Virtual Joystick (Left Side)**: Navigate Ship
- **FIRE Button (Right Side)**: Quantum Discharge (Fire)

## 🖥 How to Run Locally

Since the project uses ES6 Modules, it requires a local web server to bypass CORS restrictions.

1.  **Direct Folder Access**: Simply navigate to the project directory in your terminal.
2.  **Start a Server**:
    ```bash
    # Using Python (Recommended)
    python3 -m http.server 8000
    
    # Or using Node.js
    npx serve .
    ```
3.  **Access the Game**: Open your browser and navigate to `http://localhost:8000`.

## 📂 Project Structure

- `index.html`: Main UI template and HUD structure.
- `style.css`: Premium styling and layout.
- `src/`:
    - `main.js`: Core Game class and loop management.
    - `entities.js`: Player, Bullet, Enemy, and Particle classes.
    - `renderer.js`: CG Algorithms (Bresenham, Transformations).
    - `input.js`: Unified input handling (Keyboard + Touch).
    - `config.js`: Global balancing and design tokens.
- `python_version/`: Original reference implementation using Pygame.

---
*Developed for the Computer Graphics Mini Project Showcase.*
