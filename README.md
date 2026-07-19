# NGS Re-analysis of *Exiguobacterium profundum* TSS-3

**Status:** 🔄 In progress — QC and read trimming complete, hybrid genome assembly running

An independent bioinformatics project re-analyzing publicly available Illumina + Nanopore sequencing data for *Exiguobacterium profundum* TSS-3, using entirely free, cloud-based tools (Galaxy, KBase, antiSMASH). This strain is closely related to *E. profundum* PZ163977, which I isolated and characterized during my MSc dissertation (GenBank: PZ163977).

---

## Motivation

TSS-3 (isolated from a saline-alkaline spring, Chiapas, Mexico) and my own strain PZ163977 (isolated from coastal fish water, Kerala, India) are the same species from very different environments. Re-analyzing TSS-3's published genome lets me:

- Practice and demonstrate a full NGS bioinformatics pipeline (QC → assembly → annotation → BGC mining → comparative genomics) using real published data
- Compare genomic features (biosynthetic gene clusters, stress-adaptation genes) between two geographically distant strains of the same species
- Look for genomic evidence — specifically a carotenoid biosynthesis cluster — that could explain the anti-inflammatory activity (COX-2 inhibition, IC₅₀ = 4.43 µg/mL) I measured experimentally in PZ163977 during my dissertation

## Data source

| | |
|---|---|
| Organism | *Exiguobacterium profundum* TSS-3 |
| Isolation source | Saline-alkaline spring sediment, Ixtapa, Chiapas, Mexico |
| BioProject | [PRJNA887767](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA887767) |
| Reference publication | Rincón-Rosales et al. 2023, *Microbiology Resource Announcements* 12:e00171-23 |
| Illumina run | SRR24916835 (NovaSeq 6000, paired-end, 12,134,956 read pairs) |
| Nanopore run | SRR24916834 (MinION, 64,387 reads) |
| Published assembly (reference) | GCA_029026745.1 |
| Original assembly method | Unicycler hybrid v0.4.8 + NCBI PGAP annotation |

All raw data is public and was retrieved directly from NCBI SRA / ENA. No new wet-lab data was generated in this project — this is a **computational re-analysis**, and is documented as such throughout.

## Pipeline and tools

All analysis run on free-tier platforms: [Galaxy](https://usegalaxy.org) (usegalaxy.org / usegalaxy.eu) and [KBase](https://www.kbase.us).

| Stage | Tool | Status |
|---|---|---|
| Read QC | FastQC v0.12.1 | ✅ Complete |
| Illumina trimming | Trimmomatic v0.39 | ✅ Complete |
| Nanopore filtering | Filtlong v0.3.1 | ✅ Complete |
| Hybrid genome assembly | Unicycler | 🔄 Running |
| Assembly QC | QUAST, CheckM | ⏳ Pending |
| Annotation | Prokka | ⏳ Pending |
| BGC mining | antiSMASH v7 | ⏳ Pending |
| Comparative genomics | ANI (EzBioCloud) | ⏳ Pending |

---

## Results so far

### 1. Quality control (FastQC)

| Dataset | Reads | Bases | Mean quality | GC% | Notes |
|---|---|---|---|---|---|
| Illumina R1 (SRR24916835_1) | 12,134,956 | 1.8 Gbp | Q30–31 | 48% | Clean, no adapters detected |
| Illumina R2 (SRR24916835_2) | 12,134,956 | 1.8 Gbp | Q30–31 | 48% | Clean; minor GC-content WARN (not contamination) |
| Nanopore (SRR24916834) | 64,387 | 583.8 Mbp | Lower/variable (expected for platform) | 47% | Length range 43 bp–141 kb; WARN/FAIL flags typical for Nanopore, not data-quality concerns |

All GC-content values closely match the published TSS-3 genome GC content (48.16%, chromosome), indicating no detectable contamination.

**Figures:** [`figures/fig1_R1_quality.png`](figures/fig1_R1_quality.png) · [`figures/fig2_R1_gc.png`](figures/fig2_R1_gc.png) · [`figures/fig3_R2_quality.png`](figures/fig3_R2_quality.png) · [`figures/fig4_R2_gc.png`](figures/fig4_R2_gc.png) · [`figures/fig5_nanopore_quality.png`](figures/fig5_nanopore_quality.png) · [`figures/fig6_nanopore_gc.png`](figures/fig6_nanopore_gc.png)

### 2. Illumina read trimming (Trimmomatic)

```
Tool: Trimmomatic 0.39 (paired-end mode)
Parameters: ILLUMINACLIP:TruSeq3-PE:2:30:10:8:true LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50, Phred33

Input read pairs:    12,134,956
Both surviving:      12,134,888 (100.00%)
Dropped:                     68 (0.00%)
```

Essentially all reads survived trimming, consistent with the high input quality shown by FastQC.

### 3. Nanopore read filtering (Filtlong)

```
Tool: Filtlong 0.3.1
Parameters: --min_length 500 --min_mean_q 7
```

| Metric | Before | After | Retained |
|---|---|---|---|
| Reads | 64,387 | 59,061 | 91.7% |
| Bases | 583.8 Mbp | 582.1 Mbp | 99.7% |

Filtering removed mostly short, low-information fragments (8.3% of reads by count) while retaining 99.7% of total sequence data.

**Figures:** [`figures/fig7_nanopore_filtered_quality.png`](figures/fig7_nanopore_filtered_quality.png) · [`figures/fig8_nanopore_filtered_gc.png`](figures/fig8_nanopore_filtered_gc.png)

### 4. Genome assembly — in progress

Hybrid assembly (Illumina + Nanopore) via **Unicycler**, matching the method used in the original publication. Results (contig count, N50, assembly length) will be added here once complete, along with QUAST and CheckM quality metrics benchmarked against the published reference assembly (GCA_029026745.1).

---

## Skills demonstrated

- Retrieval of raw sequencing data from NCBI SRA / ENA
- Illumina and Nanopore read QC and interpretation (FastQC)
- Read trimming and long-read filtering (Trimmomatic, Filtlong)
- Cloud-based bioinformatics workflows (Galaxy, KBase) — no local compute required
- Troubleshooting real pipeline failures (quality-encoding detection errors, tool/server compatibility issues) using job logs

## Author

**Afnan Asharaf** — MSc Microbiology, independent researcher
GenBank depositions: PZ163764, PZ163949, PZ163977 · ASM member
GitHub: [@AfnanAsharf](https://github.com/AfnanAsharf)

*This repository will be updated as assembly, annotation, and comparative genomics stages complete.*
