---
title: 'Aplicación móvil que llama a las API web (pasar a producción): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación móvil que llama a las API de Web (pasar a producción)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d37d2de561a6f5841bf17a47fef86ad7639750d5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074956"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Aplicación móvil que llama a web API: pasar a producción

Este artículo proporciona detalles sobre cómo mejorar la calidad y la confiabilidad de la aplicación para moverla a producción.

## <a name="handling-errors-in-mobile-applications"></a>Control de errores en aplicaciones móviles

En los diferentes flujos que hasta ahora hemos resaltado, hay una variedad de condiciones de error que puede producirse. El escenario principal para controlar es silenciosas errores y respaldo a la interacción. Hay condiciones adicionales, que también debe tener en cuenta para incluidos ningún situaciones de red, interrupciones de servicio, requerido el consentimiento de administrador y otros casos específicos del escenario de producción.

Cada biblioteca MSAL tiene contenido wiki y de código de ejemplo que profundiza más en estas condiciones de control.

- [MSAL Android Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Mitigar e investigar los problemas

Recopilación de datos le ayudará a la aplicación a diagnosticar problemas. Para obtener más detalles sobre el tipo de datos puede recopilar, ver cada wiki de plataformas MSAL.

- Los usuarios pueden pedir ayuda al encontrar un problema. Es una práctica recomendada capturar y almacenar los registros temporalmente y permitir que los usuarios a cargarlas en algún lugar. MSAL proporciona extensiones de registro para capturar información detallada acerca de la autenticación.
- Si está disponible, habilitar la telemetría a través de MSAL para recopilar datos acerca de cómo los usuarios inician en la aplicación.

## <a name="testing-your-app"></a>Prueba de la aplicación

Asegúrese de probar la aplicación con el [lista de comprobación de integración](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
