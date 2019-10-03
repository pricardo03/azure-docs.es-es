---
title: Guía de control de errores de Azure Functions
description: Aprenda a controlar los errores en Azure Functions con vínculos a errores específicos de enlace.
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: gwallace
ms.assetid: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: d301940998175adb6469e46cff2de918b5cae9df
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155276"
---
# <a name="azure-functions-error-handling"></a>Control de errores de Azure Functions

El control de los errores en Azure Functions es importante para evitar la pérdida de datos, eventos perdidos y para supervisar el estado de la aplicación.

En este artículo se describen las estrategias generales para el control de errores junto con vínculos a errores específicos de enlace.

## <a name="handling-errors"></a>Control de errores

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Códigos de error de enlace

Cuando se realizan integraciones con los servicios de Azure, pueden originarse errores desde las API de los servicios subyacentes. La información relacionada con los errores específicos de enlace está disponible en la sección **Excepciones y códigos de retorno** de los siguientes artículos:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Centros de notificaciones](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Table storage](functions-bindings-storage-table.md#exceptions-and-return-codes)
