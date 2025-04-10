Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:

- sono presenti diversi Dipartimenti (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
- ogni Dipartimento offre più Corsi di Laurea (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
- ogni Corso di Laurea prevede diversi Corsi (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
- ogni Corso può essere tenuto da diversi Insegnanti;
- ogni Corso prevede più appelli d'Esame;
- ogni Studente è iscritto ad un solo Corso di Laurea;
- ogni Studente può iscriversi a più appelli di Esame;
- per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente. Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.

- dipartimenti
- degree_courses
- subjects
- teachers
- exam_sessions
- students
- votes

***************************************************************

## Table name: 'departments'

**columns**
- id (BIGINT) primary key - auto_increment - NOT NULL
- name: VARCHAR(255) NOT NULL
- degree_course_id: (BIGINT) foreign key - NOT NULL
- subject_id: (BIGINT) foreign key - NOT NULL
- teacher_id: (BIGINT) foreign key - NOT NULL

***************************************************************

## Table name: 'degree_courses'

**columns**
- id (BIGINT) primary key - auto_increment - NOT NULL
- name: VARCHAR(255) - NOT NULL
- department_id: (BIGINT) foreign key - NOT NULL

***************************************************************

## Table name: 'subjects'

**columns**
- id (BIGINT) primary key - auto_increment - NOT NULL
- name: VARCHAR(100) - NOT NULL
- teacher_id: (BIGINT) foreign key - NOT NULL
- department_id: (BIGINT) foreign key - NOT NULL
- degree_course_id: (BIGINT) foreign key - NOT NULL

***************************************************************

## Table name: 'teachers'

**columns**
- id (BIGINT) primary key - auto_increment - NOT NULL
- name: VARCHAR(255) - NOT NULL
- lastname: VARCHAR(255) - NOT NULL
- email: VARCHAR(255) - NOT NULL - UNIQUE
- date_of_birth: DATE() - NOT NULL
- address: VARCHAR(255) - NULL
- subject_id: (BIGINT) foreign key - NOT NULL
- department_id: (BIGINT) foreign key - NOT NULL
- role: VARCHAR(50) - NOT NULL

***************************************************************

## Table name: 'exam_sessions'

**columns**
- id (BIGINT) primary key - auto_increment - NOT NULL
- subject_id: (BIGINT) foreign key - NOT NULL
- teacher_id: (BIGINT) foreign key - NOT NULL
- date: DATE - NOT NULL
- time: TIME
- exam_session_expire: DATE - NOT NULL
- number_of_students: SMALLINT() - NOT NULL
- cfu: TINYINT - NOT NULL

***************************************************************

## Table name: 'students'

**columns**
- id (BIGINT) primary key - auto_increment - NOT NULL
- name: VARCHAR(255) - NOT NULL
- lastname: VARCHAR(255) - NOT NULL
- email: VARCHAR(255) - NOT NULL - UNIQUE
- date_of_birth: DATE() - NOT NULL
- address: VARCHAR(255) - NULL 
- registration_number: SMALLINT(6) - UNIQUE
- degree_course_id: (BIGINT) foreign key - NOT NULL
- year: YEAR() - NULL
- year_of_registration: YEAR() - NOT NULL

***************************************************************

## Table name: 'votes'

**columns**
- id (BIGINT) primary key - auto_increment - NOT NULL
- vote: TINYINT(2) - NOT NULL
- is_with_honours: TINYINT DEFAULT(0)
- student_id: (BIGINT) foreign key - NOT NULL
- exam_session_id: (BIGINT) foreign key - NOT NULL
- teacher_id: (BIGINT) foreign key - NOT NULL

*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_*_

- Selezionare tutti i nati nel 1990:

SELECT *
FROM `students`
WHERE YEAR(date_of_birth) = '1990'

- Selezionare tutti i corsi che valgono più di 10 crediti:

SELECT *
FROM `courses`
WHERE cfu > 10

- Selezionare tutti gli studenti che hanno più di 30 anni:

SELECT *
FROM `students`
WHERE YEAR(date_of_birth) <= 1995

- Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea:

SELECT *
FROM `courses`
WHERE `year` = 1
AND `period` = 'I semestre'

- Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020:

