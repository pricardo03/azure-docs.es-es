---
title: Detectar recursos de Azure para administrar en PIM - Azure Active Directory | Microsoft Docs
description: Aprenda a detectar recursos de Azure que se administran en Azure AD Privileged Identity Management (PIM).
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
ms.date: 04/09/2019
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 786b5ed99cce29c7217bfa42446821848ca8e05f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59492642"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Detección de recursos de Azure que se administran en PIM

Con Azure Active Directory (Azure AD) Privileged Identity Management (PIM), puede mejorar la protección de los recursos de Azure. Esto es útil para las organizaciones que ya usa PIM para proteger los roles de Azure AD y propietarios de suscripción y de grupo de administración que desean para proteger los recursos de producción.

La primera vez que se configura PIM para recursos de Azure, hay que detectar y seleccionar los recursos que se van a proteger con PIM. No hay ningún límite en cuanto al número de recursos que se pueden administrar con PIM. Pero se recomienda empezar por los recursos más críticos (producción).

## <a name="discover-resources"></a>Detección de recursos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

    Si es la primera vez que usa PIM para recursos de Azure, aparecerá el panel Detectar recursos.

    ![Detectar recursos: primera vez](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Si otro administrador de directorios o recursos de la organización ya administra recursos de Azure en PIM, verá una lista de los recursos que se están administrando actualmente.

    ![Panel Detectar recursos](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Haga clic en **Detectar recursos** para iniciar la experiencia de detección.

    ![Panel de detección](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. En el panel de detección, use **Filtro de estado de recurso** y **Seleccionar tipo de recurso** para filtrar los grupos de administración o suscripciones para las que tiene permiso de escritura. Empezar con **Todo** es, probablemente, más fácil.

    Solo se pueden buscar y seleccionar recursos de la suscripción o de un grupo de administración para administrarlos mediante PIM. Al administrar un grupo de administración o una suscripción en PIM, también se pueden administrar sus recursos secundarios.

1. Agregue una marca de verificación junto a los recursos no administrados que quiera administrar.

1. Haga clic en **Administrar recurso** para empezar a administrar los recursos seleccionados.

    > [!NOTE]
    > Una vez definido un grupo de administración o suscripción como administrado, no puede ser no administrado. Esto impide que otro administrador de recursos pueda quitar la configuración de PIM.

    ![Detección: administrar recursos](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Si ve un mensaje para confirmar la incorporación del recurso seleccionado para la administración, haga clic en **Sí**.

    ![Mensaje Detección: administrar recurso](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de roles de recurso de Azure en PIM](pim-resource-roles-configure-role-settings.md)
- [Asignación de roles de recursos de Azure en PIM](pim-resource-roles-assign-roles.md)
