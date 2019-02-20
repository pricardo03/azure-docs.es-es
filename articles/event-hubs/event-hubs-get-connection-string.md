---
title: Obtención de una cadena de conexión de Azure Event Hubs | Microsoft Docs
description: En este artículo se proporcionan instrucciones acerca de cómo obtener una cadena de conexión que los clientes pueden usar para conectarse a Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: ee4bd5d2acf1a029486f83ee721b9e1f72347958
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238154"
---
# <a name="get-an-event-hubs-connection-string"></a>Obtención de una cadena de conexión de Event Hubs

Para utilizar Event Hubs, debe crear un espacio de nombres de Event Hubs. Un espacio de nombres es un contenedor de ámbito que puede alojar varios temas de Event Hubs y Kafka. Este espacio de nombres proporciona un [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) único. Una vez creado un espacio de nombres, puede obtener la cadena de conexión necesaria para comunicarse con Event Hubs.

La cadena de conexión para Azure Event Hubs tiene los siguientes componentes incrustados dentro de ella:

* FQDN = el FQDN del espacio de nombres de Event Hubs que creó (incluirá el nombre del espacio de nombres de Event Hubs seguido de servicebus.windows.net).
* SharedAccessKeyName = el nombre que eligió para las claves SAS de la aplicación.
* SharedAccessKey = el valor generado de la clave.

La plantilla de la cadena de conexión tiene el siguiente aspecto
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Un ejemplo de cadena de conexión podría ser similar a `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

En este artículo se explican distintas formas de obtener la cadena de conexión.

## <a name="get-connection-string-from-the-portal"></a>Obtención de la cadena de conexión del portal

Una vez que tenga el espacio de nombres de Event Hubs, la sección de información general del portal puede proporcionar la cadena de conexión como se muestra a continuación:

![Cadena de conexión de Event Hubs](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)

Al hacer clic en el vínculo de la cadena de conexión en la sección de información general, se abre la pestaña de directivas SAS como se muestra en la siguiente figura:

![Directivas SAS de Event Hubs](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)

Puede agregar una nueva directiva SAS y obtener la cadena de conexión o usar la directiva predeterminada que ya se ha creado automáticamente. Cuando se abre la directiva, se obtiene la cadena de conexión como se muestra en la siguiente figura:

![Obtención de la cadena de conexión de Event Hubs](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Obtención de la cadena de conexión con Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Puede usar Get-AzEventHubNamespaceKey para obtener la cadena de conexión del nombre de directiva o regla específica, tal como se muestra a continuación:

```azurepowershell-interactive
Get-AzEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

Consulte [Azure Event Hubs PowerShell module](https://docs.microsoft.com/powershell/module/az.eventhub/get-azeventhubkey) (Módulo de PowerShell de Azure Event Hubs) para obtener más detalles.

## <a name="getting-the-connection-string-with-azure-cli"></a>Obtención de la cadena de conexión con la CLI de Azure
Puede usar lo siguiente para obtener la cadena de conexión para el espacio de nombres:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Consulte [Azure CLI for Event Hubs](https://docs.microsoft.com/cli/azure/eventhubs) (CLI de Azure para Event Hubs) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Event Hubs, visite los vínculos siguientes:

* [Información general de Event Hubs](event-hubs-what-is-event-hubs.md)
* [Creación de un Centro de eventos](event-hubs-create.md)
