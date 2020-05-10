 # Apuntes Lenguaje SQL
 
## Índice <a name="indice"></a>
 - [Introducción al lenguaje](#intro)
 - [Estructura](#estru)
 - [Sintaxis](#sint)

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
