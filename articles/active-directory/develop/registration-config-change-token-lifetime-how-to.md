---
title: Modificación de los valores predeterminados de vigencia de los tokens en una aplicación personalizada | Microsoft Docs
description: Aprenda a actualizar las directivas de vigencia de los tokens para la aplicación que está desarrollando en Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8c0700ff40063bfb7709b583f849eed179648306
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203580"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Modificación de los valores predeterminados de vigencia de los tokens en una aplicación personalizada

Azure AD Premium permite a los desarrolladores de aplicaciones y a los administradores de inquilinos configurar la vigencia de los tokens emitidos para clientes no confidenciales. Las directivas de vigencia de los tokens se establecen para todos los inquilinos o para los recursos a los que se va a acceder.

1. Para establecer una directiva de vigencia de tokens, debe descargar el [módulo Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).
1. Ejecute el comando **Connect-AzureAD -Confirm**.

    En esta directiva de ejemplo, se establece la actualización del token con arreglo a un único factor de antigüedad máxima. Cree la directiva: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Configurable token lifetimes in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) (Duraciones de tokens configurables) para aprender a configurar las duraciones de los tokens generadas por Azure AD, lo que incluye cómo establecer las duraciones de los tokens de todas las aplicaciones de su organización, de una aplicación multiinquilino o de una entidad de servicio concreta de su organización. 
* [Referencia de tokens de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

