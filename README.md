# 2d-beam-element
2D Beam Element Finite Element Method (FEM) in Python | Mesh Generation, Boundary Conditions, Global Assembly, and Visualization

# 🧮 2D Beam Element Finite Element Method (FEM) in Python

This repository contains a **Python-based Finite Element Analysis (FEA)** solver for **2D Beam Elements**.  
It performs structural analysis of Euler–Bernoulli beams under various boundary conditions and loading scenarios, all implemented and visualized within **Google Colab**.

Developed by **Navin C. Chacko**, this project demonstrates the core workflow of FEM — from mesh generation to stiffness matrix assembly, solution, and postprocessing.

---

## ⚙️ Features

✅ **Geometry Definition** — Material and geometric properties (E, I, L, A)  
✅ **Mesh Generation** — Automatic element division based on number or size  
✅ **Boundary Conditions** — Supports Dirichlet (fixed) and Neumann (loads/moments) BCs  
✅ **Stiffness Matrix Assembly** — Element and global stiffness formulation  
✅ **Distributed Loads** — Supports both point and uniform distributed loads  
✅ **Solver** — Displacement and rotation computation using the stiffness method  
✅ **Postprocessing** — Displacement visualization, reaction forces, and VTK export for ParaView  

---

## 📘 **Classes Overview**

| Class | Purpose |
|-------|----------|
| `Geometry` | Stores material (E, I) and geometric (L, A) properties |
| `Mesh` | Generates nodes, elements, and visualizes the beam mesh |
| `BoundaryConditions` | Handles supports, loads, and distributed loading |
| `BeamFEM` | Assembles stiffness, applies BCs, and solves displacements |
| `PostProcessing` | Computes reactions, interpolates deflections, exports to VTK |

---

## 🧠 **Theory**

This solver is based on the **Euler–Bernoulli Beam Theory**, assuming:
- Small deformations
- Linear elastic materials
- Plane bending behavior

The **element stiffness matrix** is derived from the beam’s fourth-order differential equation:

\[
\frac{d^2}{dx^2}\left(EI \frac{d^2 w}{dx^2}\right) = q(x)
\]

---

## 🧩 **Example Usage**

```python
from beam_fem import Geometry, Mesh, BoundaryConditions, BeamFEM, PostProcessing

# Geometry: E (Pa), I (m^4), L (m)
geo = Geometry(E=210e9, I=8.33e-6, L=5.0)

# Mesh: divide beam into elements
mesh = Mesh(geo, num_elements=3)
mesh.plot_mesh()

# Boundary conditions
bc = BoundaryConditions(geo, mesh)
bc.add_dirichlet(0, (0, 0))     # Fixed at left end
bc.add_neumann(3, (1, 0))       # Point load at right end
bc.plot()

# FEM Solver
solver = BeamFEM(geo, mesh, bc)
K = solver.assemble_global_stiffness()
F = solver.assemble_global_load_vector()
U, K, F = solver.solve()

# Post-processing
post = PostProcessing(mesh, U, K, F)
post.plot_displacement()
R = post.reaction_force()
print("Reaction Forces:\n", R)
post.export_to_vtk("beam_results.vtk")

