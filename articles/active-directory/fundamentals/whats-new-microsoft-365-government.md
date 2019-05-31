---
title: Novedades de Azure Active Directory en Microsoft 365 Government - Azure Active Directory | Microsoft Docs
description: Obtenga información sobre algunos cambios a Azure Active Directory (Azure AD) en la instancia de la nube de Microsoft 365 Government, que podría afectar al.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.reviewer: sumitp
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b077c7b5efbad2add971d42ff31938b56f6bc33
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258900"
---
# <a name="whats-new-for-azure-active-directory-in-microsoft-365-government"></a>Novedades de Azure Active Directory en Microsoft 365 Government

Hemos realizado algunos cambios a Azure Active Directory (Azure AD) en la instancia de nube de Microsoft 365 Government, que es aplicable a los clientes que usan los siguientes servicios:

- Microsoft Azure Government

- Microsoft 365 Government – GCC High

- Microsoft 365 – DoD del gobierno de

En este artículo no se aplica a Microsoft 365 Government: los clientes GCC.

## <a name="changes-to-the-initial-domain-name"></a>Cambios realizados en el nombre de dominio inicial

Durante la suscripción para un servicio en línea de Microsoft 365 Government la inicial de su organización, se le pidió que elija el nombre de dominio de su organización, `<your-domain-name>.onmicrosoft.com`. Si ya tiene un nombre de dominio con el sufijo .com, nada cambiará.

Sin embargo, si es registrarse para un nuevo servicio de Microsoft 365 Government, le pedirá que elija un nombre de dominio mediante la `.us` sufijo. Por lo tanto, será `<your-domain-name>.onmicrosoft.us`.

>[!Note]
>Este cambio no se aplica a todos los clientes que están administrados por proveedores de servicios en la nube (CSP).

## <a name="changes-to-portal-access"></a>Cambios realizados en el acceso al portal

Hemos actualizado los extremos del portal de Microsoft Azure Government, Microsoft 365 Government – GCC High y Microsoft 365 Government – DoD, como se muestra en el [tabla de asignación de punto de conexión](#endpoint-mapping).

Previamente los clientes podrían iniciar sesión con el portal (portal.azure.com) de Azure en todo el mundo y portal de Office 365 (portal.office.com). Con esta actualización, los clientes ahora deben iniciar sesión con el Microsoft Azure Government, Microsoft 365 Government - GCC High y Microsoft 365 Government - portales de DoD específicos.

## <a name="endpoint-mapping"></a>Asignación de puntos de conexión

La siguiente tabla muestra los puntos de conexión para todos los clientes:

| NOMBRE | Detalles del punto de conexión |
|------|------------------|
| Portales |Microsoft Azure Government: https://portal.azure.us<p>Microsoft 365 Government – GCC alta: https://portal.office365.us<p>Microsoft 365 – DoD del gobierno: https://portal.apps.mil |
| Extremo de la autoridad de Azure Active Directory | https://login.microsoftonline.us |
| Graph API de Azure Active Directory | https://graph.windows.net |
| Microsoft Graph API de Microsoft 365 Government - GCC High | https://graph.microsoft.us |
| Microsoft Graph API de Microsoft 365 Government - DoD | https://dod-graph.microsoft.us |
| Los puntos de conexión de servicios de Azure Government | Para obtener más información, consulte [Guía del desarrollador de Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-developer-guide) |
| Microsoft 365 Government - puntos de conexión de GCC High | Para obtener más información, consulte [Office 365 EE. UU. Puntos de conexión de GCC High de gobierno](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-gcc-high-endpoints) |
| Microsoft 365 Government - DoD | Para obtener más información, consulte [Office 365 EE. UU. Extremos de DoD del gobierno](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints) |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte estos artículos:

- [¿Qué es Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)

- [Actualización de punto de conexión de Azure Government AAD autoridad](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/)

- [Puntos de conexión de Microsoft Graph en la nube del gobierno de Estados Unidos](https://developer.microsoft.com/graph/blogs/new-microsoft-graph-endpoints-in-us-government-cloud/)

- [Office 365 US Government GCC High y DoD](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod)