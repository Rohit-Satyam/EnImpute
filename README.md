This is improved documentation and update code of **EnImpute**. I hold no guarentee of the accuracy of changes I made until the pull request is approved by the authors.

**EnImpute paper** is published now and should be cited as follows if used:
>Zhang, X. F., Ou-Yang, L., Yang, S., Zhao, X. M., Hu, X., & Yan, H. (2019). EnImpute: imputing dropout events in single-cell RNA-sequencing data via ensemble learning. _Bioinformatics_, _35_(22), 4827-4829.


Contents of this archive
------------------------
(1) `pkg:` subdirectory that contains the R package.
(2) `down_sampling_experiments:` subdirectory that contains scripts for performing down-sampling experiments.
(3) `shiny:` subdirectory that contains the Shiny application.
(4) `EnImpute-manual.pdf:` reference manual.


The EnImpute package has the following R-package dependencies: 
- [DrImpute](https://github.com/ikwak2/DrImpute)
- [Rmagic](https://github.com/cran/Rmagic)
- [rsvd](https://cran.r-project.org/web/packages/rsvd/index.html)
- [SAVER](https://github.com/mohuangx/SAVER)
- [Seurat](https://satijalab.org/seurat/)
- [scImpute](https://github.com/Vivianstats/scImpute)
- [scRMD](https://github.com/XiDsLab/scRMD)
- [stats](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/stats-package.html). 
The dependencies will be automatically installed along with EnImpute. If the dependencies are not installed correctly, please install them by yourself. You can install the dependencies using the following commands:

```r
install.packages(c("DrImpute","rsvd","Seurat","stats","PMA","devtools"))
devtools::install_github("mohuangx/SAVER")
devtools::install_github("Vivianstats/scImpute")       
devtools::install_github("ChongC1990/scRMD")
## Adding these to install an old version of Seurat to access AddImputeScore function used by EnImpute
devtools::install_github("statsbomb/SDMTools")
## If you have New Version of Seurat(4.2) you need to downgrade it to version 2. I tried using two different Seurat version using code commented below but it failed

## To Downgrade
source("https://z.umn.edu/archived-seurat")
remotes::install_version(package = 'Seurat', version = package_version('2.3.4'))
# Creating a subdirectory in ErImpute folder
# First clone the SeuratForMayer2018 repo
#dir.create("SeuratForMayerv2")
#install.packages("SeuratForMayer2018/",repos = NULL, type = "source",lib="SeuratForMayerv2")
#library(Seurat, lib.loc = "SeuratForMayerv2/")

#You can use the following commands to install EnImpute from GitHub.
install_github("Rohit-Satyam/EnImpute", subdir="pkg")
```
**Note:** EnImpute also depends on the following two `Python` packages: `dca` (https://github.com/theislab/dca) and `MAGIC` (https://github.com/KrishnaswamyLab/MAGIC). Before installing EnImpute,  please install the two Python packages following the corresponding readme files, and check whether they can be run from the command line.

```bash
mamba create -n enimpute python==3.8
conda activate enimpute 
~/miniconda3/envs/enimpute/bin/pip3 install dca 'keras>=2.4,<2.6' 'tensorflow>=2.0,<2.5' pyyaml==5.4.1 magic-impute
```

## Usage

Load the library EnImpute in R console, by running
```r
# Load reticulate first and set python path
library(reticulate)
use_python("/home/subudhak/miniconda3/envs/enimpute/bin/python3")
library("EnImpute")
```
Simply run the function EnImpute on your favorite datasets. Take the baron dataset as an example:
```r
data("baron")
# Since AddImputedScore Function was depreciated and replaced with ALRA, there is no point in using it. Moreover, with old Seurat Version (2.3.4), creation of Seurat Object throws an error.
baron_imputation_result = EnImpute(baron$count.samp, dcaPath = "/home/subudhak/miniconda3/envs/enimpute/bin/dca", Seurat = FALSE)
```
For detailed usage, please refer to **"EnImpute-manual.pdf"**. For more examples about real data applications, please refer to the file **"down_sampling_analysis.R"**, which performs the down-sampling experiments as described in the paper and is present in the subdirectory **"down_sampling_experiments"**.

Please do not hesitate to contact *Dr. Xiao-Fei Zhang* at *zhangxf@mail.ccnu.edu.cn* to  seek any clarifications regarding any contents or operation of the archive.
