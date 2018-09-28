---
title: Autenticación mediante Azure AD en nubes nacionales
description: Obtenga información acerca de los puntos de conexión para la autenticación y registro de aplicaciones en nubes nacionales.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: negoe
ms.reviewer: negoe,andret,saeeda
ms.custom: aaddev
ms.openlocfilehash: 866a86178d66b7b4af069d684e4eb56c12db47ca
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982010"
---
# <a name="national-clouds"></a>Nubes nacionales

Las nubes nacionales (también conocidas como nubes soberanas) son instancias físicamente aisladas de Azure. Estas regiones de Azure se han diseñado para asegurarse de que cumplen los requisitos de residencia, soberanía y cumplimiento normativo de los datos dentro de las fronteras geográficas.

Incluida la nube global, Azure Active Directory se implementa en las siguientes nubes nacionales:  

- Azure US Government
- Azure Alemania
- Azure China 21Vianet

## <a name="app-registration-endpoints"></a>Puntos de conexión de registro de aplicaciones

En la tabla siguiente se enumeran las direcciones URL base de los puntos de conexión de Azure Active Directory (Azure AD) que se usan para registrar una aplicación en cada nube nacional.

| Nube nacional | Punto de conexión de portal de Azure AD
| --- | --- |
| Azure AD para el gobierno de EE. UU. |https://portal.azure.us
|Azure AD Alemania |https://portal.microsoftazure.de
|Azure AD China, operado por 21Vianet |https://portal.azure.cn
|Azure AD (servicio global)|https://portal.azure.com

## <a name="azure-ad-authentication-endpoints"></a>Puntos de conexión de autenticación de Azure AD

En la tabla siguiente se enumeran las direcciones URL base de los puntos de conexión de Azure Active Directory (Azure AD) que se usan adquirir tokens para llamar a Microsoft Graph en cada nube nacional.

| Nube nacional | Punto de conexión de autenticación de Azure AD
| --- | --- |
| Azure AD para el gobierno de EE. UU. |`https://login.microsoftonline.us`
|Azure AD Alemania| `https://login.microsoftonline.de`
|Azure AD China, operado por 21Vianet | `https://login.chinacloudapi.cn`
|Azure AD (servicio global)|`https://login.microsoftonline.com`

Las solicitudes que se realicen a los puntos de conexión de autorización o token de Azure AD se pueden formar mediante la dirección URL base específica de la región pertinente. Por ejemplo, en el caso de Alemania:

- El punto de conexión común de autorización es `https://login.microsoftonline.de/common/oauth2/authorize`
- El punto de conexión común de token es `https://login.microsoftonline.de/common/oauth2/token`. 

En el caso de las aplicaciones de inquilino único, reemplace common en las direcciones URL anteriores por el identificador o el nombre del inquilino, por ejemplo, `https://login.microsoftonline.de/contoso.com`

>[!NOTE]
> Los puntos de conexión de token y de [autorización de Azure AD v2.0]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) solo están disponibles para el servicio global. Aún no son compatibles con las implementaciones en las nubes nacionales.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- Más información acerca de [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- Más información acerca de [Azure Alemania](https://docs.microsoft.com/azure/germany/)
- Más información acerca de los [conceptos básicos de autenticación](authentication-scenarios.md) de Azure AD.
- Más información acerca de la [implementación de Microsoft Graph en una nube nacional](https://developer.microsoft.com/graph/docs/concepts/deployments)