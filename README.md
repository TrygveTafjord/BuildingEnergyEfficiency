# Bayesian Regression Analysis of Building Energy Efficiency

## Project Overview

[cite_start]This project is the final assignment for the "Bayesian Learning and Monte Carlo Simulations" course (2025)[cite: 1]. [cite_start]The objective is to analyze the Energy Efficiency dataset to predict building energy loads[cite: 136, 143, 154]. We employ Bayesian linear regression techniques to understand the relationships between building design parameters and energy consumption, specifically focusing on the **Heating Load**. The R code confirms the focus on "Heating_Load" as the response variable.

[cite_start]The analysis includes a comprehensive exploratory data analysis (EDA), data preprocessing, and the implementation of several Bayesian models[cite: 20]. We compare a model with a conjugate prior, models developed using Bayesian Model Averaging (BMA) with various prior specifications, and a model using an MCMC sampler. [cite_start]The project emphasizes model selection, sensitivity analysis, prediction, and out-of-sample validation to identify the most significant predictors of heating load[cite: 12, 14, 17, 156].

This project was developed by Moein-Taherinezhad and Trygve-Tafjord, as stated in the author metadata of the R Markdown files.

---

## Dataset

[cite_start]The analysis is performed on the **Energy Efficiency Dataset**[cite: 136].

* [cite_start]**Source**: The dataset was simulated in Ecotect [cite: 138] [cite_start]and is publicly available on Kaggle[cite: 137].
* [cite_start]**Description**: It contains 768 samples and 8 feature variables related to building design, and two response variables (Heating Load and Cooling Load)[cite: 141, 142]. The R code in `DataAnalysisCode.Rmd` confirms there are 768 observations. [cite_start]The goal is to predict the energy loads based on the building's characteristics[cite: 143].

### Features and Response Variables

* [cite_start]**X1**: Relative Compactness [cite: 144]
* [cite_start]**X2**: Surface Area [cite: 145]
* [cite_start]**X3**: Wall Area [cite: 146]
* [cite_start]**X4**: Roof Area [cite: 147]
* [cite_start]**X5**: Overall Height [cite: 148]
* [cite_start]**X6**: Orientation (Categorical) [cite: 149]
* [cite_start]**X7**: Glazing Area [cite: 150]
* [cite_start]**X8**: Glazing Area Distribution (Categorical) [cite: 151]
* [cite_start]**y1**: **Heating Load** (Response Variable Used in this Analysis) [cite: 152]
* [cite_start]**y2**: Cooling Load [cite: 153]

---

## Files in this Project

[cite_start]This project is organized into three R Markdown (`.Rmd`) files, each covering a distinct part of the analysis[cite: 20].

1.  `DataAnalysisCode.Rmd`
    * **Purpose**: Initial Exploratory Data Analysis (EDA).
    * **Contents**: This file loads the data, renames columns for clarity, and performs a thorough visual and statistical exploration. It includes histograms to check distributions, skewness analysis, boxplots to identify scale imbalances, and a correlation matrix to uncover relationships between variables. Key findings from this file include the bimodal nature of the response variables and a strong correlation between predictors like `Relative_Compactness` and `Surface_Area`.

2.  `Conjugate_prior.Rmd`
    * **Purpose**: Implementation and evaluation of a Bayesian linear model with a conjugate prior.
    * **Contents**: This file preprocesses the data (log transforms, one-hot encoding, scaling) and splits it into training (80%) and testing (20%) sets. It then analytically derives the posterior distributions for the regression coefficients ($\beta$) and variance ($\sigma^2$) using a Normal-Inverse-Gamma conjugate prior. The model's predictive performance is evaluated using Mean Squared Error (MSE) and predictive interval coverage.
    * **Note**: The file concludes that this model "performs poorly, this is most likely due to the removal of the two features Surface_Area and Roof_Area".

3.  `BAS_analysis.Rmd`
    * **Purpose**: Advanced Bayesian modeling using the `BAS` R package for model selection and averaging.
    * **Contents**: This is the core analysis file. It uses the same preprocessing steps and then applies Bayesian Model Averaging (BMA) to handle model uncertainty.
        * **Model Selection**: Identifies the Highest Posterior Model (HPM) and compares its performance against a full model and the BMA estimate.
        * **Sensitivity Analysis**: Evaluates model robustness by testing several different priors on the coefficients (`g-prior`, `JZS`, `hyper-g-n`, `EB-local`, `AIC`, `BIC`).
        * **MCMC Methods**: Implements a Bayesian linear model using a Markov Chain Monte Carlo (MCMC) sampler for a more thorough exploration of the posterior space.
        * **Evaluation**: All models are assessed based on out-of-sample MSE and the percentage of test points falling outside the 95% predictive interval.

---

## Methodology Workflow

1.  **Data Preprocessing**:
    * Log-transformed skewed features (`Relative_Compactness`, `Wall_Area`) to better approximate normality. This is performed by the `log_transform_features` function in `Conjugate_prior.Rmd` and `BAS_analysis.Rmd`.
    * Converted categorical features (`Orientation`, `Glazing_Area_Distribution`) into numerical indicators using one-hot encoding, implemented via the `encode_categorical_vars` function in `Conjugate_prior.Rmd` and `BAS_analysis.Rmd`.
    * Split the data into an 80% training set and a 20% test set, as shown in the code of `Conjugate_prior.Rmd` and `BAS_analysis.Rmd`.
    * Scaled numerical features using the mean and standard deviation of the training set to prevent data leakage. The `scale()` function is used for this purpose in `Conjugate_prior.Rmd` and `BAS_analysis.Rmd`.

2.  **Modeling and Analysis**:
    * **Conjugate Model**: An initial attempt using a conjugate Normal-Inverse-Gamma prior is detailed in `Conjugate_prior.Rmd`.
    * **Bayesian Model Averaging (BMA)**: The primary analysis method, using the `BAS` package to account for uncertainty in model selection, is described in `BAS_analysis.Rmd`.
    * **MCMC Sampling**: Used as an alternative sampling method within the `BAS` package, as shown in the latter parts of `BAS_analysis.Rmd`.

3.  **Evaluation**:
    * **Model Performance**: Measured using Mean Squared Error (MSE) on the held-out test data, as calculated in both `Conjugate_prior.Rmd` and `BAS_analysis.Rmd`.
    * **Prediction Uncertainty**: Assessed by calculating 95% predictive intervals and checking the coverage on the test set in both modeling files.
    * **Variable Importance**: Determined by examining posterior inclusion probabilities (`probne0`) and confidence intervals for the regression coefficients in `BAS_analysis.Rmd`.

---

## How to Run the Code

To replicate this analysis, you will need **R** and **RStudio**.

1.  **Download the project files**, including the dataset `Energy_Efficiency.csv` and the three `.Rmd` files.
2.  **Set the working directory** in R/RStudio to the folder containing these files.
3.  **Install the required R packages** by running the following command in the R console. The list of packages is derived from the `library()` calls in the provided R Markdown files.
    ```R
    install.packages(c("tidyverse", "dplyr", "ggplot2", "ggthemes", "knitr", "corrplot", "BAS", "bayesplot", "conflicted"))
    ```
4.  **Open each `.Rmd` file** in RStudio (`DataAnalysisCode.Rmd`, `Conjugate_prior.Rmd`, `BAS_analysis.Rmd`).
5.  [cite_start]**Click the "Knit" button** in RStudio to execute the code and generate the corresponding HTML report for each file[cite: 20]. It is recommended to run the files in the order listed above.
