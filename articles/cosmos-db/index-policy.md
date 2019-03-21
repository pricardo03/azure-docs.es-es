---
title: Directivas de indexación de Azure Cosmos DB
description: Comprenda cómo funciona la indexación en Azure Cosmos DB. Obtenga información sobre la configuración y cambio de la directiva de indexación para una indexación automática y un mayor rendimiento.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 3/1/2019
ms.author: mjbrown
ms.openlocfilehash: 0ba5cdd4f92390634d6d2bea8add8309cb1f4d3e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58014509"
---
# <a name="index-policy-in-azure-cosmos-db"></a>Directiva de indexación en Azure Cosmos DB

Puede invalidar la directiva de indexación predeterminada en un contenedor de Azure Cosmos al configurar los parámetros siguientes:

* **Include or exclude items and paths from the index** (Incluir o excluir elementos y rutas de acceso del índice): puede excluir o incluir elementos específicos en el índice al insertar o reemplazar los elementos dentro de un contenedor. También puede incluir o excluir propiedades/rutas de acceso específicas para indexarlas en contenedores. Las rutas de acceso pueden incluir patrones de caracteres comodín, por ejemplo, *.

* **Configure index types** (Configurar tipos de índice): además de incluir rutas de acceso indexadas, puede agregar otros tipos de índices, como espaciales.

* **Configure index modes** (Configurar modos de índice): al usar la directiva de indexación en un contenedor, puede configurar diferentes modos de indexación, como *Coherente* o *Ninguna*.

## <a name="indexing-modes"></a>Modos de indexación

Azure Cosmos DB admite dos modos de indexación que se pueden configurar en un contenedor de Azure Cosmos. Puede configurar los siguientes dos modos de indexación a través de la directiva de indexación:

* **Coherente**: si la directiva del contenedor de Azure Cosmos se establece en Coherente, las consultas realizadas en un contenedor específico siguen el mismo nivel de coherencia que el especificado para las lecturas de punto (es decir, alta, de uso vinculado, sesión o eventual). 

  El índice se actualiza de forma sincrónica cuando se actualizan los elementos. Por ejemplo, las operaciones de insertar, reemplazar, actualizar y eliminar en un elemento darán como resultado la actualización del índice. La indexación coherente admite consultas coherentes a costa de un impacto en el rendimiento de escritura. La reducción del rendimiento de escritura depende de las "rutas de acceso incluidas en la indexación" y del "nivel de coherencia". El modo de indexación coherente está diseñado para cargas de trabajo de tipo "escribir rápidamente, consultar inmediatamente".

* **Ninguna**: un contenedor marcado con el modo de indexación Ninguna no tiene ningún índice asociado. Se suele usar si la base datos de Azure Cosmos se emplea como almacenamiento de clave-valor y solo se puede acceder a los elementos mediante su propiedad de identificador.

  > [!NOTE]
  > La configuración del modo de indexación como Ninguna tiene el efecto secundario de quitar todo índice existente. Debe usar esta opción Úsela si los patrones de acceso solo requieren un identificador o la vinculación automática.

Los niveles de coherencia de consulta se mantienen similares a las operaciones de lectura normales. La base de datos de Azure Cosmos devuelve un error si consulta el contenedor que tiene un modo de indexación Ninguna. Puede ejecutar las consultas como exámenes del encabezado **x-ms-documentdb-enable-scan** explícito en la API de REST o con la opción de solicitud  **EnableScanInQuery** mediante el SDK de .NET. Algunas características de consulta, como ORDER BY, no se admiten con **EnableScanInQuery**, ya que imponen un índice correspondiente.

## <a name="modifying-the-indexing-policy"></a>Modificación de la directiva de indexación

En Azure Cosmos DB, puede actualizar la directiva de indexación de un contenedor en cualquier momento. Un cambio en la directiva de indexación de un contenedor de Azure Cosmos puede conducir a un cambio en la forma del índice. Este cambio afecta a las rutas de acceso que se pueden indexar, a su precisión y al modelo de coherencia del propio índice. Un cambio en la directiva de indexación requiere efectivamente una transformación del índice original en uno nuevo.

