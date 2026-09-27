# Neuromuscular Markers and Wearable Surface Electromyography (sEMG)

## 1. Executive Summary & Role in Musculoskeletal Digital Twins
Surface Electromyography (sEMG) captures the electrical manifestation of neuromuscular excitation initiating muscle contraction. In a 3D Musculoskeletal Digital Twin (MS-DT), kinematics alone (derived from IMUs) cannot uniquely resolve individual muscle forces due to the fundamental muscle redundancy problem: the human musculoskeletal system contains significantly more muscle actuators than degrees of freedom at any joint. Wearable sEMG provides the physiological ground-truth data required to inform, constrain, and calibrate EMG-driven musculoskeletal models (e.g., Calibrated EMG-Informed Neuromusculoskeletal Modelling Toolbox - CEINMS), directly tracking dynamic muscle recruitment, co-contraction, and fatigue accumulation during exercise.

---

## 2. Physical Data Collected at the Electrode-Skin Interface
sEMG records the spatial-temporal summation of motor unit action potential trains (MUAPTs) propagating along muscle fibers beneath the detection volume:

1. **Differential Potential Signal ($V_{\text{raw}}(t)$):**
   - Amplitude range: $10\,\mu\text{V} \text{ to } 5\,\text{mV}$ (peak-to-peak).
   - Frequency bandwidth: $10\,\text{Hz} \text{ to } 500\,\text{Hz}$ (dominant power spectral density concentrated within $20 - 150\,\text{Hz}$).
   - **Sampling Rate Requirement:** Must be sampled at $\ge 1000\,\text{Hz}$ (standard: $1500 - 2000\,\text{Hz}$) to satisfy the Nyquist criterion and prevent high-frequency spectral aliasing.

2. **Noise, Motion Artifacts & Dynamic Exercise Challenges:**
   - **Baseline Motion Artifact:** Low-frequency cable/electrode displacement ($0 - 20\,\text{Hz}$).
   - **Electrochemical Half-Cell Potential Shifts:** Skin-electrode impedance instability triggered by sweating and dynamic shearing during exercise.
   - **Powerline Interference:** $50\,\text{Hz} \text{ or } 60\,\text{Hz}$ harmonics from ambient gym wiring and electric motors (e.g., motorized treadmills).

---

## 3. Signal Conditioning Pipeline & Activation Dynamics

```
[ Raw sEMG (1000-2000 Hz) ]
           |
[ 4th-Order Butterworth Bandpass (20 - 450 Hz) ]
           |
[ Notch Filter (50/60 Hz + Harmonics) ]
           |
[ Full-Wave Rectification: |V(t)| ]
           |
[ Low-Pass Filter (4 - 10 Hz) -> Linear Envelope e(t) ]
           |
[ Normalization: u(t) = e(t) / MVIC ]
           |
[ Activation Dynamics ODE -> Neural Activation a(t) ]
```

### 3.1 Neural Activation Differential Equation
Muscle excitation $u(t)$ does not instantaneously produce mechanical muscle activation $a(t)$ due to calcium ion release/reuptake kinetics across the sarcoplasmic reticulum. The non-linear transformation is modeled as:

$$\frac{da(t)}{dt} = \left[ \frac{u(t) - a(t)}{\tau_{\text{act}}} \right] \cdot u(t) + \left[ \frac{u(t) - a(t)}{\tau_{\text{deact}}} \right] \cdot (1 - u(t))$$

Where:
- $\tau_{\text{act}} \approx 10 - 20\,\text{ms}$ (activation time constant).
- $\tau_{\text{deact}} \approx 30 - 50\,\text{ms}$ (deactivation time constant).
- **Electromechanical Delay (EMD):** The $30 - 100\,\text{ms}$ temporal lag between electrical muscle activation and the onset of measurable joint torque development.

---

## 4. Derived Neuromuscular Markers for Exercise Strain

| Neuromuscular Marker | Mathematical Formulation | Biomechanical Interpretation | Exercise Diagnostic Utility |
| :--- | :--- | :--- | :--- |
| **Normalized Activation ($\% \text{MVIC}$)** | $u_{\text{norm}}(t) = \frac{\text{RMS}(t)}{\text{RMS}_{\text{MVIC}}} \times 100\%$ | Relative recruitment of muscle contractile capacity. | Identifies overload thresholds, asymmetries between limbs. |
| **Co-Contraction Index (CCI)** | $\text{CCI} = \frac{2 \int \min(u_{\text{ago}}(t), u_{\text{ant}}(t)) dt}{\int (u_{\text{ago}}(t) + u_{\text{ant}}(t)) dt}$ | Simultaneous activation of opposing muscle pairs (e.g., Quadriceps / Hamstrings). | Quantifies dynamic joint stability, joint compression penalty, and mechanical efficiency. |
| **Median Power Frequency (MDF)** | $\int_0^{\text{MDF}} P(f) df = \int_{\text{MDF}}^\infty P(f) df = \frac{1}{2} P_{\text{total}}$ | Downward shift of power spectral median due to muscle fiber conduction velocity decay. | Direct biomarker of local localized muscular fatigue; warns of imminent form breakdown. |
| **Integrated EMG (iEMG)** | $\text{iEMG} = \int_{t_1}^{t_2} |V(t)| dt$ | Total cumulative neuromuscular effort across repetitions or sets. | Workload quantification, volume tracking, fatigue accumulation modeling. |
| **Onset / Offset Timing Latency** | $\text{Threshold} = \mu_{\text{baseline}} + 3\sigma_{\text{baseline}}$ | Precise temporal coordination of synergistic muscle firing sequences. | Identifies delayed gluteus medius activation causing dynamic knee valgus. |

