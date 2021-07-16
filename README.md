# Basic SQL guide
===========================

## 1) CREATE TABLA (CREATE TABLE)
CREATE TABLE nom_table (id INTEGER PRIMARY KEY, col1_name TYPE, col2_name TYPE, ...., coln_name TYPE);

- It is better start with an id as PRIMARY KEY. 
- Most frequently TYPE are: TEXT , INTEGER (number without decimals), REAL and BLOB (keep the input format).

EXAMPLE:
CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER );

---
## 2) INSERT VALUES IN A TABLE (INSERT INTO)

INSERT INTO nom_table VALUES (‘text’, 5, 2,7, ....);

- For each line, we have to make an INSERT.
- Text between quotation marks and can be double (“ ”) o simple (‘ ’), SQL accept both.

EXAMPLE:
INSERT INTO groceries VALUES (1, "Bananas", 4);
INSERT INTO groceries VALUES (2, "Peanut Butter", 1);
INSERT INTO groceries VALUES (3, "Dark chocolate bars", 2);

---
## 3) VIEW TABLE (SELECT)

SELECT * FROM nom_table;

EXAMPLE:
SELECT * FROM groceries;

---
## 4) VIEW A COLUMN

SELECT nom_col FROM nom_table;

EXAMPLE:
SELECT name FROM groceries;

---
## 5) ARRANGE TABLE ACCORDING TO A COLUMN (ORDER BY)

SELECT * FROM nom_table ORDER BY nom_col DESC ó ASC;

EXAMPLE:
SELECT * FROM groceries ORDER BY quantity;

- By default SQL arrange ascending, if we want to order descendingly we have to add a DESC at the end.

EXAMPLE:
SELECT * FROM groceries ORDER BY quantity DESC;

---
## 6) FILTER (WHERE)

... WHERE operador 5, 3.2, ‘text’, ....

- Operador could be >, <, =, >=, <=, ...
- ORDER BY always at the end.
 
EXAMPLE:
SELECT * FROM groceries WHERE quantity > 3 ORDER BY name; 

- It is possible apply filter with most of one condition adding OR, AND.

EXAMPLE:
CREATE TABLE exercise_logs (id INTEGER PRIMARY KEY AUTOINCREMENT, type TEXT, minutes INTEGER, calories INTEGER, heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);

SELECT * FROM exercise_logs WHERE calories > 50 AND minutes < 30;

SELECT * FROM exercise_logs WHERE calories > 50 OR heart_rate > 100;

---
## 7) FUNCTIONS

SELECT FUNCTION(nom_col) FROM nom_table;

- FUNCTION can be SUM, AVG(Mean), MAX, MIN.

INSERT INTO groceries VALUES (1, "Bananas", 56, 7);
INSERT INTO groceries VALUES(2, "Peanut Butter", 1, 2);
INSERT INTO groceries VALUES(3, "Dark Chocolate Bars", 2, 2);
INSERT INTO groceries VALUES(4, "Ice cream", 1, 12);
INSERT INTO groceries VALUES(5, "Cherries", 6, 2);
INSERT INTO groceries VALUES(6, "Chocolate syrup", 1, 4);

EXAMPLE:

SELECT aisle, SUM(quantity) FROM groceries GROUP BY aisle;

---
## 8) OPERATIONS WITH FILTERS

SELECT col_filter_name FUNCTION (nom_col_num) FROM nom_table GROUP BY col_filter_name;

EXAMPLE:

SELECT aisle, SUM(quantity) FROM groceries GROUP BY aisle;

---
## 9) MULTIPLE FILTERS - (IN, NOT IN)

SELECT * FROM nom_table WHERE col_num_name IN (‘texto1’, ‘texto2’, ...);

EXAMPLE:
CREATE TABLE exercise_logs (id INTEGER PRIMARY KEY AUTOINCREMENT, type TEXT, minutes INTEGER, calories INTEGER, heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs WHERE type = "biking" OR type = "hiking" OR type = "tree climbing" OR type = "rowing";

SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking", "tree climbing", "rowing");

- IN is used to avoid multiple OR and make it easier for the user.

EXAMPLE:
CREATE TABLE drs_favorites (id INTEGER PRIMARY KEY, type TEXT, reason TEXT);

INSERT INTO drs_favorites(type, reason) VALUES ("biking", "Improves endurance and flexibility.");
INSERT INTO drs_favorites(type, reason) VALUES ("hiking", "Increases cardiovascular health.");

SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites);

