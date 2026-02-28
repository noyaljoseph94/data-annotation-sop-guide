# 📋 Data Annotation Standard Operating Procedure (SOP) Guide

**Author:** Noyal Joseph  
**Role:** Senior Associate — Data Annotation  
**Version:** 1.0  
**Last Updated:** January 2025

---

## 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [Scope & Objectives](#2-scope--objectives)
3. [Tools & Platforms](#3-tools--platforms)
4. [Annotation Types](#4-annotation-types)
5. [Quality Standards](#5-quality-standards)
6. [GDPR & PII Compliance](#6-gdpr--pii-compliance)
7. [Daily Workflow](#7-daily-workflow)
8. [Error Classification](#8-error-classification)
9. [QA Process](#9-qa-process)
10. [Best Practices](#10-best-practices)

---

## 1. Introduction

This SOP guide outlines the standard procedures for performing high-quality data annotation for computer vision and AI/ML projects. It covers annotation workflows, quality benchmarks, compliance requirements, and best practices developed over 3+ years of hands-on annotation experience across automotive and technology sectors.

---

## 2. Scope & Objectives

**Scope:** Applicable to all data annotation tasks including:
- Object detection annotation (bounding boxes)
- Driver monitoring system (DMS) keypoint annotation
- ADAS (Advanced Driver Assistance Systems) frame annotation
- PII masking for GDPR compliance

**Objectives:**
- Maintain accuracy at or above **99%**
- Meet or exceed daily production targets of **250+ frames**
- Ensure **100% GDPR compliance** on all sensitive data
- Deliver **zero critical errors** per project sprint

---

## 3. Tools & Platforms

| Tool | Purpose | Proficiency |
|------|---------|-------------|
| **CVAT** | Computer Vision Annotation Tool — primary annotation platform | Advanced |
| **Dataloop** | AI data platform for annotation and QA workflows | Advanced |
| **MS Excel** | Production tracking, QA reporting, error logging | Advanced |
| **MS Teams / Slack** | Cross-team communication and issue escalation | Proficient |

---

## 4. Annotation Types

### 4.1 2D Bounding Box Annotation

**Purpose:** Object detection for autonomous driving systems  
**Used For:** Vehicles, pedestrians, traffic signs, cyclists

**Steps:**
1. Open frame in CVAT/Dataloop
2. Select the **Rectangle Tool**
3. Draw the box **tightly** around the object — no more than 2px gap
4. Assign the correct **class label** from the approved taxonomy
5. Add required **attributes** (occluded, truncated, crowd)
6. Save and move to next frame

**Quality Checks:**
- Box must not extend outside the object boundary
- All visible objects of the target class must be labeled
- Truncated objects (cut by frame edge) must be marked as `truncated: true`
- Occluded objects (>50% hidden) must be marked as `occluded: true`

---

### 4.2 Keypoint Annotation (DMS)

**Purpose:** Driver monitoring — detecting driver pose, attention, and fatigue  
**Project:** Mercedes-Benz Driver Monitoring System

**Keypoints to annotate:**
- Eyes (left & right)
- Nose tip
- Mouth corners (left & right)
- Ears (left & right)
- Shoulder joints

**Steps:**
1. Load frame in annotation tool
2. Place keypoints on the **exact anatomical location**
3. Mark keypoints as `visible`, `occluded`, or `not_visible`
4. Verify all required keypoints are present
5. Review alignment against reference images
6. Submit for QA review

---

### 4.3 Semantic Segmentation

**Purpose:** Pixel-level classification of scene elements  

**Classes:** Road, Sidewalk, Building, Vegetation, Vehicle, Person, Sky

**Steps:**
1. Use polygon or brush tool
2. Cover **every pixel** of the target class
3. Ensure no overlap between class boundaries
4. Pay special attention to **edge refinement**

---

## 5. Quality Standards

| Metric | Target | Minimum |
|--------|--------|---------|
| Annotation Accuracy | **99.5%** | 99.0% |
| Daily Frame Output | **250+ frames** | 200 frames |
| QA Review Capacity | **1,000+ frames/day** | 800 frames/day |
| Error Rate | **< 1%** | < 2% |
| SLA Compliance | **100%** | 98% |

---

## 6. GDPR & PII Compliance

### What is PII?
Personally Identifiable Information (PII) is any data that can identify a real person.

### PII Types to Mask in Automotive Data:

| PII Type | Examples | Action Required |
|----------|---------|-----------------|
| **Faces** | Driver, pedestrians, passengers | Blur / Black box |
| **License Plates** | Vehicle number plates | Blur / Black box |
| **Street Addresses** | Visible signage with addresses | Blur |
| **Personal Documents** | Any visible ID cards, documents | Blur / Black box |

### Masking Procedure:
1. Identify all PII elements in the frame
2. Apply bounding box mask (black fill) over **entire** PII region
3. Ensure **no part of the PII is visible** after masking
4. Log masked items in the PII tracking sheet
5. Escalate any uncertain cases to QA Lead immediately

> ⚠️ **CRITICAL:** Failure to mask PII is a **Critical Error** and must be escalated immediately. Never submit a frame with unmasked PII.

---

## 7. Daily Workflow

```
08:00 AM  ┌─────────────────────────────────────────┐
          │  Check task queue in CVAT/Dataloop       │
          │  Review any overnight feedback from QA   │
08:15 AM  ├─────────────────────────────────────────┤
          │  Begin annotation batch                   │
          │  Target: 125 frames before lunch         │
12:00 PM  ├─────────────────────────────────────────┤
          │  Lunch break                              │
01:00 PM  ├─────────────────────────────────────────┤
          │  Resume annotation                        │
          │  Complete remaining 125+ frames           │
04:30 PM  ├─────────────────────────────────────────┤
          │  Self QA review of completed frames      │
          │  Log production count in tracker          │
05:00 PM  ├─────────────────────────────────────────┤
          │  Submit batch for QA review               │
          │  Update daily log and error report       │
05:30 PM  └─────────────────────────────────────────┘
```

---

## 8. Error Classification

| Severity | Description | Examples | Response Time |
|----------|-------------|---------|---------------|
| 🔴 **Critical** | GDPR violation or complete annotation failure | Unmasked PII, wrong dataset submission | Immediate escalation |
| 🟠 **High** | Major annotation error affecting model accuracy | Missing objects, wrong class entirely | Fix within 2 hours |
| 🟡 **Medium** | Moderate annotation inaccuracy | Wrong attributes, loose bounding box | Fix within same day |
| 🟢 **Low** | Minor annotation issue | Slightly off boundary (<2px), cosmetic | Fix in next batch |

---

## 9. QA Process

### Self-Review Checklist (Before Submission):

- [ ] All target objects in the frame are annotated
- [ ] Class labels are correct per taxonomy
- [ ] Bounding boxes are tight (≤2px gap)
- [ ] Attributes (occluded, truncated) are correctly set
- [ ] All PII is properly masked
- [ ] No duplicate annotations
- [ ] Keypoints (if applicable) are in correct anatomical positions
- [ ] Production count logged in daily tracker

### QA Review Process:
1. Annotator submits batch
2. QA Analyst reviews 100% of frames (or agreed sample size)
3. Errors flagged and returned to annotator
4. Annotator corrects and resubmits
5. Final accuracy score calculated and logged

---

## 10. Best Practices

✅ **Always zoom in** to check boundary accuracy before submitting  
✅ **Use keyboard shortcuts** to maintain high speed and accuracy  
✅ **Take short breaks** every 90 minutes to avoid fatigue-related errors  
✅ **When in doubt, ask** — escalate uncertain cases to QA Lead  
✅ **Log everything** — keep your daily tracker updated in real time  
✅ **Stay updated** on client taxonomy changes and SOP revisions  
✅ **Review your own errors** — learn from QA feedback to improve accuracy  

❌ **Never rush** at the cost of accuracy  
❌ **Never assume** — if a class is unclear, ask before annotating  
❌ **Never submit** a frame with unmasked PII  

---

## 📎 Appendix — Common Taxonomy Labels

| Category | Labels |
|----------|--------|
| **Vehicles** | car, truck, bus, motorcycle, bicycle, van, ambulance |
| **Persons** | pedestrian, cyclist, rider, crowd |
| **Traffic** | traffic_light, stop_sign, speed_limit_sign, road_marking |
| **Environment** | road, sidewalk, vegetation, building, sky, barrier |
| **DMS** | driver_face, left_eye, right_eye, nose, mouth, left_ear, right_ear |

---

*This document was created based on real-world annotation experience across Krutrim SI Designs, OLA Electric, and KPIT Technologies projects.*

**Contact:** noyaljoseph94@gmail.com | [LinkedIn](https://www.linkedin.com/in/noyaljoseph1994/)
