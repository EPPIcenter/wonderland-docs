# Version 1.0.0 Release: Major Output Format Changes

**Release Date:** 17/02/2026

**Previous Version:** v0.2.2

Version 1.0.0 introduces significant changes to the pipeline outputs, including standardized column naming conventions and new output files. There have also been changes in the code to address bugs and improve the accuracy of results. Updates have been added to make the pipeline easier to run, more robust, and easier for us to modify in future. This guide outlines the key differences to help you transition from v0.2.2 to v1.0.0. Brief release notes can be found on GitHub [here](https://github.com/EPPIcenter/mad4hatter/releases/tag/v1.0.0).

## Overview of Changes

### Parameter Changes 

Below are the key changes to running the pipeline. 

1. `--sequencer` has been removed. 
2. `--pools` has been added.
3. `--target` has been removed. 

`--pools` replaces `--target`. This allows for multiple pools to be listed, and the pipeline will automatically extract target information, references, and resmarkers depending on the combination of pools that you supply. This means that references only need to be supplied if you want to run something bespoke, for example by using `--genome` or `refseq_fasta` to supply your own full genome or targeted reference sequences, or `--resmarker_info` to supply a different table of resmarkers. 

For pre-configured pools, you can run a command like the one below. The pipeline will handle the rest. 

Example for mad4hatter pools using docker 
```
nextflow run main.nf --readDIR path/to/fastq/folder/ --pools D1,R1,R2 -profile docker
```

Example for PfPHAST pools using sge and apptainer
```
nextflow run main.nf --readDIR path/to/fastq/folder/ --pools M1,M2 -profile sge,apptainer
```

### Output Changes 

The most substantial changes in v1.0.0 are:

1. **Column naming standardization**: Where possible, column names have been converted to conform with data standards (e.g. [PMO](https://plasmogenepi.github.io/PMO_Docs/)) and other groups' bioinformatic pipeline outputs. 
2. **Enhanced allele data outputs**: New columns have been added to `allele_data.txt` to provide additional information. A collapsed version of the allele data is also included, eliminating the need for you to collapse masked results manually.
3. **New panel information directory**: We have made it easier to run the different panels and pools using the `--pools` parameter. This can be a single pool (e.g., `--pools D1`) or multiple (e.g., `--pools D1,R1,R2`). The pipeline will put together information on the targets within these pools and output this information into Panel configuration files that are now included in outputs. This includes target information, reference sequences used, and resmarker information. 
4. **Removed quality report directory**: Quality reporting has been restructured. 

#### Column Name Mappings

**allele_data.txt**

| v0.2.2 | v1.0.0 | Notes |
|--------|--------|-------|
| `SampleID` | `sample_name` | Sample identifier |
| `Locus` | `target_name` | Target/amplicon identifier |
| `ASV` | `asv` | Amplicon Sequence Variant (unmasked) |
| `Reads` | `reads` | Read count for the unmasked sequence/ pseudocigar|
| `Allele` | *(removed)* | Allele identifier (e.g., `Locus.1`) - not needed as ASV sequences serve as unique identifiers |
| - | `pseudocigar_unmasked` | **New**: CIGAR string for unmasked sequence |
| - | `asv_masked` | **New**: Masked ASV sequence, where masking is represented by `n`. Includes alignment information (e.g., deletions represented as `-`) |
| `PseudoCIGAR` | `pseudocigar_masked` | CIGAR string for masked sequence |
| - | `pool` | **New**: Pool(s) that the target was from |

**Note:** The v1.0.0 `allele_data.txt` contains both masked and unmasked sequences and pseudocigars, while the old version only had unmasked sequences and masked pseudocigars. Where variation between sequences was masked, users previously had to collapse this themselves using the pseudocigar. A new file, `allele_data_collapsed.txt`, provides the masked-only version.

**allele_data_collapsed.txt**

This is a **new file** in v1.0.0 that contains only masked sequences:

| Column | Description |
|--------|-------------|
| `sample_name` | Sample identifier |
| `target_name` | Target/amplicon identifier |
| `asv_masked` | Masked ASV sequence |
| `pseudocigar_masked` | CIGAR string for masked sequence |
| `reads` | Read count for masked sequence/pseudocigar |
| `pool` | Pool identifier |

**sample_coverage.txt**

| v0.2.2 | v1.0.0 | Notes |
|--------|--------|-------|
| `SampleID` | `sample_name` | Sample identifier |
| `Stage` | `stage` | Processing stage |
| `Reads` | `reads` | Read count at this stage |

**amplicon_coverage.txt**

| v0.2.2 | v1.0.0 | Notes |
|--------|--------|-------|
| `SampleID` | `sample_name` | Sample identifier |
| `Locus` | `target_name` | Target/amplicon identifier |
| `Reads` | `reads` | Read count after demultiplexing |
| `OutputDada2` | `OutputDada2` | read count after DADA2 |
| `OutputPostprocessing` | `OutputPostprocessing` | Read count after OutputPostprocessing |

##### Resistance Marker Tables

Resistance marker tables use snake_case column names, and some columns have been renamed for clarity:

**resmarker_table.txt**

| v0.2.2 | v1.0.0 | Notes |
|--------|--------|-------|
| `SampleID` | `sample_name` | Sample identifier |
| `GeneID` | `gene_id` | Gene identifier |
| `Gene` | `gene` | Gene name |
| `CodonID` | `aa_position` | Codon/amino acid position in the gene |
| `RefCodon` | `ref_codon` | Reference codon (3 bases) |
| `Codon` | `codon` | Codon sequence |
| `CodonRefAlt` | `codon_ref_alt` | REF or ALT |
| `RefAA` | `ref_aa` | Reference amino acid |
| `AA` | `aa` | Amino acid |
| `AARefAlt` | `aa_ref_alt` | REF or ALT |
| `FollowsIndel` | `follows_indel` | If the codon followed an indel in the sequence |
| `CodonMasked` | `codon_masked` | If the codon fell in a masked region |
| `MultipleLoci` | `multiple_loci` | If the codon was found across targets that overlap the region |
| `Reads` | `reads` | Read count |

**resmarker_table_by_locus.txt**

| v0.2.2 | v1.0.0 | Notes |
|--------|--------|-------|
| `SampleID` | `sample_name` | Sample identifier |
| `GeneID` | `gene_id` | Gene identifier |
| `Gene` | `gene` | Gene name |
| `Locus` | `target_name` | Target/amplicon identifier |
| `CodonID` | `aa_position` | Codon/amino acid position |
| `RefCodon` | `ref_codon` | Reference codon |
| `Codon` | `codon` | Codon sequence |
| `CodonRefAlt` | `codon_ref_alt` | REF or ALT |
| `RefAA` | `ref_aa` | Reference amino acid |
| `AA` | `aa` | Amino acid |
| `AARefAlt` | `aa_ref_alt` | REF or ALT |
| `FollowsIndel` | `follows_indel` | If the codon followed an indel in the sequence |
| `CodonMasked` | `codon_masked` | If the codon fell in a masked region |
| `Reads` | `reads` | Read count |

**resmarker_microhaplotype_table.txt**

| v0.2.2 | v1.0.0 | Notes |
|--------|--------|-------|
| `SampleID` | `sample_name` | Sample identifier |
| `GeneID` | `gene_id` | Gene identifier |
| `Gene` | `gene` | Gene name |
| `Locus` | `target_name` | Target/amplicon identifier |
| `MicrohaplotypeCodonIDs` | `mhap_aa_positions` | Codon positions (e.g., `16/51/59`) |
| `RefMicrohap` | `ref_mhap` | Reference microhaplotype |
| `Microhaplotype` | `mhap` | Observed microhaplotype |
| `MicrohapRefAlt` | `mhap_ref_alt` | REF or ALT |
| `Reads` | `reads` | Read count |

**all_mutations_table.txt**

| v0.2.2 | v1.0.0 | Notes |
|--------|--------|-------|
| `SampleID` | `sample_name` | Sample identifier |
| `GeneID` | `gene_id` | Gene identifier |
| `Gene` | `gene` | Gene name |
| `Locus` | `target_name` | Target/amplicon identifier |
| `LocusPosition` | `target_position` | Position in the target |
| `Alt` | `alt` | Alternative base (lowercase) |
| `Ref` | `ref` | Reference base (lowercase) |
| `Reads` | `reads` | Read count |

#### New Output Files and Directories

**panel_information/**

A new directory containing panel configuration files:

- `amplicon_info.tsv`: Amplicon target information
- `reference.fasta`: Reference sequences used
- `resmarker_info.tsv`: Resistance marker information (if applicable)

These files provide transparency about the panel configuration used in the run. Raw pool configuration files can be found under `panel_information/` in the repository. Each pool has an amplicon_info file describing the targets and a reference, providing the reference sequences for each target in the pool. If you run the pipeline with multiple pools (e.g., `D1,R1,R2`), then the output files above will be a combination for all of the pools. The pipeline contains an extensive list of resmarkers of interest. The pipeline will check which resmarkers are covered by the pools you have set, and the resmarkers identified will be output in `panel_information/resmarker_info.tsv`. If you wish to edit the resmarkers called, see [here](../parameters.md#resistance-markers).

#### Removed Outputs

**quality_report/**

The `quality_report/` directory has been removed in v1.0.0. A more [extensive, interactive QC](https://github.com/EPPIcenter/ampseq-qc) is now available. Therefore, we have removed old visualisations that were largely unused from the pipeline outputs.

#### Target/Locus Naming Changes

The target naming convention has changed. In v0.2.2, targets were named with 1-based coordinates, including the primers. Pools were appended to the end:
```
Pf3D7_01_v3-145388-145662-1A
```

In v1.0.0, targets use 0-based genomic coordinates of the region covered by the target with zero-padding:
```
Pf3D7_01_v3-0145420-0145630
```

**Key differences:**
- Coordinates are 0-based. 
- Coordinates are for the start and end of the insert of the target and do not include the primers.
- Coordinates are now zero-padded (e.g., `0145420` instead of `145420`)
- Pool indicators (`-1A`, `-1B`) have been removed from target names as this led to inconsistency when the same target was in multiple pools. Pool information is now stored elsewhere in the outputs.

Conversion tables for target names for each pool can be found under `panel_information/` in the repository. 

## Pipeline Changes 

Some changes were made that may impact results. Below are descriptions of these changes. 

- Demultiplexing: There was an edge case issue identified where the first primer was being used to demultiplex. If the reverse primer was not a perfect match, it was not trimmed, but the read still passed through. This was an issue we found documented [here](https://github.com/marcelm/cutadapt/issues/692). We have fixed this in the new demultiplexing step by putting the reads through two rounds of cutadapt, so only reads where both the forward and reverse primers are present will pass through to the next stage of the pipeline. 
- Long amplicons: A bug was introduced in v0.2.1 [PR #133](https://github.com/EPPIcenter/mad4hatter/pull/133). The pipeline concatenates reads that do not overlap enough to merge with 10*N. This retains reads for long amplicons. These long reads are then collapsed where they overlap to provide more support for sequences. A bug was introduced that caused these sequences not to be merged into the final outputs. This has been fixed in this release. This will only impact long targets. 
- Collapsing concatenated reads was being performed across samples and is now only being done within a specific sample. This will only impact long targets.
- DADA2 merges forward and reverse sequences after clustering. In previous releases, one mismatch was allowed when merging. This sometimes caused problems when the reads were merged (e.g., both bases being retained and being flagged as an indel). We have updated this to allow for zero mismatches when merging. This removes the issue with merging, although it can cause a reduction in reads in the final outputs. We have found that this mostly impacts poor-quality samples that would not have passed basic downstream QC requirements anyway.  
- Removing the `--sequencer` flag: This flag used to be used to control whether a two-colour instrument was used in sequencing. In situations where this was the case (e.g., NextSeq), additional filters were applied in the cutadapt step. G trimming was applied to remove any trailing G bases. This was sometimes causing real biological variation of G bases at the end of reads to be removed. This meant that this variation was being missed or reported incorrectly as an indel, due to the merging issue described above. We found that these trailing G bases are filtered out elsewhere in the pipeline and have therefore removed this as a mandatory input parameter to the pipeline. We have exposed two new parameters, `--gtrim` and `--quality_score`, with defaults of false and 20, respectively. This means that the old filtering can be retained if desired. For MiSeq runs the quality score used to be 10 by default; therefore this filtering is stricter than before. 

## Other Changes 

Some updates were made to make the pipeline more robust and easier to run. Many of these will not impact users, but below is a description of these updates. 

1. Additional resmarkers have been requested and added to the outputs. 
2. Additional panels have been configured to easily run through the pipeline (e.g., spotmalaria and AMPLseq).
3. Full unit and integration testing, allowing for quicker and more robust updates to the pipeline. 
4. Automated Docker builds. 
5. A template for logging questions, bugs, and feature requests.

## Migration Tips

1. **Update scripts**: Update column references to the new names (snake_case throughout; resistance marker tables also use `gene_id`, `aa_position`, `ref_codon`, `ref_aa`, `mhap_aa_positions`, `target_position`, etc.).
2. **Use collapsed file**: If you only need masked sequences, use `allele_data_collapsed.txt` instead of filtering `allele_data.txt`.
3. **Check target names**: Target names have changed format; update lookup tables or mapping files as needed.
4. **Panel information**: The `panel_information/` directory provides reference files that can help with mapping between old and new target names.

## Summary

The v1.0.0 release standardizes output formats with snake_case column names and clearer names where needed (e.g. `CodonID` → `aa_position`, `LocusPosition` → `target_position`). Allele data now includes both masked and unmasked sequences. The underlying data structure and content remain the same; update your scripts using the mappings above.

