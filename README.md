# Complete Intro to SQL & PostgreSQL

**Requirements**

- docker
- postgres:14

```bash
docker pull postgres:14
```

```bash
docker run -e POSTGRES_PASSWORD=lol --name=pg --rm -d -p 5432:5432 postgres:14
```

```bash
docker exec -u postgres -it pg psql
```

**Creating a database**

```sql
CREATE DATABASE recipeguru;
```

**Connecting to a database using psql**

```psql
\c recipeguru;
```

**Creating a table**

```sql
CREATE TABLE ingredients (
    id INT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    title VARCHAR(255) UNIQUE NOT NULL
);
```

**Listing all table inside a database using psql**

```psql
\d
```

**Describign a table using psql**

```psql
\d ingredients
```

**Inserting data into a table**

```sql
INSERT INTO ingredients (title) VALUES ('bell pepper')
```

**Dropping a database and table**

```sql
DROP DATABASE recipeguru;
DROP table ingredients;
```

**SELECT query**

```sql
SELECT * from ingredients;
```

**Alerting a table**

```sql
ALTER TABLE ingredients ADD COLUMN image VARCHAR(255);

ALTER TABLE ingredients DROP COLUMN image VARCHAR(255);

ALTER TABLE ingredients
ADD COLUMN image VARCHAR(255),
ADD COLUMN type VARCHAR(50) NOT NULL;

ALTER TABLE ingredients ALTER COLUMN age TYPE integer;
ALTER TABLE ingredients RENAME COLUMN name TO username;
ALTER TABLE ingredients ALTER COLUMN status SET DEFAULT 'active';
ALTER TABLE ingredients ALTER COLUMN username SET NOT NULL;
```