EXAMPLE:
SELECT * FROM exercise_logs WHERE type NOT IN (SELECT type FROM drs_favorites);

---
## 10) QUERIES WITH DESIRED VALUE (LIKE)

SELECT * FROM nom_table1 WHERE nom_col IN(SELECT nom_col FROM nom_table2 WHERE col_search_name LIKE ‘%valor_bucado%);

- The secound part of SELECT is an other related table by nom_col.
- Before IN goes the wanted filter.

EXAMPLE:
SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites WHERE reason LIKE "%cardiovascular%");

---
## 11) MAKE A FILTER TO AN OPERATION (HAVING)

SELECT nom_col FUNCION(nom_col_num) AS nom_col_resul FROM nom_tabla GROUP BY nom_col HAVING nom_result;

EXAMPLE:
SELECT type, SUM(calories) AS total_calories FROM exercise_logs
GROUP BY type HAVING total_calories > 150

---
## 12) CASE
SELECT OPERATION(*) 
CASE 
WHEN name_col_num operador # or operation THEN ‘text 1’ 
WHEN name_col_num operador # or operation THEN ‘text 2’ ...
ELSE ‘text if donesn’t fulfill the conditions’
END AS ‘col_search_name’
FROM nom_table GROUP BY col_search_name;

- CASE is like a SWITCH.
- CASE create a new column ‘col_search_name’ accordin to given ranges (WHERE; ELSE). Wuth this new column is easier to do the searching.

EXAMPLE:
SELECT COUNT(*),
    CASE 
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs
GROUP BY hr_zone;

---
## 13) EXPLICIT INNER JOIN
SELECT nom_table.nom_col1, nom_table.nom_col2, ... FROM table1 
JOIN table2 ON table1.id(PK) = table2.col_id;

- It is very important input at the beginning of the table (nom_table) where we are going to take the column (nom_col), because it is possible that other tables has columns with the same name.

EXAMPLE:
CREATE TABLE students (id INTEGER PRIMARY KEY, first_name TEXT,    last_name TEXT, email TEXT, phone TEXT, birthdate TEXT);

INSERT INTO students (first_name, last_name, email, phone, birthdate)
VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
    
CREATE TABLE student_grades (id INTEGER PRIMARY KEY, student_id INTEGER, test TEXT, grade INTEGER);

INSERT INTO student_grades (student_id, test, grade) VALUES (1, "Nutrition", 95);
INSERT INTO student_grades (student_id, test, grade) VALUES (2, "Nutrition", 92);
INSERT INTO student_grades (student_id, test, grade) VALUES (1, "Chemistry", 85);
INSERT INTO student_grades (student_id, test, grade) VALUES (2, "Chemistry", 95);

SELECT students.first_name, students.last_name, students.email, student_grades.test, student_grades.grade FROM students
JOIN student_grades ON students.id = student_grades.student_id
WHERE grade > 90;

---
## 14) LEFT OUTER JOIN
SELECT nom_col1, nom_col2, ... FROM tabla1 LEFT OUTER JOIN tabla2
ON tabla1.id(PK) = tabla2.col_id;

- LEFT OUTER JOIN is used to when we want to show all the values of the columns selected, inclusive this hasn’t registers in the other table.

EXAMPLE:
CREATE TABLE student_projects (id INTEGER PRIMARY KEY, student_id INTEGER, title TEXT);
INSERT INTO student_projects (student_id, title) VALUES (1, "Carrotapault");

SELECT students.first_name, students.last_name, student_projects.title FROM students LEFT OUTER JOIN student_projects
ON students.id = student_projects.student_id;

---
## 15) SELF JOIN
SELECT nom_col1, nom_col2, ..., alias.nom_col AS nom_com_wanted FROM table JOIN table alias
ON table.col_id = alias.id(PK);

