
# PyCity Schools Analysis
  - In the entire district, the overall passing percentage for the combined math and reading scores is only 65.2%.
  - Cabrera High School was the school the highest overall passing rate at 91.3%.
  - Rodriguez High School was the school the lowest overall passing rate at 53.0%.
  - The top 5 performing schools were all charter schools, while the bottom 5 performing schools were all district schools.


```python
#Dependencies
import pandas as pd
```


```python
#File Paths
file_1 = "Resources/schools_complete.csv"
file_2 = "Resources/students_complete.csv"

#File Reads
school_file = pd.DataFrame(pd.read_csv(file_1))
student_file = pd.DataFrame(pd.read_csv(file_2))

#Rename columns for merge 
school_file.rename(columns={"name":"school"}, inplace=True)

#Merge files together into one DataFrame
merged_file = pd.merge(school_file, student_file, left_index=True, on="school")
merged_file.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>reading_score</th>
      <th>math_score</th>
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
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>



### District Summary


```python
#------------------District Summary----------------
#Calculate "Total Schools"
total_schools = len(merged_file["school"].unique())
#Calculate "Total Students"
total_students = merged_file["size"].unique().sum()
#Calculate "Total Budget"
total_budget = merged_file["budget"].unique().sum()
#Calculate "Average Math Score"
aver_math_score = merged_file["math_score"].mean()
#Calculate "Average Reading Score"
aver_reading_score = merged_file["reading_score"].mean()
#Calculate "% Passing Math"
amount_passing_m = merged_file.loc[merged_file["math_score"] >= 70]["math_score"].count()
percent_passing_math = amount_passing_m/total_students
#Calculate "% Passing Reading"
amount_passing_r = merged_file.loc[merged_file["reading_score"] >= 70]["reading_score"].count()
percent_passing_reading = amount_passing_r/total_students
#Calculate "Overall Passing Rate"
amount_passing_both = merged_file[(merged_file['math_score'] >= 70) & (merged_file['reading_score'] >= 70)]["name"].count()
overall_passing_rate = amount_passing_both/total_students

#Create DataFrame for "District Summary" 
district_summary_df = pd.DataFrame({
    "Total Schools": [total_schools],
    "Total Students": [total_students],
    "Total Budget": [total_budget],
    "Average Math Score": [aver_math_score],
    "Average Reading Score": [aver_reading_score],
    "% Passing Math":[percent_passing_math],
    "% Passing Reading": [percent_passing_reading],
    "Overall Passing Rate": [overall_passing_rate]}, columns=["Total Schools","Total Students",
                                                              "Total Budget","Average Math Score",
                                                              "Average Reading Score","% Passing Math",
                                                              "% Passing Reading","Overall Passing Rate"])
