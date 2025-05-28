¡Perfecto\! Me encanta la idea de un taller práctico con errores intencionados para fomentar el análisis y el uso de IA. Aquí tienes un borrador de un taller de DDL y DML en MySQL, formateado en Markdown, con errores comunes y situaciones de versión, diseñado para que tus alumnos los identifiquen y corrijan.

Lo he estructurado para que puedas copiarlo y pegarlo directamente en un archivo .md o cualquier editor de texto que soporte Markdown.

# ---

**Taller Práctico: DDL y DML en MySQL \- Identificación y Corrección de Errores**

## **Objetivo del Taller**

Este taller tiene como objetivo reforzar los conocimientos sobre los lenguajes DDL (Data Definition Language) y DML (Data Manipulation Language) en MySQL. Se presentarán ejercicios que contienen errores comunes de sintaxis o de compatibilidad de versión. Los estudiantes deberán identificar la causa de estos errores y corregir el código, utilizando su propio análisis o mediante la formulación de *prompts* adecuados para herramientas de Inteligencia Artificial (como ChatGPT, Gemini, Copilot, etc.).

## **Herramientas Necesarias**

* Un servidor MySQL (versión 8.0 recomendada para este taller).  
* MySQL Workbench o un cliente de línea de comandos (CLI) como mysql.  
* Un editor de texto (VS Code, Sublime Text, Notepad++, etc.).  
* Acceso a una herramienta de IA (opcional, pero recomendado para el ejercicio de *prompts*).

## ---

**Parte 1: Errores en DDL (Data Definition Language)**

El DDL se utiliza para definir o modificar la estructura de la base de datos (tablas, índices, etc.).

### **Ejercicio 1: Creación de Base de Datos y Tablas**

Se desea crear una base de datos universidad y dos tablas: estudiantes y cursos.

**Código Propuesto (con Errores):**

SQL

\-- Ejercicio 1.1: Crear la base de datos  
USE universidad;  
CREATE DATABASE IF EXISTS universidad;

\-- Ejercicio 1.2: Crear la tabla 'estudiantes'  
USE universidad;  
CREATE TABLE estudiantes (  
    id\_estudiante INT PRIMARY KEY AUTO\_INCREMENT,  
    nombre VARCHAR(50) NOT NULL,  
    apellido VARCHAR(50) NOT NULL,  
    fecha\_nacimiento DATE,  
    email VARCHAR(100) UNIQUE,  
    carrera\_id INT,  
    \-- Intento de añadir una restricción CHECK (ERROR INTENCIONADO para MySQL \< 8.0.16)  
    CHECK (fecha\_nacimiento \< CURDATE())  
);

\-- Ejercicio 1.3: Crear la tabla 'cursos'  
USE universidad;  
CREATE TABLE cursos (  
    id\_curso INT PRIMARI KEY AUTO\_INCREMENT, \-- Error de sintaxis aquí  
    nombre\_curso VARCHAR(100) NOT NULL,  
    creditos INT DEFAULT 3,  
    departamento VARCHAR(50)  
);

\-- Ejercicio 1.4: Añadir una clave foránea (FOREIGN KEY)  
\-- Se quiere relacionar 'estudiantes' con 'cursos' a través de una columna 'curso\_actual\_id'  
ALTER TABLE estudiantes  
ADD COLUMN curso\_actual\_id INT;

ALTER TABLE estudiantes  
ADD CONSTRAINT fk\_curso\_actual  
FOREIGN KEY (curso\_actual\_id) REFERENCES cursos(id\_curso)  
ON DELETE SET NULL ON UPDATE CASCADE;

**Tarea para el Alumno:**

1. Ejecuta el código anterior en MySQL Workbench o tu CLI.  
2. Identifica todos los errores que MySQL te reporta.  
3. Para cada error:  
   * Describe el error (mensaje que muestra MySQL).  
   * Explica la causa del error.  
   * Propón la corrección del código SQL.  
   * **(Opcional)** Formula un *prompt* para una IA que te ayude a identificar y/o corregir este tipo de error.

### ---

**Ejercicio 2: Modificación y Eliminación de Estructuras**

Se desea modificar la tabla estudiantes y eliminar una columna.

**Código Propuesto (con Errores y Detalles de Versión):**

SQL

USE universidad;

\-- Ejercicio 2.1: Renombrar una columna  
\-- Se quiere cambiar 'carrera\_id' a 'id\_carrera\_principal'  
ALTER TABLE estudiantes  
CHANGE COLUMN carrera\_id id\_carrera\_principal INT; \-- Error si la versión es MySQL \< 8.0, o falta la definición completa del tipo

\-- Ejercicio 2.2: Mover una columna  
\-- Se quiere mover la columna 'email' para que quede después de 'apellido'  
ALTER TABLE estudiantes  
MODIFY COLUMN email VARCHAR(100) UNIQUE AFTER apellido; \-- Esto es correcto en MySQL 8.0, pero ¿qué pasa si el alumno usa una versión anterior y luego intenta moverla con BEFORE?  
                                                        \-- (Aquí se simulará el error de la lección previa si intentaran usar 'BEFORE' en 8.0)

\-- Simulación de error de compatibilidad de versión (solo ejecutar si se quiere ver el error)  
\-- ALTER TABLE estudiantes  
\-- MODIFY COLUMN email VARCHAR(100) UNIQUE BEFORE nombre; \-- ESTE CÓDIGO CAUSARÁ ERROR EN MySQL 8.0 (Error 1064\)

\-- Ejercicio 2.3: Eliminar una columna  
\-- Se desea eliminar la columna 'fecha\_nacimiento'  
ALTER TABLE estudiantes  
DROP COLUMN fecha\_nacimiento;

**Tarea para el Alumno:**

