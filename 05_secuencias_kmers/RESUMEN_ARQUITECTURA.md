#  Resumen de Arquitectura - Cuaderno 05: Secuencias y k-mers

## ¿Qué implementé?

### 1. **Estructura de Datos Principal**

```python
# Diccionario anidado: k -> nombre_secuencia -> frecuencias de k-mers
total_frecuencias_kmers_analizadas = {
    3: {
        "ejemplo_labo": {"ATG": 5, "TGA": 3, ...},
        "NM_000518.5": {"ATG": 12, "TGA": 8, ...},
        ...
    },
    6: {
        "ejemplo_labo": {"ATGCAT": 2, ...},
        ...
    },
    ...
}
```

**Ventaja**: Acceso claro y autodocumentado
```python
# Acceso simple y legible
kmers_seq = total_frecuencias_kmers_analizadas[3]["ejemplo_labo"]
```

---

### 2. **Clase `KmerCounter` con Múltiples Estrategias**

```python
counter = KmerCounter(secuencia=secuencias["ejemplo_labo"])

# Elegir método:
counter.contar_kmers(k=5, metodo="diccionario")      # Base
counter.contar_kmers(k=5, metodo="sliding_window")   # Optimizado
counter.contar_kmers(k=5, metodo="paralelo")         # Paralelizado
```

**Métodos implementados:**

| Método | Complejidad | Uso | Ventajas |
|--------|---|---|---|
| `diccionario` | O(n·k) | Base | Simple, predecible |
| `sliding_window` | O(n·k) | Optimización | Evita recomputación de hash |
| `paralelo` | O(n·k/p) + overhead | Grandes secuencias | Divide el trabajo |

---

### 3. **Análisis Completo Realizado**

El notebook ahora contiene:

✅ **Descarga de secuencias** desde NCBI (4 accessions: 2 genes + 2 genomas bacterianos)  
✅ **Lectura de FASTA** con función personalizada (`leer_fasta`)  
✅ **Clase `Secuencia`** como contenedor de datos (dataclass)  
✅ **Clase `KmerCounter`** con 3 estrategias diferentes  
✅ **Análisis de múltiples k** (k=3, 6, 11, 21)  
✅ **Benchmarking visual** con gráficos (tiempo vs k, ratio de métodos)  
✅ **Ejemplos de acceso** a la estructura de datos  

---

##  Complejidad y Rendimiento

### Complejidad Teórica

| Aspecto | Fórmula | Notas |
|---------|---------|-------|
| k-mers en secuencia | `n - k + 1` | Crecimiento lineal con n |
| k-mers posibles | `4^k` | Espacio potencial (nunca alcanzado en práctico) |
| Conteo diccionario | O(n·k) | Slicing es O(k) en Python |
| Sliding window optimizado | O(n) | Solo en C/Cython |
| Paralelización | O(n·k/p) | Teórico; práctica depende de overhead |

### Que es `4^k` realmente:
- **NO** es la complejidad del algoritmo
- **SÍ** es el espacio potencial de k-mers únicos posibles en ADN
- En práctica: siempre `len(frecuencias_kmers) ≤ n - k + 1 << 4^k`

---

##  Recomendaciones de Uso

### Para análisis pequeños (<1MB):
```python
counter.contar_kmers(k=7, metodo="diccionario")
```

### Para análisis medianos (1-100MB):
```python
counter.contar_kmers(k=11, metodo="sliding_window")
```

### Para análisis grandes (>100MB) en sistemas multi-core:
```python
counter.contar_kmers(k=15, metodo="paralelo")
```

---

##  Estructura del Notebook

### Secciones principales:

1. **Marco teórico** (conceptos de k-mers)
2. **Descarga y lectura** (NCBI + FASTA)
3. **Clases `Secuencia` y `KmerCounter`**
4. **Ejemplos básicos** (cómo usar)
5. **Análisis completo** (todas las secuencias, múltiples k)
6. **Benchmarking** (comparación de métodos con gráficos)
7. **Conclusiones** (resumen de arquitectura)

---

##  Cómo Extender el Proyecto

### Opción 1: Añadir Trie
```python
def _contar_trie(self, k: int) -> dict[str, int]:
    # Implementación con estructura Trie
    pass
```

### Opción 2: Visualizar distribuciones
```python
import seaborn as sns
sns.barplot(data=kmers_ordenados[:20])
```

### Opción 3: Comparar secuencias
```python
kmers_seq1 = total_frecuencias_kmers_analizadas[3]["seq1"]
kmers_seq2 = total_frecuencias_kmers_analizadas[3]["seq2"]
comunes = set(kmers_seq1.keys()) & set(kmers_seq2.keys())
```

---

## Notas Importantes

1. **`__str__` vs `__repr__`**:
   - `__str__()` → usuarios finales (print)
   - `__repr__()` → desarrolladores (debugging)

2. **Property `@longitud`**: Es un atributo, no un método
   ```python
   seq.longitud  # NO: seq.longitud()
   ```

3. **Diccionario vs lista**: Elegimos dict porque:
   - Acceso por nombre (no por índice frágil)
   - O(1) en busquedas
   - Mejor documentación automática

4. **Multiprocessing en Jupyter**: Puede ser más lento que serial en secuencias pequeñas

---

##  Para Ejecutar

```bash
cd D:\TRABAJOS 2025B\GITHUB\bio-notebooks\05_secuencias_kmers
jupyter notebook notebook.ipynb
```

Ejecuta TODAS las celdas en orden. El benchmarking tomará pocos segundos.

---

**Autor**: Estructura didáctica para Bioinformática  
**Herramientas**: Python 3.10+, Biopython, matplotlib, numpy  
**Licencia**: Educativa (bio-notebooks)
