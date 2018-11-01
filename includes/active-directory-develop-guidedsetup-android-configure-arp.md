---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 0154aac14168c9d897698a15e31b3124b208db46
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142889"
---
## <a name="add-the-applications-registration-to-your-code"></a>Incorporación del registro de la aplicación al código

En este paso, debe agregar el Id. de la aplicación o del cliente a su proyecto.

1. Abra `MainActivity` (en `app` > `java` > *`{host}.{namespace}`*)
2. Reemplace la línea que empieza con `final static String CLIENT_ID` por:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Abra `app` > `manifests` > `AndroidManifest.xml`
4. Agregue la siguiente actividad a `manifest\application`. `BrowserTabActivity` permite a Microsoft volver a llamar a la aplicación una vez completada la autenticación:

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```
