---
title: Trabajo con rutas de acceso de índice en Azure Cosmos DB
description: Información general sobre rutas de acceso de índice en Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/5/2018
ms.author: rimman
ms.openlocfilehash: 0515397fb9ab0f05b4c763a2b05e9d986960bd91
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628838"
---
# <a name="index-paths-in-azure-cosmos-db"></a>Rutas de acceso de índice en Azure Cosmos DB

Al usar rutas de acceso de índice en Azure Cosmos DB, puede elegir incluir o excluir una ruta de acceso específica de la indexación. Elegir rutas de acceso específicas ofrece un mejor rendimiento de escritura y un menor almacenamiento de índices en escenarios en los que se conocen los patrones de consulta. Las rutas de acceso de índice comienzan con el operador comodín raíz (`/`) y normalmente terminan con el operador comodín `?`. Este patrón indica que hay varios valores posibles para el prefijo. Por ejemplo, para atender la consulta `SELECT * FROM Families F WHERE F.familyName = "Andersen"`, debe incluir una ruta de acceso de índice para `/familyName/?` en la directiva de índice del contenedor.

Las rutas de acceso del índice también pueden usar el operador comodín `*`para especificar el comportamiento de las rutas de acceso de forma recursiva en el prefijo. Por ejemplo, `/payload/*` puede usarse para excluir de la indexación todo el contenido de la propiedad payload.

## <a name="common-patterns-for-index-paths"></a>Patrones comunes para las rutas de acceso de índice

Estos son los patrones comunes para especificar las rutas de acceso del índice:

| **Path** | **Descripción/caso de uso** |
| ---------- | ------- |
| /   | Ruta de acceso predeterminada de la recopilación. Recursiva. Se aplica a la totalidad del árbol de documentos.|
| /prop/?  | Ruta de acceso de índice necesaria para atender consultas como la siguiente (con tipos hash o de intervalo respectivamente):<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop > 5<br><br>SELECT FROM collection c ORDER BY c.prop  |
| /"prop"/*  | Ruta de acceso del índice para todas las rutas de acceso situadas en la etiqueta especificada. Funciona con las siguientes consultas<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop.subprop > 5<br><br>SELECT FROM collection c WHERE c.prop.subprop.nextprop = "value"<br><br>SELECT FROM collection c ORDER BY c.prop |
| /props/[]/?  | Ruta de acceso de índice necesaria para servir a la iteración y a las consultas JOIN frente a las matrices de valores escalares como ["a", "b", "c"]:<br><br>SELECT tag FROM tag IN collection.props WHERE tag = "value"<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag > 5  |
| /props/[]/subprop/? | Ruta de acceso de índice necesaria para servir a la iteración y a las consultas JOIN frente a las matrices de objetos como [{subprop: "a"}, {subprop: "b"}]:<br><br>SELECT tag FROM tag IN collection.props WHERE tag.subprop = "value"<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag.subprop = "value" |
| /prop/subprop/? | Ruta de acceso de índice necesaria para atender consultas (con tipos hash o de intervalo respectivamente):<br><br>SELECT FROM collection c WHERE c.prop.subprop = "value"<br><br>SELECT FROM collection c WHERE c.prop.subprop > 5  |

Al establecer rutas de acceso de índice personalizadas, es necesario especificar la regla de indexación predeterminada para el elemento completo, que se indica mediante la ruta de acceso especial `/*`.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la indexación en Azure Cosmos DB en los siguientes artículos:

- [Información general de la indexación](index-overview.md)
- [Directivas de indexación en Azure Cosmos DB](indexing-policies.md)
- [Tipos de índice en Azure Cosmos DB](index-types.md)
