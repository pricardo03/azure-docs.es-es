---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 07aac49e7aed7c95863a2058a9de3d1e8f2cd1ad
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060682"
---
## <a name="register-your-application"></a>Registrar su aplicación

Hay numerosas maneras de crear una aplicación, seleccione una de ellas:

### <a name="option-1-register-your-application-express-mode"></a>Opción 1: Registro de la aplicación (modo Rápido)
Ahora tiene que registrar la aplicación en el *Portal de registro de aplicaciones de Microsoft*:

1.  Registre su aplicación en el [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure).
2.  Escriba el nombre de la aplicación y su correo electrónico.
3.  Asegúrese de que está activada la opción **Configuración guiada**.
4.  Siga las instrucciones para obtener el identificador de aplicación y péguelo en el código.

### <a name="option-2-register-your-application-advanced-mode"></a>Opción 2: Registro de la aplicación (modo Avanzado)

1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registrar una aplicación.
2. Escriba el nombre de la aplicación y su correo electrónico.
3. Asegúrese de que la opción **Configuración guiada** está desactivada.
4.  Haga clic en **Agregar plataforma** y, después, seleccione **Web**.
5. Agregue la **URL de redireccionamiento** correspondiente a la dirección URL de la aplicación basada en el servidor web. Consulte las secciones siguientes para obtener instrucciones sobre cómo establecer y obtener la URL de redireccionamiento en Visual Studio y Node.
6. Seleccione **Guardar**.

> #### <a name="setting-redirect-url-for-node"></a>Configuración de la URL de redireccionamiento para Node
> Para Node.js, puede establecer el puerto del servidor web en el archivo *server.js*. En este tutorial se usa el puerto 30662 como referencia, pero se puede usar cualquier otro puerto que esté disponible. Cualquiera sea el caso, siga las instrucciones siguientes para configurar una dirección URL de redireccionamiento en la información de registro de aplicación:<br/>
> - Vuelva al *Portal de registro de aplicaciones* y establezca `http://localhost:30662/` como `Redirect URL`, o utilice `http://localhost:[port]/` si usa un puerto TCP personalizado (donde *[port]* es el número de puerto TCP personalizado) y haga clic en "Guardar".

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Instrucciones en Visual Studio para obtener la dirección URL de redireccionamiento
> Siga estos pasos para obtener la dirección URL de redireccionamiento:
> 1.    En el **Explorador de soluciones**, seleccione el proyecto y fíjese en la ventana **Propiedades**. Si no ve una ventana **Propiedades**, presione **F4**.
> 2.    Copie el valor de **URL** en el Portapapeles:<br/> ![Propiedades del proyecto](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Vuelva al *Portal de registro de aplicaciones* y pegue el valor como **URL de redireccionamiento**; a continuación, haga clic en **Guardar**.


#### <a name="configure-your-javascript-spa"></a>Configuración de JavaScript SPA

1.  En el archivo `index.html` creado durante la instalación del proyecto, agregue la información de registro de aplicación. Agregue el código siguiente en la parte superior, dentro de las etiquetas `<script></script>` en el cuerpo del archivo `index.html`:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
<ol start="3">
<li>
Reemplace <code>Enter the application Id here</code> por el identificador de aplicación que acaba de registrar.
</li>
</ol>
