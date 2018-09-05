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
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: b5d48b3f854afaa79574e0ec13cff91f60396ac6
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190665"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Detección de recursos de Azure que se administran en PIM

Obtenga información sobre cómo detectar y administrar recursos de Azure al usar Privileged Identity Management (PIM) en Azure Active Directory (Azure AD). Esta información puede ser útil para las organizaciones que ya usan PIM para proteger los recursos de administrador y para los propietarios de suscripciones que buscan proteger los recursos de producción.

La primera vez que se configura PIM para recursos de Azure, hay que detectar y seleccionar los recursos que se van a proteger con PIM. No hay ningún límite en cuanto al número de recursos que se pueden administrar con PIM. Pero se recomienda empezar por los recursos más críticos (producción).

> [!NOTE]
> Solo se pueden buscar y seleccionar recursos de la suscripción o de un grupo de administración para administrarlos mediante PIM. Al administrar un grupo de administración o una suscripción en PIM, también se pueden administrar sus recursos secundarios.

## <a name="discover-resources"></a>Detección de recursos

En Azure Portal, vaya al panel **Privileged Identity Management**. En el menú de la izquierda, en la sección **ADMINISTRAR**, seleccione **Recursos de Azure**.

![Panel "Privileged Identity Management - Recursos de Azure"](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Si es la primera vez que usa PIM para recursos de Azure, primero debe ejecutar la detección para encontrar los recursos que se van a administrar. En el panel **Detectar recursos**, seleccione el botón **Detectar recursos** para iniciar la experiencia de detección.

![Panel "Detectar recursos"](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Si otro administrador de directorios o recursos de la organización ya administra un recurso de Azure con PIM, o si tiene una asignación de roles apta para un recurso, la vista de lista muestra el mensaje **Discover resources or activate an eligible role assignment to continue** (Detectar recursos o activar una asignación de roles apta para continuar). 

![Botón "Detectar recursos" del panel "Privileged Identity Manager - Recursos de Azure"](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Cuando se selecciona el botón **Detectar recursos**, aparece una lista de suscripciones que se pueden administrar en el menú superior o en medio del panel. Las suscripciones que están resaltadas ya están protegidas mediante PIM.

> [!NOTE]
> Para evitar que otro administrador de recursos quite la configuración de PIM, una vez que se establece una suscripción como administrada, no se puede dejar de administrar.

![Panel "Recursos de Azure - Detectar"](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

En la columna **RECURSO**, coloque el mouse sobre una suscripción que quiera proteger con PIM. Luego active la casilla situada a la izquierda del nombre del recurso. Puede seleccionar varias suscripciones a la vez.

![Lista de recursos del panel "Recursos de Azure - Detectar"](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Para iniciar el proceso de incorporación, en el menú superior, seleccione **Administrar recurso**.

![Botón "Administrar recurso" del panel "Recursos de Azure - Detectar"](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Los recursos seleccionados se administran ahora con PIM. Para cerrar la pantalla de detección, en la esquina superior derecha, seleccione **X**. Para empezar a administrar la configuración de PIM y a asignar miembros, en el menú de la parte superior del panel **Privileged Identity Management - Recursos de Azure**, seleccione el botón **Actualizar**.

![Botón "Actualizar" del menú superior del panel "Privileged Identity Management - Recursos de Azure"](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de roles de recurso de Azure en PIM](pim-resource-roles-configure-role-settings.md)
- [Asignación de roles de recursos de Azure en PIM](pim-resource-roles-assign-roles.md)
