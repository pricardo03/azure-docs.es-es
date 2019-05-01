---
title: Descripción de cómo utilizar Swagger de referencia de Azure Digital Twins | Microsoft Docs
description: Descripción de cómo utilizar la documentación de referencia de Swagger de Azure Digital Twins | Microsoft Docs
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/31/2018
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 1746e1d53be01e6c40b5d1948c666960970b75a0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60922995"
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

![Parte superior de Swagger][1]

También se muestran los modelos de objetos de la API de administración.

![Modelos de Swagger][2]

Puede seleccionar cada modelo de objetos de la lista para obtener un resumen más detallado de los atributos clave.

![Modelo de Swagger][3]

Los modelos de objetos de Swagger generados resultan prácticos para ver todos los [objetos y las API](./concepts-objectmodel-spatialgraph.md) de Azure Digital Twins que están disponibles. Los desarrolladores utilizar recurso al crear soluciones en Azure Digital Twins.

## <a name="endpoint-summary"></a>Resumen de puntos de conexión

Swagger también proporciona un resumen exhaustivo de todos los puntos de conexión que componen las API de administración.

Cada punto de conexión de la lista también incluye la información de solicitud necesaria, como:

* Parámetros obligatorios.
* Tipos de datos de parámetro obligatorios.
* Método HTTP para acceder al recurso.

![Puntos de conexión de Swagger][4]

Para ver una descripción más detallada, seleccione cada recurso.

## <a name="use-swagger-to-test-endpoints"></a>Uso de Swagger para probar los puntos de conexión

Una de las funcionalidades más eficaces de Swagger proporciona la capacidad de probar un punto de conexión de la API directamente mediante la interfaz de usuario de la documentación.

Después de seleccionar un punto de conexión específico, verá **Try it out** (Pruébelo).

![Prueba de Swagger][5]

Al expandir esa sección se muestran los campos de entrada para cada parámetro obligatorio y opcional. Escriba los valores correctos y seleccione **Ejecución**.

![Swagger probado][6]

Después de ejecutar la prueba, puede comprobar los datos de respuesta.

## <a name="swagger-response-data"></a>Datos de respuesta de Swagger

Cada punto de conexión en la lista también incluye datos del cuerpo de respuesta para comprobar desarrollo y pruebas. Estos ejemplos incluyen los códigos de estado y JSON que desea ver para las solicitudes correctas de HTTP.

![Respuesta de Swagger][7]

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
    | NOMBRE  | Reemplazar por | Ejemplo |
    |---------|---------|---------|
    | YOUR_SWAGGER_URL | La dirección URL de documentación de API de REST de administración se encuentra en el portal  | `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` |

3. Conceder permisos para la aplicación tener acceso a los gemelos Digital de Azure. En **Permisos necesarios**, escriba `Azure Digital Twins` y seleccione **Permisos delegados**. A continuación, seleccione **Conceder permisos**.

    ![api de incorporación de registros de aplicación de Azure AD](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

4. Configurar el manifiesto de aplicación para permitir flujo implícito de OAuth 2.0. Haga clic en **Manifiesto** para abrir el manifiesto de aplicación para la aplicación. Establezca *oauth2AllowImplicitFlow* en `true`.

    ![Flujo implícito de Azure AD](../../includes/media/digital-twins-permissions/aad-app-allow-implicit-flow.png)

5. Copie el identificador de la aplicación de Azure AD.

6. Haga clic en el botón autorizar en la página swagger.

    ![Botón autorizar de swagger](../../includes/media/digital-twins-permissions/swagger-select-authorize-btn.png)

7. Pegue el identificador de aplicación en el campo client_id.

    ![Campo client_id de swagger](../../includes/media/digital-twins-permissions/swagger-auth-form.png)

    ![Conceder permisos de aplicación de swagger](../../includes/media/digital-twins-permissions/swagger-grant-application-permissions.png)

8. Ahora debería ver el portador pasa el token de autenticación en el encabezado de autorización y la identidad del usuario que ha iniciado sesión aparece en el resultado.

    ![Resultado de token de swagger](../../includes/media/digital-twins-permissions/swagger-token-example.png)

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los modelos de objeto de Azure Digital Twins y el grafo de inteligencia espacial, lea [Descripción de los modelos de objetos de Azure Digital Twins](./concepts-objectmodel-spatialgraph.md).

- Para aprender cómo autenticar con Management API, lea [Autenticación con las API](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.PNG
[2]: media/how-to-use-swagger/swagger_management_models.PNG
[3]: media/how-to-use-swagger/swagger_management_model.PNG
[4]: media/how-to-use-swagger/swagger_management_endpoints.PNG
[5]: media/how-to-use-swagger/swagger_management_try.PNG
[6]: media/how-to-use-swagger/swagger_management_tried.PNG
[7]: media/how-to-use-swagger/swagger_management_response.PNG
