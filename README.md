# CoResidence
## Project Description:

This project analyzes the [“CoResidence Database”](https://zenodo.org/records/8142652), a recent study of household composition. Data from the World Bank and United Nations is joined to the database for improved data quality. 

**The Question**: How does household configuration relate to economic well-being (Gini Index) across countries and time? 

CoResidence is my capstone project as an autodidact in data science. My goal was to take a real-world, high-dimensional dataset and treat it as if I had been assigned a forecasting task under realistic constraints.

In other words:

1) Assume the future must be predicted.
2) Assume missing values must be filled, not dropped.

In the formal data science courses that I took, .dropna() was almost always the first preprocessing step. And I usually wondered: What if line deletion weren’t an option? What if we actually had to use the data we were given?

This question is central in many real applications:

1) Econometrics, where demographic or country-level panels are incomplete or unevenly reported
2) Customer and business analytics, where CRM fields, transaction logs, or historical data are often missing or inconsistently collected
3) Operational forecasting, where you cannot discard imperfect inputs simply because they contain gaps


