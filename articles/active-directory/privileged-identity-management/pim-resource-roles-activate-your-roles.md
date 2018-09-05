---
title: Activación de mis roles de recursos de Azure en PIM | Microsoft Docs
description: Aprenda a activar sus roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
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
ms.date: 08/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 234c1d71f0ec17d15a4dd589e3db92fd9bf68df2
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189496"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Activación de mis roles de recursos de Azure en PIM
Privileged Identity Management (PIM) presenta una nueva experiencia de activación de roles para los recursos de Azure. Los miembros del rol que sean aptos pueden programar la activación para una fecha y hora futuras. Asimismo, también pueden seleccionar una duración de activación específica dentro del valor máximo establecido (configurado por los administradores). Para obtener más información, consulte [How to activate or deactivate roles in Azure AD Privileged Identity Management](pim-how-to-activate-role.md) (Cómo activar o desactivar roles en Privileged Identity Management para Azure AD).

## <a name="activate-a-role"></a>Activación de un rol
Vaya a la sección **Mis roles** en el panel izquierdo. Seleccione **Activar** para el rol que quiera activar.

![Pestaña "Roles aptos" del panel "Mis roles".](media/azure-pim-resource-rbac/rbac-roles.png)

En el menú **Activaciones**, escriba la fecha de inicio y la hora para activar el rol. Si lo desea, reduzca la duración de la activación (el período de tiempo durante el cual el rol permanece activo) y escriba una justificación si procede. A continuación seleccione **Activar**.

Si no se modifican la fecha de inicio y la hora, el rol se activa en unos segundos. En el panel **Mis roles**, se muestra un mensaje emergente que indica que el rol se puso en la cola para su activación. Seleccione el botón Actualizar para borrar este mensaje.

![Panel "Mis roles" con un mensaje emergente y una notificación acerca de un elemento pendiente de aprobar](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Si la activación está programada para una fecha y hora futuras, la solicitud pendiente aparecerá en la pestaña **Solicitudes pendientes** del panel izquierdo. Si la activación del rol ya no es necesaria, puede cancelar la solicitud si selecciona el botón **Cancelar** .

![Lista de solicitudes pendientes con botones "Cancelar"](media/azure-pim-resource-rbac/rbac-activate-pending.png)

## <a name="use-a-role-immediately-after-activation"></a>Uso de un rol inmediatamente después de la activación

El almacenamiento en caché es el responsable de que las activaciones no se produzcan inmediatamente en Azure Portal sin una actualización. Si necesita reducir la posibilidad de retrasos tras la activación de un rol, puede usar la página **Acceso a la aplicación** del portal. Las aplicaciones a las que se accede desde esta página comprueban si hay nuevas asignaciones de roles de inmediato.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en la página **Acceso a la aplicación**.

    ![Acceso a la aplicación de PIM: captura de pantalla](./media/pim-resource-roles-activate-your-roles/pim-application-access.png)

1. Haga clic en **Recursos de Azure** para volver a abrir el portal en la página **Todos los recursos**.

    Al hacer clic en este vínculo, se fuerza la actualización y se comprueba si hay nuevas asignaciones de roles de recursos de Azure.

## <a name="apply-just-enough-administration-practices"></a>Aplicar prácticas de Just Enough Administration.

PIM para recursos de Azure facilita la aplicación de los procedimientos recomendados de JEA (Just Enough Administration) en las asignaciones de roles para recursos. Los usuarios y los miembros del grupo con asignaciones a suscripciones o grupos de recursos de Azure pueden activar su asignación de roles existente en un ámbito menor. 

En la página de búsqueda, busque el recurso subordinado que necesite administrar.

![Seleccionar un recurso](media/azure-pim-resource-rbac/azure-resources-02.png)

Seleccione **Mis roles** del panel izquierdo y elija el rol que desee activar. El tipo de asignación es **Heredado**, puesto que el rol se asignó a la suscripción, en lugar de al grupo de recursos.

![Lista de asignaciones de roles aptos, con el tipo de asignación resaltado](media/azure-pim-resource-rbac/my-roles-02.png)

## <a name="next-steps"></a>Pasos siguientes

- [Activación de mis roles de directorio de Azure AD en PIM](pim-how-to-activate-role.md)