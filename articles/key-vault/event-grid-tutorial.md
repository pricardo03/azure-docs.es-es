---
title: Recepción de notificaciones del almacén de claves y respuesta con Azure Event Grid
description: Aprenda a integrar Key Vault con Azure Event Grid.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: 3b24da4d988554da240baba2984df44ff4744aaf
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464092"
---
# <a name="how-to-receive-and-respond-to-key-vault-notifications-with-azure-event-grid-preview"></a>Procedimientos para: Recibir notificaciones del almacén de claves y responder con Azure Event Grid (versión preliminar)

La integración de Key Vault con Azure Event Grid, actualmente en versión preliminar, permite a los usuarios recibir una notificación cuando cambia el estado de un secreto almacenado en el almacén de claves. Para información general sobre la característica, consulte [Supervisión de Key Vault con Azure Event Grid](event-grid-overview.md).

En esta guía se muestra cómo recibir notificaciones de Key Vault mediante Azure Event Grid y cómo responder a los cambios de estado con Azure Automation.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
- Un almacén de claves en la suscripción de Azure. Puede crear rápidamente un almacén de claves si sigue los pasos descritos en [Establecimiento y recuperación de un secreto desde Azure Key Vault mediante la CLI de Azure](quick-create-cli.md)

## <a name="concepts"></a>Conceptos

Azure Event Grid es un servicio de eventos para la nube. En esta guía se suscribirá a los eventos del almacén de claves y se enrutarán los eventos a Azure Automation. Cuando uno de los secretos del almacén de claves está a punto de expirar, Event Grid recibe una notificación del cambio de estado y realiza una solicitud HTTP POST al punto de conexión. Después, un webhook desencadena una ejecución de Azure Automation del script de PowerShell.

![imagen](media/image1.png)

## <a name="create-an-azure-automation-account"></a>Creación de una cuenta de Azure Automation

