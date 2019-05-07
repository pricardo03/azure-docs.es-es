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
ms.openlocfilehash: a0d4586df23548854f4acbfefd32081a36906097
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067882"
---
# <a name="national-clouds"></a>Nubes nacionales

Nubes nacionales son instancias físicamente aisladas de Azure. Estas regiones de Azure se han diseñado para asegurarse de que cumplen los requisitos de residencia, soberanía y cumplimiento normativo de los datos dentro de las fronteras geográficas.

Incluida la nube global, Azure Active Directory se implementa en las siguientes nubes nacionales:  

- Azure US Government
- Azure Alemania
- Azure China 21Vianet

Las nubes nacionales son un entorno único y diferente de Azure global. Por lo tanto, es importante tener en cuenta ciertas diferencias al desarrollar la aplicación en estos entornos, como el registro de aplicaciones, la adquisición de tokens y la configuración de puntos de conexión.

## <a name="app-registration-endpoints"></a>Puntos de conexión de registro de aplicaciones

Hay un portal de Azure independiente para cada una de las nubes nacionales. Para integrar aplicaciones con la Plataforma de identidad de Microsoft en una nube nacional, deberá registrar la aplicación por separado en cada instancia de Azure Portal específica del entorno.

En la tabla siguiente se enumeran las direcciones URL base de los puntos de conexión de Azure Active Directory (Azure AD) que se usan para registrar una aplicación en cada nube nacional.

| Nube nacional | Punto de conexión de portal de Azure AD |
|----------------|--------------------------|
| Azure AD para el gobierno de EE. UU. | `https://portal.azure.us` |
| Azure AD Alemania | `https://portal.microsoftazure.de` |
| Azure AD China, operado por 21Vianet | `https://portal.azure.cn` |
| Azure AD (servicio global) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Puntos de conexión de autenticación de Azure AD

Todas las nubes nacionales autentican a los usuarios por separado en cada entorno y tienen puntos de conexión de autenticación independientes.

En la tabla siguiente se enumeran las direcciones URL base de los puntos de conexión de Azure Active Directory (Azure AD) que se usan para adquirir tokens en cada nube nacional.

| Nube nacional | Punto de conexión de autenticación de Azure AD |
|----------------|-------------------------|
| Azure AD para el gobierno de EE. UU. | `https://login.microsoftonline.us` |
| Azure AD Alemania| `https://login.microsoftonline.de` |
| Azure AD China, operado por 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (servicio global)| `https://login.microsoftonline.com` |

- Las solicitudes que se realicen a los puntos de conexión de autorización o token de Azure AD se pueden formar mediante la dirección URL base específica de la región pertinente. Por ejemplo, para Azure Alemania:

  - El punto de conexión común de autorización es `https://login.microsoftonline.de/common/oauth2/authorize`.
  - El punto de conexión común de token es `https://login.microsoftonline.de/common/oauth2/token`.

- En el caso de las aplicaciones de inquilino único, reemplace common en las direcciones URL anteriores por el identificador o el nombre del inquilino, por ejemplo, `https://login.microsoftonline.de/contoso.com`.

> [!NOTE]
> Los puntos de conexión de token y de [autorización de Azure AD v2.0]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) solo están disponibles para el servicio global. Aún no son compatibles con las implementaciones en las nubes nacionales.

## <a name="microsoft-graph-api"></a>Microsoft Graph API

Para obtener información sobre cómo llamar a Microsoft Graph API en el entorno de nube nacionales, vaya a [Implementaciones de nube nacionales](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Ciertos servicios y características que se encuentran en regiones específicas del servicio global pueden no estar disponibles en todas las nubes nacionales. Para averiguar qué servicios están disponibles consulte [los productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Siga este [tutorial de la biblioteca de autenticación de Microsoft (MSAL)](msal-national-cloud.md) para obtener información sobre cómo compilar una aplicación mediante la plataforma Microsoft identity. En concreto, esta aplicación se iniciará sesión en un usuario, obtener un token de acceso para llamar a Microsoft Graph API.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Alemania](https://docs.microsoft.com/azure/germany/)
- [Conceptos básicos de autenticación de Azure AD](authentication-scenarios.md)
