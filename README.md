# CoResidence
## Project Description:

This project uses the “CoResidence” dataset, a recent study of household composition, combined with World Bank and United Nations demographic data, to study how household structure relates to economic well-being across countries and time. 

CoResidence is my capstone project as an autodidact in data science. My goal was to take a real-world, high-dimensional dataset—preferably complex panel data spanning decades—and treat it as if I had been assigned a forecasting task under realistic constraints.

In other words:

1) Assume the future must be predicted.
2) Assume missing values must be filled, not dropped.

In the formal data science courses that I took, .dropna() was almost always the first preprocessing step. And I almost always wondered: What if that weren’t an option? What if we actually had to use the data we were given?

This question is central in many real applications:

1) Econometrics, where demographic or country-level panels are incomplete or unevenly reported
2) Customer and business analytics, where CRM fields, transaction logs, or historical data are often missing or inconsistently collected
3) Operational forecasting, where you cannot discard imperfect inputs simply because they contain gaps

This is my first attempt to “walk down every path” inside a real-world data labyrinth. The forecasting portion of the project is not yet complete, but the data is now ready for it. Top features are robustly identified, forming the foundation for a future forecasting pipeline.

**Workflow Highlights**

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

As you can see, the baseline model (.dropna()) produced some of the best results, in terms of explanatory power (R2). This is likely due to the fact that countries with poor data quality were mostly dropped, by definition. Higher R2 values of the baseline model hid an underlying problem: Unstable matrix. Unstable matrices produce unreliable model results. Tiny changes to any value in an unstable matrix can cause your model to whip around wildly. Dimensionality reduction and imputation was needed to stabilize the matrix.



## Conclusion

A complex dataset requires a complex conclusion. In the interest of simplicity, I distill the following insights. If you want to anticipate the future of wealth inequality in a given nation, you must consider the following two insights:
1) GDP is important to wealth inequality in a linear way
2) Pay attention the the number of children, spouses, and non-relatives in medium sized households

Models produced encouraging results, in terms of showing the importance of household configuration features, to outcomes of wealth inequality. One fear when starting this project was that population and demographics features would dominate results, essentially showing that household configuration was irrelevant. While GDP was the number one feature for linear signal, that was the only such feature. All other top features pertained to household configuration. 

Linear and Non-Linear Overlap:
- Non-relatives in 3-5 person households
- Non-relatives in male-headed households
- Proportion of 4-5 person households

Linear Only:
- GDP (number one for all three models)
- The number of children, or the number of non-relatives, in small and medium sized households (2-5 persons)
- The proportion of large (9-person) female-headed households

Non-Linear Only:
SVR lacked any direct overlap with KNN and RF. 


**SVR**: 
- Household size,
- Number of children in the household
- Fact that those children belonged to the head


**KNN and RF:**
- The number of non-relatives
- The number of spouses, in small and medium sized households
- Proportion of male-headed households


Note:
My analysis looks at feature importances in a number of different ways. Here, I only report permutation importances, and not linear coefficients or Random Forest splitting criteria. Permutation importance measures predictive dependence. 

Permutation importances tell us which of the purified features are indispensable for prediction, once redundancy is taken into account. Thus, PI doesn't just tell us which features align with a certain vector direction in a subspace, they tell us which subspaces are the most indispensible to model performance.



Linear-Only Heatmap:

![Feature Importances Matrix](top_ten_importances_heatmap_desc.jpg)


**Principal Componenent Composition**

![Feature Importance by Sub-Family](feature_rank_swarmplot.jpg)



# How to Run:
Conda python env
Don't run the final section "Addendum," because it takes forever, and it's just for fun.


 


