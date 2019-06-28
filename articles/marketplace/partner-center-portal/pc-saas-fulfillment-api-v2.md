---
title: API de suministro de SaaS v2 | Azure Marketplace
description: Explica cómo crear y administrar una oferta de SaaS en AppSource y Azure Marketplace mediante las API de suministro v2 asociadas.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 05/23/2019
ms.author: evansma
ms.openlocfilehash: da23b90e44869dcbd21acf9b2c4e04f30153ae09
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66751783"
---
# <a name="saas-fulfillment-apis-version-2"></a>API de suministro de SaaS versión 2 

En este artículo se describe la API que permite a los proveedores de software independientes (ISV) vender sus aplicaciones de SaaS en AppSource y Azure Marketplace. Esta API es un requisito para las ofertas de SaaS que permiten transacciones en AppSource y Azure Marketplace.

## <a name="managing-the-saas-subscription-lifecycle"></a>Administrar el ciclo de vida de la suscripción a SaaS

El servicio de SaaS de Microsoft administra la compra de una suscripción a SaaS durante todo su ciclo de vida y usa la API de suministro como mecanismo para controlar el suministro real, los cambios en los planes y la eliminación de la suscripción con el ISV. La factura que recibe el cliente se basa en el estado de la suscripción a SaaS que mantiene Microsoft. En el siguiente diagrama se muestran los estados y las operaciones que controlan los cambios entre estados.

