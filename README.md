# 🧬 bio-notebooks

Cuadernos de Jupyter para el curso completo de **Bioinformática**.  
Cada carpeta numerada es un tema: teoría explicada paso a paso + código ejecutable + visualizaciones.

## Contenido

| # | Tema | Secuencias | Estado |
|---|------|-----------|--------|
| 01 | [Secuencias FASTA](01_secuencias_fasta/notebook.ipynb) | HBB, Mitocondria, E. coli, Genes humanos | ✅ Completo |
| 02 | [Alineamiento de Secuencias](02_alineamiento_secuencias/notebook.ipynb) | BRCA1 humano vs ratón | ✅ Completo |
| 03 | [BLAST y Búsqueda por Similitud](03_blast/notebook.ipynb) | BRCA1 humano | ✅ Completo |
| 04 | Filogenética y Árboles Evolutivos | — | 🔜 Próximamente |
| 05 | Análisis de Expresión Génica | — | 🔜 |
| 06 | Estructura de Proteínas | — | 🔜 |
| 07 | Genómica Comparativa | — | 🔜 |
| 08 | Ensamblaje de Genomas | — | 🔜 |

## Requisitos

```bash
pip install -r requirements.txt
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
│   ├── notebook.ipynb
│   ├── README.md
│   └── data/
│       ├── hbb_small.fasta
│       ├── mito_medium.fasta
│       ├── ecoli_large.fasta
│       └── genes_humanos_multi.fasta
├── 02_alineamiento_secuencias/
│   ├── notebook.ipynb
│   ├── README.md
│   └── data/
│       ├── U14680.1.fasta
│       └── NM_009764.fasta
├── 03_blast/
│   ├── notebook.ipynb
│   ├── README.md
│   └── data/
│       ├── U14680.1.fasta
│       └── blast_result.xml  (generado automáticamente)
└── ...
```

## Fuentes de datos

- [NCBI Nucleotide](https://www.ncbi.nlm.nih.gov/nucleotide/)
- [Ensembl](https://www.ensembl.org/)
- [UniProt](https://www.uniprot.org/)
- [European Nucleotide Archive](https://www.ebi.ac.uk/ena/)