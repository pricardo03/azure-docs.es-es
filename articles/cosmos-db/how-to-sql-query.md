---
title: Consultas SQL para Azure Cosmos DB
description: Más información sobre la sintaxis SQL, los conceptos de base de datos y las consultas SQL para Azure Cosmos DB. Puede utilizar SQL como lenguaje de consulta JSON en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: mjbrown
ms.openlocfilehash: f2ad46e7738582f82edcef6b54ac8234901c887d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885339"
---
# <a name="sql-query-examples-for-azure-cosmos-db"></a>Ejemplos de consultas SQL para Azure Cosmos DB

Azure Cosmos DB admite la realización de consulta de elementos mediante SQL (lenguaje de consulta estructurado) como lenguaje de consulta de JSON en cuentas de API de SQL. Al diseñar el lenguaje de consulta para Azure Cosmos DB, se tienen en cuenta los dos objetivos siguientes:

* En lugar de inventar un nuevo lenguaje de consulta, creamos Azure Cosmos DB para que fuera compatible con SQL, uno de los lenguajes de consulta más conocidos y populares. SQL para Azure Cosmos DB proporciona un modelo de programación formal para consultas enriquecidas en elementos JSON.  

* Azure Cosmos DB usa el modelo de programación de JavaScript como base para el lenguaje de consulta. La API de SQL se basa en el sistema de tipos de JavaScript, la evaluación de expresiones y la invocación de funciones. A su vez, esto proporciona un modelo de programación natural para proyecciones relacionales, navegación jerárquica por elementos JSON, autocombinaciones, consultas espaciales e invocación de funciones definidas por el usuario (UDF) escritas íntegramente en JavaScript, entre otras características.

Este artículo le guiará por algunos ejemplos de consultas SQL mediante el uso de elementos JSON sencillos. Para obtener información sobre la sintaxis del lenguaje SQL para Azure Cosmos DB, consulte el artículo de [referencia de la sintaxis SQL](sql-api-query-reference.md).

## <a id="GettingStarted"></a>Introducción a los comandos de SQL

Vamos a crear dos elementos JSON sencillos y a realizar consultas en esos datos. Tiene dos elementos JSON acerca de familias que debe insertar en una contenedor y, después, realizar consultas en los datos. Aquí tenemos un elemento JSON sencillo para las familias Andersen y Wakefield, los padres, los hijos (y sus mascotas), la dirección y la información de registro. El elemento tiene cadenas, números, valores booleanos, matrices y propiedades anidadas.

**Item1**

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow",
         "gender": "female",
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Aquí se muestra un segundo elemento que tiene una sutil diferencia, se usan `givenName` y `familyName` en lugar de `firstName` y `lastName`.

**Elemento 2**

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

Ahora realicemos algunas consultas con estos datos para entender algunos aspectos clave del lenguaje de consulta SQL de Azure Cosmos DB.

**Query1**: Por ejemplo, la consulta siguiente devuelve los elementos en los que el campo id coincide con `AndersenFamily`. Al ser una instrucción `SELECT *`, la salida de la consulta es el elemento JSON completo; para más información acerca de la sintaxis, consulte la [instrucción SELECT](sql-api-query-reference.md#select-query):

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Results**

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

**Query2:** Ahora preste atención al caso donde debemos cambiar el formato al resultado JSON con otra forma. Esta consulta proyecta un nuevo objeto JSON con dos campos seleccionados, Nombre y Ciudad, cuando la ciudad de la dirección tiene el mismo nombre que el estado. En este caso, "NY, NY" coincide.

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

**Results**

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

**Query3**: Esta consulta devuelve todos los nombres proporcionados de los niños de la familia cuyo id. coincida con `WakefieldFamily`, ordenados por ciudad de residencia.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

**Results**

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

A continuación encontrará algunos de los aspectos del lenguaje de consulta de Cosmos DB explicados mediante los ejemplos que hemos visto hasta ahora:  

* Como la API de SQL trabaja en valores JSON, trata entidades en forma de árbol en lugar de filas y columnas. Por consiguiente, el lenguaje permite que se haga referencia a los nodos del árbol a cualquier profundidad arbitraria, como `Node1.Node2.Node3…..Nodem`, de forma similar al lenguaje SQL relacional que hace alusión a la referencia dos partes de `<table>.<column>`.

* El lenguaje de consulta estructurado trabaja con datos sin esquemas. Por lo tanto, es necesario que el sistema de tipo se enlace dinámicamente. La misma expresión podría generar diversos tipos en distintos elementos. El resultado de una consulta es un valor JSON válido, pero no se garantiza que sea de un esquema fijo.  

* Azure Cosmos DB solo admite elementos JSON estrictos. Esto significa que el sistema de tipo y las expresiones se restringen para tratar únicamente tipos JSON. Para obtener más detalles, consulte la [especificación de JSON](https://www.json.org/).  

* Un contenedor de Cosmos DB es una colección sin esquemas de elementos JSON. Las relaciones en las entidades de datos tanto dentro de los elementos de un contenedor como entre ellos se capturan de manera implícita por independencia, no por las relaciones entre la clave principal y la clave externa. Se trata de un aspecto importante que merece la pena señalar teniendo en cuenta las combinaciones internas descritas posteriormente en este artículo.

## <a id="SelectClause"></a>Cláusula SELECT

Todas las consultas constan de una cláusula SELECT y cláusulas FROM y WHERE opcionales por estándares ANSI-SQL. Normalmente, para cada consulta, se enumera el origen de la cláusula FROM. Luego, el filtro de la cláusula WHERE se aplica en el origen para recuperar un subconjunto de elementos JSON. Por último, la cláusula SELECT se usa para proyectar los valores JSON solicitados en la lista seleccionada. Para obtener información sobre la sintaxis, consulte la [sintaxis SELECT](sql-api-query-reference.md#bk_select_query).

En el ejemplo siguiente se muestra una consulta SELECT típica:

**Consultar**

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Results**

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "seattle"
      }
    }]
```

### <a name="nested-properties"></a>Propiedades anidadas

En el ejemplo siguiente, se proyectan dos propiedades anidadas, `f.address.state` and `f.address.city`.

**Consultar**

```sql
    SELECT f.address.state, f.address.city
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Results**

```json
    [{
      "state": "WA",
      "city": "seattle"
    }]
```

La proyección también admite expresiones de JSON, como se muestra en el siguiente ejemplo:

**Consultar**

```sql
    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Results**

```json
    [{
      "$1": {
        "state": "WA",
        "city": "seattle",
        "name": "AndersenFamily"
      }
    }]
