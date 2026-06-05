⭐ **1. Introduction**

This project focuses on the implementation of a state estimation system for autonomous vehicles by fusing measurements from multiple onboard sensors. The primary objective is to accurately estimate the vehicle’s pose and velocity in real time under noisy and uncertain conditions.

To achieve robust localization, the system integrates data from GNSS, IMU, and LiDAR sensors. Each sensor contributes complementary information: GNSS provides global position measurements, IMU delivers high-frequency inertial data, and LiDAR aids in correcting drift and improving spatial awareness through environmental observations.

<table>
  <tr>
    <td align="center">
      <b></b><br>
      <img src="Intro_GIF.gif" width="700"/>
    </td>
  </tr>
</table>

---

🧩 **2. Challenge**

This project addresses the challenges of accurate state estimation and localization for autonomous vehicles using multi-sensor fusion.

Key challenges include:

- Sensor failures and degraded measurements (e.g., GNSS dropouts in tunnels or urban canyons), leading to loss of absolute positioning information.  
- Growing state uncertainty over time, especially when relying heavily on IMU integration without external corrections.  
- Multi-rate and asynchronous sensor fusion, where IMU (~200 Hz) and LiDAR (~10–20 Hz) operate at different frequencies, requiring precise temporal alignment.  
- Accuracy constraints for safe autonomous driving, where even small localization errors can affect lane-level positioning and decision-making.  

---

🎯 **3. Objectives**

- Develop a multi-sensor state estimation framework to accurately estimate vehicle pose and velocity in real time  
- Design an Error-State EKF to fuse high-frequency IMU data with GNSS and LiDAR measurements  
- Improve localization robustness by handling sensor noise, failures, and asynchronous measurement updates  
- Evaluate estimator performance through trajectory reconstruction and comparison against ground truth data

---

🛠 **4. Tech Stack**

The key methods used in this rpoject include:

- CARLA Simulator – autonomous driving simulation environment
- Python – core implementation of the ES-EKF sensor fusion pipeline  
- NumPy – numerical computations for state propagation, covariance updates, and linear algebra  
- Matplotlib – visualization of trajectories, estimation error, and uncertainty bounds  
- GNSS / IMU / LiDAR data – multi-sensor inputs for real-world vehicle localization  
- Quaternion-based math utilities – orientation representation and update operations  
- Kalman Filter framework (ES-EKF) – probabilistic state estimation and sensor fusion method

---

🧠 **5. Key Concepts & Implementation**

*(A) Need for multiple sensors* <br>
Autonomous vehicles rely on multiple sensors because no single sensor is reliable enough to provide accurate and continuous localization under all driving conditions. Each sensor has complementary strengths and weaknesses, making sensor fusion essential for robust state estimation. This motivates the use of sensor fusion techniques to combine complementary measurements into a single, consistent estimate of the vehicle state.
<table>
  <tr>
    <td align="center">
      <b>Sensor Stack </b><br>
      <img src="Sensor_Stack.png" width="600"/>
    </td>
  </tr>
</table>


*(B) Kalman Filter and ES-EKF* <br>
This project uses an Error-State Extended Kalman Filter (ES-EKF) to recursively estimate the vehicle’s state by combining a motion model with noisy sensor measurements. The filter operates in two steps: a prediction step driven by IMU data, and a correction step using GNSS and LiDAR observations.

The ES-EKF formulation improves numerical stability by estimating small error states around a nominal trajectory, making it well-suited for highly non-linear vehicle dynamics and real-world sensor noise.

<table>
  <tr>
    <td align="center">
      <b>Estimation Workflow Setup</b><br>
      <img src="Kalman_Filter.png" width="600"/>
    </td>
  </tr>
</table>

*(C) Motion Model - State Representation* <br>
The system tracks the vehicle’s position, velocity, and orientation (using quaternions) in 3D space. Instead of directly estimating large changes in these values, the ES-EKF focuses on estimating small errors around a predicted motion, which are then used to continuously refine the state for better accuracy and stability.

A more detailed derivation of the ES-EKF formulation is provided in the references section.

<table>
  <tr>
    <td align="center">
      <img src="Motion_Model.png" width="100%"/><br>
      <sub><b></b> Motion Model</sub>
    </td>
    <td align="center">
      <img src="Error_Model.png" width="100%"/><br>
      <sub><b></b> Error Model</sub>
    </td>
  </tr>
</table>

*(D) Position Observation (GNSS & LiDAR)* <br>

In this project, both GNSS and LiDAR are modeled as direct noisy observations of the vehicle’s position in the inertial frame. Each measurement is assumed to be corrupted by additive Gaussian noise, characterized by a sensor-specific covariance.

The GNSS and LiDAR covariances define how much trust is placed in each sensor during the correction step of the ES-EKF.

*(E) EstimationLoop*

The ES-EKF operates in a continuous loop, alternating between prediction using IMU data and correction using GNSS/LiDAR measurements.

```mermaid
flowchart TD

A[IMU Measurement] --> B[Prediction Step]
B --> C[State Propagation]
C --> D[Covariance Propagation]

D --> E{GNSS / LiDAR Available?}

E -- No --> A

E -- Yes --> F[Measurement Update]
F --> G[Kalman Gain Computation]
G --> H[State Correction]
H --> I[Covariance Update]

I --> A
```

NOTE: In the chart above, **State propagation** refers to the step where the vehicle state (position, velocity, and orientation) is predicted forward in time using the motion model and IMU measurements.  

**Covariance propagation** is the step where the uncertainty of the predicted state is updated based on the motion model and process noise.

***
📈 **6. Simulation Results**


