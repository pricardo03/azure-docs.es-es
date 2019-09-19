---
title: Establecimiento de alias en Azure Cosmos DB
description: Obtenga información acerca del establecimiento de alias para valores en las consultas SQL de Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: 3b17cbc7710647b1e1875025a1db1849034ec1dc
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002084"
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

- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Cláusula SELECT](sql-query-select.md)
- [Cláusula FROM](sql-query-from.md)
