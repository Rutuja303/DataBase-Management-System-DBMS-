# Task 3


Below are few of the SQL queries.



```bash
CREATE TABLE join_table AS
SELECT accounts.username, items.type, accounts_items.quality, items.name FROM items
INNER JOIN accounts_items
ON items.id = accounts_items.item_id
INNER JOIN accounts
ON accounts.id = accounts_items.account_id;

SELECT * FROM join_table;
```



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



```bash
SELECT username, type, quality, GROUP_CONCAT(name SEPARATOR ', ') AS combined_name
FROM rank_table
WHERE rn = 1
GROUP BY username, type, quality
ORDER BY username, type;
```
