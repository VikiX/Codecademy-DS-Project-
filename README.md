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

  
  ##### a.City: 11 (Large), 12 (Midsize), 13 (Small)
  ##### b. Suburb：21 (Large), 22 (Midsize), 23 (Small)
  ##### c. Town:　31 (Fringe), 32 (Distant), 33 (Remote)
  ##### d. Rural：41 (Fringe), 42 (Distant), 43 (Remote)

## 3. What is the minimum, maximum, and average median_household_income of the nation? for each state?

# **Intermediate Challenge**
## 4. On average, do students perform better on the math or reading exam? Find the number of states where students do better on the math exam, and vice versa.

# **Advance Challenge**
## 5. What is the average proficiency on state assessment exams for each zip code, and how do they compare to other zip codes in the same state?
