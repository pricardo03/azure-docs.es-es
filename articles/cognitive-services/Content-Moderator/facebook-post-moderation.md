---
title: Moderación de contenido de Facebook con Azure Content Moderator | Microsoft Docs
description: Moderación de páginas de Facebook con Content Moderator basado en aprendizaje automático
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/18/2017
ms.author: sajagtap
ms.openlocfilehash: 316c7212c30e9fe1151cdf5ceabf875439ad4c65
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380335"
---
# <a name="facebook-content-moderation-with-content-moderator"></a>Moderación de contenido de Facebook con Content Moderator

En este tutorial, aprenderá a utilizar Content Moderator basado en aprendizaje automático para ayudar a moderar los comentarios y las publicaciones de Facebook.

Este tutorial le guiará por estos pasos:

1. Crear un equipo de Content Moderator.
2. Crear funciones de Azure que escuchen los eventos HTTP de Content Moderator y Facebook.
3. Crear una aplicación y una página de Facebook y conectarlas a Content Moderator.

Una vez terminado, Facebook enviará el contenido publicado por los visitantes a Content Moderator. En función de los umbrales de coincidencia, los flujos de trabajo de Content Moderator publican el contenido o crean revisiones dentro de la herramienta de revisión. 

La siguiente ilustración muestra los bloques de creación de la solución.

