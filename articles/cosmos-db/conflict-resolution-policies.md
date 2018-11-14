---
title: Tipos de resolución de conflictos y directivas de resolución en Azure Cosmos DB
description: En este artículo se describen las categorías de conflicto y las directivas de resolución de conflictos de Azure Cosmos DB.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: c682b61a39224f2c80db8fe5fa153ea5e5d82922
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958578"
---
# <a name="conflict-types-and-resolution-policies"></a>Tipos de conflicto y directivas de resolución de conflictos

Los conflictos y las directivas de resolución de conflictos son aplicables si la cuenta de Cosmos está configurada con varias regiones de escritura.

En el caso de las cuentas de Cosmos DB configuradas con varias regiones de escritura, pueden producirse conflictos de actualización cuando varios objetos de escritura actualizan simultáneamente el mismo elemento en varias regiones. Los conflictos de actualización se clasifican en los tres tipos siguientes:

1. **Conflictos de inserción:** estos conflictos pueden producirse cuando una aplicación inserta simultáneamente dos o más elementos con un mismo índice único (por ejemplo, la propiedad ID) en dos o más regiones. En este caso, puede que todas las escrituras tengan éxito inicialmente en sus respectivas regiones locales, pero según la directiva de resolución de conflictos que se elija, al final solo se confirma un elemento con el identificador original.

1. **Conflictos de reemplazo:** estos conflictos pueden producirse cuando una aplicación actualiza simultáneamente un único elemento en dos o más regiones.

1. **Conflictos de eliminación:** estos conflictos pueden producirse cuando una aplicación elimina simultáneamente un elemento de una región y lo actualiza en otra región.

## <a name="conflict-resolution-policies"></a>Directivas de resolución de conflictos

Cosmos DB ofrece un mecanismo flexible controlado por directivas para resolver conflictos de actualización. En un contenedor de Cosmos, puede seleccionar una de las dos directivas de resolución de conflictos siguientes:

- **Últimos casos de escritura correcta (LWW):**  esta directiva de resolución utiliza de forma predeterminada una propiedad de marca de tiempo definida por el sistema (que se basa en el protocolo de reloj de sincronización de hora). Como alternativa, al usar la SQL API, Cosmos DB le permite especificar cualquier otra propiedad numérica personalizada (también denominada "ruta de acceso para la resolución de conflictos") que se vaya a utilizar para la resolución de conflictos.  

  Si dos o más elementos entran en conflicto en operaciones de inserción o de reemplazo, el elemento que contiene el valor más alto para la "ruta de acceso para la resolución de conflictos" se convierte en el "ganador". Si varios elementos tienen el mismo valor numérico para la ruta de acceso para la resolución de conflictos, el sistema determina la versión "ganadora". Se garantiza que todas las regiones convergen en un solo ganador y finalizan con la versión idéntica del elemento confirmado. En caso de que haya conflictos de eliminación, la versión eliminada siempre se impone a otros conflictos, ya sean de inserción o de reemplazo, independientemente del valor de la ruta de acceso para la resolución de conflictos.

  > [!NOTE]
  > Últimos casos de escritura correcta (LWW) es la directiva de resolución de conflictos predeterminada y está disponible para las cuentas de API de SQL, Table, MongoDB, Cassandra y Gremlin.

  Para más información, consulte [ejemplos de uso de directivas de resolución de conflictos LWW](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Personalizada:** esta directiva de resolución se ha diseñado para la conciliación de conflictos con la semántica definida por la aplicación. Al establecer esta directiva en el contenedor de Cosmos, debe registrar también un procedimiento almacenado de combinación, que se invoca automáticamente cuando se detectan conflictos de actualización en una transacción de base de datos en el servidor. El sistema garantiza exactamente una vez la ejecución de un procedimiento de combinación como parte del protocolo de confirmación.  

  Aunque, si configura el contenedor con la opción de resolución personalizada, pero no registra un procedimiento de combinación en el contenedor o el procedimiento de combinación produce una excepción en tiempo de ejecución, los conflictos se escriben en la fuente de conflictos. La aplicación tendrá luego que resolver manualmente los conflictos en la fuente de conflictos. Para más información, consulte [ejemplos de uso de la directiva de resolución personalizada y la fuente de conflictos](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > La directiva de resolución de conflictos personalizada solo está disponible para las cuentas de API de SQL.

## <a name="next-steps"></a>Pasos siguientes

A continuación puede aprender a configurar directivas de resolución de conflictos mediante los siguientes artículos:

* [Modo de uso de la directiva de resolución de conflictos LWW](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Modo de uso de la directiva de resolución de conflictos personalizada](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Modo de uso de la fuente de conflictos](how-to-manage-conflicts.md#read-from-conflict-feed)
