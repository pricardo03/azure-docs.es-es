---
title: Descripción de cómo utilizar Swagger de referencia de Azure Digital Twins | Microsoft Docs
description: Descripción de cómo utilizar la documentación de referencia de Swagger de Azure Digital Twins | Microsoft Docs
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 842c35a1575c7bcf6f547fb04d5680178b3bee78
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730413"
---
# <a name="azure-digital-twins-swagger-reference-documentation"></a>Documentación de referencia de Swagger de Azure Digital Twins

Cada instancia aprovisionada de Azure Digital Twins incluye su propia documentación de referencia de Swagger generada automáticamente.

[Swagger](https://swagger.io/) (u [OpenAPI](https://www.openapis.org/)) reúne información compleja de las API en un recurso de referencia interactivo e independiente del lenguaje. Swagger proporciona material de referencia crítico sobre qué cargas JSON, métodos HTTP y puntos de conexión concretos se deben usar para realizar operaciones en una API.

## <a name="swagger-summary"></a>Resumen de Swagger

Swagger proporciona un resumen interactivo de la API, en el que se incluye:

* Información de la API y del modelo de objetos.
* Los puntos de conexión de API REST con especificación de las cargas de solicitud necesarias, encabezados, parámetros, las rutas de acceso de contexto y los métodos HTTP.
* Pruebas de las funcionalidades de la API.
* Información de respuesta de ejemplo usada para comprobar y confirmar las respuestas HTTP.
* Información de código de error.

Swagger es una herramienta muy útil para ayudar con el desarrollo y las pruebas de las llamadas realizadas a Management API de Azure Digital Twins.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

## <a name="reference-material"></a>Material de referencia

El material de referencia de Swagger generado automáticamente proporciona una introducción rápida a los conceptos importantes, puntos de conexión de API de administración disponibles, así como una descripción de cada modelo de objetos para ayudar en el desarrollo y las pruebas.

Un breve resumen describe la API.

[![Parte superior de swagger](media/how-to-use-swagger/swagger_management_top.PNG)](media/how-to-use-swagger/swagger_management_top.PNG#lightbox)

También se muestran los modelos de objetos de la API de administración.

[![Modelos de swagger](media/how-to-use-swagger/swagger_management_models.PNG)](media/how-to-use-swagger/swagger_management_models.PNG#lightbox)

Puede seleccionar cada modelo de objetos de la lista para obtener un resumen más detallado de los atributos clave.

[![Modelo de swagger](media/how-to-use-swagger/swagger_management_model.PNG)](media/how-to-use-swagger/swagger_management_model.PNG#lightbox)

Los modelos de objetos de Swagger generados resultan prácticos para ver todos los [objetos y las API](./concepts-objectmodel-spatialgraph.md) de Azure Digital Twins que están disponibles. Los desarrolladores utilizar recurso al crear soluciones en Azure Digital Twins.

## <a name="endpoint-summary"></a>Resumen de puntos de conexión

Swagger también proporciona un resumen exhaustivo de todos los puntos de conexión que componen las API de administración.

Cada punto de conexión de la lista también incluye la información de solicitud necesaria, como:

* Parámetros obligatorios.
* Tipos de datos de parámetro obligatorios.
* Método HTTP para acceder al recurso.

[![Puntos de conexión de swagger](media/how-to-use-swagger/swagger_management_endpoints.PNG)](media/how-to-use-swagger/swagger_management_endpoints.PNG#lightbox)

Para ver una descripción más detallada, seleccione cada recurso.

## <a name="use-swagger-to-test-endpoints"></a>Uso de Swagger para probar los puntos de conexión

Una de las funcionalidades más eficaces de Swagger proporciona la capacidad de probar un punto de conexión de la API directamente mediante la interfaz de usuario de la documentación.

Después de seleccionar un punto de conexión específico, verá **Try it out** (Pruébelo).

[![Try swagger](media/how-to-use-swagger/swagger_management_try.PNG)](media/how-to-use-swagger/swagger_management_try.PNG#lightbox)

Al expandir esa sección se muestran los campos de entrada para cada parámetro obligatorio y opcional. Escriba los valores correctos y seleccione **Ejecución**.

[![Ha intentado swagger](media/how-to-use-swagger/swagger_management_tried.PNG)](media/how-to-use-swagger/swagger_management_tried.PNG#lightbox)

Después de ejecutar la prueba, puede comprobar los datos de respuesta.

## <a name="swagger-response-data"></a>Datos de respuesta de Swagger

Cada punto de conexión en la lista también incluye datos del cuerpo de respuesta para comprobar desarrollo y pruebas. Estos ejemplos incluyen los códigos de estado y JSON que desea ver para las solicitudes correctas de HTTP.

[![Respuesta de swagger](media/how-to-use-swagger/swagger_management_response.PNG)](media/how-to-use-swagger/swagger_management_response.PNG#lightbox)

Los ejemplos incluyen también los códigos de error para ayudar a depurar o mejorar los errores de las pruebas.

## <a name="swagger-oauth-20-authorization"></a>Autorización de Swagger OAuth 2.0

Para más información sobre las solicitudes de pruebas interactivas protegidas por OAuth 2.0, consulte la [documentación oficial](https://swagger.io/docs/specification/authentication/oauth2/).

> [!NOTE]
> La entidad de usuario que creó el recurso Digital gemelos de Azure tendrá una asignación de roles de administrador de espacio y podrá crear asignaciones de roles adicionales para otros usuarios.

1. Siga los pasos de [este inicio rápido](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) para crear una aplicación de Azure AD de tipo ***aplicación Web / API***. O bien, puede volver a usar un registro de aplicación existente.

2. Agregue la siguiente dirección url de respuesta para el registro de aplicación:

    ```plaintext
    https://YOUR_SWAGGER_URL/ui/oauth2-redirect-html
    ```
    | Name  | Reemplazar por | Ejemplo |
    |---------|---------|---------|
    | YOUR_SWAGGER_URL | La dirección URL de documentación de API de REST de administración se encuentra en el portal  | `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` |

3. Conceder permisos para la aplicación tener acceso a los gemelos Digital de Azure. En **Permisos necesarios**, escriba `Azure Digital Twins` y seleccione **Permisos delegados**. A continuación, seleccione **Conceder permisos**.

    ![api de incorporación de registros de aplicación de Azure AD](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

4. Configurar el manifiesto de aplicación para permitir flujo implícito de OAuth 2.0. Haga clic en **Manifiesto** para abrir el manifiesto de aplicación para la aplicación. Establezca *oauth2AllowImplicitFlow* en `true`.

    ![Flujo implícito de Azure AD](../../includes/media/digital-twins-permissions/aad-app-allow-implicit-flow.png)

5. Copie el identificador de la aplicación de Azure AD.

Después de completar el registro de Azure Active Directory:

6. Haga clic en el **Authorize** botón en la página swagger.

    [![Haga clic en el Swagger botón autorizar](media/how-to-use-swagger/swagger-select-authorize-btn.png)](media/how-to-use-swagger/swagger-select-authorize-btn.png#lightbox)

7. Pegue el identificador de aplicación en el **client_id** campo.

    [![Campo client_id de swagger](media/how-to-use-swagger/swagger-auth-form.png)](media/how-to-use-swagger/swagger-auth-form.png#lightbox)

8. Se le redirigirá al éxito siguiente modal.

    [![Modal de redirección de swagger](media/how-to-use-swagger/swagger_auth_redirect.PNG)](media/how-to-use-swagger/swagger_auth_redirect.PNG#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los modelos de objeto de Azure Digital Twins y el grafo de inteligencia espacial, lea [Descripción de los modelos de objetos de Azure Digital Twins](./concepts-objectmodel-spatialgraph.md).

- Para aprender cómo autenticar con Management API, lea [Autenticación con las API](./security-authenticating-apis.md).