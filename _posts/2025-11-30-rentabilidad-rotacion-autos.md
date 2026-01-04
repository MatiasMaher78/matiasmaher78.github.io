---
layout: post
title: "Análisis de Ventas y Stock de Vehículos (2010-2025)"
subtitle: "Dashboard interactivo en Power BI y limpieza de datos con Python"
cover-img: /assets/img/portada-ventas.jpg
thumbnail-img: /assets/img/thumb-ventas.jpg
share-img: /assets/img/portada-ventas.jpg
tags: [powerbi, python, data-analysis, automotriz]
---

### 1. El Problema de Negocio
La empresa necesitaba optimizar las compras estratégicas basándose en el histórico de ventas y altas de vehículos en España de los últimos 15 años.

### 2. La Solución Interactiva
Desarrollé este tablero en Power BI que permite filtrar por año, marca y tipo de vehículo.

<iframe title="Report Section" width="100%" height="600" src="PEGAR_AQUI_TU_ENLACE_DE_POWER_BI" frameborder="0" allowFullScreen="true"></iframe>

### 3. Metodología y Código
Para asegurar la calidad de los datos, utilicé **Python** para la limpieza preliminar antes de cargar el modelo.

**Desafío técnico:** Normalizar los nombres de las marcas de coches que venían con errores de tipeo.
```python
# Ejemplo de limpieza con Pandas
import pandas as pd

df['marca'] = df['marca'].str.upper().str.strip()
print("Datos normalizados correctamente")