```

Analicemos el rol que `$1` tiene aquí. La cláusula `SELECT` debe crear un objeto JSON y, como no se proporciona ninguna clave, usamos nombres de variable de argumentos implícitos que empiezan por `$1`. Por ejemplo, esta consulta devuelve dos variables de argumentos implícitos, etiquetadas como `$1` and `$2`.

**Consultar**

```sql
    SELECT { "state": f.address.state, "city": f.address.city },
           { "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Results**

```json
    [{
      "$1": {
        "state": "WA",
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]
```

## <a id="FromClause"></a>Cláusula FROM

La cláusula FROM <from_specification> es opcional, a menos que el origen se filtre o se proyecte posteriormente en la consulta. Para obtener información sobre la sintaxis, consulte la [sintaxis FROM](sql-api-query-reference.md#bk_from_clause). Una consulta como `SELECT * FROM Families` indica que todo el contenedor Families entero es el origen sobre el que se va a realizar la enumeración. Se puede usar una RAÍZ de identificador especial para representar el contenedor en lugar de usar el nombre del contenedor.
La lista siguiente contiene las reglas que se aplican por consulta:

* Se puede establecer un alias para el contenedor, como `SELECT f.id FROM Families AS f` o simplemente `SELECT f.id FROM Families f`. Aquí, `f` es el equivalente de `Families`. `AS` es el identificador de una palabra clave opcional al alias.  

* Una vez establecido un alias, el origen original no puede enlazarse. Por ejemplo, `SELECT Families.id FROM Families f` no es válido sintácticamente porque el identificador "Families" no puede resolverse.  

* Todas las propiedades a las que es necesario hacer referencia deben estar completas. A falta de un cumplimiento del esquema estricto, esto se impone para evitar cualquier enlace ambiguo. Por lo tanto, `SELECT id FROM Families f` no es válido sintácticamente porque la propiedad `id` no está enlazada.

### <a name="get-subitems-using-from-clause"></a>Obtención de subelementos mediante una cláusula FROM

El origen también se puede reducir a un subconjunto más pequeño. Por ejemplo, para enumerar únicamente un subárbol en cada elemento, la subraíz podría convertirse en el origen, como se muestra en el ejemplo siguiente:

**Consultar**

```sql
    SELECT *
    FROM Families.children
```

**Results**

```json
    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

Aunque en el ejemplo anterior se usó una matriz como origen, también podría usarse un objeto como origen como se muestra en el ejemplo siguiente: Cualquier valor JSON válido (que no sea Undefined) que se pueda encontrar en el origen se tendrá en cuenta para su inclusión en el resultado de la consulta. Si algunas familias no tienen un valor `address.state` , se excluyen del resultado de la consulta.

**Consultar**

```sql
    SELECT *
    FROM Families.address.state
```

**Results**

```json
    [
      "WA",
      "NY"
    ]
```

## <a id="WhereClause"></a>Cláusula WHERE

La cláusula WHERE (**`WHERE <filter_condition>`**) es opcional. Especifica las condiciones que los elementos JSON proporcionados por el origen deben cumplir para que se les incluya como parte del resultado. Todos los elementos JSON deben evaluar que las condiciones especificadas sean "true" para que se les tenga en cuenta para el resultado. La capa de índice usa la cláusula WHERE para determinar el subconjunto absoluto más pequeño de documentos de origen que puede formar parte del resultado. Para obtener información sobre la sintaxis, consulte la [sintaxis WHERE](sql-api-query-reference.md#bk_where_clause).

En la consulta siguiente se solicitan elementos que contienen una propiedad de nombre cuyo valor es `AndersenFamily`. Se excluyen todos aquellos elementos que no tengan una propiedad de nombre o en los que el valor no coincida con `AndersenFamily`.

**Consultar**

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Results**

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "seattle"
      }
    }]
```

En el ejemplo anterior se mostraba una sencilla consulta de igualdad. La API de SQL también admite diversas expresiones escalares. Las que más se suelen usar son binarias y unarias. Las referencias de propiedad del objeto JSON de origen también son expresiones válidas.

Actualmente se admiten los siguientes operadores binarios y pueden usarse en consultas como se muestra en los ejemplos siguientes:  

|**Tipo de operador**  | **Valores** |
|---------|---------|
|Aritméticos | +,-,*,/,% |
|Bit a bit    | \|, &, ^, <<, >>, >>> (desplazamiento a la derecha con relleno de ceros) |
|Lógicos    | AND, OR, NOT      |
|De comparación | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|string     |  \|\| (concatenar) |

Echemos un vistazo a algunas consultas usando operadores binarios.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5
```

También se admiten los operadores unarios +,-, ~ y NOT, y se pueden usar dentro de consultas, tal como se muestra en el ejemplo siguiente:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

Además de los operadores unarios y binarios, también se permiten referencias de propiedad. Por ejemplo, `SELECT * FROM Families f WHERE f.isRegistered` devuelve el elemento JSON que contiene la propiedad `isRegistered` en la que el valor de la propiedad es igual al valor `true` JSON. Cualquier otro valor (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>`, etc.) hace que el elemento de origen se excluya del resultado. 

### <a name="equality-and-comparison-operators"></a>Operadores de igualdad y de comparación

En la siguiente tabla se muestra el resultado de las comparaciones de igualdad en la API de SQL entre dos tipos JSON cualesquiera.

| **Op** | **Undefined** | **Null** | **boolean** | **Number** | **string** | **Objeto** | **Matriz** |
|---|---|---|---|---|---|---|---|
| **Undefined** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Null** | Undefined | **Aceptar** | Undefined | Undefined | Undefined | Undefined | Undefined |
| **boolean** | Undefined | Undefined | **Aceptar** | Undefined | Undefined | Undefined | Undefined |
| **Number** | Undefined | Undefined | Undefined | **Aceptar** | Undefined | Undefined | Undefined |
| **string** | Undefined | Undefined | Undefined | Undefined | **Aceptar** | Undefined | Undefined |
| **Objeto** | Undefined | Undefined | Undefined | Undefined | Undefined | **Aceptar** | Undefined |
| **Matriz** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | **Aceptar** |

En cuanto a otros operadores de comparación como >, >=, !=, < y <=, se aplican las siguientes reglas:

* La comparación entre los tipos da lugar a Undefined.  
* La comparación entre dos objetos o dos matrices da lugar a Undefined.

Si el resultado de la expresión escalar del filtro es Undefined, el elemento correspondiente no se incluiría en el resultado, ya que Undefined no es igual lógicamente a "true".

## <a name="between-keyword"></a>Palabra clave BETWEEN
También puede usar la palabra clave BETWEEN para expresar consultas en intervalos de valores como en ANSI SQL. BETWEEN puede utilizarse con cadenas o números.

Por ejemplo, esta consulta devuelve todos los elementos de la familia en los que el curso del primer hijo se encuentre entre 1 y 5 (ambos inclusive).

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

Al contrario que en ANSI SQL, también se puede usar la cláusula BETWEEN en la cláusula FROM, como en el ejemplo siguiente.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

La principal diferencia entre usar BETWEEN en la API de SQL y SQL ANSI es que puede expresar consultas por rangos en propiedades de tipos mixtos (por ejemplo, "grade" podría ser un número (5) en algunos elementos y una cadena ("grade4") en otros). En estos casos, al igual que en JavaScript, una comparación entre dos tipos distintos da como resultado "undefined" y el elemento se omite.

> [!NOTE]
> Para que la consulta se ejecute de forma más rápida, no olvide crear una directiva de indización que use un tipo de índice de intervalo en cualquier ruta o propiedad numérica que se filtre en la cláusula BETWEEN.

### <a name="logical-and-or-and-not-operators"></a>Operadores lógicos (Y, O y NO)
Los operadores lógicos operan en valores booleanos. Las tablas de verdad lógica para estos operadores se muestran en las siguientes tablas.

**Operador OR**

| OR | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Undefined |
| Undefined |True |Undefined |Undefined |

**Y el operador**

| Y | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |False |Undefined |
| False |False |False |False |
| Undefined |Undefined |False |Undefined |

**Operador no**

| NO |  |
| --- | --- |
| True |False |
| False |True |
| Undefined |Undefined |

## <a name="in-keyword"></a>Palabra clave IN

La palabra clave IN puede usarse para comprobar si un valor especificado coincide con algún valor de una lista. Por ejemplo, esta consulta devuelve todos los elementos de la familia en los que el identificador sea "WakefieldFamily" o "AndersenFamily".

```sql
    SELECT *
    FROM Families
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')
```

Este ejemplo devuelve todos los elementos en los que el estado sea cualquiera de los valores especificados.

```sql
    SELECT *
    FROM Families
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")
```

## <a name="ternary--and-coalesce--operators"></a>Operadores ternario (?) y de fusión (??)

El operador ternario y el operador de combinación pueden usarse para crear expresiones condicionales, de forma similar a lenguajes de programación populares como C# y JavaScript. El operador ternario (?) puede resultar útil al construir nuevas propiedades JSON sobre la marcha. Así, ahora puede escribir consultas para clasificar los niveles de clase en formato de lenguaje natural, por ejemplo Principiante/Intermedio/Avanzado, como se muestra a continuación.

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

También puede anidar las llamadas al operador como en la consulta siguiente.

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel
    FROM Families.children[0] c
```

Como ocurre con otros operadores de consulta, si las propiedades a las que se hace referencia en la expresión condicional faltan en cualquiera de los elementos o si los tipos que se comparan son diferentes, dichos elementos se excluyen de los resultados de la consulta.

El operador de fusión (??) se puede usar para comprobar eficazmente la presencia de una propiedad en un elemento. Este operador es útil cuando se realizan consultas en datos semiestructurados o de tipos combinados. Por ejemplo, esta consulta devuelve el valor "lastName" si está presente o "surname" si no lo está.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a id="EscapingReservedKeywords"></a>Descriptor de acceso de propiedad entre comillas
También es posible obtener acceso a las propiedades mediante el operador de la propiedad entre comillas `[]`. Por ejemplo, `SELECT c.grade` and `SELECT c["grade"]` son equivalentes. Esta sintaxis es útil cuando se necesita crear una secuencia de escape para una propiedad que contiene espacios en blanco, caracteres especiales, o que comparte el nombre con una palabra clave SQL o una palabra reservada.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

## <a name="aliasing"></a>Establecimiento de alias

Ampliemos ahora el ejemplo anterior con un establecimiento de alias explícito para valores. AS es la palabra clave usada para el establecimiento de alias. Es opcional, como se muestra al proyectarse el segundo valor como `NameInfo`.

En caso de que una consulta tenga dos propiedades con el mismo nombre, el establecimiento de alias debe usarse para cambiar el nombre de las propiedades de modo que se elimine su ambigüedad en el resultado proyectado.

**Consultar**

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Results**

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="scalar-expressions"></a>Expresiones escalares

Además de las referencias de propiedad, la cláusula SELECT también admite expresiones escalares como constantes, expresiones aritméticas, expresiones lógicas, etc. Por ejemplo, aquí hay una sencilla consulta "Hello World".

**Consultar**

```sql
    SELECT "Hello World"
```

**Results**

```json
    [{
      "$1": "Hello World"
    }]
```

A continuación se muestra un ejemplo más complejo que usa una expresión escalar.

**Consultar**

```sql
    SELECT ((2 + 11 % 7)-2)/3
```

**Results**

```json
    [{
      "$1": 1.33333
    }]
```

En el ejemplo siguiente, el resultado de la expresión escalar es un valor booleano.

**Consultar**

```sql
    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f
```

**Results**

```json
    [
      {
        "AreFromSameCityState": false
      },
      {
        "AreFromSameCityState": true
      }
    ]
```

## <a name="object-and-array-creation"></a>Creación de objetos y matrices

Otra característica clave de la API de SQL es la creación de matrices u objetos. En el ejemplo anterior, creó un nuevo objeto JSON. De manera similar, también se pueden construir matrices como se muestra en los siguientes ejemplos:

**Consultar**

```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

**Results**

```json
    [
      {
        "CityState": [
          "seattle",
          "WA"
        ]
      },
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]
```

## <a id="ValueKeyword"></a>Palabra clave VALUE

La palabra clave **VALUE** proporciona una forma de devolver un valor JSON. Por ejemplo, la consulta que se muestra a continuación devuelve la expresión escalar `"Hello World"`, en lugar de `{$1: "Hello World"}`.

**Consultar**

```sql
    SELECT VALUE "Hello World"
```

**Results**

```json
    [
      "Hello World"
    ]
```

La siguiente consulta devuelve el valor JSON sin la etiqueta `"address"` en los resultados.

**Consultar**

```sql
    SELECT VALUE f.address
    FROM Families f
```

**Results**

```json
    [
      {
        "state": "WA",
        "county": "King",
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan",
        "city": "NY"
      }
    ]
```

El ejemplo siguiente se amplía para mostrar cómo devolver valores primitivos JSON (el nivel de hoja del árbol JSON).

**Consultar**

```sql
    SELECT VALUE f.address.state
    FROM Families f
```

**Results**

```json
    [
      "WA",
      "NY"
    ]
```

## <a name="-operator"></a>Operador *
Se admite el operador especial (*) para proyectar el elemento tal cual. Al usarse, debe ser el único campo proyectado. Aunque una consulta como `SELECT * FROM Families f` es válida, `SELECT VALUE * FROM Families f` y `SELECT *, f.id FROM Families f` no lo son.

**Consultar**

```sql
    SELECT * 
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Results**

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

## <a id="TopKeyword"></a>Operador TOP

La palabra clave TOP se puede usar para limitar la cantidad de valores de una consulta. Cuando se usa TOP junto con la cláusula ORDER BY, el conjunto de resultados se limita a los primeros N valores ordenados; de otro modo, devuelve los primeros N resultados en orden indefinido. Como procedimiento recomendado, en una instrucción SELECT, siempre use una cláusula ORDER BY con la cláusula TOP. La combinación de estas dos cláusulas es la única manera de indicar de forma previsible las filas a las que afecta TOP. 

**Consultar**

```sql
    SELECT TOP 1 *
    FROM Families f
```

**Results**

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

TOP se puede usar con un valor constante (como se muestra anteriormente) o con un valor variable usando consultas con parámetros. Si desea obtener más información, consulte las consultas con parámetros que aparecen a continuación.

## <a id="Aggregates"></a>Funciones de agregado

También puede realizar agregaciones en la cláusula `SELECT`. Las funciones de agregado realizan un cálculo en un conjunto de valores y devuelven un valor único. Por ejemplo, la consulta siguiente devuelve el número de elementos de la familia dentro del contenedor.

**Consultar**

```sql
    SELECT COUNT(1)
    FROM Families f
```

**Results**

```json
    [{
        "$1": 2
    }]
```

También puede devolver el valor escalar del agregado mediante la palabra clave `VALUE`. Por ejemplo, la siguiente consulta devuelve el número de valores como un único número:

**Consultar**

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

**Results**

```json
    [ 2 ]
```

También puede realizar agregados en combinación con filtros. Por ejemplo, la consulta siguiente devuelve el número de elementos cuya dirección se encuentre en el estado de Washington.

**Consultar**

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

**Results**

```json
    [ 1 ]
```

En la tabla siguiente se muestra la lista de funciones de agregado compatibles en la API de SQL. `SUM` y `AVG` se realizan a través de los valores numéricos, mientras que `COUNT`, `MIN`, y `MAX` se pueden realizar sobre valores nulos, valores booleanos, cadenas y números.

| Uso | DESCRIPCIÓN |
|-------|-------------|
| COUNT | Devuelve el número de elementos de la expresión. |
| SUM   | Devuelve la suma de todos los valores de la expresión. |
| MÍN   | Devuelve el valor mínimo de la expresión. |
| MÁX   | Devuelve el valor máximo de la expresión. |
| MEDIA   | Devuelve la media de los valores de la expresión. |

Las funciones de agregado también se pueden aplicar a los resultados de una iteración de la matriz. Para más información, vea [Iteración de matriz en consultas](#Iteration).

> [!NOTE]
> Al usar el Explorador de datos de Azure Portal, tenga en cuenta que las consultas de agregación pueden devolver resultados agregados parcialmente a través de una página de consulta. Los SDK generan un único valor acumulado en todas las páginas.
>
> Para realizar consultas de agregación mediante código, necesita .NET SDK 1.12.0, .NET Core SDK 1.1.0 o Java SDK 1.9.5 o superior.
>

## <a id="OrderByClause"></a>Cláusula ORDER BY

Al igual que en ANSI SQL, puede incluir una cláusula Order By opcional al realizar la consulta. La cláusula puede incluir un argumento ASC o DESC opcional para especificar el orden en que se deben recuperar los resultados.

Por ejemplo, aquí hay una consulta que recupera las familias ordenadas por nombre de la ciudad de residencia.

**Consultar**

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

**Results**

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Y la siguiente es una consulta que recupera las familias ordenadas por fecha de creación, que se almacena como un número que representa el tiempo epoch, es decir, el tiempo transcurrido desde el 1 de enero de 1970, en segundos.

**Consultar**

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.creationDate DESC
```

**Results**

```json
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472
      }
    ]
```

## <a id="Advanced"></a>Conceptos avanzados de base de datos y consultas SQL

### <a id="Iteration"></a>Iteración

Se ha agregado una nueva construcción mediante la palabra clave **IN** en la API de SQL para proporcionar compatibilidad con la iteración en las matrices JSON. El origen FROM proporciona compatibilidad con la iteración. Empecemos con el ejemplo siguiente:

**Consultar**

```sql
    SELECT *
    FROM Families.children
```

**Results**

```json
    [
      [
        {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        }, 
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

Ahora se analizará otra consulta que realiza una iteración sobre los elementos secundarios del contenedor. Observe la diferencia en la matriz de salida. En este ejemplo se divide `children` y se reducen los resultados a una sola matriz.  

**Consultar**

```sql
    SELECT *
    FROM c IN Families.children
```

**Results**

```json
    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]
