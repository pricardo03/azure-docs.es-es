---
title: Autenticación con Azure Active Directory en nubes nacionales
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235489"
---
# <a name="national-clouds"></a>Nubes nacionales

Las nubes nacionales son instancias físicamente aisladas de Azure. Estas regiones de Azure se han diseñado para asegurarse de que cumplen los requisitos de residencia, soberanía y cumplimiento normativo de los datos dentro de las fronteras geográficas.

Incluida la nube mundial, Azure Active Directory (Azure AD) se implementa en las siguientes nubes nacionales:  

- Azure Government
- Azure Alemania
- Azure China 21Vianet

Las nubes nacionales son únicas y están en un entorno independiente de la nube mundial de Azure. Es importante tener en cuenta las diferencias clave al desarrollar la aplicación para estos entornos. Entre las diferencias se incluyen el registro de aplicaciones, la adquisición de tokens y la configuración de puntos de conexión.

## <a name="app-registration-endpoints"></a>Puntos de conexión de registro de aplicaciones

Hay una instancia de Azure Portal independiente para cada una de las nubes nacionales. Para integrar aplicaciones con la plataforma de identidad de Microsoft en una nube nacional, deberá registrar la aplicación por separado en cada instancia de Azure Portal específica del entorno.

En la tabla siguiente se enumeran las direcciones URL base de los puntos de conexión de Azure AD que se usan para registrar una aplicación en cada nube nacional.

| Nube nacional | Punto de conexión de portal de Azure AD |
|----------------|--------------------------|
| Azure AD para el gobierno de EE. UU. | `https://portal.azure.us` |
| Azure AD Alemania | `https://portal.microsoftazure.de` |
| Azure AD China, operado por 21Vianet | `https://portal.azure.cn` |
| Azure AD (servicio global) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Puntos de conexión de autenticación de Azure AD

Todas las nubes nacionales autentican a los usuarios por separado en cada entorno y tienen puntos de conexión de autenticación independientes.

En la tabla siguiente se enumeran las direcciones URL base de los puntos de conexión de Azure AD que se usan para adquirir tokens en cada nube nacional.

| Nube nacional | Punto de conexión de autenticación de Azure AD |
|----------------|-------------------------|
| Azure AD para el gobierno de EE. UU. | `https://login.microsoftonline.us` |
| Azure AD Alemania| `https://login.microsoftonline.de` |
| Azure AD China, operado por 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (servicio global)| `https://login.microsoftonline.com` |

Puede realizar solicitudes a los puntos de conexión de autorización o token de Azure AD mediante la dirección URL base específica de la región pertinente. Por ejemplo, para Azure Alemania:

  - El punto de conexión común de autorización es `https://login.microsoftonline.de/common/oauth2/authorize`.
  - El punto de conexión común de token es `https://login.microsoftonline.de/common/oauth2/token`.

En el caso de las aplicaciones de inquilino único, reemplace "common" en las direcciones URL anteriores por el identificador o el nombre del inquilino. Un ejemplo es `https://login.microsoftonline.de/contoso.com`.

> [!NOTE]
> Los puntos de conexión de token y de [autorización de Azure AD v2.0]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) solo están disponibles para el servicio mundial. Aún no se admiten para las implementaciones de nubes nacionales.

## <a name="microsoft-graph-api"></a>Microsoft Graph API

Para obtener información sobre cómo llamar a las API de Microsoft Graph en el entorno de nube nacional, vaya a [Microsoft Graph en implementaciones de nube nacional](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Ciertos servicios y características que se encuentran en regiones específicas del servicio mundial pueden no estar disponibles en todas las nubes nacionales. Para saber qué servicios están disponibles, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Para aprender a compilar una aplicación mediante la plataforma de identidad de Microsoft, siga el [Tutorial de la biblioteca de autenticación de Microsoft (MSAL)](msal-national-cloud.md). En concreto, esta aplicación iniciará la sesión de un usuario y obtendrá un token de acceso para llamar a la API de Microsoft Graph.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Alemania](https://docs.microsoft.com/azure/germany/)
- [Conceptos básicos sobre la autenticación de Azure AD](authentication-scenarios.md)
