# Task 1
## SQL Query

### GOAL
- Create positive samples -> rows with high scores (stored in descending order).
- Create negative samples -> rows with low scores (stored in ascending order).
- Select every 3rd row in both cases.
- limit to 10,000 rows in each sample.
- Combine them using UNION ALL.

Here is the SQL Query for the Task 1:

```bash
  CREATE TABLE positive_samples AS
SELECT image_id, score
FROM (
	SELECT image_id, score,
	ROW_NUMBER() OVER(order by score DESC) as row_num
	FROM unlabeled_image_predictions 
) AS ans
WHERE row_num % 3 = 0
Limit 10000;

CREATE TABLE negative_samples AS
SELECT image_id, score
FROM (
	SELECT image_id, score,
	ROW_NUMBER() OVER(order by score ASC) as row_num
	FROM unlabeled_image_predictions 
) AS ans
WHERE row_num % 3 = 0
Limit 10000;

SELECT*FROM positive_samples
UNION ALL
SELECT*FROM negative_samples;
```
