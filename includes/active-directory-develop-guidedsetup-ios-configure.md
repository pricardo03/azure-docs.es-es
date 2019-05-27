---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: include file
ms.openlocfilehash: a4b0c2e2d7732c6d2a4f57411731f7145cf3e73e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967659"
---
## <a name="register-your-application"></a>Registre su aplicación.

Puede registrar su aplicación de dos maneras distintas, como se describe en las dos secciones siguientes.

### <a name="option-1-express-mode"></a>Opción 1: Modo rápido

Ahora tiene que registrar la aplicación en el *Portal de registro de aplicaciones de Microsoft*:

1. Registre la aplicación en el [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure).
2. Escriba el nombre de la aplicación y su correo electrónico.
3. Asegúrese de que está activada la opción Configuración guiada.
4. Siga las instrucciones para obtener el identificador de aplicación y péguelo en el código.

### <a name="option-2-advanced-mode"></a>Opción 2: Modo avanzado

1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app).
2. Escriba un nombre para la aplicación.
3. Asegúrese de que la opción Configuración guiada está desactivada.
4. Seleccione `Add Platform` y, después, `Native Application`.
5. Seleccione `Save`.
6. Vuelva a Xcode. En `ViewController.swift`, reemplace la línea que empieza por '`let kClientID`' por el identificador de aplicación que acaba de registrar:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Presione Control y haga clic en <code>Info.plist</code> para mostrar el menú contextual y, a continuación, haga clic en: <code>Open As</code> > <code>Source Code</code>
</li>
<li>
Agregue lo siguiente bajo el nodo raíz <code>dict</code>:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Reemplace <i><code>[Your_Application_Id_Here]</code></i> por el identificador de aplicación que acaba de registrar.
</li>
</ol>
