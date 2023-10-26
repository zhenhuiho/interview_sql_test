## Reference files
- `original_sql.sql` = the original SQL query
- `improvement_sql.sql` = new improvement SQL query
- `original_SQL_mindmap` = reference diagram created based on understanding of original SQL query

## Improvement logic
The following are reasons for new SQL query.

1. Based on the original SQL query, the following tables are joined into `Jobs` table beforehand:
    a. `JobsPersonalities` tables (joined by `Personalities` table)
    b. `JobsPracticalSkills` tables (joined by `PracticalSkills` table)
    c. `JobsBasicAbilities` tables (joined by `BasicAbilities` table)
    d. `JobsTools`, `JobsCareerPaths`, `JobsRecQualifications`, `JobsReqQualifications` tables (each joined by an of `affiliates` table under separate aliases)
2. `Jobs` table would finally be joined together with `JobCategories` & `JobTypes` via `INNER JOIN` with specific conditions.

---
Due to the tables in **1a**, **1b**, and **1c** are being joined into `Jobs` table before **2**, it seems logical to apply `LEFT JOIN` as a set first.

Also, it appears that the processing of **1d** can be improved by
    - joining `JobsTools`, `JobsCareerPaths`, `JobsRecQualifications`, `JobsReqQualifications` tables into one table (set as `JobsGroup` in new SQL)
    - reducing instances of `affiliates` table to one
    - finally use `LEFT JOIN` and set the results as `JobsSets`
After this, `JobSets` will then joined into `Jobs` table via `LEFT JOIN` as well.

Therefore, some of the original `WHERE` conditions involving the following tables can also be omitted since they are being merged into `Jobs` table:
    - `Tools`
    - `CareerPaths`
    - `RecQualifications`
    - `ReqQualifications`
In this setup, the `WHERE` conditions for these instances can be done via `Jobs.name LIKE ...` in one go.