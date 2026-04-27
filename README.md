# 🧬 bio-notebooks

Cuadernos de Jupyter para el curso completo de **Bioinformática**.  
Cada carpeta numerada es un tema: teoría explicada paso a paso + código ejecutable + visualizaciones.

## Contenido

| # | Tema | Secuencias | Estado |
|---|------|-----------|--------|
| 01 | [Secuencias FASTA](01_secuencias_fasta/notebook.ipynb) | HBB, Mitocondria, E. coli | ✅ Completo |
| 02 | Alineamiento de secuencias (Needleman-Wunsch, Smith-Waterman) | — | 🔜 Próximamente |
| 03 | BLAST y búsqueda por similitud | — | 🔜 |
| 04 | Filogenética y árboles evolutivos | — | 🔜 |
| 05 | Análisis de expresión génica | — | 🔜 |

## Requisitos

```bash
pip install jupyter matplotlib biopython pandas numpy
```

## Cómo usar

```bash
git clone https://github.com/tu-usuario/bio-notebooks.git
cd bio-notebooks
jupyter notebook
```

Abre el notebook de la carpeta que quieras y ejecuta las celdas en orden (`Shift + Enter`).

## Estructura

```
bio-notebooks/
├── README.md
├── requirements.txt
├── 01_secuencias_fasta/
│   ├── notebook.ipynb      ← teoría + código + visualizaciones
│   ├── data/               ← archivos FASTA de NCBI
│   └── README.md
├── 02_alineamiento/
│   └── ...
└── ...
```

## Fuentes de datos

- [NCBI Nucleotide](https://www.ncbi.nlm.nih.gov/nucleotide/)
- [Ensembl](https://www.ensembl.org/)
- [UniProt](https://www.uniprot.org/)