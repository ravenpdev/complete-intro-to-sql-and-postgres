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
INSERT INTO ingredients (title) VALUES ('bell pepper');

INSERT INTO ingredients (title, image, type) -- comment
VALUES('red pepper', 'red_pepper.jpg', 'vegetable');

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
ALERT TABLE ingredients ALTER COLUMN type DROP NOT NULL;
```

**Inserting Data and Managing Conflicts**

```sql
INSERT INTO ingredients (
  title, image, type
) VALUES
  ( 'avocado', 'avocado.jpg', 'fruit' ),
  ( 'banana', 'banana.jpg', 'fruit' ),
  ( 'beef', 'beef.jpg', 'meat' ),
  ( 'black_pepper', 'black_pepper.jpg', 'other' ),
  ( 'blueberry', 'blueberry.jpg', 'fruit' ),
  ( 'broccoli', 'broccoli.jpg', 'vegetable' ),
  ( 'carrot', 'carrot.jpg', 'vegetable' ),
  ( 'cauliflower', 'cauliflower.jpg', 'vegetable' ),
  ( 'cherry', 'cherry.jpg', 'fruit' ),
  ( 'chicken', 'chicken.jpg', 'meat' ),
  ( 'corn', 'corn.jpg', 'vegetable' ),
  ( 'cucumber', 'cucumber.jpg', 'vegetable' ),
  ( 'eggplant', 'eggplant.jpg', 'vegetable' ),
  ( 'fish', 'fish.jpg', 'meat' ),
  ( 'flour', 'flour.jpg', 'other' ),
  ( 'ginger', 'ginger.jpg', 'other' ),
  ( 'green_bean', 'green_bean.jpg', 'vegetable' ),
  ( 'onion', 'onion.jpg', 'vegetable' ),
  ( 'orange', 'orange.jpg', 'fruit' ),
  ( 'pineapple', 'pineapple.jpg', 'fruit' ),
  ( 'potato', 'potato.jpg', 'vegetable' ),
  ( 'pumpkin', 'pumpkin.jpg', 'vegetable' ),
  ( 'raspberry', 'raspberry.jpg', 'fruit' ),
  ( 'red_pepper', 'red_pepper.jpg', 'vegetable' ),
  ( 'salt', 'salt.jpg', 'other' ),
  ( 'spinach', 'spinach.jpg', 'vegetable' ),
  ( 'strawberry', 'strawberry.jpg', 'fruit' ),
  ( 'sugar', 'sugar.jpg', 'other' ),
  ( 'tomato', 'tomato.jpg', 'vegetable' ),
  ( 'watermelon', 'watermelon.jpg', 'fruit' )
ON CONFLICT DO NOTHING;

INSERT INTO ingredients (title, image, value) VALUES ('watermelon', 'banana.jpg', 'this wont''t be updated') ON CONFLICT DO NOTHING;

-- upsert
-- update the image value when there is a conflict with title
INSERT INTO ingredients (title, image, value)
VALUES ('watermelon', 'banana.jpg', 'this wont''t be updated')
ON CONFLICT (title) DO UPDATE SET image = excluded.image;
```

**Updating & Deleting Data**

```sql
UPDATE ingredients SET image = 'watermelon.jpg'  WHERE title = 'watermelon';

UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon'
RETURNING title, image, id, type;

-- return everything using *
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon'
RETURNING *;

-- insert sample data
INSERT INTO ingredients (title, image, type)
VALUES ('not real 1', 'delete.jpg', 'demo'), ('not real 2', 'delete.jpg', 'demo');

-- delete multiple data
DELETE FROM ingredients WHERE image = 'delete.jpg'
RETURNING *;
```
