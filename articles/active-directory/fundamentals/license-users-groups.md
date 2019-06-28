---
title: 'Asignación o eliminación de licencias: Azure Active Directory | Microsoft Docs'
description: Instrucciones sobre cómo asignar o quitar licencias de Azure Active Directory de sus usuarios o grupos.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e7a3f80067adb3093bd27e34a45b3afd72b4993
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60247603"
---
# <a name="assign-or-remove-licenses-using-the-azure-active-directory-portal"></a>Asignación o eliminación de licencias mediante el portal de Azure Active Directory
Muchos servicios de Azure Active Directory (Azure AD) exigen que active un producto de Azure AD y asigne licencias a cada uno de los usuarios o grupos (y los miembros asociados) para ese producto. Solo los usuarios con licencias activas podrán acceder y usar los servicios de Azure AD licenciados.

## <a name="available-product-editions"></a>Ediciones de productos disponibles
Hay varias ediciones disponibles para el producto de Azure AD.

- Azure AD Free

- Azure AD Basic

- Azure AD Premium 1 (Azure AD P1)

- Azure AD Premium 2 (Azure AD P2)

Para obtener información específica acerca de cada edición del producto y los detalles de licencias asociados, consulte [¿Qué licencia necesito?](../authentication/concept-sspr-licensing.md)

## <a name="view-your-product-edition-and-license-details"></a>Visualización de los detalles de edición y licencia del producto
Puede ver los productos disponibles, incluidas las licencias individuales, y comprobar las fechas de caducidad pendientes y el número de asignaciones disponibles.

### <a name="to-find-your-product-and-license-details"></a>Para buscar los detalles del producto y de las licencias
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador global del directorio.

2. Seleccione **Azure Active Directory** y luego seleccione **Licencias**.

    Aparece la página **Licencias**.

    ![Página de licencias, con el número de productos comprados y licencias asignadas](media/license-users-groups/license-details-blade.png)
    
3. Seleccione el vínculo de **productos comprados** para ver la página **Productos** y los detalles de **Asignado**, **Disponible** y **Expira próximamente** para cada edición del producto específico.

    ![Página Productos, con las ediciones de producto e información de licencias asociadas](media/license-users-groups/license-products-blade-with-products.png)

4. Seleccione el nombre de una edición del producto para ver sus usuarios y grupos con licencias.

## <a name="assign-licenses-to-users-or-groups"></a>Asignación de licencias a usuarios o grupos
Asegúrese de que cualquier usuario que tenga que usar un servicio de Azure AD licenciado tenga la licencia apropiada. Depende de usted si quiere agregar los derechos de licencias a usuarios individuales o a todo un grupo.

>[!Note]
>Las licencias basadas en grupos son una característica en vista previa (GB) pública de Azure AD y están disponibles con cualquier plan de licencias de Azure AD de pago. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).<br><br>Para obtener información detallada acerca de cómo agregar usuarios, consulte [cómo agregar o eliminar usuarios en Azure Active Directory](add-users-azure-active-directory.md). Para obtener información detallada acerca de cómo crear grupos y agregar miembros, consulte [cómo crear un grupo básico y agregar miembros](active-directory-groups-create-azure-portal.md).

### <a name="to-assign-a-license-to-a-specific-user"></a>Para asignar una licencia a un usuario específico
1. En la página **Productos**, seleccione el nombre de la edición que quiere asignar al usuario. Por ejemplo, _Azure Active Directory Premium Plan 2_.

    ![Página Productos, con la edición del producto resaltada](media/license-users-groups/license-products-blade-with-product-highlight.png)

