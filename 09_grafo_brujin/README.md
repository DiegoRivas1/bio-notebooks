# Cuaderno 09 Ensamblaje de Secuencias (Grafo de De Bruijn)

Implementación del grafo de De Bruijn y el algoritmo de Hierholzer para
reconstruir secuencias de ADN a partir de fragmentos cortos (reads).

## Estructura del notebook

| Celda | Contenido |
|-------|-----------|
| Marco teórico | K-mers, prefijo/sufijo, camino Euleriano, condición de existencia, Hierholzer |
| `DeBruijnGraph` | Lista de adyacencia, grados entrada/salida, verificación Euleriana |
| `camino_euleriano` | Algoritmo de Hierholzer $O(E)$ |
| `reconstruir_secuencia` | Primer nodo + último carácter de cada nodo visitado |
| Actividad del lab | `ATGCGATGAC` k=4 reproduce exactamente el grafo del enunciado |
| Experimentación k=3..6 | Tabla comparativa + respuestas a las 4 preguntas + gráfica |
| Ejercicio 1 | 9 fragmentos con k=5,7,9 grafos visualizados + análisis |

## Actividad: Ejemplo del laboratorio

Secuencia: `ATGCGATGAC`, k=4.

### K-mers generados

| K-mer | Prefijo | Sufijo |
|-------|---------|--------|
| `ATGC` | `ATG` | `TGC` |
| `TGCG` | `TGC` | `GCG` |
| `GCGA` | `GCG` | `CGA` |
| `CGAT` | `CGA` | `GAT` |
| `GATG` | `GAT` | `ATG` |
| `ATGA` | `ATG` | `TGA` |
| `TGAC` | `TGA` | `GAC` |

### Grafo (lista de adyacencia)

```
ATG → TGC, TGA
TGC → GCG
GCG → CGA
CGA → GAT
GAT → ATG
TGA → GAC
```

### Camino Euleriano y reconstrucción

```
ATG → TGC → GCG → CGA → GAT → ATG → TGA → GAC
Secuencia reconstruida: ATGCGATGAC  ✓
```

## Experimentación con k

| k | K-mers | Nodos | Aristas | Euleriano | Reconstruida |
|---|--------|-------|---------|-----------|--------------|
| 3 | 8 | 6 | 8 | Camino (bifurcación en `TG`) | `ATGCGATGAC` ✓ |
| 4 | 7 | 7 | 7 | Camino | `ATGCGATGAC` ✓ |
| 5 | 6 | 7 | 6 | Camino | `ATGCGATGAC` ✓ |
| 6 | 5 | 6 | 5 | Camino | `ATGCGATGAC` ✓ |

### Respuestas a las preguntas del laboratorio

**¿Cómo cambia el número de nodos?**  
A mayor k, los nodos (k−1-mers) son más largos y específicos. Con k pequeño
varios k-mers comparten prefijos/sufijos, generando menos nodos pero más conexiones.

**¿Cómo cambia el número de aristas?**  
El número de aristas = número de k-mers en la secuencia = N−k+1. A mayor k,
hay menos k-mers posibles en una secuencia corta → menos aristas.

**¿Qué sucede cuando k es muy pequeño?**  
Con k=3 el nodo `TG` tiene dos sucesores (`GC` y `GA`) bifurcación. El grafo
es más denso y hay ambigüedades. En secuencias reales esto genera ensamblajes
erróneos o múltiples contigs.

**¿Qué sucede cuando k es muy grande?**  
Con k=6 el grafo es casi lineal. Pero si los reads son cortos, muchos k-mers
no se generan → grafo desconectado. Requiere mayor cobertura.

## Ejercicio 1: 9 fragmentos

Secuencia de referencia esperada (63 nt):
```
ATGGCAGATTAGTGCAATGGCTTCAATTTTAGGTTCTATGCTTGGAGGCTTCGGAAACTGACT
```

| k | Nodos | Aristas | Long. reconstruida | Contiene referencia |
|---|-------|---------|--------------------|---------------------|
| 5 | variable | variable | >63 nt | No (camino ambiguo) |
| 7 | variable | variable | >63 nt | No (menos ambiguo) |
| 9 | variable | variable | >63 nt | No (más específico) |

Ningún k produce la secuencia exacta porque los fragmentos tienen solapamientos
redundantes que generan múltiples caminos Eulerianos válidos matemáticamente.
En ensamblaje real se combina De Bruijn con heurísticas adicionales
(corrección de errores, scaffolding).

## Dependencias

```
numpy  matplotlib  networkx
```