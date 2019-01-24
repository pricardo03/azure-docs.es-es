---
title: Introducción a la API de generación de informes de Azure AD | Microsoft Docs
description: Introducción a la API de generación informes de Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: fe65766afa02de60e7e3b1576e31a141e5a1961e
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817697"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Introducción a la API de generación informes de Azure Active Directory

Azure Active Directory proporciona diversos [informes](overview-reports.md) que contienen información útil para las aplicaciones, como sistemas SIEM, auditoría y herramientas de inteligencia empresarial. 

Mediante Microsoft Graph API para informes de Azure AD, puede acceder mediante programación a los datos por medio de un conjunto de API basadas en REST. Estas API pueden llamarse desde una variedad de lenguajes de programación y herramientas.

En este artículo se proporciona información general de la API de informes, como las maneras de obtener acceso a ella.

Si tiene problemas, consulte [Obtención de soporte técnico para Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="prerequisites"></a>Requisitos previos

Para acceder a la API de informes, con o sin la intervención del usuario, necesita:

1. Asignar roles (Lector de seguridad Reader, Administrador de seguridad y Administrador global)
2. Registro de una aplicación
3. Concesión de permisos
4. Recopilación de configuraciones

Para obtener instrucciones detalladas, consulte [Requisitos previos para acceder a la API de generación de informes de Azure AD](howto-configure-prerequisites-for-reporting-api.md). 

## <a name="api-endpoints"></a>Puntos de conexión de API 

El punto de conexión de Microsoft Graph API para registros de auditoría es `https://graph.microsoft.com/beta/auditLogs/directoryAudits` y el punto de conexión de Microsoft Graph API para inicios de sesión es `https://graph.microsoft.com/beta/auditLogs/signIns`. Para más información, consulte la [referencia de API de auditoría](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) y la [referencia de API de inicio de sesión](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signIn).

Además, puede utilizar la [API de eventos de riesgo Identity Protection](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) para acceder mediante programación a las detecciones de seguridad con Microsoft Graph. Para obtener más información, consulte [Introducción a Azure Active Directory Identity Protection y Microsoft Graph](../identity-protection/graph-get-started.md). 

> [!NOTE]
>  El punto de conexión **https:\/\/graph.windows.net\/\<nombre-de-inquilino\>\/reports\/** está en desuso. Use los nuevos puntos de conexión de API descritos anteriormente para acceder mediante programación a los informes de actividad y seguridad.
  
## <a name="apis-with-graph-explorer"></a>APIs con probador de Graph

Puede usar el [probador de MSGraph](https://developer.microsoft.com/graph/graph-explorer) para comprobar los datos de inicio de sesión de la API de auditoría. Asegúrese de iniciar sesión en su cuenta con ambos botones de inicio de sesión en la interfaz de usuario del Probador de Graph y establezca los permisos **AuditLog.Read.All** y **Directory.Read.All** para el inquilino tal como se muestra a continuación.   

![Explorador de Graph](./media/concept-reporting-api/graph-explorer.png)

![Modificación de la interfaz de usuario de permisos](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Uso de certificados para acceder a la API de generación de informes de Azure AD 

Si planea recuperar datos de informes sin la intervención del usuario, use la API de generación de informes de Azure AD con certificados.

Para instrucciones detalladas, consulte [Obtención de datos mediante la API de generación de informes de Azure Active Directory con certificados](tutorial-access-api-with-certificates.md).

## <a name="next-steps"></a>Pasos siguientes

 * [Requisitos previos para acceder a la API de informes](howto-configure-prerequisites-for-reporting-api.md) 
 * [Obtención de datos mediante la API de informes de Azure AD con certificados](tutorial-access-api-with-certificates.md)
 * [Solución de errores de la API de generación de informes de Azure AD](troubleshoot-graph-api.md)


