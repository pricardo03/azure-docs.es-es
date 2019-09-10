---
title: 'Tutorial: Creación de una aplicación web de página única de Azure Time Series Insights | Microsoft Docs'
description: Aprenda a crear una aplicación web de página única que consulta y representa los datos de un entorno de Azure Time Series Insights.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 08/29/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 031e8074585426584d7ef63a103c9c2b4d90e6c3
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70194235"
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

Regístrese para obtener [suscripción gratuita a Azure](https://azure.microsoft.com/free/), en caso de que no tenga ninguna.

## <a name="prerequisites"></a>Requisitos previos

* Una copia gratuita de Visual Studio. Descargue las [versiones 2017 o 2019 Community](https://www.visualstudio.com/downloads/) para empezar.

* Los componentes de las herramientas principales de IIS Express, Web Deploy y Azure Cloud Services para Visual Studio. Agregue los componentes mediante la modificación de la instalación de Visual Studio.

## <a name="understand-application-design"></a>Descripción del diseño de la aplicación

La SPA de ejemplo de Time Series Insights es la base del diseño y del código que se usan en este tutorial. El código usa la biblioteca cliente JavaScript de Time Series Insights. La biblioteca cliente de Time Series Insights ofrece una abstracción de dos categorías principales de API:

- **Métodos de contenedor para llamar a las API de consulta Time Series Insights**: API REST que puede usar para consultar los datos de Time Series Insights mediante el uso de expresiones basadas en JSON. Los métodos se organizan en el espacio de nombres TsiClient.Server de la biblioteca.

- **Métodos para crear y rellenar varios tipos de controles de gráficos**: Métodos que puede usar para visualizar los datos de Time Series Insights en una página web. Los métodos se organizan en el espacio de nombres TsiClient.UX de la biblioteca.

En este tutorial también se usan datos del entorno de Time Series Insights de la aplicación de ejemplo. Para detalles sobre la estructura de la aplicación de ejemplo de Time Series Insights y cómo usa la biblioteca cliente de Time Series Insights, consulte el tutorial [Exploración de la biblioteca cliente JavaScript de Azure Time Series Insights](tutorial-explore-js-client-lib.md).

## <a name="register-with-azure-ad"></a>Registro con Azure AD

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

## <a name="build-and-publish"></a>Compilación y publicación

1. Cree un directorio para almacenar los archivos de proyecto de la aplicación. Luego, vaya a cada una de las direcciones URL siguientes. Haga clic con el botón derecho del vínculo **Raw** (Sin formato) en la esquina superior derecha de la página y, luego, seleccione **Guardar como** para guardar los archivos en el directorio del proyecto.

   - [*index.html*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): el código HTML y JavaScript de la página.
   - [*sampleStyles.css*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): la hoja de estilos de CSS.

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

      [!code-html[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

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

   1. Para configurar la aplicación para usar el identificador del registro de aplicación de Azure AD, cambie el valor de `clientID` para usar el **identificador de aplicación** que copió en el **paso 3** cuando [registró la aplicación para usar Azure AD](#register-with-azure-ad). Si ha creado una **dirección URL de cierre de sesión** en Azure AD, establezca ese valor como el valor de `postLogoutRedirectUri`.

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

   1. Elija publicar una nueva instancia de Azure App Service o usar una existente.

      [![Selección de una instancia de Azure App Service](media/tutorial-create-tsi-sample-spa/vs-publish-select-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-select-target.png#lightbox)

   1. Seleccione la suscripción que quiere usar para publicar la aplicación. Seleccione el proyecto **TsiSpaApp**. Después, seleccione **Aceptar**.

      [![Visual Studio: el panel App Service del Perfil de publicación](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   1. Seleccione **Publicar** para implementar la aplicación web.

      [![Visual Studio: la opción Publicar y la salida de registro de publicación](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   1. Aparece un registro de publicación satisfactorio en el panel **Salida** de Visual Studio. Una vez finalizada la implementación, Visual Studio abre la aplicación web en una pestaña del explorador y pide iniciar sesión. Al iniciar sesión correctamente, los controles de Time Series Insights se rellenan con datos.

   1. Vaya a la aplicación web e inicie sesión para ver los datos visuales de Time Series Insights representados.

      [![Revisión de la aplicación web hospedada](media/tutorial-create-tsi-sample-spa/vs-publish-hosted-app.png)](media/tutorial-create-tsi-sample-spa/vs-publish-hosted-app.png#lightbox)

## <a name="troubleshoot"></a>Solución de problemas  

Código o condición de error | DESCRIPCIÓN
---------------------| -----------
*AADSTS50011: no hay direcciones de respuesta registradas para la aplicación.* | El registro de Azure AD no tiene una propiedad **URI de redirección**. Vaya a **Autenticación** > **URI de redirección** del registro de la aplicación de Azure AD. Compruebe que exista el **URI de redirección** que tuvo la opción de especificar en el **paso 2** o el **paso 4** cuando [registró la aplicación para usar Azure AD](#register-with-azure-ad).
*AADSTS50011: la dirección URL de respuesta especificada en la solicitud no coincide con las direcciones URL de respuesta configuradas para la aplicación: "\<Identificador único global de la aplicación>".* | El valor de `postLogoutRedirectUri` especificado en el **paso 6.b** de [Creación y publicación de la aplicación web](#build-and-publish) debe coincidir con el valor especificado en **Autenticación** > **URI de redirección** en el registro de aplicación de Azure AD. |
Se carga la aplicación web, pero tiene una página de inicio de sesión de solo texto sin estilo con un fondo blanco. | Compruebe que las rutas de acceso que se describen en el **paso 6** de [Creación y publicación de la aplicación web](#build-and-publish) son correctas. Si la aplicación web no encuentra los archivos .css, no se aplicará el estilo correcto a la página.

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