```

Esto puede usarse más veces para filtrar por cada entrada individual de la matriz como se muestra en el ejemplo siguiente:

**Consultar**

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

**Results**

```json
    [{
      "givenName": "Lisa"
    }]
```

También puede aplicar agregaciones al resultado de la iteración de la matriz. Por ejemplo, la consulta siguiente cuenta el número de hijos entre todas las familias.

**Consultar**

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

**Results**

```json
    [
      {
        "$1": 3
      }
    ]
```

### <a id="Joins"></a>Combinaciones

En una base de datos relacional, la necesidad de combinar en tablas es importante. Es la consecuencia lógica de diseñar esquemas normalizados. En cambio, la API de SQL aborda el modelo de datos desnormalizado de los elementos sin esquemas, que es el equivalente lógico de una "autocombinación".

La sintaxis que admite el lenguaje es `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`. Por lo general, esta consulta devuelve un conjunto de **N** tuplas (tupla con **N** valores). Cada tupla tiene valores generados mediante la iteración de todos los alias de contenedor en los conjuntos correspondientes. En otras palabras, esta consulta realiza un cruce de productos completo de los conjuntos que participan en la combinación.

En los ejemplos siguientes se muestra cómo funciona la cláusula JOIN. En el siguiente ejemplo, el resultado está vacío porque tanto el cruce de productos de cada elemento del origen como un conjunto vacío están vacíos.

**Consultar**

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

**Results**

```json
    [{
    }]
