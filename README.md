# fenicsx-3d-cable-dynamics
Geometrically nonlinear 3D cable dynamics in FEniCSx with tension-only behavior, circular boundary excitation, Newmark integration, and ParaView visualization.

## Main features

- One-dimensional cable embedded in three-dimensional space
- Geometrically nonlinear kinematics
- Tension-only constitutive behavior
- Circular endpoint excitation with a smooth ramp
- Newmark time integration
- PETSc nonlinear solution through FEniCSx
- Midpoint-displacement history
- Maximum stretch and tension histories
- Time-dependent `.pvd/.vtu` output for ParaView
- Google Colab-compatible notebook

## Cable formulation

The current cable position is

```math
\mathbf{r}(X,t)
=
\begin{bmatrix}
X \\
0 \\
0
\end{bmatrix}
+
\mathbf{u}(X,t)
```


where \(X\) is the reference coordinate and \(\mathbf u\) is the three-dimensional displacement.

The stretch ratio is

$$
\lambda
=
\left\|
\frac{\partial \mathbf{r}}{\partial X}
\right\|.
$$

The elastic tension-only constitutive law is

$$
T =
EA\max(\lambda-1,0).
$$

An axial viscous contribution is included when the cable is stretched. The governing dynamic equilibrium is solved through a nonlinear finite-element weak formulation.

## Reference parameters

| Parameter | Value |
|---|---:|
| Cable length | \(1.0\ \mathrm{m}\) |
| Number of nodes | 31 |
| Young’s modulus | \(4.456\times10^8\ \mathrm{Pa}\) |
| Cable diameter | \(1.0\times10^{-3}\ \mathrm{m}\) |
| Density | \(970\ \mathrm{kg/m^3}\) |
| Excitation amplitude | \(0.03L\) |
| Excitation frequency | \(1\ \mathrm{Hz}\) |
| Ramp duration | \(1.0\ \mathrm{s}\) |

## Run in Google Colab

Click the **Open in Colab** badge at the top of this README.

Run the notebook cells sequentially. The first installation cell uses FEM-on-Colab to prepare the FEniCSx environment.

For an initial short test, use:

```python
Tend = 0.25
dt = 2.0e-4
```

For a more developed response, use:

```python
Tend = 2.0
dt = 2.0e-4
```

Longer simulations and smaller time steps require significantly more computational time because a nonlinear problem is solved at every time step.

## Results

The notebook calculates:

- midpoint displacement in the \(y\)- and \(z\)-directions;
- maximum axial strain;
- maximum elastic tension;
- time-dependent displacement fields for visualization.

## ParaView visualization

After the simulation:

1. Download and extract `fenicsx_cable_vtk.zip`.
2. Open `fenicsx_cable_motion.pvd` in ParaView.
3. Select **Warp By Vector**.
4. Set `Displacement` as the vector field.
5. Use scale factor `1` for physical deformation or a larger value for visualization.
6. Apply a **Tube** filter to give the cable a visible diameter.
7. Press **Play** to view the motion.

Generated `.pvd`, `.pvtu`, and `.vtu` files should remain in the same directory.

## Repository contents

```text
fenicsx_cable_dynamics.ipynb  FEniCSx model and post-processing
README.md                     Model documentation
LICENSE                       MIT license
```

## Important modelling note

This repository contains a native geometrically nonlinear FEniCSx cable formulation. It is not a discrete mass–spring–damper implementation. FEniCSx uses a finite-element weak formulation and a consistent mass representation.

## Author

**Mohammad Bigdeli**

Research interests: computational mechanics, nonlinear dynamics, finite-element modelling, flexible structures, and space-debris capture systems.

## License

This project is distributed under the MIT License. See [LICENSE](LICENSE) for details.
