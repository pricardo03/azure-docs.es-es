---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 2cdc6ea01e6c3555740102f319d0f4e8e4fc1c22
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121677"
---
## <a name="register-your-application"></a>Registrar su aplicación

Tiene dos opciones para registrar la aplicación y agregar la información de registro de la misma a la solución:

### <a name="option-1-express-mode"></a>Opción 1: Modo rápido

Puede registrar rápidamente la aplicación mediante estos pasos:

1. Vaya al nuevo panel de [Azure Portal: Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs).
1. Escriba un nombre para la aplicación y haga clic en **Registrar**.
1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación en un solo clic.

### <a name="option-2-advanced-mode"></a>Opción 2: Modo avanzado

Para registrar la aplicación y agregar la información de registro de la aplicación a la solución de forma manual, siga estos pasos:

1. Vaya a Visual Studio y:
   1. En el Explorador de soluciones, seleccione el proyecto y observe la ventana Propiedades (si no la ve, presione F4).
   1. Cambie SSL habilitado a `True`.
   1. Haga clic con el botón derecho en el proyecto en Visual Studio y elija **Propiedades** y la pestaña **Web**. En la sección *Servidores*, cambie la *dirección URL del proyecto* para que sea la dirección URL de SSL.
   1. Copie la URL de SSL. En el siguiente paso, agregará esta dirección URL a la lista de direcciones URL de redireccionamiento del Portal de registro correspondiente:<br/><br/>![Propiedades del proyecto](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
1. Seleccione **Nuevo registro**.
1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
   1. En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `ASPNET-Tutorial`.
   1. Agregue la dirección URL de SSL que ha copiado de Visual Studio en el paso 1 (por ejemplo, `https://localhost:44368/`) en **URL de respuesta** y haga clic en **Registrar**.
1. Seleccione el menú **Autenticación**, establezca los **Tokens de identificador** en **Concesión implícita** y, a continuación, seleccione **Guardar**.
1. Agregue lo siguiente en `web.config`, ubicado en la carpeta raíz en la sección `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

1. Reemplace `ClientId` por el identificador de aplicación que acaba de registrar.
1. Reemplace `redirectUri` por la dirección URL de SSL del proyecto.
