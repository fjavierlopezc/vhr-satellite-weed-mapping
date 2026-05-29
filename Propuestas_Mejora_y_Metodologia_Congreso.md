# Propuesta Metodológica y Mejoras de Teledetección
## Mapeo de Malas Hierbas en Cultivos Herbáceos mediante Satélites de Muy Alta Resolución (Colaboración ESA - Proyecto PP0106977)

---

## 1. Viabilidad de Publicación en Congreso (Resultados Preliminares)

Presentar los resultados preliminares en un congreso de malherbología (por ejemplo, el de la **Sociedad Española de Malherbología - SEMh** o el de la **European Weed Research Society - EWRS**) es una excelente idea:

* **Novedad tecnológica y de datos:** La ESA ha facilitado una cuota muy selecta de imágenes WorldView-2 (0.5m en Pansharpened) y PlanetScope (8 bandas, 3m). Mostrar el potencial de estas nuevas constelaciones de muy alta resolución (VHR) genera gran interés en el sector agronómico, ya que el principal obstáculo para el mapeo de malas hierbas en etapas tempranas ha sido siempre la resolución espacial y espectral.
* **Retroalimentación (Feedback):** Los congresos son foros de discusión idóneos para validar metodologías antes de redactar un artículo científico de alto impacto (Q1). Presentar la problemática de la pureza de píxel en rodales pequeños ("Low") y la influencia de la atmósfera te permitirá recibir aportaciones valiosas de otros investigadores.
* **Planteamiento de futuro honesto:** Es perfectamente aceptable (y común) presentar resultados preliminares y una metodología inicial, indicando en la discusión y conclusiones que el trabajo futuro ya planificado incluye:
  1. Calibración atmosférica mediante modelos de transferencia radiativa física (6S).
  2. Técnicas avanzadas de coregistro mediante GCPs.
  3. Escalabilidad espacial y temporal aplicando modelos de Deep Learning (redes neuronales convolucionales como U-Net).

---

## 2. Propuesta de Contenido Técnico para el Artículo / Resumen

A continuación se detallan las técnicas, modelos y metodologías que se han utilizado en el proyecto hasta el momento y que deben estructurar el documento del artículo para el congreso.

### Metodología Desarrollada

#### Datos de Teledetección Utilizados

| Plataforma / Sensor | Resolución Espacial | Resolución Espectral | Unidades Radiométricas |
| :--- | :---: | :---: | :--- |
| **PlanetScope SuperDove** | 3.0 metros | 8 bandas (incluye RedEdge y NIR) | Reflectancia TOA / BOA (DOS1) |
| **WorldView-2 Multiespectral** | 2.0 metros | 8 bandas (incluye Coastal, Yellow, RedEdge, NIR2) | Nivel Digital (DN) / Calibrado a BOA |
| **WorldView-2 Pansharpened** | 0.5 metros | 8 bandas (fusión con banda PAN) | Reflectancia BOA (GS / Brovey / ESRI) |

#### Procesamiento y Corrección Radiométrica
Para garantizar la comparación espectral entre sensores, los Niveles Digitales (DN) de WorldView-2 fueron calibrados a radiancia física y luego a reflectancia en el techo de la atmósfera (TOA) utilizando los factores de calibración absoluta (`AbsCalFactor`) y el ancho de banda efectivo (`EffectiveBandwidth`) extraídos de los metadatos XML. Posteriormente, se aplicó la corrección por el método del Objeto Oscuro (DOS1 - *Dark Object Subtraction*) para la remoción de la bruma atmosférica y la aproximación a reflectancia en la superficie (BOA).

#### Corrección Geométrica y Coregistro
Se identificó un desfase sistemático de paralelaje en la adquisición de WorldView-2 respecto al ortomosaico de referencia generado por vehículo aéreo no tripulado (dron). Este desplazamiento, de aproximadamente 6 metros hacia el oeste y 2.6 metros hacia el norte en la esquina de referencia, se corrigió mediante un vector de traslación en el espacio vectorial para lograr la coincidencia exacta de los rodales digitalizados de maíz y sorgo.

#### Algoritmos de Pansharpening Evaluados
Se evaluaron y compararon cuatro algoritmos de fusión de imágenes (pansharpening) para elevar la resolución multiespectral de WorldView-2 de 2.0m a 0.5m: Brovey Modificado, Media Simple, Fusión ESRI y Gram-Schmidt (GS). El método de Gram-Schmidt fue seleccionado para el análisis cuantitativo final al simular la respuesta de la banda pancromática mediante la media de las bandas multiespectrales, preservando en mayor medida las firmas espectrales de la vegetación.

#### Criterio de Pureza Espectral y Selección de Rodales
Se implementó un análisis de buffer interno (`all_touched=False`) para diferenciar píxeles puros centrales de aquellos de borde contaminados por la firma del suelo desnudo o del cultivo adyacente. Los rodales se clasificaron por tamaño (Alto, Medio, Bajo). La reducción en la resolución espacial de 0.5m a 3m evidenció la pérdida crítica de píxeles puros en la clase "Bajo", lo que requirió el filtrado estadístico y la exclusión de estas muestras en los sensores de menor resolución espacial.

#### Análisis Estadístico y Modelos de Clasificación
La separabilidad espectral de las clases (maíz, sorgo, y parches con malas hierbas) se evaluó mediante análisis de varianza (ANOVA) y pruebas de diferencias honestamente significativas de Tukey (Tukey's HSD) para cada banda e índice de vegetación calculado. Como modelo predictivo de clasificación preliminar, se entrenó un algoritmo de Bosques Aleatorios (*Random Forest Classifier*) y un Análisis Discriminante Lineal (LDA), evaluando su exactitud global, precisión, exhaustividad y F1-score.

---

## 3. Hoja de Ruta y Mejoras Críticas para el Futuro

Para consolidar este estudio piloto en una publicación de revista de alto impacto, se proponen las siguientes mejoras metodológicas que se presentarán como "Trabajo Futuro" en el congreso:

* **Corrección Atmosférica por Transferencia Radiativa Física (6S):** Reemplazar el método empírico DOS1 por el modelo radiativo Py6S, parametrizado con datos reales de espesor óptico de aerosoles (AOD) y vapor de agua obtenidos de estaciones de la red AERONET cercanas.
* **Coregistro de Imágenes por Puntos de Control (GCPs):** Realizar una rectificación geométrica de las imágenes satelitales mediante una matriz de deformación local (*Thin Plate Spline*) utilizando al menos 15 *Ground Control Points* identificables tanto en el ortomosaico de dron como en el satélite, eliminando cualquier distorsión por relieve.
* **Desmezclado Espectral (Spectral Unmixing):** En las resoluciones de 2m y 3m (donde el píxel mixto es predominante), aplicar modelos lineales de mezcla espectral para estimar el porcentaje exacto de cobertura de malas hierbas, en lugar de clasificar el píxel de forma binaria.
* **Clasificación Orientada a Objetos (OBIA):** Aprovechar la altísima resolución de 0.5m segmentando la imagen en parches homogéneos (algoritmo SLIC o similar), permitiendo al clasificador usar características de textura (GLCM) y geométricas (compacidad, área), lo que evita el ruido *pixel-level*.
* **Modelos de Aprendizaje Profundo (Deep Learning):** Migrar la clasificación hacia redes neuronales convolucionales (CNN) tipo U-Net utilizando el canal pancromático de alta resolución espacial combinado con las firmas espectrales BOA, permitiendo la segmentación semántica automática de malas hierbas y cultivos.
