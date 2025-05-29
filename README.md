# CoResidence
## Project Description:

Household formation is generally recognized as an important driver of economic growth, prosperity, and human flourisihing. But what does it mean to be a household? 

Households composition varies, ranging from nuclear families, to roommates arrangements, to extended kinship situations, etc. This analysis examines household composition, and the relationship of household composition to markers of prosperity, by country and over time. 

In essence, this project aims to answer the question, "What types of household configurations predict happy lives?"

A number of proxies for "happy" are considered. Initially, HDI (Human Development Index) is chosen as a proxy, but this caused indirect data leakage with features that constitute the index. To mitigate this, Gini Index, was imported from outside the dataset, as an alternative target. This preserved key features while stopping the leakage.

The primary dataset in this analysis, CoResidence, was compiled by researchers at the University of Barcelona. It was high dimensional, and once merged with Gini values, also sparse. Sparsity and high dimensionality presented challenges to handling missing values without significant information loss. 

Reconstructing incomplete data can increase bias, noise, and multicollinearity. To address this, I deploy three different strategies for data imputation: 
1) Interpolation
2) Iterative Imputation
3) Matrix Decomposition

All imputation methods are tested against all models, and feature importances are compared among best performing model-dataset combinations. 


Tables included:
1) A0_CORESIDENCE_NATIONAL_DATASET_2024.csv
2) A1_CORESIDENCE_SUBNATIONAL_DATASET.csv
3) CODEBOOK_2024.csv
4) API_SI.POV.GINI_DS2_en_csv_V2_3401539/PI_SI.POV.GINI_DS2_en_csv__v2_3401539.csv
5) WPP2024_GEN_F01_DEMOGRAPHIC_INDICATORS_COMPACT.csv
6) HDR23-24_composite_indices_complete_timeseries.csv
7) HDR23-24_composite_indices_metadata.xlsx

## Model Comparison
![Model Comparison](final_score_matrix.jpg)
Best overall R2 scores—with stable matrices—were achieved with SVD 40 and 60 singular values, paired with KNN-imputed targets. Initial optimal values for n_components were chosen using singular value decay and explained variance ration (pictured above). A higher value of 125 was also tested for further validation. Non-linear models performed best, specifically KNN, SVR, and Random Forest. Linear models also achieve acceptable performance.

Condition Matrix stability was determined through matrix condition number on XTX (X_train.T @ X_train) for all models except SVR. Because SVR is a kernel model, XXT (X_train @ X_train.T) was used to gauge matrix stability. Matrix condition number is less relevant for distance-based and tree models, as their loss functions do not involve inverting matrices. However, matrix stability speaks to overall health of the dataset. Collinearity can confuse distance-based models, and weak feature variance can stump tree-based models.

Metrics Hyperparameters such as Lasso and Ridge alpha, and KNN components, were tuned using MSE as a comparison metric, rather than R-Squared. This approach was intentional: R-Squared is a mediated metric, which depends not only upon the squared residuals (model error), but also the relationship of those residuals to the total variance of the target. Mean squared error provides a cleaner comparison metric. That said, R-Squared was used as the final metric for model comparison, because it ultimately captures what we really care about, which is, How well does the model explain the target variance?

Scaling Scaling for SVD-imputed data was applied prior to matrix factorization itself, and not applied within the modelling loop (tscv_loop()). All other datasets were scaled using X_train to fit the scaler, with the fitted scaler then applied to both X_train and X_test seperately. Columns that constituted simplex groupings, and were subjected to central log ratio transformation, were excluded from all scaling.

Target The KNN target was imputed with variable n_components values, tailored to data availability per country.

Time Only Fold 3 was useful, as scores tended to improve across folds. Examining all three time series folds allowed us to be ready to detect time-dependent changes in data quality. Patterms indicate that the datasets had better data in later years, which aligns with prior expections. There exists the potential to enhance model performance using the sample_weight parameter inside of the models, given that more recent data is more complete.

