1. Selezionare tutti gli studenti nati nel 1990 (160)
<!-- SELECT * 
FROM `students`
WHERE `date_of_birth` 
LIKE '1990%'; -->

SELECT * 
FROM `students` 
WHERE YEAR(`date_of_birth`) = 1990;

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
SELECT * 
FROM `courses`
WHERE `cfu` > '10';

3. Selezionare tutti gli studenti che hanno più di 30 anni
<!-- SELECT * 
FROM `students` 
WHERE (YEAR(CURRENT_DATE) - YEAR(`date_of_birth`)) > 30; -->

SELECT * 
FROM `students` 
WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURRENT_DATE) > 30;

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
SELECT * 
FROM `courses`
WHERE `period` = 'I semestre'
AND `year` = '1';

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
<!-- SELECT * 
FROM `exams`
WHERE `hour` > '14%'
AND `date` = '2020-06-20'; -->

SELECT * 
FROM `exams` 
WHERE HOUR(hour) >= 14 
AND `date` = '2020-06-20';

6. Selezionare tutti i corsi di laurea magistrale (38)
SELECT * 
FROM `degrees`
WHERE `level` = 'magistrale';

7. Da quanti dipartimenti è composta l'università? (12)
SELECT COUNT(id) 
FROM `departments`;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
SELECT COUNT(id) 
FROM `teachers`
WHERE `phone` IS NULL;


<!-- Query con JOIN -->

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT `degrees`.`name` , `students`.`name`, `students`.`surname`
FROM `degrees` 
INNER JOIN `students`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze
SELECT `degrees`.`name` AS courses_name, `departments`.`name` AS department_name
FROM `degrees`
INNER JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `degrees`.`level` = 'Magistrale'
AND `departments`.`name` = 'Dipartimento di Neuroscienze';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT `courses`.`name`,`teachers`.`surname`, `teachers`.`name` 
FROM `courses`
INNER JOIN `course_teacher` ON `course_id` = `courses`.`id`
INNER JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
WHERE `teachers`.`id` = 44;

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome
SELECT `students`.`surname`, `students`.`name`,`degrees`.* ,`departments`.`name`
FROM `students`
INNER JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id`
INNER JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
ORDER BY `students`.`surname`;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT CONCAT(`teachers`.`name`, ' ' , `teachers`.`surname`) AS fullname, `degrees`.`name`, `courses`.`name`
FROM `degrees`
INNER JOIN `courses` ON `courses`.`degree_id` = `degrees`.`id`
INNER JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
INNER JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)
SELECT CONCAT(`teachers`.`surname`, ' ' , `teachers`.`name`), `departments`.`name` FROM `teachers` INNER JOIN `course_teacher` ON `course_teacher`.`teacher_id` = `teachers`.`id` INNER JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id` INNER JOIN `degrees` ON `degrees`.`id` = `courses`.`degree_id` INNER JOIN `departments` ON `departments`.`id` = `degrees`.`department_id` WHERE `departments`.`id` = 5 ORDER BY `teachers`.`surname` ASC;

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.


<!-- Query con GROUP BY -->

1. Contare quanti iscritti ci sono stati ogni anno
SELECT COUNT(id), YEAR(`enrolment_date`)
FROM `students`
GROUP BY YEAR(`enrolment_date`);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT COUNT(id), `office_address` 
FROM `teachers`
GROUP BY `office_address`;

3. Calcolare la media dei voti di ogni appello d'esame
SELECT `exam_id`, AVG(`vote`)
FROM `exam_student`
GROUP BY `exam_id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT COUNT(id) AS number_courses, `department_id` 
FROM `degrees`
GROUP BY `department_id`;