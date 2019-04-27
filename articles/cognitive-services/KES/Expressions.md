---
title: Expresiones de consulta estructuradas de Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Aprenda a utilizar expresiones de consulta estructuradas en Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: a544cdca1ef4be56fcf368a39040f4ee85076a9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60815100"
---
# <a name="structured-query-expression"></a>Expresiones de consulta estructuradas

Una expresión de consulta estructurada especifica un conjunto de operaciones para evaluar en el índice de datos.  Se compone de expresiones de consulta de atributos y funciones de nivel superior.  Use el método [*evaluate*](evaluateMethod.md) para procesar los objetos que coinciden con la expresión.  El siguiente es un ejemplo del dominio de publicaciones académicas que devuelve las publicaciones creadas por Jaime Teevan desde el año 2013.

`And(Composite(Author.Name=='jaime teevan'),Y>=2013)`

Las expresiones de consulta estructuradas se pueden obtener a partir de solicitudes [*interpret*](interpretMethod.md), en las que la salida semántica de cada interpretación es una expresión de consulta estructurada que devuelve objetos de índice que coinciden con la consulta de entrada en lenguaje natural.  Como alternativa, también se pueden crear manualmente mediante la sintaxis descrita en esta sección.

## <a name="attribute-query-expression"></a>Expresión de consulta de atributo

Una expresión de consulta de atributo identifica un conjunto de objetos basados en la coincidencia con un atributo determinado.  Se admiten diferentes operaciones de establecimiento de coincidencias dependiendo del tipo de atributo y de la operación indexada que se especificó en el [esquema](SchemaFormat.md):

| Type | Operación | Ejemplos |
|------|-------------|------------|
| String | equals | Title='latent semantic analysis'  (canonical + synonyms) |
| String | equals | Author.Name=='susan t dumais'  (canonical only)|
| String | starts_with | Title='latent s'... |
| Int32/Int64/Double | equals | Year=2000 |
| Int32/Int64/Double | starts_with | Year='20'... (any decimal value starting with "20") |
| Int32/Int64/Double | is_between | Year&lt;2000 <br/> Year&lt;=2000 <br/> Year&gt;2000 <br/> Year&gt;=2000 <br/> Year=[2010,2012) *(includes only left boundary value: 2010, 2011)* <br/> Year=[2000,2012] *(includes both boundary values: 2010, 2011, 2012)* |
| Date | equals | BirthDate='1984-05-14' |
| Date | is_between | BirthDate&lt;='2008/03/14' <br/> PublishDate=['2000-01-01','2009-12-31'] |
| Guid | equals | Id='602DD052-CC47-4B23-A16A-26B52D30C05B' |


Por ejemplo, la expresión "Title='latent s'..." busca coincidencias con todas las publicaciones académicas cuyo título empieza por la cadena "latent s".  Para evaluar esta expresión, el atributo Title debe especificar la operación "starts_with" en el esquema que se usó para compilar el índice.

Para los atributos con sinónimos asociados, una expresión de consulta puede especificar los objetos cuyo valor canónico coincide con una cadena determinada mediante el operador "==", o los objetos en los que cualquiera de sus valores canónicos o de sinónimos coinciden mediante el operador "=".  Ambos requieren que se especifique el operador "equals" en la definición de atributo.


## <a name="functions"></a>Functions

Hay un conjunto integrado de funciones que permiten la construcción de expresiones de consulta más sofisticadas a partir de consultas de atributos básicas.

### <a name="and-function"></a>Función And

`And(expr1, expr2)`

Devuelve la intersección de las dos expresiones de consulta de entrada.

El ejemplo siguiente devuelve publicaciones académicas publicadas en el año 2000 sobre recuperación de información:

`And(Year=2000, Keyword=='information retrieval')`

### <a name="or-function"></a>Función Or

`Or(expr1, expr2)`

Devuelve la unión de las dos expresiones de consulta de entrada.

El ejemplo siguiente devuelve publicaciones académicas publicadas en el año 2000 sobre recuperación de información o modelado de usuario:

`And(Year=2000, Or(Keyword='information retrieval', Keyword='user modeling'))`

### <a name="composite-function"></a>Función Composite

`Composite(expr)`

Devuelve una expresión que encapsula una expresión interna compuesta de consultas en atributos secundarios de un atributo común compuesto.  La encapsulación requiere que el atributo compuesto de cualquier objeto de datos coincidente disponga al menos de un valor que satisface individualmente la expresión interna.  Tenga en cuenta que una expresión de consulta sobre atributos secundarios de un atributo compuesto tiene que estar encapsulada mediante la función Composite() antes de poderse combinar con otras expresiones de consulta.

Por ejemplo, la siguiente expresión devuelve publicaciones académicas de "harry shum" durante su época en "microsoft":

```
Composite(And(Author.Name="harry shum", 
              Author.Affiliation="microsoft"))
```

Por otra parte, la siguiente expresión devuelve publicaciones académicas en las que uno de los autores es "harry shum" y una de las afiliaciones es "microsoft":

```
And(Composite(Author.Name="harry shum"), 
    Composite(Author.Affiliation="microsoft"))
```
