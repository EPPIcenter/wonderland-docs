# Quality Control

For interactive QC visualization and reporting after running the MAD4HATTER pipeline, use [ampseq-qc](https://github.com/EPPIcenter/ampseq-qc). This tool is designed for targeted amplicon sequencing of *Plasmodium* and works with MAD4HATTER pipeline outputs.

This tool was designed around assessing MAD4HatTeR and PfPHAST sequencing runs, but can be run on pipeline outputs from any panel. 

## What It Provides

The QC report is available as a **Quarto Markdown** (`.qmd`) document (or Jupyter Notebook). It produces an HTML report with:

- Summaries of coverage across samples and amplicons  
- Visualizations of QC metrics  
- A list of suggested samples for reprep/repooling
- Filtered outputs

## What You Need

- **Pipeline outputs** from MAD4HATTER, including `sample_coverage.txt`, `amplicon_coverage.txt`, `allele_data.txt`, `panel_information/amplicon_info.tsv`
- **Sample manifest** with sample metadata such as sample_name, SampleType, Batch, well position, and parasitemia

## How to Use

You need to clone the [ampseq-qc](https://github.com/EPPIcenter/ampseq-qc) repository to your computer before you can edit paths and render the report:

```bash
git clone https://github.com/EPPIcenter/ampseq-qc.git
```

1. Set paths to your MAD4HATTER results directory and sample manifest in the Quarto document
2. Change any filters you would like to set
3. Render the document (e.g., in RStudio or via the Quarto extension) to generate the HTML report

For installation, required inputs, and detailed usage, see the [ampseq-qc README](https://github.com/EPPIcenter/ampseq-qc).
