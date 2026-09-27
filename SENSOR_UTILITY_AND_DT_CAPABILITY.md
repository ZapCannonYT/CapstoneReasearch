# Sensor Utility & Digital Twin Capability

A quick-reference summary, distilled from [`LEDGER.md`](LEDGER.md) and [`README.md`](README.md) §7: what each sensor actually measures, why it is or isn't in the current stack, and exactly how far the resulting Digital Twin can be trusted. This is a summary of research already in the repo, not a new document to review from scratch — full citations live in `LEDGER.md`.

---

## 1. The Sensor Stack

### Monocular Smartphone Camera — primary sensor (₹0)

Measures 2D pixel-space body keypoints from RGB video (30-60 fps). Feeds the DT with 3D joint angles, continuous range of motion, rep count, tempo, phase split, and gross bilateral asymmetry. This is a direct measurement, near-clinical accuracy: RMSE 1.82° ± 0.45°, r = 0.98 vs. Vicon (Row 1); 2.8°-4.1° in multi-view setups (Row 13); within 10°-15° single-camera (Row 17). It costs nothing and requires no worn hardware. It cannot see internal force, muscle activity, or tissue state. A camera-only GRF estimate exists in recent literature (Row 23) but is workshop-tier, not peer-reviewed, and has no extracted error margin — not yet reliable enough to use.

### One Combined IMU + sEMG Node per Key Segment — ~₹3,860, uMyo-class (Row 21)

A single 9g PCB measuring both acceleration/angular velocity (IMU) and muscle electrical potential (sEMG). Gives the DT individual muscle activation and force, co-contraction index, and segment kinematics as a camera backup during occlusion. Both signals are direct measurements: sEMG agreement with clinical Delsys systems is r = 0.93 ± 0.04 (Row 4); IMU drift is corrected via EKF/ZUPT (Rows 14-15). A standalone sEMG module plus a standalone IMU would cost more and add a second wearable for no accuracy gain, so the combined node replaces both. The same IMU stream can also be cross-modally decoded into plantar pressure, GRF, and CoP without an insole (Row 22, Motion2Press) — this is what makes Inverse Dynamics possible without extra hardware, though the source's exact error figures were paywalled and are not claimed here; treat that output as a trend, not an absolute number. Note: uMyo itself is an open-hardware product, not a peer-reviewed validation study (Row 21, flagged partial) — the 9g/IMU+EMG+magnetometer spec is solid, but no independent lab study benchmarks this specific product.

### Commodity Smartwatch / Fitness Band — already owned, ₹0 marginal

Measures heart rate and RR-interval via PPG or chest strap. Gives HRV (RMSSD) and HR-recovery slope, a systemic/autonomic recovery signal. Direct measurement: chest strap error 2.16% vs. ECG, wrist PPG error 17.49% (usable, not lab-grade) (Row 20). This runs in parallel to the biomechanical mesh — it never feeds Inverse Kinematics, Inverse Dynamics, or muscle-force estimation, and only informs the separate recovery/progress report.

### Optional, Session-Specific Add-Ons

| Sensor | What it uniquely restores | Why it stays optional |
| :--- | :--- | :--- |
| Plantar pressure insole | Sub-foot spatial pressure map and static/isometric-hold GRF, where IMU has little motion signal to work from (Row 9: 4.8% ± 1.2% BW) | Not a front-line focus — IMU-only GRF is already close for dynamic movement (6.2% ± 1.8% BW); see `LEDGER.md` §3.1 |
| Paraspinal motion tape | Lumbar L4/L5 shear/strain and spinal flexion curvature during heavy hinge movements (Row 6) | Niche use case, no verified India vendor pricing yet |
| Continuous Glucose Monitor (CGM) | Interstitial glucose trend, nocturnal-hypoglycemia flag for under-recovery/overtraining (Row 19) | Purely metabolic, outside the biomechanical mesh, and a recurring cost (~₹4,200-5,249 per 14-day sensor) rather than one-time |