#Format "District Summary DataFrame"
district_summary_df.style.format({"Total Students":"{:,}",
                                  "Total Budget":"${:,.2f}",
                                  "Average Math Score":"{:.1f}",
                                  "Average Reading Score":"{:.1f}",
                                  "% Passing Math":"{:.1%}",
                                  "% Passing Reading":"{:.1%}",
                                  "Overall Passing Rate":"{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Schools</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Average Math Score</th> 
        <th class="col_heading level0 col4" >Average Reading Score</th> 
        <th class="col_heading level0 col5" >% Passing Math</th> 
        <th class="col_heading level0 col6" >% Passing Reading</th> 
        <th class="col_heading level0 col7" >Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3row0_col0" class="data row0 col0" >15</td> 
        <td id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3row0_col1" class="data row0 col1" >39,170</td> 
        <td id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3row0_col2" class="data row0 col2" >$24,649,428.00</td> 
        <td id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3row0_col3" class="data row0 col3" >79.0</td> 
        <td id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3row0_col4" class="data row0 col4" >81.9</td> 
        <td id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3row0_col5" class="data row0 col5" >75.0%</td> 
        <td id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3row0_col6" class="data row0 col6" >85.8%</td> 
        <td id="T_44ae4dae_29fd_11e8_ab2d_80ce62114cc3row0_col7" class="data row0 col7" >65.2%</td> 
    </tr></tbody> 
</table> 



### School Summary


```python
#Group data by school and set as index
school = merged_file.set_index("school").groupby(["school"])
#Calculate "School Type"
school_types = school_file.set_index("school")["type"]
#Calculate "Total Students" per school
total_students_per_school = school["Student ID"].count()
#Calculate "Total School Budget" per school
school_budget = school_file.set_index("school")["budget"]
#Calculate "Per Student Budget" per school
student_budget = school_file.set_index("school")["budget"] / school_file.set_index("school")["size"]
#Calculate "Average Math Score" per school
math_score_school = school["math_score"].mean()
#Calculate "Average Reading Score" per school
reading_score_school = school["reading_score"].mean()
#Calculate "% Passing Math" per school
passing_math = merged_file[merged_file["math_score"] >= 70].groupby("school")["Student ID"].count() / total_students_per_school
#Calculate "% Passing reading" per school
passing_reading = merged_file[merged_file["reading_score"] >= 70].groupby("school")["Student ID"].count() / total_students_per_school
#Calculate "% Overall Passing Rate" per school
overall_passing_school = merged_file[(merged_file["math_score"] >= 70) & (merged_file["reading_score"] >= 70)].groupby("school")["Student ID"].count() / total_students_per_school 

#Create DataFrame for "School Summary"
school_summary = pd.DataFrame({
    "School Type":school_types,
    "Total Students":total_students_per_school,
    "Total School Budget":school_budget, 
    "Per Student Budget": student_budget,
    "Average Math Score": math_score_school,
    "Average Reading Score": reading_score_school,
    "% Passing Math": passing_math,
    "% Passing reading": passing_reading,
    "% Overall Passing Rate": overall_passing_school}, columns=["School Type",
                                                                "Total Students",
                                                                "Total School Budget",
                                                                "Per Student Budget",
                                                                "Average Math Score",
                                                                "Average Reading Score",
                                                                "% Passing Math",
                                                                "% Passing reading",
                                                                "% Overall Passing Rate"])
                                                                                           

    
#Format "School Summary" DataFrame                                                                                            
school_summary.style.format({"Total Students": "{:,}",
                             "Total School Budget": "${:,}", 
                             "Per Student Budget": "${:.0f}",
                             "Average Math Score": "{:.1f}",
                             "Average Reading Score": "{:.1f}",
                             "% Passing Math": "{:.1%}",
                             "% Passing reading": "{:.1%}",
                             "% Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col0" class="data row0 col0" >District</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col1" class="data row0 col1" >4,976</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col2" class="data row0 col2" >$3,124,928</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col3" class="data row0 col3" >$628</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col4" class="data row0 col4" >77.0</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col5" class="data row0 col5" >81.0</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col6" class="data row0 col6" >66.7%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col7" class="data row0 col7" >81.9%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row0_col8" class="data row0 col8" >54.6%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col1" class="data row1 col1" >1,858</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col2" class="data row1 col2" >$1,081,356</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col3" class="data row1 col3" >$582</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col4" class="data row1 col4" >83.1</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col5" class="data row1 col5" >84.0</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col6" class="data row1 col6" >94.1%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col7" class="data row1 col7" >97.0%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row1_col8" class="data row1 col8" >91.3%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col0" class="data row2 col0" >District</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col1" class="data row2 col1" >2,949</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col2" class="data row2 col2" >$1,884,411</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col3" class="data row2 col3" >$639</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col4" class="data row2 col4" >76.7</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col5" class="data row2 col5" >81.2</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col6" class="data row2 col6" >66.0%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col7" class="data row2 col7" >80.7%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row2_col8" class="data row2 col8" >53.2%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col0" class="data row3 col0" >District</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col1" class="data row3 col1" >2,739</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col2" class="data row3 col2" >$1,763,916</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col3" class="data row3 col3" >$644</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col4" class="data row3 col4" >77.1</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col5" class="data row3 col5" >80.7</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col6" class="data row3 col6" >68.3%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col7" class="data row3 col7" >79.3%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row3_col8" class="data row3 col8" >54.3%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col1" class="data row4 col1" >1,468</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col2" class="data row4 col2" >$917,500</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col3" class="data row4 col3" >$625</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col4" class="data row4 col4" >83.4</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col5" class="data row4 col5" >83.8</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col6" class="data row4 col6" >93.4%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col7" class="data row4 col7" >97.1%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row4_col8" class="data row4 col8" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col0" class="data row5 col0" >District</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col1" class="data row5 col1" >4,635</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col2" class="data row5 col2" >$3,022,020</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col3" class="data row5 col3" >$652</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col4" class="data row5 col4" >77.3</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col5" class="data row5 col5" >80.9</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col6" class="data row5 col6" >66.8%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col7" class="data row5 col7" >80.9%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row5_col8" class="data row5 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col0" class="data row6 col0" >Charter</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col1" class="data row6 col1" >427</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col2" class="data row6 col2" >$248,087</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col3" class="data row6 col3" >$581</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col4" class="data row6 col4" >83.8</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col5" class="data row6 col5" >83.8</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col6" class="data row6 col6" >92.5%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col7" class="data row6 col7" >96.3%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row6_col8" class="data row6 col8" >89.2%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col0" class="data row7 col0" >District</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col1" class="data row7 col1" >2,917</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col2" class="data row7 col2" >$1,910,635</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col3" class="data row7 col3" >$655</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col4" class="data row7 col4" >76.6</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col5" class="data row7 col5" >81.2</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col6" class="data row7 col6" >65.7%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col7" class="data row7 col7" >81.3%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row7_col8" class="data row7 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col0" class="data row8 col0" >District</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col1" class="data row8 col1" >4,761</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col2" class="data row8 col2" >$3,094,650</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col3" class="data row8 col3" >$650</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col4" class="data row8 col4" >77.1</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col5" class="data row8 col5" >81.0</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col6" class="data row8 col6" >66.1%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col7" class="data row8 col7" >81.2%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row8_col8" class="data row8 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col0" class="data row9 col0" >Charter</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col1" class="data row9 col1" >962</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col2" class="data row9 col2" >$585,858</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col3" class="data row9 col3" >$609</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col4" class="data row9 col4" >83.8</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col5" class="data row9 col5" >84.0</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col6" class="data row9 col6" >94.6%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col7" class="data row9 col7" >95.9%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row9_col8" class="data row9 col8" >90.5%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col0" class="data row10 col0" >District</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col1" class="data row10 col1" >3,999</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col2" class="data row10 col2" >$2,547,363</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col3" class="data row10 col3" >$637</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col4" class="data row10 col4" >76.8</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col5" class="data row10 col5" >80.7</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col6" class="data row10 col6" >66.4%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col7" class="data row10 col7" >80.2%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row10_col8" class="data row10 col8" >53.0%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col0" class="data row11 col0" >Charter</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col1" class="data row11 col1" >1,761</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col2" class="data row11 col2" >$1,056,600</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col3" class="data row11 col3" >$600</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col4" class="data row11 col4" >83.4</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col5" class="data row11 col5" >83.7</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col6" class="data row11 col6" >93.9%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col7" class="data row11 col7" >95.9%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row11_col8" class="data row11 col8" >89.9%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col0" class="data row12 col0" >Charter</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col1" class="data row12 col1" >1,635</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col2" class="data row12 col2" >$1,043,130</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col3" class="data row12 col3" >$638</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col4" class="data row12 col4" >83.4</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col5" class="data row12 col5" >83.8</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col6" class="data row12 col6" >93.3%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col7" class="data row12 col7" >97.3%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row12_col8" class="data row12 col8" >90.9%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col0" class="data row13 col0" >Charter</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col1" class="data row13 col1" >2,283</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col2" class="data row13 col2" >$1,319,574</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col3" class="data row13 col3" >$578</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col4" class="data row13 col4" >83.3</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col5" class="data row13 col5" >84.0</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col6" class="data row13 col6" >93.9%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col7" class="data row13 col7" >96.5%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row13_col8" class="data row13 col8" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3level0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col0" class="data row14 col0" >Charter</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col1" class="data row14 col1" >1,800</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col2" class="data row14 col2" >$1,049,400</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col3" class="data row14 col3" >$583</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col4" class="data row14 col4" >83.7</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col5" class="data row14 col5" >84.0</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col6" class="data row14 col6" >93.3%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col7" class="data row14 col7" >96.6%</td> 
        <td id="T_44bbc48c_29fd_11e8_a664_80ce62114cc3row14_col8" class="data row14 col8" >90.3%</td> 
    </tr></tbody> 
</table> 



### Top Performing Schools (By Overall Passing Rate)


```python
#Sort by "% Overall Passing Rate"
top5_schools = school_summary.sort_values("% Overall Passing Rate", ascending=False)
top5_schools.head()

#Display top 5 performing school
top5_schools = top5_schools.head()

#Format "Top Performing Schools (By Overall Passing Rate)" DataFrame                                                                                            
top5_schools.style.format({"Total Students": "{:,}",
                             "Total School Budget": "${:,}", 
                             "Per Student Budget": "${:.0f}",
                             "Average Math Score": "{:.1f}",
                             "Average Reading Score": "{:.1f}",
                             "% Passing Math": "{:.1%}",
                             "% Passing reading": "{:.1%}",
                             "% Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_44c4e566_29fd_11e8_8149_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_44c4e566_29fd_11e8_8149_80ce62114cc3level0_row0" class="row_heading level0 row0" >Cabrera High School</th> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col0" class="data row0 col0" >Charter</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col1" class="data row0 col1" >1,858</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col2" class="data row0 col2" >$1,081,356</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col3" class="data row0 col3" >$582</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col4" class="data row0 col4" >83.1</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col5" class="data row0 col5" >84.0</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col6" class="data row0 col6" >94.1%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col7" class="data row0 col7" >97.0%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row0_col8" class="data row0 col8" >91.3%</td> 
    </tr>    <tr> 
        <th id="T_44c4e566_29fd_11e8_8149_80ce62114cc3level0_row1" class="row_heading level0 row1" >Thomas High School</th> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col1" class="data row1 col1" >1,635</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col2" class="data row1 col2" >$1,043,130</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col3" class="data row1 col3" >$638</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col4" class="data row1 col4" >83.4</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col5" class="data row1 col5" >83.8</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col6" class="data row1 col6" >93.3%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col7" class="data row1 col7" >97.3%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row1_col8" class="data row1 col8" >90.9%</td> 
    </tr>    <tr> 
        <th id="T_44c4e566_29fd_11e8_8149_80ce62114cc3level0_row2" class="row_heading level0 row2" >Griffin High School</th> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col0" class="data row2 col0" >Charter</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col1" class="data row2 col1" >1,468</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col2" class="data row2 col2" >$917,500</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col3" class="data row2 col3" >$625</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col4" class="data row2 col4" >83.4</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col5" class="data row2 col5" >83.8</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col6" class="data row2 col6" >93.4%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col7" class="data row2 col7" >97.1%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row2_col8" class="data row2 col8" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_44c4e566_29fd_11e8_8149_80ce62114cc3level0_row3" class="row_heading level0 row3" >Wilson High School</th> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col0" class="data row3 col0" >Charter</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col1" class="data row3 col1" >2,283</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col2" class="data row3 col2" >$1,319,574</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col3" class="data row3 col3" >$578</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col4" class="data row3 col4" >83.3</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col5" class="data row3 col5" >84.0</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col6" class="data row3 col6" >93.9%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col7" class="data row3 col7" >96.5%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row3_col8" class="data row3 col8" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_44c4e566_29fd_11e8_8149_80ce62114cc3level0_row4" class="row_heading level0 row4" >Pena High School</th> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col1" class="data row4 col1" >962</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col2" class="data row4 col2" >$585,858</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col3" class="data row4 col3" >$609</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col4" class="data row4 col4" >83.8</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col5" class="data row4 col5" >84.0</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col6" class="data row4 col6" >94.6%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col7" class="data row4 col7" >95.9%</td> 
        <td id="T_44c4e566_29fd_11e8_8149_80ce62114cc3row4_col8" class="data row4 col8" >90.5%</td> 
    </tr></tbody> 
</table> 



### Bottom Performing Schools (By Overall Passing Rate)


```python
#Sort by "% Overall Passing Rate"
bottom5_schools = school_summary.sort_values("% Overall Passing Rate", ascending=True)
bottom5_schools.head()

#Display bottom 5 performing school
bottom5_schools = bottom5_schools.head()

#Format "Lowest Performing Schools (By Overall Passing Rate)" DataFrame                                                                                            
bottom5_schools.style.format({"Total Students": "{:,}",
                             "Total School Budget": "${:,}", 
                             "Per Student Budget": "${:.0f}",
                             "Average Math Score": "{:.1f}",
                             "Average Reading Score": "{:.1f}",
                             "% Passing Math": "{:.1%}",
                             "% Passing reading": "{:.1%}",
                             "% Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3level0_row0" class="row_heading level0 row0" >Rodriguez High School</th> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col0" class="data row0 col0" >District</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col1" class="data row0 col1" >3,999</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col2" class="data row0 col2" >$2,547,363</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col3" class="data row0 col3" >$637</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col4" class="data row0 col4" >76.8</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col5" class="data row0 col5" >80.7</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col6" class="data row0 col6" >66.4%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col7" class="data row0 col7" >80.2%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row0_col8" class="data row0 col8" >53.0%</td> 
    </tr>    <tr> 
        <th id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3level0_row1" class="row_heading level0 row1" >Figueroa High School</th> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col0" class="data row1 col0" >District</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col1" class="data row1 col1" >2,949</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col2" class="data row1 col2" >$1,884,411</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col3" class="data row1 col3" >$639</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col4" class="data row1 col4" >76.7</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col5" class="data row1 col5" >81.2</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col6" class="data row1 col6" >66.0%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col7" class="data row1 col7" >80.7%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row1_col8" class="data row1 col8" >53.2%</td> 
    </tr>    <tr> 
        <th id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3level0_row2" class="row_heading level0 row2" >Huang High School</th> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col0" class="data row2 col0" >District</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col1" class="data row2 col1" >2,917</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col2" class="data row2 col2" >$1,910,635</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col3" class="data row2 col3" >$655</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col4" class="data row2 col4" >76.6</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col5" class="data row2 col5" >81.2</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col6" class="data row2 col6" >65.7%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col7" class="data row2 col7" >81.3%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row2_col8" class="data row2 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3level0_row3" class="row_heading level0 row3" >Hernandez High School</th> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col0" class="data row3 col0" >District</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col1" class="data row3 col1" >4,635</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col2" class="data row3 col2" >$3,022,020</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col3" class="data row3 col3" >$652</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col4" class="data row3 col4" >77.3</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col5" class="data row3 col5" >80.9</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col6" class="data row3 col6" >66.8%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col7" class="data row3 col7" >80.9%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row3_col8" class="data row3 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3level0_row4" class="row_heading level0 row4" >Johnson High School</th> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col0" class="data row4 col0" >District</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col1" class="data row4 col1" >4,761</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col2" class="data row4 col2" >$3,094,650</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col3" class="data row4 col3" >$650</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col4" class="data row4 col4" >77.1</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col5" class="data row4 col5" >81.0</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col6" class="data row4 col6" >66.1%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col7" class="data row4 col7" >81.2%</td> 
        <td id="T_44d237f6_29fd_11e8_ad10_80ce62114cc3row4_col8" class="data row4 col8" >53.5%</td> 
    </tr></tbody> 
</table> 



### Math Scores By Grade


```python
#Calculate average math scores for each grade level
ninth_math = student_file.loc[student_file["grade"] == "9th"].groupby("school")["math_score"].mean()
tenth_math = student_file.loc[student_file["grade"] == "10th"].groupby("school")["math_score"].mean()
eleventh_math = student_file.loc[student_file["grade"] == "11th"].groupby("school")["math_score"].mean()
twelfth_math = student_file.loc[student_file["grade"] == "12th"].groupby("school")["math_score"].mean()

#Create "Math Scores By Grade" DataFrame
math_scores = pd.DataFrame({"9th": ninth_math,
                            "10th": tenth_math,
                            "11th": eleventh_math,
                            "12th": twelfth_math}, columns=["9th", "10th", "11th", "12th"])

#Hide index title for aesthetics 
math_scores.index.name = " "

#Format "Math Scores By Grade" DataFrame
math_scores.style.format({"9th": "{:.1f}",
                           "10th": "{:.1f}",
                           "11th": "{:.1f}",
                           "12th": "{:.1f}"})

```




<style  type="text/css" >
</style>  
<table id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" > </th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row0_col0" class="data row0 col0" >77.1</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row0_col1" class="data row0 col1" >77.0</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row0_col2" class="data row0 col2" >77.5</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row0_col3" class="data row0 col3" >76.5</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row1_col0" class="data row1 col0" >83.1</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row1_col1" class="data row1 col1" >83.2</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row1_col2" class="data row1 col2" >82.8</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row1_col3" class="data row1 col3" >83.3</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row2_col0" class="data row2 col0" >76.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row2_col1" class="data row2 col1" >76.5</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row2_col2" class="data row2 col2" >76.9</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row2_col3" class="data row2 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row3_col0" class="data row3 col0" >77.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row3_col1" class="data row3 col1" >77.7</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row3_col2" class="data row3 col2" >76.9</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row3_col3" class="data row3 col3" >76.2</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row4_col0" class="data row4 col0" >82.0</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row4_col1" class="data row4 col1" >84.2</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row4_col2" class="data row4 col2" >83.8</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row4_col3" class="data row4 col3" >83.4</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row5_col0" class="data row5 col0" >77.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row5_col1" class="data row5 col1" >77.3</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row5_col2" class="data row5 col2" >77.1</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row5_col3" class="data row5 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row6_col0" class="data row6 col0" >83.8</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row6_col1" class="data row6 col1" >83.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row6_col2" class="data row6 col2" >85.0</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row6_col3" class="data row6 col3" >82.9</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row7_col0" class="data row7 col0" >77.0</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row7_col1" class="data row7 col1" >75.9</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row7_col2" class="data row7 col2" >76.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row7_col3" class="data row7 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row8_col0" class="data row8 col0" >77.2</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row8_col1" class="data row8 col1" >76.7</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row8_col2" class="data row8 col2" >77.5</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row8_col3" class="data row8 col3" >76.9</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row9_col0" class="data row9 col0" >83.6</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row9_col1" class="data row9 col1" >83.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row9_col2" class="data row9 col2" >84.3</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row9_col3" class="data row9 col3" >84.1</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row10_col0" class="data row10 col0" >76.9</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row10_col1" class="data row10 col1" >76.6</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row10_col2" class="data row10 col2" >76.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row10_col3" class="data row10 col3" >77.7</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row11_col0" class="data row11 col0" >83.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row11_col1" class="data row11 col1" >82.9</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row11_col2" class="data row11 col2" >83.4</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row11_col3" class="data row11 col3" >83.8</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row12_col0" class="data row12 col0" >83.6</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row12_col1" class="data row12 col1" >83.1</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row12_col2" class="data row12 col2" >83.5</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row12_col3" class="data row12 col3" >83.5</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row13_col0" class="data row13 col0" >83.1</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row13_col1" class="data row13 col1" >83.7</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row13_col2" class="data row13 col2" >83.2</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row13_col3" class="data row13 col3" >83.0</td> 
    </tr>    <tr> 
        <th id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3level0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row14_col0" class="data row14 col0" >83.3</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row14_col1" class="data row14 col1" >84.0</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row14_col2" class="data row14 col2" >83.8</td> 
        <td id="T_44fa4e28_29fd_11e8_b2f0_80ce62114cc3row14_col3" class="data row14 col3" >83.6</td> 
    </tr></tbody> 
</table> 



### Reading Scores By Grade


```python
#Calculate average reading scores for each grade level
ninth_reading = student_file.loc[student_file["grade"] == "9th"].groupby("school")["reading_score"].mean()
tenth_reading = student_file.loc[student_file["grade"] == "10th"].groupby("school")["reading_score"].mean()
eleventh_reading = student_file.loc[student_file["grade"] == "11th"].groupby("school")["reading_score"].mean()
twelfth_reading = student_file.loc[student_file["grade"] == "12th"].groupby("school")["reading_score"].mean()

#Create "Reading Scores By Grade" DataFrame
math_scores = pd.DataFrame({"9th": ninth_reading,
                            "10th": tenth_reading,
                            "11th": eleventh_reading,
                            "12th": twelfth_reading}, columns=["9th", "10th", "11th", "12th"])

#Hide index title for aesthetics 
math_scores.index.name = " "

#Format "Reading Scores By Grade" DataFrame
math_scores.style.format({"9th": "{:.1f}",
                           "10th": "{:.1f}",
                           "11th": "{:.1f}",
                           "12th": "{:.1f}"})
```




<style  type="text/css" >
</style>  
<table id="T_450732de_29fd_11e8_aa19_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" > </th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row0_col0" class="data row0 col0" >81.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row0_col1" class="data row0 col1" >80.9</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row0_col2" class="data row0 col2" >80.9</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row0_col3" class="data row0 col3" >80.9</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row1_col0" class="data row1 col0" >83.7</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row1_col1" class="data row1 col1" >84.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row1_col2" class="data row1 col2" >83.8</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row1_col3" class="data row1 col3" >84.3</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row2_col0" class="data row2 col0" >81.2</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row2_col1" class="data row2 col1" >81.4</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row2_col2" class="data row2 col2" >80.6</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row2_col3" class="data row2 col3" >81.4</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row3_col0" class="data row3 col0" >80.6</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row3_col1" class="data row3 col1" >81.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row3_col2" class="data row3 col2" >80.4</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row3_col3" class="data row3 col3" >80.7</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row4_col0" class="data row4 col0" >83.4</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row4_col1" class="data row4 col1" >83.7</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row4_col2" class="data row4 col2" >84.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row4_col3" class="data row4 col3" >84.0</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row5_col0" class="data row5 col0" >80.9</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row5_col1" class="data row5 col1" >80.7</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row5_col2" class="data row5 col2" >81.4</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row5_col3" class="data row5 col3" >80.9</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row6_col0" class="data row6 col0" >83.7</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row6_col1" class="data row6 col1" >83.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row6_col2" class="data row6 col2" >83.8</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row6_col3" class="data row6 col3" >84.7</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row7_col0" class="data row7 col0" >81.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row7_col1" class="data row7 col1" >81.5</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row7_col2" class="data row7 col2" >81.4</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row7_col3" class="data row7 col3" >80.3</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row8_col0" class="data row8 col0" >81.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row8_col1" class="data row8 col1" >80.8</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row8_col2" class="data row8 col2" >80.6</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row8_col3" class="data row8 col3" >81.2</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row9_col0" class="data row9 col0" >83.8</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row9_col1" class="data row9 col1" >83.6</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row9_col2" class="data row9 col2" >84.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row9_col3" class="data row9 col3" >84.6</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row10_col0" class="data row10 col0" >81.0</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row10_col1" class="data row10 col1" >80.6</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row10_col2" class="data row10 col2" >80.9</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row10_col3" class="data row10 col3" >80.4</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row11_col0" class="data row11 col0" >84.1</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row11_col1" class="data row11 col1" >83.4</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row11_col2" class="data row11 col2" >84.4</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row11_col3" class="data row11 col3" >82.8</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row12_col0" class="data row12 col0" >83.7</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row12_col1" class="data row12 col1" >84.3</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row12_col2" class="data row12 col2" >83.6</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row12_col3" class="data row12 col3" >83.8</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row13_col0" class="data row13 col0" >83.9</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row13_col1" class="data row13 col1" >84.0</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row13_col2" class="data row13 col2" >83.8</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row13_col3" class="data row13 col3" >84.3</td> 
    </tr>    <tr> 
        <th id="T_450732de_29fd_11e8_aa19_80ce62114cc3level0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row14_col0" class="data row14 col0" >83.8</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row14_col1" class="data row14 col1" >83.8</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row14_col2" class="data row14 col2" >84.2</td> 
        <td id="T_450732de_29fd_11e8_aa19_80ce62114cc3row14_col3" class="data row14 col3" >84.1</td> 
    </tr></tbody> 
</table> 



### Scores By School Spending


```python
#Create bins for data to be held in
bins = [0,585,615,645,675]
#Create titles for each bin
bin_names = ["<$585","$585-615","$615-645","$645-675"]
#Cut the calculation for spending per student to merged file under new column "spending_bins"
merged_file["spending_bins"] = pd.cut(merged_file["budget"]/merged_file["size"], bins, labels = bin_names)

#Group by spending bins
spending = merged_file.groupby("spending_bins")

#Calculate "Average Math Score"
av_math_score_per_bin = spending["math_score"].mean()
#Calculate "Average Reading Score"
av_reading_score_per_bin = spending["reading_score"].mean()
#Calculate "% Passing Math"
pass_math_per_bin = merged_file[merged_file["math_score"] >= 70].groupby("spending_bins")["Student ID"].count() / spending["Student ID"].count()
#Calculate "% Passing Reading"
pass_reading_per_bin = merged_file[merged_file["reading_score"] >= 70].groupby("spending_bins")["Student ID"].count() / spending["Student ID"].count()
#Calculate "% Overall Passing Rate"
pass_overall_per_bin = merged_file[(merged_file["math_score"] >= 70) & (merged_file["reading_score"] >= 70)].groupby("spending_bins")["Student ID"].count() / spending["Student ID"].count()

#Create "Scores By School Spending" DataFrame
scores_by_spending = pd.DataFrame({"Average Math Score": av_math_score_per_bin,
                                   "Average Reading Score": av_reading_score_per_bin,
                                   "% Passing Math": pass_math_per_bin,
                                   "% Passing Reading": pass_reading_per_bin, 
                                   "% Overall Passing Rate": pass_overall_per_bin}, columns=["Average Math Score",
                                                                                             "Average Reading Score",
                                                                                             "% Passing Math",
                                                                                             "% Passing Reading", 
                                                                                             "% Overall Passing Rate"])
#Hide index title for aesthetics 
scores_by_spending.index.name = " "

#Format "Scores By School Spending" DataFrame
scores_by_spending.style.format({"Average Math Score": "{:.1f}", 
                                 "Average Reading Score": "{:.1f}", 
                                 "% Passing Math": "{:.1%}", 
                                 "% Passing Reading": "{:.1%}",
                                 "% Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_451a47f6_29fd_11e8_9934_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >% Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" > </th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_451a47f6_29fd_11e8_9934_80ce62114cc3level0_row0" class="row_heading level0 row0" ><$585</th> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row0_col0" class="data row0 col0" >83.4</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row0_col1" class="data row0 col1" >84.0</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row0_col2" class="data row0 col2" >93.7%</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row0_col3" class="data row0 col3" >96.7%</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row0_col4" class="data row0 col4" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_451a47f6_29fd_11e8_9934_80ce62114cc3level0_row1" class="row_heading level0 row1" >$585-615</th> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row1_col0" class="data row1 col0" >83.5</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row1_col1" class="data row1 col1" >83.8</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row1_col2" class="data row1 col2" >94.1%</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row1_col3" class="data row1 col3" >95.9%</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row1_col4" class="data row1 col4" >90.1%</td> 
    </tr>    <tr> 
        <th id="T_451a47f6_29fd_11e8_9934_80ce62114cc3level0_row2" class="row_heading level0 row2" >$615-645</th> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row2_col0" class="data row2 col0" >78.1</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row2_col1" class="data row2 col1" >81.4</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row2_col2" class="data row2 col2" >71.4%</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row2_col3" class="data row2 col3" >83.6%</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row2_col4" class="data row2 col4" >60.3%</td> 
    </tr>    <tr> 
        <th id="T_451a47f6_29fd_11e8_9934_80ce62114cc3level0_row3" class="row_heading level0 row3" >$645-675</th> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row3_col0" class="data row3 col0" >77.0</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row3_col1" class="data row3 col1" >81.0</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row3_col2" class="data row3 col2" >66.2%</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row3_col3" class="data row3 col3" >81.1%</td> 
        <td id="T_451a47f6_29fd_11e8_9934_80ce62114cc3row3_col4" class="data row3 col4" >53.5%</td> 
    </tr></tbody> 
</table> 



### Scores By School Size


```python
#Create bins for data to be held in
bins = [0,1000,2000,5000]
#Create titles for each bin
bin_names = ["Small (<1000)","Medium (1000-2000)","Large (2000-5000)"]
#Cut size data to merged file under new column "size_bins"
merged_file["size_bins"] = pd.cut(merged_file["size"], bins, labels = bin_names)

#Group by size bins
size = merged_file.groupby("size_bins")

#Calculate "Average Math Score"
av_math_score_per_bin = size["math_score"].mean()
#Calculate "Average Reading Score"
av_reading_score_per_bin = size["reading_score"].mean()
#Calculate "% Passing Math"
pass_math_per_bin = merged_file[merged_file["math_score"] >= 70].groupby("size_bins")["Student ID"].count() / size["Student ID"].count()
#Calculate "% Passing Reading"
pass_reading_per_bin = merged_file[merged_file["reading_score"] >= 70].groupby("size_bins")["Student ID"].count() / size["Student ID"].count()
#Calculate "% Overall Passing Rate"
pass_overall_per_bin = merged_file[(merged_file["math_score"] >= 70) & (merged_file["reading_score"] >= 70)].groupby("size_bins")["Student ID"].count() / size["Student ID"].count()

#Create "Scores By School Size" DataFrame
scores_by_size = pd.DataFrame({"Average Math Score": av_math_score_per_bin,
                               "Average Reading Score": av_reading_score_per_bin,
                               "% Passing Math": pass_math_per_bin,
                               "% Passing Reading": pass_reading_per_bin, 
                               "% Overall Passing Rate": pass_overall_per_bin}, columns=["Average Math Score",
                                                                                         "Average Reading Score",
                                                                                         "% Passing Math",
                                                                                         "% Passing Reading", 
                                                                                         "% Overall Passing Rate"])
#Hide index title for aesthetics 
scores_by_size.index.name = " "

#Format "Scores By School Size" DataFrame
scores_by_size.style.format({"Average Math Score": "{:.1f}", 
                             "Average Reading Score": "{:.1f}", 
                             "% Passing Math": "{:.1%}", 
                             "% Passing Reading": "{:.1%}",
                             "% Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_453407b0_29fd_11e8_9705_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >% Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" > </th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_453407b0_29fd_11e8_9705_80ce62114cc3level0_row0" class="row_heading level0 row0" >Small (<1000)</th> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row0_col0" class="data row0 col0" >83.8</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row0_col1" class="data row0 col1" >84.0</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row0_col2" class="data row0 col2" >94.0%</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row0_col3" class="data row0 col3" >96.0%</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row0_col4" class="data row0 col4" >90.1%</td> 
    </tr>    <tr> 
        <th id="T_453407b0_29fd_11e8_9705_80ce62114cc3level0_row1" class="row_heading level0 row1" >Medium (1000-2000)</th> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row1_col0" class="data row1 col0" >83.4</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row1_col1" class="data row1 col1" >83.9</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row1_col2" class="data row1 col2" >93.6%</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row1_col3" class="data row1 col3" >96.8%</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row1_col4" class="data row1 col4" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_453407b0_29fd_11e8_9705_80ce62114cc3level0_row2" class="row_heading level0 row2" >Large (2000-5000)</th> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row2_col0" class="data row2 col0" >77.5</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row2_col1" class="data row2 col1" >81.2</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row2_col2" class="data row2 col2" >68.7%</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row2_col3" class="data row2 col3" >82.1%</td> 
        <td id="T_453407b0_29fd_11e8_9705_80ce62114cc3row2_col4" class="data row2 col4" >56.6%</td> 
    </tr></tbody> 
</table> 



### Scores By School Type


```python
#Group by school type
by_type = merged_file.groupby("type")

#Calculate "Average Math Score"
av_math_score_per_type = by_type["math_score"].mean()
#Calculate "Average Reading Score"
av_reading_score_per_type = by_type["reading_score"].mean()
#Calculate "% Passing Math"
pass_math_per_type = merged_file[merged_file["math_score"] >= 70].groupby("type")["Student ID"].count() / by_type["Student ID"].count()
#Calculate "% Passing Reading"
pass_reading_per_type = merged_file[merged_file["reading_score"] >= 70].groupby("type")["Student ID"].count() / by_type["Student ID"].count()
#Calculate "% Overall Passing Rate"
pass_overall_per_type = merged_file[(merged_file["math_score"] >= 70) & (merged_file["reading_score"] >= 70)].groupby("type")["Student ID"].count() / by_type["Student ID"].count()

#Create "Scores By School Type" DataFrame
scores_by_type = pd.DataFrame({"Average Math Score": av_math_score_per_type,
                               "Average Reading Score": av_reading_score_per_type,
                               "% Passing Math": pass_math_per_type,
                               "% Passing Reading": pass_reading_per_type, 
                               "% Overall Passing Rate": pass_overall_per_type}, columns=["Average Math Score",
                                                                                         "Average Reading Score",
                                                                                         "% Passing Math",
                                                                                         "% Passing Reading", 
                                                                                         "% Overall Passing Rate"])
#Hide index title for aesthetics 
scores_by_type.index.name = " "

#Format "Scores By School Type" DataFrame
scores_by_type.style.format({"Average Math Score": "{:.1f}", 
                             "Average Reading Score": "{:.1f}", 
                             "% Passing Math": "{:.1%}", 
                             "% Passing Reading": "{:.1%}",
                             "% Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >% Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" > </th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3level0_row0" class="row_heading level0 row0" >Charter</th> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row0_col0" class="data row0 col0" >83.4</td> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row0_col1" class="data row0 col1" >83.9</td> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row0_col2" class="data row0 col2" >93.7%</td> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row0_col3" class="data row0 col3" >96.6%</td> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row0_col4" class="data row0 col4" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3level0_row1" class="row_heading level0 row1" >District</th> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row1_col0" class="data row1 col0" >77.0</td> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row1_col1" class="data row1 col1" >81.0</td> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row1_col2" class="data row1 col2" >66.5%</td> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row1_col3" class="data row1 col3" >80.9%</td> 
        <td id="T_453e5b98_29fd_11e8_98d1_80ce62114cc3row1_col4" class="data row1 col4" >53.7%</td> 
    </tr></tbody> 
</table> 


