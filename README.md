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

**Selecting, Paginating, & Using Where Clauses**

```sql
SELECT * FROM ingredients;
SELECT title, type FROM ingredients;
SELECT * FROM ingredients where type = 'vegetable';
SELECT * FROM ingredients where type <> 'vegetable'; -- where type is not a fruit
SELECT * FROM ingredients where id <= 10 OR id >= 20;
SELECT * FROM ingredients where id >= 10 AND id <= 20 LIMIT 10;
SELECT * FROM ingredients ORDER BY id LIMIT 10;
SELECT * FROM ingredients ORDER BY id DESC LIMIT 10;
SELECT * FROM ingredients ORDER BY title LIMIT 10;
SELECT * FROM ingredients ORDER BY title DESC LIMIT 10;
SELECT id, title, image FROM ingredients LIMIT 10;
SELECT id, title, image FROM ingredients LIMIT 10 OFFSET 10;
SELECT id, title, image FROM ingredients WHERE id > 23 LIMIT 10;

```

**Using LIKE, ILIKE, & SQL Functions**

```sql
SELECT * FROM ingredients WHERE title LIKE '%pota'; -- find where pota is at the end
SELECT * FROM ingredients WHERE title LIKE 'pota%'; -- find where pota is at the start
SELECT * FROM ingredients WHERE title LIKE '%pota%'; -- find from and or start
SELECT * FROM ingredients WHERE title LIKE LOWER('%Pota%'); -- case insensitive
SELECT * FROM ingredients WHERE title ILIKE '%Pota%'; -- case insensitive

SELECT * FROM ingredients WHERE CONCAT(title, type) ILIKE '%fruit%';
SELECT * FROM ingredients WHERE LOWER(CONCAT(title, type)) ILIKE '%fruit%';
SELECT * FROM ingredients WHERE title ILIKE 'ch_rry'; -- match exactly one character

SELECT NOW();
SELECT LOWER('HI THERE');

-- window function
SELECT *, COUNT(*) OVER ()::INT AS total_count FROM ingredients WHERE CONCAT(title, type) ILIKE '%fruit%' OFFSET 0 LIMIT 10;
```

**Understanding Relationships & Joins**

```sql
CREATE TABLE recipes (
    id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    title VARCHAR(255) UNIQUE NOT NULL,
    body TEXT
);

INSERT INTO recipes
  (title, body)
VALUES
  ('cookies', 'very yummy'),
  ('empanada','ugh so good'),
  ('jollof rice', 'spectacular'),
  ('shakshuka','absolutely wonderful'),
  ('khachapuri', 'breakfast perfection'),
  ('xiao long bao', 'god I want some dumplings right now');

CREATE TABLE recipes_photos (
    id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    recipe_id INTEGER, -- not a foreign key, just integer
    url VARCHAR(255) NOT NULL
);

INSERT INTO recipes_photos
  (recipe_id, url)
VALUES
  (1, 'cookies1.jpg'),
  (1, 'cookies2.jpg'),
  (1, 'cookies3.jpg'),
  (1, 'cookies4.jpg'),
  (1, 'cookies5.jpg'),
  (2, 'empanada1.jpg'),
  (2, 'empanada2.jpg'),
  (3, 'jollof1.jpg'),
  (4, 'shakshuka1.jpg'),
  (4, 'shakshuka2.jpg'),
  (4, 'shakshuka3.jpg'),
  (5, 'khachapuri1.jpg'),
  (5, 'khachapuri2.jpg');

 -- WIHOUT ALIAS AND WHERE CLAUSE
SELECT recipes.title, recipes.body, recipes_photos.url
FROM recipes_photos
INNER JOIN recipes
ON recipes_photos.recipe_id = recipes.id
WHERE recipes_photos.recipe_id = 4;

-- INNER JOIN WITH ALIAS
SELECT r.title, r.body, p.url
FROM recipes_photos p
INNER JOIN recipes r
ON p.recipe_id = r.id;

 -- INNER JOIN WITH ALIAS AND WHERE CLAUSE
SELECT r.title, r.body, p.url
FROM recipes_photos p
INNER JOIN recipes r
ON p.recipe_id = r.id
WHERE p.recipe_id =4;

-- RIGHT OUTER JOIN
SELECT r.title, r.body, p.url
FROM recipes_photos p
RIGHT OUTER JOIN recipes r
ON p.recipe_id = r.id;

-- LEFT OUTER JOIN
SELECT r.title, r.body, p.url
FROM recipes_photos p
LEFT OUTER JOIN recipes r
ON p.recipe_id = r.id;

-- FULL OUTER JOIN
SELECT r.title, r.body, p.url
FROM recipes_photos p
FULL OUTER JOIN recipes r
ON p.recipe_id = r.id;

-- NATURAL JOIN
-- Natural Join automatically joins tables based on columns with the same name.
SELECT * FROM recipes_photos NATURAL JOIN recipes;

-- CROSS JOIN
-- get all permutation
SELECT r.title, r.body, rp.url
FROM recipes_photos rp
CROSS JOIN recipes r;
```

