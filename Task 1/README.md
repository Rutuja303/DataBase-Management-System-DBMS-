# Task 1
## SQL Query

### GOAL
- Create positive samples -> rows with high scores (stored in descending order).
- Create negative samples -> rows with low scores (stored in ascending order).
- Select every 3rd row in both cases.
- limit to 10,000 rows in each sample.
- Combine them using UNION ALL.

To sample data from the unsampled_image_prediction table, we first create two separate tables:
- positive_samples
- negative samples

Positive samples are selected by ordering the data by score in descending order and picking every 3rd row using ROW_NUMBER(), limited to 10,000 rows.
Similarly, negative samples are selected by ordering the data by score in ascending order and again selecting every 3rd row, limited to 10,000 rows.
Finally, both the tables are combined using UNION ALL to form a balanced dataset of high and low scoring samples for further analysis or training.


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


