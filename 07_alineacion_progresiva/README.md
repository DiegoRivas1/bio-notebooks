# Cuaderno 07 Alineación Múltiple Progresiva (MSA)

Implementación del algoritmo de alineación múltiple progresiva con árbol guía UPGMA y Needleman-Wunsch en Python.

## Estructura del notebook

| Celda | Contenido |
|-------|-----------|
| `ScoreParams` | Modelo de costo unitario: `s(a,a)=0`, `s(a,b)=1`, `gap=1` |
| `NeedlemanWunsch` | Alineamiento global; `distance()` retorna `-score` (distancia de edición entera) |
| `construir_arbol_upgma` | UPGMA con promedio ponderado; `verbose=True` imprime cada matriz intermedia |
| `MSAProgresivo` | Pipeline completo: distancias → árbol → alineamiento progresivo con símbolo X |
| Demo ejemplo de clase | Secuencias S1–S5 con verbose completo, comparación con la teoría |
| Secuencias reales | 250 nt de 5 enterobacterias descargadas de NCBI |
| Visualización | Heatmap de bases + gráfica de conservación por posición |
| Score SP | Sum of Pairs total y por columna |

## Modelo de puntuación

El laboratorio exige el **modelo de costo unitario**:

```
s(a, a) = 0     (match    → sin penalización)
s(a, b) = 1     (mismatch → 1 de penalización)
gap     = 1     (gap      → 1 de penalización)
s(X, *) = 0     (símbolo neutro → sin penalización)
```

Para usar Needleman-Wunsch (que maximiza) se niegan los valores:
`match=0, mismatch=-1, gap=-1`. La distancia se obtiene como `-score`.

## Verificación con el ejemplo de la clase

Secuencias de prueba:

```
S1: ATTTACGCCT
S2: TTAAGCCAT
S3: TTAATTAACC
S4: ATTTTCCGGA
S5: AATTTACCGCCT
```

### Matriz inicial (coincide exactamente con la teoría)

|    | S1 | S2 | S3 | S4 | S5 |
|----|----|----|----|----|-----|
| S1 |  0 |  4 |  6 |  5 |  2 |
| S2 |  4 |  0 |  5 |  7 |  6 |
| S3 |  6 |  5 |  0 |  8 |  7 |
| S4 |  5 |  7 |  8 |  0 |  5 |
| S5 |  2 |  6 |  7 |  5 |  0 |

### Árbol guía UPGMA

El proceso tiene un **empate triple** en el Paso 2 (distancia 5.00 entre S2–S3, S6–S2 y S6–S4). La resolución del empate determina el camino tomado:

| Camino | Paso 1 | Paso 2 | Paso 3 | Paso 4 |
|--------|--------|--------|--------|--------|
| **Teoría (PDF)** | S1+S5→S6 | S6+S4→S7 | S2+S3→S8 | S7+S8 |
| **Nuestro código** | S1+S5→G1 | S2+S3→G2 | S4+G1→G3 | G2+G3 |

Ambos caminos son **igualmente válidos** por UPGMA — el empate en distancia 5.00 no tiene resolución única. Lo relevante es que la **matriz de distancias final** y la **estructura del árbol** (grupos formados) son equivalentes.

### Alineamientos intermedios verificados

**Paso 1 — S1 + S5** (coincide con la teoría):
```
S1:  XATTTAXCGCCT
S5:  AATTTACCGCCT
```

**Paso 2 (nuestro código) — S2 + S3** (también mostrado en la teoría):
```
S2:  TTAAGCCAXT
S3:  TTAATTAACC
```

### Resultado final

El código produce un MSA válido con todas las secuencias de la misma longitud, con las mismas regiones conservadas que la solución de referencia. El MSA progresivo no garantiza unicidad — distintos desempates producen alineamientos diferentes pero igualmente óptimos en puntuación SP.

## Símbolo neutro X

Durante el proceso progresivo los gaps se marcan con `X` para que no acumulen penalización en pasos posteriores. Al final se reemplazan por `-`:

```
Alineamiento intermedio:   XATTTAXCGCCT
Resultado final:           -ATTTA-CGCCT
```

La función `propagar_gaps` inserta `X` en las posiciones donde el alineamiento de referencia tiene `-`, avanzando sobre la secuencia original (que ya puede contener `X` de pasos anteriores).

## Secuencias reales

| Organismo | Accession | Región usada |
|-----------|-----------|--------------|
| *Escherichia coli* K-12 | NC_000913.2 | primeros 250 nt |
| *Salmonella enterica* LT2 | NC_003197.2 | primeros 250 nt |
| *Shigella flexneri* | NC_004337.2 | primeros 250 nt |
| *Klebsiella pneumoniae* | NC_009648.1 | primeros 250 nt |
| *Enterobacter cloacae* | NZ_CP013070.1 | primeros 250 nt |

## Dependencias

```
numpy
matplotlib
biopython
```

## Ejecución

Abrir `notebook.ipynb` en Jupyter y ejecutar todas las celdas en orden. La celda de descarga de NCBI requiere conexión a internet; las secuencias se cachean en `data/`.
