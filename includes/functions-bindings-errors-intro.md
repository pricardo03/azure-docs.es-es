---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0c04e7812d023cd394b54cf03bcca11a5589b18a
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2019
ms.locfileid: "75564769"
---
Los errores que se producen en una instancia de Azure Functions pueden provenir de cualquiera de los orígenes siguientes:

- Uso de [desencadenadores y enlaces](..\articles\azure-functions\functions-triggers-bindings.md) de Azure Functions integrados
- Llamadas a las API de los servicios de Azure subyacentes
- Llamadas a puntos de conexión REST
- Llamadas a bibliotecas de cliente, paquetes o API de terceros

Seguir las siguientes prácticas de control de errores sólidas es importantes para evitar la pérdida de datos o mensajes perdidos. Entre las prácticas recomendadas de control de errores se incluyen las acciones siguientes:

- [Habilitación de Application Insights](../articles/azure-functions/functions-monitoring.md)
- [Uso del control de errores estructurado](#use-structured-error-handling)
- [Diseño para idempotencia](../articles/azure-functions/functions-idempotent.md)
- [Implementar directivas de reintentos](../articles/azure-functions/functions-reliable-event-processing.md) (si procede)

### <a name="use-structured-error-handling"></a>Uso del control de errores estructurado

La captura y publicación de los errores es fundamental para supervisar el estado de la aplicación. El nivel superior de cualquier código de función debe incluir un bloque try/catch. En el bloque catch, puede capturar y publicar errores.

### <a name="retry-support"></a>Compatibilidad con los reintentos

Los siguientes desencadenadores admiten el reintento integrado:

* [Almacenamiento de blobs de Azure](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure Queue Storage](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Service Bus (cola/tema)](../articles/azure-functions/functions-bindings-service-bus.md)

De forma predeterminada, estos desencadenadores reintentan las solicitudes hasta cinco veces. Después del quinto reintento, los desencadenadores de Azure Queue Storage como Azure Service Bus escriben un mensaje en una [cola de mensajes dudosos](..\articles\azure-functions\functions-bindings-storage-queue.md#trigger---poison-messages).

Debe implementar manualmente las directivas de reintento para cualquier otro desencadenador o tipo de enlace. Las implementaciones manuales pueden incluir la escritura de información de error en una [cola de mensajes dudosos](..\articles\azure-functions\functions-bindings-storage-blob.md#trigger---poison-blobs). Al escribir en una cola de mensajes dudosos, tiene la oportunidad de reintentar las operaciones más adelante. El desencadenador de Blob Storage usa este mismo enfoque.
