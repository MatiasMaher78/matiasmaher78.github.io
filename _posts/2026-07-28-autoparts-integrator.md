---
layout: post
title: "🧠 Autoparts Integrator — Computer Vision Pipeline for Autoparts Cataloguing"
subtitle: "A YOLO-based detection and human-in-the-loop system that turns raw vehicle photos into structured, exportable parts data"
date: 2026-07-28 12:00:00 -0300
categories: [cv, automation]
project_type: cv
image: "/assets/img/thumb.png"
share-description: "Independent product in active development: a YOLO-based detection and human-in-the-loop system that turns raw vehicle photos into structured parts data, with a provenance-tracked dataset layer (Dataset Factory)."
tags: [Computer Vision, YOLO, Object Detection, ONNX, OCR, FastAPI, React, TypeScript, Streamlit, Human-in-the-Loop, Data Quality, Dataset Engineering, CVAT, Automotive, Python, SQLite]
comments: false
---

**Independent product · In active development · Components validated offline · No external client or pilot · Not commercially deployed**

Cataloguing used autoparts starts with a photo. Someone has to look at it, figure out what part it is, match it to the right vehicle, and get it into a system correctly enough to publish. **Autoparts Integrator** is an independent product I'm building to automate the parts of that flow that can be automated safely, and route the rest to a human — by design, not by accident.

<!--more-->

## The operational problem

In automotive parts operations, part identification from photos is repetitive, error-prone, and hard to scale consistently. Getting a part wrong — or worse, mixing up a left/right symmetric part — has a real cost: broken listings, wrong pricing, wasted stock movements.

The goal isn't to remove human judgment. It's to automate high-confidence cases and concentrate manual effort exactly where it's needed.

## What is implemented

A photo intake and detection pipeline with a human review layer built into the core design, not bolted on:

1. **Intake** — a watcher monitors Google Drive for new vehicle photo folders and queues a job per vehicle.
2. **Quality gate** — before any inference runs, a perceptual-hash + blur/brightness filter discards duplicates and unusable shots.
3. **Detection** — a baseline YOLO model (ONNX, CPU inference) classifies parts in each image. Detections above a configured confidence threshold are auto-assigned; everything else goes to a review queue instead of being guessed.
4. **OCR validation (optional, off by default)** — for auto-assigned parts, a deterministic OCR step (OCR.space or self-hosted PaddleOCR) reads part labels. A rule-based scoring engine — regex brand matching, per-class thresholds, penalty rules for known false-positive patterns — decides READY / REVIEW / FAILED. No model does the reasoning here; it's an auditable, hand-tuned decision layer.
5. **Human-in-the-loop review** — a Streamlit tool lets an operator resolve the review queue and correct OCR candidates.
6. **Export** — validated results are written to an ERP-compatible Excel import layout, ready for commercial review.

A FastAPI backend and a separate React + TypeScript frontend expose the same flow as a web application, alongside the internal Streamlit operator tool. The application runs in development and internal testing; it has no external users and is not commercially deployed.

## Baseline model and shadow evaluation

In the current pipeline, a single baseline YOLO model decides what gets auto-assigned. Two newer model generations are configured to run in **shadow mode**:

- **v3:** 3 models split by class group, 115 classes;
- **v4:** 5 models, 237 classes, end-to-end ONNX export.

Shadow models process images and log predictions for offline comparison, but they never change what gets assigned, and each has a kill switch. Neither has been promoted. The design choice is deliberate: split hard-to-detect classes into smaller specialized models rather than one large one, and promote them only once independent evaluation justifies it.

## Evaluation and safety infrastructure

Implemented and covered by tests, but intentionally **not** connected to product decisions yet:

- an **independent evaluation harness** with a sealed holdout gate and a read-only metrics report;
- a **versioned `SAFE_AUTO` policy registry** — every policy is created as `DISABLED`, and promotion is blocked until acceptance criteria (minimum sample size, precision, tolerated false-auto rate) are formally defined. No runtime code consults it yet;
- **fail-closed evidence reducers** — a safety layer that can only rule candidates out, never invent new ones — built as a standalone primitive, not yet wired into the decision path;
- an isolated **shadow path for the new visual taxonomy**, disabled by default.

The codebase has an automated pytest suite of 1,366 test cases (collected count, October 2026).

Still designed but not implemented: **W1-B** (separating CV visual identity from ERP business identity from the physical part instance) and **photo-reuse lineage** (bounded, non-transitive rules for photos shared across near-identical parts). Both are required before the identity/lineage architecture can be called complete, and before any automation policy can be enabled.

## Pipeline stress test

- 1,671 real photos from a single vehicle, processed end-to-end with 0 processing errors at ~1.2s/image (CPU-only inference).

## Dataset Factory — status (October 2026)

**Dataset Factory** is the data governance layer that turns the pipeline's legacy classes and historical annotations into a clean, provenance-tracked training dataset. Current state:

- **Taxonomy:** a versioned taxonomy build (late September 2026) consolidates legacy classes into **164 training target classes**, with lineage, rollback manifests and explicit left/right laterality handling.
- **Semantic human review:** completed for the first review scope — **3,268 records** reviewed and ingested under strict validation, of which 2,972 were confirmed as correct class images, 295 rejected as unusable and 1 removed as a duplicate.
- **Next step:** bounding-box re-annotation in CVAT for the confirmed records, currently in preparation.
- **Coverage gaps:** a small number of target classes still lack sufficient image support and need newly sourced, provenance-resolved images.
- **Not done yet:** the clean canonical dataset is not finalized, the dataset is not training-ready, and no new model has been trained or promoted on it.

From there, the sequence is: CVAT re-annotation → full class coverage verified → clean canonical dataset v1 → global human QA → train the next model generation → offline evaluation → independent/shadow evaluation → promote or iterate.

That's the point this project is built to demonstrate: not a single trained detector, but the data and evaluation infrastructure to replace and improve that detector safely, repeatedly, over time.

## Stack

**Python** · **YOLO (ONNX Runtime)** · **OCR (OCR.space / PaddleOCR)** · **FastAPI** · **React + TypeScript** · **Streamlit** · **SQLAlchemy + SQLite** · **Google Drive API** · **CVAT** · **pytest**

<!--
Agregar capturas cuando estén disponibles, siguiendo el mismo patrón que las otras páginas:

![Detection example](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/01-deteccion.png)
![Review queue (HITL)](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/02-review-queue.png)
![Excel export](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/03-export.png)
-->
