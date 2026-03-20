# Desempeño de índices espectrales en la detección de áreas quemadas) 🛰️🔥

Arquitectura geoespacial de tres niveles para la detección, emparejamiento biofísico en campo y extracción espectral de índices de fuego en la selva alta, combinando procesamiento en la nube y computación móvil en terreno.

## 📌 Arquitectura del Proyecto

El flujo de trabajo se divide en tres módulos operativos diseñados para superar la alta nubosidad y complejidad topográfica de la selva alta:

### Fase 1: Scouting y Planificación Logística (GEE)
* **Objetivo:** Filtrado de ruido térmico y delimitación de áreas de búsqueda.
* **Proceso:** Ingesta de datos vectoriales FIRMS (vía NASA LANCE API) para preservar la resolución espacial VIIRS y MODIS.
* Generación dinámica de cuadrículas de inspección sobre el límite provincial (FAO/GAUL) cruzadas con colecciones Sentinel-2 bitemporales para evaluación visual previa al despliegue.

### Fase 2: Emparejamiento Biofísico en Terreno (Python Mobile)
* **Objetivo:** Supresión del sesgo topográfico y fenológico en el muestreo de validación.
* **Proceso:** Despliegue de un algoritmo desarrollado en Python (ejecutado offline vía Pydroid 3 en dispositivos móviles durante la expedición). El script ingesta la matriz biofísica pre-incendio (Elevación, Pendiente, Orientación, NDVI) y calcula en tiempo real las coordenadas óptimas para el establecimiento de los Puntos de Control (Clase 0) que coincidan paramétricamente con las cicatrices de fuego confirmadas (Clase 1).(Tolerancias: Elev ±50m, Pendiente ±10%, Orientación ±45°, NDVI ±0.05)

### Fase 3: Extracción Hiperespectral y Análisis Estadístico (GEE + Python)
* **Objetivo:** Cálculo de severidad mediante índices relativizados y absolutos.
* **Proceso:** Composición de medias bitemporales (± 10 días) con enmascaramiento estricto de nubes sobre imágenes Sentinel-2. Extracción de índices (dNBR, RBR, RdNBR) remuestreados a resolución unificada de 10 metros para 50 puntos validados. Análisis estadístico posterior (Curvas ROC, AUC) para determinar el umbral óptimo de detección.

## ⚙️ Stack Tecnológico
* **Cloud Computing:** Google Earth Engine (JavaScript API).
* **Computación Móvil (Campo):** Python 3 (Pydroid 3, Pandas, Numpy).
* **Fuentes de Datos:** Sentinel-2 SR (Copernicus), NASA LANCE FIRMS, SRTM (30m).
