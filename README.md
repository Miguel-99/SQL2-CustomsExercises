# SQL2-CustomsExercises

Ejercitación de base de datos utilizando SQL  
temas: ***funciones de agregación, agrupación de datos e índices***

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