```

En el ejemplo siguiente, la combinación se realiza entre la raíz del documento y la subraíz de `children`. Es un producto cruzado entre dos objetos JSON. El hecho de que los elementos secundarios sean una matriz no funciona en JOIN porque abordamos una sola raíz que es la matriz secundaria. Por consiguiente, el resultado contiene únicamente dos resultados, ya que el cruce de productos de cada elemento con la matriz genera exactamente solo un elemento.

**Consultar**

```sql
    SELECT f.id
    FROM Families f
    JOIN f.children
```

**Results**

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

En el ejemplo siguiente se muestra una combinación más convencional:

**Consultar**

```sql
    SELECT f.id
    FROM Families f
    JOIN c IN f.children
```

**Results**

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

Lo primero que hay que tener en cuenta es que `from_source` de la cláusula **JOIN** es un iterador. Por lo tanto, el flujo en este caso es como sigue:  

* Amplía cada elemento secundario **c** en la matriz.
* Aplique un cruce de productos con la raíz del elemento **f** con cada elemento secundario **c** del que se quitó el formato en el primer paso.
* Por último, proyecte solo la propiedad de nombre **f** del objeto raíz.

El primer elemento (`AndersenFamily`) contiene solo un elemento secundario, por lo que el conjunto de resultados contiene solo un objeto correspondiente a dicho elemento. El segundo elemento (`WakefieldFamily`) contiene dos elementos secundarios. Por consiguiente, el cruce de productos genera un objeto independiente para cada elemento secundario, lo que da lugar a dos objetos, uno para cada elemento secundario correspondiente a este elemento. Los campos de raíz de ambos elementos son los mismos, tal como se esperaría en un cruce de productos.

La utilidad real de JOIN es la formación de tuplas a partir del producto cruzado con una forma que de otro modo es difícil de proyectar. Además, como se ve en el siguiente ejemplo, podría filtrarse por la combinación de una tupla que permite que el usuario elija una condición satisfecha por las tuplas en general.

**Consultar**

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
```

**Results**

```json
    [
      {
        "familyName": "AndersenFamily",
        "childFirstName": "Henriette Thaulow",
        "petName": "Fluffy"
      },
      {
        "familyName": "WakefieldFamily",
        "childGivenName": "Jesse",
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

Este ejemplo es una ampliación natural del anterior y realiza una combinación doble. De este modo, el producto cruzado se puede ver como el pseudocódigo siguiente:

```
    for-each(Family f in Families)
    {
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName,
                  c.givenName AS childGivenName,
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }
```

`AndersenFamily` tiene un hijo que tiene una mascota. De esta manera, el producto cruzado produce una fila (1\*1\*1) a partir de esta familia. La familia Wakefield tiene, sin embargo, dos hijos, pero solo uno, "Jesse", tiene mascotas. Pero Jesse tiene dos mascotas. Así pues, el producto cruzado produce 1\*1\*2 = 2 filas a partir de esta familia.

En el ejemplo siguiente, hay un filtro adicional en `pet` que excluye todas las tuplas en las que el nombre de la mascota no sea "Shadow". Tenga en cuenta que podemos crear tuplas a partir de matrices, filtrar por cualquiera de los elementos de la tupla y proyectar cualquier combinación de los elementos.

**Consultar**

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"
```

**Results**

```json
    [
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

## <a id="JavaScriptIntegration"></a>Integración de JavaScript

Azure Cosmos DB proporciona un modelo de programación para ejecutar la lógica de aplicación basada en JavaScript directamente a los contenedores en términos de procedimientos almacenados y desencadenadores, este método permite soportes:

* La capacidad para realizar operaciones CRUD transaccionales de alto rendimiento y consultas en los elementos de un contenedor en virtud de una mayor integración del entorno en tiempo de ejecución de JavaScript directamente en el motor de base de datos.
* Un modelo natural de flujo de control, ámbito variable, asignación e integración de primitivas de control de excepciones con transacciones de base de datos. Para más información acerca de la compatibilidad de Azure Cosmos DB con la integración de JavaScript, consulte la documentación de programación del servidor de JavaScript.

### <a id="UserDefinedFunctions"></a>Funciones definidas por el usuario (UDF)

Junto con los tipos ya definidos en este artículo, la API de SQL ofrece compatibilidad para las funciones definidas por el usuario (UDF). En particular, se admiten las UDF escalares allí donde los desarrolladores puedan proporcionar cero o muchos argumentos y devolver un solo resultado de argumento. Cada uno de estos argumentos se comprueba para ver si se trata de valores JSON válidos.  

La sintaxis del lenguaje SQL se amplía para admitir una lógica de aplicación personalizada usando estas funciones definidas por el usuario. Las funciones definidas por el usuario pueden registrarse con la API de SQL y, a continuación, se puede hacer referencia a ellas como parte de una consulta SQL. De hecho, dichas funciones están exquisitamente diseñadas para que se las pueda llamar desde consultas. Como resultado de esta opción, las UDF no tienen acceso al objeto de contexto que los otros tipos de JavaScript (procedimientos y desencadenadores almacenados) tienen. Puesto que las consultas se ejecutan como de solo lectura, pueden ejecutarse en réplicas principales o secundarias. Por consiguiente, las UDF están diseñadas para ejecutarse en réplicas secundarias, a diferencia de otros tipos de JavaScript.

A continuación, se puede ver un ejemplo de cómo registrar una función definida por el usuario en la base de datos de Cosmos DB, concretamente en un contenedor de elementos.

```javascript
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) {
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
           regexMatchUdf).Result;  
```

El ejemplo anterior crea una UDF cuyo nombre es `REGEX_MATCH`. Acepta dos valores de cadena JSON, `input` and `pattern` , y comprueba si el primero coincide con el patrón especificado en el segundo mediante la función string.match() de JavaScript.

Ahora podemos usar esta UDF en una consulta de una proyección. Las UDF deben estar calificadas con el prefijo, que distingue mayúsculas de minúsculas, "udf." cuando se las llama desde las consultas.

> [!NOTE]
> Antes del 17/03/2015, Cosmos DB admitía las llamadas de UDF sin el prefijo "udf." al igual que SELECT REGEX_MATCH(). Este patrón de llamada está desusado.  
>

**Consultar**

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

**Results**

```json
    [
      {
        "$1": true
      },
      {
        "$1": false
      }
    ]
```

La UDF también puede usarse en un filtro tal como se muestra en el ejemplo siguiente, calificado igualmente con el prefijo "udf." prefijo:

**Consultar**

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

**Results**

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

Básicamente, las UDF son expresiones escalares válidas y pueden usarse en ambas proyecciones y filtros.

Para expandir el poder de las UDF, echemos un vistazo a otro ejemplo con lógica condicional:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
                seaLevelUdf);
```

A continuación se muestra un ejemplo que ejerce la UDF.

**Consultar**

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f
```

**Results**

```json
     [
      {
        "city": "seattle",
        "seaLevel": 520
      },
      {
        "city": "NY",
        "seaLevel": 410
      }
    ]