![Estados del ciclo de vida de la suscripción a SaaS](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Estados de una suscripción a SaaS

En la tabla siguiente se enumeran los estados de aprovisionamiento de una suscripción a SaaS, e incluye una descripción y un diagrama de secuencias para cada uno (si procede). 

#### <a name="provisioning"></a>Aprovisionamiento

Cuando un cliente inicia una compra, el ISV recibe esta información en un código de autenticación de una página web interactiva del cliente mediante un parámetro de URL. Por ejemplo: `https://contoso.com/signup?token=..`, donde el proveedor de URL de la página de aterrizaje en el centro de partners es `https://contoso.com/signup`. El código de autenticación se puede validar e intercambiar por los detalles de lo que debe aprovisionarse mediante una llamada a la API de resolución.  Cuando el servicio de SaaS finaliza el aprovisionamiento, envía una llamada de activación para indicar que el procesamiento ha finalizado y que se puede facturar al cliente.  En el diagrama siguiente se muestra la secuencia de llamadas API en un escenario de aprovisionamiento.  

![La API hace una llamada para aprovisionar un servicio de SaaS.](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>aprovisionado

Es el estado estable de un servicio de aprovisionamiento.

#### <a name="provisioning-for-update"></a>Aprovisionamiento para actualizar
(desde Marketplace) 

Este estado indica que un servicio existente tiene una actualización pendiente. El cliente puede iniciar esta actualización en el marketplace o en el servicio de SaaS (solo para transacciones directas al cliente). En el siguiente diagrama se muestran las acciones cuando se inicia una actualización desde el marketplace.

![La API hace una llamada cuando se inicia una actualización desde marketplace.](./media/saas-update-api-v2-calls-from-marketplace-a.png)

#### <a name="provisioning-for-update"></a>Aprovisionamiento para actualizar  
(desde el servicio de SaaS) 

En el siguiente diagrama se muestran las acciones cuando el servicio de SaaS inicia una actualización. (La llamada de webhook se sustituye por una actualización de la suscripción que inicia el servicio de SaaS). 

![La API hace una llamada cuando el servicio de SaaS inicia una actualización.](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Suspended

Este estado indica que no se ha recibido el pago de un cliente. En cumplimiento de la directiva, proporcionaremos al cliente un período de gracia antes de dejar de ofrecer la suscripción. Cuando una suscripción está en este estado: 

- Como ISV, puede optar por degradar o bloquear el acceso del usuario al servicio. 
- La suscripción debe permanecer en un estado recuperable que pueda restaurar la funcionalidad completa sin que se pierdan datos ni la configuración. 
- Puede esperar recibir una solicitud de restablecimiento para esta suscripción a través de la API de suministro o una solicitud de desaprovisionamiento al finalizar el período de gracia. 

#### <a name="unsubscribed"></a>Suscripción cancelada 

Las suscripciones adquieren este estado en respuesta a una solicitud explícita del cliente o como respuesta a un impago de cuotas. Se espera que el ISV conserve los datos del cliente durante un período mínimo de X días para llevar a cabo una recuperación si se solicita y que después los elimine. 


## <a name="api-reference"></a>Referencia de API

En esta sección se documentan la *API de suscripción* a SaaS y la *API de operaciones*.  El valor del parámetro `api-version` para la versión 2 de las API es `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Definiciones de parámetros y entidades

En la tabla siguiente se recogen las definiciones de entidades y parámetros comunes utilizados por las API de suministro.

|     Entidad/parámetro     |     Definición                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | Identificador GUID para un recurso de SaaS.  |
| `name`                   | Nombre descriptivo proporcionado por el cliente para este recurso. |
| `publisherId`            | Identificador de cadenas único para cada anunciante, por ejemplo, “contoso”. |
| `offerId`                | Identificador de cadenas único para cada oferta, por ejemplo, “oferta1”.  |
| `planId`                 | Identificador de cadenas único para cada plan/sku, por ejemplo, “plata”. |
| `operationId`            | Identificador GUID para una operación determinada.  |
|  `action`                | La acción realizada en un recurso, ya sea `subscribe`, `unsubscribe`, `suspend`, `reinstate` o `changePlan`, `changeQuantity`, `transfer`.  |
|   |   |

Los identificadores únicos globales ([GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)) son números de 128 bits (hexadecimal de 32) que normalmente se generan automáticamente. 

#### <a name="resolve-a-subscription"></a>Resolver una suscripción 

El punto de conexión de resolución permite al anunciante resolver un token de marketplace a un identificador de recursos persistente. El identificador de recurso es el identificador único para la suscripción de SAAS.  Cuando se redirige a un usuario al sitio web del ISV, la dirección URL contiene un token en los parámetros de consulta. Se espera que el ISV use este token y haga una solicitud para resolverlo. La respuesta contiene el id. de suscripción de SAAS, el nombre, el id. de oferta y el plan exclusivos para el recurso. Este token es válido durante una hora únicamente. 

**Post:<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |

*Encabezados de la solicitud*:
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |  Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) |
|  x-ms-marketplace-token  |  Parámetro de consulta del token de la dirección URL cuando se redirige al usuario al sitio web del ISV de SaaS desde Azure (por ejemplo: `https://contoso.com/signup?token=..`). *Nota:* La URL descodifica el valor del token del explorador antes de usarlo.  |

*Códigos de respuesta*:

Código: 200<br>
Resuelve el token opaco en una suscripción de SaaS.<br>

```json
Response body:
{
    "subscriptionId": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Código: 404<br>
No encontrado

Código: 400<br>
Solicitud incorrecta. Falta el token x-ms-marketplace-token, tiene un formato incorrecto o ha caducado.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.

Código: 500<br>
Internal Server Error

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

### <a name="subscription-api"></a>API de suscripción

La API de suscripción admite las siguientes operaciones de HTTPS: **Get**, **Post**, **Patch** y **Delete**.


#### <a name="list-subscriptions"></a>Lista de suscripciones

Se enumeran todas las suscripciones a SaaS para un anunciante.

**Get:<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  Versión de la operación que se usará para esta solicitud.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Type       |  `application/json`  |
| x-ms-requestid     |  Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
| authorization      |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de respuesta*:

Código: 200 <br/>
Según el token de autenticación, obtiene el anunciante y las suscripciones correspondientes para todas las ofertas del anunciante.<br> Carga de respuesta:<br>

```json
{
  [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

El token de continuación solo estará presente si hay más “páginas” de planes por recuperar. 

Código: 403 <br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual. 

Código: Error de servidor interno 500

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Obtener una suscripción

Obtiene la suscripción de SaaS especificada. Use esta llamada para obtener información sobre la licencia y sobre el plan.

**Get:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Identificador único de la suscripción de Saas que se obtiene después de resolver el token mediante la API de resolución.   |
|  ApiVersion        |   Versión de la operación que se usará para esta solicitud.   |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`  |
|  x-ms-requestid    |  Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de respuesta*:

Código: 200<br>
Obtiene la suscripción a SaaS del identificador<br> Carga de respuesta:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"Contoso Cloud Solution",
        "publisherId": "contoso",
        "offerId": "offer1",
        "planId": "silver",
        "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Código: 404<br>
No encontrado<br> 

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.

Código: 500<br>
Internal Server Error<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Lista de planes disponibles

Use esta llamada para averiguar si hay alguna oferta pública o privada para el anunciante actual.

**Get:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   Versión de la operación que se usará para esta solicitud.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|   x-ms-requestid   |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
|  x-ms-correlationid  | Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Códigos de respuesta*:

Código: 200<br>
Obtiene una lista de los planes disponibles para un cliente.<br>

Cuerpo de la respuesta:

```json
{
    "plans": [{
        "planId": "Platinum001",
        "displayName": "Private platinum plan for Contoso",
        "isPrivate": true
    }]
}
```

Código: 404<br>
No encontrado<br> 

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual. <br> 

Código: 500<br>
Internal Server Error<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Activar una suscripción

**Post:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de Saas que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json`  |
|  x-ms-requestid    | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid  | Valor de cadena único para la operación en el cliente. Esta cadena pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Solicitud:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Códigos de respuesta*:

Código: 200<br>
Activa la suscripción.<br>

Código: 404<br>
No encontrado

Código: 400<br>
Solicitud incorrecta: errores de validación

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.

Código: 500<br>
Internal Server Error

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-plan-on-the-subscription"></a>Cambiar el plan de la suscripción

Actualizar el plan de la suscripción

**Patch:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de Saas que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid  |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.    |
| authorization      |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Carga de la solicitud*:

```json
Request Body:
{
    "planId": "gold"
}
```

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Vínculo a un recurso para obtener el estado de la operación.   |

*Códigos de respuesta*:

Código: 202<br>
Se ha aceptado la solicitud para cambiar el plan. Se espera que el ISV sondee la operación-ubicación para determinar el éxito/error. <br>

Código: 404<br>
No encontrado

Código: 400<br>
Solicitud incorrecta: errores de validación.

>[!Note]
>Solo se puede revisar un plan o cantidad a la vez, pero no ambos. Las ediciones en una suscripción con **Update** no se encuentran en `allowedCustomerOperations`.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.

Código: 500<br>
Internal Server Error

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-quantity-on-the-subscription"></a>Cambiar la cantidad de la suscripción

Actualizar la cantidad de la suscripción

**Patch:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de Saas que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid  |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.    |
| authorization      |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Carga de la solicitud*:

```json
Request Body:
{
    "quantity": 5
}
```

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Vínculo a un recurso para obtener el estado de la operación.   |

*Códigos de respuesta*:

Código: 202<br>
Accepted. Se ha aceptado la solicitud para cambiar la cantidad. Se espera que el ISV sondee la operación-ubicación para determinar si se lleva a cabo con éxito o se produce un error. <br>

Código: 404<br>
No encontrado

Código: 400<br>
Solicitud incorrecta: errores de validación.

>[!Note]
>Solo se puede revisar un plan o cantidad a la vez, pero no ambos. Las ediciones en una suscripción con **Update** no se encuentran en `allowedCustomerOperations`.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.

Código: 500<br>
Internal Server Error

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="delete-a-subscription"></a>Eliminación de una suscripción

Cancele la suscripción y elimine la suscripción especificada.

**Delete:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId> ?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de Saas que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.   |
|  x-ms-correlationid  |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.   |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de respuesta*:

Código: 202<br>
El ISV ha iniciado una llamada para cancelar la suscripción en una suscripción de SaaS.<br>

Código: 404<br>
No encontrado

Código: 400<br>
Eliminar en una suscripción cuando **Delete** no está en `allowedCustomerOperations`.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.

Código: 500<br>
Internal Server Error

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>API de operaciones

La API de operaciones admite las siguientes operaciones Patch y Get.

#### <a name="list-outstanding-operations"></a>Lista de operaciones pendientes 

Se enumeran las operaciones pendientes para el anunciante actual. 

**Get:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   Versión de la operación que se usará para esta solicitud.                |
| subscriptionId     | Identificador único de la suscripción de Saas que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |  Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de respuesta*:

Código: 200<br> Obtiene la lista de operaciones pendientes en una suscripción.<br>
Carga de respuesta:

```json
[{
    "id": "<guid>",  
    "activityId": "<guid>",
    "subscriptionId": "<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```

Código: 404<br>
No encontrado

Código: 400<br>
Solicitud incorrecta: errores de validación

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.

Código: 500<br>
Internal Server Error

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Obtener el estado de la operación

Permite al usuario realizar un seguimiento del estado de la operación asincrónica desencadenada que se ha especificado (Suscribirse / Cancelar suscripción / Cambiar plan / Cambiar cantidad).

**Get:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`   |
|  x-ms-requestid    |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |[Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de respuesta*: Código: 200<br> Obtiene la operación de SaaS pendiente especificada.<br>
Carga de respuesta:

```json
Response body:
{
    "id  ": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}

```

Código: 404<br>
No encontrado

Código: 400<br>
Solicitud incorrecta: errores de validación

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.
 
Código: 500<br> Internal Server Error

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>Actualizar el estado de una operación

Actualiza el estado de una operación para indicar si se ha llevado a cabo con éxito o ha producido error con los valores proporcionados.

**Patch:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de Saas que se obtiene después de resolver el token mediante la API de resolución.  |
|  operationId       | La operación que se está completando. |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     | `application/json`   |
|   x-ms-requestid   |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Carga de la solicitud*:

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Códigos de respuesta*:

Código: 200<br> Llamada para informar del progreso de una operación por parte del ISV. Por ejemplo, esta respuesta podría indicar el cambio de puestos/planes.

Código: 404<br>
No encontrado

Código: 400<br>
Solicitud incorrecta: errores de validación

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud está intentando acceder a una adquisición que no pertenece al anunciante actual.

Código: 409<br>
Conflicto. Por ejemplo, ya se suministra una transacción más reciente.

Código: 500<br> Internal Server Error

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="webhook-on-the-saas-service"></a>Webhook en el servicio de SaaS

El anunciante debe implementar un webhook en este servicio de SaaS para informar de forma proactiva a los usuarios de los cambios en su servicio. Se espera que la API esté autenticada y que la llame el servicio SaaS de Microsof. Se espera que el servicio de SaaS llame a la API de operaciones para validar y autorizar antes de realizar una acción en la notificación de webhook.

```json
{
  "id": "<this is a Guid operation id, you can call operations API with this to get status>",
  "activityId": "<this is a Guid correlation id>",
  "subscriptionId": "<Guid to uniquely identify this resource>",
  "publisherId": "<this is the publisher’s name>",
  "offerId": "<this is the offer name>",
  "planId": "<this is the plan id>",
  "quantity": "<the number of seats, will be null if not per-seat saas offer>",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Unsubscribe",
  "status": "NotStarted"  

}
```
La acción puede ser una de las siguientes: 
- `Subscribe` (cuando se ha activado el recurso)
- `Unsubscribe` (cuando se ha eliminado el recurso)
- `ChangePlan` (cuando se ha completado la operación de cambio de plan)
- `ChangeQuantity` (cuando se ha completado la operación de cambio de cantidad)
- `Suspend` (cuando se ha suspendido el recurso)
- `Reinstate` (cuando se ha recuperado el recurso tras la suspensión)

El estado puede ser uno de los siguientes: <br>
        - NotStarted <br>
        - InProgress <br>
        - Succeeded <br>
        - Failed <br>
        - Conflict <br>

Los estados que requieren acción en una notificación de webhook son Succeeded y Failed. El ciclo de vida de una operación va de NotStarted a un estado final, como Succeeded/Failed/Conflict. Si el estado es NotStarted o InProgress, antes de realizar cualquier acción, siga solicitando el estado a través de la API de la operación GET hasta que la operación alcance un estado final. 

## <a name="mock-api"></a>API simulada

Puede usar nuestras API simuladas para empezar a desarrollar, especialmente para crear prototipos, y a probar proyectos. 

Punto de conexión de host: `https://marketplaceapi.microsoft.com/api` <br/>
Versión de API: `2018-09-15` <br/>
No se requiere autenticación <br/>
Uri de ejemplo: `https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

Las rutas de acceso del punto de conexión de la API son las mismas tanto en las API simuladas como en las API reales, pero las versiones de API son diferentes. La versión de la API simulada es 2018-09-15 y la de la versión de producción es 2018-08-31. 

Todas las llamadas de API de este artículo se pueden realizar en el punto de conexión de host simulado. Puede esperar obtener datos simulados como respuesta. En general, puede esperar obtener datos simulados como respuesta. Las llamadas a los métodos de actualización de la suscripción en la API simulada siempre devuelven 500. 

## <a name="next-steps"></a>Pasos siguientes

Los desarrolladores también pueden recuperar y manipular mediante programación las ofertas de carga de trabajo y los perfiles del anunciante usando las [API de REST de Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
