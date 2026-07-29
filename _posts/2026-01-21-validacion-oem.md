---
layout: post
title: "🔎 Automotive OEM Validation Automation"
date: 2026-01-21 12:00:00 -0300
categories: [automation, python, scraping]
project_type: automation
image: "/assets/img/thumb.png"
share-description: "Automated large-scale OEM reference validation for brand-segmented automotive inventory batches, processing thousands of rows and routing zero-result cases to manual review."
tags: [Python, Pandas, Playwright, OpenPyXL, Web Scraping, Browser Automation, OEM Validation, Automotive Data, Data Quality, Excel Automation, Batch Processing, Structured Logging, Human-in-the-Loop]
---

**Completed automation · Used in real operations**

A Python-based automation that validates automotive OEM references at scale and routes only zero-result cases to manual review.

<!--more-->

## The operational problem

OEM reference validation previously relied on manual spot checks.

A technician searched individual references in a public automotive parts marketplace, reviewed the available results and decided whether the OEM information could be accepted.

A single manual check took approximately **five minutes**, making comprehensive coverage impractical for a large inventory. The process was also exposed to repetitive-work fatigue and manual checking errors.

The goal was not to remove human judgment entirely. It was to automate high-volume screening and focus manual effort only on references without sufficient search evidence.

---

## What I built

I developed a Python automation that processes automotive inventory spreadsheets containing original and aftermarket OEM references.

For each row, the system:

1. reads the part name and OEM reference;
2. constructs a search query combining the part description and reference;
3. performs browser-based searches through Playwright;
4. loads additional results through scrolling and pagination;
5. counts unique product-result links;
6. classifies each reference;
7. writes the validation result back to Excel;
8. records structured execution data for traceability.

Original and aftermarket references are evaluated independently.

---

## Validation logic

The workflow uses three primary outcomes:

- **OK:** search results were found and the reference was accepted;
- **EMPTY:** no results were found and the reference was routed to manual review;
- **FORBIDDEN_CHARS:** the reference contained unsupported characters and was not queried automatically.

This approach reduced the operational review scope: technicians no longer needed to inspect every reference manually and could focus on exceptional or zero-result cases.

The system provides an evidence-based screening signal based on result presence. It does not claim to perform semantic verification of every returned product page.

---

## End-to-end workflow

