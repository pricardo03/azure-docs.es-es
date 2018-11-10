---
title: 'Venta de SaaS mediante Azure: API | Microsoft Docs'
description: Explica cómo crear una oferta de SaaS mediante las API de Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 9ffb67a2d3d07e75df29070ca198bac1661f95cc
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212971"
---
<a name="saas-sell-through-azure---apis"></a>Venta de SaaS mediante Azure: API
==============================

En este artículo se explica cómo crear una oferta de SaaS con las API. Las API son necesarias para permitir las suscripciones a la oferta de SaaS si tiene seleccionada la venta mediante Azure. Si quiere crear un listado de SaaS normal sin el comercio habilitado, consulte [Guía de publicación técnica de aplicaciones SaaS](./cloud-partner-portal-saas-offers-tech-publishing-guide.md).

Este artículo se divide en dos secciones:

-   Autenticación de servicio a servicio entre un servicio SaaS y Azure Marketplace
-   Métodos y puntos de conexión de API

Las API siguientes se proporcionan para ayudarle a integrar su servicio SaaS con Azure:

-   Resolver
-   Suscribirse
-   Convert
-   Cancelar suscripción

El siguiente diagrama muestra el flujo de suscripción de un nuevo cliente y cuándo se utilizan estas API:

![Flujo de la API de la oferta de SaaS](media/saas-offer-publish-with-subscription-apis/saas-offer-publish-api-flow.png)

<a name="service-to-service-authentication-between-saas-service-and-azure-marketplace"></a>Autenticación de servicio a servicio entre un servicio SaaS y Azure Marketplace
----------------------------------------------------------------------------

Azure no impone ninguna restricción en la autenticación que el servicio SaaS expone a los usuarios finales. Sin embargo, en cuanto a la comunicación del servicio SaaS con las API de Azure Marketplace, la autenticación se realiza en el contexto de una aplicación de Azure Active Directory (Azure AD).

La siguiente sección describe cómo crear una aplicación de Azure AD.

### <a name="register-an-azure-ad-application"></a>Registro de una aplicación de Azure AD

Cualquier aplicación que quiera usar las funciones de Azure AD debe registrarse primero en un inquilino de Azure AD. Este proceso de registro implica proporcionar a Azure AD los detalles de la aplicación, como la dirección URL donde se encuentra, la dirección URL para enviar respuestas cuando se autentica un usuario, el identificador URI que identifica la aplicación, etc.

Para registrar una aplicación nueva mediante Azure Portal, realice los pasos siguientes:

