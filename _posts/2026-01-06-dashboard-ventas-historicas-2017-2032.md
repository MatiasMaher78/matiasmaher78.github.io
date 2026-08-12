---
layout: post
title: "Historical Sales & Data Quality Dashboard"
date: 2026-01-06 12:00:00 -0300
categories: [powerbi, data-analysis, bi]
project_type: bi
tags: [Power BI, DAX, Data Quality, Data Modeling, ETL, Excel, Business Intelligence]
image: "/assets/img/thumb.png"
permalink: /powerbi/data-analysis/bi/2026/01/06/dashboard-ventas-historicas-2017-2032.html
---

**Power BI MVP · Automotive Used Parts · Presented to Management**

> **Demo note:** dates, monetary values and inventory volumes were transformed for confidentiality. The analytical logic, relative patterns, rankings and dashboard behavior were preserved.

<!--more-->

---

## The business problem

The company had years of historical information inside its ERP, but that information was primarily used for day-to-day operations rather than analytics.

Before asking questions such as:

- Which vehicle brands generate the most revenue?
- Which parts sell the most?
- What inventory deserves more attention?
- Where should cataloging effort be prioritized?

there was a more fundamental question:

> **How reliable is the historical ERP data for decision-making?**

At the time, I did not have direct access to the ERP's SQL database. I worked with limited Excel exports containing historical sales, vehicle intake and current stock data.

The goal of this MVP was therefore twofold:

1. **Measure the reliability of the available historical data.**
2. **Demonstrate that ERP data + Excel + Power BI could already support better commercial decisions**, while providing a clear roadmap for a future SQL-based analytical model.

---

# 1. Data Quality & Catalog Reliability — Overview

![Data Quality Overview](/assets/img/projects/ventas-historicas/01-data-quality-overview.png)

The first page intentionally covers the **full historical dataset**, including the different ERP maturity phases:

- Pre-ERP
- Transition
- Consolidation
- Optimization

This makes it possible to evaluate not only current data quality, but also how reliable older records are before using them for commercial analysis.

### Executive indicators

The overview combines:

- **Net Revenue (Demo)**
- **Historical Units Sold (Demo)**
- **Average Selling Price (Demo)**
- **Composite Data Quality Index**

In the final demo view, the Data Quality Index reaches:

> **99% · HIGH**

`Historical Units Sold` is intentionally shown as a broad historical reference. It provides scale and context for the dataset rather than acting as the denominator of every commercial KPI displayed elsewhere in the report.

**A note on scope:** although this page spans the full 2017–2032 range, most of the commercially usable revenue is concentrated in the more recent years. Narrowing the same view from 2017–2032 down to 2028–2032 only reduces Net Revenue from €755.7K to €748.4K — the older records add volume to the historical dataset, but relatively little to validated revenue. That's part of why the next page narrows its focus deliberately (see below).

---

## Data Quality Index

The composite score implemented in DAX combines four underlying factors:

1. **Price availability**
2. **Technical-data completeness — Brand / Model / Engine**
3. **Return-rate quality**  
   Lower return rates increase the score.
4. **Share of non-generic catalog records**  
   Fewer generic part descriptions increase the score.

At a high level:

```text
Data Quality Index
    = Average(
        Price Completeness,
        Technical Completeness,
        1 - Return Rate,
        1 - Generic Parts Rate
      )
```

This index was designed to summarize whether the filtered dataset was sufficiently reliable for business analysis.

### Technical Data Completeness

The dashboard also exposes a stricter **Technical Data Completeness** classification.

This visual is related to the technical-completeness component of the index, but it is **not the exact same calculation**.

The donut requires:

- Brand
- Model
- Version
- Engine

while the component used inside the composite index requires Brand / Model / Engine.

In the current demo overview, approximately **98.6%** of the evaluated records meet the stricter completeness requirement.

### Intake-to-sale Date Consistency

The second validation visual checks whether:

> **Vehicle/part intake date ≤ sale date**

This is an additional data-quality control and **is not included in the composite Data Quality Index**.

It exists to identify chronological inconsistencies that could compromise time-based analysis.

---

## What the quality analysis revealed

The ERP exports showed several historical cataloging issues:

