## Table of Contents
- [Table of Contents](#table-of-contents)
- [Problem Statement | Source](#problem-statement--source)
- [Setup: Table Schema and Sample Data](#setup-table-schema-and-sample-data)
- [Table Preview](#table-preview)
- [Expected Output](#expected-output)
- [Solution](#solution)


## Problem Statement | [Source](https://www.youtube.com/watch?v=4MH8iRnhPO0&ab_channel=LearnatKnowstar "Knowstar")

Fill the EndOfDayRate column with the most recent non-NULL value for rows where it is NULL. Use SQL techniques to accomplish this.

## Setup: Table Schema and Sample Data

```
DROP TABLE IF EXISTS CurrencyRate
GO

CREATE TABLE CurrencyRate (
	CurrencyKey INT NOT NULL,
	DateKey INT NOT NULL,
	EndOfDayRate FLOAT NULL,
	Date DATETIME NULL
)
GO

INSERT INTO CurrencyRate
VALUES 
	(3, 20201229, 0.999800039992002, '2020-12-29'),
	(3, 20201230, 1.00090081072966, '2020-12-30'),
	(3, 20201231, 0.999600159936026, '2020-12-31'),
	(3, 20210101, Null, '2021-01-01'),
	(3, 20210102, Null, '2021-01-02'),
	(3, 20210103, Null, '2021-01-03'),
	(3, 20210104, 0.999500249875062, '2021-01-04'),
	(3, 20210105, 1.000200040008, '2021-01-05'),
	(3, 20210106, 0.999200639488409, '2021-01-06'),
	(3, 20210107, 1.000200040008, '2021-01-07'),
	(3, 20210108, 0.999600159936026, '2021-01-08'),
	(3, 20210109, Null, '2021-01-09'),
	(3, 20210110, Null, '2021-01-10'),
	(3, 20210111, 1.00090081072966, '2021-01-11'),
	(3, 20210112, 0.99930048965724, '2021-01-12')
GO
```

## Table Preview

```
SELECT * 
FROM CurrencyRate
```

| CurrencyKey 	| DateKey  	| EndOfDayRate      	| Date                    	|
|-------------	|----------	|-------------------	|-------------------------	|
| 3           	| 20201229 	| 0.999800039992002 	| 2020-12-29 00:00:00.000 	|
| 3           	| 20201230 	| 1.00090081072966  	| 2020-12-30 00:00:00.000 	|
| 3           	| 20201231 	| 0.999600159936026 	| 2020-12-31 00:00:00.000 	|
| 3           	| 20210101 	| NULL              	| 2021-01-01 00:00:00.000 	|
| 3           	| 20210102 	| NULL              	| 2021-01-02 00:00:00.000 	|
| 3           	| 20210103 	| NULL              	| 2021-01-03 00:00:00.000 	|
| 3           	| 20210104 	| 0.999500249875062 	| 2021-01-04 00:00:00.000 	|
| 3           	| 20210105 	| 1.000200040008    	| 2021-01-05 00:00:00.000 	|
| 3           	| 20210106 	| 0.999200639488409 	| 2021-01-06 00:00:00.000 	|
| 3           	| 20210107 	| 1.000200040008    	| 2021-01-07 00:00:00.000 	|
| 3           	| 20210108 	| 0.999600159936026 	| 2021-01-08 00:00:00.000 	|
| 3           	| 20210109 	| NULL              	| 2021-01-09 00:00:00.000 	|
| 3           	| 20210110 	| NULL              	| 2021-01-10 00:00:00.000 	|
| 3           	| 20210111 	| 1.00090081072966  	| 2021-01-11 00:00:00.000 	|
| 3           	| 20210112 	| 0.99930048965724  	| 2021-01-12 00:00:00.000 	|


## Expected Output

| CurrencyKey 	| DateKey  	| EndOfDayRate      	| Date                    	|
|-------------	|----------	|-------------------	|-------------------------	|
| 3           	| 20201229 	| 0.999800039992002 	| 2020-12-29 00:00:00.000 	|
| 3           	| 20201230 	| 1.00090081072966  	| 2020-12-30 00:00:00.000 	|
| 3           	| 20201231 	| 0.999600159936026 	| 2020-12-31 00:00:00.000 	|
| 3           	| 20210101 	| 0.999600159936026 	| 2021-01-01 00:00:00.000 	|
| 3           	| 20210102 	| 0.999600159936026 	| 2021-01-02 00:00:00.000 	|
| 3           	| 20210103 	| 0.999600159936026 	| 2021-01-03 00:00:00.000 	|
| 3           	| 20210104 	| 0.999500249875062 	| 2021-01-04 00:00:00.000 	|
| 3           	| 20210105 	| 1.000200040008    	| 2021-01-05 00:00:00.000 	|
| 3           	| 20210106 	| 0.999200639488409 	| 2021-01-06 00:00:00.000 	|
| 3           	| 20210107 	| 1.000200040008    	| 2021-01-07 00:00:00.000 	|
| 3           	| 20210108 	| 0.999600159936026 	| 2021-01-08 00:00:00.000 	|
| 3           	| 20210109 	| 0.999600159936026 	| 2021-01-09 00:00:00.000 	|
| 3           	| 20210110 	| 0.999600159936026 	| 2021-01-10 00:00:00.000 	|
| 3           	| 20210111 	| 1.00090081072966  	| 2021-01-11 00:00:00.000 	|
| 3           	| 20210112 	| 0.99930048965724  	| 2021-01-12 00:00:00.000 	|

## Solution

1. **Solution 1**: This solution groups the table into rows that have the LowerBound and UpperBound columns through which the original table can then be merged if the target column's values fall between the LB and UB, here LowerBound and UpperBound are Rn and NextRn respectively.

    ```sql
    ;WITH A AS (
        SELECT 
            *, 
            Rn = ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) 
            -- ^ Generates Arbitrary Index column
        FROM CurrencyRate
    ),
    B AS (
        SELECT 
            *, 
            NextRn = LEAD(Rn, 1, 999999999) OVER(ORDER BY Rn) 
            -- ^ Gets the RowNumber from the immediate next row to create UpperBound
        FROM A
        WHERE NOT EndOfDayRate IS NULL
    )

    SELECT 
        A.CurrencyKey, 
        A.DateKey, 
        B.EndOfDayRate, 
        A.Date
    FROM A
        LEFT JOIN B ON A.Rn >= B.Rn
                    AND A.Rn < B.NextRn
    ```

2. **Solution 2**: This solution uses Subquery and generates a Running Count of EndOfDayRate column rows with NULL get the number of the last non null cell which then we consider as a cluster/bin and they later use it to partition the table so that we can get the first value in that cluster/partition.

    ```sql
    SELECT 
        T.CurrencyKey, 
        T.DateKey, 
        EndOfDayRate = FIRST_VALUE(EndOfDayRate) OVER(
            PARTITION BY Cluster 
            ORDER BY DateKey
        ),
        T.Date
    FROM (
        SELECT 
            *, 
            Cluster = COUNT(EndOfDayRate) OVER(
                ORDER BY DateKey 
                ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
            ) 
            -- ^ Does a RunningCount of EndOfDayRate
        FROM CurrencyRate
    ) AS T
    ```


3. **Solution 3**: This soution is just like Solution 2 but with 1 additional step.
    ```sql
    ;WITH A AS (
        SELECT 
            *, 
            Rn = ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) 
        FROM CurrencyRate
    ),
    B AS (
        SELECT 
            *, 
            Cluster = COUNT(EndOfDayRate) OVER(ORDER BY Rn) 
            -- ^ Running Count
        FROM A
    ),
    C AS (
        SELECT 
            CurrencyKey, 
            DateKey, 
            EndOfDayRate = FIRST_VALUE(EndOfDayRate) OVER(
                PARTITION BY Cluster 
                ORDER BY Rn
            ), 
            Date 
        FROM B
    )

    SELECT *
    FROM C
    ```