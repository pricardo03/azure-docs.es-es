---
title: Establecimiento de alias en Azure Cosmos DB
description: Obtenga información acerca del establecimiento de alias para valores en las consultas SQL de Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: e532fb7180af8a21de6ae9a2e4d798abd9e93e7b
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343130"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Establecimiento de alias en Azure Cosmos DB

Puede establecer alias para valores de manera explícita en las consultas. Si una consulta tiene dos propiedades con el mismo nombre, establezca alias para cambiar el nombre de una o ambas propiedades de modo que se elimine su ambigüedad en el resultado proyectado.

## <a name="examples"></a>Ejemplos

La palabra clave AS usada para el alias es opcional, tal como se muestra en el ejemplo siguiente al proyectar el segundo valor como `NameInfo`:

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Los resultados son:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>Pasos siguientes

- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Cláusula SELECT](sql-query-select.md)
- [Cláusula FROM](sql-query-from.md)
