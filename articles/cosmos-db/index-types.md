---
title: Tipos de índice en Azure Cosmos DB
description: Información general sobre tipos de índice en Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 3/13/2019
ms.author: mjbrown
ms.openlocfilehash: 56c0fcb24ac5d255c6a36bcffd327df76f459963
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57990552"
---
# <a name="index-types-in-azure-cosmos-db"></a>Tipos de índice en Azure Cosmos DB

Hay varias opciones para configurar la directiva de indexación de una ruta de acceso. Puede especificar una o más definiciones de indexación para cada ruta de acceso:

- **Tipo de datos:** String, Number, Point, Polygon o LineString (solo puede contener una entrada por tipo de datos y ruta de acceso).

- **Tipo de índice:** Intervalo (igualdad, intervalo o consultas por ORDER BY) o espacial (consultas espaciales).

- **Precisión:** Para un índice de intervalo, el valor de precisión máxima es de -1, que también es el valor predeterminado.

## <a name="index-kind"></a>Tipo de índice

Azure Cosmos DB admite el índice de intervalo para cada ruta de acceso que se puede configurar para los tipos de datos de cadena o un número o ambos.

- **Índice de intervalo** admite consultas de igualdad eficaz, las consultas de combinación, las consultas por rango (con >, <>, =, < =,! =) y consultas por ORDER BY. De forma predeterminada, las consultas por ORDER BY también requieren una precisión de índice máxima (-1). El tipo de datos puede ser Cadena o Número.

- **Índice espacial** admite consultas espaciales eficaces (internas y a distancia). El tipo de datos puede ser Punto, Polígono o LineString. Azure Cosmos DB también admite la clase de índice espacial para cada ruta de acceso, que se puede especificar para el tipo de datos Punto, Polígono o LineString. El valor en la ruta especificada debe ser un fragmento de GeoJSON válido como {"type": "Point", "coordinates": [0.0, 10.0]}. Azure Cosmos DB admite la indexación automática de los tipos de datos Punto, Polígono y LineString.

Estos son ejemplos de consultas de intervalo y los índices espaciales se pueden usar para atender:

| **Tipo de índice** | **Descripción/caso de uso** |
| ---------- | ---------------- |
| Intervalo      | Range over /prop/? (o /) puede utilizarse para servir de forma eficaz las siguientes consultas:<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop > 5<br><br>SELECT FROM collection c ORDER BY c.prop<br><br>¿Intervalo de/props / [] /? (o / o /props/) puede utilizarse para servir de forma eficaz las siguientes consultas:<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag = 5  |
| Espacial    | Range over /prop/? (o /) puede utilizarse para servir de forma eficaz las siguientes consultas:<br><br>SELECT FROM colección c donde ST_DISTANCE(c.prop, {"type": "Point", "coordinates": [0.0, 10.0]}) < 40<br><br>SELECT FROM collection c WHERE ST_WITHIN(c.prop, {"type": "Point", ... }) --con indexación en los puntos habilitados.<br><br>SELECT FROM collection c WHERE ST_WITHIN({"type": "Polygon", ... }, c.prop) --con indexación en los polígonos habilitados. |

## <a name="default-behavior-of-index-kinds"></a>Comportamiento predeterminado de los tipos de índice

- De forma predeterminada, se devuelve un error para las consultas con operadores de rango como >= si no hay ningún índice de rango de ninguna precisión para indicar que podría ser necesario realizar un examen para atender la consulta.

- Las consultas por rango se pueden realizar sin un índice de rango mediante el encabezado "x-ms-documentdb-enable-scan" en la API de REST o con la opción de solicitud "EnableScanInQuery" mediante el SDK de .NET. Si hay otros filtros en la consulta en los que Azure Cosmos DB puede usar el índice para realizar el filtrado, no se devuelve ningún error.

- De forma predeterminada, se devuelve un error para las consultas espaciales si no hay ningún índice espacial y no hay ningún otro filtro que pueda obtenerse del índice. Tales consultas se pueden realizar como un examen mediante x-ms-documentdb-enable-scan o EnableScanInQuery.

## <a name="index-precision"></a>Índice de precisión

> [!NOTE]
> Los contenedores de Cosmos Azure admiten un nuevo diseño de índice que ya no requiere una precisión de índice personalizada que no sea el valor de precisión máxima (-1). Con este método, las rutas de acceso se indexan siempre con la precisión máxima. Si especifica un valor de precisión en la directiva de indexación, las solicitudes CRUD en un contenedor omitirán silenciosamente el valor de precisión y la respuesta del contenedor solo contiene el valor de precisión máximo (-1).  Todos los nuevos contenedores de Cosmos usan el nuevo diseño del índice de forma predeterminada.

- La precisión de índice se puede usar para lograr el equilibrio entre la sobrecarga de almacenamiento de índices y el rendimiento de las consultas. Para los números, se recomienda usar la configuración de precisión predeterminada de -1 (máxima). Puesto que los números son 8 bytes en JSON, equivale a una configuración de 8 bytes. La selección de un valor inferior para la precisión (p. ej., de 1 a 7) significa que los valores de algunos rangos se asignan a la misma entrada de índice. Por lo tanto, se puede reducir el espacio de almacenamiento del índice, pero la ejecución de la consulta podría tener que procesar más elementos. Por lo tanto, consume más rendimiento y unidades de solicitud.

- La precisión del índice tiene una aplicación más práctica con intervalos de cadena. Dado que las cadenas pueden ser de cualquier longitud arbitraria, la elección de la precisión del índice puede afectar al rendimiento de las consultas de rango de cadena. También puede influir en la cantidad de espacio de almacenamiento del índice necesario. Los índices de rango de cadena se pueden configurar con una precisión de índice comprendida entre 1 y 100, o con -1 (máxima). Si desea realizar consultas por ORDER BY en las propiedades de cadena, debe especificar una precisión de -1 para las rutas de acceso correspondientes.

- Los índices espaciales siempre usan la precisión de índice predeterminada para todos los tipos (Punto, LineString y Polígono). La precisión de índice predeterminada de los índices espaciales no se puede invalidar.

Azure Cosmos DB devuelve un error cuando una consulta usa Order By, pero no tiene un índice de rango en la ruta de acceso consultada con la precisión máxima.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la indexación en Azure Cosmos DB, consulte los siguientes artículos:

- [Información general de la indexación](index-overview.md)
- [Directiva de indexación](indexing-policies.md)
- [Rutas de acceso del índice](index-paths.md)

