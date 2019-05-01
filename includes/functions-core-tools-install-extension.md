---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: craigshoemaker
ms.service: functions
ms.topic: include
ms.date: 09/25/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: fc5b43dcdee394fea023124171fb42c1a18224dc
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64733265"
---
Paquetes de extensión realizar todos los enlaces publicados por el equipo de Azure Functions disponible a través de una configuración en el *host.json* archivo. Para el desarrollo local, asegúrese de que tiene la versión más reciente de [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#install-the-azure-functions-core-tools).

Para usar paquetes de extensión, actualice el *host.json* archivo para incluir la siguiente entrada para `extensionBundle`:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

- El `id` propiedad hace referencia el espacio de nombres de los paquetes de extensión de Microsoft Azure Functions.
- El `version` hace referencia a la versión de la agrupación.

Incremento de las versiones de agrupación como paquetes en los cambios del lote. Cambios de versión principal realizan solo cuando mueve los paquetes en el paquete de una versión principal. El `version` propiedad usa el [notación de intervalo para especificar intervalos de versiones](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges-and-wildcards). El tiempo de ejecución de Functions siempre elige la versión permitida máxima definida por el intervalo de versiones o el intervalo.

Una vez que se hace referencia a los paquetes de extensión en el proyecto, a continuación, todos los enlaces predeterminados están disponibles para las funciones. Los enlaces disponibles en el [conjunto de extensiones](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) son:

|Paquete  |`Version`  |
|---------|---------|
|Microsoft.Azure.WebJobs.Extensions.CosmosDB|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.DurableTask|1.8.0|
|Microsoft.Azure.WebJobs.Extensions.EventGrid|2.0.0|
|Microsoft.Azure.WebJobs.Extensions.EventHubs|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.SendGrid|3.0.0|
|Microsoft.Azure.WebJobs.Extensions.ServiceBus|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.SignalRService|1.0.0|
|Microsoft.Azure.WebJobs.Extensions.Storage|3.0.4|
|Microsoft.Azure.WebJobs.Extensions.Twilio|3.0.0|