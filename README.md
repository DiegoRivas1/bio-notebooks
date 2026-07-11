# 🧬 bio-notebooks

Cuadernos de Jupyter para el curso completo de **Bioinformática**.  
Cada carpeta numerada es un tema: teoría explicada paso a paso + código ejecutable + visualizaciones.

## Contenido

| #  | Tema                                                                               | Secuencias | Estado |
|----|------------------------------------------------------------------------------------|-----------|--------|
| 01 | [Secuencias FASTA](01_secuencias_fasta/notebook.ipynb)                             | HBB, Mitocondria, E. coli, Genes humanos | ✅ Completo |
| 02 | [Alineamiento de Secuencias](02_alineamiento_secuencias/notebook.ipynb)            | BRCA1 humano vs ratón | ✅ Completo |
| 03 | [BLAST y Búsqueda por Similitud](03_BLAST/notebook.ipynb)                          | BRCA1 humano | ✅ Completo |
| 04 | [Alineamiento Global](04_alineamiento_global/notebook.ipynb)                       | NM_000518.5, NM_009764 | ✅ Completo |
| 05 | [Secuencias k-mers](05_secuencias_kmers/notebook.ipynb)                            | NM_000518.5, NM_009764.3, NC_000913.2, NC_003197.2 | ✅ Completo |
| 06 | [Herramientas bioinformática](06_herramientas_bioinformatica/notebook.ipynb)       | NM_000518.5, NM_009764.3, NC_000913.2, NC_003197.2 | ✅ Completo |
| 07 | [Alineación Múltiple de Secuencias (MSA)](07_alineacion_progresiva/notebook.ipynb) | E. coli, S. enterica, Sh. flexneri, K. pneumoniae, En. cloacae (250 nt) | ✅ Completo |
| 08 | [Descubrimiento de Motifs](08_descubrimiento_motifs/notebook.ipynb)                | Secuencias sintéticas (Ej. 1) + Anexo 1 | ✅ Completo |
| 09 | [Ensamblaje (Grafo de De Bruijn)](09_grafo_brujin/notebook.ipynb)                  | ATGCGATGAC; fragmentos (ejercicio de laboratorio) | ✅ Completo |
| 10 | [Filogenia (Neighbor Joining)](10_neighbor_joining/notebook.ipynb)                | COX1 (7 especies) | ✅ Completo |
| 11 | Estructura de Proteínas                                                            | — | 🔜 Próximamente |
| 12 | Genómica Comparativa                                                               | — | 🔜 |
| 13 | Ensamblaje de Genomas                                                              | — | 🔜 |
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