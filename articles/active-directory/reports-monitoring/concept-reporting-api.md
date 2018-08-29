---
title: Introducción a la API de generación de informes de Azure AD | Microsoft Docs
description: Introducción a la API de generación informes de Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 7de7c87e5cf1747f4899f33d9e6b9cbf0bb430e1
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "42143422"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Introducción a la API de generación informes de Azure Active Directory

Azure Active Directory proporciona una variedad de [informes](overview-reports.md). Los datos de estos informes pueden ser muy útiles para las aplicaciones, como sistemas SIEM, auditorías y herramientas de inteligencia empresarial. 

Mediante la API de generación de informes de Azure AD, puede acceder mediante programación a los datos a través de un conjunto de API de REST. Estas API pueden llamarse desde una variedad de lenguajes de programación y herramientas.

En este artículo se proporciona un plan para acceder a los datos de informes mediante la API relacionada.

Si tiene problemas, consulte [Obtención de soporte técnico para Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Requisitos previos

Para acceder a la API de generación de informes, incluso si planea el acceso a la API mediante un script, necesita:

1. Asignar roles (Lector de seguridad Reader, Administrador de seguridad y Administrador global)
2. Registro de una aplicación
3. Concesión de permisos
4. Recopilación de configuraciones

Para obtener instrucciones detalladas, consulte [Requisitos previos para acceder a la API de generación de informes de Azure AD](howto-configure-prerequisites-for-reporting-api.md).

## <a name="apis-with-graph-explorer"></a>APIs con probador de Graph

Puede usar el [probador de MSGraph](https://developer.microsoft.com/graph/graph-explorer) para comprobar los datos de inicio de sesión de la API de auditoría. Asegúrese de iniciar sesión en su cuenta con ambos botones de inicio de sesión en la interfaz de usuario del Probador de Graph y establezca los permisos **AuditLog.Read.All** y **Directory.Read.All** para el inquilino tal como se muestra a continuación.   

![Explorador de Graph](./media/concept-reporting-api/graph-explorer.png)

![Modificación de la interfaz de usuario de permisos](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Uso de certificados para acceder a la API de generación de informes de Azure AD 

Si planea recuperar datos de informes sin la intervención del usuario, use la API de generación de informes de Azure AD con certificados.

Para instrucciones detalladas, consulte [Obtención de datos mediante la API de generación de informes de Azure Active Directory con certificados](tutorial-access-api-with-certificates.md).


## <a name="next-steps"></a>Pasos siguientes

 * [Referencia de la API de auditoría](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
 * [Referencia de la API de generación de informes de actividad de inicio de sesión](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
 * [Solución de errores de la API de generación de informes de Azure AD](troubleshoot-graph-api.md)


