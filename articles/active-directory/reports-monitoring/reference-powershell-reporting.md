---
title: Cmdlets de Azure AD PowerShell para informes | Microsoft Docs
description: Referencia de los cmdlets de Azure AD PowerShell para informes
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/12/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27fa3d7be5238527f86e9dfde3be70ae09259d69
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302750"
---
# <a name="azure-ad-powershell-cmdlets-for-reporting"></a>Cmdlets de Azure AD PowerShell para informes

Con los informes de Azure Active Directory (Azure AD), puede obtener toda la información que necesita para determinar cómo funciona el entorno. Puede recuperar los datos de los informes mediante los cmdlets de PowerShell de Azure AD para la generación de informes.

En este artículo se proporciona información general sobre el cmdlet.




## <a name="audit-logs"></a>Registros de auditoría

Los [registros de auditoría](concept-audit-logs.md) proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.

Puede acceder a los registros de auditoría con el cmdlet "Get-AzureADAuditDirectoryLogs".


| Escenario                      | Comando de PowerShell |
| :--                           | :--                |
| Nombre para mostrar de la aplicación      | Get-AzureADAuditDirectoryLogs -Filter "initiatedBy/app/displayName eq 'Azure AD Cloud Sync'" |
| Categoría                      | Get-AzureADAuditDirectoryLogs -Filter "category eq 'Application Management'" |
| Fecha y hora de la actividad            | Get-AzureADAuditDirectoryLogs -Filter "activityDateTime gt 2019-04-18" |
| Todo lo anterior              | Get-AzureADAuditDirectoryLogs -Filter "initiatedBy/app/displayName eq 'Azure AD Cloud Sync' and category eq 'Application Management' and activityDateTime gt 2019-04-18"|


En la imagen siguiente se muestra un ejemplo de este comando. 

![Botón "Data Summary" (Resumen de datos)](./media/reference-powershell-reporting/get-azureadauditdirectorylogs.png)



## <a name="sign-in-logs"></a>Registros de inicio de sesión

Los registros de [inicio de sesión](concept-sign-ins.md) proporcionan información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.

Puede acceder a los registros de inicio de sesión con el cmdlet "Get-AzureADAuditSignInLogs".


| Escenario                      | Comando de PowerShell |
| :--                           | :--                |
| Nombre para mostrar del usuario             | Get-AzureADAuditSignInLogs -Filter "userDisplayName eq 'Timothy Perkins'" |
| Fecha y hora de creación              | Get-AzureADAuditSignInLogs -Filter "createdDateTime gt 2019-04-18T17:30:00.0Z" (Everything since 5:30 pm on 4/18) |
| Status                        | Get-AzureADAuditSignInLogs -Filter "status/errorCode eq 50105" |
| Nombre para mostrar de la aplicación      | Get-AzureADAuditSignInLogs -Filter "appDisplayName eq 'StoreFrontStudio [wsfed enabled]'" |
| Todo lo anterior              | Get-AzureADAuditSignInLogs -Filter "userDisplayName eq 'Timothy Perkins' and status/errorCode ne 0 and appDisplayName eq 'StoreFrontStudio [wsfed enabled]'" |


En la imagen siguiente se muestra un ejemplo de este comando. 

![Botón "Data Summary" (Resumen de datos)](./media/reference-powershell-reporting/get-azureadauditsigninlogs.png)



## <a name="next-steps"></a>Pasos siguientes

- [Información general sobre informes de Azure AD](overview-reports.md).
- [Informe de registros de auditoría](concept-audit-logs.md) 
- [Acceso mediante programación a los informes de Azure AD](concept-reporting-api.md)
