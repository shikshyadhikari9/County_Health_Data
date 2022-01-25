```python
import pandas as pd
```

### Exercise with MYSQL

In this exercise, I used MySQL to filter and combine information I wanted from the County Health Rankings National Data. I sincerely thank Philip Gigliotti for pointing me to the dataset. https://www.countyhealthrankings.org/explore-health-rankings/rankings-data-documentation

If you download '2021 County Health Rankings National Data' dataset, you will get a very comprehensive dataset that includes a lot of information on each county in the United States. I am interested in exploring health trends in counties that can contribute to better marketing of health insurance policies that could potentially be useful for health insurance companies. 

Keeping the question in mind, I undertook the following steps to derive a dataset that would help me solve the above problem. I used SQL to do the initial cleaning, filtering, and joining datasets. I am going to use Python to do additional wrangling and exploratory data analysis. 

1. I extracted the 'Ranked Measure Data and 'Additional Measure Data' tab. 
2. I saw that it had a lot of 'blanks', as in missing values. This will be problematic when you import the table into SQL. SQL will convert the columns with the Blanks into 'txt' datatype although your column is numeric. Therefore, I used Excel to replace all the blanks with -99. The dataset also had 'x' in the columns which I took to be no information as well. I also changed that to -99. MYSQL will not import rows that it doesn't like resulting in missing data. Therefore, it is necessary to explore your data well before importing. 
3. I then converted the two excel files into separate csv files. The encoding is important here. Excel saves it with UTF-B BOM encoding, which MySQL did not like. Therefore, I opened the files with VSCODE and changed the encoding to UTF. 
4. I created a schema in MySQL called 'County Health Data' and imported the two tables. I checked the datatype of all the columns to make sure that it is what is should be. 
5. The next step was to replace all the '-99s' with NULL so that we can run Null value operations with the data. I did this for one table, which was pretty time intensive as the datasets had a lot of columns. I am going to do the rest in python. I used this query: 

UPDATE tablename
SET columnname = NULL
WHERE columnname = -99;

6. I then joined the two tables. I used the JOIN command because I wanted columns/rows from both the tables. Here is my query. I selected the columns of interest to me at this stage. 

-- CREATE VIEW county_data
-- AS
-- SELECT ranked.FIPS, ranked.State, ranked.County, Deaths, `% Fair or Poor Health`, `Average Number of Physically Unhealthy Days`, `Average Number of Mentally Unhealthy Days`,
-- `% Low birthweight`, `% Smokers`, `% Adults with Obesity`, `% Physically Inactive`, `% With Access to E-99ercise Opportunities`,
-- `% E-99cessive Drinking`, `% Driving Deaths with Alcohol Involvement`, `Chlamydia Rate`, `Teen Birth Rate`, ranked.`# Uninsured`, ranked.`% Uninsured`,
-- `# Primary Care Physicians`, `# Dentists`, `% With Annual Mammogram`, `% Vaccinated`, `# Completed High School`, `# Some College`, `# Unemployed`,
-- `% Children in Poverty`, `80th Percentile Income`, `20th Percentile Income`, `# Children in Single-Parent Households`,
-- `# Children in Households`, `Violent Crime Rate`, `Injury Death Rate`, `% Severe Housing Problems`, `Life E-99pectancy`, `Age-Adjusted Death Rate`, `Child Mortality Rate`, 
-- `Infant Mortality Rate`, `% Adults with Diabetes`, `HIV Prevalence Rate`, `# Food Insecure`, `# Limited Access`, `# Drug Overdose Deaths`, `# Motor Vehicle Deaths`,
-- `Median Household Income`, `Homicide Rate`, `Firearm Fatalities Rate`, `# Homeowners`, `# Households with Severe Cost Burden`, `% Broadband Access`,
-- additional.`Population`, `% Less Than 18 Years of Age`, `% 65 and Over`, `# Black`, `# American Indian & Alaska Native`, `# Asian`, 
-- `# Native Hawaiian/Other Pacific Islander`, `# Hispanic`, `# Non-Hispanic White`, `# Not Proficient in English`, `% Female`, `# Rural`
-- FROM ranked
-- 	JOIN additional
-- 	ON ranked.FIPS = additional.FIPS

