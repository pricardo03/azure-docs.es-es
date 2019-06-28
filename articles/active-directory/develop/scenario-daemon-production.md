---
title: 'Aplicación de demonio que llama a las API web (paso a producción): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación de demonio que llama a las API web (paso a producción)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 627dab0cb23800664c5fb5b3df9c61f5071d4b87
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545400"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Aplicación de demonio que llama a las API web: paso a producción

Ahora que sabe cómo adquirir y usar un token para una llamada de servicio a servicio, aprenda a pasar la aplicación a producción.

## <a name="deployment---case-of-multi-tenant-daemon-apps"></a>Implementación: caso de aplicaciones de demonio de varios inquilinos

Si es un ISV que desea crear una aplicación de demonio que se pueda ejecutar en varios inquilinos, deberá asegurarse de que los administradores de los inquilinos:

- Aprovisionen una entidad de servicio para la aplicación.
- Den su consentimiento a la aplicación.

Deberá explicar a sus clientes cómo realizar estas operaciones. Para más información, consulte [Solicitud de consentimiento para un inquilino al completo](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

A continuación, se proporcionan algunos vínculos con más información:

### <a name="net"></a>.NET

- Si aún no lo ha hecho, pruebe el artículo de inicio rápido [Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de consola mediante la identidad de la aplicación](./quickstart-v2-netcore-daemon.md).
- Documentación de referencia para:
  - Creación de instancias de [ConfidentialClientApplication](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder)
  - Llamada a [AcquireTokenForClient](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder)
- Otros ejemplos y tutoriales:
  - [microsoft-identity-platform-console-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) incluye una sencilla aplicación de consola de demonio de .NET Core que muestra los usuarios de un inquilino que consultan Microsoft Graph.

    ![Topología](media/scenario-daemon-app/daemon-app-sample.svg)

    El ejemplo también ilustra la variación con certificados.

    ![Topología](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [microsoft-identity-platform-aspnet-webapp-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) incluye aplicación web de ASP.NET MVC que sincroniza datos de Microsoft Graph mediante la identidad de la aplicación, en lugar hacerlo en nombre de un usuario. El ejemplo también ilustra el proceso de consentimiento del administrador.

    ![Topología](media/scenario-daemon-app/damon-app-sample-web.svg)

### <a name="python"></a>Python

MSAL Python está actualmente en versión preliminar pública. Para más información, consulte el [ejemplo de credenciales de cliente de MSAL Python en el repositorio](https://github.com/AzureAD/azure-activedirectory-library-for-python/blob/dev/sample/client_credentials_sample.py).

### <a name="java"></a>Java

MSAL Python está actualmente en versión preliminar pública. Para más información, consulte el [ejemplo de MSAL Java en el repositorio](https://github.com/AzureAD/azure-activedirectory-library-for-java/tree/dev/src/samples).