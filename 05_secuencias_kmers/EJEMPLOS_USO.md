# 🧪 Ejemplos Prácticos de Uso

## 1. Acceder a seq uencias del diccionario

```python
# Cargar una secuencia específica
seq = secuencias["ejemplo_labo"]
print(seq)  # Usa __str__()
print(repr(seq))  # Usa __repr__()
print(seq.longitud)  # Property (sin paréntesis)
```

## 2. Contar k-mers con método específico

```python
counter = KmerCounter(secuencia=secuencias["NM_000518.5"])

# Método 1: Diccionario (recomendado para la mayoría)
kmers_dict = counter.contar_kmers(k=7, metodo="diccionario")

# Método 2: Sliding window
kmers_sw = counter.contar_kmers(k=7, metodo="sliding_window")

# Método 3: Paralelizado (para secuencias grandes)
kmers_par = counter.contar_kmers(k=7, metodo="paralelo")

# Verificar que todos dan el mismo resultado
assert len(kmers_dict) == len(kmers_sw) == len(kmers_par)
```

## 3. Obtener k-mers más frecuentes

```python
kmers_freq = total_frecuencias_kmers_analizadas[7]["NC_000913.2"]

# Top 10
top_10 = sorted(kmers_freq.items(), key=lambda x: x[1], reverse=True)[:10]
for kmer, freq in top_10:
    print(f"{kmer}: {freq}")
```

## 4. Comparar k-mers entre dos secuencias

```python
kmers_bacteria1 = total_frecuencias_kmers_analizadas[9]["NC_000913.2"]  # E. coli
kmers_bacteria2 = total_frecuencias_kmers_analizadas[9]["NC_003197.2"]  # Salmonella

# K-mers comunes
comunes = set(kmers_bacteria1.keys()) & set(kmers_bacteria2.keys())
print(f"K-mers comunes: {len(comunes)} de {len(kmers_bacteria1)}")

# K-mers únicos de bacteria 1
unicos_b1 = set(kmers_bacteria1.keys()) - set(kmers_bacteria2.keys())
print(f"K-mers únicos de E. coli: {len(unicos_b1)}")

# K-mers únicos de bacteria 2
unicos_b2 = set(kmers_bacteria2.keys()) - set(kmers_bacteria1.keys())
print(f"K-mers únicos de Salmonella: {len(unicos_b2)}")
```

## 5. Análisis de frecuencias: obtener estadísticas

```python
kmers = total_frecuencias_kmers_analizadas[5]["ejemplo_labo"]

# Estadísticas básicas
import numpy as np

frecuencias = list(kmers.values())
print(f"Promedio de frecuencia: {np.mean(frecuencias):.2f}")
print(f"Mediana: {np.median(frecuencias):.2f}")
print(f"Desviación estándar: {np.std(frecuencias):.2f}")

# ¿Cuántos k-mers aparecen solo una vez?
unicos = sum(1 for f in frecuencias if f == 1)
print(f"K-mers que aparecen una sola vez: {unicos}")

# Distribución de frecuencias
print(f"\nDistribución:")
print(f"  Aparecen 1 vez: {sum(1 for f in frecuencias if f == 1)}")
print(f"  Aparecen 2-5 veces: {sum(1 for f in frecuencias if 2 <= f <= 5)}")
print(f"  Aparecen 6-10 veces: {sum(1 for f in frecuencias if 6 <= f <= 10)}")
print(f"  Aparecen >10 veces: {sum(1 for f in frecuencias if f > 10)}")
```

## 6. Iterar sobre todas las secuencias y sus análisis

```python
K = [3, 6, 11, 21]

for k in K:
    print(f"\n=== k = {k} ===")
    for nombre_seq, kmers in total_frecuencias_kmers_analizadas[k].items():
        seq_obj = secuencias[nombre_seq]
        print(f"{nombre_seq} ({seq_obj.longitud} pb):")
        print(f"  k-mers únicos: {len(kmers)}")
        print(f"  Cobertura: {len(kmers) / (4**k) * 100:.2f}%")
```

## 7. Crear estructura para comparación entre k

```python
# Comparar cómo cambia el número de k-mers únicos con k
secuencia_nombre = "ejemplo_labo"
datos_comparacion = []

for k in [3, 5, 7, 9, 11, 15, 21]:
    num_kmers = len(total_frecuencias_kmers_analizadas[k][secuencia_nombre])
    max_posibles = 4**k
    datos_comparacion.append({
        'k': k,
        'kmers_unicos': num_kmers,
        'kmers_posibles': max_posibles,
        'cobertura_%': (num_kmers / max_posibles) * 100
    })

for data in datos_comparacion:
    print(f"k={data['k']:2d}: {data['kmers_unicos']:>8} único, {data['cobertura_%']:>6.2f}% cobertura")
```

## 8. Buscar un k-mer específico

```python
# ¿En qué secuencias aparece el k-mer "ATGCAT"?
kmer_buscar = "ATGCAT"
k = len(kmer_buscar)

if k in total_frecuencias_kmers_analizadas:
    print(f"Buscando '{kmer_buscar}' (k={k}):\n")
    for nombre_seq, kmers_dict in total_frecuencias_kmers_analizadas[k].items():
        if kmer_buscar in kmers_dict:
            frecuencia = kmers_dict[kmer_buscar]
            print(f"  {nombre_seq}: {frecuencia} veces")
        else:
            print(f"  {nombre_seq}: NO ENCONTRADO")
else:
    print(f"No hay análisis para k={k}")
```

## 9. Exportar resultados a CSV

```python
import pandas as pd

# Convertir a DataFrame para análisis más fácil
k_valor = 7
secuencia_nombre = "NC_000913.2"
kmers_dict = total_frecuencias_kmers_analizadas[k_valor][secuencia_nombre]

df = pd.DataFrame(list(kmers_dict.items()), columns=['kmer', 'frecuencia'])
df = df.sort_values('frecuencia', ascending=False)

# Guardar
df.to_csv(f"kmers_k{k_valor}_{secuencia_nombre}.csv", index=False)
print(df.head(10))
```

## 10. Visualizar distribución de frecuencias

```python
import matplotlib.pyplot as plt

kmers = total_frecuencias_kmers_analizadas[7]["ejemplo_labo"]
frecuencias = list(kmers.values())

plt.figure(figsize=(10, 6))
plt.hist(frecuencias, bins=30, edgecolor='black')
plt.xlabel("Frecuencia de k-mers")
plt.ylabel("Número de k-mers")
plt.title(f"Distribución de frecuencias (k=7)")
plt.xscale('log')  # Escala logarítmica para ver mejor
plt.yscale('log')
plt.grid(True, alpha=0.3)
plt.show()
```

---

## 📌 Cheat Sheet Rápido

```python
# Acceso rápido
seq = secuencias["nombre"]               # Obtener secuencia
kmers = counter.contar_kmers(k=7)        # Contar k-mers
freq = total_frecuencias_kmers_analizadas[k]["nombre"]  # Obtener diccionario
top = sorted(kmers.items(), key=lambda x: x[1], reverse=True)[:10]  # Top 10

# Properties
seq.longitud                             # Longitud (sin paréntesis)
seq.id, seq.descripcion, seq.secuencia   # Atributos

# Iteración
for nombre, seq_obj in secuencias.items():  # Sobre secuencias
for k in K:                             # Sobre valores de k
for kmer, freq in kmers.items():        # Sobre k-mers y frecuencias
```

---

¡Diviértete analizando tus secuencias! 🧬