- Generic part descriptions
- Missing technical attributes
- Inconsistent vehicle information
- Legacy records created under different operational standards
- Provider-related labels appearing inside fields intended for vehicle brands in some atypical records

These issues were not equally important across the catalog.

The **Top Selling Parts** ranking provided the business context needed to prioritize remediation.

High-volume families such as:

- Alternators
- Front headlamps
- Batteries
- Starter motors
- Rear lighting components
- Window regulators
- Wheel rims

have more commercial impact than low-volume references.

That changed the question from:

> "How do we clean the entire database?"

to:

> **"Which data should we improve first because it affects the products the business sells the most?"**

---

# 2. Sales Performance — Overall View

![Sales Performance Overview](/assets/img/projects/ventas-historicas/02-sales-overview.png)

Once the reliability of the historical information had been evaluated, the second page moves into commercial performance.

Unlike the quality page, the Sales Performance view is scoped to the two more mature ERP phases — **Consolidation** and **Optimization**. With no individual phase selected, both phases are included in the analysis. This scoping isn't arbitrary: as the data-quality page shows, older records contribute comparatively little to validated revenue, so commercial analysis focuses on the periods the business can act on with confidence.

The page answers three questions quickly:

> **How much are we selling? Which brands generate revenue? Which parts generate volume?**

---

## Executive KPIs

The commercial overview includes:

- **Net Revenue**
- **Units Sold**
- **Average Selling Price**
- **Current Parts in Stock**

The demo values remain internally consistent.

For example:

```text
Net Revenue / Units Sold
≈ Average Selling Price
```

The stock KPI is intentionally treated as a **global inventory reference**, providing operational context alongside historical sales.

---

## Monthly Net Revenue Trend

The monthly chart shows revenue in true chronological order across the selected demo period.

This provides a time-based view of commercial activity and makes it possible to identify:

- periods of stronger demand,
- quieter months,
- revenue spikes,
- and changes in commercial activity over time.

The visual is descriptive at this stage: it shows **what happened**, while deeper explanations would require additional business variables.

---

## Revenue drivers vs. volume drivers

Two separate rankings were intentionally used.

### Top 10 Brands by Net Revenue

This answers:

> **Which vehicle brands contribute the most monetary value?**

The overview shows revenue concentrated among a relatively small group of brands, with Toyota leading the demo ranking, followed by other major manufacturers.

### Top 20 Parts by Units Sold

This answers a different question:

> **Which product families move the most units?**

The ranking is strongly represented by:

- Bumpers
- Headlamps
- Wheel rims
- Rear lighting
- Mirrors
- Body components

This distinction matters.

A vehicle brand can generate high revenue without leading unit volume, while a part family can rotate frequently without having the highest average selling price.

The dashboard therefore provides a **first evidence-based layer** for prioritizing:

- purchasing analysis,
- dismantling decisions,
- catalog improvement,
- and pricing reviews.

It does not yet calculate profitability — that requires additional cost information.

---

## Current inventory as context

Adding current inventory to the commercial view also exposed the next analytical gap:

> **Sales volume alone is not enough to evaluate stock efficiency.**

The next stage would require:

- Stock age
- Acquisition cost
- Time-to-sale
- Inventory turnover
- Gross margin
- Vehicle-level return

Those variables would allow the company to distinguish:

> **fast-moving inventory from capital potentially remaining immobilized.**

---

# 3. Brand Drill-Down — Toyota Example

![Toyota Drill-Down](/assets/img/projects/ventas-historicas/03-toyota-drilldown.png)

The third view demonstrates how the same analytical model moves from an executive overview to a **brand-specific operational analysis**.

Selecting Toyota recalculates:

- Net Revenue
- Units Sold
- Average Selling Price
- Monthly Revenue Trend
- Top-selling parts

The Top Brands visual collapses to the selected manufacturer, while the parts ranking reveals which components are driving demand within that vehicle population.

In the demo example:

- **€70,242 Net Revenue**
- **180 Units Sold**
- **€390 Average Selling Price**

and:

```text
€70,242 / 180 ≈ €390
```

This keeps the demo internally consistent while protecting the original business values.

---

## From executive BI to operational questions

The Toyota drill-down shows strong demand around several exterior, lighting and body-related components.

That allows the dashboard to support questions such as:

