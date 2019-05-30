---
title: Autenticación con Azure Active Directory en las nubes nacionales
description: Obtenga información acerca de los puntos de conexión para la autenticación y registro de aplicaciones en nubes nacionales.
services: active-directory
documentationcenter: ''
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: negoe,CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd37366697a9c1f5019d2864e6d81a4dcd02e3a2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235489"
---
# <a name="national-clouds"></a>Nubes nacionales

Nubes nacionales son instancias físicamente aisladas de Azure. Estas regiones de Azure se han diseñado para asegurarse de que cumplen los requisitos de residencia, soberanía y cumplimiento normativo de los datos dentro de las fronteras geográficas.

Incluida la nube global, Azure Active Directory (Azure AD) se implementa en las nubes nacionales siguientes:  

- Azure Government
- Azure Alemania
- Azure China 21Vianet

Nubes nacionales son únicas y un entorno independiente de Azure global. Es importante tener en cuenta las diferencias clave al desarrollar la aplicación para estos entornos. Diferencias incluyen el registro de aplicaciones, adquisición de tokens y la configuración de puntos de conexión.

## <a name="app-registration-endpoints"></a>Puntos de conexión de registro de aplicaciones

Hay un portal de Azure independiente para cada una de las nubes nacionales. Para integrar aplicaciones con la plataforma Microsoft identity en una nube nacional, son necesarios para registrar la aplicación por separado en cada portal de Azure que es específico del entorno.

En la tabla siguiente se enumera las direcciones URL base para los puntos de conexión de Azure AD usadas para registrar una aplicación para cada nube nacional.

| Nube nacional | Punto de conexión de portal de Azure AD |
|----------------|--------------------------|
| Azure AD para el gobierno de EE. UU. | `https://portal.azure.us` |
| Azure AD Alemania | `https://portal.microsoftazure.de` |
| Azure AD China, operado por 21Vianet | `https://portal.azure.cn` |
| Azure AD (servicio global) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Puntos de conexión de autenticación de Azure AD

Todas las nubes nacionales autentican a los usuarios por separado en cada entorno y tienen puntos de conexión de autenticación independientes.

En la tabla siguiente se enumera las direcciones URL base para los puntos de conexión de Azure AD usadas para adquirir tokens para cada nube nacional.

| Nube nacional | Punto de conexión de autenticación de Azure AD |
|----------------|-------------------------|
| Azure AD para el gobierno de EE. UU. | `https://login.microsoftonline.us` |
| Azure AD Alemania| `https://login.microsoftonline.de` |
| Azure AD China, operado por 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (servicio global)| `https://login.microsoftonline.com` |

Puede formar las solicitudes a la autorización de Azure AD o puntos de conexión de token mediante el uso de la URL base específicas de la región adecuada. Por ejemplo, para Azure Alemania:

  - El punto de conexión común de autorización es `https://login.microsoftonline.de/common/oauth2/authorize`.
  - El punto de conexión común de token es `https://login.microsoftonline.de/common/oauth2/token`.

Para las aplicaciones de inquilino único, reemplace "common" en las direcciones URL anteriores con el Id. de inquilino o el nombre. Un ejemplo es `https://login.microsoftonline.de/contoso.com`.

> [!NOTE]
> El [autorización de Azure AD v2.0]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) y puntos de conexión de token solo están disponibles para el servicio global. No se admiten para las implementaciones de nube nacional.

## <a name="microsoft-graph-api"></a>Microsoft Graph API

Para obtener información sobre cómo llamar a la API de Microsoft Graph en un entorno de nube nacionales, vaya a [Microsoft Graph en las implementaciones de nube nacionales](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Ciertos servicios y características que se encuentran en regiones específicas del servicio global no esté disponibles en todas las nubes nacionales. Para averiguar qué servicios están disponibles, vaya a [productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Para obtener información sobre cómo compilar una aplicación mediante el uso de la plataforma Microsoft identity, siga el [tutorial de la biblioteca de autenticación de Microsoft (MSAL)](msal-national-cloud.md). En concreto, esta aplicación se inicie sesión en un usuario y obtener un token de acceso para llamar a Microsoft Graph API.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Alemania](https://docs.microsoft.com/azure/germany/)
- [Conceptos básicos de autenticación de Azure AD](authentication-scenarios.md)
