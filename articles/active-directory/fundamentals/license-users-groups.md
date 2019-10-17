---
title: 'Asignación o eliminación de licencias: Azure Active Directory | Microsoft Docs'
description: Instrucciones sobre cómo asignar o quitar licencias de Azure Active Directory de sus usuarios o grupos.
services: active-directory
author: msaburnley
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 869713967a6a89611ab2ea9ca7b7516f090a5f92
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72034285"
---
# <a name="assign-or-remove-licenses-in-the-azure-active-directory-portal"></a>Asignación o eliminación de licencias en el portal de Azure Active Directory

Muchos servicios de Azure Active Directory (Azure AD) exigen que asigne licencias a cada uno de los usuarios o grupos (y los miembros asociados) para ese servicio. Solo los usuarios con licencias activas podrán acceder y usar los servicios de Azure AD licenciados que presentan este requisito.

## <a name="available-license-plans"></a>Planes de licencia disponibles

Hay varios planes de licencia disponibles para el servicio de Azure AD, entre los que se incluyen:

- Azure AD Free

- Azure AD Premium P1

- Azure AD Premium P2

Para obtener información específica acerca de cada plan de licencia y los detalles de licencias asociados, consulte [¿Qué licencia necesito?](https://azure.microsoft.com/pricing/details/active-directory/)

No todos los servicios de Microsoft están disponibles en todas las ubicaciones. Antes de poder asignar una licencia a un grupo, tiene que especificar la **Ubicación de uso** para todos los miembros. Puede establecer este valor en el área **Azure Active Directory &gt; Usuarios &gt; Perfil &gt; Configuración** en Azure AD. Cualquier usuario cuya ubicación de uso no se especifique hereda la ubicación de la organización de Azure AD.

## <a name="view-license-plans-and-plan-details"></a>Consulta de los planes de licencia y sus detalles

Puede ver los planes de servicio disponibles —incluidas las licencias individuales—, comprobar las fechas de caducidad pendientes y ver el número de asignaciones disponibles.

### <a name="to-find-your-service-plan-and-plan-details"></a>Para buscar el plan de servicio y sus detalles

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador de licencias en la organización de Azure AD.

1. Seleccione **Azure Active Directory** y luego seleccione **Licencias**.

    ![Página Licencias, con el número de servicios comprados y licencias asignadas](media/license-users-groups/license-details-blade.png)

1. Seleccione el vínculo **Comprado** para acceder a la página **Productos** y ver los números en **Asignado**, **Disponible** y **Expira próximamente** para los planes de licencias.

    ![Página Servicios, con los planes de licencia de servicio y la información de licencia asociada](media/license-users-groups/license-products-blade-with-products.png)

1. Seleccione un nombre de plan para ver sus usuarios y grupos con licencias.

## <a name="assign-licenses-to-users-or-groups"></a>Asignación de licencias a usuarios o grupos

Asegúrese de que cualquier usuario que tenga que usar un servicio de Azure AD licenciado tenga la licencia apropiada. Puede agregar los derechos de licencia a usuarios o a un grupo completo.

### <a name="to-assign-a-license-to-a-user"></a>Para asignar una licencia a un usuario

1. En la página **Productos**, seleccione el nombre del plan de licencia que quiere asignar al usuario.

    ![Página Servicios, con el plan de licencia de servicio resaltado](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. En la página de información general del plan de licencia, seleccione **Asignar**.

    ![Página Servicios, con la opción Asignar resaltada](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. En la página **Asignar**, seleccione **Usuarios y grupos** y, luego, busque y seleccione el usuario al que va a asignar la licencia.

    ![Página Asignar licencia, con las opciones búsqueda y Seleccionar resaltadas](media/license-users-groups/assign-license-blade-with-highlight.png)

1. Seleccione **Opciones de asignación**, asegúrese de tener activadas las opciones de licencia apropiadas y, luego, seleccione **Aceptar**.

    ![Página Opciones de licencia, con todas las opciones disponibles en el plan](media/license-users-groups/license-option-blade-assignments.png)

    La página **Asignar licencia** se actualiza para mostrar que hay un usuario seleccionado y que las asignaciones están configuradas.

    > [!NOTE]
    > No todos los servicios de Microsoft están disponibles en todas las ubicaciones. Antes de poder asignar una licencia a un usuario, tiene que especificar la **Ubicación de uso**. Puede establecer este valor en el área **Azure Active Directory &gt; Usuarios &gt; Perfil &gt; Configuración** en Azure AD. Cualquier usuario cuya ubicación de uso no se especifique hereda la ubicación de la organización de Azure AD.

1. Seleccione **Asignar**.

    El usuario se agrega a la lista de usuarios con licencia y tiene acceso a los servicios de Azure AD incluidos.

### <a name="to-assign-a-license-to-a-group"></a>Para asignar una licencia a un grupo

1. En la página **Productos**, seleccione el nombre del plan de licencia que quiere asignar al usuario.

    ![Hoja Productos, con el plan de licencia del producto resaltado](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. En la página **Azure Active Directory Premium Plan 2**, seleccione **Asignar**.

    ![Página Productos, con la opción Asignar resaltada](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. En la página **Asignar**, seleccione **Usuarios y grupos** y, luego, busque y seleccione el grupo al que va a asignar la licencia.

    ![Página Asignar licencia, con las opciones búsqueda y Seleccionar resaltadas](media/license-users-groups/assign-group-license-blade-with-highlight.png)

1. Seleccione **Opciones de asignación**, asegúrese de tener activadas las opciones de licencia apropiadas y, luego, seleccione **Aceptar**.

    ![Página Opciones de licencia, con todas las opciones disponibles en el plan](media/license-users-groups/license-option-blade-group-assignments.png)

    La página **Asignar licencia** se actualiza para mostrar que hay un usuario seleccionado y que las asignaciones están configuradas.

1. Seleccione **Asignar**.

    El grupo se agrega a la lista de grupos con licencias, y todos los miembros tienen acceso a los servicios de Azure AD incluidos.

## <a name="remove-a-license"></a>Eliminación de una licencia

Puede quitar una licencia desde la página de usuario de Azure AD de un usuario, desde la página de información general del grupo en una asignación de grupo o empezando desde la página **Licencias** de Azure AD para ver los usuarios y grupos de una licencia.

### <a name="to-remove-a-license-from-a-user"></a>Para quitar una licencia de un usuario

1. En la página **Usuarios con licencias** para el plan de servicio, seleccione el usuario que ya no debe tener la licencia. Por ejemplo, _Alain Charon_.

1. Seleccione **Quitar licencia**.

    ![Página Usuarios con licencias con la opción Quitar licencia resaltada](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

> [!IMPORTANT]
> Las licencias que un usuario hereda de un grupo no se pueden quitar directamente. En su lugar, tiene que quitar el usuario del grupo desde el que haya heredado la licencia.

### <a name="to-remove-a-license-from-a-group"></a>Para quitar una licencia de un grupo

1. En la página **Grupos con licencias** para el plan de licencia, seleccione el grupo que ya no debe tener la licencia.

1. Seleccione **Quitar licencia**.

    ![Página Grupos con licencias con la opción Quitar licencia resaltada](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)

## <a name="next-steps"></a>Pasos siguientes

Después de haber asignado las licencias, puede seguir los procesos a continuación:

- [Identificación y resolución de problemas de asignación de licencias](../users-groups-roles/licensing-groups-resolve-problems.md)

- [Cómo agregar usuarios a un grupo para obtener licencias](../users-groups-roles/licensing-groups-migrate-users.md)

- [Escenarios, limitaciones y problemas conocidos del uso de grupos para administrar las licencias en Azure Active Directory](../users-groups-roles/licensing-group-advanced.md)

- [Agregar o cambiar la información del perfil](active-directory-users-profile-azure-portal.md)
