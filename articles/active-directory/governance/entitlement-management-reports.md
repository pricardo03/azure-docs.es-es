---
title: 'Ver informes y registros de administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo ver el informe de asignaciones de usuario y los registros de auditoría en la administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60a61a581574c77a57939ea23fdadc7b060b82af
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541546"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management-preview"></a>Ver informes y registros de administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="view-resources-a-user-has-access-to"></a>Ver un usuario tiene acceso a los recursos

1. Haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **informe de asignaciones de usuario**.

1. Haga clic en **Seleccionar usuarios** para abrir el panel de determinados usuarios.

1. Busque el usuario en la lista que desea ver los recursos que tienen acceso.

1. Haga clic en el usuario y, a continuación, haga clic en **seleccione**.

    Se muestra una lista de recursos que el usuario tiene acceso. Incluye el paquete de acceso, la directiva y las fechas.

    ![Informe de asignaciones de usuarios](./media/entitlement-management-reports/user-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>Determinar el estado de una solicitud de usuario

Para obtener más información sobre cómo un usuario había solicitado y recibido acceso a un paquete de acceso, puede usar el registro de auditoría de Azure AD. En concreto, puede usar las entradas del registro en el `EntitlementManagement` y `UserManagement` categorías para obtener detalles adicionales sobre los pasos de procesamiento para cada solicitud.  

1. Haga clic en **Azure Active Directory** y, a continuación, haga clic en **registros de auditoría**.

1. En la parte superior, cambie el **categoría** como `EntitlementManagement` o `UserManagement`, según el registro de auditoría que está buscando.  

1. Haga clic en **Aplicar**.

1. Para descargar los registros, haga clic en **descargar**.

Cuando Azure AD recibe una solicitud nuevo, escribe un registro de auditoría, en el que el **categoría** es `EntitlementManagement` y **actividad** suele ser `User requests access package assignment`.  En el caso de una asignación directa creada en el portal de Azure, el **actividad** campo del registro de auditoría es `Administrator directly assigns user to access package`, y el usuario que realiza la asignación se identifica mediante el **ActorUserPrincipalName**.

Azure AD escribirá los registros de auditoría adicionales mientras la solicitud está en curso, incluidas:

| Category | Actividad | Estado de la solicitud |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | Solicitud no requiere aprobación |
| `UserManagement` | `Create request approval` | Requiere la aprobación de solicitud |
| `UserManagement` | `Add approver to request approval` | Requiere la aprobación de solicitud |
| `EntitlementManagement` | `Approve access package assignment request` | Solicitud aprobada |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |Solicitud aprobada o no requiere aprobación |

Cuando un usuario está asignado acceso, Azure AD escribe un registro de auditoría para el `EntitlementManagement` categoría con **actividad** `Fulfill access package assignment`.  El usuario que recibe el acceso se identifica mediante **ActorUserPrincipalName** campo.

Si no se ha asignado acceso, Azure AD escribe un registro de auditoría para el `EntitlementManagement` categoría con **actividad** cualquier `Deny access package assignment request`, si se denegó la solicitud al aprobador, o `Access package assignment request timed out (no approver action taken)`, si la solicitud agotó el tiempo de espera antes de un puede aprobar el aprobador.

Cuando expira la asignación de paquetes de acceso del usuario, es cancelada por el usuario, o quitar un administrador, a continuación, Azure AD escribe un registro de auditoría para el `EntitlementManagement` categoría con **actividad** de `Remove access package assignment`.

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de administración de derechos de Azure AD](entitlement-management-troubleshoot.md)
- [Escenarios comunes](entitlement-management-scenarios.md)
