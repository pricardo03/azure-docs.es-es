---
title: Azure Managed Applications con notificaciones
description: Configure la aplicación administrada con puntos de conexión de webhook para recibir notificaciones acerca de la creación, actualización, eliminación y errores en las instancias de la aplicación administrada.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.reviewer: ''
ms.author: ilahat
author: ilahat
ms.date: 11/01/2019
ms.openlocfilehash: 8cf9fc0b3d9c13ebc5309be6d27c7be0f2e60878
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74805695"
---
# <a name="azure-managed-applications-with-notifications"></a>Azure Managed Applications con notificaciones

Las notificaciones de aplicaciones administradas de Azure permiten a los publicadores automatizar acciones basadas en eventos del ciclo de vida de las instancias de la aplicación administrada. Los publicadores pueden especificar puntos de conexión de webhook de notificación personalizados para recibir notificaciones de eventos sobre las instancias de aplicaciones administradas nuevas y existentes. Permite al publicador configurar flujos de trabajo personalizados en el momento del aprovisionamiento, las actualizaciones y la eliminación de la aplicación.

## <a name="getting-started"></a>Introducción
Para empezar a recibir aplicaciones administradas, ponga en marcha un punto de conexión HTTPS público y especifíquelo al publicar la definición de la aplicación de catálogo de servicios o la oferta de Marketplace.

Esta es la serie de pasos recomendadas para ponerse en marcha rápidamente:
1. Ponga en marcha un punto de conexión HTTPS público que registre las solicitudes POST entrantes y devuelva `200 OK`.
2. Agregue el punto de conexión a la definición de aplicación de catálogo de servicios o a la oferta de Marketplace, como se explica a continuación.
3. Cree una instancia de aplicación administrada que haga referencia a la definición de la aplicación o a la oferta de Marketplace.
4. Compruebe que las notificaciones se reciben correctamente.
5. Habilite la autorización tal y como se explica en la siguiente sección **Autenticación de punto de conexión**.
6. Siga la documentación de **Esquema de notificación** siguiente para analizar las solicitudes de notificación e implementar la lógica de negocios basada en la notificación.

## <a name="adding-service-catalog-application-definition-notifications"></a>Adición de notificaciones de definición de aplicación del catálogo de servicios
#### <a name="azure-portal"></a>Portal de Azure
Lea [Publicación de una aplicación de catálogo de servicios mediante Azure Portal](./publish-portal.md) para comenzar.

![Notificaciones de definición de aplicación de catálogo de servicios en el portal](./media/publish-notifications/service-catalog-notifications.png)
#### <a name="rest-api"></a>API DE REST

> [!NOTE]
> Actualmente solo se admite un punto de conexión como parte de **notificationEndpoints** en las propiedades de definición de la aplicación

``` JSON
    {
      "properties": {
        "isEnabled": true,
        "lockLevel": "ReadOnly",
        "displayName": "Sample Application Definition",
        "description": "Notification-enabled application definition.",
        "notificationPolicy": {
            "notificationEndpoints": [
                {
                    "uri": "https://isv.azurewebsites.net:1214?sig=unique_token"
                }
            ]
        },
        "authorizations": [
          {
            "principalId": "d6b7fbd3-4d99-43fe-8a7a-f13aef11dc18",
            "roleDefinitionId": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635"
          },
        ...

```
## <a name="adding-marketplace-managed-application-notifications"></a>Adición de notificaciones de aplicaciones administradas por Marketplace
Para obtener más información, consulte [Crear una oferta de una aplicación de Azure](../marketplace/cloud-partner-portal/azure-applications/cpp-create-offer.md).

![Notificaciones de definición de aplicación de catálogo de servicios en el portal](./media/publish-notifications/marketplace-notifications.png)
## <a name="event-triggers"></a>Desencadenadores de eventos
En la tabla siguiente se describen todas las posibles combinaciones de EventType + ProvisioningState y sus desencadenadores:

EventType | ProvisioningState | Desencadenador para la notificación
---|---|---
PUT | Accepted | El grupo de recursos administrados se ha creado y proyectado correctamente después de la aplicación. (Antes de que se inicie la implementación dentro del RG administrado).
PUT | Succeeded | El aprovisionamiento completo de la aplicación administrada se realizó correctamente después de un PUT.
PUT | Con error | Error al poner el aprovisionamiento de la instancia de aplicación en cualquier momento.
PATCH | Succeeded | Después de revisar correctamente la instancia de la aplicación administrada para actualizar las etiquetas, la directiva de acceso JIT o la identidad administrada.
DELETE | Eliminando | En cuanto el usuario inicia una eliminación de una instancia de la aplicación administrada.
DELETE | Deleted | Después de la eliminación completa y correcta de la aplicación administrada.
DELETE | Con error | Después de cualquier error durante el proceso de desaprovisionamiento que bloquea la eliminación.
## <a name="notification-schema"></a>Esquema de la notificación
Al poner en marcha el punto de conexión de webhook para controlar las notificaciones, necesitará analizar la carga para obtener propiedades importantes y, después, actuar sobre la notificación. Tanto las notificaciones de aplicaciones administradas del catálogo de servicios como las de Marketplace proporcionan muchas de las mismas propiedades con la pequeña diferencia que se describe a continuación.

