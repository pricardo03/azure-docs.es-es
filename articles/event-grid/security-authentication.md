---
title: Seguridad y autenticación de Azure Event Grid
description: Describe Azure Event Grid y sus conceptos.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: babanisa
ms.openlocfilehash: 2fd8712cbe5d34baed158a56e6f06b6235f5d4b2
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068202"
---
# <a name="event-grid-security-and-authentication"></a>Seguridad y autenticación de Event Grid 

Azure Event Grid tiene tres tipos de autenticación:

* Entrega de eventos de WebHook
* Suscripciones a eventos
* Publicación de temas personalizados

## <a name="webhook-event-delivery"></a>Entrega de eventos de WebHook

Los webhooks son una de las muchas maneras de recibir eventos de Azure Event Grid. Cuando un nuevo evento está preparado, el servicio EventGrid publica una solicitud HTTP en el punto de conexión configurado con el evento en el cuerpo de la solicitud.

Al igual que muchos otros servicios que admiten webhooks, EventGrid requiere que demuestre la "propiedad" de su punto de conexión de Webhook antes de que comience a entregar eventos a ese punto de conexión. Este requisito es para evitar que un punto de conexión confiado se convierta en el punto de conexión objetivo para la entrega de eventos desde EventGrid. Sin embargo, cuando utiliza cualquiera de los tres servicios de Azure enumerados a continuación, la infraestructura de Azure controla automáticamente esta validación:

* Azure Logic Apps,
* Azure Automation,
* Azure Functions para desencadenador de EventGrid.

Si utiliza cualquier otro tipo de punto de conexión, como una función de Azure basada en un desencadenador HTTP, el código del punto de conexión debe participar en un protocolo de enlace de validación con EventGrid. EventGrid admite dos modelos de protocolo de enlace de validación diferentes:

1. **Protocolo de enlace ValidationCode**: en el momento de la creación de la suscripción a eventos, EventGrid publica un "evento de validación de suscripción" en el punto de conexión. El esquema de este evento es similar a cualquier otro EventGridEvent y la parte de datos de este evento incluye una propiedad `validationCode`. Una vez que la aplicación ha comprobado que la solicitud de validación es para una suscripción a eventos esperada, el código de aplicación debe responder devolviendo el código de validación a EventGrid. Este mecanismo del protocolo de enlace se admite en todas las versiones de EventGrid.

