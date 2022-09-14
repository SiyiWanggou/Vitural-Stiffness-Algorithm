#### Load the TCGA LGG-GBM RNAseq dataset.

```R
TCGA_Matrix <- read.delim("LGG-GBMRNA-seq-clean-t.txt",header=T,row.names = 1)
TCGA_Matrix <- as.matrix(TCGA_Matrix)
```

#### Load the TCGA annotation of LGG-GBM RNAseq dataset.

```R
Annotation <- read.delim("TCGA_A_Phenotype_data_from_cell.txt",header=T,row.names = 1,na.strings = "NA")
```

#### Load the virtual stiffness gene sets.

```R
library(GSVA)
library(GSEABase)
geneSets<-getGmt("Pathways.txt")
```

#### Generate normalized ssGSEA score.

```R
gsva_results<-gsva(TCGA_Matrix,geneSets,method="ssgsea",parallel.sz = 10,verbose = TRUE)
gsva_results_score <- t(scale(t(gsva_results)))
```

#### Add annotation to annotation_col.

```R
annotation_col <- Annotation
annotation_col$Sample <- row.names(annotation_col)
```

#### Generate virtual stiffness of each sample.

```R
virtual_stiffness <- colMeans(gsva_results_score)
virtual_stiffness <- cbind(names(virtual_stiffness),virtual_stiffness)
colnames(virtual_stiffness) <- c("Sample","virtual_stiffness")
```

#### Add virtual stiffness score to annotation file.

```R
virtual_stiffness_anno <- merge(annotation_col,virtual_stiffness,by="Sample",all=FALSE)
```

#### Plot the heatmap of ssGSEA score on virtual stiffness gene sets annotated by virtual stiffness and associated biomarkers.

```R
selected_anno <- data.frame(virtual_stiffness_anno$Sample,
                            virtual_stiffness_anno$Supervised.DNA.Methylation.Cluster,
                            virtual_stiffness_anno$TERT.promoter.status,
                            virtual_stiffness_anno$Chr.7.gain.Chr.10.loss,
                            virtual_stiffness_anno$MGMT.promoter.status,
                            virtual_stiffness_anno$IDH.codel.subtype,
                            virtual_stiffness_anno$X1p.19q.codeletion,
                            virtual_stiffness_anno$IDH.status,
                            virtual_stiffness_anno$Grade,
                            virtual_stiffness_anno$virtual_stiffness
)
colnames(selected_anno) <- c("Sample","Supervised.DNA.Methylation.Cluster","TERT.promoter.status",
                             "Chr.7.gain.Chr.10.loss","MGMT.promoter.status","IDH.codel.subtype",
                             "X1p.19q.codeletion","IDH.status","Grade","virtual_stiffness")
row.names(selected_anno) <- selected_anno$Sample
selected_anno <- selected_anno[,-1]
col_order <- rownames(selected_anno[order(selected_anno$virtual_stiffness),])
row_order <- c("GO_EXTRACELLULAR_MATRIX_ASSEMBLY","GO_CELL_MATRIX_ADHESION","GO_CELL_SUBSTRATE_ADHESION","GO_INTEGRIN_BINDING",
               "GO_COLLAGEN_BINDING","GO_EXTRACELLULAR_MATRIX_BINDING","GO_EXTRACELLULAR_MATRIX_STRUCTURAL_CONSTITUENT",
               "GO_EXTRACELLULAR_MATRIX_COMPONENT","GO_PROTEINACEOUS_EXTRACELLULAR_MATRIX","GO_COMPLEX_OF_COLLAGEN_TRIMERS",
               "GO_COLLAGEN_FIBRIL_ORGANIZATION","GO_COLLAGEN_TRIMER","GO_ACTIN_FILAMENT_ORGANIZATION",
               "GO_ACTOMYOSIN_STRUCTURE_ORGANIZATION","GO_ACTIN_CYTOSKELETON_REORGANIZATION")
ann_colors = list(
  virtual_stiffness = c("yellow","black","red"),
  Supervised.DNA.Methylation.Cluster = c("Classic-like"="#FF0000","Codel"="#00FF00","G-CIMP-high"="#0000FF","G-CIMP-low"="#FFFF00","LGm6-GBM"="#FF8000","Mesenchymal-like"="#FF00FF","PA-like"="#00FFFF"),
  TERT.promoter.status = c("Mutant" = "#FF0000","WT" = "#3333FF"),
  Chr.7.gain.Chr.10.loss = c("Gain chr 7 & loss chr 10" = "#FF0000","No combined CNA" = "#3333FF"),
  MGMT.promoter.status = c("Methylated" = "#FF0000","Unmethylated" = "#3333FF"),
  IDH.codel.subtype = c("IDHmut-codel" = "#3333FF","IDHmut-non-codel" = "#00FFFF","IDHwt" = "#FF0000"),
  X1p.19q.codeletion = c("codel" = "#3333FF", "non-codel"="#FF0000"),
  IDH.status = c("Mutant"="#3333FF","WT"="#FF0000"),
  Grade = c("G2" = "#3333FF","G3" = "#FF8800","G4"="#FF0000")
)
breaksList = seq(-1.5,1.5, by = 0.01)
Cairo(file="ssGSEA_VISA.png",type="png",units="in",bg="white",width=12,height=4.5,pointsize=10,dpi=300)
pheatmap(gsva_results_score[row_order,col_order],
         annotation_col = selected_anno, annotation_colors = ann_colors,
         border_color = "black",
         color = colorRampPalette(c("yellow","black","red"))(length(breaksList)), breaks = breaksList,
         cluster_cols = FALSE, cluster_rows = FALSE,
         show_colnames = FALSE,
         legend = F,annotation_legend = F)
dev.off()
```

#### The heatmap shows virtual stiffness and ssGSEA scores of associated pathways on each glioma patient.

![ssGSEA_VISA.png](https://github.com/SiyiWanggou/Vitural-Stiffness-Algorithm/blob/main/Results/ssGSEA_VISA.png)