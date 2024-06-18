# 1 Overview

starTracer is an R package for marker gene identification in single-cell RNA-seq
data analysis. This package comprises two primary functional modules:
"searchMarker" and "filterMarker". The "searchMarker" function operates
independently, taking an expression matrix as input and generating a marker gene
matrix as output. On the other hand, the "filterMarker" function is tailored to
work as a complementary pipeline to the Seurat "FindAllMarkers" function,
offering a more accurate list of marker genes for each cluster in conjunction
with Seurat results.

# 2 Vignette

## 2.1 INTRODUCTION

A **beta-version**. This package will be able to used to search marker genes in
an efficient way. It takes an `SeuratObject` or the direct output `data.frame`
of `Seurat::FindAllMarkers()` function to search the ideal top N marker gene
with a higher specificity level and efficiency.

The function also provides a intuitive way to descibe the specificity of the
selected top N marker genes.

## 2.2 INTSALLATION

starTracer could now be downloaded from github via the following command:

```{r}
if (!requireNamespace("devtools", quietly = TRUE)){
install.packages("devtools")}

devtools::install_github("JerryZhang-1222/starTracer")
```

## 2.3 USAGE

### 2.3.1 Introduction

`starTracer` could be used to search Marker Genes from the single-cell/nuclear
sequencing data. Marker genes are defined as the most up-regulated and
most-specific genes in each cluster. `starTracer` will be able to generate the
topN marker genes for each cluster in a effective way. The two most commonly
used function are:

1.  `starTracer::searchMarker()`: a de-novo pipeline. The function takes in a
    `Seurat` object/Sparse matrix + Annotation/Average Expression Matrix and
    then calculate the marker genes for each cluster.
2.  `starTracer::filterMarker()`:an in-conjunction pipeline. The function takes
    in a `data.frame` from the `Seurat::FindAllMArkers()` function, providing a
    new data.frame with, which could be used to reordering the marker genes from
    the `data.frame` the user provided.

### 2.3.2 searchMrker

searchMarker is designed to take multiple kinds of input data. Users may provide
input data as the following 3 formats:

1.  A Seurat object, with annotations.
2.  A Sparse Matrix of the single cell expression matrix and a annotation
    matrix.
3.  An average expression matrix.

For more details:

**importing a Seurat object**

The object should meet certain requirements:

1.  Annotations should be provided in the Seurat object.
2.  High Variable Features should be found before running starTracer, if users
    prefer using "HVG" as genes to use.
3.  Integrated data should be avoided. If users have integrated data from
    multiple samples, please do not overwrite the original "RNA" slot.

```{r}
res <- searchMarker(
  x = x, #the input seurat object
  thresh.1 = 0.5,
  thresh.2 = NULL,
  method = "del_MI", 
  num = 2,
  gene.use = NULL,
  meta.data = NULL,
  ident.use = NULL
)
```

**Importing a sparse matrix**

For users who may have processed the single cell experiment data in softwares
other than Seurat, users are suggested to output their expression data as a
Sparse Matrix(dgCMatrix) with cells in columns and features in rows and
populated with the normalized data. An annotation matrix is also required with
rows as cells corresponded with columns in the expression data.

```{r}
res <- searchMarker(
  x = x, #the input dgCMatrix
  thresh.1 = 0.5,
  thresh.2 = NULL,
  method = "del_MI",
  num = 2,
  gene.use = NULL,
  meta.data = meta.data, # the annotation matrix
  ident.use = NULL #the ident to use in the annotation matrix
)
```

**Importing an average expression matrix**

For users who have an average expression matrix with columns as clusters and
rows as features, you may directly input the data.

```{r}
res <- searchMarker( x = x, 
                     thresh.1 = 0.5, thresh.2 = NULL, method = "del_MI", num = 2, gene.use = NULL, 
                     meta.data = NULL, # the annotation matrix 
                     ident.use = NULL #the ident to use in the annotation matrix 
                     )
```

# 3 Benchmarking and Testing Scripts of searchMarker

Please refer to the following links for details in our original research
article. You may also find the basic usage examples here:

## 3.1 Speed, Specificity and Accuracy

In the following 3 scripts, we test the specificity, calculation speed and
accuracy in three samples:

[human brain](https://jerryzhang-1222.github.io/brain.html)

[human heart](https://jerryzhang-1222.github.io/heart_big.html)

[mouse kidney](https://jerryzhang-1222.github.io/kidney.html)

## Systematically Evaluating FPR in Generated Single Cell Data

## Comparisons with Multiple Methods

[Presto](https://jerryzhang-1222.github.io/Presto.html)

[Seurat Methods](https://jerryzhang-1222.github.io/Controls.html)

## Finding Marker Genes in Rare Cell Types

[rare cell
types](https://jerryzhang-1222.github.io/rare_cell_type_performance.html)

## Ability to Find Marker Genes in Different Cluster Levels

We here use the human brain sample, with the annotation of "bio_clust",
"major_clust" and "sub_clust", to test `starTracer::searchMarker`'s ability to
find maker genes across different annotation levels.

[different cluster
level](https://jerryzhang-1222.github.io/different_cluster_level.html)

## Parameter Control

Here we show the different results of using different thresh.2 (denoted as S2 in
the original research article). You may refer to this results to better
understand the influence of thresh.2 on your results.

[different threshold](https://jerryzhang-1222.github.io/Different_thresh.html)

# Testing Scripts of filterMarker

Here we show the results of filterMarker. filterMarker takes the output
matrix/data.frame of Seurat's FindAllMarkers function. Note that calculating
with samples of big cell numbers will be time consumable.

[filterMarker test](https://jerryzhang-1222.github.io/FilterMarkerTest_v3.html)
