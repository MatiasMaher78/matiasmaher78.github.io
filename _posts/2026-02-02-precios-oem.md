---
layout: post
title: "⚙️ Automotive OEM Pricing ETL: From Outsourced Task to Internal Service"
date: 2026-01-23 12:00:00 -0300
categories: [automation, scripts]
project_type: automation
image: "/assets/img/thumb.png"
share-description: "Internal OEM pricing ETL pipeline for used automotive parts that replaced an outsourced weekly process. Processes 250–300 products per week with Python, pandas, HTTP web scraping, caching, retries, checkpoints and statistical outlier filtering."
tags: [Python, Pandas, Web Scraping, ETL, Automotive, OEM Pricing, Pricing Automation, Data Engineering, Data Quality, Pytest, Data Pipeline, Batch Processing, Automation]
---

Built an internal **OEM pricing ETL pipeline** for used parts at an automotive parts recycler in Spain, replacing a contractor-dependent weekly workflow with a repeatable and auditable in-house service. The pipeline processes **250–300 products per week** in approximately **five hours**, producing structured minimum/maximum market prices and listing counts that feed the commercial team's pricing and ERP publication process. It was handed over in October 2026 working, documented and covered by automated tests, and it remains in use.

<!--more-->

## The operational problem

The company previously outsourced its weekly OEM pricing research to external developers.

This created several operational constraints:

- recurring third-party execution fees;
- dependency on contractor availability;
- variable delivery times;
- increased risk during high-demand periods;
- limited internal control over maintenance and execution;
- delays of one to two days when the assigned developer was unavailable.

The process was business-critical because pricing teams needed consistent market references for hundreds of used automotive parts every week.

After the internal pipeline was validated, the workflow no longer depended on an external provider.

---

## What the pipeline does

The program reads automotive part batches from **CSV, XLSX or XLS files**, processes each OEM reference and generates a structured pricing output.

For every product, it:

1. normalizes the OEM search query;
2. retrieves market listings through HTTP web scraping;
3. parses the returned HTML;
4. filters results by title relevance;
5. extracts published prices excluding VAT;
6. removes statistical outliers;
7. calculates the observed minimum and maximum market prices;
8. records the number of valid matching listings;
9. exports a consolidated XLSX or CSV file.

### Output structure

For each product with matching listings, the pipeline returns:

- `ID` and `OEM` — the product identifiers from the input batch;
- `Units` — the number of valid matching listings found;
- `Min Price` and `Max Price` — the observed market price range after filtering.

The pipeline only extracts, filters and formats market prices. It does not calculate margins or set final prices. This output is the market price reference for the downstream commercial workflow.

---

## End-to-end business workflow

```text
Pipeline scope (automated):
OEM product batch
→ automated market extraction
→ relevance and outlier filtering
→ min/max price range per product
→ structured XLSX/CSV output

Downstream (commercial team):
→ margin calculation and manual pricing of unmatched products
→ Sales Director review
→ ERP upload and bulk publication
```

The pipeline's responsibility ends at the structured output. Margins, final prices and the ERP upload are handled by the commercial team.

Products without reliable matches are not assigned an automatic price range. They are separated for manual calculation and review.

This human validation step keeps commercial decisions under business control while automating the repetitive data collection and consolidation work.

---

## Architecture and key decisions

The project evolved from browser-based automation into a lighter **HTTP extraction pipeline**.

The final implementation uses a batch-oriented architecture built around:

* a configurable HTTP client;
* HTML parsing;
* OEM query normalization;
* pagination;
* relevance filtering;
* statistical price filtering;
* persistent caching;
* retry and backoff strategies;
* incremental checkpoints;
* anomaly logging;
* structured file export.

### Direct HTTP extraction

Moving away from browser automation reduced runtime overhead and made large batch execution more efficient.

### Persistent cache

Previously processed OEM references are stored locally with a defined expiration period.

This avoids unnecessary repeated requests, reduces processing time and lowers the risk of triggering external rate limits.

### Retry and backoff strategy

Temporary failures are handled through controlled retries and progressively longer waiting periods instead of immediately aborting the batch.

### Smoke test before execution

The pipeline validates external availability before starting a full production batch.

This prevents hundreds of rows from being processed under invalid connectivity or response conditions.

### Incremental checkpoints

Intermediate results are saved during execution so that long-running jobs can recover from partial failures without restarting from the beginning.

### Data quality filters

Two complementary filters improve the reliability of the final pricing range:

1. **Title relevance filtering** removes listings that do not sufficiently match the original OEM query.
2. **P5/P95 statistical filtering** removes extreme prices before calculating the final range.

These decisions make the result more stable when external listings contain unrelated products, incorrect values or unusual market entries.

---

## Business impact

* **250–300 products processed each week**
* approximately **five hours of automated execution** per weekly batch
* weekly delivery no longer depends on external developer availability
* the workflow no longer requires an outsourced execution
* unmatched products remain under manual commercial review
* the commercial team calculates margins, the Sales Director validates the final pricing, and prices are then loaded into the ERP

The main outcome was not only faster extraction. The company converted an outsourced dependency into an internal operational capability.

---

## Reliability and maintainability

The pipeline includes safeguards designed for recurring production use:

* configurable batch execution;
* persistent OEM cache;
* retries with backoff;
* external availability smoke test;
* incremental checkpoints;
* anomaly and detection logs;
* CSV fallback if XLSX export fails;
* automated tests covering critical behavior;
* CLI parameters for diagnostics and controlled runs.

This makes the project a maintainable data pipeline rather than a one-off scraping script.

---

## My role

I designed, implemented, documented and maintained the pipeline until its handover in October 2026, including:

* requirements analysis with the automotive business;
* Python ETL development;
* HTTP web scraping;
* HTML parsing;
* data cleaning and normalization;
* statistical outlier filtering;
* batch processing;
* caching and retry strategies;
* automated testing;
* operational documentation;
* handoff of structured results to the commercial team.

The project combines **data engineering**, **automotive domain knowledge** and **business process automation**.

---

## Results

* Replaced a contractor-dependent weekly process with an internal service.
* Processes 250–300 used automotive products per week.
* Produces structured and auditable OEM market price ranges (Min, Max, Units).
* Removed the operational dependency on an external provider.
* Keeps human review for unmatched and commercially sensitive cases.
* Feeds the downstream pricing and ERP publication workflow run by the commercial team.

---

## Status

`Delivered · Documented and tested · In use`

The pipeline was handed over in October 2026 working, documented and covered by automated tests, and it remains in use as a recurring weekly internal service at the company.

---

## Screenshots

### Input batch

![OEM pricing input batch](/assets/img/projects/precios-oem/input-precios-oem.png)

### Structured output

![OEM pricing structured output](/assets/img/projects/precios-oem/output-precios-oem.png)

---

A sanitized public version of the engine core is available on [GitHub](https://github.com/MatiasMaher78/oem-pricing-portfolio). It runs against a public scraping sandbox and contains no production data; batch input files, relevance filtering, listing counts, checkpoints, the availability check and the CSV fallback remain in the private production version.

## Stack

**Python** · **pandas** · **curl_cffi** · **BeautifulSoup** · **HTTP Web Scraping** · **ETL** · **pytest** · **CLI** · **XLSX/CSV**

---

*Note: the OEM references and part IDs shown in the screenshots have been deliberately altered.*
