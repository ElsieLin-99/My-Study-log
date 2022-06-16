*My code example here is just a reference and may not be the most efficient and concise.*  
# Study Notes for MySQL
## Date
#197 Rising Temperature (Easy)  
#1251 Average Selling Price (Easy)  
#1607 Sellers With No Sales (Easy)  
#1294 Weather Type in Each Country (Easy)  
  

**Tips:**
1. Sometimes using "BETWEEN date1 AND date2" will run faster than using "expr <= date1 and expr >= date2"  
2. [DATEDIFF(expr1,expr2)](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_datediff)  
DATEDIFF() returns expr1 − expr2 expressed as a value in days from one date to the other. expr1 and expr2 are date or date-and-time expressions. Only the date parts of the values are used in the calculation.  
Sometimes using DATEDIFF(current_date, compared_date) < certain_days to judge whether the date is within certain day range is better than manually calculating date ranges.  
3. [DATE_FORMAT()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format)  
Use the DATE_FORMAT to convert the date to string to compare. For example, in #1607: DATE_FORMAT(sale_date,'%Y') = '2020'  


## Group By VS. Partition
### Group By
#1174 Immediate Food Delivery II (Medium)  
#1251 Average Selling Price (Easy)  
#1571 Warehouse Manager (Easy)  
#578 Get Highest Answer Rate Question (Medium)  
#574 Winning Candidate (Medium)  
#614 Second Degree Follower (Medium)  
#1077 Project Employees III (Medium)  
#1303 Find the Team Size (Easy)  
#580 Count Student Number in Departments (Medium)  
  

Problem #1303 can be done using group by or partition by.  
If using group by: Runtime: 312 ms, faster than 43.13% of MySQL online submissions for Find the Team Size.  
```
with cte as (select team_id, count(*) as team_size
from Employee
group by team_id)
select employee_id, team_size
from Employee e join cte on e.team_id = cte.team_id;
```
  
If using partition by: Runtime: 316 ms, faster than 41.02% of MySQL online submissions for Find the Team Size.  
```
SELECT employee_id, COUNT(employee_id) OVER(PARTITION BY team_id) team_size
FROM Employee;
```
  
### Partition By (Window Funtion)
[MySQL official website about Window Function](https://dev.mysql.com/doc/refman/8.0/en/window-functions-usage.html)  
*"A window function performs an aggregate-like operation on a set of query rows. However, whereas an aggregate operation groups query rows into a single result row, a window function produces a result for each query row:*  
*The row for which function evaluation occurs is called the **current row**.*  
*The query rows related to the current row over which function evaluation occurs **comprise the window for the current row**."*  
  
[Lag()](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html#function_lag)  

```
Window_function ( expression ) Over ( partition by expr [order_clause] [frame_clause] )
```
  
expr can be column names or built-in functions in MySQL.  
  
Window_function:  
[dense-rank](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html#function_dense-rank):  
#185 Department Top Three **(Unique)** Salaries (Hard)  
  
  
#1699 Number of Calls Between Two Persons (Medium)  
#1454 Active Users (Medium)  
#178 Rank Score (Medium)  
#534 Game Play Analysis III (Medium)  
#1308 Running Total for Different Genders (Medium)  
#1303 Find the Team Size (Easy)  
#185 Department Top Three Salaries (Hard)  

  
For example:
LeetCode #1308: Write an SQL query to find the total score for each gender on each day.  
Input:   
Scores table:  
+-------------+--------+------------+--------------+  
| player_name | gender | day        | score_points |  
+-------------+--------+------------+--------------+  
| Aron        | F      | 2020-01-01 | 17           |  
| Alice       | F      | 2020-01-07 | 23           |  
| Bajrang     | M      | 2020-01-07 | 7            |  
| Khali       | M      | 2019-12-25 | 11           |  
| Slaman      | M      | 2019-12-30 | 13           |  
| Joe         | M      | 2019-12-31 | 3            |  
| Jose        | M      | 2019-12-18 | 2            |  
| Priya       | F      | 2019-12-31 | 23           |  
| Priyanka    | F      | 2019-12-30 | 17           |  
+-------------+--------+------------+--------------+  
Output should be:   
+--------+------------+-------+  
| gender | day        | total |  
+--------+------------+-------+  
| F      | 2019-12-30 | 17    |  
| F      | 2019-12-31 | 40    |  
| F      | 2020-01-01 | 57    |  
| F      | 2020-01-07 | 80    |  
| M      | 2019-12-18 | 2     |  
| M      | 2019-12-25 | 13    |  
| M      | 2019-12-30 | 26    |  
| M      | 2019-12-31 | 29    |  
| M      | 2020-01-07 | 36    |  
+--------+------------+-------+  
If I use *group by*: I was calculating the total score for each gender for each day.
```
select gender, day, sum(score_points) as total
from Scores
group by gender, day
order by gender asc, day asc;
```
![image](https://user-images.githubusercontent.com/95600628/172246818-ccd49e00-d96f-41af-979c-8759e381ecf1.png)

The correct one should use *partiion by* to calculate the accumulated total score for each gender on each day.
```
select gender, day, sum(score_points) over (partition by gender order by gender, day) as total
from Scores;
```

### IFNULL(), ISNULL(), NULLIF(), IF()
#1173 Immediate Food Delivery I (Easy)  
#1699 Number of Calls Between Two Persons (Medium)  
#1193 Monthly Transactions I (Medium)  
#1280 Students and Examinations (Easy)  

**Tips:**  
Sometimes if the result is null, we need to return 0 or vice versa. These funtions can be really helpful.  

### IN, NOT IN - The set problem
#607 Sales Person (Easy)  
#1264 Page Recommendations (Medium)  
#1398 Customers Who Bought Products A and B but Not C (Medium）  
#184 Department Highest Salary (Medium)  

**Tips:**  
1. Use [GROUP_CONCAT](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_group-concat) for #1398.

### CASE
#1211 Queries Quality and Percentage (Easy)  

**Tips:**(Some tricks to use case)  
1. sum(case when condition then 1 else 0 end): count the number of each condition when there're too many conditions  
2. select case whhen condition then result end as new_column_name: to alter table structure  
  
### JOIN
#1077 Project Employees III (Medium)  
#1440 Evaluate Boolean Expression (Medium)  
#1280 Students and Examinations (Easy)  
#1501 Countries You Can Safely Invest In (Medium)  
#184 Department Highest Salary (Medium)  
#580 Count Student Number in Departments (Medium)  
  
  
# Some Typical Problems
## Consecutive Problems / Adjacent Problems
#180 Consecutive Numbers (Medium)  
#1454 Active Users (Medium)  
#601 Human Traffic of Stadium (Hard)  
#626 Exchange Seats (Medium)  

## Get Nth Highest
#1076 Project Employees II (Easy)  
#1077 Project Employees III (Medium)  

## Get the top n data
#619 Biggest Single Number (Easy)  


