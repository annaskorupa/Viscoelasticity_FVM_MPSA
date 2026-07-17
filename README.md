# Viscoelasticity FVM MPSA

This repository contains an implementation of a viscoelastic material model extending the standard momentum balance module (`MomentumBalance`) in the **PorePy** library. 

While the original model in PorePy is based on Hooke's law (purely elastic material), this project introduces **viscoelastic** behavior. This results in the introduction of a second displacement component (`u2`), which represents the viscous part. The final displacement in such a material is time-dependent (creep behavior).

## Project Structure

The codebase strictly follows PorePy's architecture, based on the **Mixin Module** (multiple inheritance) design pattern. The functionality for the `u2` variable is divided into logical blocks (creating variables, equations, constitutive laws, initial conditions, solution strategies).

*   `run_simulation.py`: Unified simulation runner for viscoelastic creep analysis. It can be run in different modes:
    *   `1D`: Quasi-1D PorePy simulation (uniaxial creep)
    *   `2D`: 2D PorePy simulation without fracture
    *   `2D_frac`: 2D PorePy simulation with fracture
*   `run_convergence.py`: Convergence testing script.
*   `config.py`: Centralized configuration for material parameters (Young's moduli, Poisson's ratio, viscosity) and time stepping.
*   `src/viscoelastic_porepy/`: Core modules extending PorePy, including custom variables, mechanical stresses, and solvers for the `u2` component.
*   `Dockerfile`: Provides an isolated environment with all necessary dependencies installed.

## Getting Started

### Using Docker (Recommended)

An isolated Docker environment is provided to run the simulations without manually installing dependencies.

1. **Build the Docker image:**
   ```bash
   docker build -t porepy-viscoelastic .
   ```

2. **Run a simulation:**
   ```bash
   # 1D simulation
   docker run --rm -v "${PWD}/_output:/app/_output" porepy-viscoelastic python run_simulation.py 1D
   
   # 2D simulation without fracture
   docker run --rm -v "${PWD}/_output:/app/_output" porepy-viscoelastic python run_simulation.py 2D
   
   # 2D simulation with fracture
   docker run --rm -v "${PWD}/_output:/app/_output" porepy-viscoelastic python run_simulation.py 2D_frac
   ```

Simulation results and plots will be saved in the `_output/` directory, which is synchronized with your host machine.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
