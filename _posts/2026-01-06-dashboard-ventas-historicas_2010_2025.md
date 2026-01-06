---
layout: post
title: "De Excel a Power BI: Optimizando la Inteligencia de Negocios"
date: 2026-01-04 12:00:00 -0300
categories: [powerbi, data-analysis, etl]
image: "/assets/img/thumb.png"
---

Proyecto BI end-to-end para una **empresa de desguace automotor (España)**: transformé exportaciones de un ERP legado + Excel en un modelo analítico en Power BI para **diagnosticar calidad de datos** y **medir performance comercial** (2010–2025).  
Resultados: visibilidad operativa, detección de inconsistencias críticas (p. ej. registros “genéricos”) y base técnica para migración futura a SQL.

<!--more-->

## 🚀 Resumen del Proyecto

Como parte de una estrategia de digitalización, desarrollé un tablero analítico integral para diagnosticar la calidad de los datos históricos y medir el rendimiento comercial (2010–2025).

El objetivo principal fue transformar exportaciones estáticas de un ERP legado en un modelo de datos dinámico que permitiera la toma de decisiones basada en evidencia.

---

## 🧩 El Desafío: Del Caos a la Estructura

El reto principal residía en la infraestructura de datos. Al carecer de acceso directo a SQL, el análisis dependía de exportaciones de Excel ("Alta", "Ventas" y "Stock") que no estaban diseñadas para un modelo analítico.

Los problemas principales incluían:
* **Datos sucios:** inconsistencias en fechas y textos.
* **Claves inexistentes:** dificultad para cruzar ventas con stock histórico.
* **Silos de información:** tablas desconectadas.

---

## 🛠️ La Solución Técnica

Implementé una solución utilizando **Power BI** y **Excel**, enfocándome en tres pilares:

### 1. Ingeniería de Datos (ETL)

Para normalizar la información, creé procesos que logran:
* Generar claves sintéticas (`VehiculoKey`) para unificar tablas.
* Normalizar el campo "Estado Stock".
* Crear indicadores de calidad (ej. `Coherente_AltaVsVenta`).

### 2. Modelado Avanzado (DAX)

Debido a las limitaciones de las claves originales, el modelo relacional estándar no era suficiente.

> **Highlight técnico:** utilicé la función **`TREATAS`** en DAX para manejar dimensiones desconectadas. Esto permitió propagar filtros entre tablas que no tenían una relación física directa.

---

## 📊 Resultados e Insights Clave

El dashboard final se dividió en dos lienzos estratégicos:

### A. Diagnóstico de Calidad de Datos

Logramos visualizar por primera vez la “salud” de la información:
* **Índice de calidad:** semáforo que evalúa completitud y consistencia.
* **Problema “Genérico”:** se detectó un volumen crítico de ventas categorizadas como “GENÉRICA”, lo que impedía análisis de rentabilidad. Esto impulsó un cambio inmediato en la política de registro.

### B. Rendimiento Comercial (2020–2025)

Se habilitó la visión estratégica del negocio mediante:
* **Top 10 marcas:** identificación de marcas que sostienen facturación.
* **Análisis de rotación:** piezas con mayor salida real.
* **Tendencias:** estacionalidad y ticket promedio.

---

## 🔮 Conclusión y Próximos Pasos

Este proyecto sentó las bases para una cultura de datos en la empresa. Los siguientes pasos incluyen:

1. Transición de Excel a conexión directa **SQL**.
2. Construcción de tablas maestras estandarizadas.
3. Integración de costes operativos para calcular márgenes reales.

---

> *Este proyecto demuestra cómo técnicas avanzadas de modelado pueden extraer valor estratégico incluso de sistemas heredados.*

![Vista previa del dashboard](/assets/img/bgimage.png)
*(Nota: datos anonimizados por confidencialidad. Valores ajustados/escala aplicada para publicación.)*

