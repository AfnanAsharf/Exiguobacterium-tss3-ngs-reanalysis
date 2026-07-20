# NGS Re-analysis of *Exiguobacterium profundum* TSS-3

**Status:** ✅ Complete — all planned analysis stages finished (QC → assembly → annotation → BGC mining → comparative genomics)

An independent bioinformatics project re-analyzing publicly available Illumina + Nanopore sequencing data for *Exiguobacterium profundum* TSS-3, using entirely free, cloud-based tools (Galaxy, KBase, antiSMASH, EzBioCloud). This strain is closely related to *E. profundum* PZ163977, which I isolated and characterized during my MSc dissertation (GenBank: PZ163977).

A full write-up (Introduction, Methods, Results, Discussion) is available as a manuscript-style document — see `manuscript.docx` in this repo.

---

## Motivation

TSS-3 (isolated from a saline-alkaline spring, Chiapas, Mexico) and my own strain PZ163977 (isolated from coastal fish water, Kerala, India) are the same species from very different environments. Re-analyzing TSS-3's published genome let me:

- Independently reproduce a full NGS bioinformatics pipeline (QC → assembly → annotation → BGC mining → comparative genomics) using real published data, entirely on free infrastructure
- Look for genomic evidence of carotenoid biosynthesis that could provide context for the anti-inflammatory activity (COX-2 inhibition) I measured experimentally in PZ163977 during my dissertation

## Data source

| | |
|---|---|
| Organism | *Exiguobacterium profundum* TSS-3 |
| Isolation source | Saline-alkaline spring sediment, Ixtapa, Chiapas, Mexico |
| BioProject | [PRJNA887767](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA887767) |
| Reference publication | Rincón-Rosales et al. 2023, *Microbiology Resource Announcements* 12:e00171-23 |
| Illumina run | SRR24916835 (NovaSeq 6000, paired-end, 12,134,956 read pairs) |
| Nanopore run | SRR24916834 (MinION, 64,387 reads) |
| Reference assembly | GCA_029026745.1 |
| Type strain (for ANI) | GCF_025234635.1 |

All raw data is public; this is a **computational re-analysis**, not new wet-lab data generation.

## Pipeline and tools

| Stage | Tool | Status |
|---|---|---|
| Read QC | FastQC v0.12.1 | ✅ |
| Illumina trimming | Trimmomatic v0.39 | ✅ |
| Nanopore filtering | Filtlong v0.3.1 | ✅ |
| Hybrid genome assembly | Unicycler v0.5.1 | ✅ |
| Assembly QC | QUAST v5.3.0, CheckM v1.0.18 | ✅ |
| Annotation | Prokka v1.14.6 | ✅ |
| BGC mining | antiSMASH v8.0.4 | ✅ |
| Comparative genomics | ANI (EzBioCloud, OrthoANIu) | ✅ |

All run on free-tier platforms: [Galaxy](https://usegalaxy.org), [KBase](https://www.kbase.us), [antiSMASH](https://antismash.secondarymetabolites.org), [EzBioCloud](https://www.ezbiocloud.net).

---

## Results

### Genome assembly

Hybrid assembly (Unicycler) produced a **fully circularized chromosome (2,880,082 bp) + plasmid (4,645 bp)** — closely matching the originally published assembly (2.8 Mb chromosome + 4.6 kb plasmid, GC 48.16%).

| Metric | Published | This assembly |
|---|---|---|
| Chromosome | ~2.8 Mb | 2,880,082 bp (circular) |
| Plasmid | ~4.6 kb | 4,645 bp |
| GC content | 48.16% | 48.15% |

**QUAST** (vs. reference GCA_029026745.1): **100% genome fraction, 0 misassemblies**, 0.45 mismatches/100kbp, duplication ratio 1.

**CheckM**: **99.34% completeness, 0.66% contamination** (271/273 single-copy markers present, lineage c__Bacilli).

![QUAST cumulative length](figures/fig9_quast_cumulative_length.png)
![QUAST GC content](figures/fig10_quast_gc_content.png)

### Annotation (Prokka)

**2,936 CDS**, 27 rRNA, 67 tRNA, 1 tmRNA — closely matching the original PGAP annotation (2,900 CDS, ~1.2% difference, consistent with expected variation between annotation pipelines).

### Biosynthetic gene cluster mining (antiSMASH)

Two terpene-associated regions identified on the chromosome:

| Region | Type | Core genes |
|---|---|---|
| 1.1 | Terpene-precursor | Farnesyl diphosphate synthase, dxs |
| 1.2 | Terpene | **crtB, crtN, crtNb, crtNc** |

The Region 1.2 gene set corresponds to the **C30 (diapocarotenoid) branch** of bacterial carotenoid biosynthesis — structurally analogous to the staphyloxanthin pathway in *S. aureus*. This confirms native carotenoid biosynthetic capacity in TSS-3, providing genomic context for the carotenoid pigments identified in my own strain PZ163977. **Important caveat, stated honestly:** this is the C30 pathway, not the C40 (crtI/crtY/crtZ) pathway that typically produces the specific carotenoids (astaxanthin, lycoxanthin) I measured in PZ163977 — so this is complementary evidence of species-level carotenoid capacity, not a direct compound-level match.

### Comparative genomics (ANI)

**96.21% ANI** against the *E. profundum* type strain (GCF_025234635.1) — confirms species-level identity (≥96% threshold).

---

## Skills demonstrated

- Retrieval of raw sequencing data from NCBI SRA / ENA
- Illumina and Nanopore read QC and interpretation (FastQC)
- Read trimming and long-read filtering (Trimmomatic, Filtlong)
- Hybrid genome assembly (Unicycler) and quality assessment (QUAST, CheckM)
- Bacterial genome annotation (Prokka)
- Biosynthetic gene cluster mining and interpretation (antiSMASH)
- Comparative genomics (ANI)
- Cloud-based bioinformatics workflows (Galaxy, KBase) — no local compute required
- Troubleshooting real pipeline failures (quality-encoding detection errors, tool/server compatibility, file-format mismatches) using job logs

## Author

**Afnan Asharaf** — MSc Microbiology, independent researcher
GenBank depositions: PZ163764, PZ163949, PZ163977 · ASM member
GitHub: [@AfnanAsharf](https://github.com/AfnanAsharf)
