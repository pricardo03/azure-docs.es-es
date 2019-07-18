---
title: Funciones definidas por el usuario (UDF) en Azure Cosmos DB
description: Obtenga información sobre las funciones definidas por el usuario en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: e168e450230720f4ad78516e6edcdc3aa08ba3e1
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343121"
---
# <a name="user-defined-functions-udfs-in-azure-cosmos-db"></a>Funciones definidas por el usuario (UDF) en Azure Cosmos DB

La API de SQL brinda compatibilidad con funciones definidas por el usuario (UDF). Con las UDF escalares, puede pasar cero o muchos argumentos y devolver un resultado de argumento único. La API comprueba que cada argumento sean valores JSON legales.  

La API amplía la sintaxis de SQL para admitir una lógica de aplicación personalizada con UDF. Puede registrar las UDF con la API de SQL y hacer referencia a ellas en las consultas SQL. De hecho, dichas funciones están exquisitamente diseñadas para que se las pueda llamar desde consultas. Como consecuencia, las UDF no tienen acceso al objeto de contexto, como lo tienen otros tipos de JavaScript, por ejemplo, los procedimientos y desencadenadores almacenados. Las consultas son de solo lectura y pueden ejecutarse en réplicas principales o secundarias. Las UDF, a diferencia de otros tipos de JavaScript, están diseñadas para ejecutarse en réplicas secundarias.

En el ejemplo siguiente se registra una UDF en un contenedor de elementos en la base de datos de Cosmos DB. El ejemplo crea una UDF cuyo nombre es `REGEX_MATCH`. Acepta dos valores de cadena JSON, `input` y `pattern`, y comprueba si el primero coincide con el patrón especificado en el segundo mediante la función `string.match()` de JavaScript.

## <a name="examples"></a>Ejemplos

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

Ahora, use esta UDF en la proyección de una consulta. Debe calificar las UDF con el prefijo que distingue mayúsculas de minúsculas `udf.` al llamarlas desde las consultas.

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

Los resultados son:

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

Puede usar la UDF calificada con el prefijo `udf.` dentro de un filtro, como se muestra en el ejemplo siguiente:

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

Los resultados son:

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

Básicamente, las UDF son expresiones escalares válidas que puede usar tanto en proyecciones como en filtros.

Para ampliar la capacidad de las UDF, echemos un vistazo a otro ejemplo con lógica condicional:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'Seattle':
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

En el ejemplo siguiente se pone en práctica la UDF:

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f
```

Los resultados son:

```json
     [
      {
        "city": "Seattle",
        "seaLevel": 520
      },
      {
        "city": "NY",
        "seaLevel": 410
      }
    ]
```

Si las propiedades a las que hacen referencia los parámetros de UDF no están disponibles en el valor JSON, el parámetro se considera Undefined y la invocación de UDF se omite. De forma similar, si el resultado de la UDF es Undefined, no se incluye en el resultado.

Como se muestran en los ejemplos anteriores, las UDF integran la eficacia del lenguaje JavaScript con la API de SQL. Las UDF proporcionan una interfaz programable enriquecida para lograr una lógica condicional de procedimientos compleja con la ayuda de funcionalidades de tiempo de ejecución de JavaScript integradas. La API de SQL proporciona a las UDF los argumentos para cada elemento de origen en la fase actual de procesamiento de las cláusulas WHERE o SELECT. El resultado se incorpora a la perfección en la canalización de ejecución general. En resumen, las UDF son excelentes herramientas para hacer lógica de negocios compleja como parte de las consultas.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Cosmos DB](introduction.md)
- [System functions](sql-query-system-functions.md) (Funciones del sistema)
- [Aggregates](sql-query-aggregates.md) (Agregados)