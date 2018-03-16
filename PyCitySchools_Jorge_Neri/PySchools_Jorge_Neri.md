

```python
import pandas as pd
import numpy as np
import csv
import collections
```


```python
students_data = '../raw_data/students_complete.csv'
schools_data = '../raw_data/schools_complete.csv'
```


```python
students_data_df = pd.read_csv(students_data)
schools_data_df = pd.read_csv(schools_data)
```


```python
schools_data_df.head()
#students_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Storing final table values in variables
num_schools = schools_data_df['School ID'].count()
num_students = students_data_df['Student ID'].count()
budget = schools_data_df['budget'].sum()
reading_avg = students_data_df['reading_score'].mean()
math_avg = students_data_df['math_score'].mean()
#print('School District has ' + str(num_schools) + ' schools')
#print('School District has ' + str(num_students) + ' students')
#print('School District has a budget of ' + str(budget))
#print('The average reading score in the district is ' + str(reading_avg))
#print('The average math score in the district is ' + str(math_avg))
```


```python
#Determine whether a student has passed or failed their reading and reading tests and assign a value to sum the passes
students_data_df['math_pass/fail'] = np.where(students_data_df['math_score'] > 70, 'Pass', 'Fail')
students_data_df['reading_pass/fail'] = np.where(students_data_df['reading_score'] > 70, 'Pass','Fail')
students_data_df['math_pass_count'] = np.where(students_data_df['math_pass/fail'] == 'Pass', 1 , 0)
students_data_df['reading_pass_count'] = np.where(students_data_df['reading_pass/fail'] == 'Pass', 1 , 0)
students_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>math_pass/fail</th>
      <th>reading_pass/fail</th>
      <th>math_pass_count</th>
      <th>reading_pass_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
      <td>Pass</td>
      <td>Fail</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>Fail</td>
      <td>Pass</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
      <td>Fail</td>
      <td>Pass</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
      <td>Fail</td>
      <td>Fail</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
      <td>Pass</td>
      <td>Pass</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Calculate math passing rate, reading passing rate and overall passing rate and store them in variables
math_pass = students_data_df[students_data_df['math_pass/fail']=='Pass'].count()['math_pass/fail']
math_pass_rate = (math_pass/num_students)*100
reading_pass = students_data_df[students_data_df['reading_pass/fail']=='Pass'].count()['reading_pass/fail']
reading_pass_rate = (reading_pass/num_students)*100
passing_avg = ((reading_avg + math_avg)/2)
#math_pass_rate
#reading_pass_rate
#passing_avg
```


```python
#students_data_df['reading_score'].min()
```


```python
#store values for district summary in a dictionary and create the summary dataframe
summary_dict = {'Total Schools': [num_schools], 'Total Students':[num_students], 'Total Budget':[budget], 'Average Math Score': [math_avg],
               'Average Reading Score':[reading_avg],'% Passing Math':[math_pass_rate], '% Passing Reading':[reading_pass_rate],'% Overall Passing Rate':[passing_avg] }
summary_df = pd.DataFrame(summary_dict) 
#({'Total Schools': [num_schools], 'Total Students':[num_students], 'Total Budget':[budget], 'Average Math Score': [math_avg],
#  'Average Reading Score':[reading_avg],'% Passing Math':[math_pass_rate], '% Passing Reading':[reading_pass_rate],
#  '% Overall Passing Rate':[passing_avg] })
summary_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Overall Passing Rate</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Total Budget</th>
      <th>Total Schools</th>
      <th>Total Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>80.431606</td>
      <td>72.392137</td>
      <td>82.971662</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>24649428</td>
      <td>15</td>
      <td>39170</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Rename the columns in the Schools data dataframe
schools_data_df.rename(columns={'name' : 'school_name'}, inplace=True)
schools_data_df.columns
```




    Index(['School ID', 'school_name', 'type', 'size', 'budget'], dtype='object')




