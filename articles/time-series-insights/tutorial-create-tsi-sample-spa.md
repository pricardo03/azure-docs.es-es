---
title: 'Tutorial: Creación de una aplicación web de página única de Azure Time Series Insights | Microsoft Docs'
description: Aprenda a crear una aplicación web de página única que consulta y representa los datos de un entorno de Azure Time Series Insights.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/25/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 5ee6e9aefb235feb28468798c3bd6b107f8c7c49
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244037"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Tutorial: Creación de una aplicación web de página única de Azure Time Series Insights

Este tutorial lo guía por el proceso de creación de su propia aplicación web de página única (SPA) para acceder a los datos de Azure Time Series Insights. 

En este tutorial, aprenderá:

> [!div class="checklist"]
> * El diseño de la aplicación
> * Cómo registrar la aplicación con Azure Active Directory (Azure AD)
> * Cómo crear, publicar y probar la aplicación web

> [!NOTE]
> * El código fuente de este tutorial se encuentra en [GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * La [aplicación cliente de ejemplo](https://insights.timeseries.azure.com/clientsample) de Time Series Insights se hospeda para mostrar la aplicación completa que se usa en este tutorial.

## <a name="prerequisites"></a>Requisitos previos

* Regístrese para obtener [suscripción gratuita a Azure](https://azure.microsoft.com/free/), en caso de que no tenga ninguna.

* Una copia gratuita de Visual Studio. Descargue las [versiones 2017 o 2019 Community](https://www.visualstudio.com/downloads/) para empezar.

* Los componentes de las herramientas principales de IIS Express, Web Deploy y Azure Cloud Services para Visual Studio. Agregue los componentes mediante la modificación de la instalación de Visual Studio.

## <a name="application-design"></a>Diseño de aplicación

La SPA de ejemplo de Time Series Insights es la base del diseño y del código que se usan en este tutorial. El código usa la biblioteca cliente JavaScript de Time Series Insights. La biblioteca cliente de Time Series Insights ofrece una abstracción de dos categorías principales de API:

- **Métodos de contenedor para llamar a las API de consulta Time Series Insights**: API REST que puede usar para consultar los datos de Time Series Insights mediante el uso de expresiones basadas en JSON. Los métodos se organizan en el espacio de nombres TsiClient.Server de la biblioteca.

- **Métodos para crear y rellenar varios tipos de controles de gráficos**: Métodos que puede usar para visualizar los datos de Time Series Insights en una página web. Los métodos se organizan en el espacio de nombres TsiClient.UX de la biblioteca.

En este tutorial también se usan datos del entorno de Time Series Insights de la aplicación de ejemplo. Para detalles sobre la estructura de la aplicación de ejemplo de Time Series Insights y cómo usa la biblioteca cliente de Time Series Insights, consulte el tutorial [Exploración de la biblioteca cliente JavaScript de Azure Time Series Insights](tutorial-explore-js-client-lib.md).

## <a name="register-the-application-with-azure-ad"></a>Registre la aplicación en Azure AD

Antes de compilar la aplicación, debe registrarla con Azure AD. El registro proporciona la configuración de identidad para que la aplicación pueda usar la compatibilidad con OAuth para el inicio de sesión único. OAuth requiere SPA para usar el tipo de concesión de autorización implícita. Puede actualizar la autorización en el manifiesto de aplicación. Un manifiesto de aplicación es una representación de JSON de la configuración de la identidad de la aplicación.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de suscripción de Azure.  
1. Seleccione **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro de aplicaciones**.

   [![Azure Portal: inicio del registro de aplicación de Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png#lightbox)

1. En el panel **Crear**, rellene los parámetros obligatorios.

   Parámetro|DESCRIPCIÓN
   ---|---
   **Nombre** | Escriba un nombre de registro descriptivo.  
   **Tipo de aplicación** | Deje **Aplicación web/API**.
   **URL de inicio de sesión** | Escriba la dirección URL de la página de inicio de sesión (principal) de la aplicación. Como la aplicación se hospedará más adelante en Azure App Service, debe usar una dirección URL del dominio https:\//azurewebsites.net. En este ejemplo, el nombre se basa en el nombre de registro.

   Seleccione **Crear** para crear el nuevo registro de aplicación.

   [![Azure Portal: la opción Crear en el panel del registro de aplicación de Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png#lightbox)

1. Las aplicaciones de recursos proporcionan API REST que las demás aplicaciones pueden usar. Las API también se registran con Azure AD. Las API proporcionan acceso pormenorizado y protegido a las aplicaciones cliente mediante la exposición de *ámbitos*. Como la aplicación llama a la API Azure Time Series Insights, se deben especificar la API y el ámbito. El permiso para la API y el ámbito s concede en tiempo de ejecución. Seleccione **Configuración** > **Permisos requeridos** > **Agregar**.

   [![Azure Portal: la opción Agregar para agregar permisos de Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png#lightbox)

1. En el panel **Agregar acceso de API**, seleccione **1 Seleccionar una API** para especificar la API Azure Time Series Insights. En el panel **Seleccionar una API**, escriba **azure time** en el cuadro de búsqueda. Luego, seleccione **Azure Time Series Insights** en la lista de resultados. Elija **Seleccionar**.

   [![Azure Portal: la opción Buscar para agregar permisos de Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png#lightbox)

1. Para seleccionar un ámbito para la API, en el panel **Agregar acceso de API**, seleccione **2 Seleccionar permisos**. En el panel **Habilitar acceso**, seleccione el ámbito **Access Azure Time Series Insights service** (Acceder al servicio Azure Time Series Insights). Elija **Seleccionar**. Volverá al panel **Agregar acceso de API**. Seleccione **Listo**.

   [![Azure Portal: establecer un ámbito para agregar permisos de Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png#lightbox)

1. En el panel **Permisos requeridos**, ahora se muestra la API Azure Time Series Insights. También necesita brindar permiso previo al consentimiento para que la aplicación acceda a la API y al ámbito, para todos los usuarios. Haga clic en **Conceder permisos** y, a continuación, haga clic en **Sí**.

   [![Azure Portal: la opción Conceder permisos para agregar permisos requeridos de Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png#lightbox)

1. Tal como se explicó anteriormente, también debe actualizar el manifiesto de aplicación. En el menú horizontal que se encuentra en la parte superior del panel (la "ruta de navegación"), seleccione el nombre de la aplicación para volver al panel **Aplicación registrada**. Seleccione **Manifiesto**, cambie la propiedad `oauth2AllowImplicitFlow` a `true` y, luego, seleccione **Guardar**.

   [![Azure Portal: actualizar el manifiesto de Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png#lightbox)

1. En la ruta de navegación, seleccione el nombre de la aplicación para volver al panel **Aplicación registrada**. Copie los valores de la **página principal** y el **id. de la aplicación** correspondientes a la aplicación. Usará estas propiedades más adelante en el tutorial.

   [![Azure Portal: copie los valores de la dirección URL de la página principal y el identificador de la aplicación correspondientes a la aplicación](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png#lightbox)

## <a name="build-and-publish-the-web-application"></a>Creación y publicación de la aplicación

1. Cree un directorio para almacenar los archivos de proyecto de la aplicación. Luego, vaya a cada una de las direcciones URL siguientes. Haga clic con el botón derecho del vínculo **Raw** (Sin formato) en la esquina superior derecha de la página y, luego, seleccione **Guardar como** para guardar los archivos en el directorio del proyecto.

   - [*index.html*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): HTML y JavaScript para la página
   - [*sampleStyles.css*]( https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): hoja de estilo CSS

   > [!NOTE]
   > Según el explorador, es posible que tenga que cambiar las extensiones de archivo a .html o .css antes de guardarlo.

1. Compruebe que los componentes requeridos estén instalados en Visual Studio. Los componentes de las herramientas principales de IIS Express, Web Deploy y Azure Cloud Services para Visual Studio deben estar instalados.

    [![Visual Studio: modificar los componentes instalados](media/tutorial-create-tsi-sample-spa/vs-installation.png)](media/tutorial-create-tsi-sample-spa/vs-installation.png#lightbox)

    > [!NOTE]
    > Su experiencia con Visual Studio puede variar levemente de los ejemplos ilustrados según la versión y los ajustes de configuración.

1. Abra Visual Studio e inicie sesión. Para crear un proyecto para la aplicación web, en el menú **Archivo**, seleccione **Abrir** > **Sitio web**.

    [![Visual Studio: crear una solución nueva](media/tutorial-create-tsi-sample-spa/vs-solution-create.png)](media/tutorial-create-tsi-sample-spa/vs-solution-create.png#lightbox)

1. En el panel **Abrir sitio web**, seleccione el directorio de trabajo donde almacenó los archivos HTML y CSS y, luego, seleccione **Abrir**.

   [![Visual Studio: el menú Archivo, con las opciones Abrir y Sitio web](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png#lightbox)

1. En el menú **Ver** de Visual Studio, seleccione **Explorador de soluciones**. Se abre la solución nueva. Contiene un proyecto de sitio web (icono de globo terráqueo), el que incluye los archivos HTML y CSS.

   [![Visual Studio: la solución nueva en el Explorador de soluciones](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png#lightbox)

1. Antes de publicar la aplicación, deberá modificar los ajustes de configuración en *index.html*.

   1. Quite el comentario de las tres líneas debajo del comentario `"PROD RESOURCE LINKS"` para cambiar las dependencias de DEVELOPMENT a PRODUCTION. Quite el comentario de las tres líneas debajo del comentario `"DEV RESOURCE LINKS"`.

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      Las dependencias deben tener comentarios como en el ejemplo siguiente:

      ```HTML
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css">

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="../../dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="../../dist/tsiclient.css"> -->
      ```

   1. Para configurar la aplicación y usar el identificador de registro de la aplicación de Azure AD, cambie los valores `clientID` y `postLogoutRedirectUri` para usar los valores de **Identificador de aplicación** y **Página principal** que copió en el paso 9 de [Registro de la aplicación en Azure AD](#register-the-application-with-azure-ad).

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Por ejemplo: 

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ```

   1. Cuando termine de realizar modificaciones, guarde *index.html*.

1. Publique la aplicación web en una suscripción de Azure como Azure App Service.  

   > [!NOTE]
   > Varias de las opciones que aparecen en las capturas de pantalla y que se muestran en los pasos siguientes se completar de manera automática con datos de su suscripción de Azure. Cada panel puede tardar unos segundos en cargarse por completo.  

   1. En el Explorador de soluciones, haga clic con el botón derecho en el nodo del proyecto de sitio web y, luego, seleccione **Publicar aplicación web**.  

      [![Visual Studio: seleccione la opción Publicar aplicación web del Explorador de soluciones](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png#lightbox)

   1. Seleccione **Iniciar** para empezar a publicar la aplicación.

      [![Visual Studio: el panel Perfil de publicación](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png#lightbox)

   1. Seleccione la suscripción que quiere usar para publicar la aplicación. Seleccione el proyecto **TsiSpaApp**. Después, seleccione **Aceptar**.

      [![Visual Studio: el panel App Service del Perfil de publicación](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   1. Seleccione **Publicar** para implementar la aplicación web.

      [![Visual Studio: la opción Publicar y la salida de registro de publicación](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   1. Aparece un registro de publicación satisfactorio en el panel **Salida** de Visual Studio. Una vez finalizada la implementación, Visual Studio abre la aplicación web en una pestaña del explorador y pide iniciar sesión. Al iniciar sesión correctamente, los controles de Time Series Insights se rellenan con datos.

## <a name="troubleshoot"></a>Solución de problemas  

Código o condición de error | DESCRIPCIÓN
---------------------| -----------
*AADSTS50011: no hay direcciones de respuesta registradas para la aplicación.* | Al registro de Azure AD le falta la propiedad **Dirección URL de respuesta**. Vaya a **Configuración** > **Direcciones URL** para el registro de aplicación de Azure AD. Compruebe que existe la dirección URL de **cierre de sesión** que se especificó en el paso 3 de [Registro de aplicación en Azure AD](#register-the-application-with-azure-ad).
*AADSTS50011: la dirección URL de respuesta especificada en la solicitud no coincide con las direcciones URL de respuesta configuradas para la aplicación: "\<Identificador único global de la aplicación>".* | El valor `postLogoutRedirectUri` especificado en el paso 6 de [Creación y publicación de la aplicación web](#build-and-publish-the-web-application) debe coincidir con el valor especificado en **Configuración** > **Direcciones de respuesta** en el registro de aplicación de Azure AD. No olvide cambiar también el valor de **Dirección URL de destino** para usar *https* según lo indicado en el paso 5 de [Creación y publicación de la aplicación web](#build-and-publish-the-web-application).
Se carga la aplicación web, pero tiene una página de inicio de sesión de solo texto sin estilo con un fondo blanco. | Compruebe que las rutas de acceso que se describen en el paso 4 de [Creación y publicación de la aplicación web](#build-and-publish-the-web-application) son correctas. Si la aplicación web no encuentra los archivos .css, no se aplicará el estilo correcto a la página.

## <a name="clean-up-resources"></a>Limpieza de recursos

En este tutorial se crean varios servicios de Azure en ejecución. Si no planea finalizar esta serie de tutoriales, se recomienda eliminar todos los recursos para no incurrir en costos innecesarios.

En el menú de la izquierda de Azure Portal:

1. Seleccione **Grupos de recursos** y, después, seleccione el grupo de recursos que ha creado para el entorno de Time Series Insights. En la parte superior de la página, seleccione **Eliminar grupo de recursos**, escriba el nombre del grupo de recursos y seleccione **Eliminar**.
1. Seleccione **Grupos de recursos** y, a continuación, seleccione el grupo de recursos que creó el acelerador de la solución Simulación de dispositivo. En la parte superior de la página, seleccione **Eliminar grupo de recursos**, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió sobre lo siguiente:

> [!div class="checklist"]
> * El diseño de la aplicación
> * Cómo registrar la aplicación con Azure AD
> * Cómo crear, publicar y probar la aplicación web

Este tutorial se integra con Azure AD y usa la identidad del usuario que inició sesión para adquirir un token de acceso. Para información sobre cómo acceder a la API Time Series Insights a través de la identidad de un servicio o una aplicación de demonio, consulte este artículo:

> [!div class="nextstepaction"]
> [Autenticación y autorización de la API Azure Time Series Insights](time-series-insights-authentication-and-authorization.md)
