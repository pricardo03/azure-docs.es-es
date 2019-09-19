---
title: Introducción a las consultas SQL en Azure Cosmos DB
description: Introducción a las consultas SQL
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: tisande
ms.openlocfilehash: 8de5140d0146ccbb18f41867e1c716aa2f3897b7
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71001903"
---
# <a name="getting-started-with-sql-queries"></a>Introducción a las consultas SQL

Las cuentas de API SQL de Azure Cosmos DB admiten la realización de consulta de elementos mediante SQL (lenguaje de consulta estructurado) como lenguaje de consulta de JSON. Los objetivos de diseño del lenguaje de consulta de Azure Cosmos DB son:

* Admitir SQL, uno de los lenguajes de consulta más conocidos, en lugar de inventar uno nuevo. SQL proporciona un modelo de programación formal para consultas enriquecidas a través de elementos JSON.  

* Usar el modelo de programación de JavaScript como base para el lenguaje de consulta. El sistema de tipos de JavaScript, la evaluación de expresiones y la invocación de funciones son las bases de la API de SQL. Estas bases proporcionan un modelo de programación natural para características como proyecciones relacionales, navegación jerárquica por elementos JSON, autocombinaciones, consultas espaciales e invocación de funciones definidas por el usuario (UDF) escritas íntegramente en JavaScript.

## <a name="upload-sample-data"></a>Cargar datos de ejemplo

En la cuenta de API de SQL de Cosmos DB, cree un contenedor denominado `Families`. Cree dos elementos JSON sencillos en el contenedor. Puede ejecutar la mayoría de las consultas de ejemplo en la documentación de consultas de Azure Cosmos DB con este conjunto de datos.

### <a name="create-json-items"></a>Crear elementos JSON

Con el siguiente código se crean dos elementos JSON sencillos sobre familias. Los elementos JSON sencillos para las familias Andersen y Wakefield incluyen a los padres, los hijos, sus mascotas, la dirección y la información de registro. El primer elemento tiene cadenas, números, valores booleanos, matrices y propiedades anidadas.


```json
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
  "address": { "state": "WA", "county": "King", "city": "Seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

El segundo elemento usa `givenName` y `familyName` en lugar de `firstName` y `lastName`.

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
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

### <a name="query-the-json-items"></a>Consulta de los elementos JSON

Pruebe algunas consultas con los datos JSON para entender algunos aspectos clave del lenguaje de consulta SQL de Azure Cosmos DB.

La consulta siguiente devuelve los elementos en los que el campo `id` coincide con `AndersenFamily`. Puesto que es una consulta `SELECT *`, la salida de la consulta es el elemento JSON completo. Para más información sobre la sintaxis de SELECT, consulte [Instrucción SELECT](sql-query-select.md). 

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Los resultados de la consulta son: 

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
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

La siguiente consulta vuelve a formatear la salida JSON en una forma diferente. La consulta proyecta un nuevo objeto JSON `Family` con dos campos seleccionados, `Name` y `City`, cuando la ciudad de la dirección es la misma que el estado. "NY, NY" coincide con esto.

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

Los resultados de la consulta son:

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

La consulta siguiente devuelve todos los nombres proporcionados de los hijos de la familia cuyo `id` coincide con `WakefieldFamily`, ordenados por ciudad.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

Los resultados son:

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

## <a name="remarks"></a>Comentarios

Los ejemplos anteriores muestran varios aspectos del lenguaje de consulta de Cosmos DB:  

* Como la API de SQL trabaja en valores JSON, trata con entidades en forma de árbol en lugar de con filas y columnas. Puede hacer referencia a los nodos de árbol de cualquier profundidad arbitraria, como `Node1.Node2.Node3…..Nodem`, de forma similar a la referencia de dos partes de `<table>.<column>` en SQL ANSI.

* Dado que el lenguaje de consulta funciona con datos sin esquema, el sistema de tipos debe enlazarse dinámicamente. La misma expresión podría generar diversos tipos en distintos elementos. El resultado de una consulta es un valor JSON válido, pero no se garantiza que sea de un esquema fijo.  

* Azure Cosmos DB solo admite elementos JSON estrictos. El sistema de tipos y las expresiones se restringen para tratar únicamente con tipos JSON. Para más información, consulte la [especificación de JSON](https://www.json.org/).  

* Un contenedor de Cosmos es una colección sin esquemas de elementos JSON. Las relaciones tanto dentro de los elementos de un contenedor como entre ellos se capturan de manera implícita por independencia, no por las relaciones entre la clave principal y la clave externa. Esta característica es importante para las combinaciones dentro del elemento descritas más adelante en este artículo.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Cosmos DB](introduction.md)
- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Cláusula SELECT](sql-query-select.md)
