# CoResidence
## Project Description:

This analysis examines how household composition relates to national prosperity across countries and time, and prepares a panel for forecasting future Gini values. Methods emphasize leakage-aware design (train-only scaling), time-series cross-validation, clustered confidence intervals, and a comparative study of imputation strategies (interpolation, Iterative Imputer, Truncated SVD).


Tables included:
1) A0_CORESIDENCE_NATIONAL_DATASET_2024.csv
2) A1_CORESIDENCE_SUBNATIONAL_DATASET.csv (not used but included for reference)
3) CODEBOOK_2024.csv
4) API_SI.POV.GINI_DS2_en_csv_V2_3401539/PI_SI.POV.GINI_DS2_en_csv__v2_3401539.csv
5) WPP2024_GEN_F01_DEMOGRAPHIC_INDICATORS_COMPACT.csv
6) HDR23-24_composite_indices_complete_timeseries.csv
7) HDR23-24_composite_indices_metadata.xlsx

## Model Comparison
![Model Comparison](final_score_matrix.jpg)



## Conclusion
![Feature Importances Matrix](top_ten_importances_heatmap_desc.jpg)

We began this analysis with the question, "What types of household configurations best predict happy lives?"

Gini Index, which is a measure of wealth inequality, was selected as a proxy for happiness. Generally speaking, countries with extremely high Gini values are unhappy places, plagued by corruption, violence, and instability. However, countries that report equitable distribution of meager resources—a small pie sliced evenly—are also not necessarily happy places. Therefore, additional nuance is required to improve our analysis.

Feature Importances results showed a surprisingly crisp separation between features deemed important via linear models, versus non-linear models. This suggests that the dataset contains both linear and nonlinear signal. The tree model, interestingly, picks up some features that overlap with other models, but identifies its own set of salient features. 


