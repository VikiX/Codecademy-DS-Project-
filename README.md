# Codecademy-DS-Project # 
DS-  Education &amp; Census Data Analysis 
Education & Census Data Analysis 

Census data can be found [here](https://docs.google.com/spreadsheets/d/1NAgjKKhdGrvwwlc0aoH4JvjrScsytst0g_cVCsdX0Jk/edit#gid=1774413306)
Public High School information can be found [here](https://docs.google.com/spreadsheets/d/1EyKaewf2Oyhh_Qfmn_csZxxC1ypkb5oPsqMFfJTlndE/edit#gid=274575715)

# **Basic Requirements**

## 1.How many public high schools are in each zip code? in each state?
```
select zip_code, count(*) as num_hs_by_zip_code
from public_highschool
group by zip_code
order by count(*) desc; 
```

```
select state_code, count(*) as num_hs_by_state_code
from public_highschool
group by state_code
order by count(*) desc; 
````

## 2.The locale_code column in the high school data corresponds to various levels of urbanization as listed below. Use the CASE statement to display the corresponding locale_text and locale_size in your query result.


|locale_text||locale_code(locale_size)  ||
| ------|-------------------| ------| ------|
|City|11 (Large)| 12 (Midsize)|13 (Small)|
Suburb |	21 (Large)| 22 (Midsize)|23 (Small)|
Town |	31 (Fringe) | 32 (Distant)|33 (Remote)|
Rural	| 41 (Fringe) | 42 (Distant) | 43 (Remote)|
  
 ```
 with A as 
(select school_id, school_name, locale_code, 
(case when locale_code= 11  or 12 or 13 then 'City' 
when locale_code =21 or  22 or 23 then 'Subrub' 
when locale_code =31 or 32 or 33 then 'Town' 
else 'Rural' end) as local_txt
from public_highschool) 

select A.school_id, A.school_name, A.locale_code, A.local_txt, 
(case when A.locale_code =11 or 21 then 'Large' 
when A.locale_code = 12 or 22 then 'Midsize'
when A.locale_code = 13 or 23 then 'Small' 
when A.locale_code = 31 or 41 then 'Fringe' 
when A.locale_code = 32 or 42 then 'Distant' 
else 'Remote' end) as local_size
from A;
``` 

## 3. What is the minimum, maximum, and average median_household_income of the nation? for each state?

```
select ifnull(min(median_household_income),0) as minimum, max(median_household_income) as maximium, 
round(avg(median_household_income),2) as average from census_data
where median_household_income is not NULL
and median_household_income is not 'NULL';


``` 
| mininum   |maximium | average |
| -------- |--------|--------|
| 2499  | 250001 | 54683.12 | 

```
select state_code, ifnull(min(median_household_income),0) as minimum, max(median_household_income) as maximium, 
round(avg(median_household_income),2) as average from census_data
where median_household_income is not NULL
and median_household_income is not 'NULL'
group by state_code
order by average DESC; 
```
|state | mininum   |maximium | average |
| -------- |--------|--------|--------|
|NJ|	15855|	250001 |	87140.49|
|CT|	11755	|218152 |	84020.73|
|MD|	20341 |	250001	|81827.66|
|DC|	30665	|165425	|81712.13|
|MA|	2499	|191744	|78934.98|
|RI|	28901|	230052	|72048.03|
|NH|	21802	|175714	|69985.63|
|CA|	2499	|250001	|65724.77|
|NY|	12454	|250001	|65674.66|
|HI|	26719	|121307	|63464.97|
|DE|	26810	|140400	|62800.41|
|ND|	24211|	130625	|61041.61|
|VA|	13375|	215122	|60531.46|
|UT|	22986| 127847	|59422.65|
|CO|	13750	|194750	|58976.2|
|MN|	19198	|161500	|58601.27|
|WA|12500	|182604	|58387.94|
|WY|	23516	|97933	|58284.61|
|IL| 10268	|211023	|57985.58|
|NV|	23477	|140523	|57735.2|


# **Intermediate Challenge**
## 4. On average, do students perform better on the math or reading exam? Find the number of states where students do better on the math exam, and vice versa.

# **Advance Challenge**
## 5. What is the average proficiency on state assessment exams for each zip code, and how do they compare to other zip codes in the same state?