---

## 2. Sensors Considered but Not Adopted

| Sensor / approach | Why it appeared in the research | Why it's not in the final stack |
| :--- | :--- | :--- |
| Standalone dedicated IMU (separate from the combined node) | Core kinematic sensor in the original multi-sensor design; indigenous unit already available at ₹0 | Theory kept (EKF, ZUPT, quaternion fusion — see `01_kinematic_markers_imu.md`), but as a procurement item it's absorbed into the combined IMU+sEMG node wherever a segment also needs muscle force, which is most segments of interest |
| Standalone dedicated sEMG (MyoWare 2.0, ₹4,599) | Well-validated muscle-activation sensor (Row 4) | Superseded by the combined node: same signal, one fewer wearable, lower total cost |
| 5-7 node full IMU array / research wearable suits (Xsens, Moticon, Delsys) | Gold-standard comparison benchmarks used throughout the ledger | Priced for research labs (₹13-85+ lakh), functionally redundant with camera + one combined node at student budget |
| Piezoelectric bone-strain patches (Row 11) | Emerging materials-science literature on implant/orthopedic mechanobiology | Clinical/implant-focused use case, no consumer sourcing path |
| Microfluidic sweat-biomarker patches (Row 12) | Emerging wearable-biosensor literature (sweat conductivity, hydration) | Early-stage research device, not available in India at student price, overlaps with what CGM already covers |
| Camera-only GRF (GRF-MV, Row 23) | The most aggressive possible offload — zero wearables for kinetics | Not dropped, but not yet relied upon — workshop-tier, no peer review, no extracted accuracy figures. Worth revisiting if a journal version is published |

---

## 3. Digital Twin Capability Ceiling

| DT Output | Status with camera + 1 combined node + smartwatch | To make it fully trustworthy |
| :--- | :--- | :--- |
| 3D joint angles, ROM, rep/tempo, gross asymmetry | Direct, near-clinical (within a few degrees of Vicon) | Already there |
| Individual muscle activation & force, co-contraction | Direct (sEMG in the combined node) | Already there |
| HRV / recovery trend | Direct, but a parallel systemic layer, separate from the biomechanical mesh | Already there |
| GRF & Center of Pressure (dynamic movement) | ML-estimated from the IMU stream (Motion2Press pathway) — useful for trend/flagging, error not independently verified | A physical insole for that session, or a peer-reviewed replication of Motion2Press with quantified error |
| Net joint moments (Inverse Dynamics) | ML-estimated, inherits the GRF-estimation uncertainty above | Same as above — show as a trend, not an absolute torque value, until validated |
| Cartilage/tendon stress (FEA surrogate) | ML-estimated, inherits the same upstream uncertainty a second time | Same as above, plus subject-specific FEA calibration |
| True spatial plantar pressure map, static/isometric-hold GRF | Not achievable without the optional insole | Add the insole for that specific session |
| Lumbar L4/L5 shear strain | Not achievable without the optional motion tape | Add the motion tape for that specific session |
| Metabolic / energy-availability state | Not achievable without the optional CGM, and even then parallel to, not fused with, the mesh | Add the CGM for that specific tracking period |

**Summary:** camera plus one combined IMU+sEMG node per key segment (plus a smartwatch already owned) directly and reliably reconstructs a person's skeleton, its motion, and individual muscle activation — enough for rehab ROM tracking, movement-quality flags, and asymmetry alerts at clinically-adjacent accuracy. It also produces a first-pass kinetics chain (GRF, joint moments, tissue stress) at no extra hardware cost, but every number past the sEMG itself is currently a machine-learning estimate riding on an unquantified error bar, not a validated measurement — present it as a trend, not a clinical figure, until a properly benchmarked replication exists. Force-plate-grade kinetics, true spatial pressure mapping, spinal shear strain, and metabolic status all remain achievable by adding the relevant optional sensor for the specific session or question that needs it, but none of them belong in the default always-on stack.
