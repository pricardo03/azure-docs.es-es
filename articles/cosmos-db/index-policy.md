---
title: Directivas de indexación de Azure Cosmos DB
description: Comprenda cómo funciona la indexación en Azure Cosmos DB. Obtenga información sobre la configuración y cambio de la directiva de indexación para una indexación automática y un mayor rendimiento.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: 6998db1679e67f8ac4bf7c81ea9373c66a9618ee
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278571"
---
# <a name="index-policy-in-azure-cosmos-db"></a>Directiva de indexación en Azure Cosmos DB

Puede invalidar la directiva de indexación predeterminada en un contenedor de Azure Cosmos al configurar los parámetros siguientes:

* **Include or exclude items and paths from the index** (Incluir o excluir elementos y rutas de acceso del índice): Puede excluir o incluir elementos específicos en el índice, al insertar o reemplazar los elementos dentro de un contenedor. También puede incluir o excluir propiedades/rutas de acceso específicas para indexarlas en contenedores. Las rutas de acceso pueden incluir patrones de caracteres comodín, por ejemplo, *.

* **Configure index types** (Configurar tipos de índice): Además para rutas de acceso indizadas de intervalo, puede agregar otros tipos de índices, como espacial.

* **Configure index modes** (Configurar modos de índice): al usar la directiva de indexación en un contenedor, puede configurar diferentes modos de indexación, como *Coherente* o *Ninguna*.

## <a name="indexing-modes"></a>Modos de indexación

Azure Cosmos DB admite dos modos de indexación que se pueden configurar en un contenedor de Azure Cosmos mediante Directiva de indexación:

* **Coherente**: Si se establece la directiva de un contenedor Azure Cosmos *coherente*, las consultas realizadas en un contenedor específico siguen el mismo nivel de coherencia que se especificó para lecturas de punto (por ejemplo, fuerte, obsolescencia limitada, sesión y eventual). 

  El índice se actualiza de forma sincrónica cuando se actualizan los elementos. Por ejemplo, las operaciones de insertar, reemplazar, actualizar y eliminar en un elemento darán como resultado la actualización del índice. La indexación coherente admite consultas coherentes a costa de un impacto en el rendimiento de escritura. La reducción del rendimiento de escritura depende de las "rutas de acceso incluidas en el índice" y "nivel de coherencia". Modo de indexación coherente está diseñado para mantener al día con todas las actualizaciones de índice y para atender consultas de inmediato.

* **Ninguna**: un contenedor marcado con el modo de indexación Ninguna no tiene ningún índice asociado. Se suele usar si la base datos de Azure Cosmos se emplea como almacenamiento de clave-valor y solo se puede acceder a los elementos mediante su propiedad de identificador.

  > [!NOTE]
  > Configurar el modo de indexación como un *ninguno* tiene el efecto de quitar cualquier índice existente. Debe usar esta opción Úsela si los patrones de acceso solo requieren un identificador o la vinculación automática.

Los niveles de coherencia de consulta se mantienen similares a las operaciones de lectura normales. La base de datos de Cosmos Azure devuelve un error si consulta el contenedor que tiene un *ninguno* modo de indexación. Puede ejecutar las consultas como exámenes a través de la configuración explícita **x-ms-documentdb-enable-scan** encabezado en la API de REST o el **EnableScanInQuery** opción de solicitud mediante el SDK. NET. Algunas características de consulta, como ORDER BY, no se admiten con **EnableScanInQuery**, ya que imponen un índice correspondiente.

## <a name="modifying-the-indexing-policy"></a>Modificación de la directiva de indexación

En Azure Cosmos DB, puede actualizar la directiva de indexación de un contenedor en cualquier momento. Un cambio en la directiva de indexación en un contenedor de Azure Cosmos puede dar lugar a un cambio en la forma del índice. Este cambio afecta a las rutas de acceso que se pueden indexar, a su precisión y al modelo de coherencia del propio índice. Un cambio en la directiva de indexación requiere efectivamente una transformación del índice original en uno nuevo.

### <a name="index-transformations"></a>Transformaciones del índice

Todas las transformaciones del índice se realizan en línea. Los elementos indexados por la directiva antigua se transforman eficazmente según la directiva nueva sin que afecte a la disponibilidad de escritura o el rendimiento aprovisionado en el contenedor. La coherencia de leer y escribir las operaciones que se realizan utilizando la API de REST, SDK, o mediante procedimientos almacenan y desencadenadores no se ve afectada durante la transformación de índice.

Cambiar la directiva de indexación es una operación asincrónica y el tiempo para completar la operación depende del número de elementos, el rendimiento aprovisionado y el tamaño de los elementos. Mientras la reindexación está en curso, la consulta no puede devolver todos los resultados coincidentes, si se realizan las consultas a usar el índice que se está modificando y no devuelve los errores de las consultas. Mientras la reindexación está en curso, las consultas son coherentes independientemente de la configuración del modo de indexación. Después de que se complete la transformación del índice, continuará viendo resultados coherentes. Esto se aplica a las consultas realizadas con interfaces como la API de REST, los SDK o los procedimientos almacenados y desencadenadores. Transformación de índice se realiza de forma asincrónica, en segundo plano, en las réplicas mediante el uso de los recursos de reserva que están disponibles para las réplicas específicas.

Todas las transformaciones de índice se realizan in situ. Azure Cosmos DB no mantiene dos copias del índice. Por tanto, no se requiere ni se usa espacio en disco adicional en los contenedores mientras se realiza la transformación del índice.

Al cambiar la directiva de indexación, los cambios se aplican para mover desde el índice antiguo al nuevo índice y se basan principalmente en las configuraciones de modo de indexación. Las configuraciones de modo de indexación desempeñan un rol más importante en comparación con otras propiedades, como las rutas de acceso incluidas y excluidas, los tipos de índice y la precisión.

Si las directivas de indexación antigua y nueva usan indexación **coherente**, la base de datos de Azure Cosmos DB realiza una transformación del índice en línea. No se puede aplicar otro cambio de directiva de indexación con el modo de indexación Coherente mientras la transformación está en curso. Si cambia al modo de indexación Ninguna, el índice se elimina inmediatamente. El cambio a Ninguna resulta útil cuando se quiere cancelar una transformación en curso y empezar de nuevo con una directiva de indexación distinta.

## <a name="modifying-the-indexing-policy---examples"></a>Modificación de la directiva de indexación: ejemplos

Los siguientes son los casos de uso más común cuando desea actualizar una directiva de indexación:

* Si desea tener resultados coherentes durante el funcionamiento normal, pero se retrocede a la **ninguno** modo de indexación durante importaciones de datos.

* Si quiere empezar a usar características de indexación en sus contenedores actuales de Azure Cosmos. Por ejemplo, puede usar consultas geoespaciales, que requieren el tipo de índice espacial, o bien consultas por ORDER BY o por rango de cadena, que requieren el tipo de índice por rango de cadena.

* Si quiere seleccionar manualmente las propiedades para indexarlas y cambiarlas con el tiempo para adaptarse a las cargas de trabajo.

* Si quiere optimizar la precisión de indexación para mejorar el rendimiento de las consultas o reducir el almacenamiento usado.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

* [Introducción a la indización](index-overview.md)
* [Tipos de índice](index-types.md)
* [Rutas de acceso del índice](index-paths.md)
* [Cómo administrar la directiva de indexación](how-to-manage-indexing-policy.md)