```

Al igual que en los ejemplos anteriores se muestran casos, las UDF integran el poder del lenguaje de JavaScript con la API de SQL para proporcionar una interfaz programable enriquecida a fin de hacer lógica condicional de procedimientos compleja con la ayuda de capacidades en tiempo real de JavaScript integradas.

La API de SQL proporciona los argumentos a las funciones definidas por el usuario de cada elementos del origen en la fase actual (cláusulas WHERE o SELECT) de procesamiento de la UDF. El resultado se incorpora en el proceso de ejecución general perfectamente. Si las propiedades a las que los parámetros UDF hacen referencia no están disponibles en el valor JSON, el parámetro se considera Undefined y así pues, la invocación de UDF se omite por completo. De forma similar, si el resultado de la UDF es Undefined, no se incluye en el resultado.

En resumen, las UDF son excelentes herramientas para hacer lógica de negocios como parte de la consulta.

### <a name="operator-evaluation"></a>Evaluación de operadores

Cosmos DB, en virtud de ser una base de datos JSON, establece paralelismos con los operadores de JavaScript y su semántica de evaluación. Aunque Cosmos DB intenta conservar la semántica de JavaScript en términos de soporte para JSON, la evaluación de operaciones se desvía en algunas instancias.

En la API de SQL, a diferencia del lenguaje SQL tradicional, los tipos de valores no suelen conocerse hasta que los valores se recuperan de la base de datos. Para ejecutar consultas de forma eficaz, la mayoría de los operadores tienen requisitos de tipo estrictos.

La API de SQL no realiza conversiones implícitas a diferencia de JavaScript. Por ejemplo, una consulta como `SELECT * FROM Person p WHERE p.Age = 21` coincide con elementos que contienen una propiedad Age cuyo valor es 21. Cualquier otro elemento cuya propiedad Age coincida con la cadena "21", u otras variaciones posiblemente infinitas, como "021", "21,0", "0021", "00021", etc. no producirán coincidencias. Esto es en contraste al JavaScript donde los valores de cadena se convierten de manera implícita en números (en función del operador, por ejemplo, ==). Esta opción es fundamental para conseguir una coincidencia de índices eficaz en la API de SQL.

## <a name="parameterized-sql-queries"></a>Consultas SQL con parámetros

Cosmos DB admite consultas con parámetros que se expresen con la notación \@ ya conocida. El uso de SQL con parámetros permite controlar y evitar de forma sólida la entrada por parte de los usuarios, impidiendo así la exposición accidental de datos a través de la inyección de código SQL.

Por ejemplo, puede escribir una consulta que acepte los apellidos y el estado de la dirección como parámetros y, a continuación, ejecutarla para distintos valores de los parámetros mencionados en función de la entrada del usuario.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Después, esta solicitud puede enviarse a Cosmos DB como consulta JSON con parámetros, como se muestra a continuación.

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

El argumento para TOP se puede definir mediante el uso de consultas con parámetros, tal como se muestra a continuación.

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Los valores de los parámetros pueden ser cualquier tipo de JSON válido (cadenas, números, booleanos, null o incluso matrices o JSON anidado). Además, como Cosmos DB no tiene ningún esquema, los parámetros no se validan respecto a ningún tipo.

## <a id="BuiltinFunctions"></a>Funciones integradas

Cosmos DB también admite un número de funciones integradas para operaciones comunes, que se pueden usar dentro de las consultas como funciones definidas por el usuario (UDF).

| Grupo de funciones | Operaciones |
|---------|----------|
| Funciones matemáticas | ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN y TAN |
| Funciones de comprobación de tipos | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED e IS_PRIMITIVE |
| Funciones de cadena | CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING y UPPER |
| Funciones de matriz | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH y ARRAY_SLICE |
| Funciones espaciales | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID y ST_ISVALIDDETAILED |

Si actualmente usa una función definida por el usuario (UDF) para la que ahora hay disponible una función integrada, debe usar la función integrada correspondiente ya que se va a ejecutar más rápidamente y va a ser más eficaz.

### <a name="mathematical-functions"></a>Funciones matemáticas

Las funciones matemáticas realizan un cálculo, basado en valores de entrada proporcionados como argumentos, y devuelven un valor numérico. Esta es una tabla de las funciones matemáticas integradas admitidas.

| Uso | DESCRIPCIÓN |
|----------|--------|
| [ABS (num_expr) | Devuelve el valor absoluto (positivo) de la expresión numérica especificada. |
| CEILING (num_expr) | Devuelve el valor entero más pequeño mayor o igual que la expresión numérica especificada. |
| FLOOR (num_expr) | Devuelve el valor entero más grande menor o igual que la expresión numérica especificada. |
| EXP (num_expr) | Devuelve el exponente de la expresión numérica especificada. |
| LOG (num_expr ,base) | Devuelve el logaritmo natural de la expresión numérica especificada o bien el logaritmo que utiliza la base especificada |
| LOG10 (num_expr) | Devuelve el valor logarítmico de base 10 de la expresión numérica especificada. |
| ROUND (num_expr) | Devuelve un valor numérico, redondeado al valor entero más cercano. |
| TRUNC (num_expr) | Devuelve un valor numérico, truncado al valor entero más cercano. |
| SQRT (num_expr) | Devuelve la raíz cuadrada de la expresión numérica especificada. |
| SQUARE (num_expr) | Devuelve el cuadrado de la expresión numérica especificada. |
| POWER (num_expr, num_expr) | Devuelve la potencia de la expresión numérica especificada al valor especificado. |
| SIGN (num_expr) | Devuelve el valor de signo (-1, 0, 1) de la expresión numérica especificada. |
| ACOS (num_expr) | Devuelve el ángulo, en radianes, cuyo coseno es la expresión numérica especificada; también se denomina arcocoseno. |
| ASIN (num_expr) | Devuelve el ángulo, en radianes, cuyo seno es la expresión numérica especificada. Esta función también se denomina arcoseno. |
| ATAN (num_expr) | Devuelve el ángulo, en radianes, cuya tangente es la expresión numérica especificada. También se denomina arcotangente. |
| ATN2 (num_expr) | Devuelve el ángulo, en radianes, entre el eje x positivo y el rayo desde el origen hasta el punto (y, x), donde x e y son los valores de las dos expresiones de punto flotante especificadas. |
| COS (num_expr) | Devuelve el coseno trigonométrico del ángulo especificado, en radianes, en la expresión especificada. |
| COT (num_expr) | Devuelve la cotangente trigonométrica del ángulo especificado, en radianes, en la expresión numérica especificada. |
| DEGREES (num_expr) | Devuelve el ángulo correspondiente en grados de un ángulo especificado en radianes. |
| PI () | Devuelve el valor constante de PI. |
| RADIANS (num_expr) | Devuelve radianes cuando se especifica una expresión numérica en grados. |
| SIN (num_expr) | Devuelve el seno trigonométrico del ángulo especificado, en radianes, en la expresión especificada. |
| TAN (num_expr) | Devuelve la tangente de la expresión de entrada en la expresión especificada. |

Por ejemplo, ya puede ejecutar como se muestra en el siguiente ejemplo:

**Consultar**

```sql
    SELECT VALUE ABS(-4)
```

**Results**

```json
    [4]
```

La principal diferencia entre las funciones de Cosmos DB y SQL ANSI es que están diseñadas para funcionar bien con datos sin esquemas y datos de esquemas mixtos. Por ejemplo, si tiene un elemento en el que falta la propiedad Size o que tiene un valor no numérico, como "desconocido", se omite en lugar de devolver un error.

### <a name="type-checking-functions"></a>Funciones de comprobación de tipos

Las funciones de comprobación de tipos permiten comprobar el tipo de una expresión dentro de consultas SQL. Las funciones de comprobación de tipos pueden utilizarse para determinar el tipo de propiedades dentro de los elementos sobre la marcha cuando es variable o desconocido. Esta es una tabla de las funciones de comprobación de tipos integradas admitidas.

| **Uso** | **DESCRIPCIÓN** |
|-----------|------------|
| [IS_ARRAY (expr)](sql-api-query-reference.md#bk_is_array) | Devuelve un valor booleano que indica si el tipo del valor es una matriz. |
| [IS_BOOL (expr)](sql-api-query-reference.md#bk_is_bool) | Devuelve un valor booleano que indica si el tipo del valor es un valor booleano. |
| [IS_NULL (expr)](sql-api-query-reference.md#bk_is_null) | Devuelve un valor booleano que indica si el tipo del valor es nulo. |
| [IS_NUMBER (expr)](sql-api-query-reference.md#bk_is_number) | Devuelve un valor booleano que indica si el tipo del valor es un número. |
| [IS_OBJECT (expr)](sql-api-query-reference.md#bk_is_object) | Devuelve un valor booleano que indica si el tipo del valor es un objeto JSON. |
| [IS_STRING (expr)](sql-api-query-reference.md#bk_is_string) | Devuelve un valor booleano que indica si el tipo del valor es una cadena. |
| [IS_DEFINED (expr)](sql-api-query-reference.md#bk_is_defined) | Devuelve un valor booleano que indica si se ha asignado un valor a la propiedad. |
| [IS_PRIMITIVE (expr)](sql-api-query-reference.md#bk_is_primitive) | Devuelve un valor booleano que indica si el tipo del valor es una cadena, número, valor booleano o null. |

Mediante estas funciones ya puede ejecutar consultas como se muestra en el siguiente ejemplo:

**Consultar**

```sql
    SELECT VALUE IS_NUMBER(-4)
