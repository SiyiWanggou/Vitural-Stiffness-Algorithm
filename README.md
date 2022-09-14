# **Welcome to VIrtual Stiffness Algorithm (VISA) project.**

This code repository is supportive for generating virtual stiffness on TCGA glioma RNAseq dataset from Xi Huang Lab, Sickkids, CA. 

Link to Xi Huang Lab:https://lab.research.sickkids.ca/huang/

## Introduction

Tissue stiffness is collectively determined by the stiffness of ECM and cells. Gliomas are characterized by dysregulated expression of ECM proteins, ECM crosslinking enzymes, and aberrant cellular contractility.While The Cancer Genome Atlas (TCGA) datasets have been comprehensively analyzed to interrogate tumor genomes, epigenomes, and transcriptomes, we realized an untapped potential: using transcriptomic data coupled with analysis of Gene Ontology (GO) gene sets associated with ECM and actomyosin contractility to establish a bioinformatic tool, which we named VIrtual Stiffness Algorithm (VISA), capable of inferring tissue stiffness.

### Workflow of virtual stiffness algorithm

![ssGSEA_VISA.png](https://github.com/SiyiWanggou/Vitural-Stiffness-Algorithm/blob/main/Results/ssGSEA_VISA.png)

To generate virtual stiffness using bulk RNA-seq data, we first selected GO gene sets associated with extracellular matrix and actomyosin contractility from MSigDB. Then, the selected gene sets were enriched by ssGSEA for each sample to generate ssGSEA score. To normalize ssGSEA score, we calculated Z-score of each gene set across samples independently. The matrix of normalized ssGSEA score of selected extracellular matrix and actomyosin contractility gene sets in each sample was imported into the following algorithm to generate virtual stiffness of each sample:



As such, the virtual stiffness is considered as a transcriptomic index to infer tissue stiffness.