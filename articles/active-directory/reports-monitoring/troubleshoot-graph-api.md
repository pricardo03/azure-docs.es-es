---
title: Solución de errores de la API de informes de Azure Active Directory | Microsoft Docs
description: Le proporciona una resolución a los errores mientras llama a las API de informes de Azure Active Directory.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 299c9acb43fd0f3d7284a3b2ffb0c400b96cbc0b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811696"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Solución de errores de la API de informes de Azure Active Directory

En este artículo se enumeran los mensajes de error más comunes con los que puede encontrarse al acceder a los informes de actividad mediante la MS Graph API y los pasos para su resolución.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>Error de servidor interno HTTP 500 al obtener acceso al punto de conexión de Microsoft Graph V2

Actualmente no se admite el punto de conexión Microsoft Graph v2; asegúrese de acceder a los registros de actividad mediante el punto de conexión Microsoft Graph v1.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Error: No se pudieron obtener los roles de usuario de Graph AD

Es posible que aparezca este mensaje de error al intentar acceder a los inicios de sesión mediante el Probador de Graph. Asegúrese de que ha iniciado sesión en su cuenta con los dos botones de inicio de sesión de la interfaz de usuario del Probador de Graph, tal como se muestra en la imagen siguiente. 

![Explorador de Graph](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Error: No se pudo realizar la comprobación de licencia Premium de AD Graph 

Si se encuentra con este mensaje de error al intentar acceder a los inicios de sesión mediante el Probador de Graph, seleccione **Modificar permisos** debajo de su cuenta en la barra de navegación de la izquierda y seleccione **Tasks.ReadWrite** y **Directory.Read.All**. 

![Modificación de la interfaz de usuario de permisos](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Error: El inquilino no es B2C o el inquilino no tiene licencia Premium

El acceso a los informes de inicio de sesión requiere una licencia Premium 1 (P1) de Azure Active Directory. Si ve este mensaje de error al acceder a los inicios de sesión, asegúrese de que su inquilino tiene una licencia P1 de Azure AD.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Error: El usuario no está en los roles permitidos 

Si ve este mensaje de error mientras intenta acceder a los registros de auditoría o a los inicios de sesión mediante la API, asegúrese de que su cuenta forma parte del rol **Lector de seguridad** o **Lector de informes** en su inquilino de Azure Active Directory. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Error: Falta el permiso de AAD de la aplicación "Leer datos de directorio" 

Siga los pasos del artículo [Requisitos previos para acceder a la API de generación de informes de Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md) para asegurarse de que la aplicación se ejecuta con el conjunto correcto de permisos. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Error: Falta el permiso de MSGraph API "Leer todos los datos de registro de auditoría"

Siga los pasos del artículo [Requisitos previos para acceder a la API de generación de informes de Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md) para asegurarse de que la aplicación se ejecuta con el conjunto correcto de permisos. 

## <a name="next-steps"></a>Pasos siguientes

[Uso de la referencia de la API de auditoría](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit)
[Uso de la referencia de la API de informes de actividad de inicio de sesión](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)