---
layout: post
title: "🚗 MVP Homologación de Vehículos — Consolidación técnica multi-fuente"
date: 2026-01-22 12:00:00 -0300
categories: [mvp, product]
project_type: mvp
tags: [MVP, Product, FastAPI, React, Web Scraping, Data Normalization, Automation]
image: "/assets/img/thumb.png"
---

🚀 MVP para **automatizar la recolección, normalización y consolidación de especificaciones técnicas de vehículos** desde múltiples fuentes web, orientado a equipos de homologación y validación técnica.

<!--more-->

## 🧠 Contexto / problema

En procesos de homologación, los datos técnicos de un vehículo suelen estar **dispersos en varios portales**, con **formatos, nomenclaturas y niveles de detalle distintos**.

El trabajo manual de comparar fuentes:
- consume mucho tiempo,
- es propenso a errores,
- y dificulta la **trazabilidad por expediente**.

A medida que aumenta la presión por **acortar ciclos de validación** y reducir retrabajos documentales, este enfoque deja de escalar.

---

## 🛠️ Qué hace el MVP (valor operativo)

El sistema permite:

- Ingresar hasta **3 URLs de referencia** por vehículo.
- Ejecutar **scraping concurrente** de cada fuente.
- **Unificar campos técnicos clave** en una vista única editable.
- Exportar el resultado final en una **plantilla documental** lista para uso operativo.

El output actúa como una **ficha técnica homologada**, con origen controlado y consistencia entre fuentes.

---

## ⚙️ Cómo lo hace (alto nivel)

**Backend (FastAPI)**
- Scraping concurrente por fuente.
- Transformación de cada origen a un **esquema común**.
- Reglas simples de **priorización por campo**.
- Endpoints autenticados para procesamiento y exportación.

**Frontend (React)**
- Autenticación de usuarios.
- Vista comparativa: datos extraídos / consolidados.
- Edición manual del valor final por campo.
- Historial de exportaciones por usuario.

---

## ⭐ Diferenciales frente al proceso manual

- 🔗 **Un solo flujo**: extracción + normalización + priorización + exportación.
- 🧾 **Trazabilidad**: historial de descargas y ediciones por usuario.
- ⏱️ **Reducción drástica de tiempos** frente a comparación manual.
- 📦 **Base escalable** para futuras integraciones y reglas avanzadas.

---

## 🧱 Alcance del MVP

**Incluye**
- Scraping multi-fuente en paralelo.
- Consolidación por campo técnico.
- Edición del valor final.
- Exportación documental.
- Autenticación y seguimiento básico.

**No incluye**
- Integraciones directas con ERP / PLM / CRM.
- Motor avanzado de reglas por país o normativa específica.

---

## 📈 Métricas de éxito

- **KPI principal:** tiempo promedio desde URLs → ficha homologada exportada.
- **Secundaria:** reducción de discrepancias entre fuentes y retrabajo manual por expediente.

---

## 🗺️ Roadmap

**Siguiente iteración**
- Validaciones automáticas de calidad de dato.
- Observabilidad del pipeline de scraping.

**Iteración 2**
- Reglas configurables por mercado / modelo.
- Plantillas de salida adicionales.

**Iteración 3**
- Integración vía API con sistemas internos.
- Panel analítico de productividad y errores.

---

## 🧪 Estado actual

MVP funcional con:
- autenticación,
- procesamiento multi-fuente,
- edición manual,
- exportación de fichas técnicas.

---

## 🧰 Stack

- **Frontend:** React, TypeScript, Vite, TailwindCSS, Axios  
- **Backend:** FastAPI, Uvicorn, pandas, BeautifulSoup / requests  
- **Plataforma:** Supabase (Auth + DB), docxtpl / python-docx

---

## 🔒 Notas de confidencialidad

- Datos anonimizados.
- Valores y endpoints omitidos.
- Sin exposición de credenciales o información sensible.
