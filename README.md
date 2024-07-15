SQL Question

<img width="960" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/4a4732cd-c527-4674-84e6-49fa70f6472f">

Solution

The SQL query aims to select a subset of image predictions from the unlabeled_image_predictions table. Here's a concise breakdown of what each part of the query does:

Common Table Expressions (CTEs):

posi: This CTE selects rows from unlabeled_image_predictions, assigns a row number (rno) based on descending order of score, and renames score as weak_label.

nega: This CTE selects rows similarly to posi, but assigns row numbers based on ascending order of score.


Main Query:

The main query selects image_id and weak_label from the combined results of two subqueries (one from posi and one from nega):

From posi, it selects rows where rno is divisible by 3 and less than or equal to 30,000.

From nega, it selects rows where rno is divisible by 3 and less than or equal to 30,000.


Union Operation:

The UNION operation combines the results of the above two subqueries, ensuring unique rows (removes duplicates).


Final Output:
The result is ordered by image_id and includes image_id and weak_label (rounded to the nearest whole number).


output screenshot

<img width="1433" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/9fc1c3c8-f44c-471f-935b-cb6a06b6ddbe">
<img width="1440" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/d7b5d220-eec7-4f9f-9ec2-495e2b686c1c">



ER - diagram

Question: Make the ER diagram for the following tables and relations

<img width="677" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/4832b6a5-0335-47c1-895d-620e785efe5f">

Solution
<img width="1440" alt="image" src="https://github.com/naman-dwivedi1/DB_Practice/assets/174860133/15884927-1fb3-46d2-be86-3439418b8355">
