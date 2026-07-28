---
layout: post
title: "🚗 Vehicle Data Print — Automotive Homologation MVP"
date: "2025-06-05 12:00:00 -0300"
categories: [mvp, product]
project_type: mvp
image: "/assets/img/thumb.png"
share-description: "Functional automotive homologation MVP that reduced vehicle technical documentation time from 40–50 minutes to around 15 minutes, processing 97 structured fields and generating DOCX files in 10 languages."
tags: [React, TypeScript, FastAPI, Python, Supabase, Automotive Homologation, Vehicle Technical Data, Document Automation, Web Scraping, Human-in-the-Loop, DOCX Generation, Multilingual Application, WLTP, NEDC, Full-Stack MVP]
---

**Functional MVP · Real-user validated · Currently paused**

A full-stack tool that automates vehicle technical data collection and multilingual DOCX generation for automotive homologation.

<!--more-->

## The operational problem

Automotive homologation technicians had to manually search public technical databases, compare information across multiple sources and transcribe vehicle specifications into technical documentation.

The workflow required repetitive data entry and continuous value checking, which created three main problems:

- processing each vehicle took approximately **40–50 minutes**;
- repetitive transcription increased the risk of typing errors and fatigue-related mistakes;
- producing technical documents in multiple languages required additional manual work.

The same three automotive technicians who performed the manual workflow later used Vehicle Data Print during its operational validation.

---

## What I built

Vehicle Data Print is a functional full-stack MVP that collects, consolidates and prepares structured vehicle technical data for automotive homologation workflows.

The application:

1. receives up to three URLs from specialized public technical sources;
2. validates and processes the requested sources;
3. extracts vehicle specifications concurrently;
4. transforms source-specific data into a canonical schema;
5. merges the results using field-level priority rules;
6. presents the consolidated values in an editable interface;
7. allows the technician to review and correct every field;
8. generates a final DOCX document in one of 10 supported languages;
9. stores an auditable snapshot of each export.

The current data model contains **97 structured vehicle technical fields**.

---

## End-to-end workflow

```text
Public technical sources
→ concurrent vehicle data extraction
→ source-specific transformation
→ field-level merging and prioritization
→ technician review and correction
→ language and document selection
→ DOCX generation
→ export snapshot stored in Supabase
```

The system does not blindly publish extracted information. Every final value remains editable before document generation, preserving human control over technically sensitive data.

---

## Real-world validation

Vehicle Data Print was validated in a real operational environment:

- **3 automotive homologation technicians** used the application;
- the tool was used **daily**;
- approximately **125 vehicles** were processed in one month;
- the time per vehicle decreased from **40–50 minutes to around 15 minutes**;
- the time measurement was checked across five different vehicles;
- the system reduced repetitive manual data entry and transcription errors.

The most valuable outcomes for users were:

- faster processing;
- lower manual workload;
- consistent document generation;
- support for 10 languages.

---

## Architecture

### Frontend

The user interface was built with **React and TypeScript**.

It provides:

- authenticated access;
- URL input for technical data sources;
- editable field-by-field review;
- source values and consolidated values;
- document language selection;
- confirmation before export;
- download history;
- local draft persistence.

### Backend

The backend was built with **FastAPI and Python**.

Its responsibilities include:

- URL processing;
- concurrent web scraping;
- HTML parsing;
- data transformation;
- field-level merge rules;
- document generation;
- authentication integration;
- export history and audit data.

### Data and platform services

**Supabase** provides:

- authentication;
- PostgreSQL persistence;
- DOCX template storage;
- export snapshots;
- user-specific download history.

### Document generation

The application uses predefined DOCX templates rendered through **docxtpl and Jinja2**.

One template is maintained for each supported language, allowing document layout and terminology to remain controlled and consistent.

---

## Key architecture decisions

### Multi-source data consolidation

Vehicle technical information can differ between public sources.

Instead of trusting one global source, the system applies explicit priority rules at field level. Specific values, such as fuel type, particulate emissions and trailer mass, use dedicated merge logic.

### Human-in-the-loop review

All consolidated values remain editable before export.

This design reduces repetitive work without removing the technician from the final technical decision.

### Deterministic multilingual generation

The runtime does not depend on an external translation API or LLM.

The system uses controlled document templates and predefined translations for supported enumerated values. This keeps output predictable and avoids translation costs during document generation.

### Auditable export snapshots

Each generated document stores a snapshot of the submitted data in PostgreSQL.

This provides traceability of what information was used for every export, even though the generated DOCX file itself is delivered directly to the user.

---

## Automotive data coverage

The application processes 97 structured fields across areas including:

- vehicle identification;
- dimensions and body structure;
- masses and loads;
- engine and propulsion;
- transmission and chassis;
- body configuration;
- emissions and regulatory data;
- consumption and efficiency.

The implemented regulatory data model includes:

- **WLTP**
- **NEDC**
- **Euro emissions formats**

The system also contains dedicated handling for hybrid and electric vehicle fields.

---

## Multilingual document generation

Vehicle Data Print generates DOCX documents in 10 languages:

- English
- German
- Portuguese
- Italian
- French
- Dutch
- Swedish
- Romanian
- Polish
- Czech

The use of controlled templates improves terminology consistency and reduces the manual work required to prepare multilingual technical documentation.

---

## Business impact

- Reduced processing time from **40–50 minutes to around 15 minutes per vehicle**.
- Processed approximately **125 vehicles in one month**.
- Supported **3 concurrent automotive homologation technicians**.
- Reduced repetitive manual data entry.
- Reduced typing errors caused by transcription and fatigue.
- Consolidated data from multiple sources into one review interface.
- Generated consistent technical documents in **10 languages**.
- Converted a real manual workflow into a validated digital product.

---

## My role

I designed and built the product end-to-end, combining automotive homologation knowledge with full-stack development.

My work included:

- operational workflow analysis;
- automotive data modeling;
- multi-source web scraping;
- transformation and merge rules;
- React and TypeScript frontend development;
- FastAPI backend development;
- Supabase integration;
- authentication and persistence;
- multilingual DOCX generation;
- human-in-the-loop workflow design;
- deployment and real-user validation.

LLMs were used as development accelerators for architecture exploration, implementation support and iteration, but they are not part of the production runtime.

---

## Product status

`Functional MVP · Real-user validated · Currently paused`

The product was developed over approximately four months and validated through daily use with real users and real vehicle data.

It was paused after the validation period because a commercial agreement on the subscription price was not reached. The product was not paused because of a failure in its core technical workflow.

---

## Product demo

The demo uses anonymized real vehicle data and shows the complete workflow from source URLs to the final multilingual DOCX document.

[Watch the Vehicle Data Print demo](VIDEO_URL_HERE)

---

## Results

- 97 structured technical fields.
- 10 supported document languages.
- 3 real automotive homologation users.
- Approximately 125 vehicles processed in one month.
- Daily operational use during validation.
- Processing time reduced to around 15 minutes per vehicle.
- Human review retained before every export.
- Private source-code repository.
- Functional full-stack MVP validated with real-world automotive data.

---

## Stack

**React** · **TypeScript** · **Vite** · **FastAPI** · **Python** · **Supabase** · **PostgreSQL** · **BeautifulSoup** · **Concurrent Web Scraping** · **docxtpl** · **Jinja2** · **DOCX Generation** · **JWT Authentication**
