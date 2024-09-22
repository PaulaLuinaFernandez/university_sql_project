# Project Requirements for SQL Course

* Crea una base de datos con las siguientes tablas: `Students`, `Courses`, `Professors`, `Grades`.
* Crea claves foráneas para relacionar las tablas entre sí.
* Crea un *script* que rellene todas las tablas con datos de ejemplo.
* Crea *scripts* de consulta SQL para:
    * La nota media que da cada profesor.
    * Las mejores notas de cada estudiante.
    * Ordenar a los estudiantes por los cursos en los que están matriculados.
    * Crear un informe resumido de los cursos y sus notas medias, ordenados desde el curso más difícil (*curso con la nota media más baja*) hasta el curso más fácil.
    * Encontrar qué estudiante y profesor tienen más cursos en común.
    
## Base de datos: university_sql_project

### Tabla `Students`:

| Nombre de columna | Tipo de dato   | Características                               |
| ----------------- | -------------- | --------------------------------------------- |
| `students_id`     | `INT`          | Primary Key, Not Null, Unique, Auto Increment |
| `students_name`   | `VARCHAR(45)`  | Not Null                                      |
| `students_email`  | ` VARCHAR(80)` | Not Null, Unique                              |

### Tabla `Courses`:

| Nombre de columna | Tipo de dato  | Características                               |
| ----------------- | ------------- | --------------------------------------------- |
| `courses_id`      | `INT`         | Primary Key, Not Null, Unique, Auto Increment |
| `courses_name`    | `VARCHAR(80)` | Not Null                                      |

### Tabla `Professors`:

| Nombre de columna  | Tipo de dato  | Características                               |
| ------------------ | ------------- | --------------------------------------------- |
| `professors_id`    | `INT`         | Primary Key, Not Null, Unique, Auto Increment |
| `professors_name`  | `VARCHAR(45)` | Not Null                                      |
| `professors_email` | `VARCHAR(80)` | Not Null, Unique                              |

### Tabla `Grades`:

| Nombre de columna      | Tipo de dato    | Características                               |
| ---------------------- | --------------- | --------------------------------------------- |
| `grades_id`            | `INT`           | Primary Key, Not Null, Unique, Auto Increment |
| `grades_students_id`   | `INT`           | Not Null                                      |
| `grades_professors_id` | `INT`           | Not Null                                      |
| `grades_courses_id`    | `INT`           | Not Null                                      |
| `grades_mark`         | `DECIMAL(4, 2)` | Not Null                                   

### Las claves foráneas se encontrarán todas en la tabla `Grades`:

* `grades_students_id` estará relacionada con `students_id` de la tabla `Students`.
* `grades_courses_id` estará relacionada con `courses_id` de la tabla `Courses`.
* `grades_professors_id` estará relacionada con `professors_id` de la tabla `Professors`.

### Rellenar todas las tablas con datos de ejemplo:
```sql
USE university_sql_project;

INSERT INTO students(students_name, students_email)
VALUES
	('Student1', 'student1@test.com'),
    ('Student2', 'student2@test.com'),
    ('Student3', 'student3@test.com'),
    ('Student4', 'student4@test.com'),
    ('Student5', 'student5@test.com'),
    ('Student6', 'student6@test.com'),
    ('Student7', 'student7@test.com'),
    ('Student8', 'student8@test.com'),
    ('Student9', 'student9@test.com'),
    ('Student10', 'student10@test.com');

INSERT INTO courses(courses_name)
VALUES
	('Algebra'),
    ('Physics'),
    ('Structural Calculation'),
    ('Technical Design'),
    ('Coding');

INSERT INTO professors(professors_name, professors_email)
VALUES
	('Professor1', 'professor1@test.com'),
    ('Professor2', 'professor2@test.com'),
    ('Professor3', 'professor3@test.com'),
    ('Professor4', 'professor4@test.com'),
    ('Professor5', 'professor5@test.com');


INSERT INTO grades(grades_courses_id, grades_students_id, grades_professors_id, grades_mark)
VALUES
	(1, 1, 1, 6.2),
    (1, 2, 1, 7.3),
    (1, 3, 1, 4.5),
    (1, 4, 1, 8.7),
    (1, 5, 1, 5.4),
    (2, 6, 2, 8.8),
    (2, 7, 2, 6.3),
    (2, 8, 2, 5.7),
    (2, 9, 2, 7.7),
    (2, 10, 2, 4.5),
    (3, 1, 3, 6.8),
    (3, 2, 3, 9.2),
    (3, 3, 3, 10.0),
    (3, 4, 3, 6.5),
    (3, 5, 3, 8.3),
    (4, 6, 4, 7.7),
    (4, 7, 4, 9.6),
    (4, 8, 4, 6.5),
    (4, 9, 4, 8.5),
    (4, 10, 4, 7.8),
    (5, 1, 5, 5.5),
    (5, 3, 5, 9.7),
    (5, 5, 5, 4.6),
    (5, 7, 5, 6.6),
    (5, 9, 5, 8.7);
```

### Crear *scripts* de consulta SQL:

* **Para la nota media que da cada profesor:**

```sql
USE university_sql_project;

SELECT
p.professors_name,
AVG(g.grades_mark) AS 'Average grade'
FROM grades g
JOIN professors p ON g.grades_professors_id = p.professors_id
GROUP BY g.grades_professors_id;
```

* **Para las mejores notas de cada estudiante:**

```sql
USE university_sql_project;

SELECT
s.students_name,
MAX(g.grades_mark) AS top_grade
FROM grades g
JOIN students s ON g.grades_students_id = s.students_id
GROUP BY g.grades_students_id;
```

* **Para ordenar a los estudiantes por los cursos en los que están matriculados:**

```sql
USE university_sql_project;

SELECT
s.students_name,
c.courses_id,
c.courses_name
FROM grades g
JOIN students s ON g.grades_students_id = s.students_id
JOIN courses c ON g.grades_courses_id = c.courses_id
ORDER BY g.grades_courses_id ASC;
```

* **Para crear un informe resumido de los cursos y sus notas medias, ordenados desde el curso más difícil (*curso con la nota media más baja*) hasta el curso más fácil:**

```sql
USE university_sql_project;

SELECT
c.courses_name,
AVG(g.grades_mark) AS grade_average
FROM grades g
JOIN courses c ON g.grades_courses_id = c.courses_id
GROUP BY g.grades_courses_id
ORDER BY grade_average ASC;
```

* **Encontrar qué estudiante y profesor tienen más cursos en común:**

```sql
USE university_sql_project;

SELECT
s.students_name,
p.professors_name,
COUNT(DISTINCT c.courses_id) AS courses_in_common
FROM grades g
JOIN students s ON g.grades_students_id = s.students_id
JOIN professors p ON g.grades_professors_id = p.professors_id
JOIN courses c ON g.grades_courses_id = c.courses_id
GROUP BY s.students_name, p.professors_name
ORDER BY courses_in_common DESC
```
En esta última consulta, como cada profesor sólo da una única asignatura, el resultado de cursos en común estudiante - profesor, siempre sale 1.