```

**Results**

```json
    [true]
```

### <a name="string-functions"></a>Funciones de cadena

Las siguientes funciones escalares realizan una operación sobre un valor de entrada de cadena y devuelven una cadena, un valor numérico o un booleano. A continuación se facilita una tabla de funciones de cadena integradas:

| Uso | DESCRIPCIÓN |
| --- | --- |
| [LENGTH (str_expr)](sql-api-query-reference.md#bk_length) | Devuelve el número de caracteres de la expresión de cadena especificada. |
| [CONCAT (str_expr, str_expr [, str_expr])](sql-api-query-reference.md#bk_concat) | Devuelve una cadena que es el resultado de concatenar dos o más valores de cadena. |
| [SUBSTRING (str_expr, num_expr, num_expr)](sql-api-query-reference.md#bk_substring) | Devuelve parte de una expresión de cadena. |
| [STARTSWITH (str_expr, str_expr)](sql-api-query-reference.md#bk_startswith) | Devuelve un valor booleano que indica si la primera expresión de cadena empieza con la segunda. |
| [ENDSWITH (str_expr, str_expr)](sql-api-query-reference.md#bk_endswith) | Devuelve un valor booleano que indica si la primera expresión de cadena finaliza con la segunda. |
| [CONTAINS (str_expr, str_expr)](sql-api-query-reference.md#bk_contains) | Devuelve un valor booleano que indica si la primera expresión de cadena contiene la segunda. |
| [INDEX_OF (str_expr, str_expr)](sql-api-query-reference.md#bk_index_of) | Devuelve la posición inicial de la primera aparición de la expresión de la segunda cadena dentro de la primera expresión de cadena especificada, o -1 si no se encuentra la cadena. |
| [LEFT (str_expr, num_expr)](sql-api-query-reference.md#bk_left) | Devuelve la parte izquierda de una cadena con el número especificado de caracteres. |
| [RIGHT (str_expr, num_expr)](sql-api-query-reference.md#bk_right) | Devuelve la parte derecha de una cadena con el número especificado de caracteres. |
| [LTRIM (str_expr)](sql-api-query-reference.md#bk_ltrim) | Devuelve una expresión de cadena después de quitar los espacios en blanco iniciales. |
| [RTRIM (str_expr)](sql-api-query-reference.md#bk_rtrim) | Devuelve una expresión de cadena después de truncar todos los espacios en blanco finales. |
| [LOWER (str_expr)](sql-api-query-reference.md#bk_lower) | Devuelve una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas. |
| [UPPER (str_expr)](sql-api-query-reference.md#bk_upper) | Devuelve una expresión de cadena después de convertir datos de caracteres en minúsculas a mayúsculas. |
| [REPLACE (str_expr, str_expr, str_expr)](sql-api-query-reference.md#bk_replace) | Reemplaza todas las apariciones de un valor de cadena especificado por otro valor de cadena. |
| [REPLICATE (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/sql-api-sql-query-reference#bk_replicate) | Repite un valor de cadena un número especificado de veces. |
| [REVERSE (str_expr)](sql-api-query-reference.md#bk_reverse) | Devuelve el orden inverso de un valor de cadena. |

Con estas funciones, ya puede ejecutar consultas similares a las siguientes. Por ejemplo, puede devolver el nombre de familia en mayúsculas del modo siguiente:

**Consultar**

```sql
    SELECT VALUE UPPER(Families.id)
    FROM Families
```

**Results**

```json
    [
        "WAKEFIELDFAMILY",
        "ANDERSENFAMILY"
    ]
```

O concatenar cadenas como en este ejemplo:

**Consultar**

```sql
    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families
```

**Results**

```json
    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]
```

Las funciones de cadena también pueden usarse en la cláusula WHERE para filtrar los resultados, al igual que en el ejemplo siguiente:

**Consultar**

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")
```

**Results**

```json
    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]
```

### <a name="array-functions"></a>Funciones de matriz

Las siguientes funciones escalares realizan una operación en un valor de entrada de matriz y devuelven un valor numérico, booleano o de matriz. A continuación se facilita una tabla de funciones de matriz integradas:

| Uso | DESCRIPCIÓN |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](sql-api-query-reference.md#bk_array_length) |Devuelve el número de elementos de la expresión de matriz especificada. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](sql-api-query-reference.md#bk_array_concat) |Devuelve una matriz que es el resultado de concatenar dos o más valores de la matriz. |
| [ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](sql-api-query-reference.md#bk_array_contains) |Devuelve un valor booleano que indica si la matriz contiene el valor especificado. Puede especificar si la coincidencia es completa o parcial. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](sql-api-query-reference.md#bk_array_slice) |Devuelve parte de una expresión de matriz. |

Las funciones de matriz pueden usarse para manipular matrices en JSON. Por ejemplo, a continuación hay una consulta que devuelve todos los elementos en los que uno de los elementos primarios es "Robin Wakefield". 

**Consultar**

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })
```

**Results**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Puede especificar un fragmento parcial para los elementos que coinciden en la matriz. La siguiente consulta busca todos los elementos primarios con el `givenName` de `Robin`.

**Consultar**

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)
```

**Results**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Este es otro ejemplo que usa ARRAY_LENGTH para obtener el número de hijos por familia.

**Consultar**

```sql
    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 
```

**Results**

```json
    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]
```

### <a name="spatial-functions"></a>Funciones espaciales

Cosmos DB admite las siguientes funciones integradas de Open Geospatial Consortium (OGC) para realizar consultas geoespaciales. 

| Uso | DESCRIPCIÓN |
| --- | --- |
| ST_DISTANCE (point_expr, point_expr) | Devuelve la distancia entre dos expresiones Point, Polygon o LineString de GeoJSON. |
| T_WITHIN (point_expr, polygon_expr) | Devuelve una expresión booleana que indica si el primer objeto de GeoJSON (Point, Polygon o LineString) está en el segundo objeto de GeoJSON (Point, Polygon o LineString). |
| ST_INTERSECTS (spatial_expr, spatial_expr) | Devuelve una expresión booleana que indica si los dos objetos de GeoJSON especificados (Point, Polygon o LineString) intersecan. |
| ST_ISVALID | Devuelve un valor booleano que indica si la expresión de Point, Polygon o LineString de GeoJSON especificada es válida. |
| ST_ISVALIDDETAILED | Devuelve un valor JSON que contiene un valor booleano si la expresión de Point, Polygon o LineString de GeoJSON especificada es válida; si no es válida, devuelve también el motivo como un valor de cadena. |

Las funciones espaciales pueden usarse para realizar consultas de proximidad con datos espaciales. Por ejemplo, esta es una consulta que devuelve todos los elementos de la familia que estén dentro de un radio de 30 km de la ubicación especificada mediante la función integrada ST_DISTANCE.

**Consultar**

```sql
    SELECT f.id
    FROM Families f
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000
```

**Results**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Para más información acerca de la compatibilidad geoespacial en Cosmos DB, consulte [Uso de datos geoespaciales en Azure Cosmos DB](geospatial.md), que contiene funciones espaciales y sintaxis de SQL para Cosmos DB. Ahora veamos cómo funciona la consulta LINQ y cómo interactúa con la sintaxis que hemos visto hasta ahora.

## <a id="Linq"></a>API de LINQ to SQL

LINQ es un modelo de programación de .NET que expresa cálculos como consultas de secuencias de objetos. Cosmos DB proporciona una biblioteca del cliente para interactuar con LINQ facilitando una conversión entre objetos JSON y .NET, y una asignación a partir de un subconjunto de consultas de LINQ a consultas de Cosmos DB.

En la imagen que se muestra a continuación vemos la arquitectura de consultas compatibles con LINQ que usa Cosmos DB.  Con el cliente de Cosmos DB, los desarrolladores pueden crear un objeto **IQueryable** que consulta directamente al proveedor de consultas de Cosmos DB que, a continuación, convierte la consulta de LINQ en una consulta de Cosmos DB. La consulta pasa entonces al servidor de Cosmos DB para recuperar un conjunto de resultados en formato JSON. Los resultados devueltos se deserializan en una secuencia de objetos .NET en el cliente.

