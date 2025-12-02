## PREDICTbreastML R package 
PREDICTbreastML is an advanced machine-learning implementation of the PREDICT breast cancer prognostic framework, developed to provide accurate, equitable, and clinically interpretable survival predictions for women with early-stage breast cancer. The package is trained and tested on the largest and most racially and ethnically diverse SEER cohort to date (>800,000 patients) and incorporates population reweighting, treatment-effect modeling, and modern survival-learning algorithms to improve calibration and fairness across diverse groups.

Using a comprehensive set of demographic, tumor, and treatment variables, PREDICTbreastML estimates 10-year breast cancer–specific mortality, and provides individualized projections of chemotherapy benefit. Our evaluation shows that population-level recalibration, sample reweighting, and treatment-effect adjustment yield substantial improvements in accuracy and subgroup performance across racial and ethnic groups.

### Installation

``` r
install.packages("devtools")
library(devtools)
install_github("pengpclab/PREDICTbreastML")
library(PREDICTbreastML)
```

### Example code
``` r
data(toyData)
PREDICTbreastML(ext_val_data = toyData,censor = 10)
```