#### <a name="service-catalog-application-notification-schema"></a>Esquema de notificación de aplicación de catálogo de servicios
A continuación se muestra una notificación del catálogo de servicios de ejemplo después de un aprovisionamiento correcto de una instancia de la aplicación administrada.
``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Succeeded",
    "applicationDefinitionId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applicationDefinitions/<appDefName>"    
}

```

Si se produce un error en el aprovisionamiento, se enviará una notificación con los detalles del error al punto de conexión especificado.

``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Failed",
    "applicationDefinitionId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applicationDefinitions/<appDefName>",
    "error": {
        "code": "ErrorCode",
        "message": "error message",
        "details": [
            {
                "code": "DetailedErrorCode",
                "message": "error message"
            }
        ]
    }
}

```

#### <a name="marketplace-application-notification-schema"></a>Esquema de notificación de aplicación de Marketplace

A continuación se muestra una notificación del catálogo de servicios de ejemplo después de un aprovisionamiento correcto de una instancia de la aplicación administrada.
``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Succeeded",
    "billingDetails": {
        "resourceUsageId":"<resourceUsageId>"
    },
    "plan": {
        "publisher": "publisherId",
        "product": "offer",
        "name": "skuName",
        "version": "1.0.1"
    }
}

```

Si se produce un error en el aprovisionamiento, se enviará una notificación con los detalles del error al punto de conexión especificado.

``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Failed",
    "billingDetails": {
        "resourceUsageId":"<resourceUsageId>"
    },
    "plan": {
        "publisher": "publisherId",
        "product": "offer",
        "name": "skuName",
        "version": "1.0.1"
    },
    "error": {
        "code": "ErrorCode",
        "message": "error message",
        "details": [
            {
                "code": "DetailedErrorCode",
                "message": "error message"
            }
        ]
    }
}

```

Parámetro | DESCRIPCIÓN
---|---
eventType | Tipo de evento que desencadenó la notificación. (por ejemplo, "PUT", "PATCH", "DELETE")
applicationId | El identificador de recurso completo de la aplicación administrada para la que se desencadenó la notificación. 
eventTime | Marca de tiempo del evento que desencadenó la notificación. (Fecha y hora en formato UTC ISO 8601).
provisioningState | El estado de aprovisionamiento de la instancia de la aplicación administrada. (por ejemplo, "Succeeded", "Failed", "Deleting", "Deleted")
billingDetails | El estado de facturación de la instancia de la aplicación administrada. Contiene el identificador resourceUsageId que se puede usar para consultar los detalles de uso de Marketplace.
error | *Solo se especifica cuando se produce un error en provisioningState*. Contiene el código de error, el mensaje y los detalles del problema que causó el error.
applicationDefinitionId | *Solo se especifica para las aplicaciones administradas del catálogo de servicios*. Representa el identificador de recurso completo de la definición de la aplicación para la que se aprovisionó la instancia de la aplicación administrada.
plan | *Solo se especifica para las aplicaciones administradas del Marketplace*. Representa el publicador, la oferta, la SKU y la versión de la instancia de la aplicación administrada.

## <a name="endpoint-authentication"></a>Autenticación de punto de conexión
Para proteger el punto de conexión del webhook y garantizar la autenticidad de la notificación:
- Proporcione un parámetro de consulta en la parte superior del URI del webhook, como https://your-endpoint.com?sig=Guid. Con cada notificación, realice una comprobación rápida de que el parámetro de consulta `sig` tiene el valor esperado `Guid`.
- Emita un GET en la instancia de la aplicación administrada con applicationId. Compruebe que el provisioningState coincide con el provisioningState de la notificación para garantizar la coherencia.

## <a name="notification-retries"></a>Reintentos de notificación

El servicio de notificaciones de aplicaciones administradas espera una respuesta `200 OK` del punto de conexión de webhook a la notificación. El servicio de notificación volverá a intentarlo si el punto de conexión de webhook devuelve un código de error HTTP > = 500, 429 o si el punto de conexión no está disponible temporalmente. Si el punto de conexión de webhook no está disponible en 10 horas, se quitará el mensaje de notificación y se detendrán los reintentos.