Cree una cuenta de Azure Automation mediante [Azure Portal](https://portal.azure.com).

1.  Vaya a portal.azure.com e inicie sesión en la suscripción.

1.  En el cuadro de búsqueda, escriba "Cuentas de Automation".

1.  En la sección "Servicios" de la lista desplegable de la barra de búsqueda, seleccione "Cuentas de Automation".

1.  Haga clic en Agregar.

    ![](media/image2.png)

1.  Rellene la información necesaria en la hoja "Agregar cuenta de Automation" y seleccione "Crear".

## <a name="create-a-runbook"></a>Creación de un runbook

Una vez que la cuenta de Azure Automation esté lista, cree un runbook.

![](media/image3.png)

1.  Seleccione la cuenta de Automation que acaba de crear.

1.  En la sección Automatización de procesos, seleccione "Runbooks".

1.  Haga clic en "Crear un runbook".

1.  Asígnele un nombre al runbook y seleccione "PowerShell" como tipo del runbook.

1.  Haga clic en el runbook que ha creado y seleccione el botón "Editar".

1.  Escriba el código siguiente (con fines de prueba) y haga clic en el botón "Publicar". De este modo se generará el resultado de la solicitud POST recibida.

```azurepowershell
param
(
[Parameter (Mandatory = $false)]
[object] $WebhookData
)

#If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

#rotate secret:
#generate new secret version in key vault
#update db/service with generated secret

#Write-Output "WebhookData <$WebhookData>"
Write-Output $WebhookData.RequestBody
}
else
{
# Error
write-Error "No input data found." 
}
```

![](media/image4.png)

## <a name="create-a-webhook"></a>Creación de un webhook

Ahora cree un webhook para desencadenar el runbook que acaba de crear.

1.  Seleccione "Webhooks" en la sección de recursos del runbook que acaba de publicar.

1.  Haga clic en "Agregar webhook".

    ![](media/image5.png)

1.  Seleccione "Crear nuevo Webhook".

1. Asígnele un nombre al webhook, establezca una fecha de expiración y **copie la dirección URL**.

    > [!IMPORTANT] 
    > Una vez creada, no verá la dirección URL. Asegúrese de guardar una copia en una ubicación segura a la que pueda acceder durante el resto de esta guía.

1. Haga clic en "Configuración de ejecución y parámetros" y seleccione "Aceptar". No escriba ningún parámetro. Esto habilitará el botón "Crear".

1. Seleccione "Aceptar" y "Crear".

    ![](media/image6.png)

## <a name="create-an-event-grid-subscription"></a>Creación de una suscripción de Event Grid

Cree una suscripción de Event Grid con [Azure Portal](https://portal.azure.com).

1.  Abra Azure Portal desde el vínculo siguiente: https://portal.azure.com/?Microsoft_Azure_KeyVault_ShowEvents=true&Microsoft_Azure_EventGrid_publisherPreview=true

1.  Vaya al almacén de claves y seleccione la pestaña "Eventos". Si no ve la pestaña Eventos, asegúrese de que está usando la [versión preliminar del portal](https://ms.portal.azure.com/?Microsoft_Azure_KeyVault_ShowEvents=true&Microsoft_Azure_EventGrid_publisherPreview=true).

    ![](media/image7.png)

1.  Haga clic en el botón "+ Suscripción de eventos".

1.  Cree un nombre descriptivo para la suscripción.

1.  Elija "Esquema de Event Grid".

1.  "Recurso de tema" debe ser el almacén de claves cuyos cambios de estado desea supervisar.

1.  En "Filtro para tipos de evento", deje activadas todas las casillas ("9 seleccionadas").

1.  En "Tipo de punto de conexión", seleccione "Webhook".

1.  Seleccione "Select an endpoint" (Seleccionar un punto de conexión). En el panel del nuevo contexto, pegue la dirección URL del webhook del paso [Creación de un webhook](#create-a-webhook) en el campo "Punto de conexión de suscriptor".

1.  Seleccione "Confirmar selección" en el panel de contexto.

1.  Seleccione "Crear".

    ![](media/image8.png)

## <a name="test-and-verify"></a>Prueba y comprobación

Compruebe que la suscripción de Event Grid tiene la propiedad configurada.  En esta prueba se supone que se ha suscrito a la notificación "Secret New Version Created" (Nueva versión del secreto creada) en [Creación de una suscripción de Event Grid](#create-an-event-grid-subscription) y que tiene los privilegios necesarios para crear una versión de un secreto en un almacén de claves.

![](media/image9.png)

![](media/image10.png)

1.  Vaya al almacén de claves en Azure Portal.

1.  Cree un secreto. Para fines de prueba, establezca la fecha de caducidad para el día siguiente.

1.  Vaya a la pestaña eventos en el almacén de claves.

1.  Seleccione la suscripción de Event Grid que ha creado.

1.  En Métricas, consulte si se capturó un evento. Se esperan dos: SecretNewVersion y SecretNearExpiry. Esto confirma que Event Grid capturó correctamente el cambio de estado del secreto en el almacén de claves.

    ![](media/image11.png)

1.  Vaya a la cuenta de Azure Automation.

1.  Seleccione la pestaña "Runbooks" y el runbook que creó.

1.  Seleccione la pestaña "Webhooks" y confirme que la marca de tiempo "Última vez que se desencadenó" se encuentra en un plazo de 60 segundos a partir del momento de la creación del secreto.  Esto confirma que Event Grid realizó una solicitud POST en el webhook con los detalles del evento de cambio de estado en el almacén de claves y que el webhook se desencadenó.

    ![](media/image12.png)

1. Vuelva al runbook y seleccione la pestaña "Información general".

1. Examine la lista Trabajos recientes. Debería ver que se ha creado un trabajo y que el estado es completado.  Esto confirma que el webhook desencadenó el runbook para iniciar la ejecución de su script.

    ![](media/image13.png)

1. Seleccione el trabajo reciente y examine la solicitud POST enviada desde Event Grid al webhook. Examine el archivo JSON y asegúrese de que los parámetros para el almacén de claves y el tipo de evento son correctos. Si el parámetro "event type" del objeto JSON coincide con el evento que se produjo en el almacén de claves (en este ejemplo, Microsoft.KeyVault.SecretNearExpiry), la prueba se realizó correctamente.

## <a name="troubleshooting"></a>solución de problemas

### <a name="unable-to-create-event-subscription"></a>Imposibilidad de creación de una suscripción de eventos

Vuelva a registrar Event Grid y el proveedor de Key Vault en los proveedores de recursos de la suscripción de Azure. Consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/resource-manager-supported-services.md).

## <a name="next-steps"></a>Pasos siguientes

Felicidades. Si ha seguido todos los pasos anteriores ya está listo para responder mediante programación a los cambios de estado de los secretos almacenados en el almacén de claves.

Si ha usado un sistema basado en sondeo para buscar cambios de estado en los secretos de los almacenes de claves, realice la migración con esta característica de notificación. También puede reemplazar el script de prueba del runbook con código para renovar los secretos mediante programación cuando están a punto de expirar.

Más información:

- [Introducción a Azure Key Vault](key-vault-overview.md)
- [Introducción a Azure Event Grid](../event-grid/overview.md)
- [Supervisión de Key Vault con Azure Event Grid (versión preliminar)](event-grid-overview.md)
- [Esquema de eventos de Azure Event Grid para Azure Key Vault (versión preliminar)](../event-grid/event-schema-key-vault.md)
- [Información general sobre Azure Automation](../automation/index.yml)
