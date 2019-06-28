---
title: Consideraciones sobre Xamarin Android (Microsoft Authentication Library for .NET) | Azure
description: Obtenga información sobre las consideraciones específicas al usar Xamarin Android con Microsoft Authentication Library for .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c57feb33967732481d78e0ddaba5e90f4f82f327
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544440"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Consideraciones específicas de Xamarin Android con MSAL.NET
En este artículo se describen las consideraciones específicas al usar el explorador del sistema en Xamarin Android con Microsoft Authentication Library for .NET (MSAL.NET).

A partir de la versión preliminar de MSAL.NET 2.4.0, MSAL.NET es compatible con los exploradores que no sean Chrome y ya no requiere que Chrome esté instalado en el dispositivo Android para la autenticación.

Se recomienda usar exploradores que admitan pestañas personalizadas, como los siguientes:

| Exploradores con compatibilidad con pestañas personalizadas | Nombre del paquete |
|------| ------- |
|Chrome | com.android.chrome|
|Microsoft Edge | com.microsoft.emmx|
|Firefox | org.mozilla.firefox|
|Ecosia | com.ecosia.android|
|Kiwi | com.kiwibrowser.browser|
|Brave | com.brave.browser|

De acuerdo con nuestras pruebas, además de los exploradores compatibles con las pestañas personalizadas, algunos exploradores que no las admiten también funcionan para la autenticación: Opera, Opera Mini, InBrowser y Maxthon. Para obtener más información, consulte la [tabla de los resultados de pruebas](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Android-system-browser#devices-and-browsers-tested).

## <a name="known-issues"></a>Problemas conocidos

- Si el usuario no tiene ningún explorador habilitado en el dispositivo, MSAL.NET producirá una excepción `AndroidActivityNotFound`. 
  - **Mitigación**: Informe al usuario que debe habilitar un explorador (preferiblemente compatible con las pestañas personalizadas) en el dispositivo.

- Si se produce un error de autenticación (por ejemplo, la autenticación se inicia con DuckDuckGo), MSAL.NET devolverá una `AuthenticationCanceled MsalClientException`. 
  - **Problema raíz**: No se ha habilitado en el dispositivo ningún explorador compatible con las pestañas personalizadas. La autenticación se ha iniciado con un explorador alternativo que no ha podido completarla. 
  - **Mitigación**: Informe al usuario que debe instalar un explorador (preferiblemente compatible con las pestañas personalizadas) en el dispositivo.

## <a name="devices-and-browsers-tested"></a>Dispositivos y exploradores probados
En la tabla siguiente se enumeran los dispositivos y exploradores que se han probado.

| | Browser&ast;     |  Resultado  | 
| ------------- |:-------------:|:-----:|
| Huawei/One+ | Chrome&ast; | Pass (pasado)|
| Huawei/One+ | Edge&ast; | Pass (pasado)|
| Huawei/One+ | Firefox&ast; | Pass (pasado)|
| Huawei/One+ | Brave&ast; | Pass (pasado)|
| One+ | Ecosia&ast; | Pass (pasado)|
| One+ | Kiwi&ast; | Pass (pasado)|
| Huawei/One+ | Opera | Pass (pasado)|
| Huawei | OperaMini | Pass (pasado)|
| Huawei/One+ | InBrowser | Pass (pasado)|
| One+ | Maxthon | Pass (pasado)|
| Huawei/One+ | DuckDuckGo | Autenticación cancelada por el usuario|
| Huawei/One+ | Explorador UC | Autenticación cancelada por el usuario|
| One+ | Dolphin | Autenticación cancelada por el usuario|
| One+ | Explorador CM | Autenticación cancelada por el usuario|
| Huawei/One+ | ninguno instalado | AndroidActivityNotFound p. ej.|

&ast; admite las pestañas personalizadas

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre los fragmentos de código e información adicional sobre el uso del explorador del sistema con Xamarin Android, consulte esta [guía](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid).  