2. **Protocolo de enlace ValidationURL (protocolo de enlace manual)**: en algunos casos, puede que no tenga el control del código fuente del punto de conexión para poder implementar el protocolo de enlace ValidationCode. Por ejemplo, si usa un servicio de terceros (como [Zapier](https://zapier.com) o [IFTTT](https://ifttt.com/)), puede que no sea capaz de responder con el código de validación mediante programación. A partir de la versión 2018-05-01-preview, EventGrid ahora admite un protocolo de enlace de validación manual. Si va a crear una suscripción de eventos mediante SDK o herramientas que usan esta nueva versión de API (2018-05-01-preview), EventGrid envía una propiedad `validationUrl` como parte de los datos del evento de validación de suscripción. Para completar el protocolo de enlace, simplemente haga una solicitud GET en esa dirección URL, ya sea a través de un cliente de REST o mediante el explorador web. La dirección URL de validación proporcionada es válida solo durante unos 10 minutos. Durante ese tiempo, el estado de aprovisionamiento de la suscripción al eventos es `AwaitingManualAction`. Si no ha completado la validación manual en 10 minutos, el estado de aprovisionamiento se establece en `Failed`. Tendrá que crear la suscripción de eventos antes de volver a intentar la validación manual.

Este mecanismo de validación manual se encuentra disponible en versión preliminar. Para usarla, debe instalar la [extensión de Event Grid](/cli/azure/azure-cli-extensions-list) para [CLI de Azure](/cli/azure/install-azure-cli). Puede instalarla con `az extension add --name eventgrid`. Si usa la API REST, asegúrese de que está usando `api-version=2018-05-01-preview`.

### <a name="validation-details"></a>Detalles de la validación

* En el momento de crear o actualizar la suscripción a eventos, Event Grid publica un evento de validación de suscripción en el punto de conexión de destino. 
* El evento contiene un valor de encabezado "aeg-event-type: SubscriptionValidation".
* El cuerpo del evento tiene el mismo esquema que otros eventos de Event Grid.
* La propiedad eventType del evento es `Microsoft.EventGrid.SubscriptionValidationEvent`.
* La propiedad de datos del evento incluye una propiedad `validationCode` con una cadena generada aleatoriamente. Por ejemplo, "validationCode: acb13…".
* Si utiliza la versión de API 2018-05-01-preview, los datos del evento también incluyen una propiedad `validationUrl` con una dirección URL para validar manualmente la suscripción.
* La matriz contiene solo el evento de validación. Otros eventos se envían en una solicitud aparte después de devolver el código de validación.
* Los SDK de EventGrid DataPlane tienen clases que corresponden a los datos de evento de validación de suscripción y a la respuesta de validación de suscripción.

En el siguiente ejemplo se muestra un evento SubscriptionValidationEvent de ejemplo:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=B2E34264-7D71-453A-B5FB-B62D0FDC85EE&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1BNqCxBBSSE9OnNSfZM4%2b5H9zDegKMY6uJ%2fO2DFRkwQ%3d"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Para comprobar la propiedad del punto de conexión, devuelva el código de validación en la propiedad validationResponse, como se muestra en el siguiente ejemplo:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

O bien, puede enviar una solicitud GET a la dirección URL de validación para validar la suscripción manualmente. La suscripción a eventos permanece en estado pendiente hasta que se valida.

Puede encontrar el ejemplo en C# que muestra cómo controlar el protocolo de enlace de validación de suscripción en https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs.

### <a name="checklist"></a>Lista de comprobación

Durante la creación de la suscripción a eventos, si ve un mensaje de error, parecido a "Error al intentar validar el punto de conexión proporcionado https://your-endpoint-here". Para más información, visite https://aka.ms/esvalidation"; indica que hay un error en el protocolo de enlace de validación. Para resolver este error, compruebe los siguientes aspectos:

* ¿Tiene control sobre el código de aplicación en punto de conexión de destino? Por ejemplo, si está escribiendo un desencadenador HTTP basado en Azure Function, ¿tiene acceso al código de aplicación para realizar cambios en él?
* Si tiene acceso al código de aplicación, implemente el mecanismo del protocolo de enlace basado en ValidationCode como se muestra en el ejemplo anterior.

* Si no tiene acceso al código de aplicación (por ejemplo, si usa un servicio de terceros que admita webhooks), puede usar el mecanismo del protocolo de enlace manual. Asegúrese de que usa la versión de la API 2018-05-01-preview o posterior (instale la extensión de la CLI de Azure Event Grid) para recibir la propiedad validationUrl en el evento de validación. Para completar el protocolo de enlace de validación manual, obtenga el valor de la propiedad `validationUrl` y visite esa dirección URL en el explorador web. Si la validación es correcta, verá un mensaje en el explorador web que lo indica. Verá que el valor provisioningState de la suscripción del evento es "Succeeded". 

### <a name="event-delivery-security"></a>Seguridad de entrega de eventos

Puede proteger el punto de conexión del webhook mediante la incorporación de parámetros de consulta a la URL del webhook al crear una suscripción a eventos. Establezca uno de estos parámetros de consulta como secreto, como un [token de acceso](https://en.wikipedia.org/wiki/Access_token) con el que el webhook reconozca que el evento viene de Event Grid con permisos válidos. Event Grid incluye estos parámetros de consulta en cada entrega de eventos al webhook.

Al editar la suscripción al evento, los parámetros de consulta no se muestran ni se devuelven, a menos que el parámetro [--include-full-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) se utilice en la [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) de Azure.

Por último, es importante tener en cuenta que Azure Event Grid solo admite puntos de conexión de webhook HTTPS.

## <a name="event-subscription"></a>Suscripción a eventos

Para suscribirse a un evento, debe probar que tiene acceso al origen del evento y al controlador. La demostración de que posee un WebHook se ha tratado en la sección anterior. Si usa un controlador de eventos que no sea un WebHook (como un almacenamiento de cola o un centro de eventos), necesita acceso de escritura a ese recurso. Esta comprobación de permisos impide que un usuario no autorizado envíe eventos al recurso.

Debe tener el permiso **Microsoft.EventGrid/EventSubscriptions/Write** en el recurso que constituya el origen del evento. Necesita este permiso porque está escribiendo una nueva suscripción en el ámbito del recurso. El recurso requerido difiere en función de si se suscribe a un tema del sistema o a un tema personalizado. Ambos tipos se describen en esta sección.

### <a name="system-topics-azure-service-publishers"></a>Temas del sistema (editores del servicio de Azure)

Para temas del sistema, necesita permiso para escribir una nueva suscripción a eventos en el ámbito del recurso que publica el evento. El formato del recurso es: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Por ejemplo, para suscribirse a un evento en una cuenta de almacenamiento denominada **myacct**, necesita el permiso Microsoft.EventGrid/EventSubscriptions/Write en:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Temas personalizados

Para temas personalizados, necesita permiso para escribir una nueva suscripción a eventos en el ámbito del tema de Event Grid. El formato del recurso es: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Por ejemplo, para suscribirse a un tema personalizado denominado **mytopic**, necesita el permiso Microsoft.EventGrid/EventSubscriptions/Write en: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="custom-topic-publishing"></a>Publicación de temas personalizados

Los temas personalizados utilizan la Firma de acceso compartido (SAS) o la autenticación de clave. Se recomienda el uso de SAS, pero la autenticación de clave proporciona programación simple y es compatible con muchos editores de webhook existentes. 

Incluya el valor de autenticación en el encabezado HTTP. Para SAS, use **aeg-sas-token** para el valor del encabezado. Para autenticación de clave, utilice **aeg-sas-key** para el valor del encabezado.

### <a name="key-authentication"></a>Autenticación de clave

La autenticación de clave es la forma más sencilla de autenticación. Utilice el siguiente formato: `aeg-sas-key: <your key>`

Por ejemplo, pasa una clave con:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>Tokens de SAS

Los tokens de SAS para Event Grid incluyen el recurso, un tiempo de expiración y una firma. El formato del token de SAS es: `r={resource}&e={expiration}&s={signature}`.

El recurso es la ruta de acceso del tema de Event Grid al que envía los eventos. Por ejemplo, una ruta de acceso de recurso válida es: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Genere la firma a partir de una clave.

Por ejemplo, un valor **aeg-sas-token** válido es:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

En el ejemplo siguiente se crea un token de SAS para su uso con Event Grid:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Control de acceso de administración

Azure Event Grid permite controlar el nivel de acceso dado a distintos usuarios para realizar diversas operaciones de administración, como enumerar las suscripciones a eventos, crear otras nuevas y generar claves. Event Grid usa el control de acceso basado en roles (RBAC) de Azure.

### <a name="operation-types"></a>Tipos de operación

Azure Event Grid admite las siguientes acciones:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Las tres últimas operaciones devuelven información potencialmente confidencial, la cual se filtra de las operaciones de lectura normales. Se recomienda restringir el acceso a estas operaciones. Se pueden crear roles personalizados con [Azure PowerShell](../role-based-access-control/role-assignments-powershell.md), la [interfaz de la línea de comandos (CLI) de Azure](../role-based-access-control/role-assignments-cli.md) y la [API de REST](../role-based-access-control/role-assignments-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Exigencia de la comprobación de acceso basada en roles (RBAC)

Utilice los pasos siguientes para exigir RBAC para los distintos usuarios:

#### <a name="create-a-custom-role-definition-file-json"></a>Creación de un archivo de definición de rol personalizado (.json)

Las siguientes son definiciones de roles de Event Grid de ejemplo que permiten a los usuarios realizar distintas acciones.

**EventGridReadOnlyRole.json**: Permitir únicamente operaciones de solo lectura.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: Permitir acciones de publicación restringidas pero denegar acciones de eliminación.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: Permite todas las acciones de Event Grid.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

#### <a name="create-and-assign-custom-role-with-azure-cli"></a>Creación y asignación de roles personalizados con la CLI de Azure

Para crear un rol personalizado, use:

```azurecli
az role definition create --role-definition @<file path>
```

Para asignar el rol a un usuario, use:

```azurecli
az role assignment create --assignee <user name> --role "<name of role>"
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una introducción a Event Grid, vea [About Event Grid](overview.md) (Acerca de Event Grid).
