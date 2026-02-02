---
layout: post
title: "📈 Ventas Históricas + Calidad de Datos (Repuestos Usados)"
date: 2026-01-06 12:00:00 -0300
categories: [powerbi, data-analysis, bi]
project_type: bi
tags: [Power BI, DAX, Data Quality, Data Modeling, ETL, Excel, Business Intelligence]
image: "/assets/img/thumb.png"
permalink: /powerbi/data-analysis/bi/2026/01/06/dashboard-ventas-historicas_2010_2025.html
---

Proyecto BI end-to-end para una **empresa de repuestos usados**.  
Dashboard en Power BI para **entender performance comercial** y **medir calidad del catálogo** en segundos, teniendo en cuenta que la extraccion de datos fue limitada desde el ERP.

<!--more-->

## 🎯 Objetivo del proyecto

Este informe permite responder rápido preguntas típicas de dirección y operaciones:

- ¿Cómo evolucionan **ventas** y **ticket promedio** por periodo?
- ¿Qué **marcas** concentran facturación en el tramo reciente?
- ¿Qué **familias de piezas** rotan más por unidades?
- ¿Cual es la calidad de datos para publicar las piezas? 

---

## 🧩 Qué incluye el dashboard

### 1) 🌍 Vista General (2017–2032 Demo)

- Filtros por **AÑO (Demo)**, **Periodo ERP** (*Pre-ERP, Transición, Consolidación, Optimización*) y **Marca**.  
- KPIs: **Importe Neto € (Demo)**, **Ventas Netas (Demo)**, **Ticket Promedio Neto € (Demo)**.  
- Calidad de datos:
  - **Datos técnicos completos** (*marca / modelo / motor*).
  - **Coherencia Alta vs Venta** (control de consistencia entre alta de piezas y ventas).  
- Ranking de rotación histórica: **Top piezas vendidas 2017–2032 (Demo)** por unidades (familias típicas de alta demanda como alternadores, faros, baterías, pilotos, llantas, amortiguadores, etc.).

### 2) 📆 Vista Performance Reciente (2027–2032 Demo)

- **Evolución mensual** del Importe Neto € (Demo) para detectar tendencia y estacionalidad.
- **Top 10 marcas** por Importe Neto € (Demo) para identificar concentración de facturación.
- **Top 20 piezas** por unidades vendidas (Demo) con foco en carrocería/rotación (paragolpes, faros, llantas, pilotos, retrovisores, aletas, capó, etc.).

### 3) 🔍 “Radiografía de Marca” (ejemplo: TOYOTA)

- KPIs específicos de la marca para 2027–2032 (Demo).
- Curva mensual de ventas + ranking de piezas más vendidas por unidades.
- Útil para decisiones operativas: **priorización de catalogación**, **foco de stock**, **mejoras de ficha** y **control de devoluciones**.

---

## 🧠 Modelado y enfoque técnico (por qué esto es “accionable”)

El informe está construido sobre **exportes de ERP + Excel**, aplicando:

- **Tablas CLEAN** (normalización y estandarización de campos críticos).
- Dimensiones de **Fecha / Periodo ERP / Vehículo / Pieza / Marca**.
- Medidas **DAX** para KPIs y calidad.
- Patrones avanzados para lidiar con origen “imperfecto”: **dimensiones desconectadas + TREATAS**, habilitando análisis estable aún con claves inconsistentes o relaciones frágiles.

---

## ⭐ Highlights (valor para negocio)

- ✅ **Visibilidad ejecutiva en 5 minutos:** performance + calidad en una sola pantalla.
- 🧹 **Calidad de datos como KPI:** la ficha técnica deja de ser “opinión” y se vuelve medible.
- 🎯 **Priorización operativa:** identifica qué marcas/piezas conviene atacar primero para maximizar ventas futuras.
- 🧱 **Base escalable:** deja preparado el camino para migración a **SQL / ETL formal** sin rehacer el reporting.

---

## 🖼️ Capturas del Dashboard

### 1) Calidad de Datos – General
![Calidad de Datos - General](/assets/img/projects/ventas-historicas/01-calidad-general.png)

### 2) Ventas – General
![Ventas - General](/assets/img/projects/ventas-historicas/02-ventas-general.png)

### 3) Ventas – Toyota
![Ventas - Toyota](/assets/img/projects/ventas-historicas/03-ventas-toyota.png)

---

## 🧰 Stack

📊 **Power BI** · 🧠 **DAX** · 🧩 **Data Modeling** · 🧼 **ETL (Power Query)** · 📄 **Excel/ERP Exports** · 🧪 **Calidad de Datos**
