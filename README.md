⭐ **1. Introduction**

This project focuses on the implementation of a state estimation system for autonomous vehicles by fusing measurements from multiple onboard sensors. The primary objective is to accurately estimate the vehicle’s pose and velocity in real time under noisy and uncertain conditions.

To achieve robust localization, the system integrates data from GNSS, IMU, and LiDAR sensors. Each sensor contributes complementary information: GNSS provides global position measurements, IMU delivers high-frequency inertial data, and LiDAR aids in correcting drift and improving spatial awareness through environmental observations.

---

🧩 **2. Challenge**

This project addresses the challenges of accurate state estimation and localization for autonomous vehicles using multi-sensor fusion.

Key challenges include:

- Sensor failures and degraded measurements (e.g., GNSS dropouts in tunnels or urban canyons), leading to loss of absolute positioning information.  
- Growing state uncertainty over time, especially when relying heavily on IMU integration without external corrections.  
- Multi-rate and asynchronous sensor fusion, where IMU (~200 Hz) and LiDAR (~10–20 Hz) operate at different frequencies, requiring precise temporal alignment.  
- Accuracy constraints for safe autonomous driving, where even small localization errors can affect lane-level positioning and decision-making.  

---

