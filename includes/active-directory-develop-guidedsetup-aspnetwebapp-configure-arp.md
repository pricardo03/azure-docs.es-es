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
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 5940195207f85d8011f61336c0318e456c2a8a4c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60300593"
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>Configuración de la aplicación web ASP.NET con la información de registro de la aplicación

En este paso, se configura el proyecto para usar SSL y, a continuación, se usa la dirección URL de SSL para configurar la información de registro de la aplicación. Después, se agrega la información de registro de la aplicación a la solución a través de *web.config*.

1. En el Explorador de soluciones, seleccione el proyecto y fíjese en la ventana `Properties` (si no ve una ventana de propiedades, presione F4).
2. Cambie `SSL Enabled` a `True`.
3. Copie el valor de `SSL URL` anterior y péguelo en el campo `Redirect URL` en la parte superior de esta página; a continuación, haga clic en *Actualizar*:<br/><br/>![Propiedades del proyecto](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
4. Agregue lo siguiente en el archivo `web.config` ubicado en la carpeta raíz, en la sección `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
