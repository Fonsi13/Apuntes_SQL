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
     

### Introducción al lenguaje <a name="intro"></a>

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

A la hora de realizar una consulta esta generalmente tendrá una estructura determinada que puede variar. Contendrá:

 - Un SELECT, componente que selecciona los datos que queremos mostrar.
 - Un FROM, componente que indica la tabla, o tablas, de donde vamos a sacar los datos.
 - Un WHERE, componente en donde desarrollamos la condición que deben cumplir los datos a mostrar
 - Existen algunos componentes que nos ayudan a mostrar los datos ordenados, como ORDER BY, HAVING o GROUP BY.
 - Lo último, y más importante, que hay que colocar es un ";", para indicar el final de la consulta. Si no se escribe saltará un error.

[Volver al índice](#indice)

### Sintaxis <a name="sint"></a>

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
