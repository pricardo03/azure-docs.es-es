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
ms.openlocfilehash: 7a6fa45d0f4ea09fbc84d3185aa0e58db165ac19
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64947370"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Adición de información de registro de la aplicación a su aplicación

En este paso, debe configurar la dirección URL de redireccionamiento de la información de registro de su aplicación y luego agregar el identificador de la aplicación a la aplicación JavaScript SPA.

### <a name="configure-redirect-url"></a>Configuración de la URL de redireccionamiento

Configure el campo `Redirect URL` con la dirección URL de la página index.html basada en el servidor web y luego haga clic en *Actualizar*.

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Instrucciones en Visual Studio para obtener la dirección URL de redireccionamiento
> Siga estos pasos para obtener la dirección URL de redireccionamiento:
> 1. En el **Explorador de soluciones**, seleccione el proyecto y fíjese en la ventana **Propiedades**. Si no ve una ventana **Propiedades**, presione **F4**.
> 2. Copie el valor de **URL** en el Portapapeles:<br/> ![Propiedades del proyecto](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3. Pegue el valor como una **URL de redireccionamiento** en la parte superior de esta página, y seleccione **Actualizar**.

<p>

> #### <a name="setting-redirect-url-for-node"></a>Configuración de la URL de redireccionamiento para Node
> Para Node.js, puede establecer el puerto del servidor web en el archivo *server.js*. En este tutorial se usa el puerto 30662 como referencia, pero puede usar cualquier otro puerto que esté disponible. Siga estas instrucciones para configurar una dirección URL de redireccionamiento en la información de registro de aplicación:<br/>
> Establezca `http://localhost:30662/` como **URL de redireccionamiento** en la parte superior de esta página, o bien use `http://localhost:[port]/` si usa un puerto TCP personalizado (donde *[puerto]* es el número de puerto TCP personalizado) y, luego, haga clic en **Update** (Actualizar).

### <a name="configure-your-javascript-spa-application"></a>Configuración de la aplicación JavaScript SPA

1. En el archivo `index.html` creado durante la instalación del proyecto, agregue la información de registro de aplicación. Agregue el código siguiente en la parte superior, dentro de las etiquetas `<script></script>` en el cuerpo del archivo `index.html`:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    authority: "https://login.microsoftonline.com/common",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
