---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 12/26/2018
ms.author: v-junlch
ms.openlocfilehash: c1784111cd2fc2c93b67510f310b9e513cf2b86e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61438159"
---
Los [desencadenadores y enlaces](../articles/azure-functions/functions-triggers-bindings.md) de Azure Functions se comunican con varios servicios de Azure. Cuando se integran con estos servicios, pueden surgir errores que se originan desde las API de los servicios de Azure subyacentes. También pueden producirse errores al intentar comunicarse con otros servicios con el código de función mediante bibliotecas de cliente o REST. Para evitar la pérdida de datos y garantizar el buen comportamiento de las funciones, es importante administrar los errores desde uno u otro origen.

Los siguientes desencadenadores admiten el reintento integrado:

- [Almacenamiento de blobs de Azure](../articles/azure-functions/functions-bindings-storage-blob.md)
- [Azure Queue Storage](../articles/azure-functions/functions-bindings-storage-queue.md)
- [Azure Service Bus (cola/tema)](../articles/azure-functions/functions-bindings-service-bus.md)

De forma predeterminada, estos desencadenadores se reintentan hasta cinco veces. Después del quinto reintento, estos desencadenadores escriben un mensaje en una [cola especial de mensajes dudosos](../articles/azure-functions/functions-bindings-storage-queue.md#trigger---poison-messages).

Para el resto de desencadenadores de funciones, no hay ningún reintento integrado cuando se producen errores durante la ejecución de funciones. Para evitar la pérdida de información del desencadenador si se produce un error en la función, se recomienda usar bloques try-catch en el código de función para capturar los posibles errores. Cuando se produce un error, la información pasada a la función por el desencadenador se escribe en una cola especial de mensajes "dudosos". Este enfoque es el mismo que se usa el [desencadenador de Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---poison-blobs).

De esta manera, puede capturar eventos de desencadenador que podrían haberse perdido debido a errores y reintentarlos en otro momento mediante otra función, para procesar los mensajes de la cola de mensajes dudosos usando la información almacenada.  

<!-- ms.date: 12/26/2018 -->