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

*Need for multiple sensors* <br>
Autonomous vehicles rely on multiple sensors because no single sensor is reliable enough to provide accurate and continuous localization under all driving conditions. Each sensor has complementary strengths and weaknesses, making sensor fusion essential for robust state estimation. This motivates the use of sensor fusion techniques to combine complementary measurements into a single, consistent estimate of the vehicle state.
<table>
  <tr>
    <td align="center">
      <b>Sensor Stack </b><br>
      <img src="Sensor_Stack.png" width="600"/>
    </td>
  </tr>
</table>


*Kalman Filter and ES-EKF* <br>
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

*State Representation* <br>
The system estimates the vehicle state as position, velocity, and orientation (quaternion) in 3D space. An error-state formulation is used in the EKF to model and correct small deviations around this nominal state.
<table>
  <tr>
    <td align="center">
      <img src="Configure_Suspension_Settings.PNG" width="100%"/><br>
      <sub><b>Vehicle & Rider Setup</b>: Set suspension and damping properties to match vehicle dynamics performance.</sub>
    </td>
    <td align="center">
      <img src="Configure_Road_and_Driver.PNG" width="100%"/><br>
      <sub><b>Road & Environment Setup</b>: Configure road profiles, friction levels and driving conditions to match real world.</sub>
    </td>
  </tr>
</table>
