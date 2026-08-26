# 3D Mass–Spring–Damper Cable Dynamics

A three-dimensional discrete mass–spring–damper cable model implemented in Python, prescribed circular endpoint excitation, explicit time integration, and ParaView visualization. The model is intended for comparison with geometrically nonlinear cable formulations developed using FEniCSx.

## Main features

- Three-dimensional cable motion
- Lumped nodal masses
- Discrete axial springs and dampers
- Tension-only cable behavior
- Smoothly ramped circular endpoint excitation
- Symplectic Euler time integration
- Midpoint-displacement histories
- Maximum stretch and tension histories
- Time-dependent `.pvd/.vtu` output
- ParaView animation
- Google Colab-compatible notebook

## Discrete cable model

The cable is divided into \(N-1\) segments connecting \(N\) lumped nodes.

For segment \(e\), the current segment vector and length are

```math
\mathbf{d}_e
=
\mathbf{r}_{e+1}-\mathbf{r}_e,
\qquad
l_e
=
\left\|\mathbf{d}_e\right\|.
```

The corresponding unit direction vector is

```math
\widehat{\mathbf{d}}_e
=
\frac{\mathbf{d}_e}{l_e}.
```

The unstretched segment length is

```math
l_0 = \frac{L}{N-1}.
```

The segment stiffness is determined from the cable material and geometry:

```math
k_{\mathrm{seg}}
=
\frac{EA}{l_0}.
```

The stretch ratio of each segment is

```math
\lambda_e
=
\frac{l_e}{l_0}.
```

## Tension-only spring–damper law

The segment-length rate is calculated from the relative nodal velocity:

```math
\dot{l}_e
=
\left(
\mathbf{v}_{e+1}-\mathbf{v}_e
\right)
\cdot
\widehat{\mathbf{d}}_e.
```

The tension-only spring–damper force is

```math
T_e
=
\begin{cases}
\max\left[
k_{\mathrm{seg}}(l_e-l_0)
+
c_{\mathrm{edge}}\dot{l}_e,\,
0
\right],
&
l_e>l_0,
\\
0,
&
l_e\leq l_0.
\end{cases}
```

The force vector transmitted by the segment is

```math
\mathbf{f}_e
=
T_e\widehat{\mathbf{d}}_e.
```

Therefore, the cable can transmit tensile forces but cannot resist compression.

## Lumped nodal mass

The mass of each cable segment is

```math
m_e = \rho A l_0.
```

Half of this mass is assigned to each endpoint of the segment. Consequently, each internal node receives half the mass of both adjacent segments.

## Boundary conditions

The left endpoint is fixed:

```math
\mathbf{r}_1(t)
=
\begin{bmatrix}
0\\
0\\
0
\end{bmatrix}.
```

The right endpoint follows a prescribed circular trajectory:

```math
\mathbf{r}_N(t)
=
\begin{bmatrix}
L\\
a\,R(t)\cos(\omega t)\\
a\,R(t)\sin(\omega t)
\end{bmatrix},
```

where \(a\) is the excitation amplitude and \(R(t)\) is a smooth ramp:

```math
R(t)
=
\begin{cases}
\dfrac{1}{2}
\left[
1-\cos\left(\dfrac{\pi t}{T_{\mathrm{ramp}}}\right)
\right],
&
t\leq T_{\mathrm{ramp}},
\\
1,
&
t>T_{\mathrm{ramp}}.
\end{cases}
```

## Reference parameters

| Parameter | Value |
|---|---:|
| Cable length | $1.0\ \mathrm{m}$ |
| Number of nodes | 31 |
| Number of segments | 30 |
| Young’s modulus | $4.456\times10^8\ \mathrm{Pa}$ |
| Cable diameter | $1.0\times10^{-3}\ \mathrm{m}$ |
| Density | $970\ \mathrm{kg/m^3}$ |
| Excitation amplitude | $0.03L$ |
| Excitation frequency | $1\ \mathrm{Hz}$ |
| Ramp duration | $1.0\ \mathrm{s}$ |
| Time step | $2.0\times10^{-5}\ \mathrm{s}$ |

## Running the model

Click the **Open in Colab** badge at the top of this README and run the notebook cells sequentially.

The model requires:

```text
NumPy
Matplotlib
```

FEniCSx, PETSc, and MPI are not required because this is a discrete mass–spring–damper implementation.

For a short initial test, use:

```python
Tend = 0.25
dt = 2.0e-5
```

For a more developed response, use:

```python
Tend = 2.0
dt = 2.0e-5
```

Because the model uses explicit integration, the time step must remain sufficiently small for numerical stability.

## Results

The notebook calculates and plots:

- midpoint displacement in the \(y\)-direction;
- midpoint displacement in the \(z\)-direction;
- maximum segment stretch ratio;
- maximum cable tension.

The numerical histories are saved in:

```text
mass_spring_cable_results.npz
```

## ParaView visualization

After completing the simulation:

1. Download `mass_spring_cable_vtk.zip`.
2. Extract all files into the same directory.
3. Open `mass_spring_cable_motion.pvd` in ParaView.
4. Click **Apply**.
5. Apply the **Tube** filter to make the cable visible.
6. Select a suitable tube radius.
7. Press **Play** to view the cable motion.

The VTU files already contain the deformed nodal coordinates. Therefore, **Warp By Vector is not required**.

The `Displacement`, `Velocity`, and segment `Tension` fields are also available for coloring and post-processing.

## Repository contents

```text
mass_spring_damper_cable.ipynb  Model, simulation and post-processing
README.md                       Model documentation
LICENSE                         MIT license
.gitignore                      Excluded generated files
```

## Modelling note

This repository implements a discrete mass–spring–damper cable model. It is not a continuous elastic-string finite-element formulation.

The cable is represented by lumped nodal masses connected by tension-only axial spring–damper elements.

## Author

**Mohammad Bigdeli**

Research interests include computational mechanics, nonlinear dynamics, finite-element modelling, flexible structures, and space-debris capture systems.

## License

This project is distributed under the MIT License. See [LICENSE](LICENSE) for details.