```python
#Rename the columns in the Students data dataframe
students_data_df.rename(columns={'name':'student_name','gender':'student_gender', 'grade':'student_grade','school':'school_name'}, inplace=True)
students_data_df.columns
```




    Index(['Student ID', 'student_name', 'student_gender', 'student_grade',
           'school_name', 'reading_score', 'math_score', 'math_pass/fail',
           'reading_pass/fail', 'math_pass_count', 'reading_pass_count'],
          dtype='object')




```python
#Get Student population by school and store in a dataframe
grp_students_data = students_data_df.groupby(['school_name'])
school_student_count_df = pd.DataFrame(grp_students_data['Student ID'].count())
school_student_count_df.rename(columns={'Student ID':'student_population'})
school_student_count_df.reset_index(inplace=True)
school_student_count_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school_name</th>
      <th>Student ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>4976</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>1858</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>2949</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>2739</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>1468</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>4635</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>427</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>2917</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>4761</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>962</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>3999</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>1761</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>1635</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>2283</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>1800</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Pull out the test scores data from the students datafreame and store in a dataframe
school_testscores_df = students_data_df.iloc[:,[4,5,6,7,8,9,10]]
school_testscores_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school_name</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>math_pass/fail</th>
      <th>reading_pass/fail</th>
      <th>math_pass_count</th>
      <th>reading_pass_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
      <td>Pass</td>
      <td>Fail</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>Fail</td>
      <td>Pass</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
      <td>Fail</td>
      <td>Pass</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
      <td>Fail</td>
      <td>Fail</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
      <td>Pass</td>
      <td>Pass</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Get the avg tests scores by school and store in dataframe
grouped_school_testscores = school_testscores_df.groupby(['school_name'])
avg_testscores_by_school_df = pd.DataFrame(grouped_school_testscores['reading_score','math_score'].mean())
avg_testscores_by_school_df.reset_index(inplace=True)
school_test_pass_df = pd.DataFrame(grouped_school_testscores['math_pass_count', 'reading_pass_count'].sum())
school_test_pass_df.reset_index(inplace=True)
#print(avg_testscores_by_school_df)
#print(school_test_pass_df.head)
```


```python
#Merged school test data with remaining school information
merged_school_data_df = pd.merge(schools_data_df, avg_testscores_by_school_df, on='school_name')
final_merged_school_data_df = pd.merge(merged_school_data_df, school_test_pass_df, on='school_name')
#final_merged_school_data_df.head()
```


```python
#Change the index for the final merged table to school name
final_merged_school_data_df.set_index('school_name',inplace=True)
final_merged_school_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>math_pass_count</th>
      <th>reading_pass_count</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>1847</td>
      <td>2299</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>1</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>1880</td>
      <td>2313</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>2</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1583</td>
      <td>1631</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>3</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>3001</td>
      <td>3624</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>4</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1317</td>
      <td>1371</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Calculate for each school per student budget, math and reading passing rates and overall passing rate
final_merged_school_data_df['budget_per_student'] = final_merged_school_data_df['budget'].shift(0)/final_merged_school_data_df['size'].shift(0)
final_merged_school_data_df['%_passing_math'] = (final_merged_school_data_df['math_pass_count'].shift(0)/final_merged_school_data_df['size'].shift(0))*100
final_merged_school_data_df['%_passing_reading'] = (final_merged_school_data_df['reading_pass_count'].shift(0)/final_merged_school_data_df['size'].shift(0))*100
final_merged_school_data_df['overall_passing_rate'] = (final_merged_school_data_df['%_passing_math'].shift(0)+final_merged_school_data_df['%_passing_reading'])/2
final_merged_school_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>math_pass_count</th>
      <th>reading_pass_count</th>
      <th>budget_per_student</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>overall_passing_rate</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>1847</td>
      <td>2299</td>
      <td>655.0</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>1</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>1880</td>
      <td>2313</td>
      <td>639.0</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>2</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1583</td>
      <td>1631</td>
      <td>600.0</td>
      <td>89.892107</td>
      <td>92.617831</td>
      <td>91.254969</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>3</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>3001</td>
      <td>3624</td>
      <td>652.0</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>4</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1317</td>
      <td>1371</td>
      <td>625.0</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create the presentation dataframe
presentation_df = final_merged_school_data_df[['type','size','budget','budget_per_student','math_score','reading_score','%_passing_math','%_passing_reading','overall_passing_rate']]
```


