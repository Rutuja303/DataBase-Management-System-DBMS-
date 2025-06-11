# Task 3

The goal of task 3 is to write SQL query that advise the best quality item avalable in the inventory. We divided this task into four steps:

Step 1: Perform  JOIN operations in order to get the desired columns.

```bash
CREATE TABLE join_table AS
SELECT accounts.username, items.type, accounts_items.quality, items.name FROM items
INNER JOIN accounts_items
ON items.id = accounts_items.item_id
INNER JOIN accounts
ON accounts.id = accounts_items.account_id;

SELECT * FROM join_table;
```

![App Screenshot](https://github.com/Rutuja303/DataBase-Management-System-DBMS-/blob/main/Task%203/Join_Table.jpg)

Step 2: There are three qualities ranked as epic>rare>common. Give each quality a index or value in order to get the highest quality item.

```bash
CREATE TABLE p1 AS
SELECT username, type, quality, name , 
	CASE quality 
		WHEN quality  = 'common' THEN 3
    	WHEN quality  = 'rare' THEN 2
    	WHEN quality  = 'epic' THEN 1
    	ELSE 0
	END AS quality_rank 
FROM join_table;

SELECT * FROM p1;
```
![App Screenshot](https://github.com/Rutuja303/DataBase-Management-System-DBMS-/blob/main/Task%203/p1.jpg)

Step 3: Now we have an extra column representing each quality a value. 

```bash
CREATE TABLE rank_table AS
SELECT 
	username, 
    type, 
    quality, 
    name, 
    quality_rank,
	RANK()OVER(PARTITION BY p1.username, p1.type ORDER BY p1.quality_rank ASC) AS rn
FROM p1
ORDER BY username;

SELECT * FROM rank_table;
```
![App Screenshot](https://github.com/Rutuja303/DataBase-Management-System-DBMS-/blob/main/Task%203/Rank_Table.jpg)




```bash
SELECT username, type, quality, GROUP_CONCAT(name SEPARATOR ', ') AS combined_name
FROM rank_table
WHERE rn = 1
GROUP BY username, type, quality
ORDER BY username, type;
```
![App Screenshot](https://github.com/Rutuja303/DataBase-Management-System-DBMS-/blob/main/Task%203/Final_table.jpg)
