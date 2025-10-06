<center>

# **"Sharkmageddon"**
### **NASA Space Apps Challenge**

</center>

---

#### **Team Members:**
- Isaac Yael Cantú Fernández
- Noé Garza Santos
- Bernardo Servin Daniel
- Benjamin Charlesl Legorreta
- Juan Ángel Candelaria Rodríguez

---

### **Project Overview**

"Sharkmageddon" is a data-driven project aiming to predict the probability of shark presence in specific ocean regions. Using environmental datasets, satellite-derived measurements, and historical shark tracking data, we generate probabilistic models to estimate where sharks are most likely to be. The project combines Monte Carlo simulations, Markov chains, and probabilistic neural networks (PNN) to simulate shark movements and predict potential areas of presence. These predictions are intended to enhance both scientific understanding and real-time monitoring using sensor-equipped buoys and tagged sharks.

*Insert here: `project_overview.png` — A conceptual diagram of Sharkmageddon workflow.*

---

### **Datasets Used**

We incorporated multiple global and oceanographic datasets to build a comprehensive model:

- **MODIS Ocean Color and SST Products**  
  - Chlorophyll concentration  
  - Phytoplankton absorption at 443 nm  
  [MODIS Data Portal](http://oceancolor.gsfc.nasa.gov/)

- **SWOT Mission Data Products**  
  - Simulated sea surface height (SSH) from ECCO LLC4320  
  - SWOT Level 2 radiometer brightness temperatures (troposphere corrections)  
  - Nadir altimeter geophysical data records (wave height, sea surface height, wind)  
  - Low-rate KaRIn SSH measurements  
  [SWOT Data Portal](https://podaac.jpl.nasa.gov/SWOT)

- **OCEARCH Shark Tracking**  
  - Real-time GPS positions and movements of tagged sharks

- **Sea Surface Temperature**  
  - Microsoft Planetary dataset

*Insert here: `datasets_summary.png` — Overview table or visualization of datasets.*

---

### **Methodology**

Our methodology integrates environmental variables, shark tracking, and probabilistic modeling:

1. **Data Preprocessing**  
   - Import all relevant datasets.  
   - Filter and clean data, retaining latitude, longitude, chlorophyll concentration (CHL), phytoplankton absorption (IOP), temperature (Temp), and timestamped shark positions.

2. **Grid Generation**  
   - Divide the study area (lat: 5–45°, lon: -100 to -45°) into regular grids (e.g., 20×20 cells, each ~10×10 km).  
   - Calculate average environmental features per grid, including CHL, IOP, Temp, and ocean current directions.


3. **Monte Carlo Simulation**  
   - Generate random points over water regions only, avoiding land.  
   - Simulate potential shark positions using Markov chains and observed transition probabilities between grid cells.  
   - For each point, associate environmental features based on grid averages.  

<img width="794" height="653" alt="image" src="https://github.com/user-attachments/assets/ab66121f-e3ab-4b77-8a41-40d617f34968" />


4. **Probabilistic Modeling**  
   - Combine real shark tracking data and Monte Carlo-generated points.  
   - Scale all features using MinMaxScaler for consistency.  
   - Use Kernel Density Estimation (KDE) for positive (shark) and negative (non-shark) points to estimate probability densities.

5. **Probability Estimation on Grids**  
   - Evaluate KDE on the regular lat-lon grid to obtain a probability of shark presence per cell.  
   - Apply corrections to consider water-only cells, ignoring land areas.  

<img width="910" height="651" alt="image" src="https://github.com/user-attachments/assets/45900eff-f079-473b-8d2f-5c6b150d747e" />


6. **Probabilistic Neural Network (PNN) Integration**  
   - Train a PNN using KDE-derived probabilities and environmental features.  
   - Estimate the probability and standard deviation of shark presence per cell.  
   - Monte Carlo or particle filter simulations can then generate potential shark trajectories for analysis.

![Imagen de WhatsApp 2025-10-05 a las 18 49 37_2ee4667d](https://github.com/user-attachments/assets/6e6b7696-9a57-4cfa-8470-061756bb2d4f)


7. **Model Validation**  
   - Compare model predictions with OCEARCH tracking data.  
   - Adjust KDE bandwidth, grid resolution, and Markov transition probabilities to improve accuracy.

---

### **Sensors Integration**

Our project also considers real-world sensor integration for future deployments:

| Component        | Purpose               | Design Notes                            |
| ---------------- | ------------------- | --------------------------------------- |
| 9-axis IMU       | Activity/States      | 25 Hz baseline / 200 Hz during events   |
| Pressure + Temp  | Depth + SST          | 1 Hz baseline / 5–10 Hz during events   |
| Hydrophones (3–4)| DOA + Bite detection | 48–96 kHz, correlation/beamforming      |
| Sonar (FLS)      | “Vision”             | Triggered for 5–15 s                    |
| GNSS             | Surface Position     | Ceramic patch on dorsal fin             |
| Satcom           | Summaries            | Iridium/Argos, ≤300 B per uplink        |
| Active Acoustic  | Link to buoys        | Pings with ID + timestamp               |
| Power            | Battery + Piezo      | Optional solar panel                     |
| Adhesion         | Attachment           | Silicone pad, tear-drop profile         |

*Insert here: `sensor_diagram.png` — Illustration of sensor setup on tagged shark.*

---

### **Summary**

This approach combines satellite and in-situ data, Monte Carlo simulations, probabilistic modeling, and neural network predictions to identify potential shark habitats. The integration of sensors and environmental features allows for high-resolution probabilistic predictions, which can guide conservation, research, and safety operations in oceanic regions.  

<img width="1920" height="1080" alt="Workflow (1)" src="https://github.com/user-attachments/assets/73627fd6-28a3-4a12-9061-298e9c52c029" />


