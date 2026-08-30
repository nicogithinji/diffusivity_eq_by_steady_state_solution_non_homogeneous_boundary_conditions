# Diffusion Equation with Non-Homogeneous Dirichlet Boundary Conditions

## Project Overview

This project presents the solution of the one-dimensional diffusion equation with non-homogeneous Dirichlet boundary conditions using the **steady-state regime method**.

The transient solution is obtained by decomposing the pressure field into a steady-state component and a transient component. The transient component is then solved using the method of separation of variables and represented as a Fourier sine series.

The Python implementation was developed in **JupyterLab** and evaluates the transient pressure solution for multiple pressure differences, $\Delta p$.

---

## Governing Equation

The one-dimensional diffusion equation is written as

$$
\frac{\partial^2 p}{\partial x^2}
=
\frac{1}{\alpha}
\frac{\partial p}{\partial t}
$$

where:

* $p$ is pressure,
* $x$ is the spatial coordinate,
* $t$ is time,
* $\alpha$ is the diffusivity.

In this implementation:

$$
\alpha = 1\; \text{cm}^2/\text{s}
$$

and the domain is

$$
0 \leq x \leq L
$$

with

$$
L = 1\; \text{cm}.
$$

---

## Boundary Conditions

The pressure is prescribed at both boundaries using Dirichlet boundary conditions:

$$
p(0,t) = p_1
$$

$$
p(L,t) = p_2
$$

where $p_1$ and $p_2$ are the pressures at the two boundaries.

Since the boundary conditions are non-homogeneous, the pressure solution is decomposed as

$$
p(x,t) = E(x) + u(x,t)
$$

where:

* $E(x)$ is the steady-state solution,
* $u(x,t)$ is the transient solution.

The steady-state component satisfies the boundary conditions, allowing the transient problem to be formulated with homogeneous boundary conditions.

---

## Initial Condition

The pressure distribution at $t=0$ is defined by the initial condition

$$
p(x,0)=\phi(x).
$$

The initial condition is used to determine the Fourier coefficients of the transient solution.

After subtracting the steady-state solution, the corresponding initial condition for the transient component is

$$
u(x,0)=\phi(x)-E(x).
$$

---

## Transient Solution

For homogeneous Dirichlet boundary conditions, the transient solution can be expressed as a Fourier sine series:

$$
u(x,t)
=
\sum_{n=1}^{\infty}
B_n
\sin(\lambda_n x)
e^{-\lambda_n^2\alpha t}
$$

where the eigenvalues are

$$
\lambda_n = \frac{n\pi}{L}.
$$

The Fourier coefficients are obtained from the initial condition:

$$
B_n =
\frac{2}{L}
\int_0^L
u(x,0)
\sin\left(\frac{n\pi x}{L}\right)
dx.
$$

In the numerical implementation, the infinite series is truncated to a finite number of terms.

---

## Numerical Implementation

The solution is implemented in Python using:

* **NumPy** for numerical calculations,
* **Pandas** for handling the pressure data,
* **Matplotlib** for visualization,
* **Math** for exponential calculations.

The main parameters used in the notebook are:

| Parameter                | Value | Units |
| ------------------------ | ----: | ----- |
| Diffusivity $\alpha$     |     1 | cm²/s |
| Domain length $L$        |     1 | cm    |
| Time $t$                 |     1 | s     |
| Number of terms          |     5 | —     |
| Number of pressure pairs |     7 | —     |

---

## Pressure Data Generation

The code generates seven pairs of boundary pressures $(p_1,p_2)$ using a fixed random seed.

For each pair:

$$
\Delta p = p_1-p_2
$$

is calculated.

The resulting pressure differences are sorted before plotting the solutions.

This allows the effect of different pressure differences on the transient solution to be investigated.

---

## Visualization

For each pressure pair, the code evaluates the transient solution over 1000 spatial points:

$$
x \in [0,L].
$$

The resulting plot shows:

* **x-axis:** spatial position $x$ in cm,
* **y-axis:** transient pressure solution $p^*(x,t)$,
* **different curves:** different values of $\Delta p$.

The current implementation evaluates the solution at

$$
t=1\;\text{s}.
$$

Only the **transient component** of the solution is plotted.

Therefore, the plotted quantity is not the complete pressure field $p(x,t)$, but the transient contribution resulting from the Fourier-series solution.

---

## Code Structure

The notebook is organized into four main sections:

### 1. Imports

The required Python libraries are imported.

### 2. Input Data

The physical and numerical parameters are defined:

```python
difusividad = 1
t = 1
L = 1
terminos = 5
muestras = 7
```

### 3. Solution Functions

The notebook defines functions for:

* calculating the eigenvalues $\lambda_n$,
* calculating the Fourier coefficients $B_n$,
* evaluating the transient pressure solution.

### 4. Results

The transient solution is evaluated for each pressure pair and plotted as a function of $x$.

---

## How to Run

1. Install Python and JupyterLab.
2. Install the required packages:

```bash
pip install numpy pandas matplotlib
```

3. Open the notebook in JupyterLab.
4. Run the cells sequentially.
5. Modify the input parameters if different diffusivity, domain length, time, number of terms, or pressure differences are required.

---

## Future Improvements

Possible improvements to the implementation include:

* Increasing the number of terms in the Fourier series.
* Allowing arbitrary initial pressure distributions $\phi(x)$.
* Comparing the analytical solution with a numerical finite-difference solution.
* Plotting the complete pressure solution $p(x,t)$ instead of only the transient component.
* Evaluating the solution at multiple times.
* Investigating the convergence of the Fourier series as the number of terms increases.

---

## Technologies

* Python
* NumPy
* Pandas
* Matplotlib
* JupyterLab