```python
#Sort the presentation dataframe
presentation_df.sort_values('overall_passing_rate', ascending=False)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>budget_per_student</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>overall_passing_rate</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>90.932983</td>
      <td>93.254490</td>
      <td>92.093736</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>91.683992</td>
      <td>92.203742</td>
      <td>91.943867</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>90.632319</td>
      <td>92.740047</td>
      <td>91.686183</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>90.214067</td>
      <td>92.905199</td>
      <td>91.559633</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>600.0</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>89.892107</td>
      <td>92.617831</td>
      <td>91.254969</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>628.0</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>64.630225</td>
      <td>79.300643</td>
      <td>71.965434</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>65.753925</td>
      <td>77.510040</td>
      <td>71.631982</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Performing schools by overall passing grade
presentation_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>budget_per_student</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>overall_passing_rate</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>600.0</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>89.892107</td>
      <td>92.617831</td>
      <td>91.254969</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Bottom Performing schools by overall passing grade
presentation_df.tail()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>budget_per_student</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>overall_passing_rate</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>65.753925</td>
      <td>77.510040</td>
      <td>71.631982</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>90.214067</td>
      <td>92.905199</td>
      <td>91.559633</td>
    </tr>
  </tbody>
</table>
</div>




```python
#students_data_df['student_grade'].unique()
```


```python
#create Dataframes with student data by grade level
grade_9th_df = students_data_df.loc[students_data_df['student_grade'] == '9th',:]
grade_10th_df = students_data_df.loc[students_data_df['student_grade']== '10th',:]
grade_11th_df = students_data_df.loc[students_data_df['student_grade']=='11th',:]
grade_12th_df = students_data_df.loc[students_data_df['student_grade']=='12th',:]
```


```python
#Group Students by grade level
grp_schools_9th = grade_9th_df.groupby(['school_name'])
grp_schools_10th = grade_10th_df.groupby(['school_name'])
grp_schools_11th = grade_11th_df.groupby(['school_name'])
grp_schools_12th = grade_12th_df.groupby(['school_name'])
#grp_schools_9th
#grp_schools_10th
#grp_schools_11th
#grp_schools_12th
```


```python
#create dataframes with student average math score by school
avg_9th_math = pd.DataFrame(grp_schools_9th['math_score'].mean())
avg_9th_math.rename(columns={'math_score':'9th_grade'}, inplace=True)
avg_10th_math = pd.DataFrame(grp_schools_10th['math_score'].mean())
avg_10th_math.rename(columns={'math_score':'10th_grade'}, inplace=True)
avg_11th_math = pd.DataFrame(grp_schools_11th['math_score'].mean())
avg_11th_math.rename(columns={'math_score':'11th_grade'}, inplace=True)
avg_12th_math = pd.DataFrame(grp_schools_12th['math_score'].mean())
avg_12th_math.rename(columns={'math_score':'12th_grade'}, inplace=True)
```


```python
#combine the math score averages by school into 1 data frame
avg_math_score_by_grade = avg_9th_math.join(avg_10th_math, how='outer').join(avg_11th_math, how='outer').join(avg_12th_math, how='outer')
avg_math_score_by_grade
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th_grade</th>
      <th>10th_grade</th>
      <th>11th_grade</th>
      <th>12th_grade</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create dataframes with reading score averge by school for each grade level
