# DEG_CrossRef
Computational Pipeline to co-analyze gene expression and chromatin state data to identify transcriptional changes in Cancer.
This repo is an example of the starting state for the pipeline, with the required data, files and structure to run the pipeline. 
Chromatin State data should be retrieved from EpiMap and gene expression data is available through the GDC hosted by the NCI. 

## Outputs:
- Identification of genes with changes between healthy and cancer chromatin states
- DESeq2 results from input gene expression data
- List of genes with aligned chromatin and gene expression changes, with a 'combined_score' as the weighted average of the two
- GSEA of DESeq2 results, Chromatin changes alone and "Overlap" between the two

The pipeline is written in Snakemake, see the Snakefile for an outline of the pipeline. 

The structure of the directory is very important.

See Intructions at the end to run.

### To run correctly:
- Need the root of the directory to host the Snakefile
- Genome file must be data/genomes/mane_transcripts_gene.gff, will be if you follow instructions below
- Gene Sets for GSEA must be in data/gsea_genesets/combined_gene_sets.gmt, will be if you follow instructions below
- liftOver over.chain files must be in data/enh_gene_links/liftOver, will be if you follow instructions below

## Data
### Chromatin State Data (EpiMap)
- Chromatin State data (from EpiMap) needs to be in data/chromatin_states/cancer and data/chromatin_states/healthy
    - These files must end in "_18_CALLS_segments.bed.gz", which they will be if sourced from EpiMap
    - Download these datasets from https://personal.broadinstitute.org/cboix/epimap/ChromHMM/observed_aux_18_hg19/CALLS/
    - See References for epimap_samples.csv to see the samples you want
- Enhancer-Gene Links (from EpiMap) needs to be in data/enh_gene_links/cancer and data/enh_gene_links/healthy
    - These files must end in "_collated_pred.tsv", which they will be if sourced from EpiMap
    - Download from https://personal.broadinstitute.org/cboix/epimap/links/links_corr_only/

### Gene Expression Data (GDC from NCI)
- Pipeline is configured to use gene expression data directly from the GDC (https://portal.gdc.cancer.gov)
- Create cohort of interest (for ex: select TGCA and LIHC for the Cancer Genome Atalas Liver Hepatocellular Carcinoma)
- Go to repository and select Data Type: Gene Expression Quantification
- Add files of interest to cart, download cart (data) and metadata (if desired)
- IMPORTANT: data will download as "gdc_download_{time}.tar.gz". Delete the {time} from the filename and place in data/gdc
- IMPORTANT: create a sample_sheet.csv that specifies the sample, id and filename of the downloaded datasets
    - GDC uses 01A = cancer and 11A = healthy, files must use these suffices to split into healthy/cancer groups

## Instructions to Run

For Windows/Linux:
- Clone this repo:
`git clone https://github.com/matthall1797/DEG_CrossRef`
- Optional: Replace HCC data (preloaded) with data of your choice (see above for sourcing)
- Open linux terminal (for Windows, Ubuntu)
- Ensure you have Python and Snakemake installed: 
    - `sudo apt update && sudo apt install python3 python3-pip -y` (if Python is not already installed)
    - `pip install snakemake` or 
        `conda install -c bioconda -c conda-forge snakemake`
- Run Snakefile from the root of the cloned repo:
 `snakemake -s Snakefile -p --use-conda --cores -1`
    - NOTE: the first time you run it will take ~20-30 min to download and install environments

For iOS:
- Get Windows or Linux
- Follow steps above

Any questions or errors please email me: matthall1797@gmail.com