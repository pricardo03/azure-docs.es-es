---
title: 'Paso a producción de una aplicación móvil que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web (paso a producción)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1a82fc7dc1b18fa21657170af29f7de7e84d7c1f
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702035"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Aplicación móvil que llama a las API web: paso a producción

En este artículo se proporcionan detalles sobre cómo mejorar la calidad y la confiabilidad de la aplicación antes de moverla a producción.

## <a name="handling-errors-in-mobile-applications"></a>Control de errores en aplicaciones móviles

En este punto, pueden producirse una serie de condiciones de error en la aplicación. Los escenarios principales que se deben controlar son errores silenciosos y retrocesos de interacción. Otras condiciones que se deben considerar para la producción incluyen situaciones sin red, interrupciones de servicio, requisitos para el consentimiento del administrador y otros casos específicos del escenario.

En cada biblioteca MSAL se incluye contenido wiki y de código de ejemplo en los que se describe cómo administrar estas condiciones:

- [MSAL Android Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Mitigación e investigación de problemas

Para diagnosticar problemas en su aplicación, la recopilación de datos sirve de ayuda. Para obtener información sobre los tipos de datos que puede recopilar, consulte las wikis de la plataforma MSAL.

- Los usuarios pueden pedir ayuda cuando encuentran problemas. Es una práctica recomendada capturar registros, almacenarlos temporalmente y proporcionar una ubicación donde los usuarios puedan cargarlos. MSAL proporciona extensiones de registro para capturar información detallada acerca de la autenticación.
- Si está disponible, habilite la telemetría a través de MSAL para recopilar datos acerca de cómo los usuarios inician sesión en la aplicación.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

Pruebe ejemplos adicionales disponibles en [Muestras | Aplicaciones cliente públicas de escritorio y móviles](sample-v2-code.md#desktop-and-mobile-public-client-apps)
