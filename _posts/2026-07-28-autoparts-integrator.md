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

- Generalizing the Excel export beyond a single client's fixed column layout — the current blocker to onboarding a second client (the data model already supports multi-tenant, the parsing layer doesn't yet).
- Promoting v3/v4 models from shadow to production once their offline numbers clear the current baseline.
- Moving from local execution to an actual pilot deployment (VPS, systemd, real auth hardening beyond Basic Auth).

## Stack

**Python** · **YOLO (ONNX Runtime)** · **OCR (OCR.space / PaddleOCR)** · **FastAPI** · **React + TypeScript** · **Streamlit** · **SQLAlchemy + SQLite** · **Google Drive API** · **pytest (695 tests)**

<!--
Agregar capturas cuando estén disponibles, siguiendo el mismo patrón que las otras páginas:

![Detection example](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/01-deteccion.png)
![Review queue (HITL)](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/02-review-queue.png)
![Excel export](https://matiasmaher78.github.io/assets/img/projects/autoparts-integrator/03-export.png)
-->
