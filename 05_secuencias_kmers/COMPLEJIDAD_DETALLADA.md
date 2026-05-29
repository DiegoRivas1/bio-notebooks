#  Análisis Detallado de Complejidad

## Conceptos Iniciales

### `n` = longitud de la secuencia (pb)
### `k` = tamaño del k-mer (nucleótidos)
### Total de k-mers en secuencia = `n - k + 1`
### Espacio teórico máximo = `4^k` (solo para ADN)

---

## 1. Complejidad de Conteo Ingenuo

### Pseudocódigo
```
for i from 0 to n-k:
    kmer = substring(seq, i, i+k)  // O(k) en Python
    if kmer in dict:
        dict[kmer] += 1
    else:
        dict[kmer] = 1
```

### Análisis
- **Iteraciones del loop**: `n - k + 1` ≈ O(n)
- **Costo por iteración**: 
  - Substring de Python: **O(k)**  (copia k caracteres)
  - Búsqueda en diccionario: **O(1)** promedio (hash table)
  - Incremento: **O(1)**

### **Complejidad Total: O(n · k)**

#### Desglose:
```
Tiempo = (n - k + 1) × (k + 1)
       = (n - k + 1) × k  +  (n - k + 1)
       = n·k - k² + k  +  n - k + 1
       ≈ n·k           (para k << n)
```

### Ejemplo práctico:
- n = 1,000,000 pb (1MB)
- k = 11 pb

Operaciones ≈ 1,000,000 × 11 = **11 millones de operaciones**

---

## 2. Complejidad de Sliding Window (Optimizado)

### Pseudocódigo (mejor en C/Cython)
```
hash_val = compute_hash(seq[0:k])      // O(k)
for i from 1 to n-k:
    remove_first = hash_val >> 8       // O(1) - rolling hash
    add_last = seq[i+k-1] << 8         // O(1)
    hash_val = combine(remove_first, add_last)  // O(1)
    if hash_val in dict:
        dict[hash_val] += 1
```

### En Python puro (nuestra implementación):
Sigue siendo O(n·k) porque Python no optimiza el substring, incluso con rolling hash.

### En C/Cython:
Se convierte en **O(n)** porque el rolling hash es O(1) puro.

### **Complejidad en Python: O(n · k)**
### **Complejidad en Cython/C: O(n)**

---

## 3. Complejidad de Paralelización

### Concepto: divide y conquista
```
1. Dividir secuencia en p chunks
2. Procesar cada chunk en paralelo
3. Merging de resultados
```

### Cálculo teórico
```
Tiempo por chunk = O(n/p · k)
Merging = O(m) donde m = número de k-mers únicos ≤ n

Tiempo total = O(n·k/p) + O(m) + overhead
```

### Overhead de multiprocessing
- Crear procesos: ~50-100ms
- Serializar datos: O(n)
- Transferencia IPC: O(n)

### **Complejidad teórica: O(n·k/p) + O(overhead)**
### **Complejidad práctica: Peor que serial para n < 1MB**

### Recomendaciones:
- **p = 2**: n > 5MB
- **p = 4**: n > 10MB
- **p = 8**: n > 50MB

---

## 4. Complejidad de Trie

### Pseudocódigo
```
for i from 0 to n-k:
    kmer = substring(seq, i, i+k)    // O(k)
    insert_trie(kmer, trie)           // O(k) - recorrer k nodos
```

### Análisis
- Substring: O(k)
- Inserción en Trie: O(k)

### **Complejidad: O(n · k)**

### Pero con overhead:
- **Espacio**: O(4^k) en el peor caso (todos los k-mers posibles)
- **Cache misses**: Trie tiene peor localidad de memoria que diccionario

En práctica: **LENTA para conteo simple** (útil para búsquedas por prefijo)

---

## 5. Tabla Comparativa

| Método | Tiempo | Espacio | Casos de uso |
|--------|--------|---------|---|
| Diccionario | O(n·k) | O(m) | Base, recomendado |
| Sliding Window (Python) | O(n·k) | O(m) | Igual al diccionario |
| Sliding Window (Cython) | O(n) | O(m) | Optimizaciónavanzada |
| Paralelización | O(n·k/p) + overhead | O(m×p) | n > 10MB con p cores |
| Trie | O(n·k) | O(4^k) | Búsquedas de prefijos |

### m = número de k-mers únicos observados
### p = número de procesos/cores

---

## 6. Análisis Empírico del Notebook

### Benchmarking realizado:
- Secuencia: `ejemplo_labo` (pequeña, < 100KB)
- k valores: 3, 5, 7, 9, 11, 15

