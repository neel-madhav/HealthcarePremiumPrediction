## Project Overview
This project develops a predictive model to estimate health care insurance premiums based on Age, BMI, Medical history and smoking habits. Here we will build and deploy predictive model using streamlit application.

## Objective
1. Develop high accuracy predictive model with accuracy more than 97%. Percentage difference between predicted value and actual value on a minimum of 95% errors should be less than 10%.
2. Deploy the model in cloud.
3. Create an interactive streamlit application.

## Data Collection and Cleaning

We have data in xlsx format. To read the data from xlsx format we used pandas library `read_excel()`. To analyse data we want to look for number of columns and their value. That's why used `head()` function to display all columns and 5 rows.
```python
df = pd.read_excel("premiums.xlsx")
df.head()
```
|index|Age|Gender|Region|Marital\_status|Number Of Dependants|BMI\_Category|Smoking\_Status|Employment\_Status|Income\_Level|Income\_Lakhs|Medical History|Insurance\_Plan|Annual\_Premium\_Amount|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|26|Male|Northwest|Unmarried|0|Normal|No Smoking|Salaried|\<10L|6|Diabetes|Bronze|9053|
|1|29|Female|Southeast|Married|2|Obesity|Regular|Salaried|\<10L|6|Diabetes|Bronze|16339|
|2|49|Female|Northeast|Married|2|Normal|No Smoking|Self-Employed|10L - 25L|20|High blood pressure|Silver|18164|
|3|30|Female|Southeast|Married|3|Normal|No Smoking|Salaried|\> 40L|77|No Disease|Gold|20303|
|4|18|Male|Northeast|Unmarried|0|Overweight|Regular|Self-Employed|\> 40L|99|High blood pressure|Silver|13365|

The names for columns are not consistent. We will use snake case convention. So we will convert all column names into consistent naming. 
```python
df.columns = df.columns.str.replace(" ", "_").str.lower()
```
This command will replace space " " with underscore "_". Also it will convert all letters into lower case. Now all columns are consistent.

We will check NA/NULL/null values in our dataset
```python
df.isnull().sum()
```
This will provide NA statistics for each columns of data frame.

We will drop na values as our whole data size is of 50000 records out of which maximum 15 rows have na value. So in this case we will remove those data. ```dropna()``` function will be used to drop all na values in the dataframe. 
```python
df = df.dropna()
```

