# SQL-data-cleaning

## Project Description

This is an educational project on data cleaning and preparation using SQL. The original database in CSV format is located in the file club_member_info.csv. Here, we will explore the steps that need to be applied to obtain a cleansed version of the dataset.

Get the ten first rows from the table using SQL console:
```sql
SELECT *
FROM club_member_info
LIMIT 10;
```

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

## Step 1: Duplicating Original Table
### Step 1.1: Create a new table for cleaning
Generate a new table where we can manipulate and restructure the data without modifying the original set.
```sql
CREATE TABLE club_member_info_cleaned (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);
```
### Step 1.2: Copy all values from the original table
```sql
INSERT INTO club_member_info_cleaned
SELECT * FROM club_member_info;
```

## Step 2: Clean the data
### Step 2.1: Standardize text casing

Make full_name, martial_status, email, job_title lowercase and trim whitespace.
```sql
UPDATE club_member_info_cleaned
SET full_name = TRIM(LOWER(full_name)),
    martial_status = TRIM(LOWER(martial_status)),
    email = TRIM(LOWER(email)),
    job_title = TRIM(LOWER(job_title));
```

### Step 2.2: Clean phone numbers
Trim spaces (if any).
```sql
UPDATE club_member_info_cleaned
SET phone = TRIM(phone);
```

### Step 2.3: Set unrealistic age to NULL
Assume valid ages are between 18 and 80, and mark the age as unknown so it can be handled later.
```sql
UPDATE club_member_info_cleaned
SET age = NULL
WHERE age NOT BETWEEN 18 AND 90;
```

### Step 2.4: Fill missing or blank values
Standardize missing marital status (e.g., NULL or '') as unknown:
```sql
UPDATE club_member_info_cleaned
SET martial_status = 'unknown'
WHERE martial_status IS NULL OR TRIM(martial_status) = '';
```
Set missing job titles to unemployed:
```sql
UPDATE club_member_info_cleaned
SET job_title = 'unemployed'
WHERE job_title IS NULL OR TRIM(job_title) = '';
```

## Cleaned Data Preview
```sql
SELECT *
FROM club_member_info_cleaned
LIMIT 10;
```

|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|assistant professor|7/31/2013|
|rock cradick|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|programmer iii|5/27/2018|
|sydel sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|budget/accounting analyst i|10/6/2017|
|constantin de la cruz|35|unknown|co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|desktop support technician|10/20/2015|
|gaylor redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|legal assistant|5/29/2019|
|wanda del mar|44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|human resources assistant iv|3/24/2015|
|joann kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|accountant iv|4/17/2013|
|joete cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|research nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|systems administrator iii|3/12/1921|
|fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|chemical engineer|11/5/2014|

