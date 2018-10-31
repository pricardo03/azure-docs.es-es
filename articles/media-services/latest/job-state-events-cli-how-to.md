---
title: Supervisión de eventos de Azure Media Services con Event Grid mediante la CLI | Microsoft Docs
description: En este artículo se muestra cómo puede suscribirse a Event Grid para supervisar eventos de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 8145b4eb3c39511eb9cd0ed052c36b8338191d4f
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389503"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-cli"></a>Crear y supervisar eventos de Media Services con Event Grid mediante la CLI de Azure

Azure Event Grid es un servicio de eventos para la nube. En este artículo se usa la CLI de Azure para suscribirse a eventos para su cuenta de Azure Media Services. A continuación, deberá desencadenar esos eventos para ver el resultado. Por lo general, se envían eventos a un punto de conexión que procesa los datos del evento y realiza acciones. En este artículo, los eventos se envían a una aplicación web que recopila y muestra los mensajes.

## <a name="prerequisites"></a>Requisitos previos

- Tener una suscripción de Azure activa.
- [Cree una cuenta de Media Services](create-account-cli-how-to.md).

    Asegúrese de recordar los valores que usó para el nombre de la cuenta de Media Services y el nombre del grupo de recursos.

- Instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). En este artículo se necesita la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. También puede usar [Azure Cloud Shell](https://shell.azure.com/bash).

## <a name="create-a-message-endpoint"></a>Creación de un punto de conexión de mensaje

Antes de suscribirse a los eventos de la cuenta de Media Services, vamos a crear el punto de conexión para el mensaje del evento. Normalmente, el punto de conexión realiza acciones en función de los datos del evento. En este artículo debe implementar una [aplicación web precompilada](https://github.com/Azure-Samples/azure-event-grid-viewer) que muestre los mensajes de los eventos. La solución implementada incluye un plan de App Service, una aplicación web de App Service y el código fuente desde GitHub.

1. Seleccione **Deploy to Azure** (Implementar en Azure) para implementar la solución en su suscripción. En Azure Portal, proporcione valores para los parámetros.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. La implementación puede tardar unos minutos en completarse. Después de que la implementación se haya realizado correctamente, puede ver la aplicación web para asegurarse de que se está ejecutando. En un explorador web, vaya a: `https://<your-site-name>.azurewebsites.net`

Si cambia al sitio "Visor de Azure Event Grid", verá que no tiene todavía ningún evento.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](http://portal.azure.com) e inicie **CloudShell** para ejecutar comandos de la CLI, como se muestra en los siguientes pasos.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar la CLI localmente, para este tema es preciso la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Establecimiento de la suscripción de Azure

En el siguiente comando, proporcione el identificador de suscripción de Azure que quiere usar para la cuenta de Media Services. Para ver una lista de las suscripciones a las que tiene acceso, vaya a [Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

```azurecli-interactive
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

## <a name="subscribe-to-media-services-events"></a>Suscribirse a los eventos de Media Services

Suscríbase a un artículo para indicar a Event Grid los eventos cuyo seguimiento desea realizar. En el ejemplo siguiente se realiza la suscripción a la cuenta de Media Services que creó y se pasa la dirección URL del sitio web que creó como punto de conexión para la notificación de eventos. 

Reemplace `<event_subscription_name>` por un nombre único para la suscripción de eventos. Para `<resource_group_name>` y `<ams_account_name>`, use los valores que usó al crear la cuenta de Media Services. En el caso de `api/updates`, especifique la dirección URL de la aplicación web y agregue `<endpoint_URL>` a la dirección URL de la página principal. Al especificar el punto de conexión cuando se realiza la suscripción, Event Grid controla el enrutamiento de los eventos a dicho punto de conexión. 

1. Obtenga el identificador del recurso

    ```azurecli-interactive
    amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)
    ```

    Por ejemplo: 

    ```
    amsResourceId=$(az ams account show --name amsaccount --resource-group amsResourceGroup --query id --output tsv)
    ```

2. Suscríbase a los eventos

    ```azurecli-interactive
    az eventgrid event-subscription create \
    --resource-id $amsResourceId \
    --name <event_subscription_name> \
    --endpoint <endpoint_URL>
    ```

    Por ejemplo: 

    ```
    az eventgrid event-subscription create --resource-id $amsResourceId --name amsTestEventSubscription --endpoint https://amstesteventgrid.azurewebsites.net/api/updates/
    ```    

    > [!TIP]
    > Es posible que reciba una advertencia del protocolo de enlace de validación. Espere unos minutos y el protocolo de enlace debería realizar la validación.

A continuación, desencadenaremos algunos eventos para ver cómo Event Grid distribuye el mensaje al punto de conexión.

## <a name="send-an-event-to-your-endpoint"></a>Envío de un evento al punto de conexión

Puede desencadenar eventos de la cuenta de Media Services si ejecuta un trabajo de codificación. Puede seguir [este inicio rápido](stream-files-dotnet-quickstart.md) para codificar un archivo y empezar a enviar eventos. 

Vuelva a la aplicación web y observe que se ha enviado un evento de validación de suscripción. Event Grid envía el evento de validación para que el punto de conexión pueda verificar que desea recibir datos de eventos. El punto de conexión debe establecer `validationResponse` en `validationCode`. Para más información, vea [Event Grid security and authentication](../../event-grid/security-authentication.md) (Seguridad y autenticación de Event Grid). Puede ver el código de la aplicación web para ver cómo valida la suscripción.

> [!TIP]
> Seleccione el icono del ojo para expandir los datos del evento. No actualice la página si quiere ver todos los eventos.

![Visualización del evento de suscripción](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Pasos siguientes

[Carga, codificación y streaming](stream-files-tutorial-with-api.md)

