# LU Factorization & Beam Bending in MATLAB

MATLAB lab project (TP) on solving linear systems with **LU factorization**, then applying it to a **beam bending** problem. The first part implements the core numerical routines from scratch (triangular solvers, LU factorization, system solving, matrix inversion). The second part builds a pentadiagonal system that models the deflection of a beam under different loads, solves it, plots the results and measures computation time.

## Contents

### Part 1: LU method

| File | Description |
|------|-------------|
| `descente.m` | Forward substitution: solves `Lx = b` for a lower triangular matrix |
| `remontee.m` | Back substitution: solves `Ux = b` for an upper triangular matrix |
| `factlu.m` | In-place LU factorization without pivoting; `L` (unit diagonal) and `U` are stored in a single matrix |
| `resollu.m` | Solves `Ax = b` using `factlu`, `descente` and `remontee` |
| `inverselu.m` | Computes `A⁻¹` by solving `Ax = eᵢ` for each column of the identity |

### Part 2: Beam bending application

| File | Description |
|------|-------------|
| `remplissage.m` | Builds the `n × n` pentadiagonal matrix with the stencil `[1 -4 6 -4 1]` |
| `resoudre_equire.m` | Deflection under a uniformly distributed load |
| `resoudre_local.m` | Deflection under a point load at a given position |
| `resoudre_poutre.m` | Full version including the beam length `l` and a scaling factor `alpha` (uses `h = l/(n+1)`) |
| `tracer_flexion.m` | Plots the deflection curves for all load cases |
| `mesurer_temps_calcul.m` | Times the solve for point loads at positions 3, 4 and 5 |

### Driver script

`main.m` tests every function on example matrices, then runs the beam application (`l = 10`, `n = 9`).

## Getting started

### Requirements

- MATLAB (any recent version) or [GNU Octave](https://octave.org/)

### Run

1. Clone the repository:
   ```bash
   git clone https://github.com/Malekkk25/matlab-lu-beam-bending.git
   ```
2. Open the folder in MATLAB (or set it as the current folder).
3. Run the main script:
   ```matlab
   main
   ```

You can also use the functions on their own:

```matlab
A = [4 3; 6 3];
b = [10; 12];
x = resollu(A, b);          % solve Ax = b with LU

B = inverselu(A);           % inverse of A
```

## How it works

**LU factorization.** `factlu` overwrites `A` so that the strict lower part holds `L` and the upper part (including the diagonal) holds `U`. You recover them with:

```matlab
A_facto = factlu(A);
U = triu(A_facto);
L = tril(A_facto, -1) + eye(size(A, 1));
```

Solving `Ax = b` then takes two triangular solves: `Ly = b` (`descente`), then `Ux = y` (`remontee`).

**Beam bending.** The beam is split into `n` interior points spaced `h = l / (n + 1)` apart. The fourth-order derivative in the bending equation is approximated by the stencil `[1 -4 6 -4 1]`, which gives a pentadiagonal system `A u = b`. The right-hand side `b` represents either a uniform load or a single point load, and `u` is the resulting deflection at each point.

## Results

Add your plot here once you have run the project:

```
![Beam deflection](images/flexion.png)
```

Save the figure from MATLAB with `saveas(gcf, 'images/flexion.png')`.

## Limitations

- `factlu` does not use pivoting, so it fails if a zero pivot appears on the diagonal.
- Only square matrices are supported (checked with error messages).

## Author

**Malek** ([@Malekkk25](https://github.com/Malekkk25))
