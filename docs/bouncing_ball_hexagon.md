# Overview

`bouncing_ball_hexagon.html` is a single HTML file that implements a simple physics-based animation. It displays a ball bouncing inside a spinning hexagonal container. Users can adjust parameters like gravity, friction, and the hexagon's rotation speed using sliders. The animation runs entirely in the browser using HTML5 Canvas and JavaScript.

# Key Components

The code is primarily structured within a `<script>` tag and uses the HTML5 Canvas API for rendering.

- **Canvas Setup**:
    - `canvas`: The HTML5 canvas element where the animation is drawn.
    - `ctx`: The 2D rendering context for the canvas.
    - `centerX`, `centerY`: Coordinates of the center of the canvas.

- **Physics Parameters (User-adjustable via sliders)**:
    - `gravity`: Simulates the force of gravity acting on the ball.
    - `friction`: Represents energy loss when the ball moves or collides.
    - `rotationSpeed`: Controls how fast the hexagonal container spins.

- **Object Properties**:
    - `ball`: An object storing the ball's current position (`x`, `y`), radius, velocity (`velocityX`, `velocityY`), and color.
    - `hexagon`: An object storing the hexagon's radius, current rotation angle, and color.

- **Event Listeners (for sliders)**:
    - Event listeners are attached to the input sliders (`gravitySlider`, `frictionSlider`, `rotationSlider`) to update the corresponding physics parameters in real-time.

- **Drawing Functions**:
    - `drawHexagon()`: Draws the hexagonal container on the canvas. It calculates the vertices of the hexagon based on its radius and current rotation, then strokes the outline.
    - `drawBall()`: Draws the ball on the canvas at its current position. It includes a radial gradient to give the ball a simple 3D effect.

- **Collision and Physics Functions**:
    - `distanceToSegment(point, lineStart, lineEnd)`: A utility function to calculate the shortest distance from a point to a line segment. This is used for collision detection between the ball and the hexagon's sides.
    - `getHexagonVertices()`: Calculates and returns the current screen coordinates of the hexagon's vertices based on its center, radius, and rotation.
    - `checkCollision()`: Iterates through the sides of the hexagon. For each side, it checks if the ball is colliding with it using `distanceToSegment`. If a collision occurs, it calculates the reflection vector and updates the ball's velocity to simulate a bounce, applying energy loss due to friction. It also adjusts the ball's position slightly to prevent it from getting stuck in the wall.
    - `updatePhysics()`: Updates the ball's state in each animation frame. It applies gravity to the vertical velocity, applies friction to both velocity components, updates the ball's position based on its velocity, and increments the hexagon's rotation.

- **Animation Loop**:
    - `animate()`: The main animation loop. It clears the canvas, calls functions to draw the hexagon, update physics, check for collisions, and draw the ball. It then uses `requestAnimationFrame(animate)` to schedule the next frame, creating a continuous animation.

# Important Variables/Constants

- **`canvas` (HTMLCanvasElement)**: The DOM element for drawing.
- **`ctx` (CanvasRenderingContext2D)**: The drawing context.
- **`ball` (object)**: Contains properties `x`, `y`, `radius`, `velocityX`, `velocityY`, `color`. Its state changes every frame.
- **`hexagon` (object)**: Contains properties `radius`, `rotation`, `color`. Its `rotation` changes every frame.
- **`gravity` (number)**: Affects how quickly the ball accelerates downwards.
- **`friction` (number)**: Affects how quickly the ball loses speed.
- **`rotationSpeed` (number)**: Affects how fast the hexagon spins.

# Usage Examples

To use this file:
1.  Save the code as an HTML file (e.g., `bouncing_ball_hexagon.html`).
2.  Open the HTML file in a web browser.

The animation will start automatically. The user can interact with the animation by:
- Adjusting the "Gravity" slider to change the downward force on the ball.
- Adjusting the "Friction" slider to change how much the ball slows down over time and during bounces.
- Adjusting the "Rotation" slider to change the speed at which the hexagon spins.

# Dependencies and Interactions

- **HTML5 Canvas API**: Used for all drawing and rendering.
- **JavaScript**: All logic for animation, physics, and interactivity is written in JavaScript.
- **CSS**: Basic CSS is used for styling the page background, title, canvas container, and control sliders.

The file is self-contained and does not rely on external JavaScript libraries or CSS frameworks. All interactions are handled within the script itself. The primary interaction is between the physics simulation (updating ball position and velocity) and the rendering engine (drawing the ball and hexagon on the canvas). User input via sliders directly modifies the physics parameters, affecting the simulation in real-time.
