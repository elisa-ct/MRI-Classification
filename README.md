# MRI-Classification
Código en R empleado para la clasificación de imágenes de resonancia magnética para la detección de la enfermedad de Alzheimer a partir de la técnica GLCM y los algoritmos de clasificación: KNN, random forest, árbol de decisión y regresión logística. La enfermedad de Alzheimer (EA) es una patología neurodegenerativa progresiva que afecta a millones de personas en todo el mundo. El diagnóstico temprano de la EA es crucial para el manejo adecuado de la enfermedad y la mejora de la calidad de vida de los pacientes. En este estudio, se propone un método para la detección de la EA mediante el análisis de imágenes de resonancia magnética (MRI) y técnicas de clasificación supervisada.

## Base de datos
La base de datos utilizada se extrae principalmente de **ADNI** (extraídas de Kaggle) y contiene 6400 imágenes MRI en 2D de distintas secciones del cerebro para diferentes etapas de la enfermedad de Alzheimer, lo que proporciona un amplio conjunto de datos para el análisis y la validación de los modelos de clasificación. Las imágenes pueden encontrarse en el siguiente enlace: https://www.kaggle.com/dsv/3364939 .
Se agrupan las imágenes en dos clases balanceadas: *Non Demented* y *Demented*, con 3200 imágenes cada una.

## Métodología empleada
El método se basa en la extracción de métricas de la **matriz de co-ocurrencia de niveles de gris** (GLCM) a partir de las imágenes MRI. Estas métricas permiten resumir las texturas y patrones presentes en las imágenes, las cuales están relacionadas con los cambios estructurales en el cerebro asociados a la EA. Posteriormente, se emplean algoritmos de clasificación supervisada como KNN, árboles de decisión, random forest y regresión logística para clasificar las imágenes MRI y diagnosticar a los pacientes con o sin demencia. 

### 1. Extracción de características

Ver archivo **Extraccion_GLCM.qmd**.

Se han extraído las características de las imágenes a partir del paquete *GLCM:Textures*. Se ha creado una dirección adicional llamada *radial* que resume las direcciones individuales.
Se obtienen 9 métrica GLCM: *Contrast, Homogeneity, Dissimilarity, Entropy, Variance, Mean, Correlation, ASM, SA*. Se consturyen dos datasets iniciales, uno para la dirección radial (6400 observaciones, 9 variables) y otro para el conjunto de todas las direcciones individuales (6400 observaciones, 9 x 5 = 45 variables). 

### 2. Análisis de variables

Ver archivo **ANALISIS y CLASIFICACION.qmd**

Se hace un análisis de variables a partir de: matriz de correlación, PCA, análisis VIF y clúster de variables. Las conclusiones extraídas son que las métricas GLCM se agrupan en 3 categorías. Se plantean dos datasets para la clasificación:

- Trabajar con las Componentes Principales: Utilizar las tres componentes principales obtenidas del PCA para la clasificación. Estas componentes explican el 98 % de la varianza del dataset de la dirección radial.
- Seleccionar Variables Representativas: Seleccionar una variable representativa de cada uno de los tres grupos identificados y realizar la clasificación con estas tres variables directamente. Las variables seleccionadas han sido: *Contrast, Variance y Entropy*, de la dirección radial.

Además para la clasificación se harán pruebas con los *datasets* completos de las direcciones radial y todas las direcciones. Además para random forest se usará el dataset total y para regresión logística se harán pruebas adicionales para la selección de variables a partir de step.

### 3. Clasificación

Ver archivo **ANALISIS y CLASIFICACION.qmd**

- **KNN**: con el dataset de PCA radial y variables seleccionadas. Se inlcuyen las pruebas de: dirección radial, todas las direcciones, PCA radial sin Correlation, PCA de todas las direcciones. Adicionalmente, se contruye un código propio de KNN para calcular distancias de Frobenius entre las matrices GLCM de la dirección radial (ver archivo **FROBENIUS.qmd**).
- **Árbol de decisión**: con el dataset de PCA radial y variables seleccionadas. Se inlcuyen las pruebas de: dirección radial, todas las direcciones, PCA radial sin Correlation, PCA de todas las direcciones.
- **Random Forest**: con el dataset de todas las direcciones.
- **Regresión logística**: con el dataset de PCA radial. Se hace un análisis de variables adicional y se realiza la clasificación final con las variables de la dirección radial: *Homogeneity, ASM, Mean y Variance.*
