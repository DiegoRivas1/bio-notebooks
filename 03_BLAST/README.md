# 03 — BLAST y Búsqueda por Similitud

**Tema:** Búsqueda de secuencias similares en bases de datos  
**Herramientas:** Python 3, Biopython, matplotlib, pandas

## Qué aprenderás

- Comprender cómo BLAST supera en velocidad a Smith-Waterman
- Ejecutar búsquedas BLAST remotas desde Python
- Interpretar E-value, score, identidad y cobertura
- Visualizar y comparar hits

## Datos

Coloca en la carpeta `data/`:

| Archivo | Accession | Enlace NCBI |
|---|---|---|
| `U14680.1.fasta` | U14680.1 | https://www.ncbi.nlm.nih.gov/nuccore/U14680.1?report=fasta |

El resultado BLAST se guarda automáticamente en `data/blast_result.xml`
para no repetir la búsqueda.

## Ejecutar

```bash
pip install biopython pandas matplotlib
jupyter notebook notebook.ipynb
```

## Estructura

```
03_blast/
    notebook.ipynb
    README.md
    data/
        U14680.1.fasta
        blast_result.xml    (generado automáticamente)
```

## Referencias

- Altschul et al. (1990), Journal of Molecular Biology
- NCBI BLAST: https://blast.ncbi.nlm.nih.gov/
