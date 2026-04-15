# Computer Graphics: The Advanced Viva Master Guide 🎓
**Status**: Ultra-Detailed Expert Revision | **Faculty**: Senior Faculty, Harvard/VIT

This guide is designed for an "Outstanding" evaluation. It moves beyond definitions into the mathematical 'why' and technical 'how'.

---

## 🏗️ PART 1: Theoretical Deep-Dive & Derivations

### 1.1 Scan Conversion: The Bresenham Breakthrough
While DDA is intuitive (incremental additions), it suffers from precision loss due to floating-point rounding. **Bresenham's Algorithm** is the industry standard for integer-based scan conversion.

*   **The Derivation Logic**:
    At each step $x_{k+1}$, we choose between $(x_{k+1}, y_k)$ and $(x_{k+1}, y_k+1)$.
    We define the difference between the true line and the two candidate pixels as $d_1$ and $d_2$.
    The Decision Variable $P_k = dx(d_1 - d_2)$.
    If $P_k < 0$, the true line is closer to the lower pixel.
    If $P_k \ge 0$, the true line is closer to the upper pixel.
*   **Decision Variable recurrence**:
    $P_{k+1} = P_k + 2dy$ (if $P_k < 0$)
    $P_{k+1} = P_k + 2dy - 2dx$ (if $P_k \ge 0$)

### 1.2 Transformations: The Power of Homogeneous Coordinates
We use $3 \times 3$ matrices for 2D and $4 \times 4$ for 3D because simple Translation is an additive operation, but Rotation and Scaling are multiplicative. Homogeneous coordinates unify these into a single **Matrix Multiplicative System**.

*   **Rotation Matrix (Counter-Clockwise)**:
    $$R = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$$
*   **Composite Transformation**:
    Crucial Viva Point: Order matters! $T \cdot R \neq R \cdot T$.
    To rotate an object about an arbitrary point $(x_c, y_c)$:
    1. Translate to origin: $T(-x_c, -y_c)$
    2. Rotate: $R(\theta)$
    3. Translate back: $T(x_c, y_c)$
    **Result**: $M = T_{back} \cdot R \cdot T_{origin}$

### 1.3 Clipping: Liang-Barsky Parametric Analysis
Liang-Barsky is more efficient than Cohen-Sutherland because it avoids repeated clipping against each edge. It uses the parametric equation $x = x_1 + u \cdot dx$ and $y = y_1 + u \cdot dy$, where $0 \le u \le 1$.

*   **The Parameter $p_k$ and $q_k$**:
    For each edge, $u \cdot p_k \le q_k$.
    - $p_k < 0$: Line proceeds from outside to inside.
    - $p_k > 0$: Line proceeds from inside to outside.
    - $p_k = 0$: Line is parallel to the edge.
*   **The Clipping Logic**:
    We calculate $r_k = q_k / p_k$ for all edges.
    $u_{entry} = \max(0, r_k \text{ where } p_k < 0)$
    $u_{exit} = \min(1, r_k \text{ where } p_k > 0)$
    If $u_{entry} > u_{exit}$, the line is completely rejected.

---

## 🔬 PART 2: Advanced Lab Pack & Core Algorithms

### 2.1 Midpoint Circle Algorithm (Integer Math)
The initial decision variable $P_0 = \frac{5}{4} - R$. In integer implementations, we use $P_0 = 1 - R$ because the extra $1/4$ doesn't change the initial sign comparison.
*   **Symmetry**: Eight-way symmetry allows calculating only $45^\circ$ of the circle.
*   **Recurrence**:
    - If $P_k < 0$: $P_{k+1} = P_k + 2x_{k+1} + 1$
    - If $P_k \ge 0$: $P_{k+1} = P_k + 2x_{k+1} + 1 - 2y_{k+1}$

### 2.2 Bezier Curves: Cubic Splines
A cubic Bezier curve is defined by 4 points ($P_0, P_1, P_2, P_3$).
*   **Equation**: $Q(t) = (1-t)^3P_0 + 3t(1-t)^2P_1 + 3t^2(1-t)P_2 + t^3P_3$
*   **Basis Matrix**:
    $$M_{\text{Bezier}} = \begin{bmatrix} -1 & 3 & -3 & 1 \\ 3 & -6 & 3 & 0 \\ -3 & 3 & 0 & 0 \\ 1 & 0 & 0 & 0 \end{bmatrix}$$
*   **Properties**:
    - **Convex Hull Property**: The curve always stays within the polygon formed by its control points.
    - **Endpoint Interpolation**: It passes through $P_0$ and $P_3$.

### 2.3 Shading Model Comparison
| Feature | Flat Shading | Gouraud Shading | Phong Shading |
| :--- | :--- | :--- | :--- |
| **Logic** | One color per face | Interpolates intensities | Interpolates normal vectors |
| **Speed** | Fastest | Moderate | Slowest |
| **Highlights** | None | Jaggies/Blurred | Accurate Specular Highlights |
| **Complexity** | Low | Medium | High |

---

## 👾 PART 3: "Stellar Guardian" Code Analysis (Deep Mapping)

When asked about your implementation, point to these specific spots in your code:

### 3.1 Custom Transformation Pipeline (`src/renderer.js:L44`)
> "In `transformPoint`, I don't use the simple `ctx.rotate`. I've manually implemented the **Affine Transformation Cycle**: Translate to Origin $\rightarrow$ Trigonometric Rotation $\rightarrow$ Scaling $\rightarrow$ Translation Back. This demonstrates my understanding of how GPUs actually process vertices before they hit the rasterizer."

### 3.2 The Bullet Rasterizer (`src/renderer.js:L7`)
> "For bullets, I implemented a native **Bresenham's Line Algorithm** (`drawBresenhamLine`). I use a **Double-Error Variable** approach (`let e2 = 2 * err`) which is the most optimized version of the algorithm, avoiding all multiplication within the `while` loop, satisfying the core Semester IV CG requirements for efficient scan conversion."

### 3.3 Collision as Region-Testing (`src/renderer.js:L69`)
> "My `checkAABB` function is fundamentally an **Interval Intersection Test**. It checks if the region occupied by one sprite overlaps another. In CG terms, this is effectively performing a **Clipping Check** on two rectilinear viewports to see if their intersection set is non-null."

### 3.4 Particle Systems (The Explosions)
> "When an enemy is destroyed, I generate 15 particles. This is a **Kinetic Particle System**. Each particle's trajectory is a linear transformation over time ($P_{\text{new}} = P_{\text{old}} + V \cdot \Delta t$), representing a continuous sequence of translations in the 2D world space."

---

### 🔥 Pro-Tips for the Exam:
1.  **Perspective vs Parallel**: If asked which is better, say: "Perspective is for realism (**Vanishing Points**), but Parallel (Orthographic) is for accuracy (**Preserves dimensions**, used in blueprints)."
2.  **Fractals**: If asked about applications, mention: "Generating procedural terrain, clouds, and coastlines in modern game engines like Unreal or Unity."

**This is the definitive depth you need. Go forth and conquer.**
