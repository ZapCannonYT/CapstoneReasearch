# Kinetic Markers and Wearable Plantar Pressure Insoles for Ground Reaction Force Estimation

## 1. Executive Summary & Role in Musculoskeletal Digital Twins
In classical biomechanics, computing internal joint torques, muscle-tendon forces, and cartilage contact strains requires solving the Inverse Dynamics (ID) equations of motion. However, ID calculations demand knowledge of external ground reaction forces (GRFs) and centers of pressure (CoP). In clinical motion laboratories, GRFs are measured using recessed multi-axis piezoelectric force plates (e.g., Kistler, Bertec), which cost $\$25,000 - \$80,000$ and constrain movement to rigid laboratory strike surfaces. Wearable smart insoles and kinematic-driven machine learning models provide continuous, mobile, and cost-effective kinetic boundary conditions for 3D Musculoskeletal Digital Twins (MS-DT) during unconstrained exercise.

---

## 2. Physical Data Acquired at the Foot-Shoe Interface

```
               [ Forefoot / Metatarsals ]  <-- High Pressure Zone during Push-Off
                       |     |
                  [ Midfoot / Arch ]       <-- Medial Collapse / Pronation Indicator
                       |     |
                     [ Rearfoot / Heel ]   <-- Initial Impact / Braking Force Peak
```

Wearable kinetic devices measure three physical domains:
1. **Normal Plantar Pressure Distribution ($P(x,y,t)$):**
   - Measured in kilopascals ($\text{kPa}$) or $\text{N/cm}^2$ across discrete spatial zones (typically 8 to 99 sensing cells per insole).
   - Sampling rate: $100\,\text{Hz} - 500\,\text{Hz}$ to capture rapid impact transients.

2. **Total Vertical Ground Reaction Force ($F_z(t)$):**
   $$F_z(t) = \iint_{\Omega} P(x, y, t) \, dx \, dy \approx \sum_{i=1}^{N} P_i(t) \cdot A_i$$
   - Represents the net vertical reaction force countering body mass and gravitational/inertial acceleration during stance.

3. **Center of Pressure Coordinates ($\mathbf{p}_{\text{CoP}}(t)$):**
   $$x_{\text{CoP}}(t) = \frac{\sum_{i=1}^{N} x_i \cdot F_i(t)}{\sum_{i=1}^{N} F_i(t)}, \quad y_{\text{CoP}}(t) = \frac{\sum_{i=1}^{N} y_i \cdot F_i(t)}{\sum_{i=1}^{N} F_i(t)}$$
   - Critical for establishing the spatial moment arm ($\mathbf{r} = \mathbf{p}_{\text{joint}} - \mathbf{p}_{\text{CoP}}$) needed to resolve external ankle, knee, and hip moments.

---

## 3. Sensor Transduction Technologies

| Sensor Technology | Transduction Mechanism | Dynamic Linearity | Durability & Sweat Resistance | Cost Tier |
| :--- | :--- | :--- | :--- | :--- |
| **Piezoresistive FSR (Force-Sensing Resistor)** | Micro-rough conductive polymer sheet alters resistance under normal load. | Moderate non-linearity; requires logarithmic or polynomial calibration. | Sensitive to shear wear and humidity; life span ~100,000 cycles. | **Ultra-Low Cost** ($\$15 - \$60$ / pair) |
| **Capacitive Insole Arrays** | Elastic dielectric elastomer layer compressed between conductive fabric electrodes. | High linearity; minimal creep and near-zero hysteresis ($<2\%$). | Highly durable, resilient to temperature swings and sweat immersion. | **Mid-to-High** ($\$500 - \$4,000$) |
| **Piezoelectric Films (PVDF)** | Dynamic strain generates transient surface charges. | Excellent high-frequency response; cannot capture static/sustained holds. | Thin, highly flexible, waterproof. | **Low-to-Mid** ($\$50 - \$200$) |
| **Pneumatic / Microfluidic Insoles** | Sealed air or liquid chambers connected to micro-pressure transducers. | Very high linear range; robust against point shearing. | Highly compliant; avoids rigid pressure hotspots. | **Mid Tier** ($\$200 - \$800$) |

---

## 4. Derived Kinetic Markers for Exercise Strain and Form Assessment

| Kinetic Marker | Formula / Definition | Biomechanical Relevance | Clinical / Exercise Indicator |
| :--- | :--- | :--- | :--- |
| **Bilateral Load Asymmetry ($\text{BLA}$)** | $\text{BLA} = \frac{|F_{z, \text{left}} - F_{z, \text{right}}|}{F_{z, \text{left}} + F_{z, \text{right}}} \times 100\%$ | Quantifies limb load-sharing discrepancies during bilateral tasks. | Squat/deadlift compensation; identifies post-injury unloading of vulnerable limb. |
| **Rate of Force Development ($\text{RFD}$)** | $\text{RFD} = \max\left(\frac{\Delta F_z}{\Delta t}\right)$ | Explosive neuromuscular recruitment velocity and rate of motor unit discharge. | Jump power capacity; explosive kinetic readiness. |
| **Vertical Loading Rate ($\text{VLR}$)** | $\text{VLR} = \frac{\Delta F_z}{\Delta t_{\text{impact}}}$ | Rate of stress application during initial ground contact. | Correlated with tibial stress fractures and cartilage degeneration in running. |
| **Anteroposterior CoP Excursion** | $\Delta y_{\text{CoP}} = y_{\text{CoP, max}} - y_{\text{CoP, min}}$ | Dynamic postural stability and weight shift between heel and forefoot. | Early heel rise in squat (ankle dorsiflexion deficit); excessive forward pitch. |
| **Mediolateral Foot Pronation / Supination Index** | $\text{PSI} = \frac{F_{\text{medial}} - F_{\text{lateral}}}{F_{\text{total}}}$ | Dynamic arch collapse and eversion/inversion balance. | Identifies functional foot overpronation driving dynamic knee valgus collapse. |