### <a name="index-transformations"></a>Transformaciones del índice

Todas las transformaciones del índice se realizan en línea. Los elementos indexados por la directiva antigua se transforman eficazmente según la nueva directiva sin que ello afecte a la disponibilidad de escritura ni al rendimiento aprovisionado del contenedor. La coherencia de las operaciones de lectura y escritura realizadas con la API de REST, los SDK o desde procedimientos almacenados y desencadenadores no se ve afectada durante la transformación del índice.

El cambio DE la directiva de indexación es una operación asincrónica, y el tiempo para completar la operación depende del número de elementos, el rendimiento aprovisionado y tamaño de los elementos. Mientras la reindexación está en curso, la consulta puede no devolver todos los resultados coincidentes si la consulta utiliza el índice que se está modificando, y las consultas coincidentes no devolverán ningún error. Mientras la reindexación está en curso, las consultas son coherentes finalmente, con independencia de la configuración del modo de indexación. Después de que se complete la transformación del índice, continuará viendo resultados coherentes. Esto se aplica a las consultas realizadas con interfaces como la API de REST, los SDK o los procedimientos almacenados y desencadenadores. La transformación de índice se realiza asincrónicamente en segundo plano en las réplicas mediante los recursos de reserva disponibles para una réplica determinada.

Todas las transformaciones de índice se realizan in situ. Azure Cosmos DB no mantiene dos copias del índice. Por tanto, no se requiere ni se usa espacio en disco adicional en los contenedores mientras se realiza la transformación del índice.

Cuando se cambia la directiva de indexación, se aplican los cambios para pasar del índice antiguo al nuevo en base, principalmente, a las configuraciones del modo de indexación. Las configuraciones de modo de indexación desempeñan un rol más importante en comparación con otras propiedades, como las rutas de acceso incluidas y excluidas, los tipos de índice y la precisión.

Si las directivas de indexación antigua y nueva usan indexación **coherente**, la base de datos de Azure Cosmos DB realiza una transformación del índice en línea. No se puede aplicar otro cambio de directiva de indexación con el modo de indexación Coherente mientras la transformación está en curso. Si cambia al modo de indexación Ninguna, el índice se elimina inmediatamente. El cambio a Ninguna resulta útil cuando se quiere cancelar una transformación en curso y empezar de nuevo con una directiva de indexación distinta.

Para que la transformación del índice se complete correctamente, asegúrese de que el contenedor tenga suficiente espacio de almacenamiento. Si el contenedor alcanza su cuota de almacenamiento, se pausa la transformación del índice. La transformación del índice se reanuda automáticamente cuando hay espacio de almacenamiento disponible, por ejemplo, cuando elimina algunos elementos.

## <a name="modifying-the-indexing-policy---examples"></a>Modificación de la directiva de indexación: ejemplos

Estos son los casos de uso más comunes donde se actualiza una directiva de indexación:

* Si quiere obtener resultados coherentes durante el funcionamiento normal, pero revertir al modo de indexación **Ninguna** durante las importaciones de conjuntos masivos de datos.

* Si quiere empezar a usar características de indexación en sus contenedores actuales de Azure Cosmos. Por ejemplo, puede usar consultas geoespaciales, que requieren el tipo de índice espacial, o bien consultas por ORDER BY o por rango de cadena, que requieren el tipo de índice por rango de cadena.

* Si quiere seleccionar manualmente las propiedades para indexarlas y cambiarlas con el tiempo para adaptarse a las cargas de trabajo.

* Si quiere optimizar la precisión de indexación para mejorar el rendimiento de las consultas o reducir el almacenamiento usado.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

* [Introducción a la indexación](index-overview.md)
* [Tipos del índice](index-types.md)
* [Rutas de acceso del índice](index-paths.md)
* [Cómo administrar la directiva de indexación](how-to-manage-indexing-policy.md)
