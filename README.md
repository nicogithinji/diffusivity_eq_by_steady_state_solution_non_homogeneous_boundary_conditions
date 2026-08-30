# Transient Solution of the Diffusion Equation with Non-Homogeneous Boundary Conditions

## Overview

This project presents the analytical solution of the one-dimensional diffusion equation with **non-homogeneous Dirichlet boundary conditions** using the **steady-state regime method**.

The solution is decomposed into a steady-state component and a transient component. The transient component is obtained using the **separation of variables method** and is represented as a Fourier sine series.

The Python implementation was developed in **JupyterLab** and evaluates the transient pressure solution for multiple pressure differences.

---

## Governing Equation

The one-dimensional diffusion equation is given by

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

The parameters used in this implementation are:

$$
\alpha = 1\; \text{cm}^2/\text{s}
$$

$$
L = 1\; \text{cm}
$$

$$
t = 1\; \text{s}
$$

where $L$ is the length of the spatial domain.

---

## Boundary Conditions

The problem is subject to non-homogeneous Dirichlet boundary conditions:

$$
p(0,t)=p_1
$$

$$
p(L,t)=p_2
$$

where $p_1$ and $p_2$ are the prescribed pressures at the boundaries.

The pressure difference is defined in the mathematical formulation as

$$
\boxed{\Delta p=p_2-p_1}
$$

---

## Initial Condition

The initial pressure distribution is defined as

$$
p(x,0)=\phi(x)
$$

with

$$
\boxed{
\phi(x)=\frac{p_1}{2}+(p_2-p_1)x
}
$$

---

## Steady-State Regime Method

Because the boundary conditions are non-homogeneous, the pressure is decomposed into a steady-state and a transient component:

$$
p(x,t)=E(x)+u(x,t)
$$

where $E(x)$ is the steady-state solution and $u(x,t)$ is the transient solution.

At steady state,

$$
\frac{d^2E}{dx^2}=0.
$$

Therefore,

$$
E(x)=C_1x+C_2.
$$

Applying the boundary conditions,

$$
E(0)=p_1
$$

and

$$
E(L)=p_2,
$$

gives

$$
\boxed{
E(x)=p_1+\frac{p_2-p_1}{L}x
}
$$

or, using $\Delta p$,

$$
\boxed{
E(x)=p_1+\frac{\Delta p}{L}x
}
$$

The transient component therefore satisfies homogeneous Dirichlet boundary conditions:

$$
u(0,t)=0
$$

$$
u(L,t)=0.
$$

---

## Transient Solution

Using separation of variables, the transient solution is expressed as a Fourier sine series:

$$
u(x,t)=
\sum_{n=1}^{\infty}
B_n
\sin\left(\frac{n\pi x}{L}\right)
e^{-\alpha\left(\frac{n\pi}{L}\right)^2t}.
$$

The eigenvalues are

$$
\boxed{
\lambda_n=\frac{n\pi}{L}
}
$$

so the solution can also be written as

$$
u(x,t)=
\sum_{n=1}^{\infty}
B_n
\sin(\lambda_nx)
e^{-\lambda_n^2\alpha t}.
$$

The Fourier coefficients are calculated from the initial condition of the transient component:

$$
u(x,0)=\phi(x)-E(x).
$$

Therefore,

$$
B_n=
\frac{2}{L}
\int_0^L
[\phi(x)-E(x)]
\sin\left(\frac{n\pi x}{L}\right)dx.
$$

Using the specified initial condition,

$$
\phi(x)=\frac{p_1}{2}+\Delta p\,x,
$$

and

$$
E(x)=p_1+\frac{\Delta p}{L}x,
$$

gives

$$
u(x,0)=
-\frac{p_1}{2}
+
\Delta p
\left(1-\frac{1}{L}\right)x.
$$

After evaluating the integral, the Fourier coefficient becomes

$$
\boxed{
B_n=
\frac{
p_1[\cos(n\pi)-1]
-
2\Delta p(L-1)\cos(n\pi)
}{n\pi}
}
$$