- Used to link columns from the same table.

EXAMPLE:
CREATE TABLE students (id INTEGER PRIMARY KEY AUTOINCREMENT, first_name TEXT, last_name TEXT, email TEXT, phone TEXT, birthdate TEXT,
buddy_id INTEGER);

INSERT INTO students VALUES (1, "Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24", 2);
INSERT INTO students VALUES (2, "Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04", 1);
INSERT INTO students VALUES (3, "Aladdin", "Lampland", "aladdin@lampland.com", "555-3333", "2001-05-10", 4);
INSERT INTO students VALUES (4, "Simba", "Kingston", "simba@kingston.com", "555-1111", "2001-12-24", 3);

SELECT students.first_name, students.last_name, buddies.email as buddy_email FROM students JOIN students buddies
ON students.buddy_id = buddies.id;

---
## 16) MULTIPLE JOIN
SELECT a.nom_col, b.nom_col FROM table2
JOIN table1 a ON tabla2.col_id = a.id(PK)
JOIN table1 b ON tabla2.col_id = b.id(PK)

EXAMPLE:
CREATE TABLE students (id INTEGER PRIMARY KEY, first_name TEXT, last_name TEXT, email TEXT, phone TEXT, birthdate TEXT);
INSERT INTO students (first_name, last_name, email, phone, birthdate)
VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
VALUES ("Aladdin", "Lampland", "aladdin@lampland.com", "555-3333", "2001-05-10");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
VALUES ("Simba", "Kingston", "simba@kingston.com", "555-1111", "2001-12-24");
    
CREATE TABLE student_projects (id INTEGER PRIMARY KEY, student_id INTEGER, title TEXT);
INSERT INTO student_projects (student_id, title) VALUES (1, "Carrotapault");
INSERT INTO student_projects (student_id, title) VALUES (2, "Mad Hattery");
INSERT INTO student_projects (student_id, title) VALUES (3, "Carpet Physics");
INSERT INTO student_projects (student_id, title) VALUES (4, "Hyena Habitats");
    
CREATE TABLE project_pairs (id INTEGER PRIMARY KEY, project1_id INTEGER, project2_id INTEGER);
INSERT INTO project_pairs (project1_id, project2_id) VALUES(1, 2);
INSERT INTO project_pairs (project1_id, project2_id) VALUES(3, 4);

SELECT a.title, b.title FROM project_pairs
JOIN student_projects a ON project_pairs.project1_id = a.id
JOIN student_projects b ON project_pairs.project2_id = b.id;

---
## 17) UPDATE
UPDATE table SET nom_col = ‘wath I want to update’ WHERE nom.colx = # ó  ‘char’ AND nom_coly = # ó ‘char’ o simply the id(PK) if I know it.

EXAMPLE:
CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT);
CREATE TABLE diary_logs (id INTEGER PRIMARY KEY, user_id INTEGER, date TEXT, content TEXT);

INSERT INTO diary_logs (user_id, date, content) VALUES (1, "2015-04-01",
"I had a horrible fight with OhNoesGuy and I buried my woes in 3 pounds of dark chocolate.");  
INSERT INTO diary_logs (user_id, date, content) VALUES (1, "2015-04-02",
"We made up and now we're best friends forever and we celebrated with a tub of ice cream.");

UPDATE diary_logs SET content = "I had a horrible fight with OhNoesGuy" WHERE user_id=1 AND date = "2015-04-01";

---
## 18) DELETE
DELETE FROM table WHERE id(PK) = # ó filtrando;

EXAMPLE:
DELETE FROM diary_logs WHERE id = 1;

---
## 19) ADD A NEW COLUMN (ALTER TABLE)
ALTER TABLE tabla ADD nom_col TIPO default ‘new content’;

EXAMPLE:
ALTER TABLE diary_logs ADD emotion TEXT default "unknown";

---
## 20) DELETE TABLE (DROP TABLE)
DROP TABLE tabla;

---
## Author
====================
* [Robinson Montes](https:www.github.com/mecomontes) - Github