1.  Inicie sesión en [Azure Portal](https://portal.azure.com/).
2.  Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
3.  En el panel de navegación izquierdo, haga clic en el servicio **Azure Active Directory**, haga clic en **Registros de aplicaciones** y haga clic en **Nuevo registro de aplicación**.

    ![Registros de aplicaciones SaaS de AD](media/saas-offer-publish-with-subscription-apis/saas-offer-app-registration.png)

4.  En la página Crear, escriba la información de registro de la aplicación:
    -   **Nombre**: escriba un nombre de aplicación significativo
    -   **Tipo de aplicación**: 
        - Seleccione **Nativo** para las [aplicaciones cliente](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) que están instaladas localmente en un dispositivo. Esta configuración se utiliza para [clientes nativos](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#native-client) públicos de OAuth.
        - Seleccione **Aplicación web/API** para las [aplicaciones cliente](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) y las [aplicaciones de recursos/API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#resource-server) que están instaladas en un servidor seguro. Esta configuración se utiliza para [clientes web](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#web-client) confidenciales de OAuth y [clientes basados en agente de usuario](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client) públicos.
        La misma aplicación también puede exponer tanto un cliente como un recurso o API.
    -   **Dirección URL de inicio de sesión**: para aplicaciones "Aplicación web/API", proporcione la dirección URL base de la aplicación. Por ejemplo, **http://localhost:31544** podría ser la dirección URL de una aplicación web que se ejecuta en la máquina local. Los usuarios utilizan esta dirección URL para iniciar sesión en una aplicación cliente web.
    -   **URI de redireccionamiento**: para aplicaciones nativas, proporcione el identificador URI usado por Azure AD para devolver las respuestas de los tokens. Escriba un valor específico para la aplicación, por ejemplo, **http://MyFirstAADApp**.

        ![Registros de aplicaciones SaaS de AD](media/saas-offer-publish-with-subscription-apis/saas-offer-app-registration-2.png) Para obtener ejemplos específicos para aplicaciones web o aplicaciones nativas, consulte las guías de inicio rápido de configuraciones que están disponibles en la sección Introducción de la [Guía de desarrolladores de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide#get-started).

5.  Cuando haya terminado, haga clic en **Crear**. Azure AD asigna un identificador de aplicación único a la aplicación y le lleva a la página de registro principal de la aplicación. En función de que se trate de una aplicación web o nativa, se proporcionan opciones diferentes para agregar funcionalidades adicionales a la aplicación.

    **Nota:** De forma predeterminada, la aplicación recién registrada está configurada para permitir solo el inicio de sesión en la aplicación a los usuarios del mismo inquilino.

<a name="api-methods-and-endpoints"></a>Métodos y puntos de conexión de API
-------------------------

Las secciones siguientes describen los métodos de API y los puntos de conexión disponibles para habilitar las suscripciones para una oferta de SaaS.

### <a name="get-a-token-based-on-the-azure-ad-app"></a>Obtención de un token basado en la aplicación de Azure AD

Método HTTP

`GET`

*Request URL* (URL de la solicitud)

**https://login.microsoftonline.com/*{tenantId}*/oauth2/token**

*Parámetro de URI*

|  **Nombre de parámetro**  | **Obligatorio**  | **Descripción**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| tenantId             | True          | Identificador de inquilino de la aplicación de AAD registrada   |
|  |  |  |


*Encabezado de solicitud*

|  **Nombre de encabezado**  | **Obligatorio** |  **Descripción**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  Content-Type     | True         | Tipo de contenido asociado a la solicitud. El valor predeterminado es `application/x-www-form-urlencoded`.  |
|  |  |  |


*Cuerpo de la solicitud*

| **Nombre de propiedad**   | **Obligatorio** |  **Descripción**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | True         | Tipo de concesión. El valor predeterminado es `client_credentials`.                    |
|  Client_id          | True         |  Identificador de cliente o aplicación asociado a la aplicación de Azure AD.                  |
|  client_secret      | True         |  Contraseña asociada a la aplicación de Azure AD.                               |
|  Recurso           | True         |  Recurso de destino para el que se solicita el token. El valor predeterminado es `62d94f6c-d599-489b-a797-3e10e42fbe22`. |
|  |  |  |


*Respuesta*

|  **Nombre**  | **Tipo**       |  **Descripción**    |
| ---------- | -------------  | ------------------- |
| 200 OK    | TokenResponse  | La solicitud se realizó correctamente   |
|  |  |  |

*TokenResponse*

Ejemplo de token de respuesta:

``` json
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "b3cca048-ed2e-406c-aff2-40cf19fe7bf5",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }               
```

### <a name="marketplace-api-endpoint-and-api-version"></a>Punto de conexión de la API de Marketplace y versión de la API

El punto de conexión para la API de Azure Marketplace es `https://marketplaceapi.microsoft.com`.

La versión actual de la API es `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Resolución de la suscripción

La acción POST en el punto de conexión resolve permite a los usuarios resolver un token a un identificador de recurso persistente.

*Solicitud*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Nombre de parámetro** |     **Descripción**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  api-version        |  Versión de la operación que se usará para esta solicitud.   |
|  |  |


*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Sin            | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
| x-ms-correlationid | Sin            | Valor de cadena único para la operación en el cliente. Pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| Content-type       | SÍ          | `application/json`                                        |
| authorization      | SÍ          | Token de portador JSON Web Token (JWT).                    |
| x-ms-marketplace-token| SÍ| Parámetro de consulta del token de la dirección URL cuando se redirige al usuario al sitio web del ISV de SaaS desde Azure. **Nota:** Descodifique como dirección URL el valor del token del explorador antes de usarlo.|
|  |  |  |
  

*Cuerpo de la respuesta*

 ``` json       
    { 
        “id”: “”, 
        “subscriptionName”: “”,
        “offerId”:””, 
         “planId”:””
    }     
```

| **Nombre de parámetro** | **Tipo de datos** | **Descripción**                       |
|--------------------|---------------|---------------------------------------|
| id                 | string        | Identificador de la suscripción de SaaS.          |
| subscriptionName| string| Nombre de la suscripción de SaaS establecida por el usuario en Azure al suscribirse al servicio SaaS.|
| OfferId            | string        | Identificador de la oferta a la que se ha suscrito el usuario. |
| planId             | string        | Identificador del plan al que se ha suscrito el usuario.  |
|  |  |  |


*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token resuelto correctamente.                                                            |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o se ha especificado una versión de API incorrecta. No se pudo resolver el token porque tiene formato incorrecto o ha expirado. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                                 |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                                |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                                        |
|  |  |  |


*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | SÍ          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | SÍ          | Identificador de correlación si lo pasa el cliente; en caso contrario, este valor es el identificador de correlación del servidor.                   |
| x-ms-activityid    | SÍ          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | Sin            | Este valor se establece solo para una respuesta 429.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Suscribirse

El punto de conexión de suscripción permite a los usuarios iniciar una suscripción a un servicio SaaS para un plan determinado y habilitar la facturación en el sistema de comercio.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{identificador_de_suscripción}*?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Identificador único de la suscripción de Saas que se obtiene después de resolver el token mediante la API de resolución.                              |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

|  **Clave del encabezado**        | **Obligatorio** |  **Descripción**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   Sin          | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| x-ms-correlationid     |   Sin          | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| If-Match/If-None-Match |   Sin          |   Valor ETag de validador seguro.                                                          |
| content-type           |   SÍ        |    `application/json`                                                                   |
|  authorization         |   SÍ        |    Token de portador JSON Web Token (JWT).                                               |
| x-ms-marketplace-session-mode| Sin  | Marca para habilitar el modo de simulacro al suscribirse a una oferta de SaaS. Si se establece, no se cargará la suscripción. Esto es útil para escenarios de pruebas del ISV. Se debe establecer en **'dryrun'**|
|  |  |  |

*Cuerpo*

``` json
  { 
      “planId”:””
   }      
```

| **Nombre del elemento** | **Tipo de datos** | **Descripción**                      |
|------------------|---------------|--------------------------------------|
| planId           | Cadena (obligatorio)        | Identificador del plan del servicio SaaS al que se está suscribiendo el usuario.  |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activación de la suscripción SaaS recibida para un plan determinado.                   |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o el cuerpo del elemento JSON tiene un formato incorrecto. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                   |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                  |
| 409                  | `Conflict`           | Otra operación está en curso en la suscripción.                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                  |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                          |
|  |  |  |

En el caso de una respuesta 202, realice un seguimiento del estado de la operación de solicitud en el encabezado "Operation-location". La autenticación es igual que en otras API de Marketplace.

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | SÍ          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | SÍ          | Identificador de correlación si lo pasa el cliente; en caso contrario, este valor es el identificador de correlación del servidor.                   |
| x-ms-activityid    | SÍ          | Valor de cadena único para el seguimiento de la solicitud del servicio. Este valor se utiliza para las reconciliaciones. |
| Retry-After        | SÍ          | Intervalo con el que el cliente puede comprobar el estado.                                                       |
| Operation-Location | SÍ          | Vínculo a un recurso para obtener el estado de la operación.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Punto de conexión de cambio de plan

El punto de conexión de cambio permite al usuario convertir el plan al que actualmente está suscrito a un nuevo plan.

**PATCH**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{identificador_de_suscripción}*?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Identificador de la suscripción de SaaS.                              |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**          | **Obligatorio** | **Descripción**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | Sin            | Valor de cadena único para el seguimiento de la solicitud del cliente. Se recomienda utilizar un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.   |
| x-ms-correlationid      | Sin            | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| If-Match /If-None-Match | Sin            | Valor ETag de validador seguro.                              |
| content-type            | SÍ          | `application/json`                                        |
| authorization           | SÍ          | Token de portador JSON Web Token (JWT).                    |
|  |  |  |


*Cuerpo*

``` json
                { 
                    “planId”:””
                } 
```


|  **Nombre del elemento** |  **Tipo de datos**  | **Descripción**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  Cadena (obligatorio)         | Identificador del plan del servicio SaaS al que se está suscribiendo el usuario.          |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activación de la suscripción SaaS recibida para un plan determinado.                   |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o el cuerpo del elemento JSON tiene un formato incorrecto. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                   |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                  |
| 409                  | `Conflict`           | Otra operación está en curso en la suscripción.                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                  |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                          |
|  |  |  |

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | SÍ          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | SÍ          | Identificador de correlación si lo pasa el cliente; en caso contrario, este valor es el identificador de correlación del servidor.                   |
| x-ms-activityid    | SÍ          | Valor de cadena único para el seguimiento de la solicitud del servicio. Este valor se utiliza para las reconciliaciones. |
| Retry-After        | SÍ          | Intervalo con el que el cliente puede comprobar el estado.                                                       |
| Operation-Location | SÍ          | Vínculo a un recurso para obtener el estado de la operación.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Eliminar suscripción

La acción de eliminación en el punto de conexión de suscripción permite al usuario eliminar una suscripción con un identificador especificado.

*Solicitud*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{identificador_de_suscripción}*?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Identificador de la suscripción de SaaS.                              |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Sin            | Valor de cadena único para el seguimiento de la solicitud del cliente. Se recomienda utilizar un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.                                                           |
| x-ms-correlationid | Sin            | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| authorization      | SÍ          | Token de portador JSON Web Token (JWT).                    |
|  |  |  |
 

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activación de la suscripción SaaS recibida para un plan determinado.                   |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o el cuerpo del elemento JSON tiene un formato incorrecto. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                   |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                  |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                  |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio. Inténtelo de nuevo más tarde.                          |
|  |  |  |

En el caso de una respuesta 202, realice un seguimiento del estado de la operación de solicitud en el encabezado "Operation-location". La autenticación es igual que en otras API de Marketplace.

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | SÍ          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | SÍ          | Identificador de correlación si lo pasa el cliente; en caso contrario, es el identificador de correlación del servidor.                   |
| x-ms-activityid    | SÍ          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | SÍ          | Intervalo con el que el cliente puede comprobar el estado.                                                       |
| Operation-Location | SÍ          | Vínculo a un recurso para obtener el estado de la operación.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>Obtención del estado de la operación

Este punto de conexión permite el usuario realizar un seguimiento del estado de una operación asincrónica desencadenada (Suscribirse / Cancelar suscripción / Cambiar plan).

*Solicitud*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/*{identificador_de_operación}*?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Identificador único de la operación desencadenada.                |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |


*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sin            | Valor de cadena único para el seguimiento de la solicitud del cliente. Se recomienda utilizar un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.   |
| x-ms-correlationid | Sin            | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
| authorization      | SÍ          | Token de portador JSON Web Token (JWT).                    |
|  |  |  | 
  

*Cuerpo de la respuesta*

``` json
  { 
      “id”: “”, 
      “status”:””, 
       “resourceLocation”:””, 
      “created”:””, 
      “lastModified”:”” 
  } 
```

| **Nombre de parámetro** | **Tipo de datos** | **Descripción**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | string        | Identificador de la operación.                                                                      |
| status             | Enum          | Estado de la operación, uno de los siguientes: `In Progress`, `Succeeded` o `Failed`.          |
| resourceLocation   | string        | Vínculo a la suscripción que se ha creado o modificado. Sirve de ayuda al cliente para obtener el estado actualizado de la operación POST. No se establece este valor para las operaciones `Unsubscribe`. |
| created            | Datetime      | Hora de creación de la operación en formato UTC.                                                           |
| lastModified       | Datetime      | Última actualización de la operación en formato UTC.                                                      |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | La solicitud GET se ha resuelto correctamente y el cuerpo contiene la respuesta.    |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o se ha especificado una versión de API incorrecta. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                      |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                     |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                             |
|  |  |  |

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | SÍ          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | SÍ          | Identificador de correlación si lo pasa el cliente; en caso contrario, es el identificador de correlación del servidor.                   |
| x-ms-activityid    | SÍ          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | SÍ          | Intervalo con el que el cliente puede comprobar el estado.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Obtención de la suscripción

La acción Get en el punto de conexión de suscripción permite a un usuario recuperar una suscripción con un identificador de recurso determinado.

*Solicitud*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{identificador_de_suscripción}*?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Identificador de la suscripción de SaaS.                              |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sin            | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.                                                           |
| x-ms-correlationid | Sin            | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| authorization      | SÍ          | Token de portador JSON Web Token (JWT).                                                                    |
|  |  |  |

*Cuerpo de la respuesta*

``` json
  { 
      “id”: “”, 
      “saasSubscriptionName”:””, 
      “offerId”:””, 
       “planId”:””, 
      “saasSubscriptionStatus”:””, 
      “created”:””, 
      “lastModified”: “” 
  }
```
| **Nombre de parámetro**     | **Tipo de datos** | **Descripción**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | string        | Identificador del recurso de la suscripción de SaaS en Azure.    |
| offerId                | string        | Identificador de la oferta a la que se ha suscrito el usuario.         |
| planId                 | string        | Identificador del plan al que se ha suscrito el usuario.          |
| saasSubscriptionName   | string        | Nombre de la suscripción de SaaS.                |
| saasSubscriptionStatus | Enum          | Estado de la operación.  Uno de los siguientes:  <br/> - `Subscribed`: la suscripción está activa.  <br/> - `Pending`: el usuario ha creado el recurso, pero no ha sido activado por el ISV.   <br/> - `Unsubscribed`: el usuario ha cancelado la suscripción.   <br/> - `Suspended`: el usuario ha suspendido la suscripción.   <br/> - `Deactivated`: la suscripción de Azure está suspendida.  |
| created                | Datetime      | Valor de la marca de tiempo de la creación de la suscripción en formato UTC. |
| lastModified           | Datetime      | Valor de la marca de tiempo de la modificación de la suscripción en formato UTC. |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | La solicitud GET se ha resuelto correctamente y el cuerpo contiene la respuesta.    |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o se ha especificado una versión de API incorrecta. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                      |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                     |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                             |
|  |  |  |

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | SÍ          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | SÍ          | Identificador de correlación si lo pasa el cliente; en caso contrario, es el identificador de correlación del servidor.                   |
| x-ms-activityid    | SÍ          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | Sin            | Intervalo con el que el cliente puede comprobar el estado.                                                       |
| eTag               | SÍ          | Vínculo a un recurso para obtener el estado de la operación.                                                        |
|  |  |  |


### <a name="get-subscriptions"></a>Obtención de las suscripciones

La acción Get en el punto de conexión de las suscripciones permite a un usuario recuperar todas las suscripciones de todas las ofertas del ISV.

*Solicitud*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Sin            | Valor de cadena único para el seguimiento de la solicitud del cliente. Se recomienda utilizar un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.             |
| x-ms-correlationid | Sin            | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| authorization      | SÍ          | Token de portador JSON Web Token (JWT).                    |
|  |  |  |


*Cuerpo de la respuesta*

``` json
  { 
      “id”: “”, 
      “saasSubscriptionName”:””, 
      “offerId”:””, 
       “planId”:””, 
      “saasSubscriptionStatus”:””, 
      “created”:””, 
      “lastModified”: “”
  }
```

| **Nombre de parámetro**     | **Tipo de datos** | **Descripción**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | string        | Identificador del recurso de la suscripción de SaaS en Azure.    |
| offerId                | string        | Identificador de la oferta a la que se ha suscrito el usuario.         |
| planId                 | string        | Identificador del plan al que se ha suscrito el usuario.          |
| saasSubscriptionName   | string        | Nombre de la suscripción de SaaS.                |
| saasSubscriptionStatus | Enum          | Estado de la operación.  Uno de los siguientes:  <br/> - `Subscribed`: la suscripción está activa.  <br/> - `Pending`: el usuario ha creado el recurso, pero no ha sido activado por el ISV.   <br/> - `Unsubscribed`: el usuario ha cancelado la suscripción.   <br/> - `Suspended`: el usuario ha suspendido la suscripción.   <br/> - `Deactivated`: la suscripción de Azure está suspendida.  |
| created                | Datetime      | Valor de la marca de tiempo de la creación de la suscripción en formato UTC. |
| lastModified           | Datetime      | Valor de la marca de tiempo de la modificación de la suscripción en formato UTC. |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | La solicitud GET se ha resuelto correctamente y el cuerpo contiene la respuesta.    |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o se ha especificado una versión de API incorrecta. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                      |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                     |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio. Inténtelo de nuevo más tarde.                             |
|  |  |  |

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | SÍ          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | SÍ          | Identificador de correlación si lo pasa el cliente; en caso contrario, es el identificador de correlación del servidor.                   |
| x-ms-activityid    | SÍ          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | Sin            | Intervalo con el que el cliente puede comprobar el estado.                                                       |
|  |  |  |
