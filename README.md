# Codecademy-Public High School # 
DS-  Education &amp; Census Data Analysis 
Education & Census Data Analysis 

Census data can be found [here](https://docs.google.com/spreadsheets/d/1NAgjKKhdGrvwwlc0aoH4JvjrScsytst0g_cVCsdX0Jk/edit#gid=1774413306)
Public High School information can be found [here](https://docs.google.com/spreadsheets/d/1EyKaewf2Oyhh_Qfmn_csZxxC1ypkb5oPsqMFfJTlndE/edit#gid=274575715)

# **Basic Requirements**

## 1.How many public high schools are in each zip code? in each state?
```
select zip_code, state_code, count(*) as num_hs_by_zip_code
from public_hs_data
group by zip_code
order by count(*) desc
limit 10; 
```
|zip_code|state_code|num_hs_by_zip_code|
|-------|----|--------|
|10002|	NY|	11|
|11101|	NY|	10
|10456|	NY|	9|
|10473|	NY|	9|
|11236|	NY|	9|
|60623|	IL|	9|
|10011|	NY|	8|
|10019|	NY|	8|
|10457|	NY|	8|
|10463|	NY|	8|
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
```
select round(avg(pct_proficient_math),2) as average_math_score, round(avg(pct_proficient_reading),2) as average_reading_score
from public_hs_data; 
```

On average, students perform better in the reading exam.
|average_math_score | average_reading_score| 
| -------- |--------| 
|46.31|57.42|

```
select ROW_number() over (order by state_code) as id, state_code, 
round(avg(pct_proficient_math),2) as state_avg_math_score,  round(avg(pct_proficient_reading),2) as state_avg_math_score
from public_hs_data
group by state_code having avg(pct_proficient_math) >avg(pct_proficient_reading); 
``` 

Below 6 states students perform better on the math exam. 
|id|state_code| state_avg_math_score| state_avg_math_score
| -------- |--------| -------- |--------| 
|1|	IA|	83.61|	79.95|
|2|	IN|	80.89|	77.19|
|3|	MD|	85.16|	81.24|
|4|	SC|	84.19| 76.53|
|5|	TX|	73.5|	69.51|
|6|	WY|	36.71|	30.34|

```
Select count(*) as num_of_state_w_reading_higher
from 

(select  ROW_number() over (order by state_code) as id, state_code, round(avg(pct_proficient_math),2) as state_avg_math_score,  round(avg(pct_proficient_reading),2) as state_avg_math_score
from public_hs_data
group by state_code having avg(pct_proficient_math) < avg(pct_proficient_reading)); 

```
There are 46 states have higher average reading score
|num_of_state_w_reading_higher|
|-------|
46

There are total 55 states in US. 46 states have higher reading score, 6 states have higher math score. Below 3 states have NULL value and not applicable for this analysis. 
```
WITH A as 
(select ROW_number() over (order by state_code) as id, state_code, round(avg(pct_proficient_math),2) as state_avg_math_score,  round(avg(pct_proficient_reading),2) as state_avg_math_score
from public_hs_data
group by state_code having avg(pct_proficient_math) < avg(pct_proficient_reading)),

B as 
(select ROW_number() over (order by state_code) as id, state_code, round(avg(pct_proficient_math),2) as state_avg_math_score,  round(avg(pct_proficient_reading),2) as state_avg_math_score
from public_hs_data
group by state_code having avg(pct_proficient_math) > avg(pct_proficient_reading)), 

C as (select distinct state_code from public_hs_data),

T as (select A.state_code from A 
union 
select B.state_code from B) 

select C.state_code, T.state_code 
from C left join T 
on C.state_code = T.state_code
where T.state_code is NULL;
``` 
|state_code|
|-----|
|NV|	
|BI|
|GU|	

# **Advance Challenge**
## 5. What is the average proficiency on state assessment exams for each zip code, and how do they compare to other zip codes in the same state?

Below code assess proficiency level by zip code and assign ranks within each state. Take New York state as an example.   

```
select dense_rank() over(PARTITION by state_code order by proficiency DESC) as rank, T.zip_code, T.state_code, T.proficiency
from 
(select zip_code, state_code, round(avg((pct_proficient_math+ pct_proficient_reading)/2),2) as proficiency 
from public_hs_data 
group by zip_code, state_code) as T 
where state_code = 'NY'
limit 53; 
```
|rank|zip_code|state_code|proficiency|
|------|------|------|------|
|1|10282|NY|99.5|
|1|10514|NY|99.5|
|1|10994|NY|99.5|
|1|11020|NY|99.5|
|1|11530|NY|99.5|
|1|11554|NY|99.5|
|1|11566|NY|99.5|
|1|11580|NY|99.5|
|1|11710|NY|99.5|
|1|11725|NY|99.5|
|1|11752|NY|99.5|
|1|11791|NY|99.5|
|1|12054|NY|99.5|
|1|12110|NY|99.5|
|1|12309|NY|99.5|
|1|12866|NY|99.5|
|1|13104|NY|99.5|
|1|14086|NY|99.5|
|1|14450|NY|99.5|
|1|14526|NY|99.5|
|1|14617|NY|99.5|
|2|10901|NY|98.75|
|2|10956|NY|98.75|
|2|11210|NY|98.75|
|2|11572|NY|98.75|
|2|11733|NY|98.75|
|2|11768|NY|98.75|
|2|11787|NY|98.75|
|2|11788|NY|98.75|
|2|11803|NY|98.75|
|2|12061|NY|98.75|
|2|12085|NY|98.75|
|2|12533|NY|98.75|
|2|13031|NY|98.75|
|2|14127|NY|98.75|
|2|14559|NY|98.75|
|3|10583|NY|98.5|
|3|11746|NY|98.5|
|4|11003|NY|98.25|
|5|11756|NY|98.17|
|6|11758|NY|98.12|
|7|10917|NY|98|
|7|11050|NY|98|
|7|11375|NY|98|
|7|11779|NY|98|
|7|11780|NY|98|
|7|12065|NY|98|
|7|13039|NY|98|
|7|14467|NY|98|
|7|14624|NY|98|
|8|11040|NY|97.92|
|9|14075|NY|97.88|
|10|14580|NY|97.87|