avg_9th_reading = pd.DataFrame(grp_schools_9th['reading_score'].mean())
avg_9th_reading.rename(columns={'reading_score':'9th_grade'}, inplace=True)
avg_10th_reading = pd.DataFrame(grp_schools_10th['reading_score'].mean())
avg_10th_reading.rename(columns={'reading_score':'10th_grade'}, inplace=True)
avg_11th_reading = pd.DataFrame(grp_schools_11th['reading_score'].mean())
avg_11th_reading.rename(columns={'reading_score':'11th_grade'}, inplace=True)
avg_12th_reading = pd.DataFrame(grp_schools_12th['reading_score'].mean())
avg_12th_reading.rename(columns={'reading_score':'12th_grade'}, inplace=True)
```


```python
#combine the reading score averages by school into 1 dataframe
avg_reading_score_by_grade = avg_9th_reading.join(avg_10th_reading, how='outer').join(avg_11th_reading, how='outer').join(avg_12th_reading, how='outer')
avg_reading_score_by_grade
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th_grade</th>
      <th>10th_grade</th>
      <th>11th_grade</th>
      <th>12th_grade</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create bins to hold budget per student levels
#Bins are 575-595, 595-615, 615-635, 635-655
bins = [575, 595, 615, 635, 655]

#Bin group names
group_names = ['575-595','595-615','615-635','635-655',]
```


```python
pd.cut(final_merged_school_data_df['budget_per_student'], bins, labels=group_names)
```




    school_name
    Huang High School        635-655
    Figueroa High School     635-655
    Shelton High School      595-615
    Hernandez High School    635-655
    Griffin High School      615-635
    Wilson High School       575-595
    Cabrera High School      575-595
    Bailey High School       615-635
    Holden High School       575-595
    Pena High School         595-615
    Wright High School       575-595
    Rodriguez High School    635-655
    Johnson High School      635-655
    Ford High School         635-655
    Thomas High School       635-655
    Name: budget_per_student, dtype: category
    Categories (4, object): [575-595 < 595-615 < 615-635 < 635-655]




```python
#insert the bins into the final school dataframe
final_merged_school_data_df['budget_levels'] = pd.cut(final_merged_school_data_df['budget_per_student'], bins, labels=group_names)
final_merged_school_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>math_pass_count</th>
      <th>reading_pass_count</th>
      <th>budget_per_student</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>overall_passing_rate</th>
      <th>budget_levels</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>1847</td>
      <td>2299</td>
      <td>655.0</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
      <td>635-655</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>1</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>1880</td>
      <td>2313</td>
      <td>639.0</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
      <td>635-655</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>2</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1583</td>
      <td>1631</td>
      <td>600.0</td>
      <td>89.892107</td>
      <td>92.617831</td>
      <td>91.254969</td>
      <td>595-615</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>3</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>3001</td>
      <td>3624</td>
      <td>652.0</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
      <td>635-655</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>4</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1317</td>
      <td>1371</td>
      <td>625.0</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
      <td>615-635</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create a dataframe that provides average test scores by funding levels
grp_budget_by_student = final_merged_school_data_df.groupby(['budget_levels'])
test_scores_by_funding_df = pd.DataFrame(grp_budget_by_student['math_score','reading_score','%_passing_math', '%_passing_reading',
                                                              'overall_passing_rate'].mean())
test_scores_by_funding_df.rename(columns={'math_score':'Average Math Score', 'reading_score':'Average Reading Score','overall_passing_rate':'% Overall Passing Rate'},inplace=True)
test_scores_by_funding_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>budget_levels</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>575-595</th>
      <td>83.455399</td>
      <td>83.933814</td>
      <td>90.350436</td>
      <td>93.325838</td>
      <td>91.838137</td>
    </tr>
    <tr>
      <th>595-615</th>
      <td>83.599686</td>
      <td>83.885211</td>
      <td>90.788049</td>
      <td>92.410786</td>
      <td>91.599418</td>
    </tr>
    <tr>
      <th>615-635</th>
      <td>80.199966</td>
      <td>82.425360</td>
      <td>77.172061</td>
      <td>86.346507</td>
      <td>81.759284</td>
    </tr>
    <tr>
      <th>635-655</th>
      <td>77.866721</td>
      <td>81.368774</td>
      <td>67.957362</td>
      <td>80.268067</td>
      <td>74.112715</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create a dataframe that provides average test scores by school type