**Foreign Keys & Managing References**

```sql
DELETE
FROM recipes r
WHERE r.id = 5;

SELECT *
FROM recipes_photos rp
WHERE rp.recipe_id = 5;

-- reset table
DROP TABLE IF EXISTS recipes;
DROP TABLE IF EXISTS recipes_photos;
CREATE TABLE recipes (
    id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    title VARCHAR(255) UNIQUE NOT NULL,
    body TEXT
);

INSERT INTO recipes (title, body)
VALUES
    ('cookies', 'very yummy'),
    ('empanada','ugh so good'),
    ('jollof rice', 'spectacular'),
    ('shakshuka','absolutely wonderful'),
    ('khachapuri', 'breakfast perfection'),
    ('xiao long bao', 'god I want some dumplings right now');

CREATE TABLE recipes_photos (
    id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    url VARCHAR(255) NOT NULL,
    recipe_id INTEGER REFERENCES recipes(id) ON DELETE CASCADE
);
```

**Many-to-Many Relationships**

```sql
CREATE TABLE recipe_ingredients(
    recipe_id INTEGER REFERENCES recipes(id) ON DELETE NO ACTION,
    ingredient_id INTEGER REFERENCES ingredients(id) ON DELETE NO ACTION,
    CONSTRAINT recipe_ingredients_pk PRIMARY KEY (recipe_id, ingredient_id)
);

INSERT INTO recipe_ingredients
  (recipe_id, ingredient_id)
VALUES
  (1, 10),
  (1, 11),
  (1, 13),
  (2, 5),
  (2, 13);

SELECT
    i.title AS ingredient_title,
    i.image AS ingredient_image,
    i.type AS ingredient_type,
    r.title AS recipe_title,
    r.body AS recipe_body
FROM recipe_ingredients ri
INNER JOIN ingredients i
ON i.id = ri.ingredient_id
INNER JOIN recipes r
ON r.id = ri.recipe_id;
-- WHERE ri.recipe_id = 1;
```

**Using the CHECK Constraint**

```sql
ALTER TABLE ingredients
ADD CONSTRAINT type_check_enums -- type_check_enums is the name of the constraint
CHECK
    (type IN ('meat', 'fruit', 'vegetable', 'other'));

INSERT INTO ingredients (title, image, type)
VALUES ('lol', 'wat.svg', 'obviously not a type');
```

**Using the DISTINCT Statement**

```sql
SELECT DISTINCT ON (recipe_id) * FROM recipe_ingredients;

SELECT DISTINCT type FROM ingredients;

SELECT DISTINCT ON (r.id) *
FROM recipes r
LEFT JOIN recipes_photos rp
ON r.id = rp.recipe_id;

SELECT DISTINCT ON (r.recipe_id)
    r.title,
    COALESCE (rp.url, 'default.jpg') AS url -- coalesce if rp.url is null or null like show default.jpg else return rp.url value
FROM recipes r
LEFT JOIN recipes_photos rp
ON r.recipe_id = rp.recipe_id;
```

```bash
docker run -e POSTGRES_PASSWORD=lol --name=sql -p 5432:5432 -d --rm btholt/complete-intro-to-sql
```

**JSONB**

```sql
ALTER TABlE recipes
ADD COLUMN meta JSONB;

UPDATE recipes
SET meta = '{"tags": ["chocolate", "desert", "cake"] }'
WHERE recipe_id = 16;

SELECT meta FROM recipes WHERE meta IS NOT NULL;
SELECT meta -> 'tags' FROM recipes WHERE meta IS NOT NULL;
SELECT meta -> 'tags' -> 0 FROM recipes WHERE meta IS NOT NULL;
SELECT meta -> 'tags' -> 1 FROM recipes WHERE meta IS NOT NULL;
SELECT meta -> 'tags' -> 0 AS first_tag FROM recipes WHERE meta IS NOT NULL;
-- ->> double greater than return value as plain string text
SELECT meta -> 'tags' ->> 0 AS first_tag FROM recipes WHERE meta IS NOT NULL;

-- ? do you contain top level key
SELECT recipe_id, title, meta -> 'tags'
FROM recipes
WHERE meta -> 'tags' ? 'cake';

SELECT recipe_id, title, meta -> 'tags'
FROM recipes
WHERE meta ? 'tags';

-- @> does this array contains this value
SELECT recipe_id, title, meta -> 'tags'
FROM recipes
WHERE meta -> 'tags' @> '"cake"';
```
