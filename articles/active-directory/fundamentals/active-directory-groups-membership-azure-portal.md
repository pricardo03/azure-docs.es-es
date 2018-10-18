---
title: Incorporación o eliminación de un grupo de otro grupo en Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo agregar o quitar un grupo de otro grupo con Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: c28fe5ef226fac993fde221b16bfa875ba4845ca
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579775"
---
# <a name="how-to-add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>Procedimiento para cómo agregar o quitar un grupo de otro grupo con Azure Active Directory
En este artículo encontrará ayuda para agregar y quitar un grupo de otro grupo con Azure Active Directory.

>[!Note]
>Si intenta eliminar el grupo primario, consulte el artículo sobre [la actualización y la eliminación de un grupo y sus miembros](active-directory-groups-delete-group.md).

## <a name="add-a-group-as-a-member-to-another-group"></a>Adición de un grupo como miembro de otro grupo
Puede agregar un grupo existente a otro grupo existente al crear un grupo miembro (subgrupo) y un grupo primario. El grupo miembro hereda los atributos y las propiedades del grupo primario, lo que le ahorra tiempo de configuración.

### <a name="to-add-a-group-as-a-member-to-another-group"></a>Para agregar un grupo como miembro de otro grupo

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador global del directorio.

2. Seleccione **Azure Active Directory** y después seleccione **Grupos**.

3. En la página **Grupos - Todos los grupos**, busque y seleccione el grupo que va a convertirse en miembro de otro grupo. Para este ejercicio, usaremos el grupo **MDM policy - West**.

    >[!Note]
    >Puede agregar el grupo como miembro solo a un otro grupo a la vez. Además, el cuadro **Seleccionar grupo** filtra la visualización en función de si coincide lo que ha escrito con cualquier parte del nombre de un usuario o dispositivo. Sin embargo, no se admiten caracteres comodín.

    ![Página Grupos - Todos los grupos con el grupo MDM policy - West seleccionado](media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. En la página **MDM policy - West - Group memberships**, seleccione **Pertenencia a grupos**, seleccione **Agregar**, busque el grupo de que quiere que su grupo sea miembro y, luego, elija **Seleccionar**. Para este ejercicio, usaremos el grupo **MDM policy - All org**.

    El grupo **MDM policy - West** ahora es miembro del grupo **MDM policy - All org**, y hereda todas las propiedades y la configuración del grupo MDM policy - All org.

    ![Creación de una pertenencia a grupos mediante la adición de grupo a otro grupo](media/active-directory-groups-membership-azure-portal/add-group-membership.png)

5. Revise la página **MDM policy - West - Group memberships** para ver la relación entre el grupo y el miembro.

    ![Página MDM policy - West - Group memberships que muestra el grupo primario](media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Para obtener una vista más detallada de la relación entre el grupo y el miembro, seleccione el nombre del grupo (**MDM policy - All org**) y eche un vistazo a los detalles de la página **MDM policy - West**.

    ![Página de pertenencia a grupos que muestra los detalles del miembro y del grupo](media/active-directory-groups-membership-azure-portal/group-membership-review.png)

## <a name="remove-a-member-group-from-another-group"></a>Eliminación de un grupo miembro de otro grupo
Puede quitar un grupo miembro existente de otro grupo. Sin embargo, quitar la pertenencia quita también los atributos y propiedades heredados de los usuarios.

### <a name="to-remove-a-member-group-from-another-group"></a>Para quitar un grupo miembro de otro grupo
1. En la página **Grupos - Todos los grupos**, busque y seleccione el grupo que va a quitarse como miembro de otro grupo. Para este ejercicio, volveremos a usar el grupo **MDM policy - West**.

2. En la página **Información general de MDM policy - West**, seleccione **Pertenencia a grupos**.

    ![Página Información general de MDM policy - West](media/active-directory-groups-membership-azure-portal/group-membership-overview.png)

3. Seleccione el grupo **MDM policy - All org** de la página **MDM policy - West - Group memberships** y, luego, seleccione **Quitar** de los detalles de la página **MDM policy - West**.

    ![Página de pertenencia a grupos que muestra los detalles del miembro y del grupo](media/active-directory-groups-membership-azure-portal/group-membership-remove.png)


## <a name="additional-information"></a>Información adicional
Estos artículos proporcionan información adicional sobre Azure Active Directory.

- [Visualización de grupos y miembros](active-directory-groups-view-azure-portal.md)

- [Creación de un grupo básico e incorporación de miembros](active-directory-groups-create-azure-portal.md)

- [Incorporación o eliminación de miembros de un grupo](active-directory-groups-members-azure-portal.md)

- [Edición de la configuración de un grupo](active-directory-groups-settings-azure-portal.md)

- [Asignación de licencias a usuarios por grupo](../users-groups-roles/licensing-groups-assign.md)
