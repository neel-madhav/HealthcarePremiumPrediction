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

We will drop na values as our whole data size is of 50000 records out of which maximum 15 rows have na value. So in this case we will remove those data. ```dropna()``` function will be used to drop all na values in the dataframe. Based on different requirement we can use mean, median, mode as well.
```python
df = df.dropna()
```

To check duplicated rows 
```python
df = df.duplicated.sum()
df = df.drop_duplicates()
```
To get quick statistics for our data frame
```python
df.describe()
```
|index|age|number\_of\_dependants|income\_lakhs|annual\_premium\_amount|
|---|---|---|---|---|
|count|49976\.0|49976\.0|49976\.0|49976\.0|
|mean|34\.59176404674243|1\.711841684008324|23\.021150152072995|15766\.810188890668|
|std|15\.000377630784854|1\.4981945585589727|24\.221794329612134|8419\.995270538298|
|min|18\.0|-3\.0|1\.0|3501\.0|
|25%|22\.0|0\.0|7\.0|8607\.75|
|50%|31\.0|2\.0|17\.0|13928\.0|
|75%|45\.0|3\.0|31\.0|22273\.5|
|max|356\.0|5\.0|930\.0|43471\.0|

### Analysis 
Based on statistics we can alnalyse that we have outliers. Maximum value for Age is 350 which is an outlier. In number of dependents th minmum value is -3 which is incorrect data. In income lakhs maximum value is 930 Lakhs with mean value as 23 Lakh. So this is an outlier

#### Number of Dependents
To check number of rows with negative value and unique value
```python
df[df.number_of_dependants < 0].shape
df[df.number_of_dependants < 0]['number_of_dependants'].unique()
```
Here we are taking an assumption that negative values for number of dependents can be an error. As there are 72 rows with negative value dropping these rows are not adivsable. We are replacing negative values with positive values as its not possible for number of dependents to be negaive.
```python
df['number_of_dependants'] = df['number_of_dependants'].abs()
```
#### Age
This column have outliers. To visulaize outliers box plot is preffered. 
```python
sns.boxplot(x = df['age'])
plt.show()
```




