---
title: 'Restauración o eliminación permanente de un usuario recién eliminado: Azure Active Directory | Microsoft Docs'
description: Visualización de los usuarios que se pueden restaurar, restauración de un usuario eliminado o eliminación permanente de un usuario en Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.openlocfilehash: 066c9189e0c3cea409a99217e0ab4b46002df1f9
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556788"
---
# <a name="restore-or-remove-a-recently-deleted-user-using-azure-active-directory"></a>Restauración o eliminación de un usuario recientemente eliminado mediante Azure Active Directory
Después de eliminar a un usuario, la cuenta permanece en estado de suspensión durante 30 días. Durante ese período de 30 días, la cuenta de usuario se puede restaurar, junto con todas sus propiedades. Después de que pase esa ventana de 30 días, el usuario se elimina automáticamente y de forma permanente.

Puede ver a los usuarios que se pueden restaurar, restaurar un usuario eliminado o eliminar permanentemente a un usuario con Azure Active Directory (Azure AD) en Azure Portal.

>[!Important]
>Ni usted ni la asistencia técnica de Microsoft pueden restaurar a un usuario eliminado permanentemente.

## <a name="required-permissions"></a>Permisos necesarios
Debe tener uno de los roles siguientes para restaurar y eliminar permanentemente a los usuarios.

- Administrador de la compañía

- Soporte para asociados de nivel 1

- Soporte para asociados de nivel 2

- Administrador de cuenta de usuario

## <a name="view-your-restorable-users"></a>Visualización de los usuarios que se pueden restaurar
Puede ver a todos los usuarios que se eliminaron hace menos de 30 días. Estos usuarios se pueden restaurar.

### <a name="to-view-your-restorable-users"></a>Para ver a los usuarios que se pueden restaurar
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador global del directorio.

2. Seleccione **Azure Active Directory**, **Usuarios** y, a continuación, seleccione **Usuarios eliminados**.

    Revise la lista de usuarios que están disponibles para restaurar.

    ![Página Usuarios - usuarios eliminados, con usuarios que aún se pueden restaurar](media/active-directory-users-restore/users-deleted-users-view-restorable.png)

## <a name="restore-a-recently-deleted-user"></a>Restauración de un usuario recién eliminado
Mientras una cuenta de usuario está suspendida, se conserva toda la información de directorio relacionada. Cuando se restaura un usuario, también se restaura esta información de directorio.

### <a name="to-restore-a-user"></a>Para restaurar a un usuario
1. En la página **Usuarios - usuarios eliminados**, busque y seleccione uno de los usuarios disponibles. Por ejemplo, _Mary Parker_.

2. Seleccione **Restaurar usuario**.

    ![Página Usuarios - usuarios eliminados, con la opción Restaurar usuario resaltada](media/active-directory-users-restore/users-deleted-users-restore-user.png)

>[!NOTE]
>Anteriormente, cuando se quitaba un usuario del ámbito de sincronización local y se eliminaba de la nube, el estado DirSyncEnabled de la cuenta se establecía erróneamente en "False". Si, posteriormente, ese usuario se restauraba manualmente de la papelera de reciclaje de Azure AD, se mostraba un estado incorrecto de cuenta de "Solo nube". Este problema ya se ha solucionado y el valor del estado DirSyncEnabled siempre se mantiene en "True" cuando un usuario se quita del ámbito de sincronización. A continuación, se elimina temporalmente y se restaura manualmente desde la papelera de reciclaje de Azure AD.

## <a name="permanently-delete-a-user"></a>Eliminar un usuario permanentemente
Puede eliminar permanentemente un usuario desde el directorio sin esperar a que transcurran los 30 días de la eliminación automática. Ni usted, ni otro administrador, ni la asistencia técnica de Microsoft pueden restaurar a un usuario eliminado permanentemente.

>[!Note]
>Si se elimina permanentemente a un usuario por error, tendrá que crear un nuevo usuario y escribir manualmente toda la información anterior. Para más información acerca de cómo crear un usuario, consulte [cómo agregar o eliminar usuarios](add-users-azure-active-directory.md).

### <a name="to-permanently-delete-a-user"></a>Para eliminar un usuario permanentemente

1. En la página **Usuarios - usuarios eliminados**, busque y seleccione uno de los usuarios disponibles. Por ejemplo, _Rae Huff_.

2. Seleccione **Eliminar permanentemente**.

    ![Página Usuarios - usuarios eliminados, con la opción Restaurar usuario resaltada](media/active-directory-users-restore/users-deleted-users-permanent-delete-user.png)

## <a name="next-steps"></a>Pasos siguientes
Después de restaurar o eliminar a los usuarios, puede realizar los siguientes procesos básicos:

- [Adición o eliminación de usuarios](add-users-azure-active-directory.md)

- [Asignación de roles a usuarios](active-directory-users-assign-role-azure-portal.md)

- [Add or change profile information](active-directory-users-profile-azure-portal.md) (Incorporación o modificación de la información del perfil)

- [Adición de usuarios invitados de otro directorio](../b2b/what-is-b2b.md) 

Para obtener más información acerca de otras tareas de administración de usuarios disponibles, consulte la [documentación de administración de usuarios en Azure Active Directory](../users-groups-roles/index.yml).
