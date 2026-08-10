#### What does the GENERATED ALWAYS AS IDENTITY clause do when creating a table?

It automatically generates a unique, incrementing integer ID for each new record without requiring manual input, and the database keeps track of the next available ID in a sequence.

#### What is the purpose of the PRIMARY KEY constraint when creating a database table?

It defines a unique identifier for each record in the table, allowing the database to index and quickly access individual rows.

#### What is the difference between VARCHAR and TEXT data types in PostgreSQL?

VARCHAR has a specified maximum length (e.g., 255 characters) and can help optimize storage, while TEXT allows for unlimited length storage.

#### What does an INNER JOIN do in a SQL query?

An INNER JOIN combines rows from two tables based on related column between them, returning only the rows that have matching values in both tables. Rows without a match are excluded from the result set.

An INNER JOIN returns only the matching rows between two tables, represented by the overlapping middle section of Venn diagram, which includes only records that have matching vlaues in both tables.

#### What is the purpose of a RIGHT JOIN in SQL?

A RIGHT JOIN returns all records from the right table and the matching records from the left table. IF there are no matches, NULL values are returned for left table columns, which allows including data that might not have corresponding entries in the other table.

#### What does FULL OUTER JOIN do in SQL?

A FULL OUTER JOIN returns all ercords from both tables, matching and non-matching. It combines the result of both LEFT and RIGHT JOINs, including all rows from both tables and filling in NULL values where no matches exists.

#### How can a JOIN be used to create an ignore or deny list?

By performing a RIGHT JOIN and then filtering with a NULL check on the left table's key, you can exclude specific records from a result set. For example, to create a user ignore list: perform a RIGHT JOIN between the main users table and an ignore list table, then filter WHERE the left key IS NULL to exclude ignored users from results like a timeline.

#### What is a Natural Join in SQL and what is its potential limitation?

A Natural join automatically joins tables based on columns with the same name. Its limitation is that it can unexpectedly join on columns with identical names, which might not make logical sense, potentially causing unintended join results.

#### What is the purpose of using 'ON DELETE CASCADE' in a foreign key constraint?

When a record is deleted from the parent table, 'ON DELETE CASCADE' automatically deletes all corresponding records in the child table, preventing orphaned records.

#### What are the different options for handling deletions in a foreign key constraint?

The options are: CASCADE (delete related records), NO ACTION (throw an error if related record exists), and SET NULL (set foreign key values to null when parent record is deleted).

#### What does a foreign key constraint prevent in database relationships?

A foreign key constraint prevents inserting or maintaining records in a child table that reference non-existent records in the parent table, ensuring referential integrity.

#### What syntax is used to define a foreign key constraint when creating a table?

The syntax is: column_name INT REFERENCES parent_table(parent_column) ON DELETE {CASCADE|NO ACTION|SET NULL}

#### What additional constraint can be added to a foreign key besides ON DELETE?

An ON UPDATE constraint can also be added to handle cascading updates between related tables, similar to the ON DELETE behavior.

#### How do you model a many-to-many relationship in SQL?

Create a third intermediary table (junction table) with foreign key references to both original tables, and use a compound primary key combining those foreign key columns to ensure uniqueness of connections

#### What is the purpose of using ON DELETE NO ACTION in foreign key constraints?

Prevents accidental cascading deletions across tables by requiring manual deletion of connections in the intermediary table before deleting records in the original tables

#### What type of join is typically used to retrieve data across multiple related tables in a many-to-many relationships?

INNER JOIN is used multiple times to connect the junction table with the original tables and retrieve related data from all tables

#### Why use a compound primary key in a junction table?

To guarantee uniqueness of the relationship between two entities and prevent duplication connections, such as the same ingredient being added twice to the same recipe.

#### What is the typical structure of a junction table in a many-to-many relationships?

It contains foreign key columns referencing the primary keys of both original tables, with a compound primary key made up of these foreign key columns. The foreign key constraints typically use ON DELETE NO ACTION to prevent cascading deletes that could accidentally wipe out connected data.

#### What is a constraint in a databse context?o

A constraint is a rule applied to a column that defines specific conditions or limitations for data insertion, ensuring data integrity by enforcing predefined criteria before allowing data to be added to a table.

#### What is an example of a constraint in database design?

Examples of constraints include unique constraints (ensuring unique values), primary key constraints, foreign key constraints, check constraints (limiting values to a specific set), and NOT NULL constraints

#### How do you create a check constraint for a type column with specific allowed values?

Use the SQL syntax: ALTER TABLE table_Name ADD CONSTRAINT constraint_name CHECK (column_name in ('value1' , 'value2', 'value3'))

#### What makes SQL databases particularly effective for structured data?

SQL databases excel at enforcing complex rules and constraints, maintaining data consistency through specific schema rules and validation mechanisms that ensure data meets predefined criteria.

#### What should developers consider when adding constraints to existing tables?

Developers should carefully plan database migrations, be aware of performance implications, potentially update existing data to comply with new constraints, and consider potential downtime for large tables.

#### What does the SQL DISTINCT keyword do?

The DISTINCT keyword returns only unique values, removing duplicate rows and showing the first occurence of each distinct value in a column or set of columns

#### How can you use DISTINCT to list unique types from a table?

Use the syntax 'SELECT DINSTINCT column_name FROM table_name', which will return only the unique values fro the specified column without reppetition

#### What is the purpose of the COALESCE function in SQL?

COALESCE checks a value and returns a default value if the original value is NULL, allowing for graceful handling of missing data

#### How do you count the number for rows in a table using SQL?

Use the COUNT() function followed by FROM and the table name, such as 'SELECT COUNT() from table_name'

#### What does ON (column_name) do when used with DISTINCT?

When used with DISTINCT, ON (column_name) ensures that only one row per unique value of that column is returned, effectively limiting the result to the first occurence of each distinct value
