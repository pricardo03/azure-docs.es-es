---
title: Interpretación semántica de Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Aprenda a usar la interpretación semántica en Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 26f8d885f8cf85ab849ba221392df206e492aac4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814479"
---
# <a name="semantic-interpretation"></a>Interpretación semántica

La interpretación semántica asocia una salida semántica con cada ruta de acceso interpretada mediante la gramática.  En concreto, el servicio evalúa la secuencia de instrucciones de los elementos `tag` que recorre la interpretación para procesar la salida final.  

Una instrucción puede ser una asignación de un literal o una variable a otra variable.  También puede asignar la salida de una función con 0 o más parámetros a una variable.  Se puede especificar cada parámetro de la función mediante un literal o una variable.  Si la función no devuelve ninguna salida, se omite la asignación.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Se especifica una variable mediante un identificador de nombre que comienza por una letra y consta únicamente de letras (A-Z), números (0-9) y guión bajo (\_).  Su tipo se deriva implícitamente del literal o del valor de salida de la función asignada a ella. 

A continuación se muestra una lista de los tipos de datos que se admiten actualmente:

|Type|DESCRIPCIÓN|Ejemplos|
|----|----|----|
|string|Secuencia de 0 o más caracteres|"Hello World!"<br/>""|
|Bool|Valor booleano|true<br/>false|
|Int32|Entero con signo de 32 bits.  -2.1e9 a 2.1e9|123<br/>-321|
|Int64|Entero con signo de 64 bits. -9.2e18 y 9.2e18|9876543210|
|Double|Punto flotante de doble precisión. 1,7e+/-308 (15 dígitos)|123.456789<br/>1.23456789e2|
|Guid|Identificador único global|"602DD052-CC47-4B23-A16A-26B52D30C05B"|
|Consultar|Expresión de consulta que especifica un subconjunto de objetos de datos del índice|All()<br/>And(*q1*, *q2*)|

## <a name="semantic-functions"></a>Funciones semánticas

Hay un conjunto integrado de funciones semánticas.  Permiten la construcción de consultas sofisticadas y proporcionan un control sensible al contexto sobre las interpretaciones de gramática.

### <a name="and-function"></a>Función And

`query = And(query1, query2);`

Devuelve una consulta compuesta de la intersección de dos consultas de entrada.

### <a name="or-function"></a>Función Or

`query = Or(query1, query2);`

Devuelve una consulta compuesta de la unión de dos consultas de entrada.

### <a name="all-function"></a>Función All

`query = All();`

Devuelve una consulta que incluye todos los objetos de datos.

En el siguiente ejemplo, usamos la función All() para compilar de forma iterativa una consulta basada en la intersección de 1 o varias palabras clave.

```
<tag>query = All();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = And(query, keyword);</tag>
</item>
```

### <a name="none-function"></a>Función None

`query = None();`

Devuelve una consulta que no incluye ningún objeto de datos.

En el siguiente ejemplo, usamos la función None() para compilar de forma iterativa una consulta basada en la unión de 1 o varias palabras clave.

```
<tag>query = None();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = Or(query, keyword);</tag>
</item>
```

### <a name="query-function"></a>Función Query

```
query = Query(attrName, value)
query = Query(attrName, value, op)
```

Devuelve una consulta que incluye solo los objetos de datos cuyo atributo *attrName* coincide con el valor *value* según la operación especificada *op*, cuyo valor predeterminado es "eq".  Normalmente, se utiliza un elemento `attrref` para crear una consulta basada en la cadena de consulta de entrada coincidente.  Si se proporciona u obtiene un valor a través de otros medios, la función Query() se puede utilizar para crear una consulta que coincida con este valor.

En el siguiente ejemplo, se usa la función Query() para implementar la compatibilidad con publicaciones académicas específicas de una década determinada.

```xml
written in the 90s
<tag>
  beginYear = Query("academic#Year", 1990, "ge");
  endYear = Query("academic#Year", 2000, "lt");
  year = And(beginYear, endYear);
</tag>
```

### <a name="composite-function"></a>Función Composite

`query = Composite(innerQuery);`

Devuelve una consulta que encapsula un *innerQuery* compuesto de coincidencias con atributos secundarios de un atributo común compuesto *attr*.  La encapsulación requiere que el atributo compuesto *attr* de cualquier objeto de datos coincidente disponga al menos de un valor que satisface individualmente la consulta *innerQuery*.  Tenga en cuenta que una consulta sobre atributos secundarios de un atributo compuesto tiene que estar encapsulada mediante la función Composite() antes de poderse combinar con otras consultas.

Por ejemplo, la siguiente consulta devuelve publicaciones académicas de "harry shum" durante su época en "microsoft":
```
Composite(And(Query("academic#Author.Name", "harry shum"), 
              Query("academic#Author.Affiliation", "microsoft")));
```

Por otra parte, la siguiente consulta devuelve publicaciones académicas en las que uno de los autores es "harry shum" y una de las afiliaciones es "microsoft":
```
And(Composite(Query("academic#Author.Name", "harry shum"), 
    Composite(Query("academic#Author.Affiliation", "microsoft")));
```

### <a name="getvariable-function"></a>Función GetVariable

`value = GetVariable(name, scope);`

Devuelve el valor de variable *name* que se definió en el *ámbito* especificado.  *name* es un identificador que comienza por una letra y consta únicamente de letras (A-Z), números (0-9) y guión bajo (_).  El *ámbito* se puede establecer en "solicitud" o "sistema".  Tenga en cuenta que las variables definidas en distintos ámbitos son diferentes entre sí, incluidas las que se definen a través de la salida de funciones semánticas.

Las variables del ámbito de solicitud se comparten entre todas las interpretaciones de la solicitud de interpretación actual.  Se pueden usar para controlar la búsqueda de interpretaciones sobre la gramática.

Las variables del sistema están predefinidas por el servicio y se pueden usar para recuperar diversas estadísticas acerca del estado actual del sistema.  A continuación se muestra el conjunto de variables del sistema que se admiten actualmente:

|NOMBRE|Type|DESCRIPCIÓN|
|----|----|----|
|IsAtEndOfQuery|Bool|True si la interpretación actual coincidió con todo el texto de la consulta de entrada|
|IsBeyondEndOfQuery|Bool|True si la interpretación actual le sugiere finalizaciones más allá del texto de la consulta de entrada|

### <a name="setvariable-function"></a>Función SetVariable

`SetVariable(name, value, scope);`

Asigna un *valor* a la variable *name* en el *ámbito* especificado.  *name* es un identificador que comienza por una letra y consta únicamente de letras (A-Z), números (0-9) y guión bajo (_).  Actualmente, el único valor válido de *ámbito* es "solicitud".  No hay ninguna variable del sistema configurable.

Las variables del ámbito de solicitud se comparten entre todas las interpretaciones de la solicitud de interpretación actual.  Se pueden usar para controlar la búsqueda de interpretaciones sobre la gramática.

### <a name="assertequals-function"></a>Función AssertEquals

`AssertEquals(value1, value2);`

Si *value1* y *value2* son equivalentes, la función se realiza correctamente y no tiene efectos secundarios.  En caso contrario, la función produce un error y rechaza la interpretación.

### <a name="assertnotequals-function"></a>Función AssertNotEquals

`AssertNotEquals(value1, value2);`

Si *value1* y *value2* no son equivalentes, la función se realiza correctamente y no tiene efectos secundarios.  En caso contrario, la función produce un error y rechaza la interpretación.


