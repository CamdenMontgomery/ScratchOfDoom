# ScratchOfDoom

## Description
**ScratchOfDoom** is a reimagining of the classic FPS **DOOM**, built entirely within [Scratch](https://scratch.mit.edu/).  

Scratch is best known as an educational tool for introducing programming, with simple drag-and-drop blocks and a strong focus on 2D games. It does not provide features like 3D rendering, mouse-lock camera controls, or raycasting — all things that DOOM relies on. Despite that, this project manages to simulate a first-person experience with movement, shooting, enemies, and a working HUD, created in just one week.  

Every mechanic had to be improvised: the camera is controlled by dragging the cursor, the pseudo-3D environment is built entirely from Scratch sprites, and combat had to be hacked together with creative workarounds. The end result is less about perfectly recreating DOOM, and more about seeing how far Scratch can be stretched past its intended design.  

---

## Install Guide
1. Download the project file: **`doom.sb3`**  
2. Open the [Scratch editor](https://scratch.mit.edu/projects/editor/) in your browser.  
3. Click on the menu at the top left:  
   - `File → Load from your computer`  
4. Select the `doom.sb3` file.  
5. Press the green flag to start playing.  

*(No Scratch account required to play locally — though having one allows you to save and remix the project.)*  

---

## Controls
Scratch doesn’t support standard FPS inputs, so the controls work a little differently:  

**Player Controls**  
- **W / A / S / D** → Movement   
- **Spacebar** → Shoot  
- **Click & Drag** → Rotate the camera left/right  

**Developer Controls**  
For those interested in the engine itself, these keys reveal how the renderer works:  
- **1** → Toggle wireframe mode  
- **2** → Render triangles using the custom fill algorithm  
- **3** → Render triangles line-by-line (inefficient, but more stable)  
- **N** → Toggle vertex normal visualization  

---

## Game Engine Architecture
The core of this project is a hand-built game engine designed to simulate a 3D environment within Scratch. It consists of three major subsystems:  


### 3D Renderer
At the heart of this project is a custom-built **3D software renderer**, written entirely in Scratch’s visual programming language. Since Scratch has no native 3D capabilities, every step of the graphics pipeline had to be implemented from first principles.  

Key technical components:  
- **OBJ File Parser** — Handles meshes with vertex normals, allowing external 3D models to be imported.  
- **Triangle Rasterization** — Two different filling algorithms were implemented:  
  - A custom fill algorithm optimized for speed.  
  - A line-by-line fill optimized for stability and accuracy.  
- **Matrix & Vector Math** — Full support for vector dot products, cross products, and matrix multiplication.  
- **Projection Pipeline** — Perspective projection matrices and a rendering pipeline built to simulate a 3D camera.  
- **Backface Culling** — Determined with vertex normals and dot product checks, reducing unnecessary rendering.  
- **Lighting** — Basic diffuse shading using vertex normals and light direction.  
- **Screen Clipping & Culling** — Prevents geometry from drawing outside the viewport.  
- **Depth Sorting** — Painter’s algorithm used to maintain correct triangle overlap.  
- **Triangulation** — Complex meshes and polygons are split into renderable triangles.  
- **Ray Casting for Combat** — Vector-based ray casting checks whether enemies intersect a straight ray from the camera.  
  - Each enemy is treated as if enclosed by a cylindrical hitbox, enabling hit detection along the player’s line of fire.  

---

### Enemy Management System
Beyond the renderer itself, I built a dedicated **enemy management system** to handle gameplay logic:  

- **Spawning Logic** — Enemies spawn dynamically, each with randomized attributes (e.g., position, health).  
- **Animation Handling** — Each enemy follows its own animation tracks with proper timing for idle, attack, and hit states.  
- **Z-Depth Calculation** — Enemies are continuously sorted by their distance from the camera to maintain proper render order.  
- **Integration with Combat** — Works with the ray casting system so only enemies aligned with the player’s fire line can be hit.  

---

### User Interface System
Even displaying numbers on the screen required building a custom solution. Scratch does not provide a straightforward way to render digits with custom fonts, so I had to create a **dedicated component for UI rendering**:  

- **Digit Cloning System** — Each digit of a number is represented by a sprite clone.  
- **Dynamic Costume Switching** — Cloned digits change their costume to match the correct value (0–9).  
- **Number Management** — Each number (health, ammo, etc.) is tracked as a series of digit instances, with careful positioning to maintain alignment.  

---

## Technical Challenges
The most difficult part of this project was not implementing the rendering pipeline itself, but **optimizing it to run at a playable framerate** inside Scratch.  

- On a mid-range PC, the game reaches around **18 FPS**, while higher-end systems can achieve **25 FPS**. While low by modern gaming standards, this performance is unusually high for a 3D engine built in Scratch — and it was achieved **without using TurboWarp**, since the challenge I imposed on myself required vanilla Scratch.  
- To reach these framerates, I designed a **custom triangle fill algorithm**. This method introduced some visual artifacts, but reduced draw calls by **over 100×**, boosting performance anywhere from **2× to 32×**.  
- Implementing the math was also time-intensive. Scratch’s drag-and-drop system made building **matrix multiplication, dot/cross products, and projection equations** painstakingly slow, and its poor copy/paste features made iteration frustrating.  
- Scratch lists are capped at **2000 elements**, which became a bottleneck for models with high triangle counts. This required additional parsing and careful data handling.  
- I also had to build a custom **OBJ file parser** and spent good time researching the format. Parsing structured text data in Scratch is not so straightforward.  
- External tools like **Desmos** were critical: I used it to derive equations for my custom shrink-fill algorithm for triangle rasterization, and for the **parametric clipping algorithm** that splits triangles spanning inside and outside the viewport.  

Overall, the project demanded several hours a day across the week to balance math, optimization, and Scratch’s limitations. It became both a valuable learning experience and a test of how much engineering discipline can be applied inside an unconventional environment.  

---

## Bugs & Limitations
This project was built under a **one-week time limit**, so some rough edges remain:  

- Health and ammo counters can display **negative values**.  
- Bullets hit **all enemies** along the line of fire instead of just the nearest one.  
- There is **no death screen** when health reaches zero.  
- The **damage effect** locks the cursor, preventing camera movement until it ends.  

These aren’t oversights so much as the natural byproduct of rapid prototyping on a platform that wasn’t designed for FPS gameplay. Anyone interested is welcome to rework, fix, and expand the project further.  

---

## Significance
This isn’t a faithful port of DOOM, but rather an exploration of how far Scratch can be pushed. In a space usually reserved for platformers and clicker games, ScratchOfDoom demonstrates that it’s possible to build a functioning 3D pipeline — complete with rendering, lighting, and game logic — in a tool that was never designed for it.  

It’s a case study in constraint-driven design: when traditional solutions aren’t available, you invent new ones.  

---

## Contributing
If you’d like to build on this project, here are some ideas for future improvements:  

- Add a proper **death screen** and game over state.  
- Improve the **line of fire logic** so bullets only hit the nearest target.  
- Smooth out **camera controls** for more natural play.  
- Rework the **HUD** to prevent negative values.  

Remixes are welcome! If you expand on the project, I’d love to see what you come up with.  