![Arquitectura de consultas compatibles con LINQ que usa la API de SQL: sintaxis SQL, lenguaje de consulta JSON, conceptos de base de datos y consultas SQL][1]

### <a name="net-and-json-mapping"></a>Asignación de .NET y JSON

La asignación entre objetos .NET y elementos JSON es natural (cada campo del miembro de datos se asigna a un objeto JSON, donde el nombre del campo se asigna a la parte "clave" del objeto y la parte de "valor" se asigna de forma recursiva a la parte de valor del objeto). Considere el siguiente ejemplo: El objeto Familia creado se asigna al elemento JSON como se muestra a continuación. Y viceversa, el elemento JSON se reasigna de nuevo a un objeto .NET.

**Clase de C#**

```csharp
    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };
```

**JSON**

```json
    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                "givenName": "Jesse",
                "gender": "female",
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller",
              "givenName": "Lisa",
              "gender": "female",
              "grade": 8
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };
```


### <a name="linq-to-sql-translation"></a>LINQ para traducción de lenguaje SQL

El proveedor de consulta de Cosmos DB realiza una mejor opción de asignación desde una consulta de LINQ a una consulta SQL de Cosmos DB. En la siguiente descripción, asumimos la familiaridad básica del lector con LINQ.

En primer lugar, para el sistema de tipos, admitimos todos los tipos primitivos JSON (tipos numéricos, booleanos, de cadena y null). Solo se admiten estos tipos JSON. Se admiten las siguientes expresiones escalares.

* Valores constantes: Entre estos se incluyen los valores constantes de los tipos de datos primitivos durante la evaluación de la consulta.
* Expresiones de índice de propiedad o matriz: estas expresiones hacen referencia a la propiedad de un objeto o a un elemento de matriz.
  
     family.Id;    family.children[0].familyName;    family.children[0].grade;    family.children[n].grade; //n es una variable int
* Expresiones aritméticas: entre estas se incluyen expresiones aritméticas comunes o valores numéricos y booleanos. Para ver la lista completa, consulte la especificación de SQL.
  
     2 * family.children[0].grade;    x + y;
* Expresión de comparación de cadenas: entre estas se incluye la comparación de un valor de cadena con algún valor de cadena constante.  
  
     mother.familyName == "Smith";    child.givenName == s; //s es una variable de cadena
* Expresión de creación de objetos o matrices: estas expresiones devuelven un objeto de tipo de valor compuesto o tipo anónimo o una matriz de estos objetos. Estos valores se pueden anidar.
  
     new Parent { familyName = "Smith", givenName = "Joe" }; new { first = 1, second = 2 }; //un tipo anónimo con dos campos              
     new int[] { 3, child.grade, 5 };

### <a id="SupportedLinqOperators"></a>Lista de los operadores LINQ admitidos

La siguiente es una lista de los operadores LINQ admitidos en el proveedor LINQ, incluido en el SDK de .NET de SQL.

* **Select**: Las proyecciones se traducen en la instrucción SQL SELECT, incluida la construcción de objetos.
* **Where**: Los filtros se traducen a la instrucción SQL WHERE y admiten la traducción entre &&amp;amp; , || y ! a los operadores SQL.
* **SelectMany**: Permite desenredar las matrices a la cláusula SQL JOIN. Se puede usar para encadenar/anidar expresiones para filtrar los elementos de la matriz.
* **OrderBy y OrderByDescending**: Se traduce a ORDER BY ascendente o descendente
* Los operadores **Count**, **Sum**, **Min**, **Max** y **Average** para la agregación y sus equivalentes asincrónicos **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync** y **AverageAsync**.
* **CompareTo**: Se traduce a las comparaciones de intervalos. Se usa frecuentemente para las cadenas, debido a que no son comparables en .NET.
* **Take**: Se traduce a la instrucción SQL TOP para limitar los resultados desde una consulta.
* **Funciones matemáticas**: ¿Admite la traducción desde Abs de .NET, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan, Truncate a las funciones SQL integradas equivalentes.
* **Funciones de cadena**: Admite la traducción desde Concat .NET, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString, ToUpper a las funciones SQL integradas equivalentes.
* **Funciones de matriz**: Admite la traducción desde Concat .NET, Contains y Count a las funciones SQL integradas equivalentes.
* **Funciones de extensión geoespacial**: Admite la traducción desde los métodos auxiliares Distance, Within, IsValid y IsValidDetailed a las funciones SQL integradas equivalentes.
* **Función de extensión de función definida por el usuario**: Admite la traducción desde el método de código auxiliar UserDefinedFunctionProvider.Invoke a la correspondiente función definida por el usuario.
* **Varios**: Admite la traducción de los operadores condicionales y de fusión. Puede traducir Contains a String CONTAINS, ARRAY_CONTAINS o SQL IN, según el contexto.

### <a name="sql-query-operators"></a>Operadores de consulta SQL

A continuación, vemos algunos ejemplos que ilustran la traducción de algunos de los operadores de consulta de LINQ estándar a consultas de Cosmos DB.

#### <a name="select-operator"></a>Operador Select

La sintaxis es `input.Select(x => f(x))`, donde `f` es una expresión escalar.

**LINQ lambda expression**

```csharp
    input.Select(family => family.parents[0].familyName);
```

**SQL** 

```sql
    SELECT VALUE f.parents[0].familyName
    FROM Families f
```

**LINQ lambda expression**

```csharp
    input.Select(family => family.children[0].grade + c); // c is an int variable
```

**SQL**

```sql
    SELECT VALUE f.children[0].grade + c
    FROM Families f
```

**LINQ lambda expression**

```csharp
    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });
```

**SQL** 

```sql
    SELECT VALUE {"name":f.children[0].familyName,
                  "grade": f.children[0].grade + 3 }
    FROM Families f
```


#### <a name="selectmany-operator"></a>Operador SelectMany

La sintaxis es `input.SelectMany(x => f(x))`, donde `f` es una expresión escalar que devuelve un tipo de contenedor.

**LINQ lambda expression**

```csharp
    input.SelectMany(family => family.children);
```

**SQL**

```sql
    SELECT VALUE child
    FROM child IN Families.children
```

#### <a name="where-operator"></a>Operador Where

La sintaxis es `input.Where(x => f(x))`, donde `f` es una expresión escalar que devuelve un valor booleano.

**LINQ lambda expression**

```csharp
    input.Where(family=> family.parents[0].familyName == "Smith");
```

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
```

**LINQ lambda expression**

```csharp
    input.Where(
        family => family.parents[0].familyName == "Smith" &&
        family.children[0].grade < 3);
```

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3
```

### <a name="composite-sql-queries"></a>Composición de consultas SQL

Los operadores anteriores pueden ser compuestos para formar consultas más eficaces. Dado que Cosmos DB admite contenedores anidados, la composición puede concatenarse o anidarse.

#### <a name="concatenation"></a>Concatenación

La sintaxis es `input(.|.SelectMany())(.Select()|.Where())*`. Una consulta concatenada puede empezar por una consulta `SelectMany` opcional seguida de varios operadores `Select` o `Where`.

**LINQ lambda expression**

```csharp
    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");
```

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
```

**LINQ lambda expression**

```csharp
    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);
```

**SQL**

```sql
    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3
```

**LINQ lambda expression**

```csharp
    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
```

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)
```

**LINQ lambda expression**

```csharp
    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");
```

**SQL**

```sql
    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"
```

#### <a name="nesting"></a>Anidamiento

La sintaxis es `input.SelectMany(x=>x.Q())`, donde Q es un operador `Select`, `SelectMany` o `Where`.

En una consulta anidada, la consulta interna se aplica a cada uno de los elementos del contenedor externo. Una característica importante es que la consulta interna puede hacer referencia a los campos de los elementos del contenedor externo como las autocombinaciones.

**LINQ lambda expression**

```csharp
    input.SelectMany(family=>
        family.parents.Select(p => p.familyName));
```

**SQL**

```sql
    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents
```

**LINQ lambda expression**

```csharp
    input.SelectMany(family =>
        family.children.Where(child => child.familyName == "Jeff"));
```

**SQL**

```sql
    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"
```

**LINQ lambda expression**

