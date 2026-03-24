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
     HPC TPM sum (Salmon) -> (R) visualization to decide cut-off range -> (HPC) extract contigs name -> (HPC) Filtering
     (HPC) salmon : > salmon quant -i index -l IU -1 <(gunzip -c lib_1_1.fq.gz lib_2_1.fq.gz) -2 <(gunzip -c lib_1_2.fq.gz lib_2_2.fq.gz) --validateMappings -o out
     Mu case0 salmon quant -i "${ST_INDEX}" -l IU -1 <(gunzip -c C24-05-15-5-Ad-R-B9_val_1.fq.gz C24-05-15-5-E-R-A9_val_1.fq.gz C24-05-19-02-E-G-C1_val_1.fq.gz C24-05-23-06-Ad-R-H2_val_1.fq.gz C24-05-25-03-E-G-E9_val_1.fq.gz C24-05-3-05-Ad-G-D4_val_1.fq.gz C24-05-7-03-Ad-R-B1_val_1.fq.gz C24-05-7-03-E-A1_val_1.fq.gz C24-05-8-03-Ad-G-H9_val_1.fq.gz C24-05-8-03-E-G-G9_val_1.fq.gz C24-06-1-03-E-R-E1_val_1.fq.gz C24-06-2-01-Ad-R-B4_val_1.fq.gz C24-06-2-01-E-R-A4_val_1.fq.gz C24-06-2-06-Ad-G-F2_val_1.fq.gz C24-06-2-06-E-G-E2_val_1.fq.gz C24-06-25-03-Ad-G-F9_val_1.fq.gz C24-06-3-05-E-G-C4_val_1.fq.gz C24-06-4-03-Ad-G-C2_val_1.fq.gz C24-06-4-2-Ad-R-D9_val_1.fq.gz C24-06-4-2-E-R-C9_val_1.fq.gz C24-07-2-06-Ad-G-F4_val_1.fq.gz C24-07-2-06-E-G-E4_val_1.fq.gz C24-07-5-04-Ad-R-H4_val_1.fq.gz C24-07-5-04-E-R-G4_val_1.fq.gz C24-10-4-05-Ad-R-B5_val_1.fq.gz C24-10-4-05-E-R-A5_val_1.fq.gz C24-10-5-06-Ad-G-H5_val_1.fq.gz C24-10-5-06-E-G-G5_val_1.fq.gz C24-10-6-01-Ad-R-D5_val_1.fq.gz C24-10-6-01-E-R-C5_val_1.fq.gz C24-10-6-06-Ad-R-F5_val_1.fq.gz C24-10-6-06-E-R-E5_val_1.fq.gz) -2 <(gunzip -c C24-05-15-5-Ad-R-B9_val_2.fq.gz C24-05-15-5-E-R-A9_val_2.fq.gz C24-05-19-02-E-G-C1_val_2.fq.gz C24-05-23-06-Ad-R-H2_val_2.fq.gz C24-05-25-03-E-G-E9_val_2.fq.gz C24-05-3-05-Ad-G-D4_val_2.fq.gz C24-05-7-03-Ad-R-B1_val_2.fq.gz C24-05-7-03-E-A1_val_2.fq.gz C24-05-8-03-Ad-G-H9_val_2.fq.gz C24-05-8-03-E-G-G9_val_2.fq.gz C24-06-1-03-E-R-E1_val_2.fq.gz C24-06-2-01-Ad-R-B4_val_2.fq.gz C24-06-2-01-E-R-A4_val_2.fq.gz C24-06-2-06-Ad-G-F2_val_2.fq.gz C24-06-2-06-E-G-E2_val_2.fq.gz C24-06-25-03-Ad-G-F9_val_2.fq.gz C24-06-3-05-E-G-C4_val_2.fq.gz C24-06-4-03-Ad-G-C2_val_2.fq.gz C24-06-4-2-Ad-R-D9_val_2.fq.gz C24-06-4-2-E-R-C9_val_2.fq.gz C24-07-2-06-Ad-G-F4_val_2.fq.gz C24-07-2-06-E-G-E4_val_2.fq.gz C24-07-5-04-Ad-R-H4_val_2.fq.gz C24-07-5-04-E-R-G4_val_2.fq.gz C24-10-4-05-Ad-R-B5_val_2.fq.gz C24-10-4-05-E-R-A5_val_2.fq.gz C24-10-5-06-Ad-G-H5_val_2.fq.gz C24-10-5-06-E-G-G5_val_2.fq.gz C24-10-6-01-Ad-R-D5_val_2.fq.gz C24-10-6-01-E-R-C5_val_2.fq.gz C24-10-6-06-Ad-R-F5_val_2.fq.gz C24-10-6-06-E-R-E5_val_2.fq.gz) --validateMappings -o out
     (HPC) extract contigs name : awk '$2 > 1.5 {print $1}' contig_TPM_sum.txt > keep_contigs_tpm1.5.txt #filename: contig_TPM_sum.txt #filtering file name
     (HPC) Filtering: awk 'NR==FNR {keep[$1]; next} FNR==1 || ($1 in keep)' keep_contigs_tpm1.txt supertranscript_nozero.matrix.tsv > supertranscript_counts.filtered_tpm1.matrix


- Annotation (Entap)
  link: https://entap.readthedocs.io/en/latest/Getting_Started/ini_files.html

- 
