# Cuaderno 08 Descubrimiento de Motifs

Implementación del pipeline de descubrimiento de motifs conservados en secuencias
biológicas mediante conteo de k-mers, extracción de regiones candidatas, MSA
progresivo y análisis de conservación.

## Estructura del notebook

| Celda | Contenido |
|-------|-----------|
| Portada | Competencias del curso y del laboratorio |
| Marco teórico | Motifs, k-mers, similitud coseno, divergencias, modelos IID y Markov |
| Importaciones | Reutilización de `Secuencia`, `KmerCounter` (Lab 05) y `MSAProgresivo` + `NeedlemanWunsch` (Lab 07) |
| Ej. teoría | Verificación de cos/V/J/L con S1/S2/S3 y k=1 (Laplace) |
| Ej. 1 | Secuencias del laboratorio |
| Ej. 2 | K-mers candidatos k=7,8,9 |
| Ej. 3 | Localización de ocurrencias |
| Ej. 4 | Extracción de la región conservada |
| Ej. 5 | MSA de las regiones (Lab 07) |
| Ej. 6 | Matriz de frecuencias |
| Ej. 7 | Secuencia consenso con notación `[ACT]` |
| Ej. 8 | Evaluación de conservación (% por posición y global) |
| Ej. 9 | Reporte final del motif |
| Ej. 10 | Repetición con secuencias reales (Anexo 1, NCBI) |
| Comparación | Perfiles k-mer con cos, V, L entre secuencias reales |
| Conclusiones | Síntesis de resultados |

## Pipeline de descubrimiento

```
Secuencias brutas
      ↓
[1] Conteo de k-mers (k=7,8,9) → candidatos por frecuencia en N secuencias
      ↓
[2] Localización de posiciones del k-mer candidato
      ↓
[3] Extracción de la región conservada (con fallback de 1 mismatch)
      ↓
[4] MSA Progresivo de las regiones (UPGMA + Needleman-Wunsch, Lab 07)
      ↓
[5] Matriz de frecuencias por posición
      ↓
[6] Secuencia consenso + notación degenerada [ACT]
      ↓
[7] % de conservación por posición y global
      ↓
[8] Reporte final del motif
```

## Resultados Ejercicio 1

### Secuencias del laboratorio

| ID | Secuencia | Longitud |
|----|-----------|----------|
| S1 | `ATCGTACGATGACCTGATCG` | 20 nt |
| S2 | `GGTATACGATGACGTTACCA` | 20 nt |
| S3 | `TTTCTACGATGACCATAGGT` | 20 nt |
| S4 | `AACGTACGATGACGGGTTAA` | 20 nt |
| S5 | `CGGATACGATGACTTCCGTA` | 20 nt |
| S6 | `TACCTACGATGACAGGTACA` | 20 nt |
| S7 | `GACTTACGATGACCGATAGC` | 20 nt |
| S8 | `TCGATACGATGACTGGCAAT` | 20 nt |
| S9 | `AGGCTACGATGACATTCGGA` | 20 nt |
| S10 | `CCTATACGATGACGGAATTC` | 20 nt |

### K-mers candidatos

| k | Candidato principal | #secuencias | Posición |
|---|---------------------|-------------|----------|
| 7 | `TACGATG` | 10/10 | 4 (todas) |
| 8 | `TACGATGA` | 10/10 | 4 (todas) |
| 9 | `TACGATGAC` | 10/10 | 4 (todas) |

El k-mer `TACGATGAC` (k=9) aparece en **todas las secuencias en la misma posición (4)**.

### Matriz de frecuencias

| Pos | A | C | G | T | Consenso | Conservación |
|-----|---|---|---|---|----------|-------------|
| 1 | 0 | 0 | 0 | 10 | T | 100% |
| 2 | 10 | 0 | 0 | 0 | A | 100% |
| 3 | 0 | 10 | 0 | 0 | C | 100% |
| 4 | 0 | 0 | 10 | 0 | G | 100% |
| 5 | 10 | 0 | 0 | 0 | A | 100% |
| 6 | 0 | 0 | 0 | 10 | T | 100% |
| 7 | 0 | 0 | 10 | 0 | G | 100% |
| 8 | 10 | 0 | 0 | 0 | A | 100% |
| 9 | 0 | 10 | 0 | 0 | C | 100% |

### Reporte del motif

| Propiedad | Valor |
|-----------|-------|
| Secuencia consenso | `TACGATGAC` |
| Longitud | 9 nt |
| Posición en cada secuencia | 4 (0-based) en las 10 secuencias |
| Secuencias con motif | 10 / 10 |
| Conservación media | 100.0% |
| Posiciones 100% conservadas | 9 / 9 |
| Score SP | 0 (perfectamente conservado) |

El motif es **perfectamente conservado** en las 10 secuencias — sin ninguna
mutación puntual. El Score SP = 0 confirma conservación total bajo el modelo unitario.

## Verificación del ejemplo de la teoría

Con S1/S2/S3, k=1 y suavizado de Laplace:

| Par | cos | J | L | V |
|-----|-----|---|---|---|
| (S1, S2) | 0.919 | 0.167 | 0.021 | 0.193 |
| (S1, S3) | **0.973** | 0.053 | 0.007 | 0.091 |
| (S2, S3) | 0.924 | 0.157 | 0.019 | 0.186 |

Todas las métricas confirman que **S1 y S3 son los vecinos más próximos**,
coincidiendo exactamente con los valores de la teoría.

## Nota sobre Laplace

Las frecuencias crudas se muestran **sin** el +1; el suavizado de Laplace
se aplica solo al calcular las probabilidades para evitar divisiones por cero:

```
f(A) en S1 = 4   →   p(A) = (4+1)/22 = 0.2273
```

## Dependencias

```
numpy  pandas  matplotlib  biopython
```

## Ejecución

Abrir `notebook_lab08.ipynb` en Jupyter y ejecutar todas las celdas en orden.
Las secuencias del Anexo 1 se descargan de NCBI y se cachean en `data/`.
