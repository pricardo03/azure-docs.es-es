---
title: Tipos de índice en Azure Cosmos DB
description: Información general sobre tipos de índice en Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/5/2018
ms.author: rimman
ms.openlocfilehash: f3c5d7bc1907e94ff2e590fe77cc531ac4b01f4c
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628775"
---
# <a name="index-types-in-azure-cosmos-db"></a>Tipos de índice en Azure Cosmos DB

Hay varias opciones para configurar la directiva de indexación de una ruta de acceso. Puede especificar una o más definiciones de indexación para cada ruta de acceso:

- **Tipo de datos**: Cadena, Número, Punto, Polígono o LineString (solo puede contener una entrada por tipo de datos y ruta de acceso).

- **Variante de índice**: Hash (consultas de igualdad), Rango (consultas de igualdad, por rango o por ORDER BY) o Espacial (consultas espaciales).

- **Precisión**: para el índice de hash, varía de 1 a 8, tanto para cadenas como para números y el valor predeterminado es 3. Para un índice de Rango, el valor de precisión máximo es -1. Puede variar entre 1 y 100 (precisión máxima) para valores numéricos o de cadena.

## <a name="index-kind"></a>Tipo de índice

Azure Cosmos DB admite los tipos de índice Hash y Rango para cada ruta de acceso que pueda configurar para los tipos de dato Cadena, Número o ambos.

- **Índice de hash** admite consultas de igualdad y JOIN eficientes. Para la mayoría de los casos de uso, los índices hash no requieren una precisión mayor que el valor predeterminado de 3 bytes. El tipo de datos puede ser Cadena o Número.

- **Índice de rango** admite consultas de igualdad, consultas de intervalo (con >, <, >=, <=, !=) y consultas ORDER BY eficientes. De forma predeterminada, las consultas por ORDER BY también requieren una precisión de índice máxima (-1). El tipo de datos puede ser Cadena o Número.

- **Índice espacial** admite consultas espaciales eficaces (internas y a distancia). El tipo de datos puede ser Punto, Polígono o LineString. Azure Cosmos DB también admite la clase de índice espacial para cada ruta de acceso, que se puede especificar para el tipo de datos Punto, Polígono o LineString. El valor en la ruta especificada debe ser un fragmento de GeoJSON válido como {"type": "Point", "coordinates": [0.0, 10.0]}. Azure Cosmos DB admite la indexación automática de los tipos de datos Punto, Polígono y LineString.

Estos son ejemplos de consultas que los índices de hash, de rango y espaciales pueden usar:

| **Tipo de índice** | **Descripción/caso de uso** |
| ---------- | ---------------- |
| Hash  | Hash over /prop/? (o /) puede utilizarse para servir de forma eficaz las siguientes consultas:<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>Hash over /props/[]/? (o / o /props/) puede utilizarse para servir de forma eficaz las siguientes consultas:<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag = 5  |
| Intervalo  | Range over /prop/? (o /) puede utilizarse para servir de forma eficaz las siguientes consultas:<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop > 5<br><br>SELECT FROM collection c ORDER BY c.prop   |
| Espacial     | Range over /prop/? (o /) puede utilizarse para servir de forma eficaz las siguientes consultas:<br><br>SELECT FROM collection c<br><br>WHERE ST_DISTANCE(c.prop, {"type": "Point", "coordinates": [0.0, 10.0]}) < 40<br><br>SELECT FROM collection c WHERE ST_WITHIN(c.prop, {"type": "Polygon", ... }) --with indexing on points enabled<br><br>SELECT FROM collection c WHERE ST_WITHIN({"type": "Point", ... }, c.prop) --with indexing on polygons enabled.     |

## <a name="default-behavior-of-index-kinds"></a>Comportamiento predeterminado de los tipos de índice

- De forma predeterminada, se devuelve un error para las consultas con operadores de rango como >= si no hay ningún índice de rango de ninguna precisión para indicar que podría ser necesario realizar un examen para atender la consulta.

- Las consultas por rango se pueden realizar sin un índice de rango mediante el encabezado "x-ms-documentdb-enable-scan" en la API de REST o con la opción de solicitud "EnableScanInQuery" mediante el SDK de .NET. Si hay otros filtros en la consulta en los que Azure Cosmos DB puede usar el índice para realizar el filtrado, no se devuelve ningún error.

- De forma predeterminada, se devuelve un error para las consultas espaciales si no hay ningún índice espacial y no hay ningún otro filtro que pueda obtenerse del índice. Tales consultas se pueden realizar como un examen mediante x-ms-documentdb-enable-scan o EnableScanInQuery.

## <a name="index-precision"></a>Índice de precisión

- La precisión de índice se puede usar para lograr el equilibrio entre la sobrecarga de almacenamiento de índices y el rendimiento de las consultas. Para los números, se recomienda usar la configuración de precisión predeterminada de -1 (máxima). Puesto que los números son 8 bytes en JSON, equivale a una configuración de 8 bytes. La selección de un valor inferior para la precisión (p. ej., de 1 a 7) significa que los valores de algunos rangos se asignan a la misma entrada de índice. Por lo tanto, se puede reducir el espacio de almacenamiento del índice, pero la ejecución de la consulta podría tener que procesar más elementos. Por lo tanto, consume más rendimiento y unidades de solicitud.

- La precisión del índice tiene una aplicación más práctica con intervalos de cadena. Dado que las cadenas pueden ser de cualquier longitud arbitraria, la elección de la precisión del índice puede afectar al rendimiento de las consultas de rango de cadena. También puede influir en la cantidad de espacio de almacenamiento del índice necesario. Los índices de rango de cadena se pueden configurar con una precisión de índice comprendida entre 1 y 100, o con -1 (máxima). Si desea realizar consultas por ORDER BY en las propiedades de cadena, debe especificar una precisión de -1 para las rutas de acceso correspondientes.

- Los índices espaciales siempre usan la precisión de índice predeterminada para todos los tipos (Punto, LineString y Polígono). La precisión de índice predeterminada de los índices espaciales no se puede invalidar.

Azure Cosmos DB devuelve un error cuando una consulta usa Order By, pero no tiene un índice de rango en la ruta de acceso consultada con la precisión máxima.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la indexación en Azure Cosmos DB, consulte los siguientes artículos:

- [Información general de la indexación](index-overview.md)
- [Directiva de indexación](indexing-policies.md)
- [Rutas de acceso del índice](index-paths.md)

