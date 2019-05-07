---
title: Sintaxis de lenguaje SQL en Azure Cosmos DB
description: En este artículo se explica la sintaxis del lenguaje de consulta SQL usada en Azure Cosmos DB, distintos operadores y palabras clave disponibles en este lenguaje.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: 1d874b9c8f14b1489ab5e5b8bbdddaff0669165e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145193"
---
# <a name="sql-language-reference-for-azure-cosmos-db"></a>Referencia de lenguaje SQL de Azure Cosmos DB 

Azure Cosmos DB admite consultar documentos mediante una instancia de SQL (lenguaje de consulta estructurado) familiar, como gramática, en documentos JSON jerárquicos sin necesidad de esquemas explícitos ni de índices secundarios. En este artículo se proporciona documentación para la sintaxis del lenguaje de consulta SQL utilizada en las cuentas de API de SQL. Para ver un tutorial de ejemplos de consultas SQL, consulte [ejemplos de consultas SQL en Cosmos DB](how-to-sql-query.md).  
  
Visite el [Query Playground](https://www.documentdb.com/sql/demo), donde puede probar Cosmos DB y ejecutar consultas SQL en un conjunto de datos de ejemplo.  
  
## <a name="select-query"></a>Consulta SELECT  
Todas las consultas constan de una cláusula SELECT y cláusulas FROM y WHERE opcionales por estándares ANSI-SQL. Normalmente, para cada consulta, se enumera el origen en la cláusula FROM, a continuación, se aplica el filtro en la cláusula WHERE en el origen para recuperar un subconjunto de documentos JSON. Por último, la cláusula SELECT se usa para proyectar los valores JSON solicitados en la lista seleccionada. Para ver ejemplos, consulte [Ejemplos de consultas SELECT](how-to-sql-query.md#SelectClause)
  
**Sintaxis**  
  
```sql
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ] 
    [ OFFSET <offset_amount> LIMIT <limit_amount>]
```  
  
 **Comentarios:**  
  
 Consulte las siguientes secciones para más información sobre las cláusulas:  
  
-   [Cláusula SELECT](#bk_select_query)    
-   [Cláusula FROM](#bk_from_clause)    
-   [Cláusula WHERE](#bk_where_clause)    
-   [Cláusula ORDER BY](#bk_orderby_clause)  
-   [Cláusula de límite de desplazamiento](#bk_offsetlimit_clause)

  
Las cláusulas de la instrucción SELECT se deben ordenar como se indicó anteriormente. Se puede omitir cualquiera de las cláusulas opcionales. Pero cuando se usen, deben aparecer en el orden correcto.  
  
### <a name="logical-processing-order-of-the-select-statement"></a>Orden de procesamiento lógico de la instrucción SELECT  
  
El orden en que se procesan las cláusulas es el siguiente:  

1.  [Cláusula FROM](#bk_from_clause)  
2.  [Cláusula WHERE](#bk_where_clause)  
3.  [Cláusula ORDER BY](#bk_orderby_clause)  
4.  [Cláusula SELECT](#bk_select_query)
5.  [Cláusula de límite de desplazamiento](#bk_offsetlimit_clause)

Tenga en cuenta que difiere del orden en que aparecen en la sintaxis. La ordenación es tal que todos los símbolos nuevos introducidos por una cláusula procesada son visibles y pueden utilizarse en las cláusulas que se procesen después. Por ejemplo, las cláusulas WHERE y SELECT pueden acceder a los alias declarados en una cláusula FROM.  

### <a name="whitespace-characters-and-comments"></a>Comentarios y caracteres de espacio en blanco  

Todos los caracteres de espacio en blanco que no formen parte de una cadena o identificador entrecomillados no forman parte de la gramática del lenguaje y se omiten durante el análisis.  

El lenguaje de consulta admite comentarios del estilo de T-SQL como  

-   Instrucción SQL `-- comment text [newline]`.  

Aunque los comentarios y los caracteres de espacio en blanco no tienen significado en la gramática, deben utilizarse para separar tokens. Por ejemplo: `-1e5` es un token de un solo número, mientras que `: – 1 e5` es un token de menos seguido por el número 1 y el identificador e5.  

##  <a name="bk_select_query"></a> Cláusula SELECT  
Las cláusulas de la instrucción SELECT se deben ordenar como se indicó anteriormente. Se puede omitir cualquiera de las cláusulas opcionales. Pero cuando se usen, deben aparecer en el orden correcto. Para ver ejemplos, consulte [Ejemplos de consultas SELECT](how-to-sql-query.md#SelectClause).

**Sintaxis**  

```sql
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | [DISTINCT] <object_property_list>   
      | [DISTINCT] VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Argumentos**  
  
- `<select_specification>`  

  Propiedades o valor que se van a seleccionar para los resultados.  
  
- `'*'`  

  Especifica que se debe recuperar el valor sin cambios. En particular, si el valor procesado es un objeto, se recuperarán todas las propiedades.  
  
- `<object_property_list>`  
  
  Especifica la lista de propiedades que se recuperan. Los valores devueltos serán objetos con las propiedades especificadas.  
  
- `VALUE`  

  Especifica que se debe recuperar el valor JSON en lugar del objeto JSON completo. Esto, a diferencia de `<property_list>`, no encapsula el valor proyectado en un objeto.  
 
- `DISTINCT`
  
  Especifica que se deben quitar los duplicados de propiedades proyectadas.  

- `<scalar_expression>`  

  Expresión que representa el valor que hay que calcular. Consulte la sección [Expresiones escalares](#bk_scalar_expressions) para más información.  
  
**Comentarios:**  
  
La sintaxis `SELECT *` solo es válida si la cláusula FROM ha declarado exactamente un alias. `SELECT *` proporciona una proyección de identidad, que puede resultar útil si no se necesitan proyecciones. SELECT * solo es válida si se especifica cláusula FROM y se introdujo solo un origen de entrada.  
  
`SELECT <select_list>` y `SELECT *` son código sintáctico y pueden expresarse también mediante instrucciones SELECT sencillas, como se muestra a continuación.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   equivale a:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   equivale a:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Consulte también**  
  
[Expresiones escalares](#bk_scalar_expressions)  
[Cláusula SELECT](#bk_select_query)  
  
##  <a name="bk_from_clause"></a> Cláusula FROM  
Especifica el origen o la combinación de orígenes. La cláusula FROM es opcional, a menos que el origen se filtre o se proyecte posteriormente en la consulta. El objetivo de esta cláusula es especificar el origen de datos sobre el que debe operar la consulta. Normalmente, todo el contenedor es el origen pero, en su lugar, puede especificarse un subconjunto del contenedor. Si esta cláusula no se especifica, las demás se continuarán ejecutando como si la cláusula FROM proporcionara un documento único. Para ver ejemplos, consulte [Ejemplos de la cláusula FROM](how-to-sql-query.md#FromClause)
  
**Sintaxis**  
  
```sql  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <container_expression> [[AS] input_alias]  
        | input_alias IN <container_expression>  
  
<container_expression> ::=   
        ROOT   
     | container_name  
     | input_alias  
     | <container_expression> '.' property_name  
     | <container_expression> '[' "property_name" | array_index ']'  
```  
  
**Argumentos**  
  
- `<from_source>`  
  
  Especifica un origen de datos, con o sin alias. Si no se especifica, se deducirá de `<container_expression>` con las siguientes reglas:  
  
  -  Si la expresión es un nombre de contenedor, se utilizará container_name como alias.  
  
  -  Si la expresión es `<container_expression>`, se utilizará property_name como alias. Si la expresión es un nombre de contenedor, se utilizará container_name como alias.  
  
- AS `input_alias`.  
  
  Especifica que `input_alias` es un conjunto de valores que la expresión del contenedor subyacente ha devuelto.  
 
- `input_alias` IN  
  
  Especifica que `input_alias` debe representar el conjunto de valores obtenidos al recorrer en iteración todos los elementos de las matrices que devuelve la expresión del contenedor subyacente. Se omite cualquier valor que la expresión de contenedor subyacente devuelva que no sea una matriz.  
  
- `<container_expression>`  
  
  Especifica la expresión de contenedor que se usará para recuperar los documentos.  
  
- `ROOT`  
  
  Especifica que debe recuperarse ese documento predeterminado del contenedor actualmente conectado.  
  
- `container_name`  
  
  Especifica que debe recuperarse ese documento del contenedor indicado. El nombre del contenedor debe coincidir con el nombre del contenedor conectado actualmente.  
  
- `input_alias`  
  
  Especifica que debe recuperarse ese documento desde otro origen que define el alias proporcionado.  
  
- `<container_expression> '.' property_`  
  
  Especifica que debe recuperarse ese documento mediante el acceso a la propiedad `property_name` o al elemento de matriz array_index de todos los documentos que ha recuperado la expresión de contenedor específica.  
  
- `<container_expression> '[' "property_name" | array_index ']'`  
  
  Especifica que debe recuperarse ese documento mediante el acceso a la propiedad `property_name` o al elemento de matriz array_index de todos los documentos que ha recuperado la expresión de contenedor específica.  
  
**Comentarios:**  
  
Todos los alias proporcionados o deducidos en `<from_source>(` deben ser únicos. La sintaxis de property_name de `<container_expression>.` es la misma que la de `<container_expression>' ['"property_name"']'`. Sin embargo, esta última puede usarse si un nombre de propiedad contiene algún carácter que no sea un identificador.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>Control de propiedades que faltan, elementos de matriz que faltan y valores sin definir
  
Si una expresión de contenedor accede a propiedades o elementos de matriz y el valor no existe, ese valor se omitirá y no se seguirá procesando.  
  
### <a name="container-expression-context-scoping"></a>Ámbito de contexto de la expresión del contenedor  
  
El ámbito de las expresiones de contenedor puede ser de contenedor o de documento:  
  
-   Una expresión es de ámbito de contenedor si el origen subyacente de la expresión de contenedor es ROOT o `container_name`. Este tipo de expresión representa un conjunto de documentos que se recuperan directamente del contenedor y no dependen del procesamiento de otras expresiones de contenedor.  
  
-   Una expresión es de ámbito de documento si el origen subyacente de la expresión de contenedor es el valor `input_alias` introducido anteriormente en la consulta. Esta expresión representa un conjunto de documentos obtenidos al evaluar la expresión de contenedor en el ámbito de cada documento perteneciente al conjunto asociado con el contenedor del alias.  El conjunto de resultados será una combinación de los conjuntos obtenidos al evaluar la expresión de contenedor para cada uno de los documentos del conjunto subyacente.  
  
### <a name="joins"></a>Combinaciones 
  
En la versión actual, Cosmos DB admite combinaciones internas. Las funcionalidades de combinación adicionales estarán disponibles próximamente. 

Las combinaciones internas derivan en un producto cruzado completo con los conjuntos participantes en la combinación. El resultado de una combinación de N es un conjunto de tuplas de N elementos, donde cada valor de la tupla se asocia con el conjunto del alias participante en la combinación, accesible mediante una referencia a ese alias en otras cláusulas. Para ver ejemplos, consulte [Ejemplos de palabras clave de JOIN](how-to-sql-query.md#Joins)
  
La evaluación de la combinación depende del ámbito del contexto de los conjuntos participantes:  
  
- La combinación entre un conjunto de ámbito de contenedor A y otro B da como resultado un producto cruzado de todos los elementos de los conjuntos A y B.
  
- La combinación entre un conjunto A y un conjunto de ámbito de documento B da como resultado una unión de todos los conjuntos obtenidos de la evaluación de B para cada documento del conjunto A.  
  
  En la versión actual, el procesador de consultas admite un máximo de una expresión de ámbito de contenedor.  
  
### <a name="examples-of-joins"></a>Ejemplos de combinaciones  
  
Observemos la siguiente cláusula FROM: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Los orígenes definirán `input_alias1, input_alias2, …, input_aliasN`. Esta cláusula FROM devuelve un conjunto de N tuplas (tupla con N valores). Cada tupla tiene valores generados mediante la iteración de todos los alias de contenedor en los conjuntos correspondientes.  
  
**Ejemplo 1**: 2 orígenes  
  
- `<from_source1>` tendrá ámbito de contenedor y representa el conjunto {A, B, C}.  
  
- `<from_source2>` tendrá ámbito de documento, hará referencia a input_alias1 y representará los conjuntos:  
  
    {1, 2} para `input_alias1 = A,`  
  
    {3} para `input_alias1 = B,`  
  
    {4, 5} para `input_alias1 = C,`  
  
- La cláusula FROM `<from_source1> JOIN <from_source2>` dará como resultado las siguientes tuplas:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
**Ejemplo 2**: 3 orígenes  
  
- `<from_source1>` tendrá ámbito de contenedor y representa el conjunto {A, B, C}.  
  
- `<from_source2>` tendrá ámbito de documento, hará referencia a `input_alias1` y representará los conjuntos:  
  
    {1, 2} para `input_alias1 = A,`  
  
    {3} para `input_alias1 = B,`  
  
    {4, 5} para `input_alias1 = C,`  
  
- `<from_source3>` tendrá ámbito de documento, hará referencia a `input_alias2` y representará los conjuntos:  
  
    {100, 200} para `input_alias2 = 1,`  
  
    {300} para `input_alias2 = 3,`  
  
- La cláusula FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` dará como resultado las siguientes tuplas:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
  > [!NOTE]
  > Faltan tuplas para otros valores de `input_alias1` y `input_alias2`, para los cuales `<from_source3>` no devolvió ningún valor.  
  
**Ejemplo 3**: 3 orígenes  
  
- <from_source1> tendrá ámbito de contenedor y representa el conjunto {A, B, C}.  
  
- `<from_source1>` tendrá ámbito de contenedor y representa el conjunto {A, B, C}.  
  
- <from_source2> tendrá ámbito de documento, hará referencia a input_alias1 y representará los conjuntos:  
  
    {1, 2} para `input_alias1 = A,`  
  
    {3} para `input_alias1 = B,`  
  
    {4, 5} para `input_alias1 = C,`  
  
- `<from_source3>` tendrá el ámbito `input_alias1` y representará los conjuntos:  
  
    {100, 200} para `input_alias2 = A,`  
  
    {300} para `input_alias2 = C,`  
  
- La cláusula FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` dará como resultado las siguientes tuplas:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300), (C, 5, 300)  
  
  > [!NOTE]
  > Esto dio como resultado un producto cruzado entre `<from_source2>` y `<from_source3>`, ya que ambos tienen como ámbito el mismo `<from_source1>`.  Esto dio como resultado 4 (2 x 2) tuplas con valor A, 0 tuplas con valor B (1 x 0) y 2 (2 x 1) tuplas con valor C.  
  
**Consulte también**  
  
 [Cláusula SELECT](#bk_select_query)  
  
##  <a name="bk_where_clause"></a> Cláusula WHERE  
 Especifica la condición de búsqueda para los documentos que devuelve la consulta. Para ver ejemplos, consulte [Ejemplos de la cláusula WHERE](how-to-sql-query.md#WhereClause)
  
 **Sintaxis**  
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Argumentos**  
  
- `<filter_condition>`  
  
   Especifica la condición que debe cumplirse para que se devuelvan los documentos.  
  
- `<scalar_expression>`  
  
   Expresión que representa el valor que hay que calcular. Consulte la sección [Expresiones escalares](#bk_scalar_expressions) para más información.  
  
  **Comentarios:**  
  
  Para que el documento se devuelva, debe establecerse en true una expresión especificada como condición de filtro. Solo un valor booleano true cumplirá la condición, los demás valores (undefined, null, false, número, matriz u objeto) no cumplirán la condición.  
  
##  <a name="bk_orderby_clause"></a> Cláusula ORDER BY  
 Especifica el criterio de ordenación para los resultados que devuelve la consulta. Para ver ejemplos, consulte [Ejemplos de la cláusula ORDER BY](how-to-sql-query.md#OrderByClause)
  
 **Sintaxis**  
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
  
```  

 **Argumentos**  
  
- `<sort_specification>`  
  
   Especifica una propiedad o una expresión para ordenar el conjunto de resultados de la consulta. Puede especificarse una columna de ordenación como alias de nombre o la propiedad.  
  
   Se pueden especificar varias propiedades. Los nombres de propiedad deben ser únicos. La secuencia de las propiedades de ordenación en la cláusula ORDER BY define la organización del conjunto de resultados ordenado. Es decir, el conjunto de resultados se ordena en función de la primera propiedad, esa lista ordenada se ordena en función de la segunda propiedad y así sucesivamente.  
  
   Los nombres de propiedad que se hace referencia en la cláusula ORDER BY deben corresponder a una propiedad en la lista de selección o a una propiedad definida en la colección especificada en la cláusula FROM sin ambigüedades.  
  
- `<sort_expression>`  
  
   Especifica una o varias propiedades o expresiones en el que se va a ordenar el conjunto de resultados de consulta.  
  
- `<scalar_expression>`  
  
   Consulte la sección [Expresiones escalares](#bk_scalar_expressions) para más información.  
  
- `ASC | DESC`  
  
   Especifica que los valores de la columna especificada se deben ordenar en orden ascendente o descendente. ASC ordena los valores de menor a mayor. DESC ordena los valores de mayor a menos. ASC es el criterio de ordenación predeterminado. Los valores null se tratan como valores mínimos.  
  
  **Comentarios:**  
  
   La cláusula ORDER BY requiere que la directiva de indexación incluya un índice para los campos que se están ordenando. El tiempo de ejecución de consulta de Azure Cosmos DB admite la ordenación con un nombre de propiedad y no con las propiedades calculadas. Azure Cosmos DB admite varias propiedades de ORDER BY. Para ejecutar una consulta con varias propiedades para ORDER BY, debe definir un [índice compuesto](index-policy.md#composite-indexes) en los campos que se están ordenados.


##  <a name=bk_offsetlimit_clause></a> Cláusula de límite de desplazamiento

Especifica el número de elementos omitidos y número de elementos devueltos. Para obtener ejemplos, vea [ejemplos de cláusula de límite de desplazamiento](how-to-sql-query.md#OffsetLimitClause)
  
 **Sintaxis**  
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
 **Argumentos**  
 
- `<offset_amount>`

   Especifica el número total de elementos que se deben omitir los resultados de consulta.


- `<limit_amount>`
  
   Especifica el número total de elementos que deben incluir los resultados de consulta

  **Comentarios:**  
  
  El recuento de desplazamiento y el recuento de límite son necesarios en la cláusula de LIMIT de desplazamiento. Si un elemento opcional `ORDER BY` se usa la cláusula, se genera el conjunto de resultados realizando la omisión a través de los valores ordenados. En caso contrario, la consulta devolverá un orden fijo de valores.

##  <a name="bk_scalar_expressions"></a> Expresiones escalares  
 Una expresión escalar es una combinación de símbolos y operadores que se puede evaluar para obtener un solo valor. Las expresiones simples pueden ser constantes, referencias de propiedad, referencias de elementos de matriz, referencias de alias o llamadas de función. Las expresiones simples se pueden combinar en expresiones complejas con operadores. Para ver ejemplos, consulte [Ejemplos de expresiones escalares](how-to-sql-query.md#scalar-expressions)
  
 Para más información sobre los valores que puede tener una expresión escalar, consulte la sección [Constantes](#bk_constants).  
  
 **Sintaxis**  
  
```sql  
<scalar_expression> ::=  
       <constant>   
     | input_alias   
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>   
     | <create_array_expression>  
     | (<scalar_expression>)   
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```  
  
 **Argumentos**  
  
- `<constant>`  
  
   Representa un valor constante. Consulte la sección [Constantes](#bk_constants) para más información.  
  
- `input_alias`  
  
   Representa un valor definido por `input_alias`, introducido en la cláusula `FROM`.  
  Este valor nunca será **undefined**, los valores **undefined** se omiten de la entrada.  
  
- `<scalar_expression>.property_name`  
  
   Representa un valor de propiedad de un objeto. Si la propiedad no existe o se hace referencia a ella con un valor que no es un objeto, la expresión se evalúa como valor **undefined**.  
  
- `<scalar_expression>'['"property_name"|array_index']'`  
  
   Representa un valor de propiedad con nombre `property_name` o de elemento de matriz con el índice `array_index` de un objeto o matriz. Si la propiedad o el índice de matriz no existen o se hace referencia a ellos con un valor que no es un objeto o una matriz, la expresión se evalúa como valor undefined.  
  
- `unary_operator <scalar_expression>`  
  
   Representa un operador que se aplica a un único valor. Consulte la sección [Operadores](#bk_operators) para más información.  
  
- `<scalar_expression> binary_operator <scalar_expression>`  
  
   Representa un operador que se aplica a dos valores. Consulte la sección [Operadores](#bk_operators) para más información.  
  
- `<scalar_function_expression>`  
  
   Representa un valor definido por el resultado de una llamada de función.  
  
- `udf_scalar_function`  
  
   Nombre de la función escalar definida por el usuario.  
  
- `builtin_scalar_function`  
  
   Nombre de la función escalar incorporada.  
  
- `<create_object_expression>`  
  
   Representa un valor obtenido al crear un objeto con propiedades especificadas y sus valores.  
  
- `<create_array_expression>`  
  
   Representa un valor obtenido al crear una matriz con valores especificados como elementos.  
  
- `parameter_name`  
  
   Representa un valor del nombre de parámetro especificado. Los nombres de parámetro deben tener un único \@ como primer carácter.  
  
  **Comentarios:**  
  
  Todos los argumentos deben estar definidos al llamar a una función escalar incorporada o definida por el usuario. Si alguno de los argumentos no está definido, no se llamará a la función y el resultado será undefined.  
  
  Al crear un objeto, se omitirá cualquier propiedad a la que se le haya asignado un valor undefined y no se incluirá en el objeto creado.  
  
  Al crear una matriz, se omitirá cualquier elemento al que se le haya asignado un valor **undefined** y no se incluirá en el objeto creado. Esto hará que el siguiente elemento definido ocupe su lugar de manera que la matriz creada no tenga índices omitidos.  
  
##  <a name="bk_operators"></a> Operadores  
 En esta sección se describen los operadores compatibles. Cada operador puede asignarse exactamente a una categoría.  
  
 Consulte la siguiente tabla, **Categorías de operadores**, para más información acerca del control de los valores **undefined**, los requisitos de tipo de valores de entrada y el control de los valores con tipos no coincidentes.  
  
 **Categorías de operadores:**  
  
|**Categoría**|**Detalles**|  
|-|-|  
|**aritméticos**|El operador espera que las entradas sean números. La salida también es un número. Si alguna de las entradas es **undefined** o de un tipo que no sea un número, el resultado es **undefined**.|  
|**bit a bit**|El operador espera que las entradas sean números enteros con signo de 32 bits. La salida también es un número entero con signo de 32 bits.<br /><br /> Los valores que no sean enteros se redondean. Los valores positivos se redondean a la baja y los negativos, a la alta.<br /><br /> Se convertirá cualquier valor que esté fuera del intervalo de enteros de 32 bits, aprovechando los últimos 32 bits de su notación de complemento a dos.<br /><br /> Si alguna de las entradas es **undefined** o de un tipo que no sea un número, el resultado es **undefined**.<br /><br /> **Nota:** El comportamiento anterior es compatible con el comportamiento del operador bit a bit de JavaScript.|  
|**lógicos**|El operador espera que las entradas sean booleanos. La salida también es un booleano.<br />Si alguna de las entradas es **undefined** o de un tipo que no sea un booleano, el resultado es **undefined**.|  
|**de comparación**|El operador espera que las entradas sean del mismo tipo y sin valores undefined. La salida es un booleano.<br /><br /> Si alguna de las entradas es **undefined** o tienen distintos tipos, el resultado es **undefined**.<br /><br /> Consulte la tabla **Ordenación de los valores para la comparación** para conocer los detalles de ordenación de los valores.|  
|**cadena**|El operador espera que las entradas sean cadenas. La salida también es una cadena.<br />Si alguna de las entradas es **undefined** o de un tipo que no sea una cadena, el resultado es **undefined**.|  
  
 **Operadores unarios:**  
  
|**Nombre**|**Operador**|**Detalles**|  
|-|-|-|  
|**aritméticos**|+<br /><br /> -|Devuelve el valor numérico.<br /><br /> Negativos bit a bit. Devuelve el valor numérico negativo.|  
|**bit a bit**|~|Complemento de uno. Devuelve un complemento de un valor numérico.|  
|**lógicos**|**NOT**|Negación. Devuelve el valor booleano negativo.|  
  
 **Operadores binarios:**  
  
|**Nombre**|**Operador**|**Detalles**|  
|-|-|-|  
|**aritméticos**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Suma.<br /><br /> Resta.<br /><br /> Multiplicación.<br /><br /> División.<br /><br /> Modulación.|  
|**bit a bit**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|OR bit a bit.<br /><br /> AND bit a bit.<br /><br /> XOR bit a bit.<br /><br /> Desplazamiento a la izquierda.<br /><br /> Desplazamiento a la derecha.<br /><br /> Desplazamiento a la derecha con relleno de ceros.|  
|**lógicos**|**AND**<br /><br /> **O bien**|Conjunción lógica. Devuelve **true** si ambos argumentos son **true**, devuelve **false** en caso contrario.<br /><br /> Disyunción lógica. Devuelve **true** si algún argumento es **true**, devuelve **false** en caso contrario.|  
|**de comparación**|**=**<br /><br /> **!=, &lt;&gt;**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Igual a. Devuelve **true** si ambos argumentos son iguales, en caso contrario, devuelve **false**.<br /><br /> Diferente de. Devuelve **true** si ambos argumentos no son iguales, en caso contrario, devuelve **false**.<br /><br /> Mayor que. Devuelve **true** si el primer argumento es mayor que el segundo, en caso contrario, devuelve **false**.<br /><br /> Mayor o igual que. Devuelve **true** si el primer argumento es mayor o igual que el segundo, en caso contrario, devuelve **false**.<br /><br /> Menor que. Devuelve **true** si el primer argumento es menor que el segundo, en caso contrario, devuelve **false**.<br /><br /> Menor o igual que. Devuelve **true** si el primer argumento es menor o igual que el segundo, en caso contrario, devuelve **false**.<br /><br /> Fusionar. Devuelve el segundo argumento si el primero es un valor **undefined**.|  
|**String**|**&#124;&#124;**|Concatenación. Devuelve una concatenación de ambos argumentos.|  
  
 **Operadores ternarios:**  

|**Nombre**|**Operador**|**Detalles**| 
|-|-|-|  
|Operador ternario|?|Devuelve el segundo argumento si el primero se evalúa como **true**, en caso contrario, devuelve el tercer argumento.|  

  
 **Ordenación de los valores para la comparación**  
  
|**Tipo**|**Orden de los valores**|  
|-|-|  
|**Undefined**|No comparables.|  
|**Null**|Valor único: **null**|  
|**Number**|Número real natural.<br /><br /> El valor de infinito negativo es menor que cualquier otro valor numérico.<br /><br /> El valor de infinito positivo es mayor que cualquier otro valor numérico. El valor **NaN** no es comparable. La comparación con **NaN** dará como resultado el valor **undefined**.|  
|**String**|Orden lexicográfico.|  
|**Array**|Ningún orden, pero equitativo.|  
|**Object**|Ningún orden, pero equitativo.|  
  
 **Comentarios:**  
  
 En Cosmos DB, a menudo no se conocen los tipos de los valores hasta que se recuperan de la base de datos. Para admitir la ejecución de consultas de forma eficaz, la mayoría de los operadores tienen requisitos de tipo estrictos. Además, los operadores por sí mismos no realizan conversiones implícitas.  
  
 Esto significa que una consulta como: SELECT * FROM ROOT r WHERE r.Age = 21 solo devolverá documentos con la propiedad Age igual al número 21. Los documentos con la propiedad Age igual a la cadena "21" o a la cadena "0021" no coincidirán, ya que la expresión "21" = 21 se cuenta como undefined. Esto permite un mejor uso de los índices, ya que la búsqueda de un valor específico (como el número 21) es más rápida que la búsqueda de un número indefinido de coincidencias posibles (el número 21 o las cadenas "21", "021", "21,0", etc.). Esto difiere de la evaluación de los distintos tipos de valores de los operadores en JavaScript.  
  
 **Comparación e igualdad de objetos y matrices**  
  
 La comparación de valores de Array u Object mediante los operadores de intervalo (>, > =, <, < =) dará como resultado undefined, ya que no hay orden definido en los valores de Object o Array. Sin embargo, se admite con operadores de igualdad y desigualdad (=,! =, <>) y los valores se compararán estructuralmente.  
  
 Las matrices son iguales si ambas tienen el mismo número de elementos y los elementos de las posiciones coincidentes también son iguales. Si la comparación de cualquier par de resultados de elementos es undefined, el resultado de la comparación de matrices es undefined.  
  
 Los objetos son iguales si tienen las mismas propiedades definidas y si los valores de las propiedades coincidentes también son iguales. Si la comparación de cualquier par de valores de propiedad es undefined, el resultado de la comparación de objetos es undefined.  
  
##  <a name="bk_constants"></a> Constantes  
 Una constante, también conocida como valor literal o escalar, es un símbolo que representa un valor de datos específicos. El formato de una constante depende del tipo de datos del valor que representa.  
  
 **Tipos de datos escalares admitidos:**  
  
|**Tipo**|**Orden de los valores**|  
|-|-|  
|**Undefined**|Valor único: **undefined**|  
|**Null**|Valor único: **null**|  
|**Boolean**|Valores: **false**, **true**.|  
|**Number**|Número de punto flotante de precisión doble, estándar IEEE 754.|  
|**String**|Secuencia de cero o más caracteres Unicode. Las cadenas deben ir entre comillas sencillas o dobles.|  
|**Array**|Secuencia de cero o más elementos. Cada elemento puede ser un valor de cualquier tipo de datos escalar, excepto Undefined.|  
|**Object**|Conjunto desordenado de cero o más pares nombre/valor. Nombre es una cadena Unicode, el valor puede ser de cualquier tipo de datos escalar, excepto **Undefined**.|  
  
 **Sintaxis**  
  
```sql  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
 **Argumentos**  
  
* `<undefined_constant>; undefined`  
  
  Representa un valor sin definir de tipo Undefined.  
  
* `<null_constant>; null`  
  
  Representa el valor **null** de tipo **Null**.  
  
* `<boolean_constant>`  
  
  Representa la constante de tipo booleano.  
  
* `false`  
  
  Representa el valor **false** de tipo booleano.  
  
* `true`  
  
  Representa el valor **true** de tipo booleano.  
  
* `<number_constant>`  
  
  Representa una constante.  
  
* `decimal_literal`  
  
  Los literales decimales son números representados mediante notación decimal o científica.  
  
* `hexadecimal_literal`  
  
  Los literales hexadecimales son números representados mediante el prefijo "0x-" seguido de uno o más dígitos hexadecimales.  
  
* `<string_constant>`  
  
  Representa una constante de tipo cadena.  
  
* `string _literal`  
  
  Los literales de cadena son cadenas Unicode representadas por una secuencia de cero o varios caracteres Unicode o secuencias de escape. Los literales de cadena se encierran entre comillas simples (apóstrofo: ') o comillas dobles (comillas: ").  
  
  Se permiten las secuencias de escape siguientes:  
  
|**Secuencia de escape**|**Descripción**|**Carácter Unicode**|  
|-|-|-|  
|\\'|apóstrofo (')|U + 0027|  
|\\"|comillas dobles (")|U + 0022|  
|\\\ |barra inclinada inversa (\\)|U + 005C|  
|\\/|barra inclinada (/)|U + 002F|  
|\b|retroceso|U + 0008|  
|\f|avance de página|U + 000C|  
|\n|avance de línea|U + 000A|  
|\r|retorno de carro|U + 000D|  
|\t|tabulador|U + 0009|  
|\uXXXX|Carácter Unicode definidos por 4 dígitos hexadecimales.|U + XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a> Guías de rendimiento de las consultas  
 Para que una consulta se ejecute de forma eficaz con un contenedor extenso, debe utilizar filtros que puedan obtenerse a través de uno o más índices.  
  
 Para la búsqueda de índices, se considerarán los filtros siguientes:  
  
- Utilice el operador de igualdad (=) con expresiones de ruta de acceso de documentos y constantes.  
  
- Utilice los operadores de intervalo (<, \<=, >, >=) con expresiones de ruta de acceso de documentos y constantes numéricas.  
  
- Una expresión de ruta de acceso de documento es cualquier expresión que identifica una ruta de acceso constante en los documentos del contenedor de la base de datos a la que se hace referencia.  
  
  **Expresión de ruta de acceso de documento**  
  
  Las expresiones de ruta de acceso de documento son expresiones que evalúan la ruta de una propiedad o un indexador de matrices en un documento procedente del contenedor de bases de datos. Esta ruta de acceso sirve para identificar directamente la ubicación de los valores a los que se hace referencia en un filtro en los documentos del contenedor de la base de datos.  
  
  Para que una expresión se considere una expresión de ruta de acceso de documento, debe:  
  
1.  Hacer referencia directamente a la raíz del contenedor.  
  
2.  Hacer referencia al indexador de matrices de propiedad o constantes de alguna expresión de ruta de acceso de documento.  
  
3.  Hacer referencia a un alias, que represente alguna expresión de ruta de acceso de documento.  
  
     **Convenciones de sintaxis**  
  
     En la tabla siguiente se indican las convenciones utilizadas para describir la sintaxis en la siguiente referencia de SQL.  
  
    |**Convención**|**Se usa para**|  
    |-|-|    
    |MAYÚSCULAS|Palabras clave sin distinción entre mayúsculas y minúsculas.|  
    |minúsculas|Palabras clave con distinción entre mayúsculas y minúsculas.|  
    |\<noterminal&gt;|Elemento no terminal, se define por separado.|  
    |\<noterminal&gt; ::=|Definición de la sintaxis del elemento no terminal.|  
    |otro_terminal|Terminal (token), se describe detalladamente en palabras.|  
    |identificador|Identificador. Solo admite los caracteres siguientes:, a-z, A-Z, 0-9 _El primer carácter no puede ser un dígito.|  
    |"cadena"|Cadena entrecomillada. Permite cualquier cadena válida. Vea la descripción de string_literal.|  
    |'símbolo'|Símbolo literal que forma parte de la sintaxis.|  
    |&#124; (barra vertical)|Alternativas para los elementos de la sintaxis. Puede usar solo uno de los elementos especificados.|  
    |[] /(corchetes)|Los corchetes contienen uno o varios elementos opcionales.|  
    |[, ...n]|Indica que el elemento anterior se puede repetir un número n de veces. Los elementos se separan por comas.|  
    |[ ...n]|Indica que el elemento anterior se puede repetir un número n de veces. Los elementos se separan por espacios en blanco.|  
  
##  <a name="bk_built_in_functions"></a> Funciones integradas  
 Cosmos DB proporciona muchas funciones de SQL integradas. Las categorías de las funciones integradas aparecen a continuación.  
  
|Función|DESCRIPCIÓN|  
|--------------|-----------------|  
|[Funciones matemáticas](#bk_mathematical_functions)|Las funciones matemáticas realizan un cálculo, basado normalmente en valores de entrada proporcionados como argumentos, y devuelven un valor numérico.|  
|[Funciones de comprobación de tipos](#bk_type_checking_functions)|Las funciones de comprobación de tipos permiten comprobar el tipo de una expresión dentro de consultas SQL.|  
|[Funciones de cadena](#bk_string_functions)|Las siguientes funciones de cadena realizan una operación sobre un valor de entrada de cadena y devuelven una cadena, un valor numérico o un booleano.|  
|[Funciones de matriz](#bk_array_functions)|Las siguientes funciones de matriz realizan una operación en un valor de entrada de matriz y devuelven un valor numérico, booleano o de matriz.|
|[Funciones de fecha y hora](#bk_date_and_time_functions)|Las funciones de fecha y hora le permiten obtener la fecha UTC actual y la hora de dos formas; una marca de tiempo numérica cuyo valor es el tiempo de Unix en milisegundos, o como una cadena que se ajusta al formato ISO 8601.|
|[Funciones espaciales](#bk_spatial_functions)|Las siguientes funciones espaciales realizan una operación en un valor de entrada de objeto espacial y devuelven un valor numérico o booleano.|  
  
###  <a name="bk_mathematical_functions"></a> Funciones matemáticas  
 Las siguientes funciones realizan un cálculo, basado normalmente en valores de entrada que se proporcionan como argumentos, y devuelven un valor numérico.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[CEILING](#bk_ceiling)|  
|[COS](#bk_cos)|[COT](#bk_cot)|[DEGREES](#bk_degrees)|  
|[EXP](#bk_exp)|[FLOOR](#bk_floor)|[LOG](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[POWER](#bk_power)|  
|[RADIANS](#bk_radians)|[ROUND](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[SQUARE](#bk_square)|[SIGN](#bk_sign)|  
|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  
  
####  <a name="bk_abs"></a> ABS  
 Devuelve el valor absoluto (positivo) de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestran los resultados de usar la función ABS en tres números distintos.  
  
```  
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{abs1: 1, abs2: 0, abs3: 1}]  
```  
  
####  <a name="bk_acos"></a> ACOS  
 Devuelve el ángulo, en radianes, cuyo coseno es la expresión numérica especificada; también se denomina arcocoseno.  
  
 **Sintaxis**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve el arcocoseno de -1:  
  
```  
SELECT ACOS(-1) AS acos 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"acos": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a> ASIN  
 Devuelve el ángulo, en radianes, cuyo seno es la expresión numérica especificada. También se denomina arcoseno.  
  
 **Sintaxis**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve el arcoseno de -1:  
  
```  
SELECT ASIN(-1) AS asin  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"asin": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a> ATAN  
 Devuelve el ángulo, en radianes, cuya tangente es la expresión numérica especificada. También se denomina arcotangente.  
  
 **Sintaxis**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve la arcotangente del valor especificado.  
  
```  
SELECT ATAN(-45.01) AS atan  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"atan": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a> ATN2  
 Devuelve el valor principal de la arcotangente de y/x, expresado en radianes.  
  
 **Sintaxis**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se calcula ATN2 para los componentes x e y especificados.  
  
```  
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"atn2": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a> CEILING  
 Devuelve el valor entero más pequeño mayor o igual que la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestran valores numéricos positivos, negativos y cero con la función CEILING.  
  
```  
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{c1: 124, c2: -123, c3: 0}]  
```  
  
####  <a name="bk_cos"></a> COS  
 Devuelve el coseno trigonométrico del ángulo especificado, en radianes, en la expresión especificada.  
  
 **Sintaxis**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se calcula el coseno del ángulo especificado.  
  
```  
SELECT COS(14.78) AS cos  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"cos": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a> COT  
 Devuelve la cotangente trigonométrica del ángulo especificado, en radianes, en la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se calcula la cotangente del ángulo especificado.  
  
```  
SELECT COT(124.1332) AS cot  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"cot": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a> DEGREES  
 Devuelve el ángulo correspondiente en grados de un ángulo especificado en radianes.  
  
 **Sintaxis**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelven los grados de un ángulo de pi/2 radianes.  
  
```  
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"degrees": 90}]  
```  
  
####  <a name="bk_floor"></a> FLOOR  
 Devuelve el valor entero más grande menor o igual que la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestran valores numéricos positivos, negativos y cero con la función FLOOP.  
  
```  
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{fl1: 123, fl2: -124, fl3: 0}]  
```  
  
####  <a name="bk_exp"></a> EXP  
 Devuelve el valor exponencial de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Comentarios:**  
  
  La constante **e** (2,718281 …) es la base de los logaritmos naturales.  
  
  El exponente de un número es la constante **e** elevada a la potencia del número. Por ejemplo EXP(1,0) = e^1,0 = 2,71828182845905 y EXP(10) = e^10 = 22026,4657948067.  
  
  El valor exponencial del logaritmo natural de un número es el mismo número: EXP (LOG [n]) = n. Y el logaritmo natural del valor exponencial de un número es el mismo número: LOG (EXP (n)) = n.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor exponencial de la variable especificada (10).  
  
```  
SELECT EXP(10) AS exp  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{exp: 22026.465794806718}]  
```  
  
 En el ejemplo siguiente se devuelve el valor exponencial del logaritmo natural de 20 y el logaritmo natural del valor exponencial de 20. Dado que estas funciones son inversas entre sí, el valor devuelto con redondeo para las operaciones matemáticas de punto flotante en ambos casos es 20.  
  
```  
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{exp1: 19.999999999999996, exp2: 20}]  
```  
  
####  <a name="bk_log"></a> LOG  
 Devuelve el algoritmo natural de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
- `base`  
  
   Argumento numérico opcional que establece la base del logaritmo.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Comentarios**  
  
  De forma predeterminada, LOG() devuelve el logaritmo natural. Puede cambiar la base del logaritmo a otro valor mediante el parámetro base opcional.  
  
  El logaritmo natural es el logaritmo en base **e**, donde **e** es una constante irracional aproximadamente igual a 2,718 281 828.  
  
  El logaritmo natural del valor exponencial de un número es el mismo número: LOG( EXP( n ) ) = n. Y el valor exponencial del logaritmo natural de un número es el mismo número: EXP( LOG( n ) ) = n.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor logarítmico de la variable especificada (10).  
  
```  
SELECT LOG(10) AS log  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{log: 2.3025850929940459}]  
```  
  
 En el ejemplo siguiente se calcula el logaritmo del exponente de un número.  
  
```  
SELECT EXP(LOG(10)) AS expLog  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{expLog: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a> LOG10  
 Devuelve el logaritmo de base 10 de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Comentarios:**  
  
  Las funciones LOG10 y POWER tienen una relación inversa. Por ejemplo, 10 ^ LOG10 (n) = n.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor de LOG10 de la variable especificada (100).  
  
```  
SELECT LOG10(100) AS log10 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{log10: 2}]  
```  
  
####  <a name="bk_pi"></a> PI  
 Devuelve el valor constante de PI.  
  
 **Sintaxis**  
  
```  
PI ()  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve el valor de pi.  
  
```  
SELECT PI() AS pi 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"pi": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a> POWER  
 Devuelve el valor de la expresión especificada a la potencia especificada.  
  
 **Sintaxis**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
- `y`  
  
   Es la potencia a la que se eleva `numeric_expression`.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo elevar un número a la potencia de 3 (el cubo del número).  
  
```  
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{pow1: 8, pow2: 15.625}]  
```  
  
####  <a name="bk_radians"></a> RADIANS  
 Devuelve radianes cuando se especifica una expresión numérica en grados.  
  
 **Sintaxis**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se toman unos ángulos como entrada y se devuelven sus correspondientes valores en radianes.  
  
```  
SELECT RADIANS(-45.01) AS r1, RADIANS(-181.01) AS r2, RADIANS(0) AS r3, RADIANS(0.1472738) AS r4, RADIANS(197.1099392) AS r5  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{  
       "r1": -0.7855726963226477,  
       "r2": -3.1592204790349356,  
       "r3": 0,  
       "r4": 0.0025704127119236249,  
       "r5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a> ROUND  
 Devuelve un valor numérico, redondeado al valor entero más cercano.  
  
 **Sintaxis**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se redondean los siguientes números positivos y negativos al entero más próximo.  
  
```  
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  
  
####  <a name="bk_sign"></a> SIGN  
 Devuelve el signo positivo (+1), cero (0) o negativo (-1) de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelven los valores del signo de números entre -2 y 2.  
  
```  
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  
  
####  <a name="bk_sin"></a> SIN  
 Devuelve el seno trigonométrico del ángulo especificado, en radianes, en la expresión especificada.  
  
 **Sintaxis**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se calcula el seno del ángulo especificado.  
  
```  
SELECT SIN(45.175643) AS sin  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"sin": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a> SQRT  
 Devuelve la raíz cuadrada del valor numérico especificado.  
  
 **Sintaxis**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelven las raíces cuadradas de los números 1-3.  
  
```  
SELECT SQRT(1) AS s1, SQRT(2.0) AS s2, SQRT(3) AS s3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{s1: 1, s2: 1.4142135623730952, s3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a> SQUARE  
 Devuelve el cuadrado del valor numérico especificado.  
  
 **Sintaxis**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelven los cuadrados de los números 1-3.  
  
```  
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{s1: 1, s2: 4, s3: 9}]  
```  
  
####  <a name="bk_tan"></a> TAN  
 Devuelve la tangente del ángulo especificado, en radianes, en la expresión especificada.  
  
 **Sintaxis**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se calcula la tangente de pi()/2.  
  
```  
SELECT TAN(PI()/2) AS tan 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"tan": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a> TRUNC  
 Devuelve un valor numérico, truncado al valor entero más cercano.  
  
 **Sintaxis**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se truncan los siguientes números positivos y negativos al valor entero más próximo.  
  
```  
SELECT TRUNC(2.4) AS t1, TRUNC(2.6) AS t2, TRUNC(2.5) AS t3, TRUNC(-2.4) AS t4, TRUNC(-2.6) AS t5  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{t1: 2, t2: 2, t3: 2, t4: -2, t5: -2}]  
```  
  
###  <a name="bk_type_checking_functions"></a> Funciones de comprobación de tipos  
 Las siguientes funciones admiten la comprobación de tipos de valores de entrada y devuelven un valor booleano.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a> IS_ARRAY  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es una matriz.  
  
 **Sintaxis**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_ARRAY.  
  
```  
SELECT   
 IS_ARRAY(true) AS isArray1,   
 IS_ARRAY(1) AS isArray2,  
 IS_ARRAY("value") AS isArray3,  
 IS_ARRAY(null) AS isArray4,  
 IS_ARRAY({prop: "value"}) AS isArray5,   
 IS_ARRAY([1, 2, 3]) AS isArray6,  
 IS_ARRAY({prop: "value"}.prop2) AS isArray7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isArray1":false,"isArray2":false,"isArray3":false,"isArray4":false,"isArray5":false,"isArray6":true,"isArray7":false}]
```  
  
####  <a name="bk_is_bool"></a> IS_BOOL  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es un valor booleano.  
  
 **Sintaxis**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_BOOL.  
  
```  
SELECT   
    IS_BOOL(true) AS isBool1,   
    IS_BOOL(1) AS isBool2,  
    IS_BOOL("value") AS isBool3,   
    IS_BOOL(null) AS isBool4,  
    IS_BOOL({prop: "value"}) AS isBool5,   
    IS_BOOL([1, 2, 3]) AS isBool6,  
    IS_BOOL({prop: "value"}.prop2) AS isBool7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isBool1":true,"isBool2":false,"isBool3":false,"isBool4":false,"isBool5":false,"isBool6":false,"isBool7":false}]
```  
  
####  <a name="bk_is_defined"></a> IS_DEFINED  
 Devuelve un valor booleano que indica si se ha asignado un valor a la propiedad.  
  
 **Sintaxis**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueba la presencia de una propiedad en el documento JSON especificado. En el primer caso se devuelve true, ya que "a" está presente, pero en el segundo, false, ya que "b" no lo está.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isDefined1":true,"isDefined2":false}]  
```  
  
####  <a name="bk_is_null"></a> IS_NULL  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es nulo.  
  
 **Sintaxis**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_NULL.  
  
```  
SELECT   
    IS_NULL(true) AS isNull1,   
    IS_NULL(1) AS isNull2,  
    IS_NULL("value") AS isNull3,   
    IS_NULL(null) AS isNull4,  
    IS_NULL({prop: "value"}) AS isNull5,   
    IS_NULL([1, 2, 3]) AS isNull6,  
    IS_NULL({prop: "value"}.prop2) AS isNull7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isNull1":false,"isNull2":false,"isNull3":false,"isNull4":true,"isNull5":false,"isNull6":false,"isNull7":false}]
```  
  
####  <a name="bk_is_number"></a> IS_NUMBER  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es un número.  
  
 **Sintaxis**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_NULL.  
  
```  
SELECT   
    IS_NUMBER(true) AS isNum1,   
    IS_NUMBER(1) AS isNum2,  
    IS_NUMBER("value") AS isNum3,   
    IS_NUMBER(null) AS isNum4,  
    IS_NUMBER({prop: "value"}) AS isNum5,   
    IS_NUMBER([1, 2, 3]) AS isNum6,  
    IS_NUMBER({prop: "value"}.prop2) AS isNum7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isNum1":false,"isNum2":true,"isNum3":false,"isNum4":false,"isNum5":false,"isNum6":false,"isNum7":false}]  
```  
  
####  <a name="bk_is_object"></a> IS_OBJECT  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es un objeto JSON.  
  
 **Sintaxis**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_OBJECT.  
  
```  
SELECT   
    IS_OBJECT(true) AS isObj1,   
    IS_OBJECT(1) AS isObj2,  
    IS_OBJECT("value") AS isObj3,   
    IS_OBJECT(null) AS isObj4,  
    IS_OBJECT({prop: "value"}) AS isObj5,   
    IS_OBJECT([1, 2, 3]) AS isObj6,  
    IS_OBJECT({prop: "value"}.prop2) AS isObj7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isObj1":false,"isObj2":false,"isObj3":false,"isObj4":false,"isObj5":true,"isObj6":false,"isObj7":false}]
```  
  
####  <a name="bk_is_primitive"></a> IS_PRIMITIVE  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es primitivo (cadena, booleano, numérico o NULL).  
  
 **Sintaxis**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban los objetos de los tipos JSON Boolean, number, string, null, object, array y undefined, mediante la función IS_PRIMITIVE.  
  
```  
SELECT   
           IS_PRIMITIVE(true) AS isPrim1,   
           IS_PRIMITIVE(1) AS isPrim2,  
           IS_PRIMITIVE("value") AS isPrim3,   
           IS_PRIMITIVE(null) AS isPrim4,  
           IS_PRIMITIVE({prop: "value"}) AS isPrim5,   
           IS_PRIMITIVE([1, 2, 3]) AS isPrim6,  
           IS_PRIMITIVE({prop: "value"}.prop2) AS isPrim7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isPrim1": true, "isPrim2": true, "isPrim3": true, "isPrim4": true, "isPrim5": false, "isPrim6": false, "isPrim7": false}]  
```  
  
####  <a name="bk_is_string"></a> IS_STRING  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es una cadena.  
  
 **Sintaxis**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_STRING.  
  
```  
SELECT   
       IS_STRING(true) AS isStr1,   
       IS_STRING(1) AS isStr2,  
       IS_STRING("value") AS isStr3,   
       IS_STRING(null) AS isStr4,  
       IS_STRING({prop: "value"}) AS isStr5,   
       IS_STRING([1, 2, 3]) AS isStr6,  
       IS_STRING({prop: "value"}.prop2) AS isStr7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isStr1":false,"isStr2":false,"isStr3":true,"isStr4":false,"isStr5":false,"isStr6":false,"isStr7":false}] 
```  
  
###  <a name="bk_string_functions"></a> Funciones de cadena  
 Las siguientes funciones escalares realizan una operación sobre un valor de entrada de cadena y devuelven una cadena, un valor numérico o un booleano.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[CONTAINS](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[LEFT](#bk_left)|[LENGTH](#bk_length)|  
|[LOWER](#bk_lower)|[LTRIM](#bk_ltrim)|[REPLACE](#bk_replace)|  
|[REPLICATE](#bk_replicate)|[REVERSE](#bk_reverse)|[RIGHT](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[StringToArray](#bk_stringtoarray)|
|[StringToBoolean](#bk_stringtoboolean)|[StringToNull](#bk_stringtonull)|[StringToNumber](#bk_stringtonumber)|
|[StringToObject](#bk_stringtoobject)|[SUBSTRING](#bk_substring)|[ToString](#bk_tostring)|
|[TRIM](#bk_trim)|[UPPER](#bk_upper)||
  
####  <a name="bk_concat"></a> CONCAT  
 Devuelve una cadena que es el resultado de concatenar dos o más valores de cadena.  
  
 **Sintaxis**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve la cadena concatenada de los valores especificados.  
  
```  
SELECT CONCAT("abc", "def") AS concat  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"concat": "abcdef"}  
```  
  
####  <a name="bk_contains"></a> CONTAINS  
 Devuelve un valor booleano que indica si la primera expresión de cadena contiene la segunda.  
  
 **Sintaxis**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueba si "abc" contiene "ab" y contiene "d".  
  
```  
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"c1": true, "c2": false}]  
```  
  
####  <a name="bk_endswith"></a> ENDSWITH  
 Devuelve un valor booleano que indica si la primera expresión de cadena finaliza con la segunda.  
  
 **Sintaxis**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve las terminaciones "b" y "bc"de "abc".  
  
```  
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"e1": false, "e2": true}]  
```  
  
####  <a name="bk_index_of"></a> INDEX_OF  
 Devuelve la posición inicial de la primera aparición de la expresión de la segunda cadena dentro de la primera expresión de cadena especificada, o -1 si no se encuentra la cadena.  
  
 **Sintaxis**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve el índice de varias subcadenas dentro de "abc".  
  
```  
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"i1": 0, "i2": 1, "i3": -1}]  
```  
  
####  <a name="bk_left"></a> LEFT  
 Devuelve la parte izquierda de una cadena con el número especificado de caracteres.  
  
 **Sintaxis**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
- `num_expr`  
  
   Es cualquier expresión numérica válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve la parte izquierda de "abc" para distintos valores de longitud.  
  
```  
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"l1": "a", "l2": "ab"}]  
```  
  
####  <a name="bk_length"></a> LENGTH  
 Devuelve el número de caracteres de la expresión de cadena especificada.  
  
 **Sintaxis**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve la longitud de una cadena.  
  
```  
SELECT LENGTH("abc") AS len 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_lower"></a> LOWER  
 Devuelve una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.  
  
 **Sintaxis**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar LOWER en una consulta.  
  
```  
SELECT LOWER("Abc") AS lower
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"lower": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a> LTRIM  
 Devuelve una expresión de cadena después de quitar los espacios en blanco iniciales.  
  
 **Sintaxis**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar LTRIM en una consulta.  
  
```  
SELECT LTRIM("  abc") AS l1, LTRIM("abc") AS l2, LTRIM("abc   ") AS l3 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"l1": "abc", "l2": "abc", "l3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a> REPLACE  
 Reemplaza todas las apariciones de un valor de cadena especificado por otro valor de cadena.  
  
 **Sintaxis**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar REPLACE en una consulta.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"replace": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a> REPLICATE  
 Repite un valor de cadena un número especificado de veces.  
  
 **Sintaxis**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
- `num_expr`  
  
   Es cualquier expresión numérica válida. Si el valor num_expr es negativo o no finito, el resultado es indefinido.

  > [!NOTE]
  > La longitud máxima del resultado es de 10 000 caracteres, es decir, (length(str_expr) * num_expr) <= 10 000.
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar REPLICATE en una consulta.  
  
```  
SELECT REPLICATE("a", 3) AS replicate  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"replicate": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a> REVERSE  
 Devuelve el orden inverso de un valor de cadena.  
  
 **Sintaxis**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar REVERSE en una consulta.  
  
```  
SELECT REVERSE("Abc") AS reverse  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"reverse": "cbA"}]  
```  
  
####  <a name="bk_right"></a> RIGHT  
 Devuelve la parte derecha de una cadena con el número especificado de caracteres.  
  
 **Sintaxis**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
- `num_expr`  
  
   Es cualquier expresión numérica válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve la parte derecha de "abc" para distintos valores de longitud.  
  
```  
SELECT RIGHT("abc", 1) AS r1, RIGHT("abc", 2) AS r2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"r1": "c", "r2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a> RTRIM  
 Devuelve una expresión de cadena después de quitar los espacios en blanco finales.  
  
 **Sintaxis**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar RTRIM en una consulta.  
  
```  
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a> STARTSWITH  
 Devuelve un valor booleano que indica si la primera expresión de cadena empieza con la segunda.  
  
 **Sintaxis**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueba si la cadena "abc" empieza por "b" y "a".  
  
```  
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"s1": false, "s2": true}]  
```  

  ####  <a name="bk_stringtoarray"></a> StringToArray  
 Devuelve la expresión traducida a una matriz. Si no se puede traducir expresiones, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToArray(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que debe evaluarse como una expresión de matriz JSON. Tenga en cuenta que los valores de cadena anidada deben escribirse entre comillas dobles para ser válido. Para obtener más información sobre el formato JSON, consulte [json.org](https://json.org/)
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de matriz o sin definir.  
  
  **Ejemplos**  
  
  El ejemplo siguiente muestra cómo se comporta StringToArray con diferentes tipos. 
  
 Los siguientes son ejemplos con una entrada válida.

```
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

El conjunto de resultados es el siguiente:

```
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

El siguiente es un ejemplo de entrada no válida. 
   
 Las comillas simples dentro de la matriz no son válidos en JSON.
Aunque son válidos dentro de una consulta, no se analizará para matrices válidas. O bien deben convertirse a cadenas dentro de la cadena de la matriz "[\\"\\"]" o la oferta circundante debe ser única ' [""] ".

```
SELECT
    StringToArray("['5','6','7']")
```

El conjunto de resultados es el siguiente:

```
[{}]
```

Los siguientes son ejemplos de entrada no válida.
   
 La expresión pasada se analizará como una matriz JSON. el siguiente no se evalúan para el tipo de matriz y, por tanto, devuelven indefinido.
   
```
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
```

El conjunto de resultados es el siguiente:

```
[{}]
```

####  <a name="bk_stringtoboolean"></a> StringToBoolean  
 Devuelve la expresión traducida en un valor booleano. Si no se puede traducir expresiones, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToBoolean(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que debe evaluarse como una expresión booleana.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión booleana o sin definir.  
  
  **Ejemplos**  
  
  El ejemplo siguiente muestra cómo se comporta StringToBoolean con diferentes tipos. 
 
 Los siguientes son ejemplos con una entrada válida.

Se admiten espacios en blanco únicamente antes o después de "true"o "false".

```  
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"b1": true, "b2": false, "b3": false}]
```  

Los siguientes son ejemplos de entrada no válida.

 Valores booleanos se distinguen mayúsculas de minúsculas y deben escribirse con todos los caracteres en minúsculas; es decir, "true" y "false".

```  
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

