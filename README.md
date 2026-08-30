# Transient Solution of the Diffusion Equation with Non-Homogeneous Boundary Conditions

## Problem Statement

This project presents the analytical solution of the one-dimensional diffusion equation with non-homogeneous Dirichlet boundary conditions using the **steady-state regime method**.

The governing equation is

$$\frac{\partial^2 p}{\partial x^2}=\frac{1}{\alpha}\frac{\partial p}{\partial t}$$

subject to the boundary conditions

$$
p(0,t)=p_1
$$

$$
p(L,t)=p_2
$$

and the initial condition

$$
p(x,0)=\phi(x)
$$

where

$$
\phi(x)=\frac{p_1}{2}+(p_2-p_1)x.
$$

The pressure difference is defined as

$$
\Delta p=p_2-p_1.
$$

The problem is solved by decomposing the pressure into a steady-state and a transient component:

$$
p(x,t)=E(x)+p^*(x,t)
$$

where $E(x)$ represents the steady-state solution and $p^*(x,t)$ represents the transient solution.

---

## Steady-State Solution

The steady-state component satisfies

$$
\frac{d^2E}{dx^2}=0.
$$

Applying the boundary conditions,

$$
E(0)=p_1
$$

and

$$
E(L)=p_2,
$$

the steady-state solution is

$$
E(x)=p_1+\frac{p_2-p_1}{L}x.
$$

Using the definition of $\Delta p$:

$$
E(x)=p_1+\frac{\Delta p}{L}x.
$$

The transient component is therefore

$$
p^*(x,t)=p(x,t)-E(x),
$$

and satisfies homogeneous Dirichlet boundary conditions:

$$
p^*(0,t)=0
$$

$$
p^*(L,t)=0.
$$

---

## Transient Solution

Using separation of variables, the transient solution is expressed as

$$
p^*(x,t)=\sum_{n=1}^{\infty}B_n\sin(\lambda_n x)e^{-\lambda_n^2\alpha t}$$

where the eigenvalues are

$$
\lambda_n=\frac{n\pi}{L}.
$$

The Fourier coefficients are obtained from the initial condition:

$$
B_n=
\frac{2}{L}
\int_0^L
\left[\phi(x)-E(x)\right]
\sin\left(\frac{n\pi x}{L}\right)
\,dx.
$$

Substituting the initial condition and the steady-state solution gives

$$p^*(x,0)=-\frac{p_1}{2}+\Delta p\left(1-\frac{1}{L}\right)x.$$

Therefore, the Fourier coefficients are

$$B_n=\frac{p_1[\cos(n\pi)-1]-2\Delta p(L-1)\cos(n\pi)}{n\pi}.$$

Since

$$
\Delta p=p_2-p_1,
$$

the coefficients can also be written as

$$B_n=\frac{p_1[\cos(n\pi)-1]-2(p_2-p_1)(L-1)\cos(n\pi)}{n\pi}.$$


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

## Technologies

**Language:** Python

**Environment:** JupyterLab

**Libraries:** NumPy, Pandas, Matplotlib
