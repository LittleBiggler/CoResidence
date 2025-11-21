# CoResidence
## Learning Goals:


This project uses the “CoResidence” dataset, a recent study of household composition, combined with World Bank and United Nations demographic data, to study how household structure relates to economic well-being across countries and time. The methodological aim in this projet is to explore how to responsibly reconstruct missing values, model latent structure, and evaluate predictive signal in a high-dimensional multi-panel dataset with substantial missingness.

CoResidence is my capstone project as an autodidact in data science. My goal was to take a real-world, high-dimensional dataset—preferably complex panel data spanning decades—and treat it as if I had been assigned a forecasting task under realistic constraints.

In other words:

1) Assume the future must be predicted.
2) Assume the dataset must be used as-is.
3) Assume missing values must be filled, not dropped.

In the formal data science courses that I took, .dropna() was almost always the first preprocessing step. And I almost always quietly wondered: What if that weren’t an option? What if we actually had to use the data we were given?

This question is central in many real applications:

1) Econometrics, where demographic or country-level panels are incomplete or unevenly reported
2) Customer and business analytics, where CRM fields, transaction logs, or historical data are often missing or inconsistently collected
3) Operational forecasting, where you cannot discard imperfect inputs simply because they contain gaps

This is my first attempt to “walk down every path” inside a real-world data labyrinth. The forecasting portion of the project is not yet complete, but the data is now ready for it. Top features are robustly identified, forming the foundation for a future forecasting pipeline.

**Project Description**

This analysis examines how household composition relates to national prosperity (Gini Index) across countries and time. The workflow emphasizes:

1) Leakage-aware design (train-only scaling, ground-truth-only testing)
2) Time-series cross-validation
3) Comparative evaluation of imputation strategies
4) Use of TruncatedSVD both for imputation and for dimensionality reduction
5) Back-projection and back-allocation from latent components to native feature space

Of course, I did include a .dropna() baseline model for comparison. It produced reasonable scores, but the resulting design matrix contained far fewer observations and exhibited strong multicollinearity typical of macro-demographic indicators. Even after properly addressing compositional data, remaining features formed an effectively low-rank and ill-conditioned matrix.

**Model Comparison**

Multiple model classes were evaluated, including both linear and nonlinear approaches.

Feature importances for linear models were interpreted through back-projected coefficients. All models, regardless of linearity, were compared using allocated load on permutation importances (row-normalized squares), ensuring consistency across different model types.


## Model Comparison
![Model Comparison](final_score_matrix.jpg)

As you can see, the baseline model (.dropna()) produced some of the best results, in terms of explanatory power (R2). This is likely due to the fact that countries with poor data quality were mostly dropped, by definition. Higher R2 values of the baseline model hid an underlying problem: Unstable matrix. Unstable matrices produce unreliable model results. Tiny changes to any value in an unstable matrix can cause your model to whip around wildly. Thus, dimensionality reduction was needed.



## Conclusion

My analysis looks at feature importances in a number of different ways. Here, I only report permutation importances, and not linear coefficients or Random Forest splitting criteria. Permutation importance measures predictive dependence. 



Hence why I report importances in PI:

For the linear models, the top ten features can be summarized as four main insights. These are the features that really matter:
1) GDP
2) The number of children in small and medium-sized households
3) The number of non-relatives in small and medium sized households
4) The proportion of large (9-person) female-headed households

The narrative that links these four features together is murky, but plausible. In stable countries, medium households predominate. However, if those medium households are not families, but rather adult roommate situations, that does not bode well for wealth equality.

The non-linear models did overlap with the linear models on a number of features, but identified important signals of their own:

![Feature Importances Matrix](top_ten_importances_heatmap_desc.jpg)




![Feature Importance by Sub-Family](feature_rank_swarmplot.jpg)

SVD at its core collapses redundant information into clear signal. In a multicollinear, compositional dataset, many original variables share the same latent directions. Sharing latent directions is redundancy. The SVD feature space is a distillation of the original feature space. 

When coefficients are projected back onto the original features, the purity of the signal gets diffused back to redundant bases.

Permutation importances tell us which of the purified features are indispensable for prediction, once redundancy is taken into account. Thus, PI doesn't just tell us which features align with a certain vector direction in a subspace, they tell us which subspaces are the most indispensible to model performance.

# How to Run:
Conda python env
Don't run the final section "Addendum," because it takes forever, and it's just for fun.


 


