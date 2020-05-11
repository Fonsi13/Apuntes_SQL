# Apuntes Lenguaje SQL
 
## Índice <a name="indice"></a>
 - [Introducción al lenguaje](#intro)
 - [Estructura](#estru)
 - [Sintaxis](#sint)
   - [SELECT](#sel)
     - [AS](#as)
   - [WHERE](#w)
     - [AND](#and)
     - [IN](#in)
     - [OR](#or)
     - [XOR](#xor)
     - [BETWEEN](#b)
     - [LIKE](#like)

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

Una variación del propio *SELECT* es *SELECT DISTINCT*, con el cuál estaríamos pidiendo que se muestre una sola vez el dato seleccionado, por ejemplo:
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

> Nombre cuya segunda letra sea una "l" y en total sean cinco caracteres

La versión opuesta a *LIKE* es *NOT LIKE*, la cual tendría las misma funciones que con el significado contrario: 

```SQL
SELECT name 
FROM world
WHERE name NOT LIKE 'a%n';
```

> Mostraría los nombre que no empiezan por "a" y a la vez no acaban en "n"

[Volver al índice](#indice)
