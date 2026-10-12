---
layout: page
title: About me
subtitle: Data Engineer — ETL, data quality, automation and computer vision datasets
share-description: "Matias Maher — Data Engineer with an automotive background. Experience in ETL pipelines, data quality, workflow automation, Power BI and computer vision dataset engineering."
---

I'm a Data Engineer with a background in automotive homologation and used-parts operations. I build data pipelines and automation for real operational workflows, and I care about data quality, testing and keeping people in control of the decisions that matter.

I'm self-taught. I use LLM assistants during development for research, architecture exploration and implementation support; none of the systems described here depends on an LLM at runtime.

## Experience

### Automotive parts recycler, Spain — Data Engineer (Automation & AI Systems)

*November 2024 – October 2026*

- Built and ran a weekly **OEM pricing ETL** (Python, pandas, HTTP scraping) that brought a previously outsourced process in-house. It processes approximately 250–300 products per week and delivers minimum/maximum market prices and listing counts to the commercial team, which sets margins and loads prices into the ERP. Handed over in October 2026 working, documented and tested, and still in use. [Project write-up]({{ '/automation/scripts/2026/01/23/precios-oem.html' | relative_url }})
- Built an **OEM reference validation** tool with Python and Playwright, run on brand-segmented inventory batches, with documented runs of 4,907 and 2,132 rows. [Project write-up]({{ '/automation/python/scraping/2026/01/21/validacion-oem.html' | relative_url }})
- Built a **Power BI data-quality and sales dashboard** from ERP exports, used to show where catalogue data quality issues affected commercially relevant parts. [Project write-up]({{ '/powerbi/data-analysis/bi/2026/01/06/dashboard-ventas-historicas-2017-2032.html' | relative_url }})

### COC Europe — Automotive Homologation Technician

*April 2022 – October 2024*

- Worked on vehicle technical documentation for homologation, collecting and checking vehicle specifications across technical sources and preparing documents in multiple languages.
- This hands-on experience with the manual workflow is the domain knowledge behind Vehicle Data Print, my own independent product.

## Independent projects

### Vehicle Data Print

A full-stack tool (React, TypeScript, FastAPI, Supabase) that consolidates 97 structured vehicle technical fields from public sources and generates homologation documents in 10 languages. I developed it independently and offered it to COC Europe, where it was validated in daily use by three homologation technicians, reducing processing time from roughly 40–50 minutes to around 15 minutes per vehicle. It did not continue commercially because no agreement was reached on the subscription terms. [Project write-up]({{ '/mvp/product/2025/06/05/mvp-homologaciones.html' | relative_url }})

### Autoparts Integrator

An independent product in active development: a computer vision and human-in-the-loop pipeline for cataloguing used automotive parts from photos (YOLO, ONNX Runtime, OCR, FastAPI, React, Streamlit), together with **Dataset Factory**, a versioned, provenance-tracked data layer for building the next training dataset. It has no external client or pilot and is not commercially deployed. [Project write-up]({{ '/cv/automation/2026/07/28/autoparts-integrator.html' | relative_url }})

## Skills

- **Data engineering:** Python, SQL, pandas, ETL design, batch processing, caching and retry strategies, structured logging
- **Data quality and analytics:** data validation, outlier filtering, Power BI, DAX, Power Query, Excel
- **Automation:** Playwright, HTTP scraping (curl_cffi, BeautifulSoup), CLI tools, pytest
- **Computer vision and datasets:** YOLO, ONNX Runtime, OCR, CVAT annotation workflows, dataset versioning and lineage
- **Applications:** FastAPI, React, TypeScript, Supabase / PostgreSQL, SQLite / SQLAlchemy, Streamlit

## What I'm looking for

Roles in **Data Engineering**, **Data Analytics**, **Automation Engineering** or **Computer Vision / Dataset Engineering**.

You can reach me by [email](mailto:mahermatias@gmail.com), on [LinkedIn](https://www.linkedin.com/in/matias-maher) or on [GitHub](https://github.com/MatiasMaher78).
