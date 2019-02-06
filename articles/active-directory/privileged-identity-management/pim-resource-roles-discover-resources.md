---
title: Detección de recursos de Azure que se administran en PIM | Microsoft Docs
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
ms.date: 01/23/2019
ms.author: rolyon
ms.openlocfilehash: e6548c2635afd8e1ac784a1930c2277548647cfd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55192764"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Detección de recursos de Azure que se administran en PIM

Con Azure AD Privileged Identity Management (PIM), puede mejorar la protección de los recursos de Azure. Esto resulta útil para las organizaciones que ya usan PIM para proteger los roles de Azure Active Directory, así como el grupo de administración y los propietarios de la suscripción que quieren proteger los recursos de producción.

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
