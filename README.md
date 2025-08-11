# Bayesian Regression Analysis of Building Energy Efficiency

## Project Overview

This project is the final assignment for the "Bayesian Learning and Monte Carlo Simulations" course (2025). The objective is to analyze the Energy Efficiency dataset to predict building energy loads. We employ Bayesian linear regression techniques to understand the relationships between building design parameters and energy consumption, specifically focusing on the **Heating Load**. The R code confirms the focus on "Heating_Load" as the response variable.

The analysis includes a comprehensive exploratory data analysis (EDA), data preprocessing, and the implementation of several Bayesian models. We compare a model with a conjugate prior, models developed using Bayesian Model Averaging (BMA) with various prior specifications, and a model using an MCMC sampler. The project emphasizes model selection, sensitivity analysis, prediction, and out-of-sample validation to identify the most significant predictors of heating load.

---

## Dataset

The analysis is performed on the **Energy Efficiency Dataset**.

* **Source**: The dataset was simulated in Ecotect and is publicly available on Kaggle.
* **Description**: It contains 768 samples and 8 feature variables related to building design, and two response variables (Heating Load and Cooling Load). The R code in `DataAnalysisCode.Rmd` confirms there are 768 observations. The goal is to predict the energy loads based on the building's characteristics.

### Features and Response Variables

* **X1**: Relative Compactness
* **X2**: Surface Area
* **X3**: Wall Area 
* **X4**: Roof Area 
* **X5**: Overall Height 
* **X6**: Orientation (Categorical)
* **X7**: Glazing Area 
* **X8**: Glazing Area Distribution (Categorical) 
* **y1**: **Heating Load** (Response Variable Used in this Analysis) 
* **y2**: Cooling Load 

---

## Files in this Project

This project is organized into three R Markdown (`.Rmd`) files, each covering a distinct part of the analysis.

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

## How to Run the Code

To replicate this analysis, you will need **R** and **RStudio**.

1.  **Download the project files**, including the dataset `Energy_Efficiency.csv` and the three `.Rmd` files.
2.  **Set the working directory** in R/RStudio to the folder containing these files.
3.  **Install the required R packages** by running the following command in the R console. The list of packages is derived from the `library()` calls in the provided R Markdown files.
    ```R
    install.packages(c("tidyverse", "dplyr", "ggplot2", "ggthemes", "knitr", "corrplot", "BAS", "bayesplot", "conflicted"))
    ```
4.  **Open each `.Rmd` file** in RStudio (`DataAnalysisCode.Rmd`, `Conjugate_prior.Rmd`, `BAS_analysis.Rmd`).
5.  **Click the "Knit" button** in RStudio to execute the code and generate the corresponding HTML report for each file. It is recommended to run the files in the order listed above.