grp_school_type = final_merged_school_data_df.groupby(['type'])
test_scores_by_type = pd.DataFrame(grp_school_type['math_score', 'reading_score', '%_passing_math', '%_passing_reading', 
                                                   'overall_passing_rate'].mean())
test_scores_by_type.rename(columns={'math_score':'Average Math Score', 'reading_score':'Average Reading Score',
                                   'overall_passing_rate':'% Overall Passing Rate'},inplace=True)
test_scores_by_type
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>90.363226</td>
      <td>93.052812</td>
      <td>91.708019</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>64.302528</td>
      <td>78.324559</td>
      <td>71.313543</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create bins to hold school size per student levels
#Bins are 400-1500, 1500-2500, 2500-3500, 3500-5000
bins = [400, 1500, 2500, 3500, 5000]

#Bin group names
group_names = ['400-1500','1500-2500','2500-3500','3500-5000']
```


```python
#add bins to final school dataframe
final_merged_school_data_df['size_levels'] = pd.cut(final_merged_school_data_df['size'], bins, labels=group_names)
final_merged_school_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>math_pass_count</th>
      <th>reading_pass_count</th>
      <th>budget_per_student</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>overall_passing_rate</th>
      <th>budget_levels</th>
      <th>size_levels</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>1847</td>
      <td>2299</td>
      <td>655.0</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
      <td>635-655</td>
      <td>2500-3500</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>1</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>1880</td>
      <td>2313</td>
      <td>639.0</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
      <td>635-655</td>
      <td>2500-3500</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>2</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1583</td>
      <td>1631</td>
      <td>600.0</td>
      <td>89.892107</td>
      <td>92.617831</td>
      <td>91.254969</td>
      <td>595-615</td>
      <td>1500-2500</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>3</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>3001</td>
      <td>3624</td>
      <td>652.0</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
      <td>635-655</td>
      <td>3500-5000</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>4</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1317</td>
      <td>1371</td>
      <td>625.0</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
      <td>615-635</td>
      <td>400-1500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create a dataframe that provides average test scores by school size
grp_school_size = final_merged_school_data_df.groupby(['size_levels'])
test_scores_by_size = pd.DataFrame(grp_school_size['math_score', 'reading_score', '%_passing_math', '%_passing_reading', 
                                                   'overall_passing_rate'].mean())
test_scores_by_size.rename(columns={'math_score':'Average Math Score', 'reading_score':'Average Reading Score',
                                   'overall_passing_rate':'% Overall Passing Rate'},inplace=True)
test_scores_by_size
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%_passing_math</th>
      <th>%_passing_reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>size_levels</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>400-1500</th>
      <td>83.664898</td>
      <td>83.892148</td>
      <td>90.676736</td>
      <td>92.778720</td>
      <td>91.727728</td>
    </tr>
    <tr>
      <th>1500-2500</th>
      <td>83.359224</td>
      <td>83.898984</td>
      <td>90.175120</td>
      <td>93.217267</td>
      <td>91.696193</td>
    </tr>
    <tr>
      <th>2500-3500</th>
      <td>76.814591</td>
      <td>81.029000</td>
      <td>64.274276</td>
      <td>78.252419</td>
      <td>71.263347</td>
    </tr>
    <tr>
      <th>3500-5000</th>
      <td>77.063340</td>
      <td>80.919864</td>
      <td>64.323717</td>
      <td>78.378664</td>
      <td>71.351190</td>
    </tr>
  </tbody>
</table>
</div>


