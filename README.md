# db-university
    07/10/2024
 1. Selezionare tutti gli studenti nati nel 1990 (160)

    SELECT * 
    FROM students 
    WHERE YEAR(date_of_birth) = 1990;

 2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

    SELECT * 
    FROM `courses`
    WHERE `cfu` > 10;

 3. Selezionare tutti gli studenti che hanno più di 30 anni (3646) nel 2024

    SELECT * 
    FROM students 
    WHERE YEAR(date_of_birth) < 1994;

 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
    SELECT * 
    FROM `courses` 
    WHERE `period` = 'I semestre' 
    AND `year` = 1;

 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

   SELECT * 
   FROM `exams` 
   WHERE date = '2020-06-20' 
   AND hour > '14:00:00';

 6. Selezionare tutti i corsi di laurea magistrale (38)
   SELECT * 
   FROM `degrees` 
   WHERE `level` = 'magistrale';

 7. Da quanti dipartimenti è composta l'università? (12)
   SELECT COUNT(id) 
   FROM `departments`;

 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
   SELECT * 
   FROM `teachers` 
   WHERE `phone` IS NULL;
 
 9. Inserire nella tabella degli studenti un nuovo record con i propri dati (per il campo degree_id, inserire un valore casuale)


 10. Cambiare il numero dell’ufficio del professor Pietro Rizzo in 126


 11. Eliminare dalla tabella studenti il record creato precedentemente al punto 9
 
08/10/2024

QUERY CON JOIN

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT *
FROM students
JOIN degrees ON students.degree_id = degrees.id
WHERE degrees.name = 'Corso di Laurea in Economia';

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
SELECT *
FROM degrees
WHERE degrees.level = 'magistrale'
AND degrees.department_id = (SELECT id FROM departments WHERE name = 'Dipartimento di Neuroscienze');
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT *
FROM courses
JOIN teachers ON courses.teacher_id = teachers.id
WHERE teachers.id = 44;
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
SELECT *, courses.name AS course_name, teachers.name AS teacher_name
FROM degrees
JOIN courses ON degrees.id = courses.degree_id
JOIN teachers ON courses.teacher_id = teachers.id;
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT *, courses.name AS course_name, teachers.name AS teacher_name
FROM degrees
JOIN courses ON degrees.id = courses.degree_id
JOIN teachers ON courses.teacher_id = teachers.id;
6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
SELECT students.name, exams.name AS exam_name, COUNT(attempts.id) AS attempt_count, MAX(attempts.score) AS max_score
SELECT *
FROM teachers
JOIN courses ON teachers.id = courses.teacher_id
JOIN degrees ON courses.degree_id = degrees.id
WHERE degrees.department_id = 5;
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.
SELECT students.name, exams.name AS exam_name, COUNT(attempts.id) AS attempt_count, MAX(attempts.score) AS max_score
FROM attempts
JOIN students ON attempts.student_id = students.id
JOIN exams ON attempts.exam_id = exams.id
WHERE attempts.score >= 18
GROUP BY students.name, exams.name;
QUERY CON GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno
SELECT YEAR(enrollment_date) AS enrollment_year, COUNT(*) AS student_count
FROM students
GROUP BY YEAR(enrollment_date);
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT office_building, COUNT(*) AS teacher_count
FROM teachers
GROUP BY office_building;
3. Calcolare la media dei voti di ogni appello d'esame
SELECT exam_id, AVG(score) AS average_score
FROM attempts
GROUP BY exam_id;
4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT department_id, COUNT(*) AS degree_count
FROM degrees
GROUP BY department_id;