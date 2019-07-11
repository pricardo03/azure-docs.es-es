---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 050bae64a62e90bdb74c93948109e255b69d7518
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133814"
---
## <a name="register-your-application"></a>Registrar su aplicación

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Si la cuenta proporciona acceso a más de un inquilino, selecciónela en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD que desee utilizar.
1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
1. Cuando se abra la página **Registrar una aplicación**, escriba el nombre de su aplicación.
1. En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
1. En la lista desplegable de la sección **URI de redireccionamiento**, seleccione la plataforma **Web** y, luego, establezca el valor en la dirección URL de la aplicación que se basa en el servidor web. 

   Para información sobre cómo establecer y obtener la dirección URL de redireccionamiento en Visual Studio y Node.js, consulte las dos secciones siguientes.

1. Seleccione **Registrar**.
1. En la página de **información general** de la aplicación, anote el valor del **Identificador de aplicación (cliente)** para su uso posterior.
1. Para esta guía, se requiere que habilite el [flujo de concesión implícita](../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md). En el panel de navegación izquierdo de la aplicación registrada, seleccione **Autenticación**.
1. En **Configuración avanzada**, en **Concesión implícita**, active las casillas **Tokens de id.** y **Tokens de acceso**. Los tokens de identificador y los tokens de acceso son necesarios, ya que esta aplicación tiene que iniciar la sesión de los usuarios y llamar a una API.
1. Seleccione **Guardar**.

> #### <a name="set-a-redirect-url-for-nodejs"></a>Configuración de una dirección URL de redireccionamiento para Node.js
> Para Node.js, puede establecer el puerto del servidor web en el archivo *server.js*. En este tutorial se usa el puerto 30662 como referencia, pero puede usar cualquier otro puerto que esté disponible. 
>
> Para configurar una dirección URL de redireccionamiento en la información de registro de aplicación, vuelva al panel **Registro de aplicación** y realice una de las acciones siguientes:
>
> - Establezca *`http://localhost:30662/`* como la **dirección URL de redireccionamiento**.
> - Si usa un puerto TCP personalizado, use *`http://localhost:<port>/`* (donde *\<port>* es el número de puerto TCP personalizado).
>
> #### <a name="set-a-redirect-url-for-visual-studio"></a>Configuración de una dirección URL de redireccionamiento para Visual Studio
> Para obtener la dirección URL de redireccionamiento de Visual Studio, haga lo siguiente:
> 1. En el **Explorador de soluciones**, seleccione el proyecto.
>
>    Se abre la ventana **Propiedades**. Si no se abre, presione **F4**.
>
>    ![La ventana Propiedades del proyecto JavaScriptSPA](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)
>
> 1. Copie el valor **URL**.
 
> 1. Vuelva al panel **Registro de aplicación** y pegue el valor copiado como la **Dirección URL de redireccionamiento**.

#### <a name="configure-your-javascript-spa"></a>Configuración de JavaScript SPA

1. En el archivo *index.html* creado durante la configuración del proyecto, agregue la información del registro de aplicación. En la parte superior del archivo, dentro de las etiquetas `<script></script>`, agregue el código siguiente:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "<Enter_the_Application_Id_here>",
            authority: "https://login.microsoftonline.com/<Enter_the_Tenant_info_here>"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Donde:
    - *\<Enter_the_Application_Id_here>* es el **identificador de aplicación (cliente)** de la aplicación que ha registrado.
    - *\<Enter_the_Tenant_info_here >* está establecido en una de las opciones siguientes:
       - Si la aplicación admite *solo las cuentas de este directorio organizativo*, reemplace este valor por el **identificador de inquilino** o el **nombre de inquilino** (por ejemplo, *contoso.microsoft.com*).
       - Si la aplicación admite *cuentas en cualquier directorio organizativo*, reemplace este valor por **organizaciones**.
       - Si la aplicación admite *cuentas en cualquier directorio organizativo y cuentas Microsoft personales*, reemplace este valor por **común**. Para restringir la compatibilidad a *Personal Microsoft accounts only* (Solo cuentas Microsoft personales), reemplace este valor por **consumidores**.
