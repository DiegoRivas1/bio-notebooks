# 05 Análisis de Secuencias K-mers

**Tema:** Conteo y análisis de k-mers en secuencias biológicas  
**Herramientas:** Python 3, Biopython, matplotlib, numpy, wordcloud

## Qué aprenderás

- Implementar un contador de k-mers con estructuras de datos eficientes
- Extraer k-mers para distintos valores de k (6, 11, 21)
- Calcular frecuencias y estadísticas descriptivas
- Comparar genomas completos mediante similitud de Jaccard
- Visualizar frecuencias con barras, histogramas y word cloud
- Implementar conteo paralelo con `ProcessPoolExecutor`

## Datos

Coloca en la carpeta `data/`:

| Archivo | Accession | Organismo | Enlace NCBI |
|---|---|---|---|
| `NC_000913.2.fasta` | NC_000913.2 | *E. coli* K-12 MG1655 | https://www.ncbi.nlm.nih.gov/nuccore/NC_000913.2?report=fasta |
| `NC_003197.2.fasta` | NC_003197.2 | *Salmonella* LT2 | https://www.ncbi.nlm.nih.gov/nuccore/NC_003197.2?report=fasta |
| `NM_000518.5.fasta` | NM_000518.5 | HBB humana | https://www.ncbi.nlm.nih.gov/nuccore/NM_000518.5?report=fasta |
| `NM_009764.fasta` | NM_009764 | Brca1 ratón | https://www.ncbi.nlm.nih.gov/nuccore/NM_009764?report=fasta |

Si no los encuentra igual el notebook tiene las funciones apra descargar los archivos que unos elija.

> **Advertencia:** E. coli (~4.6 MB) y Salmonella (~4.8 MB) son archivos grandes.
> El análisis con k=11 puede tardar varios minutos.

## Clases implementadas

| Clase | Responsabilidad |
|---|---|
| `Secuencia` | Modelo de datos: id, descripción, secuencia |
| `KmerCounter` | Conteo por diccionario, sliding window y paralelo |

## Métodos de conteo

| Método | Complejidad | Cuándo usarlo |
|---|---|---|
| `diccionario` | O(n·k) | Secuencias < 10 Mbp, uso general |
| `sliding_window` | O(n·k) | Similar, legibilidad explícita |
| `paralelo` | O(n/p·k) | Genomas grandes, múltiples núcleos |

> El método paralelo usa `ProcessPoolExecutor` — compatible con Jupyter/Windows.
> `multiprocessing.Pool` falla en Jupyter porque no hay `__main__`.

## Ejecutar

```bash
pip install biopython matplotlib numpy wordcloud
jupyter notebook notebook_back.ipynb
```

## Estructura

```
05_secuencias_kmers/
    notebook.ipynb
    README.md
    data/
        NC_000913.2.fasta
        NC_003197.2.fasta
        NM_000518.5.fasta
        NM_009764.fasta
```

## Referencias

- Compeau, P., Pevzner, P. & Tesler, G. (2011). *How to apply de Bruijn graphs to genome assembly.* Nature Biotechnology.
- Marçais, G. & Kingsford, C. (2011). *A fast, lock-free approach for efficient parallel counting of k-mers.* Bioinformatics.
- NCBI E. coli K-12: https://www.ncbi.nlm.nih.gov/nuccore/NC_000913.2
- NCBI Salmonella LT2: https://www.ncbi.nlm.nih.gov/nuccore/NC_003197.2