### Resultados esperados:
```
k=3:  ~100μs   (muy rápido)
k=5:  ~150μs
k=7:  ~200μs   (lineal con k)
k=9:  ~250μs
k=11: ~300μs
k=15: ~400μs
```

### Conclusión:
En Python, diccionario y sliding_window tienen **rendimiento prácticamente idéntico** porque ambos usan substring.

La verdadera optimización viene de:
1. Compilación (Cython/numba)
2. Paralelización (solo para secuencias GRANDES)
3. Representación especial (rolling hash en C)

---

## 7. La Confusión de 4^k

### ❌ INCORRECTO:
"El contador de k-mers tiene complejidad O(4^k)"

### ✅ CORRECTO:
"El espacio teórico máximo de k-mers únicos es **4^k**, pero en práctica nunca alcanzamos eso"

### Comparación:
```
k=3:  4^k = 64,        en práctica: 3-50 k-mers únicos
k=7:  4^k = 16,384,    en práctica: 100-1,000 k-mers únicos
k=11: 4^k = 4.2M,      en práctica: 1K-100K k-mers únicos
k=21: 4^k = 4.4T (!!)   en práctica: 100K-10M k-mers únicos
```

**La complejidad del algoritmo depende de `n`, NO de `4^k`.**

---

## 8. Complejidad Espacial Detallada

### Diccionario/Hash
```
Espacio = m × (tamaño_promedio_kmer + overhead_hash)
       ≈ m × (k + 8 bytes)
       = m × k + 8m
```

Para k=11, m=100K:
- 100K × 11 = 1.1MB (strings)
- 100K × 8 = 0.8MB (valores frecuencia + overhead)
- **Total ≈ 2MB**

### Trie
```
Espacio = nodos × (4 punteros + 1 contador)
       ≈ 4^k × 40 bytes  (worst case)
```

Para k=11:
- **4^11 × 40 bytes ≈ 160MB** (virtual, nunca alcanzado)
- **Práctico ≈ 10-50MB** (estructura dispersa)

**El diccionario es 5-20x más eficiente en memoria que Trie.**

---

## 9. Recomendación de Método por Tamaño

| Tamaño | Método | Razón |
|--------|--------|-------|
| < 1MB | Diccionario | Sin overhead |
| 1-50MB | Diccionario | Simple y rápido |
| 50-500MB | Sliding Window + paralelización | Speedup notable |
| > 500MB | Paralelización con segmentación | Necesario |

---

## 10. Código para Medir Complejidad Empírica

```python
import time
import matplotlib.pyplot as plt

def medir_complejidad():
    secuencia = secuencias["ejemplo_labo"]
    valores_k = [3, 5, 7, 9, 11, 15, 21]
    tiempos = []
    
    for k in valores_k:
        counter = KmerCounter(secuencia)
        inicio = time.perf_counter()  # Más preciso que time.time()
        _ = counter.contar_kmers(k=k)
        tiempo = time.perf_counter() - inicio
        tiempos.append(tiempo)
    
    # Graficar en escala log-log
    plt.loglog(valores_k, tiempos, 'o-', linewidth=2)
    
    # Si fuera O(n·k), la pendiente sería 1 en log-log
    # Si fuera O(n), la pendiente sería 0
    plt.xlabel("k")
    plt.ylabel("Tiempo (s)")
    plt.title("Empirical Complexity")
    plt.grid(True, alpha=0.3)
    plt.show()
    
    # Calcular pendiente
    import numpy as np
    log_k = np.log(valores_k)
    log_t = np.log(tiempos)
    pendiente = np.polyfit(log_k, log_t, 1)[0]
    print(f"Pendiente en log-log: {pendiente:.2f}")
    print(f"Si es ~1.0: O(k), Si es ~0.0: O(1)")

medir_complejidad()
```

---

##  Resumen Final

| Concepto | Realidad |
|----------|----------|
| "Es O(4^k)" | NO. Es O(n·k). 4^k es espacio teórico, no complejidad |
| "Sliding window es más rápido que diccionario" | NO en Python. Sí en C/Cython |
| "Paralelización siempre acelera" | NO. Solo para n > 10MB en práctica |
| "El diccionario es ineficiente" | NO. Es la mejor opción para conteo |
| "Trie es mejor" | NO para conteo. Sí para búsquedas de prefijos |

**Bottom line**: Para tu cuaderno, **usa diccionario** y punto. ✅

---

Escrito con amor por la Bioinformática 
