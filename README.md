BC cfDNA Fragmentomics (TuFEst)
This repository contains R/Rmarkdown (Rmd) code for cfDNA fragmentomics feature generation and machine-learning modeling for breast lesion assessment using ultra–low-pass whole-genome sequencing (LP-WGS, ~2×).

The project includes three tasks:
TuFEst: benign vs malignant classification (cancer score, ROC/AUC, confusion matrix)
TuFEst-MS: molecular subtype prediction (ER+/PR+HER2−, HER2+, TNBC)
TuFEst-LN: lymph node metastasis prediction (mLN vs non-mLN)

1. Repository layout
BC_cfDNA_code/
01_AUC/ (TuFEst: benign vs malignant)
21_Stacked_ensemble_v3/(stacked ensemble / SEM-related scripts)
R_10_01_LIQUORICE_*.Rmd
R_10_02_Griffin_*.Rmd
R_10_03_EndMotif_Breakpoint_*.Rmd
R_64_01_Training_Dataset_*.Rmd
R_64_02_Internal_validation_Data_*.Rmd
R_64_03_External_validation_Data_*.Rmd
R_10_01_LIQUORICE_*.Rmd
R_10_02_Griffin_*.Rmd
R_10_03_EndMotif_Breakpoint_*.Rmd
R_64_01_Training_Dataset_*.Rmd
R_64_02_Internal_validation_Dataset_*.Rmd
R_64_03_External_validation_Dataset_*.Rmd
02_Subtype/ (TuFEst-MS: subtype)
R_64_01_*.Rmd 
R_64_02_*.Rmd 
R_10_03_feature_boxplot.Rmd 
03_LN_M_nonM/(TuFEst-LN: lymph node)
R_02_AUC_01_Training_Dataset_*.Rmd 
R_02_AUC_02_Internal_Validation_*.Rmd 
R_10_03_EndMotif_Breakpoint_*.Rmd
04_chipseq
bed_read_cov.Rmd
05_fragment_profile
*_fragmentation_profiles.Rmd
BC_Model/
	01_GLM
01_prediction_results.xlsx
02_prediction_results.xlsx
03_prediction_results.xlsx
BC_01_GLM_TD_IVD.ipynb
BC_01_GLM_Validation.ipynb
logistic_regression.pkl
02_SVM
01_prediction_results.xlsx
02_prediction_results.xlsx
03_prediction_results.xlsx
BC_02_SVM_TD_IVD.ipynb
BC_02_SVM_Validation.ipynb
SVM.pkl
03_XGBoost
01_prediction_results.xlsx
02_prediction_results.xlsx
03_prediction_results.xlsx
BC_03_XGBoost_TD_IVD.ipynb
BC_03_XGBoost_Validation.ipynb
XGBClassifier.pkl
	04_MLP
01_prediction_results.xlsx
02_prediction_results.xlsx
03_prediction_results.xlsx
BC_04_MLP_TD_IVD.ipynb
BC_04_MLP_Validation.ipynb
MLPClassifier_pipeline.pkl
	05_GBM
01_prediction_results.xlsx
02_prediction_results.xlsx
03_prediction_results.xlsx
BC_05_GBM_TD_IVE.ipynb
BC_05_GBM_Validation.ipynb
GradientBoostingClassifier.pkl
	06_SEM
01_prediction_results.xlsx
02_prediction_results.xlsx
03_prediction_results.xlsx
BC_06_SEM_TD_VD.ipynb
BC_06_SEM_Validation.ipynb
GradientBoostingClassifier.pkl
MLPClassifier_pipeline.pkl
SVM.pkl
XGBClassifier.pkl
logistic_regression.pkl
stacked_model.pkl

2. Requirements
2.1 R environment
R: recommended to match the analysis environment used in the study (e.g., R 4.4.1/4.4.3)
R packages (final list depends on each Rmd):
tidyverse	2.0.0
ROCR	1.0.11
pROC   1.18.5
caret    7.0.1
ggplot2	3.5.1
RColorBrewer	1.1.3
ggbeeswarm	0.7.2
pacman	0.5.1
ggpubr	0.6.0
rstatix	0.7.2
ggsci	3.2.0
ggsignif	0.6.4
reshape2	1.1.4
openxlsx	4.2.8
dplyr	1.1.4
stringr	1.5.1
pheatmap	1.0.12
devtools	2.4.5
cowplot	1.1.3

2.2 Optional preprocessing tools (if starting from FASTQ)
If you reproduce features from raw sequencing files, prepare the standard WGS toolchain:
QC/trimming: FastQC, Cutadapt, Ktrim
Alignment/post-processing: BWA-MEM, Samtools, Picard
Coverage/signal utilities: deepTools

3. Methods
3.1 Fragmentomic features
Typical feature groups implemented in the analysis include:
End motifs (e.g., 4-mer / 6-mer end-motif frequencies)
Fragment size patterns (e.g., short/long ratio; binned genome-level signals)
Repeat-associated features (e.g., LINE/SINE/LTR/satellite-related categories)
TF binding site coverage signals (coverage/fragmentation over TF peak regions, if applicable)

3.2 Machine learning
GLM, SVM, XGBoost, MLP, GBM, and stacked ensemble (SEM)
Cross-validation within training and fixed validation splits (internal/external)
Outputs: ROC/AUC, confusion matrix, and per-sample probability score (0-1)

4. How to run
4.1 Recommended (RStudio)
Open the target .Rmd file and run chunks sequentially, or Knit to generate a report.

4.2 Command-line rendering (optional)
Use rmarkdown::render() to render a specific Rmd file (adjust the path to your local file name).

5. Outputs
Suggested output organization (adapt to your local settings):
results/auc/: ROC curves, AUC tables, threshold metrics, confusion matrices, per-sample scores
results/subtype/: subtype predictions, AUC summaries, feature plots
results/ln/: LN predictions, ROC/AUC, PPV/NPV summaries, stratified analyses
results/features/: exported feature matrices for reuse

6. Reproducibility
1.Keep fixed split definitions (train/internal/external) and avoid re-splitting during execution.
2.Set and record random seeds for model training.
3.Record R session information (sessionInfo()) and package versions for each run.
4.Store trained models and intermediate objects in BC_Model/ when needed.