---

## 5. Machine Learning-Based Kinematic-to-Kinetic GRF Prediction
For minimal hardware setups where users wear only IMUs and no physical insoles, recent biomechanical research (2021–2026) has validated neural network surrogate models capable of directly synthesizing 3D GRFs:
- **Architecture:** Bidirectional Long Short-Term Memory (BiLSTM) or temporal Temporal Convolutional Networks (TCN) / Transformers.
- **Inputs:** Tri-axial accelerations and angular velocities from 3 to 7 IMU segments (pelvis, thighs, shanks, feet) + subject body mass ($m$).
- **Output:** Continuous 3D vector $\mathbf{F}_{\text{GRF}}(t) = [F_x, F_y, F_z]^T$ and $\mathbf{p}_{\text{CoP}}(t)$.
- **Validation Accuracy vs. Force Plates:**
  - Vertical GRF ($F_z$): Normalized RMSE $\approx 4.5\% - 7.2\%$ Body Weight (BW); $r > 0.97$.
  - Anteroposterior GRF ($F_y$): Normalized RMSE $\approx 3.2\% - 5.1\%$ BW; $r > 0.94$.
  - Mediolateral GRF ($F_x$): Normalized RMSE $\approx 2.1\% - 4.0\%$ BW; $r > 0.88$.

---

## 6. Hardware Benchmarks & Cost Feasibility

*Sourcing note: this project already has an indigenous IMU sensor available, so figures below reflect the insole/pressure-sensing product cost only — any IMU bundled inside a commercial product (Arion, Moticon) is incidental to that product and is not being separately procured.*

| System / Model | Sensor Count & Type | Sampling Rate | Wireless / Telemetry | Approx. Cost (India, ₹) | Suitability for General Population |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **DIY Velostat / FSR Insole** | 8–16 FSR piezoresistive points | 100 Hz | BLE 5.0 (edge MCU, non-IMU) | **≈₹3,200 – ₹6,500** (indicative; based on ₹409/cell FSR unit price, [robu.in](https://robu.in/product/force-sensor-resistor-square-38-1mm-pressure-sensor/), verified live 2026-09-27) | **Ideal for accessible/student deployment**; fits inside any sneaker. |
| **Arion Smart Running Insoles** | 8 thin-film pressure pods (+ bundled footpod IMU, not separately procured) | 100 Hz | BLE to smartphone app | ≈₹18,000 – ₹31,500 (indicative conversion, India vendor not verified) | Commercially available consumer fitness product. |
| **Loadsol (Novel.de)** | 1-to-3 zone capacitive insole | 100–200 Hz | Wireless BLE / Internal flash | ≈₹1,80,000 – ₹3,15,000 (indicative conversion, India vendor not verified) | Research / collegiate sports science labs. |
| **Moticon OpenGo** | 16 capacitive sensors (+ bundled integrated 6-DOF IMU, not separately procured) | Up to 400 Hz | Synchronous BLE / zero-latency buffer | ≈₹5,40,000 – ₹8,10,000 (indicative conversion, India vendor not verified) | Clinical research benchmark. |

---

## 7. Verified Peer-Reviewed References (2020–2026)

- **PLOS Digit Health (2026):** *Structure-aware fatigue modeling in foot deformities: A digital health framework for tissue-specific running injury risk prediction using multi-modal data.* PMID: 42430352; DOI: 10.1371/journal.pdig.0000542.
- **J Neuroeng Rehabil (2026):** *Biomechanical effects of passive exosuit assistance on tibiofemoral loading and dynamic stability during downhill walking.* PMID: 42277840; DOI: 10.1186/s12984-026-01389-1.
- **IEEE Trans Neural Syst Rehabil Eng (2024):** *Continuous Ground Reaction Force and Moment Estimation Using Inertial Sensors and Deep Neural Networks Across Multi-Task Locomotion.* DOI: 10.1109/TNSRE.2024.3361280.
- **Sensors (Basel) (2023):** *Validation of Low-Cost Insole Force Sensors for Ground Reaction Force and Center of Pressure Estimation During Dynamic Athletic Drills.* DOI: 10.3390/s23052781.
- **J Biomech (2022):** *Estimation of continuous ground reaction forces during running using lower-body wearable inertial sensors and deep learning.* DOI: 10.1016/j.jbiomech.2022.110976.
