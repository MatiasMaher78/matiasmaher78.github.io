---
layout: post
title: "🚗 Vehicle Data Print — MVP para homologación vehicular"
date: "YYYY-MM-DD 12:00:00 -0300"
categories: [mvp, product]
project_type: mvp
tags: [MVP, Product, FastAPI, React, Supabase, Automation, Compliance]
image: "/assets/img/thumb.png"
---

🚀 MVP para homologación vehicular que transforma un proceso manual, lento y propenso a errores en un flujo mucho más ágil: extrae datos técnicos desde fuentes especializadas, los consolida y genera un documento profesional listo para presentar.

<!--more-->

## 🧠 Contexto / problema

Homologar un vehículo implica recopilar información técnica de distintas fuentes, contrastarla, resolver discrepancias y preparar un documento final en el idioma correcto.

Cuando ese trabajo se hace manualmente, el costo operativo crece rápido:

- hay que revisar varias fuentes por cada vehículo
- aparecen diferencias entre datos que deben resolverse a mano
- se repite mucho trabajo administrativo
- el tiempo de entrega depende demasiado de tareas manuales
- aumenta el riesgo de errores en campos sensibles del documento final

En la práctica, no era solo “buscar datos”: era sostener un proceso técnico repetitivo que consumía horas y no escalaba bien.

---

## 💡 Solución

**Vehicle Data Print** centraliza ese flujo en una sola aplicación.

El usuario carga hasta **3 URLs** de fuentes especializadas y la plataforma:

1. **extrae automáticamente** los datos técnicos del vehículo
2. **fusiona y reconcilia** la información usando reglas predefinidas
3. **presenta el resultado** en una interfaz editable y ordenada por secciones
4. **genera un documento DOCX** en el idioma elegido, listo para entregar

El objetivo del producto no es solo ahorrar tiempo, sino también **estandarizar** cómo se construye la documentación técnica y reducir dependencia de tareas manuales de bajo valor.

### Capacidades principales

- procesamiento simultáneo de **3 fuentes especializadas**
- gestión de **121 campos técnicos** por vehículo
- exportación en **10 idiomas**
- soporte para vehículos **híbridos y eléctricos**
- historial de documentos generados por usuario
- acceso autenticado, con visibilidad restringida a la actividad propia

---

## ⚙️ Arquitectura

La aplicación sigue una arquitectura web clásica, separando interfaz, lógica de negocio y persistencia.

### Componentes principales

- **Frontend en React + TypeScript**  
  interfaz para cargar URLs, revisar datos, editar campos y disparar la exportación

- **Backend en FastAPI**  
  orquesta scraping, transformación, reconciliación y generación del documento

- **Capa de datos con Supabase / PostgreSQL**  
  autenticación de usuarios, persistencia del historial y almacenamiento de plantillas

### Flujo general

**Input → extracción → reconciliación → revisión → exportación**

1. El usuario autenticado ingresa hasta 3 URLs
2. El backend consulta las fuentes y extrae sus datos
3. Cada fuente se transforma a un esquema interno común
4. El sistema fusiona resultados con reglas de prioridad
5. El usuario revisa y corrige si hace falta
6. Se genera el DOCX final y se registra la descarga

---

## ⭐ Diferenciales

- **Reduce fricción operativa:** convierte un proceso de varias horas en un flujo mucho más corto y controlado
- **Mejora consistencia:** aplica criterios fijos para resolver conflictos entre fuentes
- **Permite revisión humana:** no obliga a confiar ciegamente en la automatización; deja el dato editable antes de exportar
- **Escala mejor:** evita repetir el mismo trabajo manual vehículo por vehículo
- **Tiene orientación de producto:** autenticación, historial, estados y exportación profesional, no solo un script aislado

---

## 📈 Impacto

Este MVP apunta a resolver un problema operativo concreto: **documentar vehículos de forma más rápida, consistente y trazable**.

### Valor para perfiles no técnicos / RRHH

- reduce trabajo manual repetitivo
- acelera la preparación de documentación técnica
- baja la probabilidad de errores por copia y pega
- ordena un flujo que antes dependía demasiado de revisión artesanal
- muestra capacidad de llevar una necesidad operativa a una solución usable

### Resumen breve para gente técnica

Aplicación full-stack con **React + TypeScript** en frontend y **FastAPI** en backend, diseñada para procesar hasta 3 fuentes por vehículo, mapearlas a un esquema común de **121 campos**, reconciliar conflictos con reglas de prioridad y exportar un **DOCX** multilenguaje. Usa **Supabase** para autenticación, persistencia del historial y almacenamiento de plantillas, y contempla casos específicos como híbridos, eléctricos y diferencias de formato entre fuentes.

---

## 🖼️ Capturas

![Pantalla principal](/assets/img/projects/vehicle-data-print/01-home.png)

![Revisión de datos](/assets/img/projects/vehicle-data-print/02-review.png)

![Exportación de documento](/assets/img/projects/vehicle-data-print/03-export.png)

---

## 🎥 Demo

Pendiente de sumar video breve del flujo completo:
**input → extracción → revisión → exportación**

---

## 🧰 Stack

React · TypeScript · FastAPI · Python · Supabase · PostgreSQL · BeautifulSoup · Pandas · docxtpl
