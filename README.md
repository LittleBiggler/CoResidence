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

Best overall 𝑅2 scores, with stable matrices, were achieved with imputation via TruncatedSVD, specifically with 40 and 60 singular values, paired with KNN-imputed targets. SVD-imputed targets paired with the aforementioned produced comparable performance. Importantly, models trained on ground truth target values, and tested on imputed values only, performed just as well as models trained and tested on all values.

Initial optimal values for SVD n_components hyperparameter were chosen using singular value decay and explained variance ratio. A higher value of 125 was also tested for further validation, with negligible gains in performance. Non-linear models performed best, specifically KNN, SVR, and Random Forest. Linear models also achieve acceptable results.

Feature Importances (shown in following section) are generated using SVD40 and SVD60 imputed datasets with SVD-imputed target. I use the SVD imputed target, because KNeighborsRegressor turned out to be the best performing model, which sparked concerned about circular bias introduced into the target via KNN imputation, even though targets were imputed independently from features. Reassuringly, the SVD imputed targets performed equally well. (Not shown here, I cross-checked the feature importances pipeline on the KNN imputed target with similar results.)

Matrix stability was non-negotiable for assessing performance. An unstable matrix can produce ostensibly good model results, but model predictions on unstable data are subject to wild swings, and are therefore impractical for any real application. Custom imputation methods also produced stable matrices without the information loss associated with dimensionality reduction. However, model scores were far worse across all target variations on iterative imputer data.

Scores on "exploratory" dataset should be largely ignored, as this dataset uses HDI as a target, rather than Gini. I include the scores here to demonstrate what types of signal and structure were present in line-deleted data. Matrices were unstable even for the line-deleted version of the dataset, demonstrating that sophisticated imputation methods materially improved the explanatory value of the dataset.

## Conclusion
![Feature Importances Matrix](top_ten_importances_heatmap_desc.jpg)

We began this analysis with the question, "What types of household configurations best predict happy lives?"

Gini Index, which is a measure of wealth inequality, was selected as a proxy for happiness. Generally speaking, countries with extremely high Gini values are unhappy places, plagued by corruption, violence, and instability. However, countries that report equitable distribution of meager resources—a small pie sliced evenly—are also not necessarily happy places. Therefore, additional nuance is required to improve our analysis.

Feature Importances results showed a surprisingly crisp separation between features deemed important via linear models, versus non-linear models. This suggests that the dataset contains both linear and nonlinear signal. The tree model, interestingly, picks up some features that overlap with other models, but identifies its own set of salient features. 

**Most Important Predictors**

The most predictive household configuration features of low Gini values fell into three categories:
1) Age of household inhabitants
2) Size of household
3) Head of household

**Linear Signal**
In linear models (such as linear regression, ridge, and lasso), the age of household inhabitants comes up frequently. The number of children in the household is particularly salient, taking the top spot. "Number of children in *x*-person household" shows up repeatedly. "Avg number of 80+ individuals" appears in two out of three models as well.

As for demographic features, population and gross domestic income show up in all of the linear models, but not the non-linear models. 

**Non-Linear Signal**
In non-linear models (random forest, support vector regression), features related to household size proportions (e.g., proportion of *x*-person households) were more influential. Several features involving the gender of household head also ranked highly.

Tree-based models, predictably, identified a mix of both linear and non-linear signals.

Other Considerations:

**Imputation Bias**
Imputation of large swaths of missing data with iterative imputer was not especially fruitful for this analysis. Singular Value Decomposition outperformed iterative imputer, unfortunately at the expense of interpretability and ease of visualization. Heuristic models still had usable performance on iteratively imputed data, however, and this data could be used to audit visualizations and forecasts made with SVD-reconstructed data.

**Simplexes**
As previously noted, this dataset contains a number of simplexes. Originally I did not scale the simplexes, after subjecting them to central log ratio transformation. CLR transformation converts the ratio geometry of a simplex into euclidean distance, and scaling may dilute the relationships qua distance. However, mean simplex variance was around 5, which is 5x the variance of scaled features. Thus, exempting simplexes from scaling had resulted in feature importances that consisted exclusively of simplex columns, which was not a realistic outcome. Overall model performance was unaffected.

**Forecasting**
My approach to missing values in this project is motivated by the desire for data preservation, and conversely, avoiding data loss. Another motivation, however, is the desire to create forecasting models. Forecasting models require data to be populated across regular time intervals. Thus, our dataset is ready for the next phase of this project, which involves forecasting the most salient features and target. 

**Next Steps**


1) Reconstruct native feature space by backwards-engineering X from SVD matrices and plot resulting features
2) Use visualization techniques to determine directionality of importances
3) Created a weighted Gini Index that weights Gini values by per captita GDP
4) Use the gapless time series data created by imputation to create a forecasting model