7. I then exported the table to csv. 




```python
county = pd.read_csv('county_data.csv')
```


```python
county.head()
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
      <th>FIPS</th>
      <th>State</th>
      <th>County</th>
      <th>Deaths</th>
      <th>% Fair or Poor Health</th>
      <th>Average Number of Physically Unhealthy Days</th>
      <th>Average Number of Mentally Unhealthy Days</th>
      <th>% Low birthweight</th>
      <th>% Smokers</th>
      <th>% Adults with Obesity</th>
      <th>...</th>
      <th>% 65 and Over</th>
      <th># Black</th>
      <th># American Indian &amp; Alaska Native</th>
      <th># Asian</th>
      <th># Native Hawaiian/Other Pacific Islander</th>
      <th># Hispanic</th>
      <th># Non-Hispanic White</th>
      <th># Not Proficient in English</th>
      <th>% Female</th>
      <th># Rural</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>Alabama</td>
      <td>Autauga</td>
      <td>787.0</td>
      <td>20</td>
      <td>4.5</td>
      <td>4.9</td>
      <td>9.0</td>
      <td>20</td>
      <td>33</td>
      <td>...</td>
      <td>16.0</td>
      <td>11098</td>
      <td>266</td>
      <td>656</td>
      <td>58</td>
      <td>1671</td>
      <td>41215</td>
      <td>419</td>
      <td>51.5</td>
      <td>22921</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1003</td>
      <td>Alabama</td>
      <td>Baldwin</td>
      <td>3147.0</td>
      <td>16</td>
      <td>3.6</td>
      <td>4.8</td>
      <td>8.0</td>
      <td>19</td>
      <td>30</td>
      <td>...</td>
      <td>21.0</td>
      <td>19215</td>
      <td>1742</td>
      <td>2380</td>
      <td>154</td>
      <td>10534</td>
      <td>185747</td>
      <td>1425</td>
      <td>51.5</td>
      <td>77060</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1005</td>
      <td>Alabama</td>
      <td>Barbour</td>
      <td>515.0</td>
      <td>30</td>
      <td>5.6</td>
      <td>5.6</td>
      <td>11.0</td>
      <td>26</td>
      <td>41</td>
      <td>...</td>
      <td>19.7</td>
      <td>11807</td>
      <td>170</td>
      <td>116</td>
      <td>52</td>
      <td>1117</td>
      <td>11235</td>
      <td>454</td>
      <td>47.1</td>
      <td>18613</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1007</td>
      <td>Alabama</td>
      <td>Bibb</td>
      <td>476.0</td>
      <td>24</td>
      <td>4.9</td>
      <td>5.3</td>
      <td>10.0</td>
      <td>23</td>
      <td>37</td>
      <td>...</td>
      <td>16.7</td>
      <td>4719</td>
      <td>103</td>
      <td>48</td>
      <td>26</td>
      <td>623</td>
      <td>16663</td>
      <td>71</td>
      <td>46.7</td>
      <td>15663</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1009</td>
      <td>Alabama</td>
      <td>Blount</td>
      <td>1100.0</td>
      <td>22</td>
      <td>5.0</td>
      <td>5.4</td>
      <td>7.0</td>
      <td>23</td>
      <td>33</td>
      <td>...</td>
      <td>18.7</td>
      <td>872</td>
      <td>370</td>
      <td>185</td>
      <td>67</td>
      <td>5582</td>
      <td>50176</td>
      <td>878</td>
      <td>50.8</td>
      <td>51562</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 61 columns</p>
</div>




```python

```
