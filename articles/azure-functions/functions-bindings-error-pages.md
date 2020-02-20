---
title: Guía de control de errores de Azure Functions
description: Aprenda a controlar los errores en Azure Functions con vínculos a errores específicos de enlace.
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: 920b887382ca55d84c66fabb8f1464fd05041094
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198467"
---
# <a name="azure-functions-error-handling"></a>Control de errores de Azure Functions

El control de los errores en Azure Functions es importante para evitar la pérdida de datos, eventos perdidos y para supervisar el estado de la aplicación.

En este artículo se describen las estrategias generales para el control de errores junto con vínculos a errores específicos de enlace.

## <a name="handling-errors"></a>Control de errores

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Códigos de error de enlace

Cuando se realizan integraciones con los servicios de Azure, pueden originarse errores desde las API de los servicios subyacentes. La información relacionada con los errores específicos de enlace está disponible en la sección **Excepciones y códigos de retorno** de los siguientes artículos:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob-output.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Centros de notificaciones](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Table storage](functions-bindings-storage-table.md#exceptions-and-return-codes)