2. En la página **Azure Active Directory Premium Plan 2**, seleccione **Asignar**.

    ![Página Productos, con la opción Asignar resaltada](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

3. En la página **Asignar**, seleccione **Usuarios y grupos** y, luego, busque y seleccione el usuario al que va a asignar la licencia. Por ejemplo, _Mary Parker_.

    ![Página Asignar licencia, con las opciones búsqueda y Seleccionar resaltadas](media/license-users-groups/assign-license-blade-with-highlight.png)

4. Seleccione **Opciones de asignación**, asegúrese de tener activadas las opciones de licencia apropiadas y, luego, seleccione **Aceptar**.

    ![Página de opciones de licencia, con todas las opciones disponibles en la edición](media/license-users-groups/license-option-blade-assignments.png)

    La página **Asignar licencia** se actualiza para mostrar que hay un usuario seleccionado y que las asignaciones están configuradas.

    >[!NOTE]
    >No todos los servicios de Microsoft están disponibles en todas las ubicaciones. Antes de poder asignar una licencia a un usuario, tiene que especificar la **Ubicación de uso**. Puede establecer este valor en el área **Azure Active Directory &gt; Usuarios &gt; Perfil &gt; Configuración** en Azure AD.

5. Seleccione **Asignar**.

    El usuario se agrega a la lista de usuarios con licencia y tiene acceso a los servicios de Azure AD incluidos.

### <a name="to-assign-a-license-to-an-entire-group"></a>Para asignar una licencia a todo un grupo
1. En la página **Productos**, seleccione el nombre de la edición que quiere asignar al usuario. Por ejemplo, _Azure Active Directory Premium Plan 2_.

    ![Hoja Productos, con la edición del producto resaltada](media/license-users-groups/license-products-blade-with-product-highlight.png)

2. En la página **Azure Active Directory Premium Plan 2**, seleccione **Asignar**.

    ![Página Productos, con la opción Asignar resaltada](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

3. En la página **Asignar**, seleccione **Usuarios y grupos** y, luego, busque y seleccione el grupo al que va a asignar la licencia. Por ejemplo, _MDM policy - West_.

    ![Página Asignar licencia, con las opciones búsqueda y Seleccionar resaltadas](media/license-users-groups/assign-group-license-blade-with-highlight.png)

4. Seleccione **Opciones de asignación**, asegúrese de tener activadas las opciones de licencia apropiadas y, luego, seleccione **Aceptar**.

    ![Página de opciones de licencia, con todas las opciones disponibles en la edición](media/license-users-groups/license-option-blade-group-assignments.png)

    La página **Asignar licencia** se actualiza para mostrar que hay un usuario seleccionado y que las asignaciones están configuradas.

    >[!NOTE]
    >No todos los servicios de Microsoft están disponibles en todas las ubicaciones. Antes de poder asignar una licencia a un grupo, tiene que especificar la **Ubicación de uso** para todos los miembros. Puede establecer este valor en el área **Azure Active Directory &gt; Usuarios &gt; Perfil &gt; Configuración** en Azure AD. Cualquier usuario cuya ubicación de uso no se especifique hereda la ubicación del inquilino.

5. Seleccione **Asignar**.

    El grupo se agrega a la lista de grupos con licencias, y todos los miembros tienen acceso a los servicios de Azure AD incluidos.

## <a name="remove-a-license"></a>Eliminación de una licencia
Puede quitar una licencia de un usuario o de un grupo desde la página **Licencias**.

### <a name="to-remove-a-license-from-a-specific-user"></a>Para quitar una licencia de un usuario específico
1. En la página **Usuarios con licencias** para la edición del producto, seleccione el usuario que ya no debe tener la licencia. Por ejemplo, _Alain Charon_.

2. Seleccione **Quitar licencia**.

    ![Página Usuarios con licencias con la opción Quitar licencia resaltada](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

### <a name="to-remove-a-license-from-a-group"></a>Para quitar una licencia de un grupo
1. En la página **Grupos con licencias** para la edición del producto, seleccione el grupo que ya no debe tener la licencia. Por ejemplo, _MDM policy - West_.

2. Seleccione **Quitar licencia**.

    ![Página Grupos con licencias con la opción Quitar licencia resaltada](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)

>[!Important]
>Las licencias heredadas por un usuario de un grupo no se pueden quitar directamente. En su lugar, tiene que quitar el usuario del grupo desde el que haya heredado la licencia.

## <a name="next-steps"></a>Pasos siguientes
Después de haber asignado las licencias, puede seguir los procesos a continuación:

- [Identificación y resolución de problemas de asignación de licencias](../users-groups-roles/licensing-groups-resolve-problems.md)

- [Cómo agregar usuarios a un grupo para obtener licencias](../users-groups-roles/licensing-groups-migrate-users.md)

- [Escenarios, limitaciones y problemas conocidos del uso de grupos para administrar las licencias en Azure Active Directory](../users-groups-roles/licensing-group-advanced.md)

- [Agregar o cambiar la información del perfil](active-directory-users-profile-azure-portal.md)
