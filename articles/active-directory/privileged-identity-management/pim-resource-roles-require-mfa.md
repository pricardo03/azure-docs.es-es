---
title: Aplicación de Azure Multi-Factor Authentication en recursos de Azure mediante Privileged Identity Management | Microsoft Docs
description: En este documento se describe cómo habilitar la autenticación multifactor para los recursos de PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 0c50a17b651fc1fb5d49915da5da4a37d40a0d3e
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260070"
---
# <a name="enforce-azure-multi-factor-authentication-in-azure-resources-by-using-privileged-identity-management"></a>Aplicación de Azure Multi-Factor Authentication en recursos de Azure mediante Privileged Identity Management

Privileged Identity Management (PIM) para los roles de recursos de Azure permite que los administradores de recursos y los administradores de identidades protejan la infraestructura crítica de Azure con pertenencia de tiempo limitado y acceso Just-In-Time. Además, PIM proporciona la aplicación opcional de Azure Multi-Factor Authentication para dos escenarios distintos.

## <a name="require-multi-factor-authentication-to-activate"></a>Requerir Multi-Factor Authentication para la activación

Los administradores de recursos pueden requerir que los miembros aptos para un rol ejecuten Azure Multi-Factor Authentication antes de poder realizar la activación. Este proceso garantiza que el usuario que solicita la activación sea quien dice ser con una exactitud razonable. Aplicar esta opción protege los recursos críticos en situaciones en las que es posible que la cuenta de usuario se haya puesto en peligro. 

Para cumplir con este requisito, seleccione un recurso en la lista de los recursos administrados. En el [panel de información general](pim-resource-roles-overview-dashboards.md), seleccione un rol de la lista de roles que aparece en la parte inferior derecha de la pantalla.

Además, puede acceder a la configuración de roles en las pestañas **Roles** o **Configuración de roles** del panel izquierdo.

>[!Note]
>Si las opciones del panel izquierdo aparecen atenuadas y en la parte superior de la página se muestra un banner con el mensaje "You have eligible roles that can be activated" ("Tiene roles elegibles que se pueden activar"), significa que no es un administrador activo. Esto significa que debe [activarlos](pim-resource-roles-activate-your-roles.md) antes de continuar.

![Pestañas de "Roles" y "Configuración de roles" ](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Para consultar la pertenencia de un rol, seleccione **Configuración de roles** en la barra que aparece en la parte superior de la pantalla para abrir **Role setting detail** (Detalles de la configuración de roles).

Seleccione el botón **Editar** en la parte superior para modificar la configuración de roles.

En la sección **Activar**, seleccione la casilla de verificación para **Require Multi-Factor Authentication to active** (Requerir Multi-Factor Authentication durante la activación). Después, seleccione **Guardar**.

![Requerir Multi-Factor Authentication durante la activación](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-multi-factor-authentication-on-assignment"></a>Requerir Multi-Factor Authentication durante la asignación

En algunos casos, es posible que un administrador de recursos quiera asignar un miembro a un rol durante un breve período (por ejemplo, un día). En este caso, no necesita los miembros asignados para solicitar la activación. En este escenario, PIM no puede aplicar Multi-Factor Authentication cuando el miembro usa su asignación de roles porque ya está activo en el rol desde el momento de su asignación.

Para garantizar que el administrador de recursos que cumple la asignación sea quien dice ser, puede aplicar Multi-Factor Authentication durante la asignación.

En la misma pantalla de detalles de la configuración de roles, active la casilla de verificación para **Require Multi-Factor Authentication on direct assignment** (Requerir Multi-Factor Authentication durante la asignación directa).

![Requerir Multi-Factor Authentication durante la asignación directa](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Pasos siguientes

[Solicitar aprobación para activar](pim-resource-roles-approval-workflow.md)

[Usar el registro de auditoría](pim-resource-roles-use-the-audit-log.md)