where

$$
\Delta p=p_2-p_1.
$$

Equivalently, directly in terms of $p_1$ and $p_2$:

$$
\boxed{
B_n=
\frac{
p_1[\cos(n\pi)-1]
-
2(p_2-p_1)(L-1)\cos(n\pi)
}{n\pi}
}
$$

---

## Numerical Implementation

The analytical solution is implemented in Python using a finite number of terms from the Fourier series.

The number of terms can be controlled using:

```python
terminos = 5
```

The eigenvalues are calculated using:

```python
def lambda_n(n):
    lam = n * np.pi / L
    return lam
```

The Fourier coefficients are calculated using:

```python
def Bn(n, p1, p2):
    B = (p1 * (np.cos(n*np.pi) - 1)
         - 2*(p2-p1)*(L-1)*np.cos(n*np.pi)) / (n*np.pi)
    return B
```

The transient solution is evaluated as:

```python
def p(x, t, p1, p2):
    sumatoria = 0
    for i in range(1, terminos + 1):
        sumatoria += Bn(i, p1, p2) \
            * np.sin(lambda_n(i) * x) \
            * math.exp(-(lambda_n(i)**2 * difusividad * t))
    return sumatoria
```

---

## Pressure Difference Data

Seven pairs of boundary pressures $(p_1,p_2)$ are generated using a fixed random seed.

The pressure values are generated such that

$$
p_2<p_1.
$$

Therefore, according to the mathematical definition,

$$
\Delta p=p_2-p_1<0.
$$

However, for visualization and presentation purposes, the table displays the **magnitude** of the pressure difference:

$$
|\Delta p|=p_1-p_2.
$$

This positive value is used only for displaying and sorting the data. The actual analytical solution and the calculation of $B_n$ use

$$
\Delta p=p_2-p_1.
$$

---

## Results and Visualization

For each pair of boundary pressures, the transient solution is evaluated at 1000 spatial points over the domain:

$$
0\leq x\leq L.
$$

The current implementation evaluates the solution at

$$
t=1\;\text{s}
$$

using the first five terms of the Fourier series.

The resulting plot shows:

* **x-axis:** spatial coordinate $x$ in cm.
* **y-axis:** transient pressure $p^*(x,t)$ in dyn/cm².
* **Each curve:** a different pressure difference between the two boundaries.

Only the **transient component** is plotted. Therefore, the plotted quantity represents $u(x,t)$ rather than the complete pressure solution

$$
p(x,t)=E(x)+u(x,t).
$$

The steady-state component is not included in the plotted curves.

---

## Parameters

| Parameter            | Value | Units |
| -------------------- | ----: | ----- |
| Diffusivity $\alpha$ |     1 | cm²/s |
| Domain length $L$    |     1 | cm    |
| Evaluation time $t$  |     1 | s     |
| Fourier terms        |     5 | —     |
| Pressure pairs       |     7 | —     |
| Spatial points       |  1000 | —     |

---

## Dependencies

The project requires Python and the following libraries:

* NumPy
* Pandas
* Matplotlib
* Math

They can be installed using:

```bash
pip install numpy pandas matplotlib
```

---

## How to Run

1. Install Python and JupyterLab.
2. Install the required dependencies.
3. Open the `.ipynb` notebook in JupyterLab.
4. Run the cells sequentially.
5. Modify the input parameters if different physical or numerical conditions are required.

---

## Possible Improvements

Some possible extensions of the project are:

* Increase the number of Fourier terms to study convergence.
* Evaluate the solution at different times.
* Allow arbitrary initial conditions $\phi(x)$.
* Plot the complete pressure solution $p(x,t)$.
* Compare the analytical solution with a finite-difference numerical solution.
* Study the influence of diffusivity and domain length on the transient response.
* Improve the visualization by comparing different pressure differences in a systematic parameter study.

---

## Technologies

**Language:** Python

**Environment:** JupyterLab

**Libraries:** NumPy, Pandas, Matplotlib
