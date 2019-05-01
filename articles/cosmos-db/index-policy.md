---
title: Directivas de indexación de Azure Cosmos DB
description: Aprenda a configurar y cambiar el valor predeterminado para la indexación automática y un mayor rendimiento en Azure Cosmos DB la directiva de indexación.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: thweiss
ms.openlocfilehash: 67bc3076be91ade140b39b7dd8037299902546a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61046325"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Directivas de indexación en Azure Cosmos DB

En Azure Cosmos DB, cada contenedor tiene una directiva de indexación que determina cómo se deben indizar los elementos del contenedor. El valor predeterminado la directiva para la indización recién había creado los índices de los contenedores todas las propiedades de cada elemento, aplicar los índices de intervalo para cualquier cadena o un número, y los índices espaciales para cualquier objeto de GeoJSON de tipo Point. Esto le permite obtener un rendimiento elevado de consultas sin tener que pensar en la indización y la administración de índices por adelantado.

En algunas situaciones, puede invalidar este comportamiento automático para satisfacer mejor sus necesidades. Puede personalizar la directiva de indexación de un contenedor estableciendo su *modo de indexación*e incluir o excluir *las rutas de acceso de propiedad*.

## <a name="indexing-mode"></a>Modo de indexación

Azure Cosmos DB admite dos modos de indexación:

- **Coherente**: Si la directiva de indexación de un contenedor está establecida en coherente, el índice se actualiza de forma sincrónica como crear, actualizar o eliminar elementos. Esto significa que la coherencia de las consultas de lectura será el [coherencia configurado para la cuenta](consistency-levels.md).

- **Ninguna**: Si la directiva de indexación de un contenedor se establece en None, indexación está deshabilitada eficazmente en ese contenedor. Esto se utiliza normalmente cuando se usa un contenedor como un almacén de pares clave-valor puro sin necesidad de índices secundarios. Puede también ayudar a acelerar de forma masiva las operaciones de inserción.

## <a name="including-and-excluding-property-paths"></a>Incluir y excluir rutas de acceso de propiedad

Una directiva de indexación personalizada puede especificar rutas de acceso de propiedad que se incluyen o excluyen de la indexación de forma explícita. Al optimizar el número de rutas de acceso que se indizan, puede reducir la cantidad de almacenamiento utilizado por el contenedor y mejorar la latencia de operaciones de escritura. Estas rutas de acceso se definen siguiendo [el método descrito en la sección de información general sobre indización](index-overview.md#from-trees-to-property-paths) con las siguientes adiciones:

- ruta a un valor escalar (cadena o número) termina con `/?`
- los elementos de una matriz se asignan entre sí a través del `/[]` notación (en lugar de `/0`, `/1` etcetera.)
- el `/*` comodín puede utilizarse para que coincida con todos los elementos debajo del nodo

Teniendo el mismo ejemplo nuevo:

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

- el `headquarters`del `employees` es la ruta de acceso `/headquarters/employees/?`
- el `locations`' `country` es la ruta de acceso `/locations/[]/country/?`
- la ruta de acceso a cualquier cosa en `headquarters` es `/headquarters/*`

Cuando una ruta de acceso se incluye explícitamente en la directiva de indexación, también debe definir qué tipos de índice se deben aplicar a esa ruta de acceso y para cada tipo de índice, el tipo de datos que se aplica este índice:

| Tipo de índice | Tipos de datos de destino permitidos |
| --- | --- |
| Intervalo | Cadena o un número |
| Espacial | Point, LineString o polígono |

Por ejemplo, podríamos incluimos la `/headquarters/employees/?` ruta de acceso y especificar que un `Range` índice se debe aplicar en esa ruta de acceso para ambos `String` y `Number` valores.

### <a name="includeexclude-strategy"></a>Incluir o excluir estrategia

Cualquier directiva de indexación tiene que incluir la ruta de acceso raíz `/*` como un incluye o una ruta excluida.

- Incluir la ruta de acceso raíz para excluir selectivamente las rutas de acceso que no es necesario indexar. Este es el enfoque recomendado, ya que permite Azure Cosmos DB de forma proactiva cualquier propiedad nueva que se puede agregar a su modelo de índice.
- Excluir la ruta de acceso raíz para incluir las rutas de acceso que se deben indexar de manera selectiva.

Consulte [en esta sección](how-to-manage-indexing-policy.md#indexing-policy-examples) para ejemplos de directivas de indexación.

## <a name="modifying-the-indexing-policy"></a>Modificación de la directiva de indexación

Directiva de indexación de un contenedor se puede actualizar en cualquier momento [mediante el portal de Azure o uno de los SDK admitidos](how-to-manage-indexing-policy.md). Una actualización de la directiva de indexación desencadena una transformación del índice antiguo al nuevo, que se realiza en línea y en su lugar (por lo que no se consume ningún espacio de almacenamiento adicional durante la operación). Índice de la directiva antigua se transforma eficazmente a la nueva directiva sin que afecte a la disponibilidad de escritura o el rendimiento aprovisionado en el contenedor. Transformación de índice es una operación asincrónica, y el tiempo que tarda en completarse depende del rendimiento aprovisionado, el número de elementos y su tamaño. 

> [!NOTE]
> Mientras la reindexación está en curso, las consultas no pueden devolver todos los resultados coincidentes y lo hará sin devolver los errores. Esto significa que los resultados de consulta no pueden ser coherentes hasta que se complete la transformación del índice. Es posible hacer un seguimiento del progreso de transformación de índice [mediante uno de los SDK](how-to-manage-indexing-policy.md).

Si el modo de la directiva de indexación nuevo está establecido en coherente, no se puede aplicar ningún otro cambio de directiva de indexación mientras la transformación de índice está en curso. Una transformación de índice de ejecución puede cancelarse estableciendo el modo de la directiva de indexación en None (que inmediatamente se eliminará el índice).

## <a name="indexing-policies-and-ttl"></a>Las directivas de indexación y TTL

El [característica período de vida (TTL)](time-to-live.md) requiere la indexación para que esté activo en el contenedor está activado. Esto significa que:

- no es posible activar el TTL en un contenedor donde se establece el modo de indexación en None,
- no es posible establecer el modo de indexación en None en un contenedor donde se activa el TTL.

Para escenarios donde no necesita ninguna ruta de acceso de propiedad indizar, pero el TTL es necesario, puede usar una directiva de indexación con:

- un modo de indexación establecido en coherente, y
- ninguna ruta de acceso incluida, y
- `/*` como la única ruta excluida.

## <a name="obsolete-attributes"></a>Atributos obsoletos

Cuando se trabaja con las directivas de indexación, puede encontrar los siguientes atributos que ahora están obsoletos:

- `automatic` es un valor booleano que se definen en la raíz de una directiva de indexación. Ahora se omite y se puede establecer en `true`, cuando lo requiera la herramienta que está utilizando.
- `precision` es un número que se definen en el nivel de índice para rutas de acceso incluidas. Ahora se omite y se puede establecer en `-1`, cuando lo requiera la herramienta que está utilizando.
- `hash` es un tipo de índice que ahora se reemplaza por el tipo de intervalo.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

- [Introducción a la indexación](index-overview.md)
- [Cómo administrar la directiva de indexación](how-to-manage-indexing-policy.md)
