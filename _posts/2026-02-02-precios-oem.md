---
layout: post
title: "💶 Calculo de Precios OEM — Automatización de pricing"
date: 2026-01-23 12:00:00 -0300
categories: [automation, scripts]
project_type: automation
tags: [Python, Pandas, Playwright, Web Scraping, Pricing, Automation]
image: "/assets/img/thumb.png"
---

🚀 Automatización de **pricing** para autopartes usadas: obtiene **rango de mercado (Min/Max)** y una señal de **oferta (Units)** por referencia OEM, con scraping robusto y trazabilidad.

<!--more-->

## 🎯 Contexto / problema

En un desguace, el pricing no es “poner un número”: es **decidir rápido y con datos** en un mercado donde la oferta cambia día a día y los precios publicados suelen venir con **inconsistencias** (formatos, descuentos, fichas incompletas).  
Hacerlo manual o depender de terceros frena la operación, encarece el proceso y limita la escalabilidad.

Este proyecto nace para darle a la empresa **autonomía real**: obtener automáticamente un **rango de precios de mercado** y una señal de **oferta/demanda por referencia**, como base para una **lista de precios dinámica** integrada a procesos internos.

---

## ✅ Qué hace el programa

A partir de un archivo de stock (**CSV o Excel**), el script toma la columna **OEM** y, para cada fila:

- Busca en la web simulando navegación real (**URL parametrizada + codificación Base64**).
- Extrae en modo masivo:
  - **Units**: proxy de oferta (cantidad de resultados / enlaces detectados).
  - **Min Price / Max Price**: rango de mercado (**parse robusto**).
- Devuelve un output limpio listo para pricing:
  - `ID | OEM | Units | Max Price | Min Price`

Este output se usa como insumo directo para reglas internas: **margen objetivo, rotación, stock disponible, prioridad comercial**, etc.

---

## 🧩 Diseño técnico (producción-friendly)

### 🌐 Automatización web con Playwright (Chromium)
- **Headless** por defecto (y modo **headful** para debug).
- **Locale** y **user-agent** realistas para consistencia.
- Manejo automático de **cookies**.

### ⚡ Extracción optimizada (performance sin perder estabilidad)
- Bloqueo de recursos no esenciales: imágenes, fuentes, estilos, ads/analytics.
- Paginación limitada (`max_pages=5`) + scroll controlado (`scroll_rounds=3`).
- **Early exit** cuando:
  - ya hay suficientes precios (≥ 50),
  - la página devuelve menos que `per_page`,
  - o no aparecen precios.

### 🧠 Fix crítico de timing 
Se agregó una **espera inicial** después de `goto()` para permitir que la web renderice cards vía JavaScript.  
Sin esto, se veía HTML “básico” pero el scraper obtenía **0 links/precios** (fallo silencioso).

### 🛡️ Fallback inteligente (evita falsos positivos)
Si una query devuelve 0 resultados, el sistema reintenta con el **token alfanumérico más relevante** (ej. OEM/código), evitando palabras genéricas del nombre de pieza.

**Ejemplo:**  
`"CAJA MARIPOSA AIRE 9640795280" → reintento con 9640795280`

### 🧾 Trazabilidad y debug
- Si una fila queda sin resultados, puede **guardar HTML** de la búsqueda en `Output/` para diagnosticar cambios del sitio o edge cases.

### ♻️ Caché persistente
- Guarda `Output/cache_oem.json` para reutilizar resultados entre ejecuciones y **acelerar corridas repetidas** (clave en lotes grandes).

### 📥 Input robusto (CSV-first)
- Prioriza **CSV** para evitar dependencias en entornos restringidos.
- Detección de separador (`;` vs `,`) + fallback de encoding.
- Si hay Excel, intenta leerlo con dependencias estándar.

---

## 📈 Impacto

Este script convierte un proceso manual y dependiente en un flujo **repetible y escalable**:

- Reduce fricción operativa para tomar decisiones de precio.
- Provee señales de mercado (**rango + oferta**) con consistencia.
- Acelera iteraciones de pricing sin pedir datos a terceros.
- Deja lista la base para una **lista de precios dinámica**.

---

## 🗺️ Roadmap (integración futura)

Pensado como un módulo dentro de un pipeline mayor:

1. **Validación_OEM** → asegurar calidad de referencia y publicación correcta.  
2. **MVP ERP + OCR** → cargar/validar OEM desde imágenes y actualizar fichas en ERP.  
3. **Pricing dinámico** → `precio = f(rango mercado, stock, rotación, demanda, margen)`  
4. **Automatización de publicación** → actualización masiva con trazabilidad.

---

## 🖼️ Capturas del resultado final

### 1) Input
![Input](/assets/img/projects/precios-oem/input-precios-oem.png)

### 2) Output
![Output](/assets/img/projects/precios-oem/output-precios-oem.png)

---

## 🧰 Stack

- **Python**, **Pandas**
- **Playwright (sync)** para navegación automatizada
- **Regex / parsing** para normalización de precios
- **JSON cache** para performance y repetibilidad

