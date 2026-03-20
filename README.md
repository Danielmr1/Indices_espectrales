# Desempeño de índices espectrales en la detección de áreas quemadas 🛰️🔥 
Arquitectura geoespacial de ciclo completo (Nube -> Terreno -> Nube -> ML) para la detección, emparejamiento biofísico in situ y extracción espectral de índices de severidad de fuego en la Amazonía Andina (Selva Alta). Este proyecto resuelve el cuello de botella del sesgo fenológico y topográfico mediante la integración de Google Earth Engine y computación móvil offline.

📌 Arquitectura del Proyecto
El flujo de trabajo sigue una metodología Expert-in-the-Loop y determinística, dividida en cinco fases operativas:

## Fase 1: Scouting y Planificación Logística (GEE)
### Objetivo: Filtrado de ruido térmico y validación óptica de anomalías.

Proceso: Ingesta de registros FIRMS (NASA LANCE). Se purgan detecciones con confianza < 60% (MODIS) o categorías sub-nominales (VIIRS). Las coordenadas filtradas se cruzan visualmente con composiciones Sentinel-2 en falso color (SWIR/NIR/Rojo) para descartar falsos positivos topográficos y determinar la fecha pre-incendio exacta y libre de nubes.

## Fase 2: Generación de Matriz Biofísica Pre-Fuego (GEE - ETL)
### Objetivo: Construcción del cubo de datos de referencia topográfica y fenológica.

Proceso: Script de extracción que ingesta un modelo SRTM de 30m de la NASA y la imagen Sentinel-2 del día exacto validado en la Fase 1. El motor ejecuta un muestreo espacial sub-píxel en un radio de 1 km alrededor de la coordenada del registro FIRMS, exportando una matriz tabular (CSV con geometría GeoJSON embebida) que contiene la Elevación, Pendiente, Orientación y NDVI base.

## Fase 3: Emparejamiento Biofísico en Terreno (Python Mobile)
### Objetivo: Ejecución offline del muestreo por pares para aislar el factor fuego.

Proceso: Despliegue de un algoritmo táctico en Python ejecutado sin conexión a internet (vía Pydroid 3) por las cuadrillas en la selva. Ingesta la matriz de la Fase 2 y calcula dinámicamente el Punto de Control (Clase 0) homólogo más cercano.

Tolerancias paramétricas estrictas: Elevación (±50m), Pendiente (±10%), NDVI (±0.05) y Orientación (±45°) para asegurar independencia térmica con una distancia mínima de exclusión de >150m del punto de quema.

## Fase 4: Micro-Extracción Espectral Determinística (GEE)
### Objetivo: Extracción los índices espectrales sin contaminación de medias temporales.

Proceso: Arquitectura de extracción por pares. El algoritmo ingesta las coordenadas de campo y las fechas exactas validadas sin nubes (pre y post-incendio). Al prescindir de algoritmos probabilísticos de enmascaramiento (maskS2clouds) y operar con fechas certificadas, extrae la reflectancia espectral pura (Remuestreada a 10m) y calcula los índices absolutos y relativizados (dNBR, RBR, RdNBR) con un 0% de error por ruido atmosférico.

## Fase 5: Inferencia Estadística y Umbrales Óptimos (Python - Scikit-Learn)
### Objetivo: Evaluación de la capacidad discriminatoria de los índices en selva alta.

Proceso: Ingesta de la base de datos final de 50 puntos emparejados. Ejecución de análisis de características operativas del receptor (Curvas ROC) para calcular el Área Bajo la Curva (AUC). Determinación matemática del umbral óptimo de detección de cicatrices utilizando la maximización del Índice de Youden, eliminando la dependencia de literatura extranjera.

⚙️ Stack Tecnológico
Cloud Computing: Google Earth Engine (JavaScript API).

Computación Móvil (Offline): Python 3 (Pydroid 3).

Análisis de Datos & ML: Python (Pandas, NumPy, Scikit-Learn, Matplotlib).

Sensores & Fuentes: Sentinel-2 Nivel 2A (Copernicus), MODIS/VIIRS FIRMS (NASA LANCE), SRTMGL1 (USGS).
