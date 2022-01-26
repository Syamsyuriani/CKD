# Handling missing values in the CKD datset
This data is taken from the [UCI repository](https://archive.ics.uci.edu/ml/datasets/Chronic_Kidney_Disease) which contains the classification of chronic kidney disease.
Data from the source has some missing values.
Therefore, various methods are used to overcome missing values in the data.
In this data, we use **mean**, **median**, and **linear regression** between features to overcome missing values.
In addition, we also use the influence of the behavior of one feature on other features.

1. Estimating the Value of Other Feature

    This approach is commonly used to estimate qualitative data from quantitative data that is connected to it, or vice versa. There are several features in this data that are related and may be used to represent one another. As a result, the missing value of a feature may be inferred using the characteristics that are related with it. Several feature values that are estimated from other feature values as follows:
    - **Blood Pressure (bp)** <- Hypertension (htn): 
    
      if htn is "no" then bp=70, if htn is "yes", bp = 80.
    - **Pus Cell (pc)** <- Pus Cell Clumps (pcc):
      
      if pcc is "not present" then pc is "normal", if pcc is "present", pc is "abnormal".
    - **Pus Cell Clumps (pcc)** <- Class:
 
      if "not ckd" then pcc is "not present".
    - **Bacteria** <- Class:
     
      if "not ckd" then ba is "not present".
    - **Blood Glucose Random (bgr)** <- Sugar (sg): 
    
      if sg=0, bgr=122; sg=1, bgr=213; sg=2, bgr=256; sg=4, bgr=302; sg=5 or NaN, bgr=172.
    - **Hypertension (htn)** <- Class: 
      
      if "not ckd" then htn is "no".
    - **Diabetes Mellitus (dm)** <- Class: 
      
      if "not ckd" then dm is "no".
    - **Coronary artery Disease (cad)** <- Class: 
    
      if "not ckd" then cad is "no".
    - **Appetite (appet)** <- Class: 
      
      if "not ckd" then appet is "good".
    - **Pedal Edema (pe)** <- Class:
    
      if "not ckd" then pe is "no".
    - **Anemia (Ane)** <- Class: 
      
      if "not ckd" then ane is "no".
2. Mean and Median

    Mean and median are utilized to approximate the missing value by considering at the distribution of data on a feature. The more normally distributed the data is, the more precise the estimated value will be. If the data distribution is sufficient and there are no outliers, mean will be prioritized first; if there are outliers, mean will shift, reducing accuracy. Median can be used instead of mean to solve this issue. This technique may be a viable solution, for quantitative data that cannot be represented by other characteristics.  On this data, we use mean to estimate missing value of **Age (ag)**, **Specific Gravity (sg)**, **Blood Urea (bu)**, **Serum Creatinine (sc)**, **Sodium (sod)**, **Pottasium (pot)**, and **Hemoglobin (hb)**, and we use median to estimate the missing value of **White Blood Cell Count (wc)**.
3. Linear Regression

    Linear regression is one of the imputation methods. This method is used for two quantitative features that have a fairly high correlation. The missing values for the two features that have a high correlation are then imputed based on the general trend of a simple linear model formed from the two features. According to [research](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6238670/), **packed cell volume (pcv)** and hemoglobin (hb) have a relationship. This is consistent with the fact that the correlation coefficient between PCV and HB is close to one. The same is true for **red blood cell count (rc)** and hemoglobin.
