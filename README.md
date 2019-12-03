# SQL
SQL_Scripts

1.

SELECT 
	COLUMN_NAME
FROM 
	information_schema.columns
WHERE
	TABLE_NAME = 'naep'


Revised:


SELECT
  	Table_Name,
COLUMN_NAME

FROM
  	information_schema.columns
WHERE
 table_name = 'naep'





2. 
SELECT *
FROM naep
FETCH FIRST 50 ROWS ONLY

Revised:

SELECT *
FROM naep
LIMIT 50;

3. 
SELECT state ,Max(avg_math_4_score) as Max_score,Min(avg_math_4_score) as Min_score,count(avg_math_4_score) as Count_score,Sum(avg_math_4_score) as Score_Total, AVG(avg_math_4_score) as Average_score
,Percentile_cont(0.25) within group (Order By avg_math_4_score) as percent_25_quantile,
Percentile_cont(0.50) within group (Order By avg_math_4_score) as percent_50_quantile,
Percentile_cont(0.75) within group (Order By avg_math_4_score) as percent_75_quantile
from naep
GROUP BY State 
Order By State Asc;

REVISED:

SELECT state ,Max(avg_math_4_score) as Max_score,Min(avg_math_4_score) as Min_score,count(avg_math_4_score),Sum(avg_math_4_score) as Score_Total, AVG(avg_math_4_score) as Average_score
FROM naep
GROUP BY state 
Order BY state ASC;




4.
SELECT state ,Max(avg_math_4_score) as Max_score,Min(avg_math_4_score) as Min_score,count(avg_math_4_score) as Count_score,Sum(avg_math_4_score) as Score_Total, AVG(avg_math_4_score) as Average_score,Percentile_cont(0.25) within group (Order By avg_math_4_score) as percent_25_quantile,Percentile_cont(0.50) within group (Order By avg_math_4_score) as percent_50_quantile,Percentile_cont(0.75) within group (Order By avg_math_4_score) as percent_75_quantile  
from naep
GROUP BY State Order By State Asc;
SELECT state , Max(avg_math_4_score),Min(avg_math_4_score) 
from naep
Group By state
Having Max(avg_math_4_score) > 30 and Min(avg_math_4_score) > 30
order By state;

REVISED:

SELECT state ,Max(avg_math_4_score) as Max_score,Min(avg_math_4_score) as Min_score,count(avg_math_4_score),Sum(avg_math_4_score) as Score_Total, AVG(avg_math_4_score) as Average_score
FROM naep
GROUP BY state Having Max(avg_math_4_score) - Min(avg_math_4_score) > 30
Order BY state ASC;



5.
SELECT year,state, avg_math_4_score as bottom_10
from naep
where year = 2000
order by avg_math_4_score asc
Limit 10

REVISED:

SELECT state as bottom_10_states
FROM naep
WHERE year = 2000
ORDER BY avg_math_4_score
LIMIT 10


6.
SELECT ROUND(avg(avg_math_4_score),2) As Average_2000
from naep
where year = 2000

REVISED:

SELECT ROUND(avg(avg_math_4_score),2) AS Average_2000
FROM naep
WHERE year = 2000


7.

SELECT state,avg_math_4_score as below_average_states_y2000
from naep
where avg_math_4_score < 224.80 and year = 2000

Revised(1):

SELECT state
from naep
where year =2000 and avg_math_4_score <(SELECT AVG(avg_math_4_score)from naep where year =2000)


Revised(2):

SELECT state AS below_average_states_y2000
FROM naep
WHERE year =2000
AND avg_math_4_score <(SELECT AVG(avg_math_4_score)FROM naep WHERE YEAR =2000)



8. 

SELECT state

from naep

where  avg_math_4_score is NULL and year = 2000; 

REVISED:

SELECT state as scores_missing_y2000
FROM naep
WHERE  avg_math_4_score is NULL and year = 2000;



9.

select 
naep.state,
Round(naep.avg_math_4_score,2) As avg_math_4_score,
finance.total_expenditure
From naep
left join
finance
on 
naep.id = finance.id
Where naep.year = 2000 and avg_math_4_score is not NULL
order BY finance.total_expenditure DESC;

REVISED:

SELECT 
naep.state,
Round(avg_math_4_score,2) AS avg_math_4_score,
finance.total_expenditure
FROM naep
LEFT OUTER JOIN
finance
ON 
naep.id = finance.id
WHERE naep.year = 2000 and avg_math_4_score IS NOT NULL
ORDER BY finance.total_expenditure DESC;