> Which components from this brand historically move fastest?

> Which references deserve better catalog coverage?

> Which parts should receive more attention during dismantling and cataloging?

> Where should pricing or stock analysis go deeper?

The objective is not simply to say:

> "Toyota performs well."

It is to understand:

> **which components inside the Toyota population are responsible for that performance.**

### Global stock reference

The **Parts in Stock** KPI intentionally remains unchanged when Toyota is selected.

This is by design.

It represents **global current inventory context**, not Toyota-specific stock, and therefore should not be interpreted as inventory belonging exclusively to the selected brand.

---

# Modeling challenge

The source data came from **limited ERP exports rather than direct SQL access**.

The initial modeling approach was a conventional star schema.

I created cleaned datasets for:

- Historical Sales
- Vehicle Intake
- Current Stock

and dimensions for:

- Date
- Vehicle
- ERP/Data Quality Period

The expected architecture was:

```text
Dimensions
    ↓
1 : *
    ↓
Fact tables
```

However, historical ERP records contained inconsistent vehicle keys and relationships across sales, stock and vehicle-intake exports.

I validated:

- Data types
- Null keys
- Duplicate keys
- Vehicle identifiers
- Cardinality
- Alternative relationship strategies

but Power BI still could not maintain the required physical one-to-many relationships reliably across the model.

---

# Why I used TREATAS

Rather than forcing unstable relationships, I moved to:

> **Disconnected dimensions + DAX measures using `TREATAS`**

Dimensions such as:

- Date
- ERP Phase
- Vehicle

remain logically separated from the fact tables.

The required filter context is then applied inside the relevant measures at calculation time.

Conceptually:

```text
User selects Year / ERP Phase / Brand
                ↓
        Disconnected Dimension
                ↓
             TREATAS
                ↓
     Sales / Vehicle / Stock context
                ↓
              KPI
```

This was a deliberate modeling decision after exhausting the physical-relationship approach — not the initial design choice.

It allowed the dashboard to preserve intuitive slicers and business behavior despite limitations in the available ERP exports.

With direct database access, the preferred production architecture would be a properly modeled SQL layer feeding a conventional dimensional model.

---

# Business impact

This MVP was used to demonstrate that useful business intelligence could be extracted from data that already existed inside the organization.

Even without direct SQL access, **Excel + Power Query + DAX + Power BI** provided:

- The first combined view of **historical sales performance, data quality and current stock context**
- Visibility into where catalog-quality problems were concentrated
- A way to prioritize remediation based on commercial relevance
- Separation between revenue-driving brands and volume-driving parts
- Brand-level drill-down for operational analysis
- Evidence that a more mature data architecture could improve decision-making

Most importantly, it provided a concrete internal example of what could become possible by connecting:

> **ERP / SQL → Data Transformation → Power BI → Business Decisions**

rather than relying only on operational ERP screens and manual Excel analysis.

---

# Next stage

The MVP also defined the roadmap for a more scalable analytical system:

### Data architecture

- Direct SQL access
- Proper dimensional/star schema
- Stable master keys for vehicles and parts
- Automated refresh pipeline

### Commercial analytics

- Profitability by brand
- Profitability by part family
- Vehicle acquisition ROI
- Average time-to-sale
- Inventory turnover
- Stock aging
- Dead-stock detection
- Pricing opportunities

### Operational analytics

- Which vehicles should receive dismantling priority?
- Which parts should be cataloged first?
- Which references need better technical data?
- Which inventory is selling quickly?
- Which inventory is consuming capital without enough rotation?

The original dashboard was therefore not intended as the final analytical system.

It was the **proof of concept that justified building one.**

---

## Stack

**Power BI · DAX · Power Query (M) · Excel · Dimensional Modeling · Data Quality**

### Technical highlights

- Historical ERP data cleaning
- Power Query transformation pipelines
- Composite Data Quality Index
- Disconnected dimensions
- Advanced DAX with `TREATAS`
- Time-series analysis
- Revenue vs. volume segmentation
- Interactive brand drill-down
- Confidential portfolio demo through transformed dates and values

---

**Tags:** Power BI, DAX, Power Query, Business Intelligence, Data Analysis, Data Quality, Data Modeling, Automotive, Excel