El conjunto de resultados es el siguiente:  
  
```  
[{}]
``` 

La expresión pasada se analizará como una expresión booleana; Estas entradas no se evalúan para el tipo booleano y, por tanto, devuelve undefined.

```  
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
```  

El conjunto de resultados es el siguiente:  
  
```  
[{}]
```  

####  <a name="bk_stringtonull"></a> StringToNull  
 Devuelve la expresión traducida en null. Si no se puede traducir expresiones, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToNull(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que debe evaluarse como una expresión null.
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión null o undefined.  
  
  **Ejemplos**  
  
  El ejemplo siguiente muestra cómo se comporta StringToNull con diferentes tipos. 

Los siguientes son ejemplos con una entrada válida.

 Se admiten espacios en blanco únicamente antes o después de "null".

```  
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"n1": null, "n2": null, "n3": true}]
```  

Los siguientes son ejemplos de entrada no válida.

NULL distingue mayúsculas de minúsculas y debe escribirse con todos los caracteres en minúsculas; es decir, "nulos".

```  
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{}]
```  

La expresión pasada se analizará como una expresión null; Estas entradas no se evalúan para el tipo es null y, por tanto, devuelven indefinido.

```  
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{}]
```  

####  <a name="bk_stringtonumber"></a> StringToNumber  
 Devuelve la expresión traducida en un número. Si no se puede traducir expresiones, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToNumber(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que debe evaluarse como una expresión del número de JSON. Números en JSON deben ser un entero o un punto flotante. Para obtener más información sobre el formato JSON, consulte [json.org](https://json.org/)  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica o no definido.  
  
  **Ejemplos**  
  
  El ejemplo siguiente muestra cómo se comporta StringToNumber con diferentes tipos. 

Se admiten espacios en blanco únicamente antes o después del número.

```  
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

Debe ser un número válido de JSON, ser un entero o flotante número de punto.

```  
SELECT   
    StringToNumber("0xF")
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
{{}}
```  

La expresión pasada se analizará como una expresión numérica; Estas entradas no se evalúan para escribir el número y, por tanto, devolver indefinido. 

```  
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
{{}}
```  

####  <a name="bk_stringtoobject"></a> StringToObject  
 Devuelve la expresión traducida a un objeto. Si no se puede traducir expresiones, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToObject(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que debe evaluarse como una expresión de objeto JSON. Tenga en cuenta que los valores de cadena anidada deben escribirse entre comillas dobles para ser válido. Para obtener más información sobre el formato JSON, consulte [json.org](https://json.org/)  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de objeto o no definido.  
  
  **Ejemplos**  
  
  El ejemplo siguiente muestra cómo se comporta StringToObject con diferentes tipos. 
  
 Los siguientes son ejemplos con una entrada válida.

``` 
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

El conjunto de resultados es el siguiente:

```
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 Los siguientes son ejemplos de entrada no válida.
Aunque son válidos dentro de una consulta, no se analizará para objetos válidos. O bien deben convertirse a cadenas dentro de la cadena del objeto "{\\" un\\":\\" str\\"}" o la oferta circundante debe ser única "{"a":"str"}'.

Las comillas simples que rodean a los nombres de propiedad no son válidos en JSON.

``` 
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

El conjunto de resultados es el siguiente:

```  
[{}]
```  

Los nombres de propiedad sin comillas circundantes no son JSON válido.

``` 
SELECT 
    StringToObject("{a:[1,2,3]}")
```

El conjunto de resultados es el siguiente:

```  
[{}]
``` 

Los siguientes son ejemplos de entrada no válida.

 La expresión pasada se analizará como un objeto JSON; Estas entradas no se evalúan para el tipo de objeto y, por tanto, devuelven indefinido.

``` 
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 
 
 El conjunto de resultados es el siguiente:

```
[{}]
```

####  <a name="bk_substring"></a> SUBSTRING  
 Devuelve parte de una expresión de cadena a partir de la posición de base cero del carácter especificado y continúa hasta la longitud especificada, o hasta el final de la cadena.  
  
 **Sintaxis**  
  
```  
SUBSTRING(<str_expr>, <num_expr>, <num_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
- `num_expr`  
  
   Cualquier expresión numérica válida para denotar el carácter inicial y final.    
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve la subcadena de "abc" que empieza por 1 y tiene una longitud de 1 carácter.  
  
```  
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"substring": "b"}]  
```  
####  <a name="bk_tostring"></a> ToString  
 Devuelve una representación de cadena de una expresión escalar. 
  
 **Sintaxis**  
  
```  
ToString(<expr>)
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo se comporta ToString en tipos diferentes.   
  
```  
SELECT 
    ToString(1.0000) AS str1, 
    ToString("Hello World") AS str2, 
    ToString(NaN) AS str3, 
    ToString(Infinity) AS str4,
    ToString(IS_STRING(ToString(undefined))) AS str5, 
    ToString(0.1234) AS str6, 
    ToString(false) AS str7, 
    ToString(undefined) AS str8
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"str1": "1", "str2": "Hello World", "str3": "NaN", "str4": "Infinity", "str5": "false", "str6": "0.1234", "str7": "false"}]  
```  
 Dada la entrada siguiente:
```  
{"Products":[{"ProductID":1,"Weight":4,"WeightUnits":"lb"},{"ProductID":2,"Weight":32,"WeightUnits":"kg"},{"ProductID":3,"Weight":400,"WeightUnits":"g"},{"ProductID":4,"Weight":8999,"WeightUnits":"mg"}]}
```    
 En el ejemplo siguiente se muestra cómo se puede usar ToString con otras funciones de cadena como CONCAT.   
 
```  
SELECT 
CONCAT(ToString(p.Weight), p.WeightUnits) 
FROM p in c.Products 
```  

El conjunto de resultados es el siguiente:  
  
```  
[{"$1":"4lb" },
{"$1":"32kg"},
{"$1":"400g" },
{"$1":"8999mg" }]

```  
Dada la entrada siguiente:
```
{"id":"08259","description":"Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX","nutrients":[{"id":"305","description":"Caffeine","units":"mg"},{"id":"306","description":"Cholesterol, HDL","nutritionValue":30,"units":"mg"},{"id":"307","description":"Sodium, NA","nutritionValue":612,"units":"mg"},{"id":"308","description":"Protein, ABP","nutritionValue":60,"units":"mg"},{"id":"309","description":"Zinc, ZN","nutritionValue":null,"units":"mg"}]}
```
En el ejemplo siguiente se muestra cómo se puede usar ToString con otras funciones de cadena como REPLACE.   
```
SELECT 
    n.id AS nutrientID,
    REPLACE(ToString(n.nutritionValue), "6", "9") AS nutritionVal
FROM food 
JOIN n IN food.nutrients
```
El conjunto de resultados es el siguiente:  
 ```
[{"nutrientID":"305"},
{"nutrientID":"306","nutritionVal":"30"},
{"nutrientID":"307","nutritionVal":"912"},
{"nutrientID":"308","nutritionVal":"90"},
{"nutrientID":"309","nutritionVal":"null"}]
``` 
 
####  <a name="bk_trim"></a> TRIM  
 Devuelve una expresión de cadena después de quitar los espacios en blanco iniciales y finales.  
  
 **Sintaxis**  
  
```  
TRIM(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar TRIM en una consulta.  
  
```  
SELECT TRIM("   abc") AS t1, TRIM("   abc   ") AS t2, TRIM("abc   ") AS t3, TRIM("abc") AS t4
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"t1": "abc", "t2": "abc", "t3": "abc", "t4": "abc"}]  
``` 
####  <a name="bk_upper"></a> UPPER  
 Devuelve una expresión de cadena después de convertir datos de caracteres en minúsculas a mayúsculas.  
  
 **Sintaxis**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar UPPER en una consulta.  
  
```  
SELECT UPPER("Abc") AS upper  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"upper": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a> Funciones de matriz  
 Las siguientes funciones escalares realizan una operación en un valor de entrada de matriz y devuelven un valor numérico, booleano o de matriz.  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a> ARRAY_CONCAT  
 Devuelve una matriz que es el resultado de concatenar dos o más valores de la matriz.  
  
 **Sintaxis**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argumentos**  
  
- `arr_expr`  
  
   Es cualquier expresión de matriz válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de matriz.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo concatenar dos matrices.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a> ARRAY_CONTAINS  
Devuelve un valor booleano que indica si la matriz contiene el valor especificado. Puede buscar una coincidencia parcial o total de un objeto mediante el uso de una expresión booleana dentro del comando. 

**Sintaxis**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Argumentos**  
  
- `arr_expr`  
  
   Es cualquier expresión de matriz válida.  
  
- `expr`  
  
   Es cualquier expresión válida.  

- `bool_expr`  
  
   Es cualquier expresión booleana. Si se establece en ' true' y el valor de búsqueda especificado es un objeto, el comando busca una coincidencia parcial (el objeto de búsqueda es un subconjunto de uno de los objetos). Si se establece en 'false', el comando busca una coincidencia total de todos los objetos dentro de la matriz. Si no se especifica, el valor predeterminado es 'false'. 
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor booleano.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo comprobar la pertenencia a una matriz con ARRAY_CONTAINS.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"b1": true, "b2": false}]  
```  

En el siguiente ejemplo se indica cómo buscar una coincidencia parcial de JSON en una matriz mediante ARRAY_CONTAINS.  
  
```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{
  "b1": true,
  "b2": false,
  "b3": false
}] 
```  
  
####  <a name="bk_array_length"></a> ARRAY_LENGTH  
 Devuelve el número de elementos de la expresión de matriz especificada.  
  
 **Sintaxis**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumentos**  
  
- `arr_expr`  
  
   Es cualquier expresión de matriz válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo obtener la longitud de una matriz con ARRAY_LENGTH.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_array_slice"></a> ARRAY_SLICE  
 Devuelve parte de una expresión de matriz.
  
 **Sintaxis**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumentos**  
  
- `arr_expr`  
  
   Es cualquier expresión de matriz válida.  
  
- `num_expr`  
  
   Índice numérico basado en cero en el que comienza la matriz. Se pueden usar valores negativos para especificar el índice inicial en relación con el último elemento de la matriz; por ejemplo, -1 hace referencia al último elemento de la matriz.  

- `num_expr`  

   Número máximo de elementos en la matriz resultante.    

  **Tipos de valor devuelto**  
  
  Devuelve una expresión de matriz.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo obtener distintos segmentos de una matriz con ARRAY_SLICE.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      
  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  

###  <a name="bk_date_and_time_functions"></a> Funciones de fecha y hora
 Las siguientes funciones escalares permiten obtener la fecha UTC actual y la hora de dos formas; una marca de tiempo numérica cuyo valor es el tiempo de Unix en milisegundos, o como una cadena que se ajusta al formato ISO 8601. 

|||
|-|-|
|[GetCurrentDateTime](#bk_get_current_date_time)|[GetCurrentTimestamp](#bk_get_current_timestamp)||

####  <a name="bk_get_current_date_time"></a> GetCurrentDateTime
 Devuelve la fecha UTC actual y la hora como una cadena ISO 8601.
  
 **Sintaxis**
  
```
GetCurrentDateTime ()
```
  
  **Tipos de valor devuelto**
  
  Devuelve el actual fecha y hora ISO 8601 cadena valor UTC. 

  Esto se expresa en el formato aaaa-MM-DDThh:mm:ss.sssZ donde:
  
  |||
  |-|-|
  |AAAA|año de cuatro dígitos|
  |MM|mes de dos dígitos (01 = enero, etcetera.)|
  |DD|día de dos dígitos del mes (del 01 al 31)|
  |T|signifier para inicio de los elementos de tiempo|
  |hh|hora de dos dígitos (00 a 23)|
  |MM|minutos de dos dígitos (00 a 59)|
  |ss|segundos de dos dígitos (00 a 59)|
  |.sss|tres dígitos del decimal de fracciones de segundo|
  |Z|Designador de hora UTC (Coordinated Universal Time)||
  
  Para obtener más detalles sobre el formato ISO 8601, vea [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

  **Comentarios:**

  GetCurrentDateTime es una función no determinista. 
  
  El resultado devuelto es UTC (Coordinated Universal Time).

  **Ejemplos**  
  
  El ejemplo siguiente muestra cómo obtener la hora de la fecha UTC actual mediante la función integrada GetCurrentDateTime.
  
```  
SELECT GetCurrentDateTime() AS currentUtcDateTime
```  
  
 Este es un conjunto de resultados de ejemplo.
  
```  
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.784Z"
}]  
```  

####  <a name="bk_get_current_timestamp"></a> GetCurrentTimestamp
 Devuelve el número de milisegundos que han transcurrido desde el jueves, 1 de enero de 1970 00:00:00. 
  
 **Sintaxis**  
  
```  
GetCurrentTimestamp ()  
```  
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor numérico, el número actual de milisegundos que han transcurrido desde la época de Unix, es decir, el número de milisegundos que han transcurrido desde el jueves, 1 de enero de 1970 00:00:00.

  **Comentarios:**

  GetCurrentTimestamp es una función no determinista. 
  
  El resultado devuelto es UTC (Coordinated Universal Time).

  **Ejemplos**  
  
  El ejemplo siguiente muestra cómo obtener la marca de tiempo actual mediante la función integrada GetCurrentTimestamp.
  
```  
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Este es un conjunto de resultados de ejemplo.
  
```  
[{
  "currentUtcTimestamp": 1556916469065
}]  
```  

###  <a name="bk_spatial_functions"></a> Funciones espaciales  
 Las siguientes funciones escalares realizan una operación en un valor de entrada de objeto espacial y devuelven un valor numérico o booleano.  
  
|||||
|-|-|-|-|
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)||||
  
####  <a name="bk_st_distance"></a> ST_DISTANCE  
 Devuelve la distancia entre dos expresiones Point, Polygon o LineString de GeoJSON.  
  
 **Sintaxis**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica con la distancia, que se expresa en metros para el sistema de referencia predeterminado.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo se devuelven todos los documentos de la familia en un radio de 30 km a partir de la ubicación especificada mediante la función integrada ST_DISTANCE. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a> ST_WITHIN  
 Devuelve una expresión condicional que indica si el objeto de GeoJSON (Point, Polygon o LineString) especificado en el primer argumento se encuentra en el objeto de GeoJSON (Point, Polygon o LineString) del segundo.  
  
 **Sintaxis**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
 
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor booleano.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo buscar todos los documentos de la familia en un polígono con ST_WITHIN.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a> ST_INTERSECTS  
 Devuelve una expresión condicional que indica si el objeto de GeoJSON (Point, Polygon o LineString) especificado en el primer argumento forma intersección con el objeto de GeoJSON (Point, Polygon o LineString) del segundo.  
  
 **Sintaxis**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
 
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor booleano.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo buscar todas las áreas de intersección con el polígono indicado.  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a> ST_ISVALID  
 Devuelve un valor booleano que indica si la expresión de Point, Polygon o LineString de GeoJSON especificada es válida.  
  
 **Sintaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo comprobar si un punto es válido con ST_VALID.  
  
  Por ejemplo, este punto tiene un valor de latitud que no está en el intervalo válido de valores [-90, 90], por lo que la consulta devuelve false.  
  
  Para los polígonos, la especificación de GeoJSON requiere que el último par de coordenadas indicado sea igual que el primero, para crear una forma cerrada. El orden de los puntos dentro de un polígono debe especificarse siguiendo el sentido contrario al de las agujas del reloj. Un polígono cuyos puntos se hayan especificado en el sentido de las agujas del reloj representa el inverso de la región dentro de él.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{ "b": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED  
 Devuelve un valor JSON que contiene un valor booleano si la expresión de Point, Polygon o LineString de GeoJSON especificada es válida; si no es válida, devuelve también el motivo como un valor de cadena.  
  
 **Sintaxis**  
  
```  
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto Point o Polygon de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor JSON que contiene un valor booleano si la expresión de punto o polígono GeoJSON especificada es válida; si no es válida, devuelve también el motivo en forma de valor de cadena.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo comprobar la validez (con detalles) mediante ST_ISVALIDDETAILED.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{  
  "b": {   
    "valid": false,   
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  
 
## <a name="next-steps"></a>Pasos siguientes  

- [Sintaxis SQL y consulta SQL para Cosmos DB](how-to-sql-query.md)

- [Documentación sobre Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)  
