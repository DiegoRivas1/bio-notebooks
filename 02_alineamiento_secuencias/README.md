# 02 Alineamiento de Secuencias

**Tema:** Alineamiento global y local de secuencias biológicas  
**Herramientas:** Python 3, numpy, matplotlib, biopython

## Qué aprenderás

- Implementar Smith-Waterman (alineamiento local) desde cero
- Implementar Needleman-Wunsch (alineamiento global) desde cero
- Analizar el efecto del parámetro gap en el alineamiento
- Medir tiempo de ejecución con secuencias de distintas longitudes
- Visualizar la matriz de scoring con el camino del traceback resaltado

## Datos

Coloca los archivos en la carpeta `data/`:

| Archivo | Accession | Organismo | Enlace NCBI |
|---|---|---|---|
| `U14680.1.fasta` | U14680.1 | *Homo sapiens* BRCA1 | https://www.ncbi.nlm.nih.gov/nuccore/U14680.1?report=fasta |
| `NM_009764.fasta` | NM_009764 | *Mus musculus* Brca1 | https://www.ncbi.nlm.nih.gov/nuccore/NM_009764?report=fasta |

También puedes descargarlos automáticamente con la celda de Biopython del notebook
(requiere email válido en `Entrez.email`).

## Ejecutar

```bash
pip install numpy matplotlib biopython
jupyter notebook notebook_back.ipynb
```

## Estructura

```
02_alineamiento_secuencias/
    notebook.ipynb
    README.md
    data/
        U14680.1.fasta
        NM_009764.fasta
```

## Referencias

- Smith & Waterman (1981), Journal of Molecular Biology
- Needleman & Wunsch (1970), Journal of Molecular Biology
