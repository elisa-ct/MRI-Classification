# MRI-Classification
R code used for the classification of magnetic resonance imaging (MRI) for the detection of Alzheimer's disease using the GLCM technique and classification algorithms: KNN, random forest, decision tree, and logistic regression. Alzheimer's disease (AD) is a progressive neurodegenerative pathology that affects millions of people worldwide. Early diagnosis of AD is crucial for the proper management of the disease and improving the quality of life of patients. In this study, a method for the detection of AD is proposed through the analysis of MRI images and supervised classification techniques.

## Database
The database used is primarily extracted from **ADNI** (sourced from Kaggle) and contains 6,400 2D MRI images of different brain sections for various stages of Alzheimer's disease, providing a large dataset for analysis and validation of classification models. The images can be found at the following link: https://www.kaggle.com/dsv/3364939. The images are grouped into two balanced classes: Non Demented and Demented, with 3,200 images each.

## Metodology
The method is based on the extraction of metrics from the **Gray Level Co-occurrence Matrix** (GLCM) from the MRI images. These metrics summarize the textures and patterns present in the images, which are related to structural changes in the brain associated with Alzheimer's disease. Subsequently, supervised classification algorithms such as KNN, decision trees, random forest, and logistic regression are used to classify the MRI images and diagnose patients with or without dementia.

### 1. Feature Extraction

See the file **Extraccion_GLCM.qmd**.

Features were extracted from the images using the GLCM package. An additional direction called radial was created, which summarizes the individual directions.
Nine GLCM metrics were obtained: Contrast, Homogeneity, Dissimilarity, Entropy, Variance, Mean, Correlation, ASM, SA. Two initial datasets were built: one for the radial direction (6,400 observations, 9 variables) and another for the set of all individual directions (6,400 observations, 9 x 5 = 45 variables).

### 2. Variable Analysis

See the file **ANALISIS y CLASIFICACION.qmd**.

A variable analysis was performed using: correlation matrix, PCA, VIF analysis, and variable clustering. The conclusions drawn indicate that the GLCM metrics can be grouped into three categories. Two datasets are proposed for classification:

- **Working with Principal Components**: Use the three principal components obtained from the PCA for classification. These components explain 98% of the variance of the radial direction dataset.
- **Selecting Representative Variables**: Select one representative variable from each of the three identified groups and classify using these three variables directly. The selected variables were: Contrast, Variance, and Entropy from the radial direction.

Additionally, for classification, tests will be conducted with the full datasets from both the radial direction and all directions. For random forest, the total dataset will be used, and for logistic regression, additional tests will be conducted for variable selection using stepwise methods.

### 3. Classification

See the file **ANALISIS y CLASIFICACION.qmd**.

- **KNN**: using the radial PCA dataset and selected variables. Tests include: radial direction, all directions, radial PCA without Correlation, PCA of all directions. Additionally, a custom KNN code was developed to calculate Frobenius distances between the GLCM matrices in the radial direction (see **FROBENIUS.qmd**).
- **Random Forest**: using the full dataset of all directions.
- **Decision tree**: using the radial PCA dataset and selected variables. Tests include: radial direction, all directions, radial PCA without Correlation, PCA of all directions.
- **Logistic Regression**: using the radial PCA dataset. An additional variable analysis was conducted, and the final classification was performed using the variables from the radial direction: Homogeneity, ASM, Mean, and Variance.