```text
Brand-segmented Excel inventory
→ original and aftermarket OEM extraction
→ part name + OEM query construction
→ Playwright browser search
→ scrolling and pagination
→ unique result counting
→ OK / EMPTY / FORBIDDEN_CHARS classification
→ Excel validation columns
→ manual review of zero-result cases
````

---

## Real-world use

The automation was used in real operations on brand-segmented inventory batches.

Brands included:

* Ford
* Hyundai
* Kia
* Volkswagen
* Nissan
* Renault
* Peugeot
* Citroën
* and additional manufacturers

The batches belonged to an automotive inventory of approximately **90,000 parts**.

The tool did not process all 90,000 parts in a single execution. Inventory was divided into manageable brand-based batches to reduce blocking risk and keep long-running executions operationally controlled.

---

## Documented executions

Two real execution records provide concrete evidence of scale:

### 4,907-row batch

A documented execution reached **4,907 processed rows**, confirming that the automation could operate on batches containing thousands of inventory records.

### 2,132-row batch

Another documented run processed:

* **2,132 rows**
* **2,109 search queries**
* **549 cache hits**
* **1,560 cache misses**
* **43 references skipped because of unsupported characters**
* **0 recorded errors**
* **0 recorded timeouts**
* approximately **4 hours and 51 minutes** of total execution time
* approximately **8.21 seconds per row**

These metrics come from the automation's structured run summary.

---

## Efficiency impact

Manual OEM verification took approximately **five minutes per reference**.

The automation replaced repetitive spot checks with batch processing and routed only zero-result cases to manual review.

This produced three operational benefits:

* reduced manual checking effort;
* reduced exposure to repetitive transcription and review errors;
* made validation of thousands of inventory rows feasible.

A configurable early-stop threshold prevented unnecessary pagination once sufficient search evidence had been collected, reducing browser activity during large batches.

---

## Technical design

### Excel input and output

The automation reads `.xls` or `.xlsx` inventory files and identifies:

* part description;
* original OEM reference;
* aftermarket OEM reference.

It creates two validation columns:

* `Validacion Original`
* `Validacion Paralelo`

Partial outputs are saved periodically during long executions.

### Browser automation

The operational implementation uses **Playwright**.

The scraper:

* reuses a browser, context and page across the run;
* performs scrolling to load additional results;
* follows pagination;
* deduplicates product links;
* applies timeouts;
* uses a small randomized delay between rows;
* stops searching after reaching a configurable result threshold.

### Cache

The scraper maintains an in-memory cache during each execution.

Repeated queries can reuse prior results instead of generating an additional browser search.

The cache is scoped to the current execution and is not presented as persistent storage.

### Automatic resume

The automation supports automatic recovery from an existing structured JSONL log.

When resume mode is enabled, it:

* identifies the last processed row;
* restores previous validation values;
* continues from the next pending row.

This reduces the cost of interruptions during long-running batches.

---

## Structured logging and traceability

Each processed row is written to a JSONL execution log containing fields such as:

* timestamp;
* row index and row number;
* part name;
* original and aftermarket OEM values;
* validation statuses;
* result counts;
* row processing time;
* cache hits and misses;
* error type and message.

A separate `summary.json` file records aggregate execution metrics, including:

* processed rows;
* total queries;
* skipped references;
* timeouts;
* errors;
* cache activity;
* total execution time;
* average seconds per row;
* processing rate.

This provides row-level operational traceability without claiming that screenshots or full result-page evidence are stored for every search.

---

## Reliability features

The implemented controls include:

* incremental autosave;
* automatic JSONL-based resume;
* in-memory query caching;
* unique-link deduplication;
* input filtering for unsupported characters;
* configurable early stop;
* timeout handling;
* structured error recording;
* randomized delay between rows;
* reusable browser resources.

The project does not claim automatic CAPTCHA solving, anti-blocking guarantees or persistent distributed caching.

---

## My role

I designed and implemented the automation based on a real automotive inventory-validation workflow.

My work included:

* analyzing the manual OEM verification process;
* defining the validation logic;
* building the Excel processing workflow;
* implementing browser automation with Playwright;
* designing result-counting and early-stop behavior;
* adding cache and autosave controls;
* implementing structured logging and run summaries;
* validating the automation with real brand-segmented batches;
* reviewing exceptional cases produced by the workflow.

I was also the operational user responsible for running the batches and reviewing the resulting outputs.

---

## Product status

`Completed automation · Used in real operations`

The project was successfully implemented for periodic validation campaigns on existing inventory.

It remains reusable for future stock-validation work. The same engineering approach can also be adapted to related automotive data extraction and OEM pricing workflows.

The automation has not been run recently, so the target website and scraping selectors may require maintenance before a new operational execution.

---

## Results

* Applied to brand-segmented batches from an inventory of approximately **90,000 parts**.
* Documented execution of **4,907 rows**.
* Documented execution of **2,132 rows in approximately 4 h 51 min**.
* **Zero recorded errors and zero timeouts** in the documented 2,132-row run.
* Original and aftermarket OEM validation.
* Automatic routing of zero-result cases to manual review.
* Incremental Excel autosave.
* Automatic JSONL-based resume.
* Structured row-level logs and aggregate run summaries.
* Reusable browser session and per-run cache.

---

## Stack

**Python** · **Pandas** · **Playwright** · **OpenPyXL** · **Excel Automation** · **Browser Automation** · **JSONL Logging** · **CLI** · **Batch Processing**
