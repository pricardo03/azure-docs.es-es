---
title: Cómo administrar grandes conjuntos de temas de Azure Event Grid y publicar eventos en ellos con dominios de eventos
description: Muestra cómo crear y administrar temas en Azure Event Grid y cómo publicar eventos en ellos con dominios de eventos.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: d6da1ee603c85556693b145ba17d1e0cd0dfabd7
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51034547"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Administrar temas y publicar eventos con dominios de eventos

En este artículo se muestra cómo:

* Crear un dominio de Event Grid
* Suscribirse a temas
* Enumeración de claves
* Publicar eventos en un dominio

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-event-domain"></a>Crear un dominio de eventos

La creación de un evento de dominio puede realizarse a través de la extensión `eventgrid` para la [CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Una vez creado un dominio, puede usarlo para administrar grandes conjuntos de temas.

```azurecli-interactive
# if you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name>
  -l <location>
```

Una creación correcta devolverá lo siguiente:

```json
{
  "endpoint": "https://<my-domain-name>.westus2-1.eventgrid.azure.net/api/events",
  "id": "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>",
  "inputSchema": "EventGridSchema",
  "inputSchemaMapping": null,
  "location": "westus2",
  "name": "<my-domain-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<my-resource-group>",
  "tags": null,
  "type": "Microsoft.EventGrid/domains"
}
```

Anote `endpoint` y `id`, ya que se requerirán para administrar el dominio y publicar eventos.

## <a name="create-topics-and-subscriptions"></a>Creación de temas y suscripciones

El servicio Event Grid crea y administra automáticamente el tema correspondiente en un dominio basado en la llamada para crear una suscripción de eventos para un tema de dominio. No hay ningún paso más para crear un tema en un dominio. De forma similar, cuando se elimina la última suscripción del evento para un tema, también se elimina el tema.

Suscribirse a un tema en un dominio es lo mismo que suscribirse a cualquier otro recurso de Azure:

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/<my-topic>" \
  --endpoint https://contoso.azurewebsites.net/api/f1?code=code
```

El identificador del recurso proporcionado es el mismo que se devolvió al crear el dominio anteriormente. Para especificar el tema al que quiere suscribirse, agregue `/topics/<my-topic>` al final del identificador del recurso.

Para crear una suscripción de eventos del ámbito de dominio que recibe todos los eventos del dominio, asigne el dominio como `resource-id` sin especificar ningún tema, por ejemplo `/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>`.

Si necesita un punto de conexión de prueba al que suscribir sus eventos, siempre puede implementar una [aplicación web precompilada](https://github.com/Azure-Samples/azure-event-grid-viewer) que muestre los eventos entrantes. Puede enviar los eventos a su sitio web de prueba en `https://<your-site-name>.azurewebsites.net/api/updates`.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Los permisos que se establecen para un tema se almacenan en Azure Active Directory y se deben eliminar explícitamente. Eliminar una suscripción de eventos no revoca el acceso de un usuario para crear suscripciones de eventos si tiene acceso de escritura en un tema.

## <a name="manage-access-to-topics"></a>Administración del acceso a temas

La administración de acceso a temas se realiza mediante [asignación de roles](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli). La asignación de roles usa la comprobación de acceso basada en roles para limitar las operaciones en recursos de Azure a los usuarios autorizados en un determinado ámbito.

Event Grid tiene dos roles integrados que puede utilizar para asignar acceso a determinados usuarios a varios temas dentro de un dominio. Estos roles son `EventGrid EventSubscription Contributor (Preview)`, que permite la creación y eliminación de suscripciones, y `EventGrid EventSubscription Reader (Preview)`, que permite solo enumerar las suscripciones a eventos.

El siguiente comando limitaría `alice@contoso.com` para crear y eliminar suscripciones de eventos solo en el tema `foo`:

```azurecli-interactive
az role assignment create --assignee alice@contoso.com --role "EventGrid EventSubscription Contributor (Preview)" --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/foo
```

Para más información, vea [Event Grid security and authentication](./security-authentication.md) (Seguridad y autenticación de Event Grid):

* Control de acceso de administración
* Tipos de operación
* Crear la definición de rol personalizada

## <a name="publish-events-to-an-event-grid-domain"></a>Publicar eventos en un dominio de Event Grid

Publicar eventos en un dominio es lo mismo que [publicar en un tema personalizado](./post-to-custom-topic.md). La única diferencia es que debe especificar el tema al que quiere que vaya cada evento. La siguiente matriz de eventos enviará eventos con `"id": "1111"` al tema `foo`, mientras que el evento con `"id": "2222"` se enviará al tema `bar`:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Para obtener las claves para usar un dominio:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

Después, use su método favorito para crear una solicitud HTTP POST para publicar los eventos en el dominio de Event Grid.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre los conceptos de alto nivel en dominios de eventos y saber por qué son útiles, consulte [información general conceptual acerca de dominios de eventos](./event-domains.md).