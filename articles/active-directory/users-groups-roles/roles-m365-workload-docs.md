---
title: Contenido del rol de administrador para cargas de trabajo de Microsoft 365 en Azure AD | Microsoft Docs
description: Buscar contenido y las referencias de API de los roles de administrador para las cargas de trabajo de Microsoft 365 en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/24/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7460c3a7d957634d14186d313994c65d89fa4a32
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217877"
---
# <a name="administrator-roles-for-microsoft-365-workloads"></a>Roles de administrador para cargas de trabajo de Microsoft 365

Todos los productos de Microsoft 365 pueden administrarse con roles de administrador de Azure AD. Algunos productos también proporcionan roles adicionales que son específicos de ese producto. Para obtener información sobre los roles compatibles con cada producto, consulte la tabla siguiente. Puede encontrar discusiones generales sobre los problemas de delegación en [Delegación de administración en Azure Active Directory](roles-concept-delegation.md).

## <a name="where-to-find-content"></a>Dónde encontrar el contenido

Carga de trabajo de Microsoft 365 | Contenido del rol | Contenido de la API
---------------------- | ------------------ | -----------------
Roles de administrador de los planes de negocios de Office 365 y Microsoft 365 | [Funciones de administración de Office 365](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles?view=o365-worldwide) | No disponible
Azure Active Directory (Azure AD) y Azure AD Identity Protection| [Roles de administrador de Azure AD](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Exchange Online| [Control de acceso basado en roles de Exchange](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help) |  [PowerShell para Exchange](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
SharePoint Online | [Roles de administrador de Azure AD](directory-assign-admin-roles.md)<br>También [Acerca del rol de administrador de SharePoint en Office 365](https://docs.microsoft.com/sharepoint/sharepoint-admin-role) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Teams/Skype Empresarial | [Roles de administrador de Azure AD](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Centro de seguridad y cumplimiento (Office 365 Advanced Threat Protection, Exchange Online Protection, Information Protection) | [Funciones de administración de Office 365](https://docs.microsoft.com/office365/SecurityCompliance/permissions-in-the-security-and-compliance-center) | [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
Puntuación segura | [Roles de administrador de Azure AD](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Administrador de cumplimiento | [Roles del Administrador de cumplimiento](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud#permissions-and-role-based-access-control) | No disponible
Azure Information Protection | [Roles de administrador de Azure AD](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Microsoft Cloud App Security | [Control de acceso basado en rol](https://docs.microsoft.com/cloud-app-security/manage-admins) | [Referencia de API](https://docs.microsoft.com/cloud-app-security/api-tokens) 
Azure Advanced Threat Protection | [Grupos de roles de ATP de Azure](https://docs.microsoft.com/azure-advanced-threat-protection/atp-role-groups) | No disponible
Advanced Threat Protection de Windows Defender | [Control de acceso basado en roles de ATP de Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/rbac-windows-defender-advanced-threat-protection) | No disponible
Privileged Identity Management | [Roles de administrador de Azure AD](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Intune | [Control de acceso basado en roles de Intune](https://docs.microsoft.com/intune/role-based-access-control) | [Graph API](https://docs.microsoft.com/graph/api/resources/intune-rbac-conceptual?view=graph-rest-beta)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/graph/api/intune-rbac-roledefinition-list?view=graph-rest-beta)
Escritorio administrado | [Roles de administrador de Azure AD](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Obtener asignaciones de roles](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)

## <a name="next-steps"></a>Pasos siguientes

* [Asignación o eliminación de roles de administrador de Azure AD](directory-manage-roles-portal.md)
* [Referencia de roles de administrador de Azure AD](directory-assign-admin-roles.md)
