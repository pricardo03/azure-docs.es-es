---
title: Directivas de indexación de Azure Cosmos DB
description: Obtenga información sobre la configuración y cambio de la directiva de indexación predeterminada para una indexación automática y un mayor rendimiento en Azure Cosmos DB.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: thweiss
ms.openlocfilehash: 01e3e1f1c9bffee0604de1260e8e466f5b1d229d
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467870"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Directivas de indexación en Azure Cosmos DB

En Azure Cosmos DB, cada contenedor tiene una directiva de indexación que determina cómo se deben indexar los elementos del contenedor. La directiva de indexación predeterminada para los contenedores recién creados indexa todas las propiedades de todos los contenedores, lo que exige que se usen índices de rango para todas las cadenas o números, e índices espaciales para todos los objetos GeoJSON del tipo Point. Esto permite obtener un rendimiento elevado de consultas sin tener que pensar en la indexación y administración de índices por adelantado.

En algunas situaciones, puede que quiera invalidar este comportamiento automático para ajustarse mejor a sus requerimientos. Puede personalizar la directiva de indexación de un contenedor estableciendo su *modo de indexación* e incluir o excluir las *rutas de acceso de propiedad*.

> [!NOTE]
> El método de actualización de las directivas de indexación que se describe en este artículo solo se aplica a la API de Azure Cosmos DB SQL (Core).

## <a name="indexing-mode"></a>Modo de indexación

Azure Cosmos DB admite dos modos de indexación:

- **Coherente**: si una directiva de indexación de un contenedor se establece en coherente, el índice se actualiza de forma sincrónica a medida que se crean, actualizan o eliminan elementos. Esto significa que la coherencia de las consultas de lectura será la [coherencia configurada para la cuenta](consistency-levels.md).

- **Ninguna**: si una directiva de indexación de un contenedor se establece en ninguna, la indexación está deshabilitada de forma efectiva en ese contenedor. Esto se utiliza normalmente cuando se usa un contenedor como un almacén de pares clave-valor puro sin necesidad de índices secundarios. También puede ayudar a acelerar las operaciones masivas de inserción.

## <a name="including-and-excluding-property-paths"></a>Inclusión y exclusión de rutas de acceso de propiedad

Una directiva de indexación personalizada puede especificar rutas de acceso de propiedad que se incluyen o excluyen de forma explícita de la indexación. Al optimizar el número de rutas de acceso que se indexan, puede reducir la cantidad de almacenamiento que utiliza el contenedor y mejorar la latencia de las operaciones de escritura. Estas rutas de acceso se definen siguiendo [el método descrito en la sección de introducción a la indexación](index-overview.md#from-trees-to-property-paths), con las siguientes adiciones:

- Una ruta de acceso que lleva a un valor escalar (cadena o número) termina con `/?`.
- Los elementos de una matriz se dirigen juntos a través de la notación `/[]` (en lugar de `/0`, `/1` etcétera).
- El carácter comodín `/*` puede utilizarse para que coincida con cualquier elemento debajo del nodo.

Tomando el mismo ejemplo de nuevo:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 }
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

- La ruta de acceso `employees` de `headquarters` es `/headquarters/employees/?`.
- La ruta de acceso `country` de `locations` es `/locations/[]/country/?`.
- La ruta de acceso de todo lo que incluye `headquarters` es `/headquarters/*`.

Cuando una ruta de acceso se incluye explícitamente en la directiva de indexación, también debe definir qué tipos de índice se deben aplicar a esa ruta de acceso y, para cada tipo de índice, el tipo de datos al que se aplica este índice:

| Tipo de índice | Tipos de datos de destino permitidos |
| --- | --- |
| Intervalo | Cadena o número |
| Espacial | Point, LineString o Polygon |

Por ejemplo, podríamos incluir la ruta de acceso `/headquarters/employees/?` y especificar que se debe aplicar un índice `Range` en esa ruta de acceso para los valores `String` y `Number`.

### <a name="includeexclude-strategy"></a>Inclusión o exclusión de estrategia

Cualquier directiva de indexación tiene que incluir la ruta de acceso raíz `/*` así como una ruta de acceso incluida o una excluida.

- Incluya la ruta de acceso raíz para excluir selectivamente las rutas de acceso que no se deban indexar. Este es el enfoque recomendado, ya que permite a Azure Cosmos DB indexar de forma proactiva las propiedades nuevas que se pueden agregar a su modelo.
- Excluya la ruta de acceso raíz para incluir selectivamente las rutas de acceso que se deban indexar.

- Para las rutas de acceso con caracteres normales que incluyen: caracteres alfanuméricos y _ (guion bajo), no tiene que aplicar el escape a la cadena de ruta de acceso en comillas dobles (por ejemplo, "/path/?"). Para las rutas de acceso con otros caracteres especiales, necesitará aplicar el escape a la cadena de ruta de acceso en comillas dobles (por ejemplo, "/\"path-abc\"/?"). Si se esperan caracteres especiales en la ruta de acceso, puede aplicar el escape a las rutas de acceso por motivos de seguridad. Funcionalmente, no hay ninguna diferencia si aplica el escape a todas las rutas de acceso frente a aplicarlo solo a las que tienen caracteres especiales.

