# `BCB420.2019.ESA`

#### (**E**xploratory **S**ystems **A**nalysis **Tools for BCB420-2019**)
<!-- [![DOI](https://zenodo.org/badge/157482801.svg)](https://zenodo.org/badge/latestdoi/157482801) -->

&nbsp;

###### [Boris Steipe](https://orcid.org/0000-0002-1134-6758),
###### Department of Biochemistry and Department of Molecular Genetics,
###### University of Toronto
###### Canada
###### &lt; boris.steipe@utoronto.ca &gt;

----

This README describes this package.
<!-- The associated Vignette can be previewed [here](http://htmlpreview.github.io/?https://github.com/hyginn/rptPlus/blob/master/doc/rptPlusVignette.html). The package can be installed from GitHub with `devtools::install_github("hyginn/rptPlus", build_opts = c("--no-resave-data", "--no-manual"))`. -->

----

**If any of this information is ambiguous, inaccurate, outdated, or incomplete,
please check the [most recent version](https://github.com/hyginn/BCB420.2019.ESA) of the
package on GitHub and 
[file an issue](https://github.com/hyginn/BCB420.2019.ESA/issues).**

----

<!-- TOCbelow -->
1. About this package:<br/>
2. Data ...<br/>
3. Notes<br/>
4. References and Further reading<br/>
5. Acknowledgements<br/>
<!-- TOCabove -->

----

## 1 About this package:

This package is a joint development platform for student designed tools for Exploratory Systems Analysis, part of the University of Toronto course BCB420H1S (Computational Systems Biology) in the 2018/2019 academic year.

&nbsp;


----

## 2 Data ...

Suporting resources include curated systems data and other data resources:

#### 2.1 The HGNC symbol reference

Load the `HGNC` object in the following way:

```R
myURL <- paste0("https://github.com/hyginn/",
                "BCB420-2019-resources/blob/master/HGNC.RData?raw=true")
load(url(myURL))  # loads HGNC data frame

str(HGNC)
# 'data.frame':	27087 obs. of  14 variables:
#  $ sym      : chr  "A1BG" "A1BG-AS1" "A1CF" "A2M" ...
#  $ name     : chr  "alpha-1-B glycoprotein" "A1BG antisense RNA 1"  ...
#  $ UniProtID: chr  "P04217" NA "Q9NQ94" "P01023" ...
#  $ RefSeqID : chr  "NM_130786" "NR_015380" "NM_001198818" "NM_000014" ...
#  $ EnsID    : chr  "ENSG00000121410" "ENSG00000268895" "ENSG00000148584" ...
#  $ UCSCID   : chr  "uc002qsd.5" "uc002qse.3" "uc057tgv.1" "uc001qvk.2" ...
#  $ GeneID   : num  1 503538 29974 2 144571 ...
#  $ OMIMID   : chr  "138670" NA "618199" "103950" ...
#  $ acc      : chr  NA "BC040926" "AF271790" "BX647329, X68728, M11313" ...
#  $ chr      : chr  "19q13.43" "19q13.43" "10q11.23" "12p13.31" ...
#  $ type     : chr  "protein" "lncRNA" "protein" "protein" ...
#  $ prev     : chr  NA "NCRNA00181, A1BGAS, A1BG-AS" NA NA ...
#  $ synonym  : chr  NA "FLJ23569" "ACF, ASP, ACF64, ACF65, APOBEC1CF"  ...
#  $ RefSeqOld: chr  "NM_130786" "NR_015380" "NM_014576" "NM_000014" ...

```

&nbsp;

#### 2.2 Transcription factors from GTRD:

`geneList` list contains transcription factors that have been found to bind to the upstream regulatory regions of genes in ChIP-seq experiments; the data is compiled by the GTRD database.
Load `geneList` in the following way:

```R
myURL <- paste0("http://steipe.biochemistry.utoronto.ca/abc/assets/",
                "geneList-2019-03-13.RData")
load(url(myURL))  # loads GTRD geneList object

str(geneList)
# List of 17864
#  $ HES4        : chr [1:82] "AR" "ATF2" "ATF3" "ATF4" ...
#  $ PUSL1       : chr [1:64] "AR" "BHLHE40" "CEBPA" "CEBPB" ...
#  $ ACAP3       : chr [1:69] "BHLHE40" "CEBPA" "CEBPB" "CEBPD" ...
#  $ ATAD3B      : chr [1:110] "AR" "ASCL2" "ATF2" "ATF3" ...
#  $ ATAD3A      : chr [1:128] "ARID4B" "ASCL2" "ATF1" "ATF3" ...
#   [list output truncated]

```

&nbsp;

#### 2.3 STRING edges:

`STRINGedges` contains edges of the STRING database mapped to HGNC symbols.
Load `STRINGedges` in the following way:

```R

myURL <- paste0("http://steipe.biochemistry.utoronto.ca/abc/assets/",
                "STRINGedges-2019-03-14.RData")
load(url(myURL))  # loads STRING edges object

str(STRINGedges)
#  Classes ‘tbl_df’, ‘tbl’ and 'data.frame':	319997 obs. of  3 variables:
#   $ a    : chr  "ARF5" "ARF5" "ARF5" "ARF5" ...
#   $ b    : chr  "SPTBN2" "KIF13B" "KIF21A" "TMED7" ...
#   $ score: num  909 910 910 906 971 915 905 927 914 902 ...
```

&nbsp;


#### 2.4 Expression profiles:

Expression profiles were compiled from 52 microarray experiments downloaded from GEO, and quantile normalized. Details to follow. Here is code to load and use the profiles.

```R

# Load the expression profiles:
myURL <- paste0("http://steipe.biochemistry.utoronto.ca/abc/assets/",
                "GEO-QN-profile-2019-03-24.rds")
myQNXP <- readRDS(url(myURL))  # loads quantile-normalized expression data

str(myQNXP)
#  num [1:27087, 1:52] 29.4 199.9 34.3 947.4 2249.2 ...
#  - attr(*, "dimnames")=List of 2
#   ..$ : chr [1:27087] "A1BG" "A1BG-AS1" "A1CF" "A2M" ...
#   ..$ : chr [1:52] "GSE35330.ctrl.4h" "GSE35330.cond.4h" "GSE35330.ctrl.16h" ...


# Some statistics:
sum( ! is.na(myQNXP)) # Number of measurements: 923361
mean(  myQNXP, na.rm = TRUE) # 502.7972
median(myQNXP, na.rm = TRUE) # 127.43
hist(log10(myQNXP), breaks = 100)

# how many HGNC genes have at least one measurement recorded?
sum( ! is.na(rowMeans(myQNXP, na.rm = TRUE))) # 21063

# how many HGNC genes have measurements recorded an all experiments?
sum( ! is.na(rowMeans(myQNXP, na.rm = FALSE))) # 10812

# colnames describe experiments (averaged over replicates)
colnames(myQNXP)

# how many unique experiments
myExp <- gsub("^([^\\.]+).+", "\\1", colnames(myQNXP))
length(unique(myExp)) # 15

#Access one profile by name
myQNXP["VAMP8", ]

# plot expression values
plot(myQNXP["VAMP8", ], ylab = "quantile normalized expression (AU)")

# add lines that separate the experiments
abline(v = cumsum(rle(myExp)$lengths) + 0.5, lwd = 0.5, col = "#0000CC44")

```

Gene / Gene correlations
```R
corGenes <- function(A, B, prf) {
  # Calculate pearson correlation between gene expression
  # profiles A and B in prf identified by the gene symbol.
  # A and B can be either gene symbol or index.
  
  r <- cor(prf[A, ], prf[B, ], use = "pairwise.complete.obs")
  return(r)
}

corGenes("MRPL18", "RPF2", myQNXP)
corGenes(100, 200, myQNXP)

plotCorGenes <- function(A, B, prf) {
  # Plot correlation between gene expression
  # profiles A and B in prf identified by the gene symbol.
  # A and B can be either gene symbol or index.
  
  xMin <- min(c(0, prf[A, ], prf[B, ]), na.rm = TRUE)
  xMax <- max(c(   prf[A, ], prf[B, ]), na.rm = TRUE)
  
  plot(prf[A, ], prf[B, ],
       xlim = c(xMin, xMax),
       ylim = c(xMin, xMax),
       main = sprintf("%s vs. %s (r = %5.2f)", A, B, corGenes(A, B, prf)),
       xlab = (sprintf("%s expresion (AU)", A)),
       ylab = (sprintf("%s expresion (AU)", B)),
       asp = 1.0)
  abline(lm(prf[B, ] ~ prf[A, ]), col = "#AA0000", lwd = 0.67)
  
  return(invisible(NULL))
}

plotCorGenes(A = "SLC9A4", B = "NLGN2", prf = myQNXP)

```
![](./inst/img/lowPairwiseCorrelation.svg?sanitize=true "uncorrelated genes") &nbsp; ![](./inst/img/highPairwiseCorrelation.svg?sanitize=true "highly correlated genes")<br/>
Example plots for uncorrelated and highly correlated gene expression profiles.

&nbsp;

![](./inst/img/QN-GEOprofileCorrelations.svg?sanitize=true "distribution of expression correlations")<br/>
Distribution of expression correlations between 10,000 randomly chosen gene pairs, compared to the distribution of shuffled expression values for the two genes. The correlations are not symmetric around zero. There are more negative correlations than expected, and more excess negative than positive correlations, but if two genes are positively correlated, the correlation trends to be higher.

&nbsp;

#### 2.5 Systems:

This is in progress. Here is a function stub that returns a set of gene symbols for a system name:

```R

fetchComponents <- function(sys) {
  # returns a fixed set of symbols.
  # Function stub for development purposes only.
 if (sys == "PHALY") {
    s <- c("AMBRA1", "ATG14", "ATP2A1", "ATP2A2", "ATP2A3", "BECN1", "BECN2", 
           "BIRC6", "BLOC1S1", "BLOC1S2", "BORCS5", "BORCS6", "BORCS7", 
           "BORCS8", "CACNA1A", "CALCOCO2", "CTTN", "DCTN1", "EPG5", "GABARAP", 
           "GABARAPL1", "GABARAPL2", "HDAC6", "HSPB8", "INPP5E", "IRGM", 
           "KXD1", "LAMP1", "LAMP2", "LAMP3", "LAMP5", "MAP1LC3A", "MAP1LC3B", 
           "MAP1LC3C", "MGRN1", "MYO1C", "MYO6", "NAPA", "NSF", "OPTN", 
           "OSBPL1A", "PI4K2A", "PIK3C3", "PLEKHM1", "PSEN1", "RAB20", "RAB21", 
           "RAB29", "RAB34", "RAB39A", "RAB7A", "RAB7B", "RPTOR", "RUBCN", 
           "RUBCNL", "SNAP29", "SNAP47", "SNAPIN", "SPG11", "STX17", "STX6", 
           "SYT7", "TARDBP", "TFEB", "TGM2", "TIFA", "TMEM175", "TOM1", 
           "TPCN1", "TPCN2", "TPPP", "TXNIP", "UVRAG", "VAMP3", "VAMP7", 
           "VAMP8", "VAPA", "VPS11", "VPS16", "VPS18", "VPS33A", "VPS39", 
           "VPS41", "VTI1B", "YKT6")
 } else {
   s <- ""
 }
  return(s)
}

fetchComponents("PHALY")
#   [1] "AMBRA1"    "ATG14"     "ATP2A1"    "ATP2A2"    "ATP2A3"   
#   [6] "BECN1"     "BECN2"     "BIRC6"     "BLOC1S1"   "BLOC1S2"  
#  [11] "BORCS5"    "BORCS6"    "BORCS7"    "BORCS8"    "CACNA1A"  

fetchComponents("NONSUCH")
#  [1] ""


```

&nbsp;

## 3 Notes

&nbsp;

## 4 References and Further Reading

&nbsp;

## 5 Acknowledgements

&nbsp;

<!-- [END] -->
