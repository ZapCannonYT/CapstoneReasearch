# Sensor Utility & Digital Twin Capability

A quick-reference summary, distilled from [`LEDGER.md`](LEDGER.md) and [`README.md`](README.md) §7: what each sensor actually measures, why it is or isn't in the current stack, and exactly how far the resulting Digital Twin can be trusted. This is a summary of research already in the repo, not a new document to review from scratch — full citations live in `LEDGER.md`.

---

## 1. The Sensor Stack

### Monocular Smartphone Camera — primary sensor (₹0)

Measures 2D pixel-space body keypoints from RGB video (30-60 fps). Feeds the DT with 3D joint angles, continuous range of motion, rep count, tempo, phase split, and gross bilateral asymmetry. This is a direct measurement, near-clinical accuracy: RMSE 1.82° ± 0.45°, r = 0.98 vs. Vicon (Row 1); 2.8°-4.1° in multi-view setups (Row 13); within 10°-15° single-camera (Row 17). It costs nothing and requires no worn hardware. It cannot see internal force, muscle activity, or tissue state. A camera-only GRF estimate exists in recent literature (Row 23) but is workshop-tier, not peer-reviewed, and has no extracted error margin — not yet reliable enough to use.

### One Combined IMU + sEMG Node per Key Segment — ~₹3,860, uMyo-class (Row 21)

A single 9g PCB measuring both acceleration/angular velocity (IMU) and muscle electrical potential (sEMG). One node on a key segment gives the DT individual muscle activation and force, co-contraction index, and segment kinematics as a camera backup during occlusion. Both signals are direct measurements: sEMG agreement with clinical Delsys systems is r = 0.93 ± 0.04 (Row 4); IMU drift is corrected via EKF/ZUPT (Rows 14-15). A standalone sEMG module plus a standalone IMU would cost more and add a second wearable for no accuracy gain, so the combined node replaces both.

Ground Reaction Force needs more than one node. With 3 of these nodes worn at once (pelvis plus two lower-limb segments), a verified study reaches 6.8% BW error on vertical GRF, r = 0.97 (Row 24) — close to what a physical insole gets (4.8% ± 1.2% BW, Row 9) — which is what makes a real Inverse Dynamics chain possible without any insole. Its anteroposterior component is also usable (7.8% BW, r = 0.91), but its mediolateral component is weak (r = 0.58) and should not be relied on. A separate, lighter pathway (Row 22, Motion2Press) can infer plantar pressure and CoP from a single node's IMU stream, but only qualitatively — its exact error was never independently verified, so treat it as a trend signal, not a number.

Note: uMyo itself is an open-hardware product, not a peer-reviewed validation study (Row 21, flagged partial) — the 9g/IMU+EMG+magnetometer spec is solid, but no independent lab study benchmarks this specific product.

### Commodity Smartwatch / Fitness Band — already owned, ₹0 marginal

Measures heart rate and RR-interval via PPG or chest strap. Gives HRV (RMSSD) and HR-recovery slope, a systemic/autonomic recovery signal. Direct measurement: chest strap error 2.16% vs. ECG, wrist PPG error 17.49% (usable, not lab-grade) (Row 20). This runs in parallel to the biomechanical mesh — it never feeds Inverse Kinematics, Inverse Dynamics, or muscle-force estimation, and only informs the separate recovery/progress report.

### Optional, Session-Specific Add-Ons

| Sensor | What it uniquely restores | Why it stays optional |
| :--- | :--- | :--- |
| Plantar pressure insole | Sub-foot spatial pressure map and static/isometric-hold GRF, where IMU has little motion signal to work from (Row 9: 4.8% ± 1.2% BW) | Not a front-line focus — 3-node IMU-only GRF is already close for dynamic movement (6.8% BW vertical, Row 24); see `LEDGER.md` §3.1 |
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

| DT Output | Status with camera + combined node(s) + smartwatch (1 node unless noted) | To make it fully trustworthy |
| :--- | :--- | :--- |
| 3D joint angles, ROM, rep/tempo, gross asymmetry | Direct, near-clinical (within a few degrees of Vicon) | Already there |
| Individual muscle activation & force, co-contraction | Direct (sEMG in the combined node) | Already there |
| HRV / recovery trend | Direct, but a parallel systemic layer, separate from the biomechanical mesh | Already there |
| Vertical & anteroposterior GRF | ML-estimated, but quantified: 6.8% BW / r=0.97 vertical, 7.8% BW / r=0.91 AP, with **3 IMU nodes worn at once** (Row 24) — one combined node alone isn't enough | Already achievable with the right node count; a physical insole only improves on this marginally (4.8% BW) |
| Mediolateral GRF, Center of Pressure | ML-estimated and unreliable (mediolateral r=0.58, Row 24) or qualitative-only (Motion2Press, Row 22) | A physical insole for that session, or a peer-reviewed replication with a better mediolateral result |
| Net joint moments (Inverse Dynamics) | ML-estimated, inherits Row 24's vertical/AP accuracy (usable) and mediolateral weakness (not usable) | Usable for the sagittal-plane moments that matter most in squats/deadlifts/hinges; treat frontal-plane moments as unreliable until a better mediolateral estimate exists |
| Cartilage/tendon stress (FEA surrogate) | ML-estimated, inherits the same upstream uncertainty a second time | Same as above, plus subject-specific FEA calibration |
| True spatial plantar pressure map, static/isometric-hold GRF | Not achievable without the optional insole | Add the insole for that specific session |
| Lumbar L4/L5 shear strain | Not achievable without the optional motion tape | Add the motion tape for that specific session |
| Metabolic / energy-availability state | Not achievable without the optional CGM, and even then parallel to, not fused with, the mesh | Add the CGM for that specific tracking period |

**Summary:** camera plus one combined IMU+sEMG node per key segment (plus a smartwatch already owned) directly and reliably reconstructs a person's skeleton, its motion, and individual muscle activation — enough for rehab ROM tracking, movement-quality flags, and asymmetry alerts at clinically-adjacent accuracy. Wearing 3 of those same nodes at once (not just one) unlocks a quantified, verified GRF estimate for the vertical and anteroposterior directions — good enough to drive sagittal-plane joint moments and tissue-stress trends without any insole. The mediolateral direction and true spatial pressure mapping stay unreliable from IMU or camera data alone, regardless of node count. Force-plate-grade kinetics in that one direction, true spatial pressure mapping, spinal shear strain, and metabolic status all remain achievable by adding the relevant optional sensor for the specific session or question that needs it, but none of them belong in the default always-on stack.
