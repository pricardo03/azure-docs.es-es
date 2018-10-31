---
title: Descripción de cómo utilizar Azure Digital Twins Swagger | Microsoft Docs
description: Uso de Azure Digital Twins Swagger
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: adgera
ms.openlocfilehash: dbadc90a206937d4c9f1d7b75a872d93b1a8a587
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323994"
---
# <a name="how-to-use-azure-digital-twins-swagger"></a>Uso de Azure Digital Twins Swagger

Cada instancia aprovisionada de Azure Digital Twins incluye su propia documentación de referencia de Swagger generada automáticamente.

[Swagger](https://swagger.io/) (u [OpenAPI](https://www.openapis.org/)) reúne información compleja de las API en un recurso de referencia interactivo e independiente del lenguaje. En concreto, Swagger proporciona material de referencia crítico sobre qué cargas de JSON, métodos HTTP y puntos de conexión concretos se deben usar para realizar operaciones en una API.

## <a name="swagger-summary"></a>Resumen de Swagger

Swagger proporciona un resumen interactivo de la API, en el que se incluye:

* Información de la API y del modelo de objetos.
* Los puntos de conexión de API REST con especificación de las cargas de solicitud necesarias, encabezados, parámetros, las rutas de acceso de contexto y los métodos HTTP.
* Pruebas de las funcionalidades de la API.
* Información de respuesta de ejemplo para comprobar y confirmar las respuestas HTTP.
* Información de código de error.

Swagger, por tanto, es una herramienta muy útil para ayudar con el desarrollo y pruebas de las llamadas realizadas a la API de administración.

> [!TIP]
> Como referencia se proporciona un preestreno de Swagger para demostrar el conjunto de características de la API.
> Se hospeda en [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Puede acceder a su propia documentación generada de Swagger de la API de administración en:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Nombre del atributo personalizado | Reemplace por |
| --- | --- |
| `yourInstanceName` | El nombre de la instancia de Azure Digital Twins |
| `yourLocation` | La región de servidor en la que está hospedada la instancia |

## <a name="reference-material"></a>Material de referencia

El material de referencia generado automáticamente explica modelos de objetos y conceptos críticos.

Un breve resumen describe la API:

![Parte superior de Swagger][1]

También se muestran los modelos de objetos de la API principal:

![Modelos de Swagger][2]

Puede hacer clic en cada modelo de objetos en la lista para obtener un resumen más detallado de los atributos clave:

![Modelo de Swagger][3]

Los modelos de objetos de Swagger generados resultan prácticos para ver todos los [objetos y las API](./concepts-objectmodel-spatialgraph.md) de Azure Digital Twins que están disponibles. Se trata de un excelente recurso para desarrolladores que pueden usar al compilar soluciones en Azure Digital Twins.

## <a name="endpoint-summary"></a>Resumen de puntos de conexión

Swagger también proporciona un resumen exhaustivo de todos los puntos de conexión que componen la API.

Cada punto de conexión de la lista también incluye la información de solicitud necesaria, como:

* Parámetros obligatorios.
* Tipos de datos de parámetro obligatorios.
* El método HTTP para acceder al recurso.

![Puntos de conexión de Swagger][4]

Se puede hacer clic en cada uno de los recursos para ver una descripción más detallada.

## <a name="using-swagger-to-test-endpoints"></a>Uso de Swagger para probar los puntos de conexión

Una de las funcionalidades más eficaces de Swagger proporciona la capacidad de **prueba** de un punto de conexión de la API directamente mediante la interfaz de usuario de la documentación.

Después de hacer clic en un punto de conexión específico, verá un botón **Try it out** (Pruébelo):

![Prueba de Swagger][5]

Al expandir esa sección de muestran los campos de entrada para cada parámetro obligatorio y opcional. Escriba los valores según corresponda y haga clic en **Ejecutar**:

![Swagger probado][6]

Después de ejecutar la prueba, puede comprobar los datos de respuesta.

## <a name="swagger-response-data"></a>Datos de respuesta de Swagger

Cada punto de conexión en la lista también incluye datos del cuerpo de respuesta para comprobar desarrollo y pruebas. Estos ejemplos incluyen los códigos de estado y JSON deseados para las solicitudes correctas de HTTP.

![Respuesta de Swagger][7]

Los ejemplos incluyen también los códigos de error para ayudar a depurar o mejorar los errores de las pruebas.

## <a name="swagger-oauth-20-authorization"></a>Autorización de Swagger OAuth 2.0

Para probar de forma interactiva solicitudes respecto a los recursos de la API protegidos por OAuth 2.0, consulte la [documentación oficial](https://swagger.io/docs/specification/authentication/oauth2/).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los modelos de objeto de Azure Digital Twins y el grafo de inteligencia espacial, lea [este artículo](./concepts-objectmodel-spatialgraph.md).

Para aprender cómo autenticar con la API de administración, lea [Autenticación con las API](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.png
[2]: media/how-to-use-swagger/swagger_management_models.png
[3]: media/how-to-use-swagger/swagger_management_model.png
[4]: media/how-to-use-swagger/swagger_management_endpoints.png
[5]: media/how-to-use-swagger/swagger_management_try.png
[6]: media/how-to-use-swagger/swagger_management_tried.png
[7]: media/how-to-use-swagger/swagger_management_response.png
