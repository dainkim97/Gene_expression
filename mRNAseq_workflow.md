Bioinformatics of gene expression

# Mapping with reference genome
- Download reference genome (e.g., NCBI)
- QC (BUSCO and Bowtie)
- Qualification (Salmon)
- Annotation (Entap)


# De novo seq workflow
- Assembly (Trinity)
  https://github.com/trinityrnaseq/trinityrnaseq/wiki

- QC (BUSCO and Bowtie)

- (SuperTranscripts (collapsing the isoform data))
  https://github.com/trinityrnaseq/trinityrnaseq/wiki/SuperTranscripts

- Qualification (Salmon)
  link: https://salmon.readthedocs.io/en/latest/salmon.html
  1. Normal version
  2. Normalized based on TPM
     HPC TPM sum -> (R) visualization to decide cut-off range -> (HPC) extract contigs name -> (HPC) Filtering
     (HPC) extract contigs name : awk '$2 > 1.5 {print $1}' contig_TPM_sum.txt > keep_contigs_tpm1.5.txt #filename: contig_TPM_sum.txt #filtering file name
     (HPC) Filtering: awk 'NR==FNR {keep[$1]; next} FNR==1 || ($1 in keep)' keep_contigs_tpm1.txt supertranscript_nozero.matrix.tsv > supertranscript_counts.filtered_tpm1.matrix


- Annotation (Entap)
  link: https://entap.readthedocs.io/en/latest/Getting_Started/ini_files.html

- 
