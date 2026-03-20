#EX.NO:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
```
import pandas as pd
import numpy as np
from scipy import stats

def initial_data_info(df):
    print("Info:")
    print(df.info())
    print("\nDescribe:")
    print(df.describe())
    print("\nNull values per column:")
    print(df.isnull().sum())
    print('=' * 40)

def remove_nulls(df):
    return df.dropna()

def remove_outliers_iqr(df, columns):
    for col in columns:
        Q1 = df[col].quantile(0.25)
        Q3 = df[col].quantile(0.75)
        IQR = Q3 - Q1
        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR
        df = df[(df[col] >= lower) & (df[col] <= upper)]
    return df

def remove_outliers_zscore(df, columns, thresh=3):
    for col in columns:
        z = np.abs(stats.zscore(df[col].dropna()))
        filtered_entries = (z < thresh)
        valid_indices = df[col].dropna().index[filtered_entries]
        df = df.loc[valid_indices]
    return df

file_names = [
    "Data_set.csv",
    "heights.csv",
    "iris.csv",
    "Loan_data.csv",
    "SAMPLEIDS.csv"
]

for file in file_names:
    print(f'Processing file: {file}')
    df = pd.read_csv(file)
    
    print("Initial Info")
    initial_data_info(df)
    
    df_no_nulls = remove_nulls(df)
    print("After Removing Nulls")
    print(df_no_nulls.shape)
    
    numeric_cols = df_no_nulls.select_dtypes(include=[np.number]).columns.tolist()
    
    df_iqr = remove_outliers_iqr(df_no_nulls.copy(), numeric_cols)
    print("After Removing Outliers (IQR method):", df_iqr.shape)
    
    df_z = remove_outliers_zscore(df_no_nulls.copy(), numeric_cols)
    print("After Removing Outliers (Z-score method):", df_z.shape)
    print('=' * 80)
```
# Result
<img width="1035" height="848" alt="image" src="https://github.com/user-attachments/assets/858a75f5-0f52-40ea-a86c-5f5f97b143c3" />
<img width="1017" height="722" alt="image" src="https://github.com/user-attachments/assets/2c30df3a-c5c7-4e6d-8388-590414db82d3" />
<img width="1046" height="826" alt="image" src="https://github.com/user-attachments/assets/2541f5a6-7c49-4f7d-9ffe-3011e206a074" />
<img width="1101" height="923" alt="image" src="https://github.com/user-attachments/assets/e516bdc6-8786-41a6-bac2-c6fdeaa894d9" />
<img width="1047" height="652" alt="image" src="https://github.com/user-attachments/assets/05feafac-d03d-4375-8b2b-5b673fa0d4c1" />
<img width="1053" height="738" alt="image" src="https://github.com/user-attachments/assets/f8005ba1-e50d-4d43-8b2f-7617b6bbc2dd" />
<img width="1043" height="542" alt="image" src="https://github.com/user-attachments/assets/5796858a-1e9d-4e40-825c-4ada3e664c30" />
<img width="1047" height="814" alt="image" src="https://github.com/user-attachments/assets/5c39d394-3155-476a-a8ab-0cbbabdf8cce" />
<img width="1040" height="449" alt="image" src="https://github.com/user-attachments/assets/25858a0c-fcb1-489b-ba46-53684045b37e" />

