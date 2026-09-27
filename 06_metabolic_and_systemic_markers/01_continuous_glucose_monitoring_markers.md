# Continuous Glucose Monitoring (CGM) as a Metabolic/Systemic Strain Marker

## 1. Executive Summary & Role in the Digital Twin

Every other marker in this repository is **biomechanical** (kinematic, kinetic, neuromuscular). CGM adds a **metabolic/systemic** dimension: it does not measure joint strain directly, but it measures whether the body has the *energy availability* to sustain training and recover from it — directly relevant to the prerequisite's requirement to track "musculoskeletal health... progress... what's wrong and what's right," not just in-the-moment exercise form. It is grouped here with HR/HRV as a low-budget, student-affordable "systemic" pod, separate from the camera-only and biomechanical-wearable markers in earlier sections.

**Important scope caveat:** CGM is FDA/CDSCO-cleared for diabetes management; for a non-diabetic exerciser it is used off-label as a research/self-monitoring tool. It is minimally invasive (a small subcutaneous filament), not fully non-invasive like a camera — flagged here for transparency.

---

## 2. What CGM Measures and Why It Matters for Strain/Recovery Tracking

| Marker | What It Reflects | Relevance to Musculoskeletal Health Tracking |
| :--- | :--- | :--- |
| **Time-in-range (70–140 mg/dL)** | Overall glycemic stability | Healthy, well-fueled athletes spend ≈80% of time in this range; persistent excursions suggest under-fueling relative to training load |
| **Nocturnal glucose dips (3–7 AM)** | Overnight metabolic recovery | Nocturnal hypoglycemia observed in elite endurance athletes under heavy load — an early flag for inadequate recovery/overtraining, complementing HRV-based fatigue flags |
| **Post-meal glucose excursions around training** | Fueling strategy effectiveness | Helps a rehab/training student time carbohydrate intake to avoid reactive hypoglycemia during a session, which can itself increase injury risk from impaired coordination |

---

## 3. Low-Budget Hardware for a Student (India)

| Device | Approx. Cost (India) | Wear Duration | Note |
| :--- | :--- | :--- | :--- |
| **Abbott FreeStyle Libre sensor** | **₹4,200 – ₹5,249 per sensor** | 14 days | Verified live 2026-09-27 across multiple India retailers; this is a **recurring** cost (new sensor every 2 weeks), unlike a one-time sEMG/HR purchase |

This is the single most expensive *recurring* line item in the low-budget suite (see [`02_cost_and_ergonomic_benchmarks/01_hardware_cost_comparison.md`](../02_cost_and_ergonomic_benchmarks/01_hardware_cost_comparison.md) §3.3), so it is presented as optional/supplementary rather than a default component, consistent with the prerequisite's "other factors we can track using simple gadgets... available to a student with a very low budget" framing.

---

## 4. Caution: Psychological Risk of Over-Monitoring

The GSSI review flags a real risk for a non-clinical student user: over-interpreting normal, non-actionable glucose fluctuations ("glucorexia"). Any student-facing dashboard built on this marker should present trends (multi-day averages, nocturnal patterns) rather than raw minute-to-minute glucose numbers, to avoid encouraging anxious over-checking.

---

## 5. Verified References (2020–2026)

- **Glucose Monitoring In Athletes Without Diabetes.** *Gatorade Sports Science Institute — Sports Science Exchange*, published 2025-09-16. [gssiweb.org/sports-science-exchange/article/continuous-glucose-monitoring-use-in-athletes-without-diabetes](https://www.gssiweb.org/sports-science-exchange/article/continuous-glucose-monitoring-use-in-athletes-without-diabetes). **Source location:** section headed *"What is a 'Normal' CGM Reading in an Athlete Without Diabetes?"* (time-in-range figures); section *"Using CGM to Observe the Impact of Training Load"* (nocturnal hypoglycemia 3–7 AM finding); section *"Potential Risks of Using CGM"* (glucorexia caution) — all verified live 2026-09-27, spread through the article body, not concentrated at top or bottom.
- **Continuous glucose monitoring to measure metabolic impact and recovery in sub-elite endurance athletes.** *J Sci Med Sport*, ScienceDirect, 2021. [sciencedirect.com/science/article/abs/pii/S174680942100656X](https://www.sciencedirect.com/science/article/abs/pii/S174680942100656X).
- **Application potential of continuous glucose monitoring (CGM) in elite endurance athletes without diabetes.** *Performance Nutrition (Springer Nature)*, 2025. [link.springer.com/article/10.1186/s44410-025-00013-7](https://link.springer.com/article/10.1186/s44410-025-00013-7).
- **Day-to-Day Glycemic Variability Using Continuous Glucose Monitors in Endurance Athletes.** *Journal of Diabetes Science and Technology (SAGE)*, 2025. DOI: 10.1177/19322968241250355.
- **India CGM pricing (FreeStyle Libre sensor):** verified live 2026-09-27, product listing near top of page — [1mg.com/otc/freestyle-libre-system-sensor-otc616035](https://www.1mg.com/otc/freestyle-libre-system-sensor-otc616035) (₹4,203, MRP ₹4,671); [moglix.com](https://www.moglix.com/abbott-freestyle-libre-glucometer-sensor/mp/msn858047xj492) (₹4,349); [dir.indiamart.com/bengaluru/freestyle-libre-reader-sensor.html](https://dir.indiamart.com/bengaluru/freestyle-libre-reader-sensor.html) (₹4,200, dated 2026-04-22).