- De forma predeterminada, la propiedad del sistema "ETag" se excluye de la indexación, a menos que la ETag se agregue a la ruta de acceso incluida para la indexación.

Vea [en esta sección](how-to-manage-indexing-policy.md#indexing-policy-examples) ejemplos de directivas de indexación.

## <a name="composite-indexes"></a>Índices compuestos

Las consultas que aplican la cláusula `ORDER BY` en dos o más propiedades requieren un índice compuesto. Actualmente, los índices compuestos solo las utilizan las consultas de tipo Multi `ORDER BY`. De forma predeterminada, no hay índices compuestos definidos, por lo que debe [agregar índices compuestos](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) según sea necesario.

Al definir un índice compuesto, especifique lo siguiente:

- Dos o más rutas de acceso de propiedad. La secuencia en la que se definen las rutas de acceso de propiedad importa.
- El orden (ascendente o descendente).

Al usar índices compuestos, se usan las siguientes consideraciones:

- Si las rutas de acceso del índice compuesto no coinciden con la secuencia de las propiedades en la cláusula ORDER BY, el índice compuesto no admite la consulta.

- El orden de las rutas de acceso del índice compuesto (ascendente o descendente) también debe coincidir con el orden de la cláusula ORDER BY.

- El índice compuesto también admite una cláusula ORDER BY con el orden inverso en todas las rutas de acceso.

Tenga en cuenta el ejemplo siguiente, donde se define un índice compuesto en las propiedades a, b y c:

| **Índice compuesto**     | **Consulta `ORDER BY` de ejemplo**      | **¿Lo admite el índice?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(a asc, b asc)```         | ```ORDER BY  a asc, b asc```        | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  b asc, a asc```        | ```No```             |
| ```(a asc, b asc)```          | ```ORDER BY  a desc, b desc```      | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  a asc, b desc```       | ```No```             |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc, c asc``` | ```Yes```            |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc```        | ```No```            |

Debe personalizar la directiva de indexación para que pueda atender todas las consultas `ORDER BY` necesarias.

## <a name="modifying-the-indexing-policy"></a>Modificación de la directiva de indexación

Se puede actualizar en cualquier momento una directiva de indexación de un contenedor [mediante Azure Portal o uno de los SDK admitidos](how-to-manage-indexing-policy.md). Una actualización de la directiva de indexación desencadena una transformación del índice antiguo al nuevo, que se realiza en línea y en local (por lo que no se consume ningún espacio de almacenamiento adicional durante la operación). El índice de la directiva antigua se transforma eficientemente en la nueva directiva sin que ello afecte a la disponibilidad de escritura ni al rendimiento aprovisionado en el contenedor. La transformación del índice es una operación asincrónica, y el tiempo que tarda en completarse depende del rendimiento aprovisionado, el número de elementos y su tamaño. 

> [!NOTE]
> Mientras la reindexación está en curso, es posible que las consultas no devuelvan todos los resultados coincidentes y la hará sin devolver ningún error. Esto significa que los resultados de consultas no pueden ser coherentes hasta que se complete la transformación del índice. Es posible realizar un seguimiento del progreso de transformación del índice [mediante uno de los SDK](how-to-manage-indexing-policy.md).

Si el modo nuevo de la directiva de indexación se establece en coherente, no se puede aplicar ningún otro cambio de directiva de indexación mientras la transformación del índice esté en curso. Se puede cancelar una transformación del índice en ejecución al establecer el modo de la directiva de indexación en None (lo que inmediatamente anulará el índice).

## <a name="indexing-policies-and-ttl"></a>Directivas de indexación y TTL

La [característica periodo de vida (TTL)](time-to-live.md) requiere que la indexación esté activa en el contenedor que esté activado. Esto significa que:

- No es posible activar el TTL en un contenedor en el que se establece el modo de indexación en None.
- No es posible establecer el modo de indexación en None en un contenedor donde el TTL está activado.

Para escenarios donde no se necesita indexar ninguna ruta de acceso de propiedad, pero el TTL es necesario, puede usar una directiva de indexación con lo siguiente:

- Un modo de indexación establecido en Consistent.
- Ninguna ruta de acceso incluida.
- `/*` como única ruta de acceso excluida.

## <a name="obsolete-attributes"></a>Atributos obsoletos

Cuando se trabaja con las directivas de indexación, puede encontrar los atributos siguientes que ahora están obsoletos:

- `automatic` es un valor booleano que se define en la raíz de una directiva de indexación. Ahora se omite y se puede establecer en `true`, cuando lo requiera la herramienta que se esté usando.
- `precision` es un número que se define en el nivel de índice para rutas de acceso incluidas. Ahora se omite y se puede establecer en `-1`, cuando lo requiera la herramienta que se esté usando.
- `hash` es un tipo de índice que ahora reemplaza el tipo de rango.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

- [Introducción a la indexación](index-overview.md)
- [Cómo administrar la directiva de indexación](how-to-manage-indexing-policy.md)
