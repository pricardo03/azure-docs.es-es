---
title: Tipos de resolución de conflictos y directivas de resolución con varias regiones de escritura de Azure Cosmos DB
description: En este artículo se describen las categorías de conflicto y las directivas de resolución de conflictos de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/24/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ebea55f769ca16bfa344d0a100fe16cec6d784d0
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684232"
---
# <a name="conflict-types-and-resolution-policies"></a>Tipos de conflicto y directivas de resolución de conflictos

Los conflictos y las directivas de resolución de conflictos son aplicables si la cuenta de Azure Cosmos DB está configurada con varias regiones de escritura.

Para las cuentas de Azure Cosmos configuradas con varias regiones de escritura, pueden producirse conflictos de actualización cuando los redactores actualizan simultáneamente el mismo elemento en varias regiones. Conflictos de actualización pueden ser de los tres tipos siguientes:

* **Conflictos de inserción**: Estos conflictos pueden producirse cuando una aplicación inserta simultáneamente dos o más elementos con el mismo índice único en dos o más regiones. Por ejemplo, podría aparecer este conflicto con una propiedad de ID.

* **Conflictos de reemplazo**: Estos conflictos pueden producirse cuando una aplicación actualiza el mismo elemento en dos o más regiones al mismo tiempo.

* **Conflictos de eliminación**: Estos conflictos pueden producirse cuando una aplicación al mismo tiempo elimina un elemento en una región y la actualiza en otra región.

## <a name="conflict-resolution-policies"></a>Directivas de resolución de conflictos

Azure Cosmos DB ofrece un mecanismo flexible basado en directivas para resolver conflictos de escritura. Puede seleccionar entre dos directivas de resolución de conflictos en un contenedor de Azure Cosmos:

- **Últimos casos de escritura correcta (LWW)**: esta directiva de resolución utiliza de forma predeterminada una propiedad de marca de tiempo definida por el sistema. (que se basa en el protocolo de reloj de sincronización de hora). Si usa la API de SQL, puede especificar cualquier otro numérico propiedad personalizada (por ejemplo, su propia noción de una marca de tiempo) que se usará para la resolución de conflictos. Una propiedad numérica personalizada también se conoce como el *ruta de acceso de resolución de conflictos*. 

  Si dos o más elementos entran en conflicto en operaciones de inserción o de reemplazo, el elemento que contiene el valor más alto para la ruta de acceso para la resolución de conflictos se convierte en el ganador. Si varios elementos tienen el mismo valor numérico para la ruta de acceso para la resolución de conflictos, el sistema determina la versión ganadora. Se garantiza que todas las regiones convergen en un solo ganador y finalizan con la versión idéntica del elemento confirmado. En caso de que haya conflictos de eliminación, la versión eliminada siempre se impone a otros conflictos, ya sean de inserción o de reemplazo, Este resultado se produce independientemente de cuál sea el valor de la ruta de acceso de resolución de conflictos.

  > [!NOTE]
  > Últimos casos de escritura correcta es la directiva de resolución de conflictos predeterminada Está disponible para las API siguientes: SQL, MongoDB, Cassandra, Gremlin y Table.

  Para obtener más información, consulte los [ejemplos de uso de directivas de resolución de conflictos LWW](how-to-manage-conflicts.md).

- **Personalizada**: esta directiva de resolución se ha diseñado para la conciliación de conflictos con la semántica definida por la aplicación. Al establecer esta directiva en el contenedor de Azure Cosmos, también debe registrar un *combinar el procedimiento almacenado*. Este procedimiento se invoca automáticamente cuando se detectan conflictos en una transacción de base de datos en el servidor. El sistema garantiza exactamente una vez la ejecución de un procedimiento de combinación como parte del protocolo de confirmación.  

  Si configura el contenedor con la opción de resolución personalizada y no puede registrar un procedimiento de mezcla en el contenedor o el procedimiento de mezcla produce una excepción en tiempo de ejecución, los conflictos se escriben en el *conflictos fuente*. La aplicación tendrá luego que resolver manualmente los conflictos en la fuente de conflictos. Para obtener más información, consulte los [ejemplos de uso de la directiva de resolución personalizada y la fuente de conflictos](how-to-manage-conflicts.md).

  > [!NOTE]
  > La directiva de resolución de conflictos personalizada solo está disponible para las cuentas de API de SQL.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo configurar directivas de resolución de conflictos:

* [Configuración de una arquitectura multimaestro en las aplicaciones](how-to-multi-master.md)
* [Cómo administrar directivas de resolución de conflictos](how-to-manage-conflicts.md)
* [Cómo se leen de la fuente de conflictos](how-to-manage-conflicts.md#read-from-conflict-feed)
