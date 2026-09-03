---
layout: post
title: "🧠 Autoparts Integrator — Computer Vision Pipeline for Autoparts Cataloguing"
subtitle: "A YOLO-based detection and human-in-the-loop system that turns raw vehicle photos into structured, exportable parts data"
date: 2026-07-28 12:00:00 -0300
categories: [cv, automation]
project_type: cv
image: "/assets/img/thumb.png"
share-description: "A YOLO-based detection and human-in-the-loop system that turns raw vehicle photos into structured, exportable parts data"
tags: [Computer Vision, YOLO, Object Detection, ONNX, OCR, FastAPI, React, TypeScript, Streamlit, Human-in-the-Loop, Data Quality, Automotive, Python, SQLite]
comments: false
---

**In active development · Offline-validated on real photo sets · Not yet deployed to production**

Cataloguing used autoparts starts with a photo. Someone has to look at it, figure out what part it is, match it to the right vehicle, and get it into a system correctly enough to publish. **Autoparts Integrator** automates the parts of that flow that can be automated safely, and routes the rest to a human — by design, not by accident.

<!--more-->

## The operational problem

In automotive parts operations, part identification from photos is repetitive, error-prone, and hard to scale consistently. Getting a part wrong — or worse, mixing up a left/right symmetric part — has a real cost: broken listings, wrong pricing, wasted stock movements.

The goal wasn't to remove human judgment. It was to automate high-confidence cases and concentrate manual effort exactly where it's needed.

## What I built

A photo intake and detection pipeline with a human review layer built into the core design, not bolted on:

1. **Intake** — a watcher monitors Google Drive for new vehicle photo folders and queues a job per vehicle.
2. **Quality gate** — before any inference runs, a perceptual-hash + blur/brightness filter discards duplicates and unusable shots.
3. **Detection** — a YOLO model (ONNX, CPU inference) classifies parts in each image. Detections above a 0.50 confidence threshold are auto-assigned; everything else goes to a review queue instead of being guessed.
4. **OCR validation (optional)** — for auto-assigned parts, a deterministic OCR step (OCR.space or self-hosted PaddleOCR) reads part labels. A rule-based scoring engine — regex brand matching, per-class thresholds, penalty rules for known false-positive patterns — decides READY / REVIEW / FAILED. No model does the reasoning here; it's an auditable, hand-tuned decision layer.
5. **Human-in-the-loop review** — a Streamlit tool lets an operator resolve the review queue and correct OCR candidates. Symmetric parts (left/right) are **forced to review regardless of confidence**, a deliberate safety choice to prevent a class of error that's expensive to get wrong.
6. **Export** — validated results are written to Excel matching the client's existing ERP layout, ready for commercial review.

A FastAPI backend and a separate React + TypeScript frontend expose this flow to a pilot-facing app, on top of the internal Streamlit operator tool.

## Why three model generations, and why most aren't live yet

Production today runs on a single YOLO model gating what gets auto-assigned. Two newer model generations (v3: 3 models split by class group, 115 classes; v4: 5 models, 237 classes, end-to-end ONNX export) run in **shadow mode** — they process every image and log predictions for offline comparison, but a kill switch keeps them from affecting what the user sees until they're validated against production. That's the deliberate trade-off: split hard-to-detect classes into smaller specialized models rather than one large one, and promote them only once the numbers justify it.

## Real numbers (offline evaluation, not production telemetry)

These come from running the pipeline against real automotive parts photo archives — not from a live monitoring system, since one doesn't exist yet:

- **1,671 real photos** from a single vehicle, processed end-to-end at ~1.2s/image average (CPU-only inference).
- On a smaller real sample (9 vehicles, 109 photos): **~9% of images reached a fully automated READY state**, with the rest routed to review — a meaningful share of that by deliberate design (symmetric parts always reviewed), not model failure.

The honest read: this is not "no manual data entry" yet. It's a system that removes the *easy* manual work and concentrates human attention on the cases that actually need it — with the infrastructure (shadow models, quality gates, audit logging) already in place to raise that automation rate over time.

## What's next

The immediate priority is closing out **Dataset Factory** — the data governance layer behind this pipeline's dataset, not a separate product. It normalizes 236 legacy classes into 194 confirmed visual identities, with provenance tracking, duplicate control, and vehicle-safe splits.

Current state (September 2026):

- **Human review closeout** — targeted for the first ten days of September. Automated triage already cut 117,013 raw candidate records down to 181 targeted review units before any manual review starts.
- **Taxonomy closeout** — pending the outcome of that review.
- **7 of 194 confirmed identities still have zero visual support.** Closing them means sourcing new real images from external channels — the internal inventory this project originated from is explicitly excluded from that sourcing strategy, to keep the dataset commercially reusable on its own.
- **The clean canonical dataset (v1) won't be built until all 194 identities have visual coverage** — a deliberately complete baseline rather than a partial one to patch later.

From there, the sequence is: taxonomy close → external data acquisition → 194/194 coverage verified → clean canonical dataset v1 → global human QA → train the next model generation → offline evaluation → independent/shadow evaluation → promote or iterate.

Two pieces of the architecture are deliberately unfinished, not abandoned:

- **Evidence reducers** — a safety layer that can only rule candidates out, never invent new ones — are built as a standalone, fail-closed primitive, but not yet wired into the decision path. The contract was validated first; integration happens once the new visual/ERP architecture leaves shadow.
- **W1-B** (separating CV visual identity from ERP business identity from physical part instance) and **photo-reuse lineage** (bounded, non-transitive reuse rules for photos shared across near-identical parts) have a defined business contract but no implementation yet. Neither blocks closing Dataset Factory or training the next model — they're required before the identity/lineage architecture can be called complete.

The `SAFE_AUTO` policy that governs full automation stays off by default until independent evaluation evidence exists to set real acceptance thresholds — no arbitrary numbers get chosen ahead of the data.

That's the point this project is built to demonstrate: not a single trained detector, but the data and evaluation infrastructure to replace and improve that detector safely, repeatedly, over time.

## Stack

**Python** · **YOLO (ONNX Runtime)** · **OCR (OCR.space / PaddleOCR)** · **FastAPI** · **React + TypeScript** · **Streamlit** · **SQLAlchemy + SQLite** · **Google Drive API** · **pytest (695 tests)**

<!--
Agregar capturas cuando estén disponibles, siguiendo el mismo patrón que las otras páginas:

![Detection example](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/01-deteccion.png)
![Review queue (HITL)](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/02-review-queue.png)
![Excel export](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/03-export.png)
-->