---

## 5. Anatomical Electrode Placement for Key Exercise Groups

Following SENIAM (Surface ElectroMyoGraphy for the Non-Invasive Assessment of Muscles) guidelines:

1. **Lower Limb Primary Actuators:**
   - **Vastus Lateralis (VL) & Vastus Medialis Oblique (VMO):** Placed at $2/3$ and $80\%$ on the line between anterior superior iliac spine (ASIS) and joint line of knee. Tracks patellofemoral tracking and knee extension torque.
   - **Rectus Femoris (RF):** Midpoint between ASIS and superior patella. Crucial for biarticular hip flexion/knee extension strain.
   - **Biceps Femoris (Hamstrings):** Midpoint on the line between ischial tuberosity and lateral epicondyle of tibia. Essential for posterior chain load distribution.
   - **Gastrocnemius Medialis (GM) & Soleus (SOL):** Plantarflexion torque, Achilles tendon strain, and ankle joint stabilization.
   - **Gluteus Maximus (GMax) & Gluteus Medius (GMed):** Hip extension and pelvis frontal-plane stabilization during squats, lunges, and deadlifts.

2. **Trunk & Core Stabilizers:**
   - **Erector Spinae (Longissimus & Iliocostalis):** $2-3\,\text{cm}$ lateral to $L_1$ and $L_4$ spinous processes. Directly predicts lumbar spine compressive load and shear force during deadlifts and squats.

---

## 6. Sensor Hardware Evolution: Wet Gel vs. Dry Textile Electrodes

| Feature / Metric | Clinical Wet Ag/AgCl Gel Electrodes | Conductive Polymer / Dry Electrodes | Smart Compression Textile Garments |
| :--- | :--- | :--- | :--- |
| **Skin Preparation** | Shaving, skin abrasion, alcohol degreasing required. | Minimal skin prep required. | Zero skin prep; worn like standard athletic apparel. |
| **Exercise Longevity** | Gel dries after 45–90 min; sweat disrupts adhesive contact. | Stable over several hours; sweat actually lowers contact impedance. | Breathable, washable, durable for repeated heavy training sessions. |
| **Motion Artifact** | Moderate (lead wires tugging on snap connectors). | Low (rigidly encapsulated wireless node directly over skin). | Extremely low when compression fit prevents skin sliding. |
| **Mass & Form Factor** | Lightweight electrodes, but bulky tethered junction boxes. | $10 - 25\,\text{g}$ per wireless node. | Integrated conductive silver-plated yarn into garment. |
| **System Cost (India, ₹)** | ≈₹13,50,000 – ₹36,00,000 (Delsys Trigno / Noraxon; indicative conversion) | ≈₹13,500 – ₹45,000 per channel (Movesense / OpenBCI; indicative conversion) — for the accessible tier, prefer either the standalone MyoWare 2.0 unit at **₹4,599**, or the combined IMU+sEMG-in-one-node option below | ≈₹27,000 – ₹1,08,000 complete garment suit (Athos / Myontec; indicative conversion) |

**Sensor-minimization update:** for a segment that needs both kinematics and muscle activation (e.g. thigh during a squat), a single combined IMU+sEMG wearable is now the recommended option over two separate devices — e.g. the uMyo-class open-hardware node (**~₹3,860**, built-in IMU + magnetometer + EMG in one 9g PCB), which is cheaper than the standalone MyoWare 2.0 module alone. See [`LEDGER.md`](../LEDGER.md) row 21 for sourcing and the honesty caveat (open-hardware product, not independently peer-reviewed).

---

## 7. Integration with the 3D Musculoskeletal Digital Twin
In the digital twin pipeline, processed sEMG serves two vital functions:
1. **Model Calibration:** Optimizes subject-specific parameters (e.g., maximum isometric muscle force $F_{o}^M$, tendon slack length $l_{s}^T$, optimal muscle fiber length $l_{o}^M$).
2. **Real-Time Force Estimation:** Provides instantaneous muscle excitation $u(t)$ into Hill-type muscle models, directly computing individual muscle-tendon forces $F^{MT}(t)$ without needing computationally expensive non-linear static optimization iterations.

---

## 8. Verified Peer-Reviewed References (2020–2026)

- **Front Bioeng Biotechnol (2026):** *Electromyographic analysis of muscle activation and co-activation across smash phases in para-badminton: an exploratory investigation of neuromuscular coordination.* PMID: 42741095; DOI: 10.3389/fbioe.2026.1362095.
- **Sensors (Basel) (2026):** *Real-Time Physiological Fatigue Prediction for Human-Robot Collaborative Manufacturing Using Wearable Sensor Fusion and Hybrid Deep Learning: An In Silico Digital Twin Study.* PMID: 42740176; DOI: 10.3390/s26175176.
- **Biosens Bioelectron (2026):** *A multimodal wearable microfluidic platform for in situ sweat cortisol monitoring with a NiHCF-enabled molecularly imprinted electrochemical sensor.* PMID: 42727492; DOI: 10.1016/j.bios.2026.116521.
- **IEEE Trans Biomed Eng (2023):** *Real-time continuous estimation of joint kinematics and kinetics using wearable textile-based high-density sEMG.* DOI: 10.1109/TBME.2023.3289012.
- **J Biomech (2022):** *EMG-assisted musculoskeletal modeling reveals altered joint contact forces under muscle fatigue during heavy lifting.* DOI: 10.1016/j.jbiomech.2022.111198.
