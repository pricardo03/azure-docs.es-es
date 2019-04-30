---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: tdykstra
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/23/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: d75dbf8f2f3de6101358274ef41f1b229950633d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60713531"
---
En la tabla siguiente se enumeran los atributos desencadenadores y de enlace que están disponibles en un proyecto de biblioteca de clases de Azure Functions. Todos los atributos están en el espacio de nombres `Microsoft.Azure.WebJobs`.

| Desencadenador | Entrada | Salida|
|------   | ------    | ------  |
| [BlobTrigger](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---attributes)| [Blob](../articles/azure-functions/functions-bindings-storage-blob.md#input---attributes)| [Blob](../articles/azure-functions/functions-bindings-storage-blob.md#output---attributes)|
| [CosmosDBTrigger](../articles/azure-functions/functions-bindings-cosmosdb.md#trigger---attributes)| [DocumentDB](../articles/azure-functions/functions-bindings-cosmosdb.md#input---attributes)| [DocumentDB](../articles/azure-functions/functions-bindings-cosmosdb.md#output---attributes) |
| [EventHubTrigger](../articles/azure-functions/functions-bindings-event-hubs.md#trigger---attributes)|| [EventHub](../articles/azure-functions/functions-bindings-event-hubs.md#output---attributes) |
| [HTTPTrigger](../articles/azure-functions/functions-bindings-http-webhook.md#trigger---attributes)|||
| [QueueTrigger](../articles/azure-functions/functions-bindings-storage-queue.md#trigger---attributes)|| [Cola](../articles/azure-functions/functions-bindings-storage-queue.md#output---attributes) |
| [ServiceBusTrigger](../articles/azure-functions/functions-bindings-service-bus.md#trigger---attributes)|| [Service Bus](../articles/azure-functions/functions-bindings-service-bus.md#output---attributes) |
| [TimerTrigger](../articles/azure-functions/functions-bindings-timer.md#attributes) | ||
| |[ApiHubFile](../articles/azure-functions/functions-bindings-external-file.md)| [ApiHubFile](../articles/azure-functions/functions-bindings-external-file.md)|
| |[MobileTable](../articles/azure-functions/functions-bindings-mobile-apps.md#input---attributes)| [MobileTable](../articles/azure-functions/functions-bindings-mobile-apps.md#output---attributes) | 
| |[Table](../articles/azure-functions/functions-bindings-storage-table.md#input---attributes)| [Table](../articles/azure-functions/functions-bindings-storage-table.md#output---attributes)  | 
| ||[NotificationHub](../articles/azure-functions/functions-bindings-notification-hubs.md#attributes) |
| ||[SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md#attributes) |
| ||[Twilio](../articles/azure-functions/functions-bindings-twilio.md#attributes)| 