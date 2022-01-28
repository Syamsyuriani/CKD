# Handling missing values in the CKD datset
This data is taken from the [UCI repository](https://archive.ics.uci.edu/ml/datasets/Chronic_Kidney_Disease) which contains the classification of chronic kidney disease.
Data from the source has some missing values.
Therefore, various methods are used to overcome missing values in the data.
In this data, we use **mean**, **median**, and **linear regression** between features to overcome missing values.
In addition, we also use the influence of the behavior of one feature on other features.
```python
dataset = pd.read_csv('https://raw.githubusercontent.com/Syamsyuriani/Handling-missing-values-in-the-CKD-dataset/main/kidney_disease.csv')
dataset.head()
```

![image](https://user-images.githubusercontent.com/72261134/151512974-80a5d9b5-f93e-4efe-bbd0-6f71a50b83f0.png)

```python
#calculate the missing value of each feature
print(dataset.isnull().sum())
```
![image](https://user-images.githubusercontent.com/72261134/151513322-7fa26f58-481f-40ec-b977-8518499b50ab.png)

1. Estimating the Value of Other Feature

    This approach is commonly used to estimate qualitative data from quantitative data that is connected to it, or vice versa. There are several features in this data that are related and may be used to represent one another. As a result, the missing value of a feature may be inferred using the characteristics that are related with it. Several feature values that are estimated from other feature values as follows:
    - **Hypertension (htn)** <- Class: 
      
      if "not ckd" then htn is "no".
      ```python
      htn_notckd = dataset['htn'].loc[dataset['classification']=='notckd']
      htn_notckd.describe()
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151515489-d954107c-9d4d-4b30-b4ec-2cd5fa33e503.png)

      
    - **Blood Pressure (bp)** <- Hypertension (htn): 
    
      if htn is "no" then bp=70, if htn is "yes", bp = 80.  
    - **Pus Cell Clumps (pcc)** <- Class:
 
      if "not ckd" then pcc is "not present".
      ```python
      dataset['pcc'].loc[dataset['classification']=='notckd'].describe()
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151517854-54c104c9-ae43-4bb1-ba74-9ee1b263a78e.png)
    - **Pus Cell (pc)** <- Pus Cell Clumps (pcc):
      
      if pcc is "not present" then pc is "normal", if pcc is "present", pc is "abnormal".
       ```python
      print('pcc is not present:\n', dataset['pc'].loc[dataset['pcc']=='notpresent'].describe())
      print('pcc is present:\n', dataset['pc'].loc[dataset['pcc']=='present'].describe())
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151518574-98c08534-cc37-4ed5-85c5-8117a726686d.png)  ![image](https://user-images.githubusercontent.com/72261134/151518629-03cf911b-af6a-432e-a3a9-74db80271f4e.png)
    - **Bacteria** <- Class:
     
      if "not ckd" then ba is "not present".
      ```python
      dataset['ba'].loc[dataset['classification']=='notckd'].describe()
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151519161-4e7435a1-d8cf-49a6-a5bf-4fb4281601ca.png)n
    - **Blood Glucose Random (bgr)** <- Sugar (sg): 
    
      if sg=0, bgr=122; sg=1, bgr=213; sg=2, bgr=256; sg=4, bgr=302; sg=5 or NaN, bgr=172.
    - **Diabetes Mellitus (dm)** <- Class: 
      
      if "not ckd" then dm is "no".
      ```python
      dataset['dm'].loc[dataset['classification']=='notckd'].describe()
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151519360-da9e83de-4d98-42ed-9fb7-661e2626f8e5.png)
    - **Coronary artery Disease (cad)** <- Class: 
    
      if "not ckd" then cad is "no".
      ```python
      dataset['cad'].loc[dataset['classification']=='notckd'].describe()
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151519460-7ff5bb67-c671-4daa-8bb5-997abb8a269c.png)
    - **Appetite (appet)** <- Class: 
      
      if "not ckd" then appet is "good".
      ```python
      dataset['appet'].loc[dataset['classification']=='notckd'].describe()
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151520054-29f875c5-d7c7-4e66-ace5-fb982ce2869d.png)
    - **Pedal Edema (pe)** <- Class:
    
      if "not ckd" then pe is "no".
      ```python
      dataset['pe'].loc[dataset['classification']=='notckd'].describe()
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151519552-2ff750da-0897-4960-afc4-0fdc378794f3.png)
    - **Anemia (Ane)** <- Class: 
      
      if "not ckd" then ane is "no".
      ```python
      dataset['ane'].loc[dataset['classification']=='notckd'].describe()
      ```
      ![image](https://user-images.githubusercontent.com/72261134/151519659-66c3c112-36b1-4aca-b8d2-2648add39f65.png)
2. Mean and Median

    Mean and median are utilized to approximate the missing value by considering at the distribution of data on a feature. The more normally distributed the data is, the more precise the estimated value will be. If the data distribution is sufficient and there are no outliers, mean will be prioritized first; if there are outliers, mean will shift, reducing accuracy. Median can be used instead of mean to solve this issue. This technique may be a viable solution, for quantitative data that cannot be represented by other characteristics.  On this data, we use mean to estimate missing value of **Age (ag)**, **Specific Gravity (sg)**, **Blood Urea (bu)**, **Serum Creatinine (sc)**, **Sodium (sod)**, **Pottasium (pot)**, and **Hemoglobin (hemo)**, and we use median to estimate the missing value of **White Blood Cell Count (wc)**.
    ![image](https://user-images.githubusercontent.com/72261134/151525274-c2e65bff-9b05-4a24-970a-9f6fb0ed3bfb.png)

3. Linear Regression

    Linear regression is one of the imputation methods. This method is used for two quantitative features that have a fairly high correlation. The missing values for the two features that have a high correlation are then imputed based on the general trend of a simple linear model formed from the two features. According to [research](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6238670/), **packed cell volume (pcv)** and hemoglobin have a relationship. This is consistent with the fact that the correlation coefficient between PCV and HB is close to one. The same is true for **red blood cell count (rc)** and hemoglobin.
    ![image](https://user-images.githubusercontent.com/72261134/151530597-2198eb9b-e5a5-4979-ac64-992354664be3.png)