## Conclusion
![Feature Importances Matrix](top_ten_importances_heatmap_desc.jpg)

We began this analysis with the question, "What types of household configurations best predict happy lives?"

Gini Index, which is a measure of wealth inequality, was selected as a proxy for happiness. Generally speaking, countries with extremely high Gini values are unhappy places, plagued by corruption, violence, and instability. However, countries that report equitable distribution of meager resources—a small pie sliced evenly—would not be happy places. Therefore, additional nuance is required to improve our analysis. 

**Most Important Predictors**
*Assuming that Gini Index is a measure of human happiness* (no matter how flawed an assumption this is), the data has spokedn very clearly: Feature Importances results are remarkably consistent across both linear and non-linear models. 
- The single most important indicator of high Gini values is the **Proportion of 10 person households of female-headed households**. 
- The second most important indication of high **Proportion of 10 person households of male-headded households**. All remaining features pertain to whether household relationships are nuclear or non-nuclear.

A surprising finding was that residual categories appeared in the final top ten. For example: 
- **Average size of households with nuclear, other family, and non family relationships to head**
- **Proportion of households with nuclear and other family relationships to head** 
These are both residual categories. Residual categories are typically a catch-all. In other words, residue is usually an afterthought, but here conceptual residue turned out to be very important.

**Interpretation**
Roughly speaking, **high Gini values are bad, and low Gini values are good**. It makes good intuitive sense that having a lot of households packed full of a lot people, who may or may not be related to each other, is probably not great for human happiness. While loneliness is a hot topic these days in developed countries, lack of privacy and overcrowding seems to be more of a concern globally. 

Interestingly, features like **Proportion of uninipersonal households of female-headed households** and **Proportion of 1-person households of female-headed households** also appear prominently, suggesting that a lot of housholds consisting of women living alone is also a bad sign.

Many of our feature importances lack directionality, because tree models and KNN models do not yield positive or negative coefficients. In many cases we do not know whether the highlighted features predict high or low Gini values, but we can analyze the results with storytelling. Plotting the features would also be useful. However, the most successful datasets were outputs of Singular Value Decomposition, which means we would need to reconstruct the native feature space in order to even begin to visualize these relationships.

This analysis has succeeded in answering our original question, except inverted. We know what features most predict **unhappiness**, i.e. high Gini Index values. We would need to invert the Gini Index number (by subtracting all of the Gini values from 1), in order to predict which features pertain to low inequality societies. In theory, strong predictors should be similar but with inverted directionality, but many factors could influence differential outcomes. Additional work needs to be done to seek deeper answers. 

Other Considerations:

**Imputation Bias**
Imputation of large swaths of missing data was not fruitful for this analysis. The onerous and large scale imputations of sparse and high dimensional data was far outperformed by the output of Singular Value Decomposition, at the expense of interpretability and ease of visualization. 

**Simplexes**
Many of the features highlighted in the top ten feature matrix appear to be duplicates, however they are not truly duplicates. We conducted an extensive duplicate analysis in the Data Validation section of this project. Apparent duplicates are due to collapsed or comprehensive simplexes. For example: 
- Features HT01:HT09 constitute a simplex. 
- Features HT20:HT25 constitute a simplex. 

While HT01 & HT20, and HTO2 & HT21 appear to be duplicates by their description names, they consist of different values. In the first simplex, more proportion categories are included to describe the same data.

Simplexes complicate modelling because they consist of linearly dependent columns by definition, which break design matrices for most models. We treated simplexes by subjecting them to Central Log Ratio transformations, which converts the interrelatedness into Euclidean geometry. Given that these simplexes also consist of very similar values across groups, it may improve model performance to select one or the other simplex, rather than both.

Here are the next steps in further analysis:
1) Invert Gini Index values and run parallel analysis to analyze which features best predict low values
2) Reconstruct native feature space by backwards-engineering X from SVD matrices and plot resulting features
3) Created a weighted Gini Index that blends HDI (Human Development Index) with Gini values
4) Retry modelling with similar simplexes excluded