1. Ejecuta el código anterior.  
2. Presta especial atención al ALTER TABLE estudiantes CHANGE COLUMN y ALTER TABLE estudiantes MODIFY COLUMN.  
3. Si trabajas con MySQL 8.0, intenta descomentar y ejecutar la línea ALTER TABLE estudiantes MODIFY COLUMN email VARCHAR(100) UNIQUE BEFORE nombre; y analiza el error.  
4. Para cada error:  
   * Describe el error y su causa (posibles problemas de sintaxis o de versión de MySQL).  
   * Propón la corrección del código SQL.  
   * **(Opcional)** Formula un *prompt* para una IA que te ayude a entender por qué ciertos comandos de DDL se comportan diferente en distintas versiones de MySQL.

## ---

**Parte 2: Errores en DML (Data Manipulation Language)**

El DML se utiliza para manipular los datos dentro de las tablas (insertar, actualizar, eliminar, seleccionar).

### **Ejercicio 3: Inserción de Datos**

Se desea insertar datos en las tablas estudiantes y cursos.

**Código Propuesto (con Errores):**

SQL

USE universidad;

\-- Ejercicio 3.1: Insertar un estudiante  
INSERT INTO estudiantes (nombre, apellido, fecha\_nacimiento, email, carrera\_id)  
VALUES ('Ana', 'Gomez', '2000-10-25', 'ana.gomez@example.com', 101); \-- carrera\_id aún no tiene FK si no se crearon carreras

\-- Ejercicio 3.2: Insertar otro estudiante (ERROR DE SINTAXIS)  
INSERT INTO estudiantes (nombre, apellido, email)  
VALUES ('Juan', 'Perez', 'juan.perez@example.com' '1999-05-15'); \-- Faltan comas o se intenta insertar un valor extra sin columna

\-- Ejercicio 3.3: Insertar un curso  
INSERT INTO cursos (nombre\_curso, creditos, departamento)  
VALUES ('Matemáticas Discretas', 4, 'Ciencias Exactas');

\-- Ejercicio 3.4: Insertar un curso con error de tipo de dato  
INSERT INTO cursos (nombre\_curso, creditos, departamento)  
VALUES ('Historia Universal', 'Cinco', 'Humanidades'); \-- Error de tipo de dato

**Tarea para el Alumno:**

1. Ejecuta el código anterior.  
2. Identifica y describe los errores.  
3. Propón la corrección del código SQL.  
4. **(Opcional)** Formula un *prompt* para una IA que te ayude a depurar sentencias INSERT con errores de sintaxis o de tipo de dato.

### ---

**Ejercicio 4: Actualización y Eliminación de Datos**

Se desea actualizar y eliminar datos de las tablas.

**Código Propuesto (con Errores):**

SQL

USE universidad;

\-- Ejercicio 4.1: Actualizar el email de un estudiante  
UPDATE estudiantes  
SET email \= 'ana.gomez.nuevo@example.com'  
WHERE nombre \= 'Ana'; \-- Potencial error si hay más de una Ana o si 'nombre' no es único. Mejor usar ID.

\-- Ejercicio 4.2: Actualizar créditos de un curso (ERROR: falta la cláusula WHERE)  
UPDATE cursos  
SET creditos \= 5; \-- Actualizará TODOS los cursos

\-- Ejercicio 4.3: Eliminar un estudiante  
DELETE FROM estudiantes  
WHERE id\_estudiante \= 999; \-- Intentando eliminar un ID que probablemente no existe (no es un error de sintaxis, pero sí lógico)

\-- Ejercicio 4.4: Eliminar todos los datos de una tabla (cuidado\!)  
DELETE estudiantes; \-- Error de sintaxis

**Tarea para el Alumno:**

1. Ejecuta el código anterior.  
2. Identifica y describe los errores (sintácticos y lógicos/de buenas prácticas).  
3. Propón la corrección del código SQL, explicando por qué tu corrección es mejor.  
4. **(Opcional)** Formula un *prompt* para una IA que te ayude a entender las implicaciones de las sentencias UPDATE y DELETE sin cláusulas WHERE.

## ---

**Parte 3: Reflexión y Uso de IA**

### **Preguntas de Reflexión:**

1. ¿Qué tipo de errores te parecieron más fáciles de identificar? ¿Cuáles fueron los más difíciles?  
2. ¿Cómo te ayudó (o podría haberte ayudado) una IA en el proceso de depuración?  
3. ¿Qué *prompts* consideras más efectivos para obtener ayuda de una IA sobre errores SQL?  
4. ¿Qué aprendiste sobre la importancia de la versión de MySQL al escribir sentencias DDL?

### **Consejos para usar IA de forma efectiva:**

* **Sé específico:** En lugar de "Mi código SQL no funciona", prueba con "Estoy ejecutando este código SQL en MySQL 8.0 y recibo el Error Code: 1064 en esta línea: \[tu línea de código\]. ¿Cuál es la causa de este error y cómo puedo corregirlo?".  
* **Proporciona contexto:** Si el error se relaciona con una tabla, incluye el CREATE TABLE de la tabla relevante.  
* **Incluye el mensaje de error:** Siempre pega el mensaje de error completo que te da MySQL.  
* **Indica la versión de MySQL:** Crucial para problemas de compatibilidad.  
* **Pregunta por alternativas:** Si una función no está soportada, pregunta a la IA por la forma equivalente en tu versión de MySQL.

---

Espero que este taller sea muy útil para tus alumnos. ¡Puedes modificarlo, añadir más ejercicios o ajustar la dificultad según sea necesario\!

**Fuentes**  
1\. [https://github.com/leidylondono23212/Proyecto-JDBC](https://github.com/leidylondono23212/Proyecto-JDBC)