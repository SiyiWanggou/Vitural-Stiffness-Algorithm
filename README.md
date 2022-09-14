# **Welcome to VIrtual Stiffness Algorithm (VISA) project.**

This code repository is supportive for generating virtual stiffness on TCGA glioma RNAseq dataset from Xi Huang Lab, Sickkids, CA. 

Link to Xi Huang Lab:https://lab.research.sickkids.ca/huang/

## Introduction

Tissue stiffness is collectively determined by the stiffness of ECM and cells. Gliomas are characterized by dysregulated expression of ECM proteins, ECM crosslinking enzymes, and aberrant cellular contractility.While The Cancer Genome Atlas (TCGA) datasets have been comprehensively analyzed to interrogate tumor genomes, epigenomes, and transcriptomes, we realized an untapped potential: using transcriptomic data coupled with analysis of Gene Ontology (GO) gene sets associated with ECM and actomyosin contractility to establish a bioinformatic tool, which we named VIrtual Stiffness Algorithm (VISA), capable of inferring tissue stiffness.

### Workflow of virtual stiffness algorithm

![VISA workflow.png](https://github.com/SiyiWanggou/Vitural-Stiffness-Algorithm/blob/main/Results/VISA%20workflow.png?raw=true)

To generate virtual stiffness using bulk RNA-seq data, we first selected GO gene sets associated with extracellular matrix and actomyosin contractility from MSigDB. Then, the selected gene sets were enriched by ssGSEA for each sample to generate ssGSEA score. To normalize ssGSEA score, we calculated Z-score of each gene set across samples independently. The matrix of normalized ssGSEA score of selected extracellular matrix and actomyosin contractility gene sets in each sample was imported into the following algorithm to generate virtual stiffness of each sample:

![VISA algorithm.png](https://github.com/SiyiWanggou/Vitural-Stiffness-Algorithm/blob/main/Results/VISA%20algorithm.png?raw=true)

As such, the virtual stiffness is considered as a transcriptomic index to infer tissue stiffness.

#### Data to reproduce the VISA results on TCGA LGG-GBM patients

TCGA LGG-GBM RNAseq dataset was downloaded from UCSC Xena browser (https://xenabrowser.net/datapages/?cohort=TCGA%20lower%20grade%20glioma%20and%20glioblastoma%20(GBMLGG)&removeHub=https%3A%2F%2Fxena.treehouse.gi.ucsc.edu%3A443). The clean RNAseq dataset to reproduce the result could be downloaded at:

https://drive.google.com/file/d/1NtKFeQ_wzdFXHXFVfiJOxBlc_lby35Q-/view?usp=sharing

Metadata of TCGA LGG-GBM RNAseq dataset was generated from TCGA pulications (). The clean metadata is provided as below:

[TCGA_glioma_metadata_from_cell_paper.txt](https://github.com/SiyiWanggou/Vitural-Stiffness-Algorithm/blob/main/Data/TCGA_glioma_metadata_from_cell_paper.txt)

The GO gene sets used for virtual stiffness algorithm is stored at:

[Pathways.txt](https://github.com/SiyiWanggou/Vitural-Stiffness-Algorithm/blob/main/Data/Pathways.txt)

#### Code to reproduce the VISA results on TCGA LGG-GBM patients

To acquire the code for virtual stiffness estimation, please see the following link:

[Generate Virtual Stiffness on TCGA gliomas.md](https://github.com/SiyiWanggou/Vitural-Stiffness-Algorithm/blob/main/Code/Generate Virtual Stiffness on TCGA gliomas.md)

