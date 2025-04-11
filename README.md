# CoResidence
Analysis of global household configuration data ~1960-present

A short description of my project is that it consists of panel data cobbled together from a variety of data sources, the features of interest pertaining to household configuration. I am trying to identify the relationship between household configuration and well being, using Gini index and human development index as proxies for well being.

As a result of multiple merges, I have a large number of null values. After culling rows where all informative columns are blank, I still have sparse data. I thus use the project as a methodological study into handling missing values, using three methods: (1) interpolation, (2), iterative imputer, and (3) matrix decomposition methods.

The target was imputed separately from features using KNN and matrix reconstruction.


Tables included:
1) A0_CORESIDENCE_NATIONAL_DATASET_2024.csv
2) A1_CORESIDENCE_SUBNATIONAL_DATASET.csv
3) CODEBOOK_2024.csv
4) API_SI.POV.GINI_DS2_en_csv_V2_3401539/PI_SI.POV.GINI_DS2_en_csv__v2_3401539.csv
5) WPP2024_GEN_F01_DEMOGRAPHIC_INDICATORS_COMPACT.csv
6) HDR23-24_composite_indices_complete_timeseries.csv
7) HDR23-24_composite_indices_metadata.xlsx
