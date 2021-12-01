# SQL2-CustomsExercises

Ejercitación de base de datos utilizando SQL  
temas: 
- Funciones de agregación
- Consultas multitablas
- Índices
- [Queries anidadas](#Queries-Anidadas)

## ***Funciones de agregación***
A partir de las siguientes tablas...  
![imagen](https://user-images.githubusercontent.com/65373208/143155914-3d0c3b99-b30f-41c5-994d-471188043b91.png)  

y de los siguientes datos...

***tabla estudiante***  
![imagen](https://user-images.githubusercontent.com/65373208/143157315-20084240-59b0-4523-8a42-6c118e9c6aac.png)

***tabla curso***  
![imagen](https://user-images.githubusercontent.com/65373208/143157450-563de471-a79e-42d4-a073-a7d92b513435.png)

***tabla inscripcion***  
![imagen](https://user-images.githubusercontent.com/65373208/143157492-c86e45b2-cae7-46c9-b660-d1b8aa874219.png)

***tabla profesor***  
![imagen](https://user-images.githubusercontent.com/65373208/143157613-3cadc296-fad2-443f-92e0-9295d6715bc8.png)

> Escriba una consulta para saber cuántos estudiantes son de la carrera Mecánica.
```
SELECT COUNT(*) AS 'Alumnos de Mecanica' FROM estudiante WHERE carrera = 'Mecanica'
```

> Escriba una consulta para saber, de la tabla PROFESOR, el salario mínimo de los profesores nacidos en la década del 80.
```
SELECT MIN(salario) AS "Salario mínimo" FROM profesor WHERE fecha_nacimiento BETWEEN "1980-01-01" AND "1981-12-31"
```

>Para el siguiente modelo relacional:  
>![imagen](https://user-images.githubusercontent.com/65373208/143159916-593cc6aa-7596-422a-a662-c9f865266aa3.png)
>
> Escriba las siguientes consultas:
> 1. Cantidad de pasajeros por país
```
SELECT p.nombre, COUNT(*) AS "cant pasajeros" FROM pasajero ps INNER JOIN pais p ON ps.idpais=p.idpais GROUP BY p.idpais
```
> 2. Suma de todos los pagos realizados
```
SELECT SUM(p.monto) AS "suma pagos" FROM pago p
```
> 3. Suma de todos los pagos que realizó un pasajero. El monto debe aparecer con dos decimales.
```
SELECT idpasajero, TRUNCATE(SUM(monto),2) AS "suma todos los pagos" FROM pago WHERE idpasajero=1 GROUP BY idpasajero
```
> 4. Promedio de los pagos que realizó un pasajero.
```
SELECT idpasajero, AVG(monto) AS "promedio de todos los pagos" FROM pago WHERE idpasajero=1 GROUP BY idpasajero
```
## Consultas Multitabla

> 1. Nombre, apellido y cursos que realiza cada estudiante  
```
SELECT e.nombre,e.apellido, c.nombre AS 'Nombre del curso', c.descripcion, c.codigo FROM estudiante e 
	INNER JOIN inscripcion i ON e.legajo=i.estudiante_legajo
		INNER JOIN curso c WHERE i.curso_codigo = c.codigo
```
> 2. Nombre, apellido y cursos que realiza cada estudiante, ordenados por el nombre del curso  
```
SELECT e.nombre,e.apellido, c.nombre AS 'Nombre del curso', c.descripcion, c.codigo FROM estudiante e 
	INNER JOIN inscripcion i ON e.legajo=i.estudiante_legajo
		INNER JOIN curso c WHERE i.curso_codigo = c.codigo ORDER BY c.nombre
```
> 3. Nombre, apellido y cursos que dicta cada profesor  
```
SELECT p.nombre, p.apellido, c.nombre AS 'Nombre del curso'
 FROM curso c INNER JOIN profesor p ON p.id = c.profesor_id
```
> 4. Nombre, apellido y cursos que dicta cada profesor, ordenados por el nombre del curso  
```
SELECT p.nombre, p.apellido, c.nombre AS 'Nombre del curso'
 FROM curso c INNER JOIN profesor p ON p.id = c.profesor_id order by c.nombre
```
> 5. Cupo disponible para cada curso (Si el cupo es de 35 estudiantes y hay 5 inscriptos, el cupo disponible será 30)  
```
SELECT nombre, cupo - COUNT(curso_codigo) AS "cupos disponibles" 
	FROM curso c LEFT JOIN inscripcion i 
	ON c.codigo=i.curso_codigo
	GROUP BY curso_codigo
```
> 6. Cursos cuyo cupo disponible sea menor a 10
```
SELECT nombre, cupo - COUNT(*) AS "cupos disponibles" 
	FROM curso c LEFT JOIN inscripcion i 
	ON c.codigo=i.curso_codigo
	GROUP BY curso_codigo HAVING COUNT(*)>=10
```

## Índices

> Crear una tabla persona sin primary key y solamente con dos campos: id y nombre
```
CREATE TABLE `persona` (
  `id` INT(11) NOT NULL,
  `nombre` VARCHAR(20) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=latin1;
```
> Inserte datos siguiendo un orden no secuencial para el id.
```
INSERT INTO persona (id, nombre) VALUES (3, "Damian");
INSERT INTO persona (id, nombre) VALUES (5, "Julian");
INSERT INTO persona (id, nombre) VALUES (1, "Braian");
```
> Consulte los datos para visualizar el orden de registros.  

![imagen](https://user-images.githubusercontent.com/65373208/143690397-250d4854-487a-40b2-8488-82604677a4bb.png)  

> Agregue una clave primaria para el campo id.
```
ALTER TABLE persona ADD PRIMARY KEY (id);
```
> Consulte los datos para visualizar el orden de registros.
  
![imagen](https://user-images.githubusercontent.com/65373208/143690377-21a4b6ad-c2a2-437b-b4b8-039197779880.png)


## Queries Anidadas

> Escriba una consulta que devuelva la cantidad de profesores que dictan más de un curso en el turno Noche.
```
SELECT COUNT(*) FROM (
	SELECT p.nombre, COUNT(*) FROM profesor p 
		INNER JOIN curso c ON p.id = c.profesor_id 
		WHERE c.turno = "Noche"
		GROUP BY profesor_id HAVING COUNT(*)>1
		) AS query_anidada
```
> Escriba una consulta para obtener la información de todos los estudiantes que no realizan el curso con código 105.
```
SELECT * FROM estudiante e WHERE e.legajo NOT IN (
	SELECT estudiante_legajo FROM inscripcion i WHERE curso_codigo = 105)
```

## Ejercitación Final

> Escriba una consulta que devuelva el legajo y la cantidad de cursos que realiza cada estudiante.  
```
SELECT legajo, COUNT(legajo) AS "Cantidad de cursos que realiza" FROM estudiante e 
	INNER JOIN inscripcion i 
	ON i.estudiante_legajo = e.legajo
	GROUP BY legajo
```
> Escriba una consulta que devuelva el legajo y la cantidad de cursos de los estudiantes que realizan más de un curso.  
```
SELECT legajo, COUNT(legajo) AS "estudiantes que realizan más de un curso" FROM estudiante e 
	INNER JOIN inscripcion i 
	ON i.estudiante_legajo = e.legajo
	GROUP BY legajo HAVING COUNT(*)>1
```
> Escriba una consulta que devuelva la información de los estudiantes que no realizan ningún curso.  
```
SELECT * FROM estudiante e WHERE e.legajo NOT IN 
	(SELECT estudiante_legajo FROM inscripcion i)
```
> Escriba para cada tabla, los index (incluyendo su tipo) que tiene cada una.  
```
Tabla PROFESOR, columna "id", tipo Primary  
Tabla CURSO, columna "codigo", tipo Primary  
	Tabla CURSO, columna "PROFESOR_id", tipo KEY  
Tabla INSCRIPCION, columna "numero", tipo Primary  
	Tabla INSCRIPCION, columna "CURSO_codigo", tipo KEY  
	Tabla INSCRIPCION, columna "ESTUDIANTE_legajo", tipo KEY  
Tabla ESTUDIANTE, columna "legajo", tipo Primary
```
> Liste toda la información sobre los estudiantes que realizan cursos con los profesores de apellido “Pérez” y “Paz”.  
```
SELECT * FROM estudiante e 
	INNER JOIN inscripcion i ON e.legajo = i.estudiante_legajo
	INNER JOIN curso c ON i.curso_codigo = c.codigo
	INNER JOIN profesor p ON c.profesor_id = p.id
		WHERE p.apellido IN ("Pérez", "Paz")
```
