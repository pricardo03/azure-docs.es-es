---
title: Consultas parametrizadas en Azure Cosmos DB
description: Obtenga información acerca de las consultas SQL parametrizadas
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: 2bfc22346c1dd43d7d3c2937ffc286e48ae774d0
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343120"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Consultas parametrizadas en Azure Cosmos DB

Cosmos DB admite consultas con parámetros que se expresen con la notación @ ya conocida. El uso de SQL con parámetros permite controlar y evitar de forma sólida la entrada por parte de los usuarios, y evita la exposición accidental de datos a través de la inyección de código SQL.

## <a name="examples"></a>Ejemplos

Por ejemplo, puede escribir una consulta que tome `lastName` y `address.state` como parámetros, y ejecutarla para distintos valores de `lastName` y `address.state` basándose en la entrada del usuario.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Luego puede enviar esta solicitud a Cosmos DB como consulta JSON parametrizada, como la siguiente:

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

En el ejemplo siguiente se establece el argumento TOP con una consulta parametrizada: 

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Los valores de los parámetros pueden ser cualquier tipo de JSON válido: cadenas, números, booleanos, null o incluso matrices o JSON anidado. Dado que Cosmos DB no tiene ningún esquema, los parámetros no se validan respecto a ningún tipo.


## <a name="next-steps"></a>Pasos siguientes

- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Modelado de datos de documentos](modeling-data.md)
