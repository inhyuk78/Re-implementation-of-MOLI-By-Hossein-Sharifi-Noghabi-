# Re-implementation-of-MOLI-By-Hossein-Sharifi-Noghabi-
This repository contains all files relating to my personal project of re-implementing "MOLI: multi-omics late integration with deep neural networks for drug response prediction" research paper by Hossein Sharifi-Noghabi.

### Motivation

I chose to reimplement “MOLI: multi-omics late integration with deep neural networks for drug response prediction” for two reasons. During my Master of Pharmacy degree, I saw first-hand how severe the adverse effects of cancer drugs can be – bone marrow suppression across most chemotherapies, severe nausea and vomiting with Cisplatin, to name a few. Treatment decisions still rely heavily on population-level clinical trial evidence, which tells us how a drug performs on average but not how a given patient’s tumour will respond. This gap creates a degree of trial-and-error where patients can be exposed to a drug’s toxicity without any guarantee it will work for them. MOLI’s approach of integrating cancer gene expression, somatic mutation, copy number alteration data to predict drug response presents as a concrete step towards individualized, precision treatment, where therapy can be guided by molecular characteristics of each tumour rather than population averages alone.

I was also drawn to how easily the model architecture could extend to additional omics layers, such as DNA methylation or proteomics. With this in mind, during the reimplementation I wrote the autoencoder that extracts features from omics data as a reusable function that could be applied to new omics data without modification, rather than hardcoding it to the three omics data used in the original study. This provides a foundation for future extensions of the model.


### Method 

*Data importing & cleaning*

As per MOLI research paper, I imported the RNA sequence, mutation, copy number variation (CNV) and drug sensitivity data from the Genomics of Drug Sensitivity in Cancer (GDSC) dataset, released by the Wellcome Sanger Institute. 

RNA sequence data was reshaped from long to wide format (944 cell lines x 41143 genes), which introduced missing values for cell line-gene pairs absent from the original dataset. Genes with fewer than 20% missing values were mean-imputed, since the mean preserves each gene’s overall expression level without introducing outliers; genes exceeding this threshold were dropped, as imputing a large proportion of missing values would risk distorting the gene’s true expression distribution. Mutation data was processed similarly, but missing values were imputed with 0, reflecting the binary encoding of the data where 0 indicates no cancer-driving mutation. CNV data followed the same approach, with missing values imputed as 0 to represent a neutral copy number state (neither amplification nor deletion). 


*Data visualization*

Principal Component Analysis was performed on gene expression data for visualization in the 3-dimensional space. However, this did not reveal clear separation by cancer type; instead, it displayed two broad clusters not aligned with cancer type labels. This could suggest batch effects, or a dominant biological signal (eg. proliferation rate) independent of cancer type. Gene mutations were sparsely distributed across the mutation data; therefore, only cell lines with mutation rate of atleast 5% were retained for oncoprint visualization. The oncoprint revealed high mutation rates in the TP53 gene, which is to be expected as it is the most commonly mutated gene in human cancers [2]. Copy number variation data (gain/amplification, loss/deletion, neutral) were numerically encoded as +1, -1, and 0 respectively. Genes with missing values in more than 10% of cell lines were excluded and remaining missing values were treated as neutral. Visualizing this data as an oncoprint showed that copy number variations were widespread. 


