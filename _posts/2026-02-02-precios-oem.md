---
layout: post
title: "💶 Precios OEM: Automatización de pricing para autopartes usadas"
date: 2026-01-23 12:00:00 -0300
categories: [automation, python, scraping]
project_type: automation
tags: [Python, Pandas, Playwright, Web Scraping, Pricing, Automation, Testing]
image: "/assets/img/thumb.png"
---

🚀 Automatización de pricing para autopartes usadas: procesa archivos de stock, consulta mercado y devuelve un output consolidado con **rango de precios sin IVA** y una señal de **oferta** por referencia OEM.

<!--more-->

## 🧠 Contexto / problema

En un desguace, poner precio no es solo completar un campo: implica tomar decisiones rápidas sobre piezas con distinta rotación, competencia cambiante y publicaciones con calidad dispar.

Cuando este trabajo se hace de forma manual, aparecen varios problemas:

- tiempo operativo alto para revisar pieza por pieza
- criterios poco consistentes entre búsquedas
- dependencia de consultas repetitivas
- dificultad para escalar el proceso sobre lotes grandes

Este proyecto nace para resolver esa fricción: transformar una búsqueda manual de mercado en un flujo **repetible, auditable y usable como base de pricing**.

---

## ⚙️ Qué hace el programa

A partir de un archivo de entrada en **CSV, XLSX o XLS**, el script procesa la columna OEM y genera una salida consolidada con información útil para pricing.

Para cada referencia:

- realiza la búsqueda automatizada en la web
- detecta resultados válidos
- extrae precios publicados sin IVA
- calcula un rango de mercado
- devuelve una señal de oferta según la cantidad de coincidencias encontradas

### Output principal

El resultado final queda listo para análisis o reglas internas de precio, con campos del tipo:

- `OEM`
- `Units`
- `Min Price`
- `Max Price`

Esto permite usar el output como insumo para decisiones posteriores, por ejemplo:

- margen objetivo
- prioridad comercial
- stock disponible
- rotación esperada
- reglas de pricing dinámico

---

## 🛠️ Diseño técnico

### Navegación automatizada con Playwright

La herramienta utiliza **Playwright con Chromium** para ejecutar búsquedas de forma consistente y reproducible, con soporte para modo headless y headful según necesidad de operación o debugging.

### Procesamiento orientado a lotes

El script fue pensado para trabajar sobre archivos reales de operación, con estructura simple de carpetas:

- `Input/`
- `Output/`
- `Done/`

Además prioriza **CSV** cuando hay más de un formato disponible, para facilitar su uso en entornos más restringidos o flujos batch.

### Robustez en el cálculo de precios

Uno de los puntos más importantes del proyecto fue mejorar la calidad del rango devuelto, evitando que el resultado final quede contaminado por publicaciones irrelevantes o valores extremos.

#### 1) Filtro por relevancia de título

Como la búsqueda del sitio es de texto libre, una OEM o query puede devolver piezas de otra categoría. Para reducir ese ruido, cada resultado se contrasta contra la búsqueda original y solo se conservan los títulos suficientemente alineados.

En la práctica, esto ayuda a evitar casos como:

- buscar un turbo y recibir un motor completo
- buscar una referencia específica y capturar piezas que solo comparten una palabra genérica

#### 2) Filtro estadístico de outliers

Sobre los precios recolectados se aplica un segundo filtro usando percentiles **P5/P95**, descartando valores extremos antes de informar el rango final.

Esto mejora la señal de pricing y reduce el impacto de publicaciones anómalas, errores de carga o listados poco representativos.

### Configuración operativa

La ejecución se controla por CLI, con parámetros para:

- tamaño de lote
- fila inicial
- delay entre filas
- timeout
- proxy
- paginación
- cantidad máxima de precios
- modo verbose
- movimiento automático a `Done/`

Esto vuelve al script flexible para corridas chicas, diagnósticos puntuales o procesamiento más amplio.

---

## ✅ Calidad y mantenimiento

El proyecto no quedó como un script aislado: se fue endureciendo para uso más confiable.

### Estado actual

- **Playwright + Chromium operativo**
- **22 tests pasando con pytest**
- utilidades auxiliares para limpieza y mantenimiento
- README público simplificado para visualización del repositorio

### Testing

Se incorporaron pruebas automatizadas para validar partes críticas del comportamiento del programa, lo que mejora mantenibilidad y reduce el riesgo de romper lógica al iterar.

---

## 📈 Impacto

Este proyecto convierte una tarea manual de consulta y comparación de precios en un proceso mucho más consistente.

### Valor operativo

- reduce tiempo de búsqueda y revisión manual
- mejora la repetibilidad del criterio de pricing
- entrega una base objetiva de mercado por referencia
- permite trabajar sobre lotes más grandes sin multiplicar esfuerzo manual

### Valor técnico

- automatización web robusta
- procesamiento batch configurable
- lógica de filtrado para mejorar calidad del dato
- tests automatizados para sostener evolución del proyecto

---

## 🧭 Dónde encaja dentro del sistema

Este módulo está pensado como una pieza dentro de un flujo mayor de catalogación y pricing:

1. validación de OEM  
2. captura o enriquecimiento de referencias  
3. cálculo de rango de mercado  
4. aplicación de reglas internas de precio  
5. publicación o actualización masiva

En ese sentido, no resuelve solo “scraping de precios”: aporta una base concreta para un sistema de pricing más automatizado.

---

## 🖼️ Capturas del resultado final

### 1) Input
![Input](/assets/img/projects/precios-oem/input-precios-oem.png)

### 2) Output
![Output](/assets/img/projects/precios-oem/output-precios-oem.png)

---

## 🧰 Stack

🐍 **Python** · 📊 **Pandas** · 🎭 **Playwright** · 🧪 **Pytest** · 🧹 **Black / Flake8** · 🧾 **CLI**
