---
title: Uso de roles personalizados para recursos de Azure en PIM - Azure Active Directory | Microsoft Docs
description: Aprenda a usar roles de personalizados para recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 13aef9b180a671a9b42bbc6319c487be36652093
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60437376"
---
# <a name="use-custom-roles-for-azure-resources-in-pim"></a>Uso de roles personalizados para recursos de Azure en PIM

Es posible que deba aplicar la configuración de Azure Active Directory (Azure AD) Privileged Identity Management (PIM) estricta a algunos miembros de un rol, al proporcionar una mayor autonomía para que otros usuarios. Imagine un escenario en el que su organización contrata varios a asociados para que le ayuden en el desarrollo de una aplicación que se ejecutará en una suscripción de Azure.

Como administrador de recursos, quiere que los empleados puedan obtener acceso sin necesidad de aprobación. Sin embargo, todos los asociados contratados deben obtener una aprobación cuando soliciten acceso a los recursos de la organización.

Siga los pasos que se describen en la siguiente sección para configurar las opciones de destino de PIM para los roles de recursos de Azure.

## <a name="create-the-custom-role"></a>Creación del rol personalizado

Para crear un rol personalizado para un recurso, siga los pasos descritos en [Create custom roles for Azure Role-Based Access Control](../role-based-access-control-custom-roles.md) (Crear roles personalizados para el control de acceso basado en rol de Azure).

Cuando cree un rol personalizado, incluya un nombre descriptivo para que pueda recordar fácilmente qué rol integrado pretende duplicar.

> [!NOTE]
> Asegúrese de que el rol personalizado sea un duplicado del rol integrado que quiere duplicar, y que su ámbito coincida con el rol integrado.

## <a name="apply-pim-settings"></a>Aplicación de la configuración de PIM

Una vez que se haya creado el rol en el inquilino, vaya a Azure Portal y acceda al panel **Privileged Identity Management - Recursos de Azure**. Seleccione el recurso al cual se aplica el rol.

![Panel "Privileged Identity Management - Recursos de Azure"](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[Configure los valores de rol de PIM](pim-resource-roles-configure-role-settings.md) que se deberían aplicar a estos miembros del rol.

Por último, [asigne roles](pim-resource-roles-assign-roles.md) al grupo distinto de miembros que quiere establecer como destino de esta configuración.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de roles de recurso de Azure en PIM](pim-resource-roles-configure-role-settings.md)
- [Roles personalizados en Azure](../../role-based-access-control/custom-roles.md)
