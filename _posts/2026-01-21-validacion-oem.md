---
layout: post
title: "🔎 Validación OEM en Stock: Eliminando Falsos Positivos a Escala"
date: 2026-01-21 12:00:00 -0300
categories: [automation, python, scraping]
project_type: automation
image: "/assets/img/thumb.png"
tags: [Python, Pandas, Playwright, Web Scraping, Data Validation, Data Quality, Autosave, CLI]
---

🚀 De “creer” que una OEM está bien… a **saberlo con evidencia y trazabilidad**.  
Este proyecto convierte una validación manual (lenta y propensa a errores) en un proceso **repetible, auditable y escalable** para catálogos de repuestos usados.

<!--more-->

## 🧩 Contexto / problema

En catálogos de repuestos usados, el dolor más grande no es “tener datos”, sino **confiar en ellos**.  
La pregunta que disparó este proyecto fue simple y concreta:

> **¿Cómo eliminamos los falsos positivos de OEM a escala?**

Un falso positivo termina generando:

- 🧨 publicaciones **rotas** o difíciles de encontrar
- 🧾 títulos / descripciones inconsistentes
- 💸 pricing desalineado
- 🧯 pérdida de confianza en el trabajo operativo

---

## 🧠 ¿Qué hace el programa?

Este script automatiza la **validación masiva de referencias OEM** contra un verificador, tomando como input un **Excel de stock**.

Para cada fila:

1. Lee **OEM original** y **OEM paralelo** (si existen).
2. Construye una **URL de búsqueda real** (parámetros equivalentes a navegación humana).
3. Navega con **Playwright (headless)**, aplica **scroll + paginación** y **cuenta coincidencias**.
4. Devuelve una señal tipo “semáforo” (0–N) en columnas:
   - `Validacion Original`
   - `Validacion Paralelo`

✅ Resultado: una señal objetiva de si esa OEM tiene **presencia real** y cuánta **evidencia** hay.

---

## ⚙️ Optimización aplicada (sin perder efectividad)

El objetivo fue mantener exactitud, pero reducir tiempos en ejecuciones grandes.

Mejoras clave:

- ♻️ **Reuso de browser/context/page** para miles de búsquedas (menos overhead por fila).
- ⚡ **Caché por OEM**: si el código se repite, **no se vuelve a consultar**.
- 🧼 **Reglas anti-ruido**: evita búsquedas con caracteres problemáticos (espacios, `-`, `/`, etc.) para reducir consultas inválidas.
- 💾 **Autosave incremental**: guarda parciales cada *N* filas para tolerar cortes y mantener trazabilidad.
- 🛑 **Early stop por tope de interés**: si ya hay suficientes coincidencias (ej. 30), corta paginación/scroll y pasa a la siguiente fila  
  → acelera mucho sin cambiar la lógica del semáforo.

---

## 🧪 Caso real: Stock Ford (8.542 filas)

Se ejecuta sobre un stock completo, validando OEM original/paralelo a escala y generando un output **auditable** para:

- detectar publicaciones débiles o inconsistentes
- priorizar limpieza de datos (nombre de pieza / descripción / OEM)
- asegurar que el catálogo publicado refleja el stock real con calidad

---

## ⭐ Impacto en operación

Con esta validación automatizada se logra:

- ✅ **Confianza:** sabemos qué piezas están bien publicadas y encontrables.
- ✅ **Calidad de catálogo:** mejora de título, descripción y normalización de OEM.
- ✅ **Precio mejor alineado:** al eliminar incertidumbre de publicación, el pricing posterior es más consistente.
- ✅ **Priorización:** identifica dónde actuar primero (OEMs sin evidencia o con evidencia baja).

---

## 🧭 Roadmap

Este validador es una pieza dentro de un pipeline mayor:

- 📸 **MVP ERP–OCR:** extracción automática de OEM y datos desde imágenes/documentos.
- 📊 **Scraping de precios semanal:** cálculo de precio competitivo.
- 📈 **Reglas de pricing variable:** stock interno + competencia + objetivos de margen.
- 🔗 **Cruce publicación + stock + competencia** para decisiones de catálogo basadas en datos.

---

### ✅ Captura del resultado final 

![Output](/assets/img/projects/validacion-oem/validacion-oem.png)

---

## 🧰 Stack

🐍 **Python** · 🎭 **Playwright** · 📊 **Pandas + OpenPyXL** · 🧪 **CLI** · ⚡ **caché** · 💾 **autosave**  
Diseño orientado a **robustez** y **escala**.
