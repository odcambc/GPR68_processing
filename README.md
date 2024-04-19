[![DOI](https://zenodo.org/badge/736493453.svg)](https://zenodo.org/doi/10.5281/zenodo.10999527)

# GPR68 DMS sequencing processing workflow with Dumpling

This repository contains the snakemake-based workflow for implementing the
deep mutational scanning experiments in ["Molecular basis of proton-sensing by G protein-coupled receptors," Matthew K Howard, Nicholas Hoppe, Xi-Ping Huang, Christian B Macdonald, Eshan Mehrotra, Patrick Rockefeller Grimes, Adam M Zahm, Donovan D Trinidad, Justin G English, Willow Coyote-Maestas, Aashish Manglik bioRxiv 2024.04.17.590000; doi: https://doi.org/10.1101/2024.04.17.590000](https://doi.org/10.1101/2024.04.17.590000). This was 
performed using our [Dumpling](https://github.com/odcambc/dumpling) pipeline.

If you don't want to reproduce the analysis, but just want to see the results,
the scores are available at XZY, and the detailed processing QC information is
available [in another repository](https://github.com/odcambc/GPR68_DMS_QC).

## Reproducing
### Overview
The entire processing workflow can be reproduced using the provided configuration files by providing the raw reads, which are available on the NCBI SRA.

See the main dumpling repository for more details about the configuration used here. The following is a minimal
list of requirements to successfully rerun the pipeline on your own system:

### Dependencies

#### Via conda (recommended)
The simplest way to handle dependencies is with [Conda](https://conda.io/docs/) and the provided environment file.

```bash
conda create --name gpr68_dumpling --file spec-file.txt
```

This will create a new environment named `gpr68_dumpling` with all the dependencies installed. Then simply activate the environment and you're ready to go.

```bash
conda activate gpr68_dumpling
```

#### Manually

The following are the dependencies required to run the pipeline:

* [Snakemake](https://snakemake.readthedocs.io/en/stable/)
* [GATK](https://software.broadinstitute.org/gatk/)
* [bbtools](https://jgi.doe.gov/data-and-tools/bbtools/)
* [samtools](http://www.htslib.org/)
* [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
* [multiqc](http://multiqc.info/)
* [Enrich2](https://enrich2.readthedocs.io/en/latest/)

### Configuration

Once the raw reads have been downloaded, the directory needs to be set in the configuration file.

In `config/gpr68_dec2022.yaml`, change the `data_dir` variable to the path to the directory containing the raw reads.

That's it!

## Usage

### Running the pipeline

Once the dependencies have been installed (whether via conda or otherwise) the pipeline can be run with the following command:

```bash
snakemake -s workflow/Snakefile --use-conda --cores 8
```

The maximum number of cores can be specified with the `--cores` flag. The `--use-conda` flag tells snakemake to use conda to create the environment specified within each rule.

## Results

### Statistics
The pipeline creates a set of QC metrics for each sample, going from
raw reads (with FastQC) through to variant calling and scoring. These
are aggregated into a single report using MultiQC, which is saved in
`stats/{experiment_name}_multiqc_report.html` upon completion.

### Scores
Final calculated Enrich2 scores will be in the `results/gpr68_dec2022/enrich/tsv/gpr68_dec2022_exp/` folder.
