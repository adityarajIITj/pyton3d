# 🪐 Pyton3D

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-brightgreen.svg)](https://www.python.org/downloads/)
[![Physics Engine](https://img.shields.io/badge/Physics-6--DOF%20Rigid%20Body-orange.svg)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey.svg)]()

> **Pyton3D** is a lightweight, high-performance, pure-Python 3D Physics Simulation Engine and CAD Workbench. It combines 6-Degrees-of-Freedom (6-DOF) Newtonian mechanics with an interactive desktop studio, real-time 3D viewport, dedicated tools for gravity and collision physics, and zero heavy binary C++ dependencies.

---

## 📸 Overview

```
+-----------------------------------------------------------------------------+
|  Pyton3D CAD Simulation Studio & Workbench                                  |
+-----------------------------------------------------------------------------+
|  [File] [Add Objects] [Tools & Windows] [Physics Labs]                      |
+-----------------------------------------------------------------------------+
|  [▶ Play] [⏭ Step] [🔄 Reset] [➕ Add Block] [🌍 Gravity] [⚙️ Collisions]   |
+----------------------+------------------------------------------------------+
|  📦 Hierarchy & Tabs |  3D Scientific CAD Viewport (Matplotlib 3D)          |
|  • Active Bodies     |     Z (Elevation)                                    |
|  • Mass & Positions  |      |   /                                           |
|  • Material Presets  |      |  /___ X (Horizontal)                           |
|  • Visual Overlays   |     /                                                |
|                      |    Y (Depth)                                         |
|  Eye Displays:       |                                                      |
|  ☑ AABB Wireframes   |  • Navigation Toolbar: Home, Pan, Zoom, Save         |
|  ☑ Contact Normals   |  • Real-time Telemetry HUD: FPS, Energy, Contacts    |
+----------------------+------------------------------------------------------+
```

---

## ✨ Features

### 🧮 1. Pure-Python Physics Dynamics (6-DOF)
- **State Vectors**: Position, Linear Velocity, Orientation Quaternion, Angular Velocity, Inertia Tensor in World Coordinates.
- **4 Numerical Integrators**:
  - `Symplectic Euler` *(Default, energy-conserving)*
  - `Velocity Verlet` *(Time-reversible)*
  - `Runge-Kutta 4th Order (RK4)` *(High accuracy)*
  - `Explicit Euler` *(Baseline comparison)*
- **Rodrigues Rotation Quaternion Optimization**: Vector rotations computed in 15 FLOPs without gimbal lock.
- **Sleep / Wake Optimization**: Automatically puts resting bodies to sleep to preserve computational bandwidth.

### 💥 2. SAT Collision Pipeline & Manifolds
- **Broad-Phase**: Fast Axis-Aligned Bounding Box (AABB) culling.
- **Narrow-Phase**:
  - **Separating Axis Theorem (SAT)** testing all 15 potential separating axes for Oriented Bounding Boxes (OBBs).
  - **Sphere vs Sphere**, **Sphere vs Box**, **Plane vs Box**, **Plane vs Sphere**.
- **Contact Manifolds**: Contact points, penetration depth, and normal conventions.

### ⚙️ 3. Impulse & Constraint Solver
- **Iterative Normal Impulse**: Restitution $(e)$ velocity rebound.
- **Dual-Axis Coulomb Friction**: Computes 2 orthogonal tangential friction impulses $(\mu_s, \mu_k)$.
- **Baumgarte Stabilization**: Positional recovery slop to eliminate body sinking and jitter.
- **Physical Constraints**:
  - `SpringConstraint`: Damped harmonic oscillators (Hooke\'s law + velocity damping).
  - `DistanceConstraint`: Rigid inelastic distance locks (e.g. Newton\'s cradle).
  - `HingeConstraint`: Revolute rotational joint axes.

### 🌍 4. Planetary Forces & Fluid Mechanics
- **Planetary Gravity**: Earth ($-9.81$), Moon ($-1.62$), Mars ($-3.71$), Zero-G ($0.0$), Jupiter ($-24.79$), or custom 3-axis vectors.
- **Aerodynamic Air Drag**: Linear and quadratic resistance ($F = -(k_1 v + k_2 v^2) \hat{v}$).
- **Archimedes Buoyancy**: Fluid immersion, displaced volume calculation, and fluid drag.

### 🖥️ 5. CAD Studio Desktop Application
- **Standard File Operations**: New Scene (`Ctrl+N`), Open JSON (`Ctrl+O`), Save JSON (`Ctrl+S`), Export Snapshot (`PNG`).
- **Dedicated Floating Windows**:
  - **➕ Object Spawner**: Configure dimensions, material, mass, spawn location, and launch velocity.
  - **🌍 Gravity & Forces Workbench**: Tune gravitational vectors, fluid buoyancy, and drag.
  - **⚙️ Collision Solver & Materials**: Tune solver iterations, penetration recovery %, and inspect materials.
- **Scene Hierarchy Inspector**: Real-time Treeview of bodies; delete or wake individual bodies.
- **Visual Overlays**: Toggle AABBs, collision contact normals & points, velocity vectors, and grid planes.
- **Built-in CAD Camera Presets**: Isometric view, Top-Down plan, Front elevation, Side profile.

---

## 🔬 7 Pre-built Physics Labs

| Lab Preset | Description |
| :--- | :--- |
| **🧪 1. Box Stacking** | Equilibrium stability test for 5 stacked boxes. |
| **🧪 2. Sphere Avalanche** | Multi-body sphere packing and avalanche mechanics. |
| **🧪 3. Mixed Collisions** | Composite shapes with different friction and bounciness properties. |
| **🧪 4. Springs & Pendulums** | 5-node harmonic oscillator chain under gravity. |
| **🧪 5. Jenga Tower Impact** | 24-piece interlocking tower struck by a high-velocity wrecking projectile. |
| **🧪 6. Fluid Buoyancy** | Floating wooden blocks bobbing in water with viscous fluid damping. |
| **🧪 7. Newton\'s Cradle** | Elastic momentum and kinetic energy transfer across 5 suspended steel spheres. |

---

## 🚀 Installation & Getting Started

### Prerequisites
- Python 3.9+
- `numpy`
- `matplotlib`

### Clone and Run
```bash
git clone https://github.com/adityarajIITj/pyton3d.git
cd pyton3d

# Install requirements
pip install -r requirements.txt

# Launch Pyton3D CAD Studio
python main.py
```

---

## ⌨️ Controls & Shortcuts

| Shortcut | Action |
| :--- | :--- |
| `SPACE` | Pause / Resume simulation |
| `S` | Step forward by one frame tick |
| `Ctrl + N` | Create a new blank scene with ground plane |
| `Ctrl + O` | Open and load a saved scene JSON |
| `Ctrl + S` | Save current scene configuration to JSON |
| `A` | Toggle AABB bounding boxes |
| `C` | Toggle contact normals and collision points |
| `V` | Toggle linear velocity vectors |
| `G` | Toggle 3D grid floor |
| `Left Click + Drag` | 3D Orbit Camera view |
| `Right Click + Drag` | Zoom In / Out |
| `Toolbar Home Button` | Reset viewport to default isometric CAD view |

---

## 📖 Programmatic API Example

You can also use Pyton3D as a headless physics engine in your own scripts:

```python
from pyton3d import PhysicsWorld, RigidBody, BoxCollider, SphereCollider, Materials, Vec3

# 1. Create World
world = PhysicsWorld(gravity=Vec3(0, -9.81, 0))

# 2. Add Ground Plane
ground = RigidBody(position=Vec3(0, -0.5, 0), is_static=True)
ground.collider = BoxCollider(Vec3(10, 0.5, 10))
ground.material = Materials.CONCRETE
world.add_body(ground)

# 3. Add Dynamic Body
box = RigidBody(position=Vec3(0, 5, 0), mass=2.0)
box.collider = BoxCollider(Vec3(0.5, 0.5, 0.5))
box.material = Materials.WOOD
world.add_body(box)

# 4. Step Physics
for step in range(120):
    world.step(dt=1/60)
    print(f"Step {step:03d} | Box Y: {box.position.y:.3f} m | Vel Y: {box.velocity.y:.3f} m/s")
```

---

## 📜 License
This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author
**Aditya Raj**  
Indian Institute of Technology Jodhpur (IIT Jodhpur)  
GitHub: [@adityarajIITj](https://github.com/adityarajIITj)