```csharp
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));
```

**SQL**

```sql
    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName
```

## <a id="ExecutingSqlQueries"></a>Ejecutar consultas de SQL

Cosmos DB expone recursos mediante la API de REST, que puede invocar cualquier lenguaje capaz de realizar solicitudes de HTTP/HTTPS. Además, Cosmos DB ofrece bibliotecas de programación para varios lenguajes populares como .NET, Node.js, JavaScript y Python. La API de REST y las diversas bibliotecas admiten la realización de consultas a través de SQL. El SDK de .NET admite la realización de consultas de LINQ, además del lenguaje SQL.

En los ejemplos siguientes se muestra cómo crear una consulta y enviarla a una cuenta de la base de datos de Cosmos DB.

### <a id="RestAPI"></a>API DE REST

Cosmos DB ofrece un modelo de programación RESTful sobre HTTP. Las cuentas de la base de datos pueden aprovisionarse usando una suscripción de Azure. El modelo de recursos de Cosmos DB consta de un conjunto de recursos en una cuenta de base de datos, cada uno de los cuales se puede dirigir mediante un URI lógico y estable. En este elemento, se hace referencia a un conjunto de recursos como fuente. Una cuenta de base de datos consta de un conjunto de bases de datos, cada una de las cuales contiene varios contenedores que, a su vez, contienen elementos, funciones definidas por el usuario y otros tipos de recursos.

El modelo de interacción básico con estos recursos se lleva a cabo a través de los verbos GET, PUT, POST y DELETE de HTTP con su interpretación estándar. El verbo POST se usa para la creación de un nuevo recurso, para ejecutar un procedimiento almacenado o para emitir una consulta de Cosmos DB. Las consultas siempre son operaciones de solo lectura sin efectos secundarios.

En los ejemplos siguientes se muestra una solicitud POST para una consulta de API de SQL realizada en un contenedor que incluye los dos elementos de ejemplo que hemos revisado hasta el momento. La consulta tiene un filtro sencillo por la propiedad de nombre JSON. Fíjese en el uso de los encabezados `x-ms-documentdb-isquery` y Content-Type: `application/query+json` para denotar que la operación es una consulta.

**Solicitud**
```
    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",
        "parameters": [
            {"name": "@familyId", "value": "AndersenFamily"}
        ]
    }
```

**Results**

```
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

En el segundo ejemplo se muestra una consulta más compleja que devuelve varios resultados de la combinación.

**Solicitud**
```
    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT
                     f.id AS familyName,
                     c.givenName AS childGivenName,
                     c.firstName AS childFirstName,
                     p.givenName AS petName
                  FROM Families f
                  JOIN c IN f.children
                  JOIN p in c.pets",
        "parameters": [] 
    }
```

**Results**

```
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Si los resultados de una consulta no caben en una sola página, la API de REST devuelve un token de continuación a través del encabezado de respuesta `x-ms-continuation-token` . Los clientes pueden paginar los resultados incluyendo el encabezado en resultados posteriores. El número de resultados por página también se puede controlar a través del encabezado numérico `x-ms-max-item-count` . Si la consulta especificada tiene una función de agregación como `COUNT`, la página de consulta puede devolver un valor parcialmente agregado sobre la página de resultados. Los clientes deben realizar una agregación de segundo nivel con estos resultados para generar los resultados finales, por ejemplo, sumar los recuentos devueltos en las páginas individuales para devolver el recuento total.

Para administrar la directiva de coherencia de datos para consultas, use el encabezado `x-ms-consistency-level` como todas las solicitudes de la API de REST. Para que la sesión sea coherente, también es necesario enviar el último encabezado de cookie `x-ms-session-token` en la solicitud de la consulta. La directiva de indexación del contenedor consultado también puede afectar a la coherencia de los resultados de la consulta. En el caso de los contenedores, con la configuración de la directiva de indexación predeterminada, el índice siempre está actualizado con el contenido del elemento y los resultados de la consulta coinciden con la coherencia elegida para los datos. Para más información, vea [Niveles de coherencia en Azure Cosmos DB][consistency-levels].

Si la directiva de indexación configurada en el contenedor no puede admitir la consulta especificada, el servidor de Azure Cosmos DB devuelve el error 400 "Solicitud incorrecta". Este mensaje de error se devuelve para las consultas con excluir explícitamente de la indexación de rutas de acceso. Se puede especificar el encabezado `x-ms-documentdb-query-enable-scan` para permitir que la consulta realice un examen si algún índice no está disponible.

Puede obtener métricas detalladas sobre la ejecución de consultas si establece el encabezado `x-ms-documentdb-populatequerymetrics` en `True`. Para más información, vea [SQL query metrics for Azure Cosmos DB](sql-api-query-metrics.md) (Métricas de consulta de SQL para la API de Azure Cosmos DB).

### <a id="DotNetSdk"></a>SDK de C# (.NET)

El SDK de .NET admite la realización de consultas de LINQ y SQL. En el ejemplo siguiente se muestra cómo realizar la consulta de filtro incluida anteriormente en este elemento.
```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(containerLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(containerLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(containerLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

En este ejemplo se comparan dos propiedades para igualdad en cada elemento y se usan proyecciones anónimas.

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family
        FROM Families f
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(containerLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(containerLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

En el ejemplo siguiente se muestran combinaciones, expresadas a través de SelectMany de LINQ.

```csharp
    foreach (var pet in client.CreateDocumentQuery(containerLink,
          @"SELECT p
            FROM Families f
                 JOIN c IN f.children
                 JOIN p in c.pets
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

El cliente de .NET se itera automáticamente a través de todas las páginas de resultados de la consulta de los bloques foreach, como se muestra anteriormente. Las opciones de consulta especificadas en la sección de la API de REST también están disponibles en el SDK de .NET mediante las clases `FeedOptions` and `FeedResponse` del método CreateDocumentQuery. El número de páginas se puede controlar con el valor `MaxItemCount` .

Puede controlar expresamente la paginación creando `IDocumentQueryable` mediante el `IQueryable` objeto, a continuación, leyendo el `ResponseContinuationToken` nuevo de valores y devolviéndolos como `RequestContinuationToken` en `FeedOptions`. `EnableScanInQuery` se puede establecer para habilitar los exámenes cuando la consulta no se admite en la directiva de indexación configurada. En el caso de los contenedores con particiones, puede usar `PartitionKey` para ejecutar la consulta en una sola partición (aunque Azure Cosmos DB puede extraer este valor automáticamente del texto de consulta) y `EnableCrossPartitionQuery` para ejecutar consultas que se deben ejecutar en varias particiones.

Consulte los [ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet) para obtener más casos que contengan consultas.

### <a id="JavaScriptServerSideApi"></a>API del servidor de JavaScript

Cosmos DB proporciona un modelo de programación para ejecutar la lógica de aplicación basada en JavaScript directamente en los contenedores mediante procedimientos almacenados y desencadenadores. La lógica de JavaScript registrada en un nivel de contenedor puede emitir operaciones de base de datos en las operaciones de los elementos del contenedor especificado. Estas operaciones se incluyen en transacciones ACID ambientales.

En el ejemplo siguiente se muestra cómo usar queryDocuments en la API del servidor de JavaScript para realizar consultas a partir de procedimientos y desencadenadores almacenados dentro.

```javascript
    function businessLogic(name, author) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a id="References"></a>Referencias

1. [Introducción a Azure Cosmos DB][introduction]
2. [Especificación de Cosmos DB SQL Azure](https://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Ejemplos de Azure Cosmos DB .NET](https://github.com/Azure/azure-cosmosdb-dotnet)
4. [Niveles de coherencia de Azure Cosmos DB][consistency-levels]
5. ANSI SQL 2011 [https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [https://json.org/](https://json.org/)
7. Específico de JavaScript [https://www.ecma-international.org/publications/standards/Ecma-262.htm](https://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [https://msdn.microsoft.com/library/bb308959.aspx](https://msdn.microsoft.com/library/bb308959.aspx) 
9. Técnicas de evaluación de consultas para bases de datos grandes [https://dl.acm.org/citation.cfm?id=152611](https://dl.acm.org/citation.cfm?id=152611)
10. Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994
11. Lu, Ooi, Tan, Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: A Not-So-Foreign Language for Data Processing, SIGMOD 2008.
13. G. Graefe. El marco de cascadas para la optimización de consultas. IEEE Data Eng. Bull., 18(3): 1995.

[1]: ./media/how-to-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md
