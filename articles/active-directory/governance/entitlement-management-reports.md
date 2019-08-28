---
title: 'Visualización de informes y registros en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo ver el informe de asignaciones de usuario y los registros de auditoría en la administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: ajburnle
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2420fc25795ec74939649fb8a17ead7c8cfdd1df
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69032451"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management-preview"></a>Visualización de informes y registros en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El informe de asignaciones de usuarios y el registro de auditoría de Azure Active Directory proporcionan detalles adicionales sobre los usuarios del directorio. Como administrador, puede ver los recursos a los que un usuario tiene acceso. Además, puede ver los registros de solicitudes para auditarlos o para determinar el estado de la solicitud de un usuario. En este artículo se describe cómo usar el informe de asignaciones del usuario y los registros de auditoría de Azure AD.

Vea el siguiente vídeo para obtener información sobre cómo usar la administración de derechos para administrar el acceso de los usuarios en Azure Active Directory:

>[!VIDEO https://www.youtube.com/embed/omtNJ7ySjS0]

## <a name="view-resources-a-user-has-access-to"></a>Visualización de los recursos a los que tiene acceso un usuario

1. Haga clic en **Azure Active Directory** y, luego, haga clic en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Informe de asignaciones de usuarios**.

1. Haga clic en **Seleccionar usuarios** para abrir el panel de selección de usuarios.

1. Busque en la lista el usuario del que quiere ver los recursos a los que tiene acceso.

1. Haga clic en el usuario y luego haga clic en **Seleccionar**.

    Se muestra una lista de los recursos a los que el usuario tiene acceso. Incluye el paquete de acceso, la directiva y las fechas.

    ![Informe de asignaciones de usuarios](./media/entitlement-management-reports/user-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>Determinar el estado de la solicitud de un usuario

Para obtener más información sobre cómo un usuario ha solicitado y recibido acceso a un paquete de acceso, puede usar el registro de auditoría de Azure AD. En concreto, puede usar las entradas de registro de las categorías `EntitlementManagement` y `UserManagement` para obtener detalles adicionales sobre los pasos de procesamiento de cada solicitud.  

1. Haga clic en **Azure Active Directory** y, luego, haga clic en **Registros de auditoría**.

1. En la parte superior, cambie el valor de **Categoría** a `EntitlementManagement` o `UserManagement`, según el registro de auditoría que esté buscando.  

1. Haga clic en **Aplicar**.

1. Haga clic en **Descargar** para descargar los registros.

Cuando Azure AD recibe una solicitud nueva, escribe un registro de auditoría, donde el valor de **Categoría** es `EntitlementManagement` y el de **Actividad** suele ser `User requests access package assignment`.  En el caso de una asignación directa creada en Azure Portal, el campo **Actividad** del registro de auditoría es `Administrator directly assigns user to access package` y el usuario que realiza la asignación se identifica mediante el campo **ActorUserPrincipalName**.

Azure AD escribirá registros de auditoría adicionales mientras la solicitud esté en curso, que incluirán:

| Categoría | Actividad | Estado de la solicitud |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | La solicitud no requiere aprobación |
| `UserManagement` | `Create request approval` | La solicitud requiere aprobación |
| `UserManagement` | `Add approver to request approval` | La solicitud requiere aprobación |
| `EntitlementManagement` | `Approve access package assignment request` | Solicitud aprobada |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |Solicitud aprobada o no requiere aprobación |

Cuando se asigna el acceso a un usuario, Azure AD escribe un registro de auditoría para la categoría `EntitlementManagement` con el valor **Actividad** `Fulfill access package assignment`.  El usuario que recibe el acceso se identifica mediante el campo **ActorUserPrincipalName**.

Si no se ha asignado el acceso, Azure AD escribe un registro de auditoría para la categoría `EntitlementManagement` con **Actividad** establecido en `Deny access package assignment request`, si un aprobador ha denegado la solicitud, o en `Access package assignment request timed out (no approver action taken)`, si la solicitud ha agotado el tiempo de espera antes de un aprobador pueda aprobarla.

Cuando la asignación de paquetes de acceso del usuario expira, la cancela el usuario o la quita un administrador, Azure AD escribe un registro de auditoría para la categoría `EntitlementManagement` con **Actividad** establecido en `Remove access package assignment`.

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de la administración de derechos de Azure AD](entitlement-management-troubleshoot.md)
- [Escenarios comunes](entitlement-management-scenarios.md)
