# Heart Rate & Heart Rate Variability (HRV) as Systemic/Autonomic Strain Markers

## 1. Executive Summary & Why This Gets Its Own Document

The existing [`04_exercise_strain_and_form_metrics/03_muscle_fatigue_and_overuse_modeling.md`](../04_exercise_strain_and_form_metrics/03_muscle_fatigue_and_overuse_modeling.md) reduces HR to a single table row ("Rest Interval Heart Rate Decay"). HR and HRV deserve full treatment because they are the cheapest possible systemic-strain gadget available to an Indian student (as low as ₹90 for a bare PPG module) and, unlike every biomechanical marker in this repository, they measure **autonomic nervous system load** — i.e., whether the body as a whole (not just a joint or muscle) is coping with training and recovering, which feeds directly into the "musculoskeletal health / rehab progress" side of the prerequisites, not just in-workout exercise strain.

---

## 2. Core Markers

| Marker | Definition | What It Reflects |
| :--- | :--- | :--- |
| **Resting Heart Rate (RHR)** | Morning HR before rising | Elevated RHR vs. personal baseline is a classic early overtraining/illness flag |
| **HR Recovery (HRR)** | BPM drop in the 60s after set/session termination | Faster drop = better cardiovascular/autonomic conditioning; the existing fatigue doc's threshold (**>30 BPM drop = nominal, <12 BPM drop = systemic fatigue**) stays valid and is now backed by a dedicated marker treatment |
| **HRV — RMSSD** (root mean square of successive RR-interval differences) | Vagal (parasympathetic) tone | Primary metric used in current sports-science literature for daily readiness/recovery tracking; more sensitive than RHR alone |

---

## 3. Hardware Options and the Accuracy/Cost Trade-off

| Device Class | Approx. Cost (India) | RMSSD Accuracy vs. ECG Gold Standard | Recommendation |
| :--- | :--- | :--- | :--- |
| **Chest strap (Polar-class, RR-interval capable)** | ≈₹8,000 (indicative, India street price for a Polar H10-class strap; not individually re-verified live) | Mean absolute % error **2.16%** vs. ECG | Best accuracy; still far cheaper than clinical telemetry |
| **Smartphone PPG app + MAX30102-class module** | **₹90 – ₹295** (verified live, India) | Mean absolute % error **17.49%** vs. ECG — "acceptable but wider agreement" | Recommended default for a very-low-budget student: good enough for trend tracking (day-to-day direction), not for absolute clinical RMSSD values |

Both device classes were rated in the same 2025 reliability study as having "acceptable reliability and validity for short-duration HRV assessment in athletes," with device choice properly framed as a cost/precision trade-off rather than one being simply "wrong."

---

## 4. Practical Use in the Digital Twin / Rehab-Progress Context

- **In-workout:** HRR at rest intervals feeds the existing 3-tier fatigue safety policy in [`04_exercise_strain_and_form_metrics/03_muscle_fatigue_and_overuse_modeling.md`](../04_exercise_strain_and_form_metrics/03_muscle_fatigue_and_overuse_modeling.md) §5 — no change needed there, just backed by this fuller marker description.
- **Longitudinal (rehab progress):** A rising 7-day rolling RMSSD trend alongside improving Limb Symmetry Index and expanding ROM (see [`05_personalized_medicine_and_clinical_translation/03_rehabilitation_progress_tracking_and_patient_feedback.md`](../05_personalized_medicine_and_clinical_translation/03_rehabilitation_progress_tracking_and_patient_feedback.md)) is a patient-facing "you are recovering well" signal distinct from any single-session strain measurement.

---

## 5. Verified References (2020–2026)

- **An observational study of the reliability and concurrent validity of heart rate variability devices in athletes.** *Frontiers in Physiology*, 2025. DOI: 10.3389/fphys.2025.1707318. **Source location:** Results section, Table 3 area (middle of page) for intra-device reliability (ICC >0.9 chest strap, 0.83–0.90 PPG app); Results, Table 4 area (middle of page) for concurrent validity (chest strap MAPE 2.16%, PPG app MAPE 17.49%); Abstract (top) for the overall "acceptable reliability and validity" verdict; Discussion (bottom) for the cost/practicality trade-off framing. Verified live 2026-09-27.
- **Monitoring Training Adaptation and Recovery Status in Athletes Using Heart Rate Variability via Mobile Devices: A Narrative Review.** *Sensors (Basel)*, 2026. [mdpi.com/1424-8220/26/1/3](https://www.mdpi.com/1424-8220/26/1/3).
- **Heart rate variability analysis method for exercise-induced fatigue monitoring.** *Biomedical Signal Processing and Control (ScienceDirect)*, 2024. [sciencedirect.com/science/article/abs/pii/S1746809424000247](https://www.sciencedirect.com/science/article/abs/pii/S1746809424000247).
- **Editorial: New perspectives and insights on heart rate variability in exercise and sports.** *PMC*, 2024. [ncbi.nlm.nih.gov/pmc/articles/PMC11897043](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11897043/).
- **India pricing, MAX30102 pulse oximeter/HR sensor module:** verified live 2026-09-27, product listing near top of page — [robokits.co.in/sensors/heart-beat-sensor/max30102-pulse-oximeter-heart-rate-upgraded-sensor-module-i2c-compatible](https://robokits.co.in/sensors/heart-beat-sensor/max30102-pulse-oximeter-heart-rate-upgraded-sensor-module-i2c-compatible) (₹295); [robokits.co.in/sensors/heart-beat-sensor/max30102-pulse-oximeter-heart-rate-sensor-module-i2c-interface](https://robokits.co.in/sensors/heart-beat-sensor/max30102-pulse-oximeter-heart-rate-sensor-module-i2c-interface) (₹90).
