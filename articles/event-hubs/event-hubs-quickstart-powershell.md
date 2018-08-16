---
title: 'Guía de inicio rápido de Azure: Procesamiento de flujos de eventos mediante PowerShell | Microsoft Docs'
description: En esta guía de inicio rápido se describe cómo enviar y recibir eventos de Azure Event Hubs utilizando PowerShell y una aplicación de .NET de ejemplo.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/26/2018
ms.author: shvija
ms.openlocfilehash: fd2b8b2e8bd075e029a07519ced186424798fd9c
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003617"
---
# <a name="quickstart-process-event-streams-using-powershell-and-net-standard"></a>Guía de inicio rápido de Azure: procesamiento de flujos de eventos mediante PowerShell y .NET Standard

Azure Event Hubs es una plataforma de streaming de datos y servicio de ingesta de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. En esta guía de inicio rápido se muestra cómo crear un centro de eventos mediante Azure PowerShell y, a continuación, enviar a un centro de eventos y recibir del mismo utilizando el SDK de .NET Standard.

Para completar esta guía de inicio rápido, necesita una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita][] antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, asegúrese de disponer de los siguientes elementos:

- [Visual Studio 2017 Update 3 (versión 15.3, 26730.01)](http://www.visualstudio.com/vs) o posterior.
- [SDK de .NET Standard](https://www.microsoft.com/net/download/windows), versión 2.0 o posterior.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si está usando PowerShell localmente, tiene que ejecutar la versión más reciente de PowerShell para completar esta guía de inicio rápido. Si necesita instalarlo o actualizarlo, consulte [Instalación y configuración de Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

## <a name="provision-resources"></a>Aprovisionamiento de recursos

### <a name="create-a-resource-group"></a>Creación de un grupo de recursos

Un grupo de recursos es una recopilación lógica de recursos de Azure, para crear un centro de eventos necesita un grupo de recursos. 

En el siguiente ejemplo, se crea un grupo de recursos en la región Este de EE. UU. Sustituya `myResourceGroup` por el nombre del grupo de recursos que quiere utilizar:

```azurepowershell-interactive
New-AzureRmResourceGroup –Name myResourceGroup –Location eastus
```

### <a name="create-an-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs

Una vez que se crea el grupo de recursos, cree un espacio de nombres de Event Hubs dentro de ese grupo de recursos. Un espacio de nombres de Event Hubs proporciona un nombre de dominio completo y exclusivo en el que puede crear el centro de eventos. Reemplace `namespace_name` por un nombre único para el espacio de nombres:

```azurepowershell-interactive
New-AzureRmEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

### <a name="create-an-event-hub"></a>Creación de un centro de eventos

Ahora que ya tiene un espacio de nombres de Event Hubs, cree un centro de eventos dentro de ese espacio de nombres:

```azurepowershell-interactive
New-AzureRmEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name
```

### <a name="create-a-storage-account-for-event-processor-host"></a>Creación de una cuenta de almacenamiento para el host de procesador de eventos

El host de procesador de eventos simplifica la recepción de eventos desde Event Hubs mediante la administración de puntos de control y recepciones paralelas. Para los puntos de control, el host de procesador de eventos requiere una cuenta de almacenamiento. Para crear una cuenta de almacenamiento y obtener sus claves, ejecute los siguientes comandos:

```azurepowershell-interactive
# Create a standard general purpose storage account 
New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name storage_account_name -Location eastus -SkuName Standard_LRS 
e
# Retrieve the storage account key for accessing it
Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name storage_account_name
```

### <a name="get-the-connection-string"></a>Obtención de la cadena de conexión

Una cadena de conexión es necesaria para conectarse al centro de eventos y procesar eventos. Para obtener la cadena de conexión, ejecute:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Name RootManageSharedAccessKey
```

## <a name="stream-into-event-hubs"></a>Transmisión a Event Hubs

Ahora puede iniciar el streaming a las instancias de Event Hubs. Los ejemplos se pueden descargar o clonar de Git desde el [repositorio de Event Hubs](https://github.com/Azure/azure-event-hubs)

### <a name="ingest-events"></a>Ingesta de eventos

Para iniciar el streaming de eventos, descargue [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) desde GitHub o clone el [repositorio de GitHub de Event Hubs](https://github.com/Azure/azure-event-hubs) ejecutando el comando siguiente:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Vaya a la carpeta \azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleSender y cargue el archivo SampleSender.sln en Visual Studio.

A continuación, agregue el paquete NuGet [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) al proyecto.

En el archivo Program.cs, reemplace los siguientes marcadores de posición con el nombre del centro de eventos y la cadena de conexión:

```C#
private const string EhConnectionString = "Event Hubs connection string";
private const string EhEntityPath = "Event Hub name";

```

Ahora compile y ejecute el ejemplo. Puede ver los eventos que ingieren en el centro de eventos:

![][3]

### <a name="receive-and-process-events"></a>Recepción y procesamiento de eventos

Descargue ahora el ejemplo de receptor del host de procesador de eventos, que recibe los mensajes que acaba de enviar. Descargue [SampleEphReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) desde GitHub o clone el [repositorio de GitHub de Event Hubs](https://github.com/Azure/azure-event-hubs) ejecutando el comando siguiente:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Vaya a la carpeta \azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleEphReceiver y cargue el archivo de solución SampleEphReceiver.sln en Visual Studio.

A continuación agregue los paquetes NuGet [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) y [Microsoft.Azure.EventHubs.Processor](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) al proyecto.

En el archivo Program.cs, reemplace las siguientes constantes con sus valores correspondientes:

```C#
private const string EventHubConnectionString = "Event Hubs connection string";
private const string EventHubName = "Event Hub name";
private const string StorageContainerName = "Storage account container name";
private const string StorageAccountName = "Storage account name";
private const string StorageAccountKey = "Storage account key";
```

Ahora compile y ejecute el ejemplo. Puede ver los eventos que se reciben en la aplicación de ejemplo:

![][4]

En Azure Portal, puede ver la velocidad a la que se están procesando los eventos para un determinado espacio de nombres de Event Hubs como se muestra:

![][5]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya completado esta guía de inicio rápido, puede eliminar el grupo de recursos y la cuenta de almacenamiento de espacio de nombres, así como el centro de eventos dentro de él. Sustituya `myResourceGroup` por el nombre del grupo de recursos que ha creado. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado un espacio de nombres de Event Hubs y otros recursos necesarios para enviar y recibir eventos de su centro de eventos. Para más información, continúe con el siguiente tutorial:

> [!div class="nextstepaction"]
> [Visualización de anomalías de datos en flujos de datos de Event Hubs](event-hubs-tutorial-visualize-anomalies.md)

[cree una cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[New-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
