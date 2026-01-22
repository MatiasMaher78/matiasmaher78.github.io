---
layout: post
title: "🔧 Validación OEM Automática para Catálogo de Autopartes"
date: 2025-01-10 12:00:00
categories: [automation, scripts]
ptype: automation
image: "/assets/img/thumb.png"
---

🚀 Automatiza la **validación de referencias OEM** para reducir *falsos positivos* y asegurar publicaciones correctas en eCommerce.

<!--more-->

## 🧠 ¿Qué hace?

Procesa un **Excel de stock** y verifica, para cada referencia OEM (original/paralelo), cuántas **coincidencias reales** existen en la plataforma de mercado mediante búsqueda automatizada.  
Clasifica resultados con un **semáforo de calidad** para priorizar revisiones y evitar piezas mal publicadas:

- 🔴 **0 coincidencias** → *Revisar*  
- 🟡 **1–2 coincidencias** → *Atención*  
- 🟢 **≥ 3 coincidencias** → *OK*

Incluye:
- 🧩 **Ejecución por lotes** (batch) para stocks grandes  
- ⚡ **Caché** para acelerar consultas repetidas  
- 💾 **Autosaves periódicos** para tolerancia a fallos en corridas largas  

---

## ⭐ Highlights

- ✅ **Calidad de datos:** reduce errores de carga (falsos positivos) *antes* de publicar, elevando la confiabilidad del catálogo.  
- ⏱️ **Eficiencia operativa:** permite validaciones masivas por stock/marca y habilita un flujo **incremental** para piezas nuevas posteriores a la fecha de validación.  
- 🛡️ **Listo para producción:** parámetros por CLI, reuso de navegador (performance), caché por código, manejo de cookies/scroll/paginación y guardado parcial automático.

---

## 🧰 Stack

🐍 **Python** · 📊 **Pandas** (Excel I/O) · 🎭 **Playwright** (automatización web) · 🧪 **CLI** (argparse) · ⚡ caché + 💾 autosave

