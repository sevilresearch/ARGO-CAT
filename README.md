# ARGO-CAT
## Adaptive Robotic Guardian Operations — Companion for Adaptive Tracking

**AI-Enhanced Robotic Companion for Nutrition and Hydration Monitoring in Older Adults**

Wright Family Research Institute for Health and Technology
Research Revolution Challenge 2026 — Phase 2 Submission
University of West Florida

---

## Project Overview

ARGO-CAT is an AI-powered robotic companion system designed to passively monitor the eating and drinking behavior of older adults with Mild Cognitive Impairment (MCI) and early Alzheimer's disease. The system uses a Joy for All companion cat as the physical platform and adds a computer vision pipeline, behavioral event logging system, personalized anomaly detection layer, and custom servo-driven physical movements to enable continuous passive monitoring without requiring any action from the user.

Malnutrition and dehydration are among the most under-recognized health threats in this population. Up to 40% of community-dwelling older adults are chronically under-hydrated, and 26–32% are at nutritional risk. ARGO-CAT is built around the idea that monitoring should work passively, in the background, without adding burden to people who are already struggling to get through the day.

---

## Team

| Name | Role |
|---|---|
| Hakki Erhan Sevil, Ph.D. | Principal Investigator |
| Rodney Guttmann, Ph.D. | Co-Principal Investigator |
| Murilo Basso | Graduate Student — Robotics and Embedded Systems |
| Sai Santosh Malladi | Graduate Student — Computer Vision and AI |
| Calypsa Coursey | Undergraduate Researcher |
| Campbell Smith | Undergraduate Researcher |

---

## Repository Contents

```
ARGO-CAT/
├── ARGO_CAT_Backend_Pipeline.ipynb   Main annotated notebook
└── test_images/                       Sample kitchen images for testing
```

---

## System Architecture

ARGO-CAT is built around two integrated subsystems that together form the complete monitoring and intervention platform.

### AI Backend Pipeline — Three Stages

**Stage 1 — Computer Vision**
Uses YOLO26n, the current state of the art object detection model from Ultralytics (January 2026), filtered to 14 food and drink relevant COCO classes. No facial recognition or biometric data is captured at any point. Only objects are observed, which is a deliberate privacy-preserving design choice.

**Stage 2 — Event Logging**
Every detection is written to a SQLite database and a CSV log with timestamp, object class, confidence score, and event type. A daily summary table pre-computes behavioral statistics for efficient querying by the anomaly detection layer.

**Stage 3 — Anomaly Detection**
Probabilistic risk scoring using z-score and sigmoid mapping. Two-stage baseline — population prior from NHATS-based literature for the first 14 days, transitioning automatically to a personal baseline as individual data accumulates. Outputs one of three action signals:

| Signal | Meaning |
|---|---|
| NORMAL | Behavior within expected range — no action |
| PROMPT | Trigger companion cat physical response |
| ALERT | Notify caregiver |

### Physical Platform — Custom Hardware

The Joy for All companion cat was modified to include two programmable physical movements driven by custom servo hardware:

- **Rear-leg motion** — operates over an approximate range of 25° to 90°
- **Tail motion** — operates over an approximately range of 45° to 135°

Both movements are controlled by an ESP-WROOM-32 ESP32 development board and powered by a dedicated 5V 5A supply. Custom servo brackets, actuator adapters, and tail components were designed and fabricated using 3D printing with ABS and PLA materials. The original cat electronics and interactive behaviors — including meowing, purring, and touch response — are fully preserved.

### Intended Interaction Sequence

```
Behavioral Monitoring → Anomaly Detection → PROMPT → 
Physical Response (tail/leg movement) → User Interaction
```
---

## How To Run

**Requirements**
- Google Colab account
- Google Drive

**Steps**
1. Open `ARGO_CAT_Backend_Pipeline.ipynb` in Google Colab
2. Go to **Runtime → Change runtime type → T4 GPU → Save**
3. Run all cells top to bottom, one at a time
4. When prompted in Cell 10, upload any image from the `test_images/` folder

**Test Images**
Sample kitchen images for pipeline testing are in the `test_images/` folder. Upload any of these in Cell 10 of the notebook to run the full detection and logging pipeline.

---

## Data Sources

| Source | Purpose |
|---|---|
| Publicly sourced kitchen images | Pipeline validation |
| 30-day synthetic behavioral dataset | Anomaly detection baseline |
| National Health and Aging Trends Study (NHATS) | Population-level prior for baseline model |

---

## Dependencies

All dependencies are installed automatically in Cell 2 of the notebook.

```
ultralytics
opencv-python-headless
scikit-learn
scipy
```
---

*University of West Florida | RRC | 2026*
