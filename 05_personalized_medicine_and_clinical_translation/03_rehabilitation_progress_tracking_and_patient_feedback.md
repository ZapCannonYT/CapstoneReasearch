# Rehabilitation Progress Tracking and Patient-Facing Feedback

## 1. Executive Summary & The Gap This Fills

Two existing documents in this directory address related but distinct problems: [`01_subject_specific_scaling_and_calibration.md`](01_subject_specific_scaling_and_calibration.md) is a **one-time baseline calibration** step, and [`02_injury_prevention_and_biofeedback_loops.md`](02_injury_prevention_and_biofeedback_loops.md) is **real-time, within-a-single-set** biofeedback. Neither answers the prerequisite's explicit ask: *"track the progress of somebody in rehabilitation... what can we track about them to tell them about their progress, and help them understand what's wrong and what's right"* — a **longitudinal, week-over-week** question, answered in a way a non-clinical person can understand.

---

## 2. Longitudinal Markers for Rehabilitation Progress

| Marker | How It's Tracked (Low-Budget) | Progress Signal |
| :--- | :--- | :--- |
| **Range-of-Motion (ROM) recovery trajectory** | Weekly camera-only joint-angle capture (see [`../01_physical_markers_and_wearables/06_camera_only_capability_boundaries.md`](../01_physical_markers_and_wearables/06_camera_only_capability_boundaries.md)) plotted over weeks | Monotonic ROM increase toward the uninjured limb's baseline; plateaus flag stalled recovery |
| **Limb Symmetry Index (LSI)** | Bilateral comparison of a functional test (hop distance, squat depth, or camera-derived joint angle) — injured limb ÷ uninjured limb × 100% | Commonly used **80% threshold** for return-to-activity readiness, but see caution below |
| **HRV/HR recovery trend** | 7-day rolling RMSSD or HR-recovery average (see [`../06_metabolic_and_systemic_markers/02_heart_rate_and_hrv_markers.md`](../06_metabolic_and_systemic_markers/02_heart_rate_and_hrv_markers.md)) | Rising trend = systemic recovery keeping pace with rehab loading, not just local joint healing |
| **Functional test battery score** | A short, repeatable set of camera-observable tasks (e.g., single-leg squat depth, step-down control) scored on a fixed rubric, repeated weekly | Trend line across weeks is more informative than any single session's score |

### 2.1 Important Caution on LSI

A 2025 critical analysis found that LSI-based pass/fail criteria for ACL return-to-sport show **near-chance discriminative ability** (Youden J 0.09–0.24, AUC 0.50–0.59) between athletes who did and did not have a safe return. **Implication for this project:** LSI should be shown to the user as *one trend line among several*, never as a single binary "cleared/not cleared" gate — the patient-facing report design in §3 reflects this.

---

## 3. Patient-Facing "What's Wrong / What's Right / How Am I Progressing" Reporting Model

Rather than exposing raw biomechanical numbers, the digital twin's rehab-progress view should translate multi-marker trends into three plain-language bands per marker, updated weekly:

```
[ Marker Trend This Week ]
        |
        v
 GREEN "On track"       — ROM/LSI/HRV all trending toward baseline, no single-session red flags
 YELLOW "Plateauing"     — trend has flattened for 2+ consecutive weeks on any one marker
 RED "Needs attention"   — trend reversing (ROM shrinking, LSI dropping, HRV declining) or an acute in-workout RED alert (§`02_injury_prevention_and_biofeedback_loops.md` §4) fired repeatedly
```

- **"What's wrong":** the specific marker(s) driving a YELLOW/RED band, stated in plain terms (e.g., "your knee bend on the injured side hasn't improved in 2 weeks").
- **"What's right":** markers still GREEN, to avoid discouraging a patient who is improving on most fronts.
- **Never** a single composite "readiness score" gate — consistent with the LSI caution above, multiple independent trend lines are shown rather than collapsed into one pass/fail number.

---

## 4. Verified References (2020–2026)

- **Questioning the rules of engagement: a critical analysis of the use of limb symmetry index for safe return to sport after anterior cruciate ligament reconstruction.** *PMC*, 2025. [pmc.ncbi.nlm.nih.gov/articles/PMC11874420](https://pmc.ncbi.nlm.nih.gov/articles/PMC11874420/). **Source location:** Abstract (top of page) states LSI "cannot differentiate between athletes who had a safe RTS and those who did not"; Discussion (middle of page) on single-snapshot measurement limitations; Results (middle of page) gives Youden J 0.09–0.24 / AUC 0.50–0.59; Limitations section (bottom of page) on Tegner Activity Scale constraints; Conclusion (bottom of page) recommends against relying solely on LSI. Verified live 2026-09-27.
- **Establishing Normal Variances and Expectations for Quadriceps Limb Symmetry Index Benchmarks Based on Time from Surgery After ACL Reconstruction.** *International Journal of Sports Physical Therapy*, 2024. [ijspt.scholasticahq.com/article/94602](https://ijspt.scholasticahq.com/article/94602-establishing-normal-variances-and-expectations-for-quadriceps-limb-symmetry-index-benchmarks-based-on-time-from-surgery-after-anterior-cruciate-ligame).
- **Limb Strength and Power Asymmetries in Professional Team Sport Athletes at Return-to-Sport Testing Following ACL Reconstruction.** *PMC*, 2024. [pmc.ncbi.nlm.nih.gov/articles/PMC13117698](https://pmc.ncbi.nlm.nih.gov/articles/PMC13117698/).
- **Testing Limb Symmetry and Asymmetry After Anterior Cruciate Ligament Injury: 4 Considerations to Increase Its Utility.** *Strength & Conditioning Journal*, 2023. [ovid.com/journals/scjr/fulltext/10.1519/ssc.0000000000000821](https://www.ovid.com/journals/scjr/fulltext/10.1519/ssc.0000000000000821~testing-limb-symmetry-and-asymmetry-after-anterior-cruciate).
- Cross-referenced: [`02_injury_prevention_and_biofeedback_loops.md`](02_injury_prevention_and_biofeedback_loops.md) (real-time layer this document builds on top of) and [`01_subject_specific_scaling_and_calibration.md`](01_subject_specific_scaling_and_calibration.md) (baseline calibration this document tracks deviation from).
