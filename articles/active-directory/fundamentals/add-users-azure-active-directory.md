---
title: 'Incorporación o eliminación de usuarios: Azure Active Directory | Microsoft Docs'
description: Instrucciones sobre cómo agregar usuarios nuevos o eliminar usuarios existentes con Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.openlocfilehash: af3078a2db7aecc8d82114f74f55d841cca9b69e
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099267"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>Incorporación o eliminación de usuarios mediante Azure Active Directory
Agregue usuarios nuevos o elimine usuarios existentes desde el inquilino de Azure Active Directory (Azure AD).

## <a name="add-a-new-user"></a>Agregar un nuevo usuario
Puede crear un nuevo usuario con el portal de Azure Active Directory.

### <a name="to-add-a-new-user"></a>Agregar un nuevo usuario
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global o administrador de usuarios del directorio.

2. Seleccione **Azure Active Directory**, **Usuarios** y, a continuación, **Nuevo usuario**.

    ![Página Users - All users (Usuarios: Todos los usuarios) con Nuevo usuario resaltado](media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. En la página **Usuario**, rellene la información necesaria.

    ![Página Agregar nuevo usuario, Usuario con información de usuario](media/add-users-azure-active-directory/new-user-user-blade.png)

    - **Nombre (obligatorio).** Nombre y apellidos del nuevo usuario. Por ejemplo, Mary Parker.

    - **Nombre de usuario (obligatorio).** Nombre de usuario del nuevo usuario. Por ejemplo, mary@contoso.com. 
    
        La parte de dominio del nombre de usuario debe usar el nombre de dominio predeterminado inicial <_nombre de dominio_>.onmicrosoft.com o un nombre de dominio personalizado, como contoso.com. Para obtener más información sobre cómo crear un nombre de dominio personalizado, vea [Incorporación de un nombre de dominio personalizado a Azure Active Directory](add-custom-domain.md).

    - **Perfil.** Si quiere, puede agregar más información sobre el usuario. También puede agregar información del usuario posteriormente. Para obtener más información sobre cómo agregar información de usuario, vea [How to add or change user profile information](active-directory-users-profile-azure-portal.md) (Incorporación o modificación de la información del perfil de usuario).

    - **Grupos.** Si quiere, puede agregar el usuario a uno o varios de los grupos existentes. También puede agregar el usuario a grupos en un momento posterior. Para obtener más información acerca de cómo agregar usuarios a grupos, consulte [Creación de un grupo básico y adición de miembros](active-directory-groups-create-azure-portal.md).

    - **Rol del directorio.** Si quiere, puede agregar el usuario a un rol de directorio. Puede asignar el rol de administrador global al usuario, o uno o varios de los otros roles de administrador de Azure AD. Para obtener más información sobre la asignación de roles, consulte [Asignación de roles a usuarios](active-directory-users-assign-role-azure-portal.md).

4. Copie la contraseña generada automáticamente proporcionada en el cuadro de texto **Contraseña**. Deberá proporcionar esta contraseña al usuario para el proceso inicial de inicio de sesión.

5. Seleccione **Crear**.

    El usuario se crea y se agrega al inquilino de Azure AD.

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Agregar un nuevo usuario en un entorno híbrido
Si tiene un entorno con Azure Active Directory (nube) y Windows Server Active Directory (local), puede agregar nuevos usuarios mediante la sincronización de los datos de la cuenta de usuario existentes. Para obtener más información sobre los entornos híbridos, consulte [Integración de los directorios locales con Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

## <a name="delete-a-user"></a>Eliminar un usuario
Puede eliminar un usuario existente mediante el portal de Azure Active Directory.

### <a name="to-delete-a-user"></a>Para eliminar un usuario
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador global para el directorio.

2. Seleccione **Azure Active Directory**, seleccione **Usuarios** y, a continuación, busque y seleccione el usuario que quiera eliminar de su inquilino de Azure AD. Por ejemplo, _Mary Parker_.

3. Seleccione **Eliminar usuario**.

    ![Página Users - All users (Usuarios: Todos los usuarios) con Eliminar usuario resaltado](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    El usuario se elimina y ya no aparece en la página **Users - All users** (Usuarios: Todos los usuarios). El usuario se puede ver en la página **Usuarios eliminados** durante los próximos 30 días y puede restaurarse durante ese tiempo. Para obtener más información acerca de cómo restaurar un usuario, consulte [How to restore or permanently remove a recently deleted user](active-directory-users-restore.md) (Cómo restaurar o quitar de forma permanente un usuario recién eliminado).

    >[!Note]
    >Debe usar Windows Server Active Directory para actualizar la identidad, la información de contacto o la información del trabajo para los usuarios cuyo origen de autoridad es Windows Server Active Directory. Después de completar la actualización, debe esperar a que se complete el próximo ciclo de sincronización antes de poder ver los cambios.

## <a name="next-steps"></a>Pasos siguientes
Después de agregar a los usuarios, puede realizar los siguientes procesos básicos:

- [Add or change profile information](active-directory-users-profile-azure-portal.md) (Incorporación o modificación de la información del perfil)

- [Asignación de roles a usuarios](active-directory-users-assign-role-azure-portal.md)

- [Creación de un grupo básico y adición de miembros](active-directory-groups-create-azure-portal.md)

- [Trabajo con usuarios y grupos dinámicos](../users-groups-roles/groups-create-rule.md)

O bien puede realizar otras tareas de administración de usuarios, como [agregar usuarios invitados de otro directorio](../b2b/what-is-b2b.md) o [restaurar un usuario eliminado](active-directory-users-restore.md). Para obtener más información acerca de otras acciones disponibles, consulte la [documentación de administración de usuarios en Azure Active Directory](../users-groups-roles/index.yml).