![Moderación de publicaciones de Facebook](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Creación de un equipo de Content Moderator

Consulte la página [Inicio rápido](quick-start.md) para registrarse en Content Moderator y crear un equipo.

## <a name="configure-image-moderation-workflow-threshold"></a>Configuración del flujo de trabajo de moderación de imágenes (umbral)

Consulte la página [Flujos de trabajo](review-tool-user-guide/workflows.md) para configurar un flujo de trabajo de imagen personalizada (umbral). Anote el **nombre** del flujo de trabajo.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Configuración del flujo de trabajo de moderación de texto (umbral)

Use unos pasos similares a los de la página [Flujos de trabajo](review-tool-user-guide/workflows.md) para configurar un umbral y un flujo de trabajo de texto personalizado. Anote el **nombre** del flujo de trabajo.

![Configuración del flujo de trabajo de texto](images/text-workflow-configure.PNG)

Pruebe el flujo de trabajo con el botón "Execute Workflow" (Ejecutar flujo de trabajo).

![Prueba del flujo de trabajo de texto](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Creación de funciones de Azure

Inicie sesión en el [Portal de administración de Azure](https://portal.azure.com/) para crear las funciones de Azure. Siga estos pasos:

1. Cree una aplicación de función de Azure, tal y como se muestra en la página [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal).
2. Abra la aplicación de función recién creada.
3. Dentro de la aplicación, vaya a **Características de la plataforma -> Configuración de la aplicación**.
4. Defina la siguiente [configuración de la aplicación](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> **cm: Region** debe ser el nombre de la región (sin espacios).
> Por ejemplo, **westeurope**, no Europa Occidental; **westcentralus**, no Centro-oeste de EE. UU. y así sucesivamente.
>

| Configuración de aplicación | DESCRIPCIÓN   | 
| -------------------- |-------------|
| cm:TeamId   | Identificador del equipo de Content Moderator.  | 
| cm:SubscriptionKey | Clave de suscripción de Content Moderator. Consulte [Credenciales](/review-tool-user-guide/credentials.md). | 
| cm:Region | Nombre de la región de Content Moderator, sin espacios en blanco. Consulte la nota anterior. |
| cm:ImageWorkflow | Nombre del flujo de trabajo para ejecutar en imágenes. |
| cm:TextWorkflow | Nombre del flujo de trabajo para ejecutar en texto. |
| cm:CallbackEndpoint | Dirección URL de la aplicación de función CMListener que creará más adelante en esta guía. |
| fb:VerificationToken | Token de secreto, que también se usa para suscribirse a los efectos de fuente de Facebook. |
| fb:PageAccessToken | El token de acceso de la API gráfica de Facebook no expira y permite a la función ocultar o eliminar publicaciones en su nombre. |

5. Cree una nueva función **HttpTrigger-CSharp** llamada **FBListener**. Esta función recibe eventos de Facebook. Para crear esta función, siga estos pasos:

    1. Mantenga abierta la página [Creación de funciones de Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) como referencia.
    2. Haga clic en **+** Agregar para crear la nueva función.
    3. En lugar de las plantillas integradas, elija la opción **Get started on your own/custom function** (Empezar una función propia o personalizada).
    4. Haga clic en el icono **HttpTrigger-CSharp**.
    5. Escriba el nombre **FBListener**. El campo **Nivel de autorización** debe establecerse en **Función**.
    6. Haga clic en **Create**(Crear).
    7. Reemplace el contenido de **run.csx** por el contenido de [**FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Cree una nueva función **HttpTrigger-CSharp** llamada **CMListener**. Esta función recibe eventos de Facebook. Siga estos pasos para crear la función.

    1. Mantenga abierta la página [Creación de funciones de Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) como referencia.
    2. Haga clic en **+** Agregar para crear la nueva función.
    3. En lugar de las plantillas integradas, elija la opción **Get started on your own/custom function** (Empezar una función propia o personalizada).
    4. Haga clic en el icono **HttpTrigger-CSharp**.
    5. Escriba el nombre **CMListener**. El campo **Nivel de autorización** debe establecerse en **Función**.
    6. Haga clic en **Create**(Crear).
    7. Reemplace el contenido de **run.csx** por el contenido de [**CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>Configuración de la página y la aplicación de Facebook
1. Cree una aplicación de Facebook.

    1. Vaya al [sitio para desarrolladores de Facebook](https://developers.facebook.com/).
    2. Haga clic en **My Apps** (Mis aplicaciones).
    3. Agregue una nueva aplicación.
    4. Seleccione **Webhooks -> Get iniciado** (Webhooks -> Comenzar)
    5. Seleccione **Page -> Subscribe to this topic** (Página -> Suscribirse a este tema)
    6. Indique la **dirección URL de FBListener** como dirección URL de devolución de llamada, y el **token de comprobación** que configuró en las **opciones de la aplicación de función**.
    7. Una vez suscrito, vaya hasta la fuente y seleccione **Subscribe** (Suscribirse).

2. Cree una página de Facebook.

    1. Vaya a [Facebook](https://www.facebook.com/bookmarks/pages) y cree una **nueva página de Facebook**.
    2. Permita a la app de Facebook acceder a esta página; para ello, siga estos pasos:
        1. Vaya al [Explorador de Graph API](https://developers.facebook.com/tools/explorer/).
        2. Seleccione **Application** (Aplicación).
        3. Seleccione **Page Access Token** (Token de acceso a página) y envíe una solicitud **Get**.
        4. Haga clic en **Page ID** (Id. de página) en la respuesta.
        5. Ahora, anexe **/subscribed_apps** a la dirección URL y envíe una solicitud **Get** (respuesta vacía).
        6. Envíe una solicitud **Post**. Obtendrá la respuesta como **success: true**.

3. Cree un token de acceso a Graph API que no expire.

    1. Vaya al [Explorador de Graph API](https://developers.facebook.com/tools/explorer/).
    2. Seleccione la opción **Application** (Aplicación).
    3. Seleccione la opción **Get User Access Token** (Obtener token de acceso de usuario).
    4. En **Select Permissions** (Seleccionar permisos), seleccione las opciones **manage_pages** y **publish_pages**.
    5. Usaremos el **token de acceso** (token de breve duración) en el paso siguiente.

4. Usamos Postman en los pasos siguientes.

    1. Abra **Postman** (u obténgalo [aquí](https://www.getpostman.com/)).
    2. Importe estos dos archivos:
        1. [Colección de Postman](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Entorno de Postman](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Actualice estas variables de entorno:
    
    | Clave | Valor   | 
    | -------------------- |-------------|
    | appId   | Inserte aquí el identificador de la aplicación de Facebook.  | 
    | appSecret | Inserte aquí el secreto de la aplicación de Facebook. | 
    | short_lived_token | Inserte el token de acceso de usuario de corta duración que generó en el paso anterior. |
    4. Ahora, ejecute las 3 API enumeradas en la colección: 
        1. Seleccione **Generate Long-Lived Access Token** (Generar token de acceso de larga duración) y haga clic en **Send** (Enviar).
        2. Seleccione **Get User ID** (Obtener id. de usuario) y haga clic en **Send** (Enviar).
        3. Seleccione **Get Permanent Page Access Token** (Obtener token de acceso de página permanente) y haga clic en **Send** (Enviar).
    5. Copie el valor de **access_token** de la respuesta y asígnelo a la opción de la aplicación, **fb:PageAccessToken**.

Eso es todo.

La solución envía todas las imágenes y el texto que se publican en la página de Facebook a Content Moderator. Se invocan los flujos de trabajo que configuró anteriormente. El contenido que no supere los criterios definidos en los flujos de trabajo producen revisiones dentro de la herramienta de revisión. El resto del contenido se publica.

## <a name="license"></a>Licencia

Todos los SDK de Microsoft Cognitive Services y los ejemplos tienen una licencia de MIT. Para más información, consulte [LICENCIA](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Código de conducta del desarrollador

Se espera que los desarrolladores que usen Cognitive Services, incluidos este ejemplo y la biblioteca cliente, cumplan con el "Código de conducta del desarrollador de Microsoft Cognitive Services" que se encuentra en http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Pasos siguientes

1. [Vea una demostración (vídeo)](https://channel9.msdn.com/Events/Build/2017/T6033) de esta solución de Microsoft Build 2017.
1. [Ejemplo de Facebook en Github](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
