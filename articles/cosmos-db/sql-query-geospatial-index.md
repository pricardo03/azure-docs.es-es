---
title: Indexación de datos geoespaciales con Azure Cosmos DB
description: Indexación de datos espaciales con Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: 2cf682a404154b9c1bb94680b3adb673892c1c72
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566477"
---
# <a name="index-geospatial-data-with-azure-cosmos-db"></a>Indexación de datos geoespaciales con Azure Cosmos DB

El motor de base de datos de Azure Cosmos DB se ha diseñado para que sea realmente independiente del esquema y proporcione compatibilidad de primera clase con JSON. El motor de base de datos optimizado para escritura de Azure Cosmos DB también entiende de forma nativa los datos espaciales representados en el estándar GeoJSON.

En pocas palabras, la geometría se proyecta a partir de coordenadas geodésicas en un plano 2D; después, se divide progresivamente en celdas con un **quadtree**. Estas celdas se asignan a 1D según la ubicación de la celda dentro de una **curva de Hilbert**, que conserva la localidad de los puntos. Además, al indexar los datos de ubicación, estos pasan por un proceso conocido como **teselación**; es decir, todas las celdas que se cruzan en una ubicación se identifican y se almacenan como claves en el índice de Azure Cosmos DB. En el momento de la consulta, los argumentos como puntos y elementos Polygon también se teselan para extraer los intervalos de Id. de celda pertinentes, y luego se usan para recuperar los datos del índice.

Si especifica una directiva de indexación que incluye el índice espacial de /* (todas las rutas de acceso), entonces todos los datos encontrados en el contenedor se indexan para que las consultas espaciales resulten eficaces.

> [!NOTE]
> Azure Cosmos DB admite la indexación de Points, LineStrings, Polygons y MultiPolygons.
>
>

## <a name="geography-data-indexing-examples"></a>Ejemplos de indexación de datos de geografía

El siguiente fragmento JSON muestra una directiva de indexación con la indexación espacial habilitada para el tipo de datos **geography**. Es válido para datos espaciales con el tipo de datos de geografía e indexará cualquier elemento GeoJSON Point, Polygon, MultiPolygon o LineString encontrado en los documentos para las consultas espaciales. Si va a modificar la directiva de indexación mediante Azure Portal, puede especificar el siguiente elemento JSON para que la directiva de indexación permita la indexación espacial en el contenedor:

**JSON de directiva de indexación de contenedores con indexación espacial de geografía**

```json
    {
       "automatic":true,
       "indexingMode":"Consistent",
        "includedPaths": [
        {
            "path": "/*"
        }
        ],
        "spatialIndexes": [
        {
            "path": "/*",
            "types": [
                "Point",
                "Polygon",
                "MultiPolygon",
                "LineString"
            ]
        }
    ],
       "excludedPaths":[]
    }
```

> [!NOTE]
> Si el valor GeoJSON de la ubicación que se encuentra en el documento es incorrecto o no válido, no se indexará para realizar consultas espaciales. Puede validar los valores de ubicación mediante ST_ISVALID y ST_ISVALIDDETAILED.
>
>
>

También puede [modificar la directiva de indexación](how-to-manage-indexing-policy.md) mediante la CLI de Azure, PowerShell o cualquier SDK.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya sabe cómo empezar a trabajar con la compatibilidad geoespacial en Azure Cosmos DB, puede hacer lo siguiente:

* Obtener más información sobre [Consultar Azure Cosmos DB](sql-query-getting-started.md)
* Más información sobre la [consulta de datos espaciales con Azure Cosmos DB](sql-query-geospatial-query.md)
* Más información sobre los [datos de ubicación geoespaciales y GeoJSON en Azure Cosmos DB](sql-query-geospatial-intro.md)