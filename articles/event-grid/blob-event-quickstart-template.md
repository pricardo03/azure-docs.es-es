---
title: Envío de eventos de almacenamiento de blobs a un punto de conexión web desde la plantilla
description: Use Azure Event Grid y una plantilla de Azure Resource Manager para crear una cuenta de almacenamiento de blobs y suscribirse a sus eventos. Envíe los eventos a un webhook.
services: event-grid
keywords: ''
author: spelluru
ms.author: spelluru
ms.date: 01/15/2020
ms.topic: quickstart
ms.service: event-grid
ms.openlocfilehash: 73f0e6cf5d7ebb3ae36d4048ce5f36f5c0547286
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122714"
---
# <a name="route-blob-storage-events-to-web-endpoint-by-using-azure-resource-manager-template"></a>Enrutamiento de eventos de almacenamiento de blobs a un punto de conexión web mediante una plantilla de Azure Resource Manager
Azure Event Grid es un servicio de eventos para la nube. En este artículo, se usará una **plantilla de Azure Resource Manager** para crear una cuenta de almacenamiento de blobs, suscribirse a eventos de ese almacenamiento de blobs y desencadenar un evento para ver el resultado. Por lo general, se envían eventos a un punto de conexión que procesa los datos del evento y realiza acciones. Sin embargo, para simplificar en este artículo, los eventos se envían a una aplicación web que recopila y muestra los mensajes.

La [plantilla de Resource Manager](../azure-resource-manager/templates/overview.md) es un archivo JSON (notación de objetos JavaScript) que contiene la infraestructura y la configuración del proyecto. La plantilla usa sintaxis declarativa, lo que permite establecer lo que pretende implementar sin tener que escribir la secuencia de comandos de programación para crearla. Si desea más información sobre el desarrollo de plantillas de Resource Manager, consulte la [documentación de Resource Manager](/azure/azure-resource-manager/).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites
### <a name="create-a-message-endpoint"></a>Creación de un punto de conexión de mensaje
Antes de suscribirse a los eventos de Blob Storage, vamos a crear el punto de conexión para el mensaje del evento. Normalmente, el punto de conexión realiza acciones en función de los datos del evento. Para simplificar esta guía de inicio rápido, se implementa una [aplicación web pregenerada](https://github.com/Azure-Samples/azure-event-grid-viewer) que muestra los mensajes de los eventos. La solución implementada incluye un plan de App Service, una aplicación web de App Service y el código fuente desde GitHub.

1. Seleccione **Deploy to Azure** (Implementar en Azure) para implementar la solución en su suscripción. En Azure Portal, proporcione valores para los parámetros.

    [Implementación en Azure](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json)
1. La implementación puede tardar unos minutos en completarse. Después de que la implementación se haya realizado correctamente, puede ver la aplicación web para asegurarse de que se está ejecutando. En un explorador web, vaya a: `https://<your-site-name>.azurewebsites.net`

1. Verá el sitio, pero aún no se ha publicado en él ningún evento.

   ![Visualización del nuevo sitio](./media/blob-event-quickstart-portal/view-site.png)


## <a name="create-a-storage-account-with-an-event-grid-subscription"></a>Creación de una cuenta de almacenamiento con una suscripción de Event Grid
La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage).

[!code-json[<Azure Resource Manager template create Blob strage Event Grid subscription>](~/quickstart-templates/101-event-grid-subscription-and-storage/azuredeploy.json)]

En la plantilla se definen dos recursos de Azure:

* **Microsoft.Storage/storageAccounts**: cree una cuenta de Azure Storage.
* **"Microsoft.Storage/storageAccounts/providers/eventSubscriptions**: cree una suscripción de Azure Event Grid para la cuenta de almacenamiento. 

1. Seleccione el vínculo siguiente para iniciar sesión en Azure y abrir una plantilla. La plantilla crea un almacén de claves y un secreto.

    [Implementación en Azure](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-event-grid-subscription-and-storage%2Fazuredeploy.json)
2. Especifique el **punto de conexión**: proporcione la dirección URL de la aplicación web y agregue `api/updates` a la dirección URL de la página principal.
3. Seleccione **Comprar** para implementar la plantilla. 

  Azure Portal se usa aquí para implementar la plantilla. También puede usar Azure PowerShell, la CLI de Azure o la API REST. Para obtener información sobre otros métodos de implementación, consulte [Implementación de plantillas](../azure-resource-manager/templates/deploy-powershell.md).

> [!NOTE]
> Puede encontrar [aquí](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid) más ejemplos de plantillas de Azure Event Grid.

## <a name="validate-the-deployment"></a>Validación de la implementación
Vuelva a la aplicación web y observe que se ha enviado un evento de validación de suscripción. Seleccione el icono del ojo para expandir los datos del evento. Event Grid envía el evento de validación para que el punto de conexión pueda verificar que desea recibir datos de eventos. La aplicación web incluye código para validar la suscripción.

![Visualización del evento de suscripción](./media/blob-event-quickstart-portal/view-subscription-event.png)

Ahora, vamos a desencadenar un evento para ver cómo Event Grid distribuye el mensaje al punto de conexión.

Desencadenará un evento para Blob Storage mediante la carga de un archivo. El archivo no necesita ningún contenido específico. En los artículos se da por hecho que tiene un archivo llamado testfile.txt, pero puede usar cualquier archivo.

Al cargar el archivo en el almacenamiento de blobs de Azure, Event Grid envía un mensaje al punto de conexión que configuró al suscribirse. El mensaje está en formato JSON y contiene una matriz con uno o más eventos. En el ejemplo siguiente, el mensaje JSON contiene una matriz con un evento. Vea la aplicación web y observe que se ha recibido un evento de blob creado. 

![Vista de resultados](./media/blob-event-quickstart-portal/view-results.png)



## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no necesite, [elimine el grupo de recursos](../azure-resource-manager/management/delete-resource-group.md?tabs=azure-portal#delete-resource-group
). 

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las plantillas de Azure Resource Manager, consulte los artículos siguientes:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager)
- [Definición de recursos en plantillas de Azure Resource Manager](/azure/templates/)
- [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/)
- [Plantillas de Azure Event Grid](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid)
