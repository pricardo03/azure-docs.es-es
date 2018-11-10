---
title: Dominios de eventos de Azure Event Grid
description: Describe cómo se usan los dominios de eventos para administrar temas de Azure Event Grid.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: fe66ca8b8f5b4474290e302f73b35868dce68caa
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669332"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Comprender los dominios de eventos para administrar temas de Event Grid

En este artículo se describe cómo utilizar los dominios de eventos para administrar el flujo de eventos personalizados en sus diversas organizaciones empresariales, clientes o aplicaciones. Use los dominios de eventos para:

* Administrar arquitecturas de eventos multiinquilino a escala.
* Administrar la autenticación y la autorización.
* Crear particiones de los temas sin tener que administrar cada uno individualmente.
* Evitar la publicación individual en cada uno de los puntos de conexión del tema.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]


## <a name="event-domains-overview"></a>Información general sobre dominios de eventos

Un dominio de evento es una herramienta de administración para un gran número de temas de Event Grid relacionados con la misma aplicación. Se puede considerar un metatema que puede contener miles de temas individuales.

Los dominios de eventos hacen que la arquitectura de servicios de Azure, como Storage y Azure IoT Hub, publique sus eventos para que estén disponibles para su uso en su propio sistema. Le permiten publicar eventos en miles de temas. Los dominios también proporcionan control de autenticación y autorización sobre cada tema para que pueda dividir los inquilinos.

### <a name="example-use-case"></a>Ejemplo de caso de uso

Los dominios de eventos se explican más fácilmente con un ejemplo. Supongamos que dirige Contoso Construction Machinery, donde se encarga de fabricar tractores, equipos de excavación y otra maquinaria pesada. Como parte de la gestión de la empresa, envía información en tiempo real a los clientes sobre el mantenimiento de los equipos, el estado de los sistemas, las actualizaciones de los contratos, etc. Toda la información llega a diferentes puntos de conexión, entre los que están su aplicación, los puntos de conexión de los clientes y otras infraestructuras que estos han configurado.

Los dominios de eventos le permiten modelar Contoso Construction Machinery como una única entidad de creación de eventos. Cada uno de sus clientes está representado como un Tema dentro del Dominio/ La autenticación y la autorización se controlan mediante Azure Active Directory. Cada uno de los clientes puede suscribirse a su tema para recibir eventos relacionados. El acceso administrado a través del dominio de eventos garantiza que solo pueden acceder a su tema.

También le ofrece un punto de conexión único, en el que puede publicar todos sus eventos de cliente. Event Grid se encargará de asegurarse de que cada Tema solo esté al tanto de los eventos asignados a su inquilino.

![Ejemplo de Contoso Construction](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>administración de acceso

 Con un dominio, obtendrá un mayor control de autenticación y autorización sobre cada tema a través de Control de acceso basado en roles (RBAC) de Azure. Estos roles pueden usarse para restringir cada arrendatario de la aplicación únicamente a los temas a los que desea concederle acceso.

Control de acceso basado en roles (RBAC) de dominios de eventos funciona del mismo modo que [control de acceso administrado](https://docs.microsoft.com/azure/event-grid/security-authentication#management-access-control) funciona en el resto de Event Grid y Azure. Utilice RBAC para crear y aplicar definiciones de roles personalizados en dominios de eventos.

### <a name="built-in-roles"></a>Roles integrados

Event Grid tiene dos definiciones de roles integradas para facilitar el RBAC:

#### <a name="eventgrid-eventsubscription-contributor-preview"></a>Colaborador de EventGrid EventSubscription (versión preliminar)

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

#### <a name="eventgrid-eventsubscription-reader-preview"></a>Lector de EventGrid EventSubscription (versión preliminar)

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="subscribing-to-topics"></a>Suscribirse a temas

Suscribirse a eventos en un tema dentro de un dominio de eventos es como [crear una suscripción de eventos en un tema personalizado](./custom-event-quickstart.md) o suscribirse a cualquiera de las ofertas de Azure de publicadores de eventos.

### <a name="domain-scope-subscriptions"></a>Suscripciones del ámbito de dominio

Dominios de evento también se permite para las suscripciones del ámbito de dominio. Una suscripción de eventos en un dominio de eventos recibirá todos los eventos enviados al dominio, independientemente del tema al que se envían los eventos. Las suscripciones del ámbito de dominio pueden ser útiles para la administración y la auditoría.

## <a name="publishing-to-an-event-domain"></a>Publicación en un dominio de eventos

Cuando crea un dominio de eventos, se le asigna un punto de conexión de publicación como si hubiera creado un tema de Event Grid. 

Para publicar eventos en cualquier tema en un dominio de eventos, inserte los eventos en el punto de conexión del dominio como [haría para un tema personalizado](./post-to-custom-topic.md). La única diferencia es que debe especificar el tema en el que quiere que se entregue el evento.

Por ejemplo, la siguiente matriz de eventos de publicación enviará eventos con `"id": "1111"` al tema `foo`, mientras que el evento con `"id": "2222"` se enviaría al tema `bar`:

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

Los dominios de eventos controlan la publicación de temas por usted. En lugar de publicar eventos en cada tema que administra de forma individual, puede publicar todos los eventos en el punto de conexión del dominio y Event Grid se ocupará de garantizar que cada evento se envíe al tema adecuado.

## <a name="limits-and-quotas"></a>Límites y cuotas

### <a name="control-plane"></a>Plano de control

Durante la versión preliminar, los dominios de evento se limitarán a 1000 temas en un dominio y a 50 suscripciones de eventos por tema dentro de un dominio. Las suscripciones del ámbito de dominio de eventos también estarán limitadas a 50.

### <a name="data-plane"></a>Plano de datos

Durante la versión preliminar, la capacidad de eventos para un dominio de eventos se limitará a la misma tasa de ingesta de 5000 eventos por segundo a la que se limitan los temas personalizados.

## <a name="pricing"></a>Precios

Durante la versión preliminar, los dominios de eventos tendrán el mismo [precio por operaciones](https://azure.microsoft.com/pricing/details/event-grid/) que el resto de características de Event Grid.

Las operaciones funcionan igual en dominios de eventos que en los temas personalizados. Cada entrada de un evento a un dominio de eventos es una operación, y cada intento de entrega de un evento es una operación.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información acerca de cómo configurar dominios de eventos, crear temas, crear suscripciones a eventos y publicar eventos, vea [Administrar dominios de eventos](./how-to-event-domains.md).