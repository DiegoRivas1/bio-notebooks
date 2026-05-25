# 03 Alineamiento Global (Needleman-Wunsch)

**Tema:** Alineamiento global de secuencias biológicas  
**Herramientas:** Python 3, numpy, matplotlib

## Qué aprenderás

- Implementar Needleman-Wunsch con traceback optimizado
- Construir la matriz completa y obtener el alineamiento global óptimo
- Comparar NW vs SW sobre las mismas secuencias
- Visualizar con heatmap y dot plot
- Medir tiempos de ejecución

## Optimización del traceback

En lugar de recalcular la dirección comparando celdas vecinas,
guardamos la dirección tomada en una matriz `T` durante el llenado:

```
T[i][j] = 'D'  → diagonal (match/mismatch)
T[i][j] = 'U'  → arriba (gap en seq2)
T[i][j] = 'L'  → izquierda (gap en seq1)
```

El traceback solo lee `T[i][j]` sin comparaciones adicionales.

## Datos

Secuencias del laboratorio incluidas directamente en el notebook.
Opcionalmente coloca en `data/`:

| Archivo | Accession | Enlace NCBI |
|---|---|---|
| `NM_000518.5.fasta` | NM_000518.5 | https://www.ncbi.nlm.nih.gov/nuccore/NM_000518.5?report=fasta |
| `NM_009764.fasta` | NM_009764 | https://www.ncbi.nlm.nih.gov/nuccore/NM_009764?report=fasta |

## Ejecutar

```bash
pip install numpy matplotlib
jupyter notebook notebook.ipynb
```

## Estructura

```
04_alineamiento_global/
    notebook.ipynb
    README.md
    data/
        NM_000518.5.fasta   (opcional)
        NM_009764.fasta     (opcional)
```

## Referencias

- Needleman & Wunsch (1970), Journal of Molecular Biology
- Smith & Waterman (1981), Journal of Molecular Biology
