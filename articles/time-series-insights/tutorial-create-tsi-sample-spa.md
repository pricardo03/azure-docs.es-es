---
title: 'Tutorial: Creación de una aplicación web de página única de Azure Time Series Insights | Microsoft Docs'
description: Aprenda a crear una aplicación web de página única que consulta y representa los datos de un entorno de TSI.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/14/2018
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: fe8b6113646589e30ff839c8bd47968138d98b03
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521443"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Tutorial: Creación de una aplicación web de página única de Azure Time Series Insights

Este tutorial le guía por el proceso de creación de su propia aplicación web de página única (SPA) para acceder a los datos de TSI, modelados a partir de la [aplicación de ejemplo de Time Series Insights (TSI)](https://insights.timeseries.azure.com/clientsample). En este tutorial, aprenderá:

> [!div class="checklist"]
> * El diseño de la aplicación
> * Cómo registrar la aplicación en Azure Active Directory (AD)
> * Cómo crear, publicar y probar la aplicación web 

## <a name="prerequisites"></a>Requisitos previos

Regístrese para obtener [suscripción gratuita a Azure:](https://azure.microsoft.com/free/), en caso de no tenga ninguna. 

Si aún no lo ha hecho, tendrá que instalar Visual Studio. Para este tutorial, puede [descargar o instalar la versión gratuita de comunidad o una evaluación gratuita](https://www.visualstudio.com/downloads/).

## <a name="application-design-overview"></a>Introducción al diseño de aplicaciones

Como ya se ha indicado, al aplicación de ejemplo de TSI proporciona la base para el diseño y el código que se usan en este tutorial. El código incluye el uso de la biblioteca de JavaScript del cliente de TSI. La biblioteca del cliente de TSI proporciona una abstracción de dos categorías principales de API:

- **Métodos de contenedor para llamar a las API de consulta de TSI**: API de REST que permiten consultar datos de TSI mediante expresiones basadas en JSON. Los métodos se organizan en el espacio de nombres `TsiClient.server` de la biblioteca.
- **Métodos para crear y rellenar varios tipos de controles de gráficos**: métodos que se usan para visualizar los datos de TSI en una página web. Los métodos se organizan en el espacio de nombres `TsiClient.ux` de la biblioteca.

Este tutorial también utilizará los datos del entorno de TSI de la aplicación de ejemplo. Para más información acerca de la estructura de la aplicación de ejemplo de TSI y su uso de la biblioteca cliente de TSI, consulte el tutorial [Exploración de la biblioteca de cliente JavaScript de Azure Time Series Insights](tutorial-explore-js-client-lib.md).

## <a name="register-the-application-with-azure-ad"></a>Registre la aplicación en Azure AD 

Antes de compilar la aplicación, es preciso registrarla en Azure AD. El registro proporciona la configuración de la identidad de una aplicación, lo que le permite usar la compatibilidad con OAuth para el inicio de sesión único. OAuth requiere varias aplicaciones de página única para usar la concesión de autorización "implícita", que actualizará en el manifiesto de la aplicación. Un manifiesto de aplicación es una representación de JSON de la configuración de la identidad de la aplicación. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) mediante la cuenta de suscripción de Azure.  
1. Seleccione el recurso **Azure Active Directory** en el panel izquierdo, luego **Registros de aplicaciones** y, después, **+ Nuevo registro de aplicaciones**:  
   
   ![Registro de aplicaciones de Azure AD en Azure Portal](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png)

1. En la página **Crear**, rellene los parámetros obligatorios:
   
   Parámetro|DESCRIPCIÓN
   ---|---
   **Nombre** | Especifique un nombre de registro descriptivo.  
   **Tipo de aplicación** | Dado que va a compilar una aplicación web de página única, déjelo como "Aplicación web/API".
   **URL de inicio de sesión** | Escriba la dirección URL de la página principal o inicio de sesión de la aplicación. Dado que la aplicación se hospedará en Azure App Service (más adelante), debe usar una dirección URL dentro del dominio "https:\//azurewebsites.net". En este ejemplo, el nombre se basa en el nombre de registro.

   Cuando termine, haga clic en **Crear** para crear el nuevo registro de aplicación.

   ![Registro de aplicaciones de Azure AD en Azure Portal (creación)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png)

1. Las aplicaciones de recursos proporcionan API REST que usan otras aplicaciones y también se registran en Azure AD. Las API proporcionan acceso pormenorizado y protegido a las aplicaciones cliente mediante la exposición de "ámbitos". Dado que la aplicación va a llamar a la API "Azure Time Series Insights", debe especificar la API y el ámbito, para el que se solicitara o concederá en tiempo de ejecución. Seleccione **Configuración**, luego **Permisos necesarios** y después **+Agregar**:

   ![Azure Portal, Azure AD, agregar permisos](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png)

1. En la página **Agregar acceso de API**, haga clic en **1 Seleccionar una API** para especificar la API de TSI. En la página **Seleccionar una API**, escriba "azure time" en el campo de búsqueda. A continuación, seleccione la API "Azure Time Series Insights" en la lista de resultados y haga clic en **Seleccionar**: 

   ![Azure Portal, Azure AD, agregar permisos: API](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png)

1. Ahora, especifique un ámbito en la API. Vuelva a la página **Agregar acceso de API**, haga clic en **2 Seleccionar permisos**. En la página **Habilitar acceso**, seleccione el ámbito "Access Azure Time Series Insights service" (Acceder al servicio Azure Time Series Insights). Haga clic en **Seleccionar**, con lo que volverá a la página **Agregar acceso de API**, donde debe hacer clic en **Listo**:

   ![Azure Portal, Azure AD, agregar permisos: ámbito](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png)

1. Cuando vuelva a la página **Permisos necesarios**, observe que ahora aparece la API "Azure Time Series Insights". También necesita permiso previo al consentimiento para que la aplicación acceda a la API y el ámbito, para todos los usuarios. Haga clic en el botón **Conceder permisos** de la parte superior y seleccione **Sí**:

   ![Azure Portal, Azure AD, permisos obligatorios: consentimiento](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png)

1. Como ya se ha indicado, también es preciso actualizar el manifiesto de aplicación. Haga clic en el nombre de la aplicación en la ruta de navegación para volver a la página **Aplicación registrada**. Seleccione **Manifiesto**, cambie la propiedad `oauth2AllowImplicitFlow` a `true` y haga clic en **Guardar**:

   ![Manifiesto de actualización de Azure AD en Azure Portal](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png)

1. Por último, haga clic en la ruta de navegación para volver atrás a la página **Aplicación registrada** y copie la dirección URL de la **página principal** y las propiedades de **Id. de aplicación**. Estas propiedades las usará en un paso posterior:

   ![Propiedades de Azure AD en Azure Portal](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png)

## <a name="build-and-publish-the-web-application"></a>Creación y publicación de la aplicación

1. Cree un directorio para almacenar los archivos de proyecto de la aplicación. A continuación, vaya a las siguientes direcciones URL, haga clic con el botón derecho en el vínculo "Raw"(Sin formato) en la parte superior derecha de la página y en "Guardar como" seleccione el directorio del proyecto:

   > [!NOTE]
   > En función del explorador, es posible que tenga que cambiar la extensión de archivo (a HTML o CSS), antes de guardar el archivo.

   - **index.html** HTML y JavaScript de la página https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html
   - **sampleStyles.css:** Hoja de estilos CSS: https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css
    
1. Inicie en Visual Studio e inicie sesión en él para crear un proyecto para la aplicación web. En el menú **Archivo**, seleccione la opción **Abrir**, **Sitio web**. En el cuadro de diálogo **Abrir sitio web**, seleccione el directorio de trabajo en el que haya almacenado los archivos HTML y CSS, y haga clic en **Abrir**:

   ![VS: File open web site](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)

1. Abra el **Explorador de soluciones** en el menú **Vista** de Visual Studio. Debería ver la nueva solución, que contiene un proyecto de sitio web (icono de globo), que contiene los archivos HTML y CSS:

   ![VS: Explorador de soluciones, nueva solución](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)

1. Para poder publicar la aplicación, es preciso actualizar partes del código JavaScript en **index.html**: 

    a. En primer lugar, cambie las rutas de acceso de los archivos de JavaScript y de las hojas de estilo a las que se hacen referencia en el elemento `<head>`. Abra el archivo **index.html** en la solución de Visual Studio y busque las siguientes líneas del código de JavaScript. Quite el comentario de las tres líneas de "PROD RESOURCE LINKS" y convierta en comentarios las tres líneas de "DEV RESOURCE LINKS":
   
      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      Una vez que realice estos cambios el código debería ser similar al siguiente:

      ```javascript
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="sampleStyles.css"></link>
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css"></link>

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="pages/samples/sampleStyles.css"></link>
      <script src="dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="dist/tsiclient.css"></link> -->
      ```

   b. A continuación, cambie la lógica del token de acceso para que use el nuevo registro de la aplicación de Azure AD. Cambie las siguientes variables `clientID` y `postLogoutRedirectUri` respectivamente para usar el identificador de la aplicación y la dirección URL de la página principal que copió en el paso 9 de la sección [Registro de la aplicación en Azure AD](#register-the-application-with-azure-ad):

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Una vez que realice estos cambios el código debería ser similar al siguiente:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ``` 

   c. Haga clic en **index.htm** cuando haya terminado la modificación.

1. A continuación, publique la aplicación web en una suscripción de Azure como instancia de Azure App Service:  

   > [!NOTE]
   > Varios de los campos de los siguientes cuadros de diálogo se rellenan con datos de la suscripción de Azure. Por lo tanto, los cuadros de diálogo pueden tardar unos segundos en cargarse antes de que pueda continuar.  

    a. Haga clic con el botón derecho en el nodo del proyecto del sitio web en el **Explorador de soluciones** y seleccione **Publicar aplicación web**:  

      ![VS: Explorador de soluciones, publicar aplicación web](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)

   b. Seleccione **Microsoft Azure App Service** para crear un destino de publicación:  

      ![VS: perfil de publicación](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)  

   c. Seleccione la suscripción que desee usar para publicar la aplicación y luego haga clic en "Nuevo": 

      ![VS: perfil de publicación: app service](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)  

   d. Los campos del cuadro de diálogo **Crear servicio de aplicaciones** tardan unos segundos en cargarse. Deje que se carguen y, después, modifíquelos:
   
      Campo | DESCRIPCIÓN
      ---|---
      **Nombre de aplicación** | Cambie al nombre del registro de la aplicación de Azure AD que utilizó en el paso 3, en [Registro de la aplicación en Azure AD](#register-the-application-with-azure-ad). 
      **Grupo de recursos** | Mediante el botón **Nuevo...**  cámbielo para que coincida con el valor del campo **Nombre de la aplicación**.
      **Plan de App Service** | Mediante el botón **Nuevo...**  cámbielo para que coincida con el valor del campo **Nombre de la aplicación**.

      Cuando haya terminado, haga clic en **Crear**. El botón **Exportar** de la esquina inferior izquierda se sustituye por "Implementando:" durante varios segundos, mientras se crean los recursos de Azure. Una vez que se creen los recursos, volverá al cuadro de diálogo anterior. 

      ![VS: perfil de publicación: agregar servicio de aplicaciones nuevo](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service-create.png)  

   e. Al volver al cuadro de diálogo **Publicar**, asegúrese de que en **Método de publicación** se ha seleccionado "Web Deploy". Cambiar el valor de **Dirección URL de destino** para que use `https`, en lugar de `http`, con el fin de que coincida con el registro de la aplicación de Azure AD. A continuación, haga clic en "Publicar" para implementar la aplicación web:  

      ![VS: publicar aplicación web: publicar el servicio de aplicaciones](media/tutorial-create-tsi-sample-spa/vs-publish-publish.png)  

   f. Debería ver un registro de publicación correcto en la ventana **Salida** de Visual Studio. Después de que se realice la implementación, Visual Studio también abrirá la aplicación web en una pestaña del explorador y se le solicitará que inicie sesión. Después de haber iniciado sesión correctamente, verá todos los controles de TSI rellenos de datos:  

      [![VS: publicar aplicación web: publicar salida de registro](media/tutorial-create-tsi-sample-spa/vs-publish-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-output.png#lightbox)

      ![Aplicación SPA de TSI: inicio de sesión](media/tutorial-create-tsi-sample-spa/tsispaapp-azurewebsites-net.png)  

## <a name="troubleshooting"></a>solución de problemas  

Código o condición de error | DESCRIPCIÓN
---------------------| -----------
*AADSTS50011: no hay direcciones de respuesta registradas para la aplicación.* | Al registro de Azure AD le falta la propiedad "Dirección URL de respuesta". Vaya a la página **Configuración** / **Direcciones URL de respuesta** para el registro de la aplicación de Azure AD. Compruebe que la dirección URL de **inicio de sesión** especificada en el paso 3 de [Registro de la aplicación en Azure AD](#register-the-application-with-azure-ad) está presente. 
*AADSTS50011: la dirección URL de respuesta especificada en la solicitud no coincide con las direcciones URL de respuesta configuradas para la aplicación: "\<Identificador único global de la aplicación>".* | El valor `postLogoutRedirectUri` especificado en el paso 4.b de [Creación y publicación de la aplicación](#build-and-publish-the-web-application), debe coincidir con el valor especificado en la propiedad **Configuración** / **Direcciones URL de respuesta**del registro de la aplicación de Azure AD. Asegúrese de cambiar también el valor de **Dirección URL de destino** para usar `https`, como en el paso #5.e de [Creación y publicación de la aplicación web](#build-and-publish-the-web-application).
Se carga la aplicación web, pero tiene una página de inicio de sesión de solo texto sin estilo, con un fondo blanco. | Compruebe que las rutas de acceso que se describen en el paso 4.a de [Creación y publicación de la aplicación web](#build-and-publish-the-web-application) son correctos. Si la aplicación web no encuentra los archivos .css, no se aplicará el estilo correcto a la página.

## <a name="clean-up-resources"></a>Limpieza de recursos

En este tutorial se crean varios servicios de Azure en ejecución. Si no planea completar esta serie de tutoriales, es aconsejable que elimine todos los recursos para no incurrir en costos innecesarios. 

En el menú izquierdo de Azure Portal:

1. Haga clic en el icono **Grupos de recursos** y, después, seleccione el grupo de recursos que ha creado para el entorno de TSI. En la parte superior de la página, haga clic en **Eliminar grupo de recursos**, escriba el nombre del grupo de recursos y haga clic en **Eliminar**. 
1. Haga clic en el icono **Grupos de recursos** y, a continuación, seleccione el grupo de recursos que creó el acelerador de la solución Simulación de dispositivo. En la parte superior de la página, haga clic en **Eliminar grupo de recursos**, escriba el nombre del grupo de recursos y haga clic en **Eliminar**. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * El diseño de la aplicación
> * Cómo registrar la aplicación en Azure Active Directory (AD)
> * Cómo crear, publicar y probar la aplicación web 

Este tutorial se integra con Azure AD y utiliza la identidad del usuario que inició sesión para adquirir un token de acceso. Para aprender a acceder a la API TSI mediante la identidad de un servicio o una aplicación demonio, consulte:

> [!div class="nextstepaction"]
> [Autenticación y autorización de la API Azure Time Series Insights](time-series-insights-authentication-and-authorization.md)
