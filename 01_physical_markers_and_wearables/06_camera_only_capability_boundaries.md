# Camera-Only Capability Boundaries: What a Single Smartphone Camera Can and Cannot Track

## 1. Executive Summary & Why This Document Exists

[`05_markerless_computer_vision_markers.md`](05_markerless_computer_vision_markers.md) treats the smartphone camera as one input inside a **hybrid IMU+camera fusion system**. This document answers a narrower, more fundamental question posed by the project's prerequisites: **if a student has zero other hardware — no IMUs, no insoles, no sEMG — what can a single monocular smartphone camera alone actually measure, and what is structurally impossible from vision alone?** This is the primary, default-tier capability boundary that every other gadget in this repository (sEMG, CGM, HR/HRV, or the indigenous IMU) is an optional addition to, not a prerequisite for.

---

## 2. Achievable With a Camera Alone (Zero Other Hardware)

| Marker | Camera-Only Method | Reported Accuracy | Diagnostic Utility |
| :--- | :--- | :--- | :--- |
| **Sagittal joint angles** (knee/hip/ankle flexion-extension, shin angle, trunk angle) | MediaPipe/YOLO-Pose/Strided-Transformer 2D→3D lifting from a single mobile-device video | RMSE within **10°** for most joints, up to **15°** for shoulder flexion / ASIS asymmetry in some exercises, vs. Vicon ground truth | Squat depth, hip-hinge mechanics, sagittal ROM tracking |
| **Frontal-plane deviations** (dynamic knee valgus / FPPA, pelvic drop) | Same monocular pipeline, frontal-view recording | RMSE **3.2°–5.1°** (see [`05_markerless_computer_vision_markers.md`](05_markerless_computer_vision_markers.md) §6) | ACL-risk screening, gluteal weakness signs |
| **Repetition counting & tempo** | Peak/trough detection on a tracked joint-angle time series | Near-perfect at ≥30 fps | Set/rep logging, eccentric:concentric tempo ratio |
| **Barbell/limb path & horizontal drift** | Color/AprilTag-assisted or pure keypoint tracking | Sub-millimeter to few-mm tracking | Squat/deadlift/press bar-path efficiency |
| **Gross bilateral asymmetry** (stance width, limb length proxies, trunk lean) | Left/right landmark comparison, single camera | Qualitative/semi-quantitative | Flags need for closer look, not a diagnosis |

---

## 3. NOT Achievable From Camera Alone (Requires Added Sensors)

| Not Measurable Camera-Only | Why | What It Requires Instead |
| :--- | :--- | :--- |
| **Internal joint moments/forces** ($\boldsymbol{\tau}$, JCF) | Inverse Dynamics needs ground reaction force, not just kinematics | Force-sensing insole or force plate |
| **Individual muscle activation / co-contraction** | No electrical or mechanical muscle signal is visible in RGB video | sEMG (see [`02_neuromuscular_markers_semg.md`](02_neuromuscular_markers_semg.md)) |
| **Tissue/tendon strain, cartilage stress** | Requires internal load estimates, which themselves need kinetic data | Kinetic + kinematic fusion, then FEA/PINN surrogate |
| **Metabolic/systemic state** (glucose, HRV, autonomic recovery) | Not a visual signal at all | CGM, HR/PPG (see [`06_metabolic_and_systemic_markers/`](../06_metabolic_and_systemic_markers/)) |
| **Sub-5° clinical-grade joint angles** | Monocular depth ambiguity and 30–60 fps sampling limit precision | Multi-camera setup (e.g., OpenCap, 2 phones) or IMU fusion |

---

## 4. Practical Implication for the Low-Budget Student Path

Camera-only tracking is sufficient for **coarse form-quality feedback and longitudinal ROM/rep tracking**, which covers a meaningful share of the "musculoskeletal strain + rehab progress" brief without spending anything. Precision-critical use cases (return-to-sport clearance, internal load estimation) need at least one added gadget — this is exactly the tiered "camera first, then cheap gadgets" structure now used in [`README.md`](../README.md) and [`02_cost_and_ergonomic_benchmarks/01_hardware_cost_comparison.md`](../02_cost_and_ergonomic_benchmarks/01_hardware_cost_comparison.md).

---

## 5. Verified Peer-Reviewed References (2020–2026)

- **Exercise quantification from single camera view markerless 3D pose estimation.** 2024. PMID: [PMC10951609](https://pmc.ncbi.nlm.nih.gov/articles/PMC10951609/). **Source location:** Abstract (top of page) states ≤15° error bound; Results section (middle of page) gives the full per-joint RMSE table (≤10° for most metrics). Verified live 2026-09-27.
- **Assessment of monocular human pose estimation models for clinical movement analysis.** *Scientific Reports*, 2025. DOI/URL: [nature.com/articles/s41598-025-22626-7](https://www.nature.com/articles/s41598-025-22626-7).
- **Markerless joint angle estimation using MediaPipe with a rapid setup for joint moment calculation.** *Multimedia Tools and Applications*, 2026. Springer Nature Link: [link.springer.com/article/10.1007/s11042-026-21256-z](https://link.springer.com/article/10.1007/s11042-026-21256-z).
- **Video-Based Markerless Motion Capture for Clinical and Rehabilitation Biomechanics: A PRISMA-ScR Scoping Review.** 2026 preprint. [arxiv.org/pdf/2609.18667](https://arxiv.org/pdf/2609.18667).
- Cross-referenced accuracy figures for hybrid IMU+camera fusion: see [`05_markerless_computer_vision_markers.md`](05_markerless_computer_vision_markers.md) §6–7 (not re-cited here to avoid duplication).
