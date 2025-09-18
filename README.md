# Multi-Sensor-Fusion-BRUEnKF

## Abstract

The project simulates an autonomous system tracking a 2D trajectory using asynchronous and noisy data from three sensors: GPS, IMU, and a biased camera. The core investigation is whether the BRUEnKF framework provides more robust state estimation and uncertainty quantification compared to a standard EnKF when faced with non-Gaussian (biased) and asynchronous sensor data.

## Key Features

- **Simulation of Ground Truth**: A 2D constant-velocity dynamical system with process noise.
- **Multi-Sensor Simulation**:
  - **GPS**: Noisy position measurements at 1 Hz.
  - **IMU**: Noisy velocity measurements at 10 Hz.
  - **Camera**: Noisy and biased position measurements at 2 Hz.
- **Asynchronous & Missing Data**: Realistic random dropout of sensor readings.
- **Filter Implementations**:
  - **Standard Ensemble Kalman Filter (EnKF)**
  - **Bayesian Recursive Update EnKF (BRUEnKF)**: Implements Algorithm 1 from Popov & Sandu (2024) with `m` recursive micro-updates.
- **Comprehensive Analysis**: Quantitative (RMSE) and qualitative (trajectory, error, uncertainty plots) comparison of filter performance.

## Results

The BRUEnKF demonstrated superior performance in handling sensor bias and asynchronous data:

| Filter | RMSE |
| :--- | :--- |
| Standard EnKF | 0.2716 |
| **BRUEnKF** | **0.2195** |

**Key Findings:**
1.  **Improved Accuracy:** The BRUEnKF achieved a ~19% lower RMSE than the standard EnKF.
2.  **Robustness to Bias:** The BRUEnKF's trajectory was less affected by the biased camera sensor.
3.  **Better Uncertainty Calibration:** The BRUEnKF maintained slightly higher, and potentially more realistic, uncertainty estimates, preventing overconfidence.

![Trajectory Comparison](images/trajectory_plot.png)
*Example visualization showing the true trajectory, filter estimates, and sensor readings.*

## Implementation Details

### Core Algorithms
- **`enkf_predict()`**: Prediction step for the ensemble.
- **`enkf_update()`**: Standard update step using perturbed observations.
- **`bruenkf_update()`**: Implementation of the BRUEnKF algorithm, performing `m` micro-updates with inflated measurement noise `R_m = R * m` for each sensor measurement.

### Technical Stack
- **Language**: Python 3
- **Libraries**:
  - `NumPy` for numerical computations and linear algebra.
  - `Pandas` for data handling and storage.
  - `Matplotlib` for visualization and plotting.
  - `SciPy` for statistical operations.
