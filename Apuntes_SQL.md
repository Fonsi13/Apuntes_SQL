 # Apuntes Lenguaje SQL
 
## Índice <a name="indice"></a>
 - [Introducción al lenguaje](#intro)
 - [Estructura](#estru)
 - [Sintaxis](#sint)
   - [SELECT](#sel)
     - [AS](#as)
     - [REPLACE](#rp)
     - [ROUND](#rnd)
   - [WHERE](#w)
     - [AND](#and)
     - [IN](#in)
     - [OR](#or)
     - [XOR](#xor)
     - [BETWEEN](#b)
     - [LIKE](#like)
     - [CONCAT](#cct)
     - [LENGTH](#len)
     - [LEFT](#lft)
     - [NOT](#not)
     - [SELECTS Anidados](#ani)
     - [ALL](#all)
     - [SUM](#sum)
     - [COUNT](#cnt)
     - [AVG](#avg)
     - [MAX](#max)
     - [ORDER BY](#ord)
     - [GROUP BY](#grp)
     - [HAVING](#hav)
     - [JOIN](#join)
     - [CASE WHEN](#case)
     - [COALESCE](#coal)
     - [NULL](#null)
     

### Introducción al lenguaje <a name="intro"></a>
---
SQL es es un lenguaje utilizado en programación, diseñado para administrar, y recuperar información de sistemas de gestión de bases de datos relacionales.

SQL sirve para insertar datos, hacer consultas, actualizaciones y borrado, la creación y modificación de esquemas y el control de acceso a los datos de una base.

Actualmente está formado por seis sublenguajes:

 - DDL 
 - DML
 - DQL
 - TCL
 - DCL
 - SCL

Nosotros vamos a tratar con DQL, para poder realizar las consultas a una base de datos

[Volver al índice](#indice)

### Estructura <a name="estru"></a>
---
A la hora de realizar una consulta esta generalmente tendrá una estructura determinada que puede variar. Contendrá:

 - Un SELECT, componente que selecciona los datos que queremos mostrar.
 - Un FROM, componente que indica la tabla, o tablas, de donde vamos a sacar los datos.
 - Un WHERE, componente en donde desarrollamos la condición que deben cumplir los datos a mostrar
 - Existen algunos componentes que nos ayudan a mostrar los datos ordenados, como ORDER BY, HAVING o GROUP BY.
 - Lo último, y más importante, que hay que colocar es un ";", para indicar el final de la consulta. Si no se escribe saltará un error.

[Volver al índice](#indice)

### Sintaxis <a name="sint"></a>
---
##### SELECT  <a name="sel"></a>
---
Una consulta básica para ir probando el *SELECT* podría ser lo siguiente:
```SQL
SELECT population 
FROM world;
```
o
```SQL
SELECT name, population  
FROM world;
```

> Podemos seleccionar una o varias cosas a la vez, y siempre de una tabla existente

Una variación del propio *SELECT* es **SELECT DISTINCT**, con el cuál estaríamos pidiendo que se muestre una sola vez el dato seleccionado, por ejemplo:
```SQL
SELECT DISTINCT continent
FROM world;
```

> Las ejemplos son sacados de la página SQLZOO, donde podéis probar los ejemplos aquí descritos 

[Volver al índice](#indice)

##### AS <a name="as"></a>

Este componente permite que el nombre de un campo se muestre como nosotros queramos:
```SQL
SELECT name AS "País"
FROM world;
```

> También podemos ponerle un alias a la tabla

```SQL
SELECT name
FROM world AS "Mundo";
```

[Volver al índice](#indice)

##### REPLACE <a name="rp"></a>

Este componente nos permite remplazar unos caracteres de una cadena por otros que indiquemos.

Debemos indicar primero el campo que queremos modificar, después los caracteres a reemplazar y por último el remplazo:
```SQL
SELECT REPLACE(name, 'e', 'o')
FROM world;
```

> Mostramos el nombre de los países, pero donde debería haber una e hay una o

[Volver al índice](#indice)

##### ROUND <a name="rnd"></a>

Gracias a este componente podemos mostrar datos numéricos redondeados.

La sintaxis sería *ROUND(número,  valor)*, con valor nos referimos a la precisión con la que queremos aproximar:
```SQL
SELECT name, ROUND(population/1000000, 2), ROUND(GDP/1000000000, 2)
FROM world
WHERE continent = 'South America';
```
> Podemos redondear a las centésimas, como en el ejemplo anterior donde mostramos en millones la población y en billones el Producto Interior Bruto
```SQL
SELECT name, ROUND(GDP/population, -3)
FROM world
WHERE gdp >= 1000000000000;
```
> También podemos redondear hacia las decenas, centenas, millares,etc, pero en este caso debemos indicarlo con un número negativo

[Volver al índice](#indice)

##### WHERE <a name="w"></a>
---
Gracias a este componente podemos filtrar los resultados con una condición a la que llamaremos predicado.

Dentro de la condición se pueden usar diferentes operadores:

 - "=" : igual
 - "<" : menor que 
 -  ">" : mayor que 
 - "<=" : menor o igual que
 - ">=" : mayor o igual que
 - "<>" : no igual, distinto

```SQL
SELECT name
FROM world
WHERE area > 250000;
```
Existe otros operadores más complejos que explicaremos a continuación.

[Volver al índice](#indice)

##### AND <a name="and"></a>

Este componente obliga al dato a cumplir las dos condiciones obligatoriamente, si no cumple una devuelve false:
```SQL
SELECT name, area
FROM world
WHERE area > 1000000 AND population > 40000000;
```
[Volver al índice](#indice)

##### OR <a name="or"></a>

Este componente permite que el dato pueda cumplir una de las dos condiciones o ambas:
```SQL
SELECT name, area
FROM world
WHERE area > 1000000 OR population > 40000000;
```
[Volver al índice](#indice)

##### XOR <a name="xor"></a>

Este componente permite que el dato pueda cumplir una condición o la otra, nunca ambas:
```SQL
SELECT name, area
FROM world
WHERE area > 1000000 XOR population > 40000000;
```
[Volver al índice](#indice)

##### IN  <a name="in"></a>

Este componente nos ayuda a comprobar si un dato está en una lista:
```SQL
SELECT name, population 
FROM world
WHERE name IN ('Brazil', 'Russia', 'India', 'China');
```
[Volver al índice](#indice)

##### BETWEEN <a name="b"></a>

Este componente nos ayuda a comprobar si un dato está en un rango que determinamos, incluyendo los valores límite:
```SQL
SELECT name, area 
FROM world
WHERE area BETWEEN 250000 AND 300000;
```
[Volver al índice](#indice)

##### LIKE <a name="like"></a>

Este componente nos ayuda a filtrar datos comparándolos con un patrón específico.

En estos patrones hay dos caracteres clave:

 - "%" : indica cualquier número de caracteres
 - "_" : indica  un espacio en blanco

Se pueden formar desde patrones simples:
```SQL
SELECT name 
FROM world
WHERE name LIKE '%a';
```

> Muestra aquellos nombres terminados en a

```SQL
SELECT name 
FROM world
WHERE name LIKE '%United%';
```

> Muestra aquellos nombres que contengan la palabra "United"

Hasta patrones más complejos:
```SQL
SELECT name 
FROM world
WHERE name LIKE '%o%o%o%';
```

> El mismo caracter repetido varias veces pero separado por otros caracteres

```SQL
SELECT name 
FROM world
WHERE name LIKE '_t___';
```

> Nombre cuya segunda letra sea una "t" y en total sean cinco caracteres

La versión opuesta a *LIKE* es **NOT LIKE**, la cual tendría las misma funciones que con el significado contrario: 

```SQL
SELECT name 
FROM world
WHERE name NOT LIKE 'a%n';
```

> Mostraría los nombre que no empiezan por "a" y a la vez no acaban en "n"

[Volver al índice](#indice)

##### CONCAT <a name="cct"></a>

Usando este componente podemos concatenar valores, creando condiciones como la siguiente:
```SQL
SELECT name, capital
FROM world
WHERE capital LIKE CONCAT('%', name, '%');
```

> Seleccionamos los países cuya capital contenga el nombre del propio país

[Volver al índice](#indice)

##### LENGTH <a name="len"></a>

Este componente nos va a devolver el total de caracteres que contenga la cadena que le pasamos:
```SQL
SELECT name, capital
FROM world
WHERE LENGTH(name) = LENGTH(capital);
```

> Seleccionamos los países cuya capital contenga el mismo número de caracteres que el nombre del propio país

```SQL
SELECT name, LENGTH(name) AS 'Cantidad de letras'
FROM world;
```

> También lo podemos usar en el SELECT si queremos

[Volver al índice](#indice)

##### LEFT <a name="lft"></a>

Podemos  extraer *n* caracteres del principio de la cadena que le pasemos a este componente:

```SQL
SELECT name, capital
FROM world
WHERE LEFT(name,1) = LEFT(capital,1) AND name<>capital;
```

> Saca los países cuya capital empieza por la misma letra, sin tener ambas el mismo nombre

```SQL
SELECT name, LEFT(name, 3) AS 'INICIALES'
FROM world;
```
> Podemos usarlo en el SELECT también, mostrando las iniciales de los países por ejemplo

[Volver al índice](#indice)

##### NOT <a name="not"></a>

Usamos este componente para excluir valores en un condición:
```SQL
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine');
```

> Seleccionamos los ganadores de un nobel en 1980, excluyendo los campos de Medicina y Química

[Volver al índice](#indice)

##### SELECTS Anidados <a name="ani"></a>
Podemos hacer predicados bastantes complejos haciendo una subconsulta en la condición, esta debe ir en paréntesis:
```SQL
SELECT name, population
FROM where
WHERE population > (SELECT population
					FROM world
					WHERE name = 'Brasil');
```
> Sacamos los países que tengan una población mayor que Brasil

Existe otro tipo de consultas del mismo estilo, llamadas **SUBCONSULTA SINCRONIZADA** o **SUBCONSULTA CORRELACIONADA**, en la que la subconsulta toma datos de la consulta de fuera:
```SQL
SELECT name
FROM world
WHERE population >= ALL(SELECT population
                        FROM world
                        WHERE population>0);
```

> Nos muestra el país con más población del mundo

[Volver al índice](#indice)

##### ALL <a name="all"></a>

En la última consulta de ejemplo he usado un componente que no había explicado, ALL.

Este componente sirve para comprobar que todas las tuplas cumplen la condición:
```SQL
SELECT continent, name, area
FROM world x
WHERE area >= ALL (SELECT area
                   FROM world y
                   WHERE y.continent=x.continent
                   AND area>0);
```

> Seleccionamos cada continente y su país más extenso

[Volver al índice](#indice)

##### SUM <a name="sum"></a>

Este componente devuelve el total de los valores de las tuplas de un campo numérico:
```SQL
SELECT SUM(population) AS 'Poblacion'
FROM world;
```

> Nos devuelve la población mundial

[Volver al índice](#indice)

##### COUNT <a name="cnt"></a>

Este componente devuelve la cantidad de valores que cumplen el predicado:
```SQL
SELECT COUNT(name)
FROM world
WHERE area >= 1000000;
```

> Nos devuelve la cantidad de países con un área como mínimo de 1000000 km2

[Volver al índice](#indice)

##### AVG <a name="avg"></a>

Este componente devuelve el valor promedio de una columna numérica:
```SQL
SELECT AVG(population)
FROM world;
```

> Nos devuelve la población media mundial

[Volver al índice](#indice)

##### MAX <a name="max"></a>

Este componente devuelve el valor máximo de una columna numérica:
```SQL
SELECT MAX(population)
FROM world;
```

> Nos devuelve la mayor población que existe

[Volver al índice](#indice)

##### ORDER BY <a name="ord"></a>

Con este componente tenemos la posibilidad de ordenar los resultados de manera ascendente (ASC) o descendiente (DESC):

> Por defecto es en orden ascendente

```SQL
SELECT name, population
FROM world
ORDER BY population DESC;
```

> Nos devuelve los países y su población, ordenados de mayor a menor habitantes

[Volver al índice](#indice)

##### GROUP BY <a name="grp"></a>

Con este componente podemos agrupar aquellas tuplas que tengan el mismo resultado:

```SQL
SELECT continent, COUNT(name) AS 'Nº Países'
FROM world
GROUP BY continent;
```

> Nos devuelve los continentes y el número de países de cada uno

[Volver al índice](#indice)

##### HAVING <a name="hav"></a>

Este componente sirve como flitro a la hora de agrupar con *GROUP BY*, podemos aplicar los agregados antes explicados como SUM, COUNT, etc.:

```SQL
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) > 1000000000;
```

> Nos devuelve los continentes cuya población supera los mil millones

[Volver al índice](#indice)

##### JOIN <a name="join"></a>

Si queremos usar dos o más tablas en una consulta debemos usar este componente, que resultará en otra tabla con la suma de las columnas de ambas. Para un mejor uso se añade el componente ON, que nos permite especificar en que campos coinciden las dos tablas:

```SQL
SELECT player, teamid, stadium, mdate
FROM game JOIN goal ON (game.id = goal.matchid)
WHERE teamid = 'GER';
```

> Entre paréntesis van los atributos coincidentes de las dos tablas, para evitar una confusión es preferible definirlos como "tabla.atributo"

Existen diferentes tipos de JOINs:

 - INNER JOIN: funciona igual que un JOIN normal, pero se salta los valores nulos
 - LEFT JOIN: saca todos los elementos de la tabla a la izquierda, aunque en la derecha sean nulos
 - RIGHT JOIN: saca todos los elementos de la tabla a la derecha, aunque en la izquierda sean nulos

```SQL
SELECT teacher.name, dept.name
FROM teacher INNER JOIN dept ON (teacher.dept=dept.id);
```
```SQL
SELECT teacher.name, dept.name
FROM teacher LEFT JOIN dept ON (teacher.dept=dept.id);
```
```SQL
SELECT teacher.name, dept.name
FROM teacher RIGHT JOIN dept ON (teacher.dept=dept.id);
```
[Volver al índice](#indice)

##### CASE WHEN <a name="case"></a>

Este componente nos permite devolver diferentes valores en función de diferentes condiciones.

En caso de que no sea de ninguna condición, y no haya *ELSE*, devolverá el valor NULL. 

```SQL
SELECT name,  
CASE dept WHEN 1 THEN 'Sci'
          WHEN 2 THEN 'Sci'
          ELSE 'Art'
END 
FROM teacher;
```

> Dependiendo del departamento en el que esté, se le asignará o "Sci" o "Art"

[Volver al índice](#indice)

##### COALESCE <a name="coal"></a>

Le pasaremos cualquier número de parámetros a este componente y nos devolverá el primero que no sea nulo.

```SQL
  COALESCE(x,y,z) = x si x no es NULL
  COALESCE(x,y,z) = y si x es NULL e y no es NULL
  COALESCE(x,y,z) = z si x e y son NULL pero z no es NULL
  COALESCE(x,y,z) = NULL si x,y,z son todas NULL
```
Ejemplo:
```SQL
SELECT name,
       COALESCE(mobile,'07986 444 2266') AS 'mobile number'
FROM teacher;
```
> Muestra el número de móvil de los profesores, si no tienen, muestra '07986 444 2266'

[Volver al índice](#indice)

##### NULL <a name="null"></a>

Con este componente filtramos aquellos datos que son o no nulos.

```SQL
SELECT name
FROM teacher
WHERE dept IS NULL;
```
> Muestra los profesores que **no tienen** un departamento asignado

```SQL
SELECT name
FROM teacher
WHERE dept IS NOT NULL;
```
> Muestra los profesores que **tienen** un departamento asignado

[Volver al índice](#indice)
