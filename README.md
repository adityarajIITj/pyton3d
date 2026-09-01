# Pyton3D

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python: 3.9+](https://img.shields.io/badge/Python-3.9+-brightgreen.svg)](https://www.python.org/downloads/)
[![Physics Engine: Built From Scratch](https://img.shields.io/badge/Physics_Engine-Built_From_Scratch-success.svg)]()
[![Platform: Cross-Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey.svg)]()

Pyton3D is a full-featured, 6-Degrees-of-Freedom (6-DOF) 3D rigid body physics simulation engine and interactive CAD workbench, **built completely from scratch in pure Python**.

It does not rely on third-party physics libraries or native wrappers (such as PyBullet, Box2D, ODE, or PhysX). Every component—from foundational linear algebra and quaternion kinematics to 3D Separating Axis Theorem (SAT) collision detection, iterative impulse manifolds, and numerical integrators—is implemented from first principles.

---

## Key Highlights

- **Built 100% From Scratch**: Zero proprietary or binary physics dependencies.
- **Full 6-DOF Dynamics**: Accurate tracking of linear momentum, angular velocity, world-space inertia tensor transformations, and quaternion rotations.
- **Multiple Numerical Integrators**: Symplectic Euler, Velocity Verlet, Runge-Kutta 4th Order (RK4), and Explicit Euler.
- **Robust Collision Detection**: Broad-phase AABB culling and Narrow-phase SAT for Oriented Bounding Boxes (OBB), Spheres, and Half-Space Planes.
- **Constraint and Contact Solver**: Iterative normal impulse resolution, 2-axis Coulomb friction cones, Baumgarte stabilization, damped springs, and distance joints.
- **Interactive CAD Desktop Studio**: Integrated graphical workbench with real-time 3D viewport, dedicated tools for object spawning, gravity configuration, collision matrix inspection, and scene JSON serialization.

---

## Architectural Overview

```
+-----------------------------------------------------------------------------+
|                     Pyton3D System Architecture                             |
+-----------------------------------------------------------------------------+
|  1. CAD Studio Interface (Tkinter + Embedded Matplotlib 3D Viewport)        |
|     - Scene Hierarchy Inspector & Property Panels                           |
|     - Floating Tool Windows: Object Spawner, Gravity, Collision Solver      |
|     - Standard Navigation Toolbar: Orbit, Pan, Zoom, Reset, Save Snapshot   |
+-----------------------------------------------------------------------------+
|  2. Constraint and Impulse Solver                                           |
|     - Normal Impulse with Restitution Recovery                              |
|     - Dual-Axis Orthogonal Coulomb Friction Formulation                    |
|     - Baumgarte Penetration Slop Stabilization                              |
|     - Constraints: Damped Springs, Inelastic Distance Rods, Hinge Pivots    |
+-----------------------------------------------------------------------------+
|  3. Collision Detection Pipeline                                            |
|     - Broad-Phase: Spatial Axis-Aligned Bounding Box (AABB) Culling         |
|     - Narrow-Phase: Separating Axis Theorem (SAT) across 15 OBB axes        |
|     - Contact Manifolds: Contact Points, Penetration Depths, Normal Vectors |
+-----------------------------------------------------------------------------+
|  4. 6-DOF Rigid Body Dynamics                                               |
|     - Mass, Linear Velocity, World-Space Inertia Tensor Translation         |
|     - Orientation Quaternions with Optimized Rodrigues Rotation Formulation |
|     - Dynamic Sleep and Wake Thresholding                                   |
+-----------------------------------------------------------------------------+
|  5. Foundational Mathematics Core                                           |
|     - Vec3, Mat3, Mat4, Quaternion, Ray, Plane, AABB, OBB                   |
+-----------------------------------------------------------------------------+
```

---

## Core Features

### 1. Mathematics and Kinematics
- **Vector Operations**: Custom `Vec3` class with optimized dot products, cross products, normalization, and geometric projections.
- **Rotations via Quaternions**: Fast vector rotation via the Rodrigues formula:
  $$\vec{v}\' = \vec{v} + 2 q_w (\vec{q}_v \times \vec{v}) + 2 (\vec{q}_v \times (\vec{q}_v \times \vec{v}))$$
  Executes in 15 floating-point operations per rotation, eliminating gimbal lock and avoiding costly full matrix constructions.
- **Dynamic Inertia Tensors**: $3 \times 3$ matrix representations transformed into world coordinates per frame:
  $$\mathbf{I}_{world}^{-1} = \mathbf{R} \mathbf{I}_{local}^{-1} \mathbf{R}^T$$

### 2. Collision Detection
- **Separating Axis Theorem (SAT)**: Evaluates 15 potential separating axes for Oriented Bounding Boxes (3 face normals of A, 3 face normals of B, and 9 edge cross products).
- **Supported Geometry Pairs**:
  - Box vs Box (OBB SAT)
  - Sphere vs Sphere
  - Sphere vs Box
  - Plane vs Box
  - Plane vs Sphere

### 3. Contact Solver and Constraints
- **Sequential Impulse Resolution**: Calculates normal impulse $j_n$ based on effective contact mass $m_{eff}$ and coefficient of restitution $e$.
- **Coulomb Dry Friction**: Enforces static and kinetic friction limits along two orthogonal tangent directions:
  $$|j_t| \le \mu j_n$$
- **Joints and Springs**:
  - `SpringConstraint`: Damped harmonic oscillators utilizing Hooke\'s law with velocity damping.
  - `DistanceConstraint`: Rigid distance constraints maintaining fixed separation between anchor points.

### 4. Environmental Forces
- **Planetary Gravity**: Presets for Earth ($-9.81\text{ m/s}^2$), Moon ($-1.62\text{ m/s}^2$), Mars ($-3.71\text{ m/s}^2$), Zero Gravity ($0\text{ m/s}^2$), or user-defined 3-axis vectors.
- **Aerodynamic Drag**: Linear and quadratic resistance models.
- **Archimedes Buoyancy**: Computes submerged volume displacement and upward buoyant force in fluid media.

---

## Interactive CAD Studio Application

The application provides a comprehensive desktop graphical interface:

- **File Management**:
  - `New Scene` (`Ctrl+N`): Resets the simulation to a clean ground plane.
  - `Open Scene` (`Ctrl+O`): Loads complete scene states from structured JSON files.
  - `Save Scene` (`Ctrl+S`): Serializes all active bodies, colliders, materials, and forces to JSON.
  - `Export Snapshot`: Renders high-resolution image captures of the 3D viewport.
- **Dedicated Tool Windows**:
  - **Object Spawner**: Configure shape (Box, Sphere), half-extents, radius, material properties, mass, spawn location, and launch velocity.
  - **Gravity and Forces Workbench**: Live interactive sliders for gravity axes, air drag, and fluid buoyancy parameters.
  - **Collision Solver and Materials**: Adjust impulse iterations ($1-30$), penetration recovery percentages, and inspect material coefficients.
- **Scene Hierarchy**: Treeview displaying active rigid bodies, mass, and elevation. Supports single-body deletion and global waking.
- **Visual Diagnostics**: Independent toggles for AABB wireframes, contact normal vectors, linear velocity indicators, and coordinate grid panes.

---

## Demonstration Labs

Pyton3D includes 7 pre-configured classical mechanics labs:

| Lab Name | Description |
| :--- | :--- |
| **1. Box Stacking** | Verification of equilibrium stability and contact manifold convergence for stacked rigid bodies. |
| **2. Sphere Avalanche** | Multi-body sphere packing, granular flow, and dynamic rolling friction. |
| **3. Mixed Collisions** | Complex interactions between diverse geometric shapes and material properties. |
| **4. Springs and Pendulums** | Multi-link damped harmonic spring chain demonstrating coupled oscillations. |
| **5. Jenga Tower Impact** | Interlocking 24-piece structural tower subjected to high-velocity projectile impact. |
| **6. Fluid Buoyancy** | Floating wooden bodies experiencing Archimedes buoyancy, fluid drag, and surface equilibrium. |
| **7. Newton\'s Cradle** | Conservation of momentum and kinetic energy transfer through five suspended elastic bodies. |

---

## Installation and Usage

### Prerequisites
- Python 3.9 or higher
- `numpy`
- `matplotlib`

### Getting Started

```bash
# 1. Clone repository
git clone https://github.com/adityarajIITj/pyton3d.git
cd pyton3d

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch Pyton3D Studio
python main.py
```

---

## Headless Python API

Pyton3D can also be imported as a standalone module in custom scripts or pipelines:

```python
from pyton3d import PhysicsWorld, RigidBody, BoxCollider, SphereCollider, Materials, Vec3

# Initialize physics world
world = PhysicsWorld(gravity=Vec3(0, -9.81, 0))

# Add static ground plane
ground = RigidBody(position=Vec3(0, -0.5, 0), is_static=True)
ground.collider = BoxCollider(Vec3(10, 0.5, 10))
ground.material = Materials.CONCRETE
world.add_body(ground)

# Add dynamic wooden box
box = RigidBody(position=Vec3(0, 5.0, 0), mass=2.5)
box.collider = BoxCollider(Vec3(0.5, 0.5, 0.5))
box.material = Materials.WOOD
world.add_body(box)

# Step simulation loop
for step in range(120):
    world.step(dt=1/60)
    print(f"Step {step:03d} | Elevation: {box.position.y:.3f} m | Velocity: {box.velocity.y:.3f} m/s")
```

---

## Keyboard and Viewport Controls

| Input | Function |
| :--- | :--- |
| `SPACE` | Toggle pause and simulation execution |
| `S` | Advance simulation by a single frame tick |
| `Ctrl + N` | Create a new blank scene |
| `Ctrl + O` | Open and load scene from JSON |
| `Ctrl + S` | Save current scene configuration to JSON |
| `A` | Toggle AABB bounding box wireframes |
| `C` | Toggle contact normal vectors and collision points |
| `V` | Toggle linear velocity direction vectors |
| `G` | Toggle coordinate plane floor grid |
| `Left Mouse Drag` | Orbit 3D camera |
| `Right Mouse Drag` | Zoom viewport in and out |
| `Toolbar Home Button` | Reset viewport to default isometric CAD perspective |

---

## Technical Documentation

Detailed mathematical derivations, SAT collision projection algorithms, constraint formulations, and serialization specifications are documented in [DOCUMENTATION.md](DOCUMENTATION.md).

---

## License

Distributed under the MIT License. See [LICENSE](LICENSE) for more information.

---

## Author

**Aditya Raj**  
Indian Institute of Technology Jodhpur (IIT Jodhpur)  
GitHub: [@adityarajIITj](https://github.com/adityarajIITj)
