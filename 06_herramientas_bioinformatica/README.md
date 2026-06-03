# Cuaderno 06 Herramientas para Bioinformática

Breve resumen del cuaderno y material incluido en la carpeta `06_herramientas_bioinformatica`.

## Competencias
- Comparar implementaciones propias (Needleman‑Wunsch, Smith‑Waterman, K‑mers) con herramientas estándar (EMBOSS, Jellyfish).
- Interpretar diferencias en rendimiento, escalabilidad y resultados.

## Contenido
- `notebook.ipynb`: cuaderno con implementaciones, ejemplos y comparativas (EMBOSS needle/water, Jellyfish).
- `data/`: archivos FASTA usados para las pruebas (p. ej. `NM_000518.5.fasta`, `NM_009764.3.fasta`).
- `resultados/`: salidas generadas por herramientas externas (dumps, histograms, resultados de EMBOSS).

## Estado
Este cuaderno contiene las implementaciones y las comparativas solicitadas; está listo para ser ejecutado en Jupyter.  
Estado: Completo

## Cómo ejecutar
1. Abrir `notebook.ipynb` con Jupyter Notebook / JupyterLab.
2. Asegurarse de tener las dependencias del repositorio (ver `requirements.txt`).
3. Opcional: para reproducir la comparativa con Jellyfish o EMBOSS use WSL / conda para instalar esas herramientas y ejecute las celdas indicadas en el cuaderno.

## Notas
- Las comparativas del cuaderno incluyen instrucciones para usar las versiones web o CLI de EMBOSS y Jellyfish.
- Los archivos de resultados (TSV/CSV) están en `resultados/` para análisis rápido.

