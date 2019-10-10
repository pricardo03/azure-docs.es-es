---
title: Descripción de cómo utilizar Swagger de referencia de Azure Digital Twins | Microsoft Docs
description: Descripción de cómo utilizar la documentación de referencia de Swagger de Azure Digital Twins | Microsoft Docs
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/03/2019
ms.custom: seodec18
ms.openlocfilehash: 06cbdf6fbba89b6666e6fe3162fd591e93dffecd
ms.sourcegitcommit: 7868d1c40f6feb1abcafbffcddca952438a3472d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71958872"
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

[![Parte superior de Swagger](media/how-to-use-swagger/swagger-management-top-img.png)](media/how-to-use-swagger/swagger-management-top-img.png#lightbox)

También se muestran los modelos de objetos de la API de administración.

[![Modelos de Swagger](media/how-to-use-swagger/swagger-management-models-img.png)](media/how-to-use-swagger/swagger-management-models-img.png#lightbox)

Puede seleccionar cada modelo de objetos de la lista para obtener un resumen más detallado de los atributos clave.

[![Modelo de Swagger](media/how-to-use-swagger/swagger-management-model-img.png)](media/how-to-use-swagger/swagger-management-model-img.png#lightbox)

Los modelos de objetos de Swagger generados resultan prácticos para ver todos los [objetos y las API](./concepts-objectmodel-spatialgraph.md) de Azure Digital Twins que están disponibles. Los desarrolladores utilizar recurso al crear soluciones en Azure Digital Twins.

## <a name="endpoint-summary"></a>Resumen de puntos de conexión

Swagger también proporciona un resumen exhaustivo de todos los puntos de conexión que componen las API de administración.

Cada punto de conexión de la lista también incluye la información de solicitud necesaria, como:

* Parámetros obligatorios.
* Tipos de datos de parámetro obligatorios.
* Método HTTP para acceder al recurso.

[![Puntos de conexión de Swagger](media/how-to-use-swagger/swagger-management-endpoints-img.png)](media/how-to-use-swagger/swagger-management-endpoints-img.png#lightbox)

Para ver una descripción más detallada, seleccione cada recurso.

## <a name="use-swagger-to-test-endpoints"></a>Uso de Swagger para probar los puntos de conexión

Una de las funcionalidades más eficaces de Swagger proporciona la capacidad de probar un punto de conexión de la API directamente mediante la interfaz de usuario de la documentación.

Después de seleccionar un punto de conexión específico, verá **Try it out** (Pruébelo).

[![Prueba de Swagger](media/how-to-use-swagger/swagger-management-try-img.png)](media/how-to-use-swagger/swagger-management-try-img.png#lightbox)

Al expandir esa sección se muestran los campos de entrada para cada parámetro obligatorio y opcional. Escriba los valores correctos y seleccione **Ejecución**.

[![Swagger probado](media/how-to-use-swagger/swagger-management-tried-img.png)](media/how-to-use-swagger/swagger-management-tried-img.png#lightbox)

Después de ejecutar la prueba, puede comprobar los datos de respuesta.

## <a name="swagger-response-data"></a>Datos de respuesta de Swagger

Cada punto de conexión en la lista también incluye datos del cuerpo de respuesta para comprobar desarrollo y pruebas. Estos ejemplos incluyen los códigos de estado y JSON que desea ver para las solicitudes correctas de HTTP.

[![Respuesta de Swagger](media/how-to-use-swagger/swagger-management-response-img.png)](media/how-to-use-swagger/swagger-management-response-img.png#lightbox)

Los ejemplos incluyen también los códigos de error para ayudar a depurar o mejorar los errores de las pruebas.

## <a name="swagger-oauth-20-authorization"></a>Autorización de Swagger OAuth 2.0

> [!NOTE]
> * La entidad de seguridad de usuario que creó el recurso de Azure Digital Twins tendrá una asignación del rol Administrador de espacio y podrá crear asignaciones de roles adicionales para otros usuarios. Estos usuarios y sus roles se pueden autorizar para llamar a las API.

1. Siga los pasos de [este inicio rápido](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) o [registre la aplicación de Digital Twins con Azure Active Directory heredado](./how-to-use-legacy-aad.md) para crear y configurar una aplicación de Azure AD. También puede volver a usar un registro de aplicación existente.

1. Agregue la siguiente dirección URL de respuesta al registro de aplicación:

    ```plaintext
    https://YOUR_SWAGGER_URL/ui/oauth2-redirect-html
    ```
    | NOMBRE  | Reemplazar por | Ejemplo |
    |---------|---------|---------|
    | YOUR_SWAGGER_URL | La dirección URL de documentación de API REST de administración que se encuentra en el portal  | `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` |

1. Copie el identificador de la aplicación de Azure AD.

Después de completar el registro de Azure Active Directory:

1. Seleccione el botón **Autorizar** botón en la página de Swagger.

    [![Selección del botón Autorizar de Swagger](media/how-to-use-swagger/swagger-select-authorize-btn.png)](media/how-to-use-swagger/swagger-select-authorize-btn.png#lightbox)

1. Pegue el identificador de la aplicación en el campo **client_id**.

    [![Campo client_id de Swagger](media/how-to-use-swagger/swagger-auth-form.png)](media/how-to-use-swagger/swagger-auth-form.png#lightbox)

1. Se le redirigirá al siguiente cuadro de diálogo modal de operación correcta.

    [![Cuadro de diálogo modal de redirección de Swagger](media/how-to-use-swagger/swagger-auth-redirect-img.png)](media/how-to-use-swagger/swagger-auth-redirect-img.png#lightbox)

Para más información sobre las solicitudes de pruebas interactivas protegidas por OAuth 2.0, consulte la [documentación oficial](https://swagger.io/docs/specification/authentication/oauth2/).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los modelos de objeto de Azure Digital Twins y el grafo de inteligencia espacial, lea [Descripción de los modelos de objetos de Azure Digital Twins](./concepts-objectmodel-spatialgraph.md).

- Para aprender cómo autenticar con Management API, lea [Autenticación con las API](./security-authenticating-apis.md).