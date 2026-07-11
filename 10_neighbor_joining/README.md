# Cuaderno 10 Filogenia Computacional (Neighbor Joining)

Implementación del algoritmo Neighbor Joining para inferir árboles filogenéticos
a partir de secuencias del gen COX1 de 7 especies.

## Estructura del notebook

| Celda | Contenido |
|-------|-----------|
| Marco teórico | p-distance, matriz Q, longitudes de rama, actualización de distancias |
| Importaciones | Reutiliza `NeedlemanWunsch` + `msa_progresivo` (Lab 07) y `fetch_fasta_from_ncbi` (Lab 05) |
| Descarga COX1 | 7 especies de NCBI, fragmento de 600 nt |
| MSA | Alineamiento progresivo del fragmento COX1 |
| p-distance | Matriz de distancias sobre el MSA |
| `NeighborJoining` | Clase completa con `verbose=True` imprime matriz Q en cada iteración |
| Visualización | Árbol filogenético con networkx |
| Análisis | Longitudes de rama + verificación taxonómica |

## Especies y accessions

| Especie | Accession | Grupo |
|---------|-----------|-------|
| *Homo sapiens* | NC_012920.1 | Primate |
| *Pan troglodytes* | NC_001643.1 | Primate |
| *Gorilla gorilla* | NC_001645.1 | Primate |
| *Mus musculus* | NC_005089.1 | Mamífero (roedor) |
| *Canis lupus familiaris* | NC_002008.4 | Mamífero (carnívoro) |
| *Danio rerio* | NC_002333.2 | Vertebrado (pez) |
| *Xenopus tropicalis* | NC_006839.1 | Vertebrado (anfibio) |

## Ejemplo manual (4 especies, verificación)

Distancias iniciales:

|   | A | B | C | D |
|---|---|---|---|---|
| A | 0 | 0.3 | 0.7 | 0.8 |
| B | 0.3 | 0 | 0.6 | 0.7 |
| C | 0.7 | 0.6 | 0 | 0.5 |
| D | 0.8 | 0.7 | 0.5 | 0 |

**Iteración 1:** Matriz Q selecciona par (A,B) con Q=−2.800:
- Rama A: 0.2000 / Rama B: 0.1000 → nuevo nodo U1

**Iteración 2:** Matriz Q selecciona par (C,D) con Q=−1.600:
- Rama C: 0.2000 / Rama D: 0.3000 → nuevo nodo U2

**Último par**: U1 — U2 con d=0.300

Árbol: `((A:0.20, B:0.10)U1, (C:0.20, D:0.30)U2)`

## Algoritmo NJ, fórmulas clave

```
Q(i,j) = (n−2)·d(i,j) − rᵢ − rⱼ       rᵢ = Σₖ d(i,k)

Lᵢ = ½·d(i,j) + (rᵢ−rⱼ)/(2(n−2))      Lⱼ = d(i,j) − Lᵢ

d(u,k) = (d(i,k) + d(j,k) − d(i,j)) / 2
```

## Diferencias NJ vs UPGMA (Lab 07)

| | UPGMA | Neighbor Joining |
|-|-------|-----------------|
| Supuesto | Tasa de evolución constante | Sin supuesto de reloj molecular |
| Ramas | Siempre iguales entre hermanos | Variables por especie |
| Árbol | Ultramétrico | Aditivo |
| Complejidad | O(n²) | O(n³) |
| Realismo | Menor | Mayor |

## Dependencias

```
numpy  matplotlib  networkx  biopython
```

## Ejecución

Las secuencias se descargan de NCBI y se cachean en `data/`. Con conexión
a internet la primera ejecución tarda ~30 segundos por las 7 descargas.
El MSA de 7 secuencias de 600 nt tarda ~30 segundos adicionales.